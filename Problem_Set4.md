# MGJW Problem Set 4

## Kahoot
Let's test what we learnt last session [here](https://play.kahoot.it/v2/?quizId=e266aef6-bfc6-4f94-8f1f-6db1845b03ff).

## Part 1

### Download needed files
* To follow the steps here, you need to download this file from this dropbox URL [problem_set4.tar.gz](https://www.dropbox.com/s/413incj5tfsp3wi/problem_set4.tar.gz?dl=0) and put it in MGJW or using `wget -O problem_set4.tar.gz https://www.dropbox.com/s/413incj5tfsp3wi/problem_set4.tar.gz?dl=0 --no-check-certificate`
* If you downloaded the file from the web and not using `wget`, you will need to move it to MGJW `mv ~/Downloads/problem_set4.tar.gz ~/MGJW`.
* Unarchive the file `tar -xf problem_set4.tar.gz -C ./`

### Prokka
**Prokka because of a blast version error (mac or windows), open your prokka environment and run the following command**
`conda install -c bioconda blast=2.9.0`
**This will roll back Blast to a compatible version (v2.9.0) and should fix the issue.**<br/>

Let's annotate an assembled genome with more options. Notice we use `--proteins` option which is a Fasta file of trusted proteins to first annotate from. This will be very helpful if you or the field have a way of naming the proteins in your species of interest and you want to be consistent. As this genome I gave you to annotate is C. difficle, I got the proteins file for the reference genome from the [C. difficile genome page](https://www.ncbi.nlm.nih.gov/genome/?term=difficile) on NCBI.
```
conda activate prokka
prokka -h
cd ~/MGJW/problem_set1/fasta
prokka --outdir annot_genome4 --kingdom Bacteria --locustag genom4 --proteins ../GCF_018885085.1_ASM1888508v1_protein.faa --genus Clostridioides --species difficile --prefix genome4 genome4.fasta
```
## Part 2

### ABRicate
[ABRicate](https://github.com/tseemann/abricate) is a tool for mass screening of contigs for antimicrobial resistance, virulence genes or genes of interest. It comes bundled with multiple databases: NCBI, CARD, ARG-ANNOT, Resfinder, MEGARES, EcOH, PlasmidFinder, Ecoli_VF and VFDB. You can also make your own database. Let's check the databases available in ABRicate. You can open abricate_DBs.csv in excel after you run these commands or using `less` command.
```
cd ~/MGJW/problem_set4/
conda activate abricate
cd ~/MGJW/problem_set4/abricate_genomes
abricate --list --csv > abricate_DBs.csv
less abricate_DBs.csv
```
Let's check the presense of the genes from the Virulence Factor Database (VFDB) in multiple samples and combine/summarize reports across multiple samples! It can take multiple files at once using wildcard `*.fasta` or using a text file having the names of the files you want to run `abricate --fofn test/files_names.txt`. Let's try the wildcard example!
```
abricate -db vfdb *.fasta > genomes.tab
abricate --summary results.tab > summary.tab
```
### Make your Own ABRicate Database
Let's make our own database of genes of interest! We will have to create this database in the folder where abricate dbs live. How can we know where they live? As we installed it with `conda`, it should be in the miniconda3/envs directory, but still how can we get to that path inside the terminal. We can use `which` command. This command will tell us where a specific tool lives.
```
which abricate
```
The output from this command is something similar to this `/Users/ahmed/miniconda3/envs/abricate/bin/abricate`. All what we need now is to change our diredctory to the conda env `abricate` main directory `/Users/ahmed/miniconda3/envs/abricate`. Of course your path would not have `ahmed` in it, it will be your username on your system. If you also wonder why we did not use the `/bin/abricate` part. `/bin` contains executable files and codes that you have in your system. It's like program files on Windows or application folder on MacOS. `abricate` is one of these applications/tools in the `bin` directory of the `abricate` env. IF you list the folders/files in this `abricate` env, you will find one folder called db. This is where we should make a new folder for our custom database.
```
cd /Users/ahmed/miniconda3/envs/abricate     
ls
cd db
mkdir Sau
cd Sau
cp ~/MGJW/problem_set4/abricate_genomes/Saureus_genes.fasta ./sequences
abricate --setupdb
```
At this step if we run `abricate --list`, we will see a db called Sau was created. Now we can start using the db. We can also use different minimum identity `--minid` and coverage `--mincov` as needed and appropriate [Default: 80%].
```
cd ~/MGJW/problem_set4/abricate_genomes
abricate --minid 30 --minco 70 --db Sau genome3.fasta > genome3_Sau.tab
```
Did the second command work? Try to fix it!<br/>
Note: **ABRicate uses blast make database command to make this database `makeblastdb -in sequences -title tinyamr -dbtype nucl`**

## Part 3 for Session 5 Readiness
If you did not do that already in the Unix session, you will need to install Blast and MLST for Session 5. If you are not sure, you can check the installed environments using `conda info --envs`.<br/>

### Install mlst
[mlst](https://github.com/tseemann/mlst)
```
mamba create -n mlst -c conda-forge -c bioconda -c defaults mlst
conda activate mlst
mlst
```
### Install blast
[blast](https://www.ncbi.nlm.nih.gov/books/NBK279690/)
```
mamba create -n blast -c bioconda blast
conda activate blast
blastn
```
