# MGJW Problem set 8

## Kahoot
Let's test what we learnt last session [here]().

## WhatsGNU
In this problem set, you'll download the genomes for 8 _Klebsiella pneumoniae_ isolates from NCBI using the WhatsGNU download script.<br/>

1. **First you need to install WhatsGNU using mamba (hint: check whatsgnu github)**
2. **Create a problem_set8 directory in your MGJW folder.**
3. **Get the data.**
Download the FASTA formatted sequences of your genomes from NCBI using WhatsGNU_get_GenBank_genomes.py. Below are the accession numbers:
```
GCA_000240185.2
GCA_010183685.1
GCA_008121515.1
GCA_000775375.1
GCA_900493625.1
GCA_900496815.1
```
**If you are stuck in this step, use NCBI website as explained in the last problem set**
4. **Annotate the genomes downloaded with Prokka**
  * Create a subdirectory in your problem set 8 folder for the Prokka output files.
  * Standardize the annotations by running Prokka on these fasta files. Feel free to do this with a shell script if you're comfortable. *HINT* Be sure you are in the proper Conda environment. Use the accession number (e.g. GCA_010183685.1) as your locustag and prefix<br/>

### Exercise 1 (make your own WhatsGNU database)
* Follow the steps to make a basic/simple WhatsGNU database on WhatsGNU github. This will create a database in a `.pickle` file format that you will use.

### Exercise 2
1. Download WhatsGNU Klebsiella database (Hint: Check GitHub). It is around 4.53 GB.
**If you are stuck in this step, use this [link](https://zenodo.org/record/7812697/files/Kp.zip?download=1) to download the database**
2. Check Proteomic Novelty for the genomes. Make a directory and put the faa files from the annotation step in it. Run WhatsGNU_main.py. Hint: after you download the database, you will need to specify where it is on your computer. **Run WhatsGNU with top_genome feature and print out proteins with GNU=0.**
* How many novel alleles (GNU=0) in the 6 genomes?
* Which one has the most? Can you explain why it has that high number of alleles with GNU=0?
* Any observations about GCA_900493625.1?

3. Create Automated Visualization - Untargeted - Histogram with NOVEL_CONSERVED cutoff of 50 and 5000 and bin size of 100 and color blue.

4. Automated Visualization - Untargeted - Volcano plot
GCA_000240185.2, GCA_010183685.1 are control, while GCA_008121515.1 and GCA_000775375.1 are case.
* How many proteins are present only in one group vs the other?
