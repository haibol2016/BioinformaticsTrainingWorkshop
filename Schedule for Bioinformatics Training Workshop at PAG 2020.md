<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Schedule for Bioinformatics Training Workshop at PAG 2020</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#schedule-for-bioinformatics-training-workshop--pag2020">Schedule for Bioinformatics Training Workshop @ PAG2020</a>
<ul>
<li><a href="#day-1-jan-8-2020-200-pm---530-pm">Day 1: Jan 8, 2020 (2:00 PM - 5:30 PM)</a></li>
<li><a href="#day-2-jan-9-2020-800-am--530-pm">Day 2: Jan 9, 2020 (8:00 AM -5:30 PM)</a></li>
<li><a href="#day-3-jan-10-2020-800-am--1200-pm">Day 3: Jan 10, 2020 (8:00 AM -12:00 PM)</a></li>
<li><a href="#appendix-resources-for-preparing-yourself-to-meet-the-perquisites">APPENDIX: Resources for preparing yourself to meet the perquisites</a></li>
</ul>
</li>
</ul>

    </div>
  </div>
  <div class="stackedit__right">
    <div class="stackedit__html">
      <h1 id="schedule-for-bioinformatics-training-workshop--pag2020"><strong>Schedule for Bioinformatics Training Workshop @ PAG2020</strong></h1>
<blockquote>
<p><strong>Notice: For participants do not meet the perquisites, the workshop will start at 2:00PM January 8, 2020; while for advanced participants who have basic knowledge of and experience with UNIX command line and R programming language are expect to start at 8:00 AM January 9, 2020. To prepare yourself for the workshop, you can teach yourself some basic Unix and R via the resources provided by the appendix</strong>.</p>
</blockquote>
<hr>
<h2 id="day-1-jan-8-2020-200-pm---530-pm"><strong>Day 1: Jan 8, 2020 (2:00 PM - 5:30 PM)</strong></h2>
<p>2:00-2:20 registration and welcome</p>
<p>2:20-3:20 Introduction to UNIX command line (Lecture and hands-on session)</p>
<ul>
<li>Intro to the UNIX operating system (ssh, terminal, file system, …)</li>
<li>Handle paths and files (ls, cp, cd, mkdir, rm, find, touch, mv, ln, …)</li>
<li>View content of files (less, more, head, tail, vi, cat)</li>
<li>Search content of files (grep, wildcards)</li>
<li>Get help (man)</li>
<li>File permission (chmod)</li>
<li>Process and job</li>
<li>Resource management (df, du, quota, …)</li>
<li>Other goodies (gzip/unzip, tar, zcat, cut, paste, join, file, diff, history,…)</li>
</ul>
<p>3:20-3:30 break</p>
<p>3:30-5:30 Introduction to R (Lecture and hands-on session)</p>
<ul>
<li>R/RStudio installation and setup</li>
<li>Data structure and manipulation (vector, list, array/matrix, data frame/table)</li>
<li>Data input and output</li>
<li>Get help</li>
<li>Flow control (if/else, for, while, repeat, break, next)</li>
<li>Function</li>
<li>Intro to Object oriented programming in R (S3/S4 classes, reference class).</li>
<li>R package management</li>
<li>Different modes of running R scripts (interactive and batch)</li>
</ul>
<hr>
<h2 id="day-2-jan-9-2020-800-am--530-pm"><strong>Day 2: Jan 9, 2020 (8:00 AM -5:30 PM)</strong></h2>
<p>8:00-8:15 registration and welcome</p>
<p>8:15 -9:15 Advanced Unix command line (Lecture and hands-on session)</p>
<ul>
<li>Regular expression, sed, awk, xargs</li>
<li>pipe, redirection, shell scripting</li>
<li>Large data management: download, copy, backup(scp, ftp, ascp, rsync)</li>
<li>Software installation and management (module, configuration file, …)</li>
<li>HPC job management systems (one  or 2 out of SLURM, SGE, PBS, LSF, …)</li>
</ul>
<p>9:15-9:25 break</p>
<p>9:25-10:55 Exploratory data analysis and Data visualization in R (Lecture and hands-on session)</p>
<ul>
<li>
<p>Descriptive statistics (frequency, correlation, statistic tests, PCA, MDS, hierarchical clustering)</p>
</li>
<li>
<p>R graphics (base, ggplot2): all types of plots, and heatmap</p>
</li>
</ul>
<p>10:55-11:05 break</p>
<p>11:05-12:00 Introduction to some very useful R/BioConductor packages (mainly lecture and some hands-on session)</p>
<ul>
<li>Readr, data.table</li>
<li>tidyverse, dplyr,reshape2,  tidyr, stringr, lubridate</li>
<li>ggvis, plotly, htmlwidgets, googleVis, threejs</li>
<li>lme4/nlme, survival, caret</li>
<li>shiny, rmarkdown, solidify</li>
<li>For more useful packages, see <a href="https://awesome-r.com/">https://awesome-r.com/</a></li>
</ul>
<p>12:00PM-1:00 lunch</p>
<p>1:00-2:00 Introduction to NGS technologies, common types of genomics data, and handling tools (Lecture)</p>
<ul>
<li>Illumina and other sequencing technologies</li>
<li>fastq, fasta, BAM, SAM, bed, gtf, gff, bigwig, …</li>
<li>read QC: FASTQC, MultiQC</li>
<li>Short read aligners, SAMtools, picardtools, bedtools-</li>
<li>Visualization: IGV, UCSC genome browser</li>
</ul>
<p>2:00-2:10 break</p>
<p>2:10-3:20 RNA-seq data analysis (I) (Lecture) (<a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/">https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4728800/</a>)</p>
<ul>
<li>Experimental design, RNA-seq data characteristics</li>
<li>Read QC (FASTQC, MultiQC)</li>
<li>Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)</li>
<li>Alignment (STAR, HISAT, GMAP/GSNAP, RSEM) or semi-alignment (Kallisto, Salmon)</li>
<li>Post –alignment QC (QoRT)</li>
<li>Optional post-alignment process: multi-mapper assignment (MMR)</li>
<li>Count summary (featureCounts)</li>
<li>Exploratory analysis (PCA, hierarchical clustering)</li>
<li>Differential expression analysis (statistic model selection): DESeq2/edgeR, Voom</li>
<li>Gene ontology and pathway analysis</li>
<li>Network analysis</li>
</ul>
<p>3:20-3:30 break</p>
<p>3:30-5:30 RNA-seq data analysis (II) (hands-on session)</p>
<ul>
<li>Exercises with toy data for QC and mapping, and real life count table for DEG, discussion</li>
</ul>
<p>5:30-6:30 ChIP-seq data analysis (I) (Lecture): <a href="https://www.ncbi.nlm.nih.gov/pubmed/22955991">https://www.ncbi.nlm.nih.gov/pubmed/22955991</a></p>
<ul>
<li>Experimental design, data characteristics</li>
<li>Read QC (FASTQC, MultiQC)</li>
<li>Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)</li>
<li>Alignment (bwa-mem, bowtie/bowtie2)</li>
<li>Post-alignment QC (picard tools, deeptools, ChIPQC, SPP)</li>
<li>Peak Calling (MASC2, MUSIC/BCP)</li>
<li>Peak annotation (ChIPSeeker, GREAT)</li>
</ul>
<hr>
<h2 id="day-3-jan-10-2020-800-am--1200-pm"><strong>Day 3: Jan 10, 2020 (8:00 AM -12:00 PM)</strong></h2>
<p>8:00-10:00 ChIP-seq data analysis (II) (Hands-on session)</p>
<ul>
<li>Exercises, discussion</li>
</ul>
<p>10:00-10:10 break</p>
<p>10:10-11:00 ATAC-seq data analysis (Lecture, providing scripts for analysis)</p>
<ul>
<li>Experimental design, data characteristics</li>
<li>Read QC (FASTQC, MultiQC)</li>
<li>Optional preprocessing: trimming (Trimmomatic/Trim Galore), error correction (Rcorrector)</li>
<li>Alignment (bwa-mem, bowtie/bowtie2)</li>
<li>Post-alignment QC (picard tools, deeptools, ATACseqQC): <a href="https://www.ncbi.nlm.nih.gov/pubmed/29490630">https://www.ncbi.nlm.nih.gov/pubmed/29490630</a></li>
<li>Peak Calling (MASC2)</li>
<li>Peak annotation (ChIPSeeker, GREAT)</li>
</ul>
<p>11:00-12:00 NGS data and metadata management (Lecture and demo) [Instructor: Peter Harrison @ EBI]</p>
<ul>
<li>FAANG data and metadata submission</li>
<li>Querying and downloading legacy NGS data from ENA SRA databases</li>
</ul>
<hr>
<h2 id="appendix-resources-for-preparing-yourself-to-meet-the-perquisites"><strong>APPENDIX: Resources for preparing yourself to meet the perquisites</strong></h2>
<h3 id="resources-for-learning-basics-of-unix-you-can-pick-one-of-them-you-like-most">Resources for learning basics of UNIX (you can pick one of them you like most)</h3>
<ul>
<li>UNIX Tutorial for Beginners: <a href="http://www.ee.surrey.ac.uk/Teaching/Unix/">http://www.ee.surrey.ac.uk/Teaching/Unix/</a></li>
<li>Learning UNIX through exercises: <a href="https://www.doc.ic.ac.uk/~wjk/UnixIntro/">https://www.doc.ic.ac.uk/~wjk/UnixIntro/</a></li>
<li>Learn Shell Programming - Free Interactive Shell Programming Tutorial: <a href="https://www.learnshell.org/">https://www.learnshell.org/</a></li>
</ul>
<h3 id="resources-for-learning-r-basics-you-can-pick-one-you-like-most"><strong>Resources for learning R basics (you can pick one you like most)</strong></h3>
<ul>
<li>Lear R, in R: <a href="https://swirlstats.com/">https://swirlstats.com/</a></li>
<li>Quick R: <a href="https://www.statmethods.net/r-tutorial/index.html">https://www.statmethods.net/r-tutorial/index.html</a></li>
<li>Introduction to R (DataCamp free course, providing interactive programming): <a href="https://www.datacamp.com/courses/free-introduction-to-r">https://www.datacamp.com/courses/free-introduction-to-r</a></li>
</ul>
<h4 id="install-r-and-rstudio">Install R and RStudio</h4>
<p>Please follow this link to install R and RStudio before workshop: <a href="https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu">https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu</a></p>

    </div>
  </div>
</body>

</html>
