# MGJW Problem Set 3

## Kahoot
Let's test what we learnt last session [here](https://play.kahoot.it/v2/?quizId=48f1299b-b610-4281-a0f0-8de0bea617cc).

## Note for file extections (multiple possible extenstions, but same file)
FASTA file Extensions: .fasta, .fna, .fa<br/>
FASTQ file Extensions: .fastq, .fq

## Part 1
**In my instructions, I do not necessarily instruct you to change directory to MGJW or any other one, use the skills you learnt in the Unix command sesstion to do that. Also, I may not instruct to activate the conda env in some cases.**

### shovill
**If you get an error saying `Cannot open temporary file kmc_00253.bin and Could not determine genome size from ''`, use `ulimit -n 2048` to increase limit for opened files for the kmc tool on MacOS.**<br/>

**You can skip this download step if you already donwloaded it for session 3.**<br/>
Let's assemble a genome. To follow the steps here, you need to download the files available [here](https://www.dropbox.com/scl/fo/8ni4qic39mb3ojapgci1x/h?dl=0&rlkey=p6gyncbcees8754go5s8votvu).
```
conda activate shovill
shovill --trim --assembler skesa --outdir out_S56 --R1 S56_R1.fastq --R2 S56_R2.fastq
conda activate mash
mash screen -w -p 8 ../../problem_set1/RefSeqSketches.msh contigs.fa > S56_screen_winning_contigs.tab
sort -gr S56_screen_winning_contigs.tab > S56_screen_winning_contigs_sorted.tab
less S56_screen_winning_contigs_sorted.tab | head -n 10
```
You will see that this assembled genome is very fragmented (around 3000 contigs). From Mash, you will be able to see that this genome is P. aeruginosa.

## Part 2

## quast
* Let's try quast! Go to NCBI and download the reference genome file for Pseudomonas aeruginosa (PAO1).
```
conda activate quast
quast.py contigs.fa -r GCF_000006765.1_ASM676v1_genomic.fna.gz
```
You will find quast_results folder and in there you will find the latest folder, you can double click report.html and it will open the file in your browser. What are the N50 and N90 for this genome?

* What is the L50 and L90 for genome4.fasta from problem_set1?

## busco
[Busco by Erin Theiller](BUSCO.md)

Let's assess contamination in some genome assemblies using single-copy orthologs! Notice on the same genome, we can check different taxonomical levels (bacteria_odb10, clostridiales_odb10, pseudomonadales_odb10).
```
conda activate busco
busco -i ~/MGJW/problem_set3/out_S56/contigs.fa -l bacteria_odb10 -o busco_op -m genome
busco -i ~/MGJW/problem_set1/fasta/genome4.fasta -l bacteria_odb10 -o busco_op2 -m genome
busco -i ~/MGJW/problem_set1/fasta/genome4.fasta -l clostridiales_odb10 -o busco_op3 -m genome
busco -i ~/MGJW/problem_set3/out_S56/contigs.fa -l pseudomonadales_odb10 -o busco_op4 -m genome
```
* What do you think of genome4.fasta vs contigs.fa?
* Which genome has better metrics?
* Why do you think this genome assembly is better?

## Part 3 for Session 4 Readiness
If you did not do that already in the Unix session, you will need to install Prokka and Abricate for Session 4. If you are not sure, you can check the installed environments using `conda info --envs`.<br/>

### Install Prokka
[Prokka](https://github.com/tseemann/prokka)
```
conda deactivate
mamba create -n prokka -c bioconda prokka
conda activate prokka
prokka
```
### Install abricate
[abricate](https://github.com/tseemann/abricate)
```
conda deactivate
mamba create -n abricate -c conda-forge -c bioconda -c defaults abricate
conda activate abricate
abricate --check
```
## Reproducibility
[Reproducibility by Daniel P. Morreale](Reproducability.md)
