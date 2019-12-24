# Chip-seq Workshop

## Bioinformatics Training Workshop at PAG2020

By Rafet Al-Tobasei [rafet.al-tobasei@mtsu.edu]

The purpose of this lab is to get participants familiar with the typical workflow of analyzing ChIP-seq data. We will cover the basics of

- Raw data fastq QC
- Sequence alignment using bowtie2.
- Post alignment QC.
- ChIP-seq: peak calling.
- Peak analysis.
- Peak visualization.
- Peak Annotation and Functional Analysis.
- Several useful Bioconductor packages for genomic data analyses.

**Before the workshop:**

Participants should pre-install the following software before they come to the workshop. **We won&#39;t have time to install these on the workshop day.**

- Fastqc([http://www.bioinformatics.babraham.ac.uk/projects/fastqc/](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/))
- MultiQC ([http://multiqc.info/](http://multiqc.info/))
- Bowtie2 ([http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml](http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml)).
- Deeptools ([https://deeptools.readthedocs.io/en/develop/](https://deeptools.readthedocs.io/en/develop/)
- Picard tools ([https://broadinstitute.github.io/picard/](https://broadinstitute.github.io/picard/)
- MACS ([https://github.com/taoliu/MACS](https://github.com/taoliu/MACS/)).
- samtools  ([http://samtools.sourceforge.net](http://samtools.sourceforge.net)).
- Integrated genome viewer ([https://software.broadinstitute.org/software/igv/](https://software.broadinstitute.org/software/igv/)).
- R/Bioconductor package diffbind, ChipQC,  ChIPseeker, and GenomicFeatures  to install.

```{r}
if (!requireNamespace("BiocManager", quietly = TRUE))

    install.packages("BiocManager")

    BiocManager::install("shengqh/ChIPQC")

    BiocManager::install("DiffBind")

    BiocManager::install("ChIPseeker")

    BiocManager::install("GenomicFeatures")

    install.packages("devtools")

    devtools::install_github("hadley/tidyverse")
```

**Data** :

We will use the following datasets.

1. Reference genome and a sequence read (chr 1 only) for Rainbow trout will be used for practicing read quality control (QC), checkup using fastqc, and sequence alignment using bowtie. The small sizes raw sequence read file make the computation fast (index file is provided to expedite the process).
2. ChIP-seq data for histone D8\_K4 and D8\_K27, and control for D8 cell line. The files were processed to keep only reads aligned to chromosome 1(NC\_035077.1)
3. If you could not finish a step, you can use the processed data in the tar file to continue to the next step.

All data can be downloaded from xxx as a zip file.



**Directory setup** (10 minutes)

First, let us setup file directory:

Create directory chipseq\_project in your home directory


```
mkdir chipseq_project

cd chipseq_project
```

 Create the following directories structure for your analyses.


```
mkdir –p data/fastq

mkdir –p data/map

mkdir peak

mkdir genome

mkdir results


```
move the data zip file you download to the chipseq_project directory using the following command:

```
mv /path/to/data_fastq_file.tar .

```
Use the following command to unzip the tar file

```
tar –xzvf data_fastq_file.tar
```

move the fastq files to your new fastq directory using the following command:

```

mv chipseq_project/data/fastq/*fastq data/fastq/
```

move the trout genome to your new genome directory using the following command:
```
mv chipseq_project/genome/genome_chr1.* genome/

```

you can use ls command to verify if you copy the files into your directory

**Workflow:**

1. **Quality Control** (5-10 minutes)

- We will check the quality of the fastq reads using fastqc.
- Move into the folder which contains the fastq sequence data using cd command
```
cd data/fastq/
```
- Run the following command (if you have a multi-core system you can add –t 4)
```
fastqc *fastqc
```
or for multi-core system

```
fastqc -t 5 *fastqc
```
- Several new html files will be generated. You can open these files using the following command
```
firefox *html &
```
For mac user 
```
open *html
```
![fastqc table 1](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/fastqc_tabl1.png)

![fastqc figure 1](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/fastqc_fig1.png)

- Check the report for read base sequence quality, adapter, over-represented sequences, and sequence duplication levels.

- (Optional step) You can use MultQC software to aggregate all the reports in one using the following command.


```
mulitqc .
```

2. **Mapping/Filtering** (30-40 minutes)
  - **Create a reference index using bowtie2-build**

First, we need to create an index based on our genome reference (bowtie has a pre-built index for common species and available to download from bowtie website.)


  - This step is slow, so that we will use a pre-built index. (whenever you run this step it will generate six .ebwt files, which represent the index files for the reference genome)

  - To build an index use the following command (5 minutes)

move to the genome directory:

```
cd ../../genome/

bowtie2-build genoem_chr1.fa Trout_genome
```
verify that you have six new files using ls command

  - **Perform alignment of reads to the genome using Bowtie2**


  - Next, we will align the reads to the reference genome.

Move to the data/map directory using cd command
```
cd ../data/map/
```

Execute the following command (10 minutes) make sure you change the input file name the correct input name 
repeat this command for all five fastq files.


```
bowtie2 -p 4 -q --local -x ../../genome/Trout_genome -U ../input_file_name.fastq -S input_file_name.sam
```


-p: number of processors/cores

-q: reads are in FASTQ format

--local: local alignment feature to perform soft-clipping

-x: genome\_indices\_directory

-U: FASTQ\_file

-S: SAM\_file

After this step, bowtie print alignment result statistics on the screen. A file called output\_unsorted.sam is created.  Take a look at the header of the output\_unsorted.sam using the head command.


```
head    output.sam
```


**Convert the alignment to BAM format, then create an index, and sort the BAM file.** (20 minutes)

A bam (smaller and faster to process) file format is needed for the next analysis, so we need to convert the sam file to bam file using samtools.

Run the following command:


```
samtools view –h -bS -o output_unsorted.bam input.sam
```


Before we can filter the alignment output based on MAPQ, we need to sort the bam file.

Run the following commands:


```
samtools sort reads.bam –o reads.sorted

samtools index reads.sorted.bam

```

Filtering based on mapping quality MAPQ (-q 20)

Run the following command:


```
samtools view -q 20 -b -o filtered_sorted.bam read_sorted.bam
```


We need to index the bam files for later analysis


```
samtools index reads_filtered_sorted.bam
```
verify that you have correct files using ls command

**Using Picard tools mark duplicate reads** (5 minutes)

**Run the following commands:**


```
java -jar path/to/picard.jar MarkDuplicates INPUT=filtered_Sorted.bam OUTPUT=filterd_sorted_mDup.bam ASSUME_SORTED=true METRICS_FILE=metrics.txt VALIDATION_STRINGENCY=SILENT
```


**Now we are ready to perform peak calling.** (10 minutes)

3. **ChIP-seq peak calling.**

First, create a directory for the output generated from MACS2 and lets call it peak:

Go to the peak directory, and run following in command window for our four bam files:


```
macs2 callpeak -t filtered_sorted_mDup.bam -c Input_control.bam –-n output_prefix 2> output.log
```


This will generate some .xls and .bed files. .bed are peak lists in bed format.



4. **Peaks quality assessment using ChIPQC** (20 minutes)

**Using Rstudio, we will check the quality of the peaks.**

**First step: construct a sample sheet describing the ChIP-seq experiment and saved it as a csv file.**

**Sample sheet format**

SampleID,Factor,Replicate,bamReads,ControlID,bamControl,Peaks,PeakCaller,Tissue,Condition

Open up a new R script (File\-> New File \-> Rscript), and save it as chipQC.R

Run the following script in Rstudio

```{r}

library(ChIPQC)

## Load sample data

samples <- read.csv("sample.csv")

View(samples)

## Create ChIPQC object

chipObj <- ChIPQC(samples)

ChIPQCreport(chipObj, reportName="ChIP QC report: K4 and K27", reportFolder="ChIPQCreport")
```

![ChipQC report](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/chiqc_tabl1.png)

1. **Handling Replicate in Chip-seq using IDR** (5 minutes)

Replicates expected to have high consistency among them, but that is not always the case. One of the common methods for handling replicates is to consider overlapping peak calls across replicates.  Other methods involve complex statistical testing by evaluating the reproducibility between replicates.

For the first method, we will use deeptools to combine duplicates:

In the chipseq\_poroject directory create a new directory  and name it combin\_replicate using mkdir as follow:


```
mkdir  combin_replicate

cd combin_replicate
```

Run the following command (change the name of replicate to the correct names):

```

bedtools intersect  -a ../peak/replicate1 –b ../peak/replicate2 –wo > combin_replicate1_and_2

```

5. **Annotation** (10 minutes)

After we have identified enriched regions, it\'s essential to find the function of these regions and the genes that are associated with the genome.

For peak annotation, we will use R package _chIPsseker_ to identify genes that close to the peaks.

**Use the following R command in Rstudio**

```{r}

# Load libraries

library(ChIPseeker)

# create database object using rainbow trout gff file

trout_txdb <-makeTxDbFromGFF("genome/GCF_002163495.1_Omyk_1.0_genomic.gff")

saveDb(trout_txdb, file="Trout.sqlite")

trout_txdb <- loadDb("Trout.sqlite")

txdb <- trout_txdb

# Load data

samplefiles <- list.files("peak", pattern= ".narrowPeak", full.names=T)

samplefiles <- as.list(samplefiles)

names(samplefiles) <- c("2D8-_K27", "2D8-_K4","1D8-_K27", "1D8-_K4")

peakAnnoList <- lapply(samplefiles, annotatePeak, TxDb=txdb,

                     tssRegion=c(-3000, 3000), verbose=FALSE)

peakAnnoList

plotAnnoBar(peakAnnoList)

plotDistToTSS(peakAnnoList, title="Distribution of peak relative to TSS")

```

![Chipseeker figure 1](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/chipsseker_fig1.png)

```{r}

# Write to file

write.table(peakAnnoList[["1D8-_K27"]]@anno, file="results/chipseqAnn.txt", sep="\t", quote=F, row.names=F)

peakAnnoList[["1D8-_K27"]]@anno

GRanges object with 5 ranges and 16 metadata columns:

         seqnames            ranges strand | ID8.K27_S10Ch1_peak_1       X95        .  X5.80207 X14.76815  X9.50397      X273

            <Rle>         <IRanges>  <Rle> |              <factor> <integer> <factor> <numeric> <numeric> <numeric> <integer>

  [1] NC_035077.1   6462386-6462787      * | ID8-K27_S10Ch1_peak_2        28        .   1.59412   7.43889   2.83001       109

  [2] NC_035077.1 18511982-18512286      * | ID8-K27_S10Ch1_peak_3        19        .   3.29459   5.93627   1.94471       211

  [3] NC_035077.1 71520644-71520967      * | ID8-K27_S10Ch1_peak_4        24        .   4.46286   6.89985   2.47762       226

  [4] NC_035077.1 78606476-78606775      * | ID8-K27_S10Ch1_peak_5        31        .   5.21212   7.82383   3.11772       155

  [5] NC_035077.1 84165948-84166506      * | ID8-K27_S10Ch1_peak_6       218        .    8.4195   29.7507  21.82188       265

                                               annotation   geneChr geneStart   geneEnd geneLength geneStrand       geneId   transcriptId

                                              <character> <integer> <integer> <integer>  <integer>  <integer>  <character>    <character>

  [1] Intron (XM_021616938.1/LOC110532765, intron 1 of 2)         2   6465836   6490638      24803          2 LOC110532690 XM_021616819.1

  [2]                                    Promoter (<=1kb)         2  18512632  18621908     109277          1 LOC110508592 XM_021589263.1

  [3]                                   Distal Intergenic         2  71507413  71511751       4339          1 LOC110529485 XM_021611701.1

  [4]                                   Distal Intergenic         2  78354929  78360796       5868          2 LOC110530634 XR_002474563.1

  [5]                                    Promoter (<=1kb)         2  84164660  84165260        601          2 LOC110531895 XR_002474772.1

      distanceToTSS

         <numeric>

  [1]         27851

  [2]          -346

  [3]         13231

  [4]       -245680

  [5]          -688

  -------

  ```

6. **Visualize the data using IGV.** (If time permitted)

Start igv tool

IGV takes .tdf files for reads counts. First, convert the bam files into tdf with following steps:

1. Select reference genome (you may need to create genome by selecting Genomes tab and the load genome from file &quot;fasta file&quot;).
2. Convert bam files to tdf files as follow:
  1. Select &quot;Run igvtools&quot; form the &quot;Tools&quot; menu.
  2. In command, select &quot;Count.&quot; Then select the Input file. Keep other options as default and click &quot;Run.&quot;

After conversion, you&#39;ll get five files with extension &quot;.bam.tdf&quot;

Now click File \-\> Load from File and select the tdf files,

The coverage will be shown as bar plots. Try zooming and moving the viewer window to see some peaks. The genomic region can be input to the text box at the top.

![IGV figure 1](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/igv_fig1.png)
![IGV figure 1](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/igv_figK4.png)

7.	**Evaluating Differential Enrichment**
The main goal of differential binding analysis is to find changes in protein-DNA interactions measured. We will look at the differential enrichment between histone K4 and histone K27. For this part we will use diffBind R Bioconductor package.
In Rstudio open new R script file and write the following code. 

```{r}
library(DiffBind)
library(tidyverse)
library(dplyr)

samples <- read.csv('sample.csv')
dbObj <- dba(sampleSheet=samples)
# calculate a binding matrix based on the read counts for every sample 
dbObj <- dba.count(dbObj, bUseSummarizeOverlaps=TRUE)
# plot correlation between replicates
plot(dbObj)

# group sample based on factor. Set the minMembers parameter to 2 (default is 3).
dbObj <- dba.contrast(dbObj, categories=DBA_FACTOR, minMembers = 2)

# using DESseq an edger methods to performs differential binding analysis  
dbObj <- dba.analyze(dbObj, method=DBA_ALL_METHODS)
dba.plotHeatmap(dbObj, contrast=1, correlations=FALSE)
dba.show(dbObj, bContrasts=T)	

#Visualizing the results
dba.plotVenn(dbObj,contrast=1,method=DBA_ALL_METHODS)

# rreport the differentially bound peak regions, identified by either method (DESeq2/edgeR).
res_deseq <- dba.report(dbObj, method=DBA_DESEQ2)
res_edger <- dba.report(dbObj, method=DBA_EDGER)
res_deseq
res_edger
# Write to file
out <- as.data.frame(res_deseq)
write.table(out, file="results/D8_K4_vs_D8_K27_diffbind.txt", sep="\t", quote=F, row.names=F)

# create bed files for each keeping only significant peaks(p < 0.05)
K4_enrich <- dplyr::filter( out, FDR < 0.05 & Fold > 0) 

# Write to file
write.table(K4_enrich, file="results/K4_enriched.bed", sep="\t", quote=F, row.names=F, col.names=F)

K27_enrich = dplyr::filter(out, FDR < 0.05 & Fold < 0 )

# Write to file
write.table(K27_enrich, file="results/K27_enriched.bed", sep="\t", quote=F, row.names=F, col.names=F)
```

![diffBind](https://www.cs.mtsu.edu/~raltobasei/chiseq_image/diffBind_overlap.png)
