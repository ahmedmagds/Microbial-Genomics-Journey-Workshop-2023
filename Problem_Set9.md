# MGJW Problem set 9

## Kahoot
Let's test what we learnt last session [here]().

## WhatsGNU
### Download genomes
From the previous problem set, if you did not do that already download the genomes for 8 _Klebsiella pneumoniae_ isolates from NCBI using the WhatsGNU download script. The issue we had in class was just because of internet issue.<br/>

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
### Fixing Download Database
Download a WhatsGNU database. I tried to do this live in class and it did not work. We got this error `ModuleNotFoundError: No module named 'tqdm'`. The reason why is that I forgot to ask bioconda to automatically install the tqdm module for the user when I did the update. For the interim, to fix that, activate the whatsgnu environment and install tqdm for now. I will update the bioconda soon to fix this bug.<br/>
**N.B. tqdm is a module to give a download progress bar so the user can know how much time is left**
```
conda activate whatsgnu
pip intall tqdm
```
When you try to use it now to download a database.
```
WhatsGNU_db_download.py Kp
```
You will get another error `FileNotFoundError: [Errno 2] No such file or directory: '/Users/ahmed/miniconda3/envs/whatsgnu/databases_available.csv'`. The reason behind this error is that almost similar to the previous error. I just forgot to add the databases info file on bioconda. It exists on GitHub, but I forgot to add it to the Bioconda installation. I will fix it soon but for now, what can be done is downloading it and putting it in the right place. You should get that location already from that error or using this command `which WhatsGNU_main.py`. Now you need to change directory to that folder and download the file.

```
cd /Users/ahmed/miniconda3/envs/whatsgnu/
wget -O databases_available.csv https://raw.githubusercontent.com/ahmedmagds/WhatsGNU/master/databases_available.csv --no-check-certificate
mkdir db
WhatsGNU_db_download.py Kp
```
The command `mkdir db` needs to be created once. This will all be fixed in next release. It should work now. If you do not need this database, you can control+C or close the terminal to exit as the download will take 1-2 hours based on your download speed.

## K-mer
I would recommend going through the different [GWAS tutorials](https://pyseer.readthedocs.io/en/master/tutorial.html#gwas-tutorial) in this page. If you have time for only one tutorial then do the [PySeer K-mer association with mixed effects model tutorial](https://pyseer.readthedocs.io/en/master/tutorial.html#k-mer-association-with-mixed-effects-model). The data can be downloaded from [here](https://figshare.com/articles/dataset/pyseer_tutorial/7588832). Explanation for the different files is available as a table at the start of the [GWAS tutorials](https://pyseer.readthedocs.io/en/master/tutorial.html#gwas-tutorial)page.
**N.B. This command may not work `python scripts/qq_plot.py penicillin_kmers.txt`. Skip it and continue with the rest**

## Recombination
[ClonalFrameML tutorial](https://github.com/xavierdidelot/clonalframeml/wiki). the data for this tutorial should be downloaded from [here](https://doi.org/10.6084/m9.figshare.19626912). Just do the standard model example. This ClonalFrameML command may take 12 hours to run, I would recommend doing that overnight after business hours.
