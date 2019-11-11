# **Schedule for Bioinformatics Training Workshop @ PAG2020**
>**Notice: For participants do not meet the prerequisites, the workshop will start at 2:00PM January 8, 2020; while for advanced participants who have basic knowledge of and experience with UNIX command line and R programming language are expected to start at 8:00 AM January 9, 2020. To prepare yourself for the workshop, you can teach yourself some basic Unix and R via the resources provided by the appendix**.
***
## **Day 1: Jan 8, 2020 (2:00 PM - 5:30 PM)**

2:00-2:20 registration and welcome

2:20-3:20 Introduction to UNIX command line (Lecture and hands-on session)
* Intro to the UNIX operating system (ssh, terminal, file system, …)
* Handle paths and files (ls, cp, cd, mkdir, rm, find, touch, mv, ln, …)
* View content of files (less, more, head, tail, vi, cat)
* Search content of files (grep, wildcards)
* Get help (man)
* File permission (chmod)
* Process and job
* Resource management (df, du, quota, …)
* Other goodies (gzip/unzip, tar, zcat, cut, paste, join, file, diff, history,…)

3:20-3:30 break

3:30-5:30 Introduction to R (Lecture and hands-on session)
* R/RStudio installation and setup
* Data structure and manipulation (vector, list, array/matrix, data frame/table)
* Data input and output
* Get help
* Flow control (if/else, for, while, repeat, break, next)
* Function
* Intro to Object oriented programming in R (S3/S4 classes, reference class).
* R package management
* Different modes of running R scripts (interactive and batch)
***
## **Day 2: Jan 9, 2020 (8:00 AM -5:30 PM)**

8:00-8:15 registration and welcome

8:15 -9:15 Advanced Unix command line (Lecture and hands-on session)
* Regular expression, sed, awk, xargs
* pipe, redirection, shell scripting
* Large data management: download, copy, backup(scp, ftp, ascp, rsync)
* Software installation and management (module, configuration file, …)
* HPC job management systems (one  or 2 out of SLURM, SGE, PBS, LSF, …)

9:15-9:25 break

9:25-10:55 Exploratory data analysis and Data visualization in R (Lecture and hands-on session)

* Descriptive statistics (frequency, correlation, statistic tests, PCA, MDS, hierarchical clustering)

* R graphics (base, ggplot2): all types of plots, and heatmap

10:55-11:05 break

11:05-12:00 Introduction to some very useful R/BioConductor packages (mainly lecture and some hands-on session)

* Readr, data.table
* tidyverse, dplyr,reshape2,  tidyr, stringr, lubridate
* ggvis, plotly, htmlwidgets, googleVis, threejs
* lme4/nlme, survival, caret
* shiny, rmarkdown, solidify
* For more useful packages, see https://awesome-r.com/

12:00PM-1:00 lunch

1:00-2:00 Introduction to NGS technologies, common types of genomics data, and handling tools (Lecture)

* Illumina and other sequencing technologies
* fastq, fasta, BAM, SAM, bed, gtf, gff, bigwig, …
* read QC: FASTQC, MultiQC
* Short read aligners, SAMtools, picardtools, bedtools-
* Visualization: IGV, UCSC genome browser

2:00-2:10 break

2:10-3:20 RNA-seq data analysis (I) (Lecture) (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/)

* Experimental design, RNA-seq data characteristics
* Read QC (FASTQC, MultiQC)
* Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)
* Alignment (STAR, HISAT, GMAP/GSNAP, RSEM) or semi-alignment (Kallisto, Salmon)
* Post –alignment QC (QoRT)
* Optional post-alignment process: multi-mapper assignment (MMR)
* Count summary (featureCounts)
* Exploratory analysis (PCA, hierarchical clustering)
* Differential expression analysis (statistic model selection): DESeq2/edgeR, Voom
* Gene ontology and pathway analysis
* Network analysis

3:20-3:30 break

3:30-5:30 RNA-seq data analysis (II) (hands-on session)

* Exercises with toy data for QC and mapping, and real life count table for DEG, discussion

5:30-6:30 ChIP-seq data analysis (I) (Lecture): https://www.ncbi.nlm.nih.gov/pubmed/22955991

* Experimental design, data characteristics
* Read QC (FASTQC, MultiQC)
* Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)
* Alignment (bwa-mem, bowtie/bowtie2)
* Post-alignment QC (picard tools, deeptools, ChIPQC, SPP)
* Peak Calling (MASC2, MUSIC/BCP)
* Peak annotation (ChIPSeeker, GREAT)
***
## **Day 3: Jan 10, 2020 (8:00 AM -12:00 PM)**

8:00-10:00 ChIP-seq data analysis (II) (Hands-on session)

* Exercises, discussion

10:00-10:10 break

10:10-11:00 ATAC-seq data analysis (Lecture, providing scripts for analysis)

* Experimental design, data characteristics
* Read QC (FASTQC, MultiQC)
* Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)
* Alignment (bwa-mem, bowtie/bowtie2)
* Post-alignment QC (picard tools, deeptools, ATACseqQC): https://www.ncbi.nlm.nih.gov/pubmed/29490630
* Peak Calling (MASC2)
* Peak annotation (ChIPSeeker, GREAT)

11:00-12:00 NGS data and metadata management (Lecture and demo) [Instructor: Peter Harrison @ EBI]

* FAANG data and metadata submission
* Querying and downloading legacy NGS data from ENA SRA databases
***
## **APPENDIX: Resources for preparing yourself to meet the prerequisites**

### Resources for learning basics of UNIX (you can pick one of them you like most)

* UNIX Tutorial for Beginners: [http://www.ee.surrey.ac.uk/Teaching/Unix/](http://www.ee.surrey.ac.uk/Teaching/Unix/)
* Learning UNIX through exercises: [https://www.doc.ic.ac.uk/~wjk/UnixIntro/](https://www.doc.ic.ac.uk/~wjk/UnixIntro/)
* Learn Shell Programming - Free Interactive Shell Programming Tutorial: [https://www.learnshell.org/](https://www.learnshell.org/)
* Free Linux shell access is available here: [https://bellard.org/jslinux/](https://bellard.org/jslinux/) or [https://linuxzoo.net/](https://linuxzoo.net/)

### **Resources for learning R basics (you can pick one you like most)**

* Lear R, in R: [https://swirlstats.com/](https://swirlstats.com/)
* Quick R: [https://www.statmethods.net/r-tutorial/index.html](https://www.statmethods.net/r-tutorial/index.html)
* Introduction to R (DataCamp free course, providing interactive programming): [https://www.datacamp.com/courses/free-introduction-to-r](https://www.datacamp.com/courses/free-introduction-to-r)
* Free RStudion cloud for practice: [https://rstudio.cloud/](https://rstudio.cloud/)

#### Install R and RStudio
Please follow this link to install R and RStudio before workshop: [https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu](https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu)

