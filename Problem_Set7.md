#MGJW Problem set 7<br/><br/>

#Background:<br/>
####You've made it far enough now that we can start chaining tools together into (small) pipelines to ask questions about the pangenome of species or isolates of interest. While we've spent most of our examples and problems sets working on S. aureus genomes, we're going to change it up and take a look at a bacterial species with an infamously large pangenome: Neisseria meningitidis. N. meningitidis is a common human commensal, and one of the major causes of bacterial meningitis world-wide. If you're in the US, you've likely been vaccinated against the most common circulating serotypes (called A, C, W, and Y), and possibly one other serotype responsible for some particularly nasty infections (B).<br/>

####In this problem set, you'll collect the genomes for 8 isolates from NCBI. This is going to include a reference serotype B strain, called MC58. This strain is very well characterized and one of the major lab strains studied in recent literature.<br/>

####I'll be providing minimal instructions; do your best to work through this and do hesitate to ask us questions. When I do include code, I have left sections it very generic. Some text will need to be replaced with the specifics for your device or strain.<br/>

####Since we haven't discussed downloading public data in class, I made a short video showing how its done:<br/><br/>
![Video_instructions] (Downloading_fasta.mov)<br/><br/>
Breifly, we navigate to NCBI's nucleotide database, paste in our accession number in the search bar, and hit search. This will bring up the record for this isolate. You then click on "Send to" in the upper right corner of the white box, then click "File", and finally select "FASTA" from the Format drop down box. This will automatically download the record to your computer. BE SURE TO RENAME THEM WITH THE PROPER IDENTIFIER WHEN YOU MOVE THEM INTO A FOLDER FOR THE PROBLEM SET.<br/>

##Get the data
####1. Download the FASTA formatted sequences of your genomes from NCBI. Create a problem_set_7 directory in your MGJW folder and move these files into that folder.<br/>
####Below are the accession numbers:
* NC_003112.2
* NC_017516.1
* NZ_CP009419.1
* NZ_CP007668.1
* NC_017517.1
* NC_017515.1
* NC_017505.1
* NC_017512.1

##Part 1<br/>
###Prokka<br/>
####2. Create a subdirectory in your problem set 7 folder for the Prokka output files.
####3. Standardize the annotations by running Prokka on these fasta files. Feel free to do this with a shell script if you're comfortable. *HINT* Be sure you are in the proper Conda environment. <br/>

```
prokka --outdir <STRAIN NAME> --kingdom Bacteria --prefix <STRAIN NAME> --gcode 11 --genus Neisseria --gram neg <file>.fasta

```

#### How many CDS did prokka identify? Can you figure out how many were identified when the submitted ran PGAP during the upload process? *Hint* Check the genome record page in the nucleotide database where you downloaded everything from.<br/>

##Part 2<br/>
###Roary<br/>
####4. Collect the gff files from Prokka (again either by hand or with a script if you are able), and create a subdirectory called <./Roary/>.
####5. Run Roary. What do all of these options I'm having you include mean?<br/>
```
roary -f <OUTPUT DIRECTORY> -e -n -i 95 -cd 99 -r -s *.gff

```
####6. Run Roary again, changing the cut-off scores (-i) to 75, then again with 50. How do the results change?<br/>
####7. Run Roary one last time:<br/>
```
roary -f <OUTPUT DIRECTORY> -e -n -i 95 -cd 90 -r -s *.gff

```
####How did this change the results?
