# Microbial Genomics Journey Workshop 2023
## Session 2: Quality Control

## Intro
The sequencing of microbial genomes has become a common practice for various applications, including identifying clusters, investigating outbreaks, and monitoring emergence and evolution of pathogens. To ensure accurate downstream analysis, it is essential to perform thorough quality control on the raw data.<br/>

The quality of assemblies is influenced by various factors, including coverage depth, genome coverage, and read accuracy (Q30). Additionally, sequencing biases such as GC bias and intergenus and intragenus contaminations can affect the integrity of assemblies and downstream analyses. It is often difficult to determine the cause of anomalies in assemblies, and assemblies may appear normal but be compromised, potentially disrupting subsequent analyses.<br/>

Thinking of quality starts early on in the project.

**How many reads do you need per genome?**
The Lander/Waterman equation is a method for computing coverage:<br/>
C = aLN / G<br/>
• C stands for coverage<br/>
• a stands for % abundance of genome in sample (for single genome, it is 1)<br/>
• G is the haploid genome length (3 Mbp for S.aureus)<br/>
• L is the read length (300 bases)<br/>
• N is the number of reads<br/>

Example:<br/>
Coverage for a 3 Mbp genome (e.g. S. aureus) = 1.0 * (5e5 reads) * (300 bp) / (3e6 bp) = 50x<br/>
Generally 30-50x for a bacterial genome is considered okay in the field. I usually shoot for 100x average coverage for a bacterial genome (around 1 Million reads (300 bases each) for a 3 Mbp genome). More depth (e.g. 1000x) may be needed in some cases if you are trying to answer more complicated questions like transmission networks while accounting for within host evolution.

**How can you confirm the integrity of the transferred data?**
MD5 (Message Digest Algorithm 5) is a hash function which calculates a hash value (MD5 value, 32-digit numbers and letters) of a given file to verify integrity of transmitted data. Single letter change will result in a completely new hash value. You can use it on Mac `md5 file_name` and linux `md5sum file_name`. Let's try it on problem_set1.tar.
```
md5 problem_set1.tar.gz
```
You should get this md5 value (cf3b9580499abd3606c0a535308c3dd6). When the core will send you the data, they will include checksum value that you compare with the value you will get on your end. If all fine, you should get an identical value. **Add this command/concept to your Knowledge Map**.

## FASTA
In FASTA format the line before the nucleotide sequence, called the FASTA definition line, must begin with a carat (">"), followed by a unique SeqID (sequence identifier). The SeqID must be unique for each nucleotide sequence and should not contain any spaces. It is optionally be followed by a text description of the sequence. Softwares may ignore this as it is optional, when it is present. One or more lines containing the sequence itself (convention is 60 characters per line).<br/>

One or more lines containing the sequence itself.

```
>seq1 Staphylococcus aureus
TGCACCAAACATGTCTAAAGCTGGAACCAAAATTACTTTCTTTGAAGACAAAAACTTTCA
AGGCCGCCACTATGACAGCGATTGCGACTGTGCAGATTTCCACATGTACCTGAGCCGCTG
CAACTCCATCAGAGTGGAAGGAGGCACCTGGGCTGTGTATGAAAGGCCCAATTTTGCTGG
```
Multifasta File (e.g. assembly file or alignment file)
```
>contig1 Staphylococcus aureus Strain xyz
TGCACCAAACATGTCTAAAGCTGGAACCAAAATTACTTTCTTTGAAGACAAAAACTTTCA
AGGCCGCCACTATGACAGCGATTGCGACTGTGCAGATTTCCACATGTACCTGAGCCGCTG
CAACTCCATCAGAGTGGAAGGAGGCACCTGGGCTGTGTATGAAAGGCCCAATTTTGCTGG
>contig2 Staphylococcus aureus Strain xyz
AAAAACCCTGGGGGGGAAAGCTGGAACCAAAATTACTTTCTTTGAAGACAAAAACTTTCA
ATTTCCACATGTTGACAGCGATTGCGACTGTGCAGATTTCCACATGTACCTAATTACTTT
TTGCGACTTCAGAGTGGAAGGAGGCACCTGGGCTGTGTATGAATGTCATCGAGGGTTAAC
```
**Tips**
* Usually if this file contains an assembly of one single bacterial genome (e.g. S. aureus), its size in megabytes (3 MB) will equal the number of bases (3 Mbp).
* This can be used for quick check of a contamination. If you are expecting a S. aureus genome and your file size on the computer is 9 Megabytes, this is a contamination signal (more one genome in this file).
* Without using any sophisticated tool, you can count the number of records in a fasta file using the `grep -c '>' file_name`

## FASTQ
FASTQ format is a text format for storing both a nucleotide sequence and
its corresponding quality scores. Both the sequence letter and quality score are
each encoded with a single ASCII character. For each cluster that passes filter,
a single sequence is written to the corresponding sample’s R1 FASTQ file, and, for a paired-end run,
a single sequence is also written to the sample’s R2 FASTQ file.<br/>
Each entry in a FASTQ files consists of 4 lines:
* A sequence identifier with information about the sequencing run and the cluster.
* The sequence (the base calls; A, C, T, G and N).
* A separator, which is simply a plus (+) sign.
* The base call quality scores. These are Phred +33 encoded, using ASCII characters to represent the numerical quality scores.
**What should I look for?**
* A quality score of 30 (Q30) represents an error rate of 1 in 1000 (meaning every 1000 bp sequencing read may contain an error), with a corresponding call accuracy of 99.9%.
* Illumina sequencing chemistry delivers high accuracy, with a vast majority of bases scoring Q30 and above. This level of accuracy is ideal for a range of sequencing applications, including clinical research. This is why Q30 is considered a benchmark for quality in next-generation sequencing (NGS).
* Common use for Q-score is to filter bases or entire reads if a quality threshold is not met.
* Common tools to use quality scores: The main purpose for these scores is to provide evidence that the data/conclusions are in fact real and not due to a sequencing errors. QC check softwares, Aligners, Variant detection calling tools, and Assemblers.

**Example of read 1:**
```
@A00901:812:HMWMCDRX2:1:2101:7211:1360 1:N:0:TATCCGAGGC+CTTATTGGCC
TTGCTGAAGCCTTCGATCTGCGCGACCAGCCGTTCGGCGAAATCCTCGCTGATGCCGCGCTCGAGCATGCCGGCGATCAGCTTGGGACGGAATTCGCCGATGTCGCCGCTCGACTTGAACGTCGCCATTGCACGACGCAGCCGGTCCGCCT
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:
@A00901:812:HMWMCDRX2:1:2101:15347:2863 1:N:0:TATCCGAGGC+CTTATTGGCC
CCACCTATCTGCTGGTCAGCTCGGTCGGCATCGAGTTCGGCGAGCATCAGCGCTTCACGCTCAAATGGGCACTCGGCTCGTGCCTGGTGTTCGGCCTGGCGGCGATCCTCACCGGCGCGTTCCCCTTCGTCGCGGCCGGTGGCTGAGCCGG
+
#######################################################################################################################################################
```
* Notice the F and # which correspond to a quality score of 37 and 2, respectively. # or Q-score of 2 is very poor quality. You can check Quality Score Encoding [here](https://support.illumina.com/help/BaseSpace_OLH_009008/Content/Source/Informatics/BS/QualityScoreEncoding_swBS.htm).
Let's check Illumina [Flowcell](https://www.google.com/search?q=illumina+flow+cell+lanes+and+tiles&rlz=1C5GCEA_enUS1010US1013&sxsrf=AJOqlzVu7fp1Q95WS-L4_n5IuO6K7T-XAA:1677069016513&source=lnms&tbm=isch&sa=X&ved=2ahUKEwj_z_j3kKn9AhXXlIkEHZwVAQsQ_AUoAXoECAEQAw&biw=1728&bih=891&dpr=2) to understand the concepts of lanes and tiles.
* Read identifier Explanation
  * Each sequence identifier line starts with @.
  * A00901 (instrument name)
  * 812 (run id on the instrument)
  * HMWMCDRX2 (flowcell identifier)
  * 1 (lane)
  * 2101 (tile number)
  * 15347 (x-cordinate of the cluster within the tile)
  * 2863 (y-cordinate of the cluster within the tile)
  * 1 (the member of the pair, it will be 2 for its mate)
  * N (if the read is filtered)
  * 0 (control bits)
  * TATCCGAGGC+CTTATTGGCC (Index of the read)

**Example of read 2 from the same genome:**
Multifasta File (e.g. assembly file or alignment file)
```
@A00901:812:HMWMCDRX2:1:2101:7211:1360 2:N:0:TATCCGAGGC+CTTATTGGCC
TCCAGGAACAGGCGATGCAAGTCGCGATCGTCGGCGCCGGCTTCACGCCGGGCGAGGCGGACCGGCTGCGTCGTGCAATGGCGACGTTCAAGTCGAGCGGCGACATCGGCGAATTCCGTCCCAAGCTGATCGCCGGCATGCTCGAGCGCGG
+
FFFFFFF,FFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
@A00901:812:HMWMCDRX2:1:2101:15347:2863 2:N:0:TATCCGAGGC+CTTATTGGCC
GGTCAGGGTGACGGACAGCAGGACCAGCGCCGCCCATTGCCGGAAACGCGCGGCCCGCGCCGGCTCAGCCACCGGCCGCGACGAAGGGGAACGCGCCGGTGAGGATCGCCGCCAGGCCGAACACCAGGCACGAGCCGAGTGCCCATTTGAG
+
FFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFF
```
**How can you count number of reads in a file using a unix command?**
`wc -l list_directory.txt | awk '{print $1/4}'`
Sometimes the number of reads in sample1_R1.fastq is different from sample1_R2.fastq. This means there are orphan reads in one file and they do not have mates.

**What does happen on the sequencer? Why is that important?**
![Adapter](Illumina_adapters.jpg)
Illumina Figure showing a paired-end flow cell for MiSeq, HiSeq 2000/2500 and NovaSeq 6000<br/>
Adapter trimming is the process of removing adapter sequences from the 3’ ends of reads. Adapter sequences should be removed from reads because they interfere with downstream analyses, such as alignment of reads to a reference. it is necessary to eliminate adapter sequences from reads. These adapter sequences contain important elements, including the sequencing primer binding sites, the index sequences, and the sites that facilitate the attachment of library fragments to the flow cell lawn. We will discuss adapter trimming in the next session.

## Metagenomics
* Metagenomics is the study of genomic sequences obtained directly from an environment/sample.
* Whole-genome “shotgun” metagenomic sequencing: sequence all DNA present (not restricted to bacteria/archaea).
* Our course focuses on single bacterial genomes. However, I would like to think in a metagenomic fashion. If you started in the lab with a S. aureus isolate, you would expect your sample to be 100% S. aureus when you sequence the DNA.
* Some sequence similarity may be shared on different taxonomic levels (e.g. all Staph will share some sequences).
* Every sample is considered a Metagenomic sample until evidences are established to prove a single genome.

## FASTQC
A quality control tool for high throughput sequence data. It does some quality control checks on raw sequence. It gives a quick overview of in which areas there may be problems. Here is an example of [good](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and [bad](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html) reports.
Let's try using FASTQC, you need to activate its environment first. You will use files from the folder that you downloaded for Problem Set 1.
```
conda activate fastqc
cd MGJW/problem_set1/fastq
gunzip *.gz
fastqc genome1_R1.fastq genome1_R2.fastq
```
Let's explore the html file. Windows User may need to copy the file to another directory so they can open the html file in a browser.
`cp /home/Ubuntu_user_name/MGJW/problem_set1/fastq/file.html /mnt/c/Users/Windows_username/Desktop/`
You will notice that the number of reads is really low. Some of our samples may fail quite a few quality metrics used by FastQC. However, this does not mean that our samples should be discarded as this may or may not be a problem for your downstream application.

**So you are telling me, I will run this for each sample and check the report and plot for each sample in a project of 100 samples. Good Point, let's check the next section!**

## MultiQC
Aggregate results from bioinformatics analyses across many samples into a single report.
Let's try using MultiQC to summarize all reports in a folder (fastq) and open the html file.
```
conda activate multiqc
cd ~/MGJW/problem_set1/fastq
multiqc .
```
## Mash
It is a Fast genome and metagenome distance estimation using MinHash. It turns large sequences to small, representative sketches. It compares a pair of genomes and approximates similarity between them in nucleotide content. It can be used for checking contamination and screening of your newly sequenced genome against the entire RefSeq database. You will need your genome and a mash RefSeq database (included in the folder that you downloaded).<br/>
Let's check the mockdna sample in the fastq folder. Mash can handle one file for the sample so we will have to concatenate our reads.
```
conda activate mash
cd ~/MGJW/problem_set1/fastq
cat mockdna_R1.fastq mockdna_R2.fastq > mockdna_reads.fastq
gunzip ../RefSeqSketches.msh.gz
mash screen -w -p 8 ../RefSeqSketches.msh mockdna_reads.fastq > mockdna_screen_winning.tab
less mockdna_screen_winning.tab
sort -gr mockdna_screen_winning.tab > mockdna_screen_winning_sorted.tab
less mockdna_screen_winning_sorted.tab
```
Let's check another sample and see how this compares to previous results.
```
cd ~/MGJW/problem_set1/fasta
mash screen -w -p 8 ../RefSeqSketches.msh genome2.fasta > genome2_screen_winning.tab
sort -gr genome2_screen_winning.tab > genome2_screen_winning_sorted.tab
less genome2_screen_winning_sorted.tab
```
---
## Further Readings
* [Mash](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x)
* [Kraken](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2014-15-3-r46) Ultrafast metagenomic sequence classification using exact alignments
* [bioconda](https://bioconda.github.io/)
