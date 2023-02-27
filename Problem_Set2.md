# MGJW Problem Set 2

## Kahoot
Let's test what we learnt last session [here](https://play.kahoot.it/v2/?quizId=b42d290a-be13-436d-80a7-de78873a50c5).

## Part 1
* Try this command to list all environments installed on your computer (conda info --envs)
* Try this command to list all packages installed in the mash environment (conda list -n mash)
* Let's try using FASTQC, you need to activate its environment first. You will use files from the folder that you downloaded for Problem Set 1.
```
conda activate fastqc
cd MGJW/problem_set1/fastq
gunzip *.gz
fastqc genome1_R1.fastq genome1_R2.fastq
```
Let's explore the html file. Windows User may need to copy the file to another directory so they can open the html file in a browser.
`cp /home/Ubuntu_user_name/MGJW/problem_set1/fastq/file.html /mnt/c/Users/Windows_username/Desktop/`
You will notice that the number of reads is really low. Some of our samples may fail quite a few quality metrics used by FastQC. However, this does not mean that our samples should be discarded as this may or may not be a problem for your downstream application.
* Let's check the mockdna sample in the fastq folder from problem set 1. Mash can handle one file for the sample so we will have to concatenate our reads.
```
conda activate mash
cd ~/MGJW/problem_set1/fastq
cat mockdna_R1.fastq mockdna_R2.fastq > mockdna_reads.fastq
gunzip ../RefSeqSketches.msh.gz
mash screen -w -p 8 ../RefSeqSketches.msh mockdna_reads.fastq > mockdna_screen_winning.tab
less mock_screen_winning.tab
sort -gr mockdna_screen_winning.tab > mockdna_screen_winning_sorted.tab
less mockdna_screen_winning_sorted.tab
```
* Let's check another sample from problem set 1 and see how this compares to previous results.
```
cd ~/MGJW/problem_set1/fasta
mash screen -w -p 8 ../RefSeqSketches.msh genome2.fasta > genome2_screen_winning.tab
sort -gr genome2_screen_winning.tab > genome2_screen_winning_sorted.tab
less genome2_screen_winning_sorted.tab
```

## Part 2
* `cd ~/MGJW/problem_set1/fasta`
* try `mash dist genome3.fna genome4.fna`
* Let's make our own mash database using `mash paste` for the three genomes in the fasta directory (`MGJW/problem_set1/fasta`)
  * `mash sketch -s 1000 -k 21 genome2.fasta`
  * `mash sketch -s 1000 -k 21 genome3.fasta`
  * `mash sketch -s 1000 -k 21 genome4.fasta`
  * `mash paste genomes.msh genome2.fasta.msh genome3.fasta.msh  genome4.fasta.msh`
  * `mash info genomes.msh | head -n 20`
* Save your history of commands in a file called commands_notes_PS2.txt
