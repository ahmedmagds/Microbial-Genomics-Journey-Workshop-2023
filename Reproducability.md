# Rigor and Reproducability in Bacterial Genomics

***Please note:*** Standards for rigor and reproducibility vary within bacterial genomics (and can vary even more in the bioinformatics world outside of bacteria). This document contians some imoprtant notes for getting started that are broadly applicable. There are a lot of things that can impact the final output of a microbial sequencing project and can cause programs to output different results. There are some important ones.

**Microbial genomics is, at its core, a bunch of statistical tests that are manipulated by you to generate a result. Like all statistics, there is some chance you will get the wrong answer. Each tool we use is essentially a slighlty different way of applying a statical test to the data. Because of this, you can get very different results even if the same dataset is used. This is compounded by a couple of factors: if ***you*** change any parameters (i.e. you change a numberical cut-off or k-mer size), if ***the developer*** changes any parameters, or if ***the data*** changes in any way.**<br/>

## The Golden Rule of bacterial genomics:<br/>
### ***BACK UP YOUR DATA MORE THAN YOU THINK IS NECESSARY.***<br/>
You cannot back up your computer too much. Do it now. You will never regret it. Back it up at least two places. Lost data is useless. <br />

## Data Fidelity <br />
#### Before you even get to a sequencer, you need to record kit numbers, lot numbers, and serial numbers for every step in library making (as well as the detailed protocol). For this class, I'm going to assume you are using a core do to these steps, in which case you should ask them for a methods section and key information you will need to publish this data. <br />
#### Once the core finishes their work, responsibility falls on you. All reads should be immediately uploaded to the [ Sequence Read Archive ](https://www.ncbi.nlm.nih.gov/sra). SRA is a public repository of sequencing data and its associated metadata. You will need to do this to publish. NCBI has a fairly easy to use interface that will walk you through the process, but it can be slow and tedious. In my opinion, it is best to do this as soon as possible.<br />
Until your project is ready for publication, you can embargo the reads to others cannot use them. Uploading to SRA ensures that other people can use your raw data in thier own analyses, and apply different pipelines as needed. It also acts as an additional backup should you lose some data.<br />

***SRA is an incredible resource and you can download reads for other strains/isolates you may want in your analysis so everything is analysed with the same pipeline.***<br />
<br />

## Version control<br />
#### Every so often, a program developer will release an updated version of a tool you need for your analysis. Typically, this is done to accomodate new kinds of data or fix a bug. However, new versions may analyze data slightly differently, and that might not be immediately clear unless you are really familiar with that analysis. One of the advantages to using Conda is version control for each tool (and project). <br />
***All analyses in a single dataset must be done using the same version of a tool. You must report the version in the methods section.*** Typically, you will write "read quality was assessed using FastQC v.X.X.X (citation)." In this class you installed FastQC v0.11.9. Make sure you record the version number of the tool used in each step. Most programs will tell you this information if you run one of these (example is with fastqc, but you can swap that for any tool):<br />

``` 
fastqc --version <br />
fastqc -v<br />
```
<br /><br />
## Using tools.
#### Each tool that you install will have default settings. These can be found by checking the Github page for that tool, and have been chosen by the developer to work in most cases. Typically they'll be good enough for you too. There are three good rules to follow in this case:<br /><br/>
### 1. If you don't know what a setting does, you can try it, but don't assume it gives better (i.e. more accurate) results.<br />
#### This essentially equates to trying all the multiple comparison corrections and choosing the one that's signficiant (a big no-no).<br />
### 2. When you call a command, type out each of the default settings, even if you don't have to.<br/>
#### You'll always know what you did exactly, especially if defaults change, or if you lose access too that tool. I personally believe in including ALL of this in a methods section as well, so its very helpful to be able to pull up a single command and write a sentence about it.<br/>
### 3. Record, verbatim, both the command and any error messages in a notebook or readme.txt file (see below).<br/>
#### Just like you should at the bench<br/><br/>

## Taking notes and Docker. <br />
### There are a lot of notebooks now that are amenable to recording your analyses. Personally, I (Dan) prefer to use a combination of Jupyter Notebook and readme.txt files. Jupyter is built for recording and notating commands, and I find it very intuitive. I also like a readme.txt in the folder with the output of the tool that gives the exact command that was run to genreate that result. That way there is no confusion. This is typically where I record the conda environment and the tool version information as well. <br />
### Docker is another system for building, in essence, sharable enviroments with all of the exact tools and settings used for a project. If you start building large workflows that you want to share, this may be for you. <br /><br/>

## Custom scripts.
### Sometimes, you will want to run the exact same command on a lot (more than 4) files. In this case, you may write a custom script to batch the process. Save these scripts onto a private repository on Github and in a dedicated folder for your project. They're a valuable resource. <br /><br/>

## **Take home messages:**
### 1. Back-up your data at every step so someone can identify any mistakes if need be.
### 2. If you don't know how to share some data so others can reproduce your study, ask!
### 3. Be sure to record your commands **exactly** as you ran them, with every flag and default setting.
