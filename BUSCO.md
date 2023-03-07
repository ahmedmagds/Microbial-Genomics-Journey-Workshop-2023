# Microbial Genomics Journey Workshop 2023
## Understanding BUSCO

##What is BUSCO?
BUSCO (**B**enchmarking **U**niversal **S**ingle-**C**opy **O**rthologs) is an open-source tool that is used to assess the quality of the genome assembly and annotation completeness based on evolutionarily informed expectations of gene content. 

##Installing BUSCO
````
conda create --name busco -c conda-forge -c bioconda busco
conda activate busco
````

##Running BUSCO
````
busco -i ~/MGJW/problem_set3/assemblies/genomeX.fa -o genomeX_busco -m genome --auto-lineage-prok
busco -i ~/MGJW/problem_set3/assemblies/genomeY.fa -o genomeY_busco -m genome --auto-lineage-prok
````
**A Note About the Lineage Parameter**: BUSCO employs clade-specific information to identify BUSCO genes in the analyzed sequence. The lineage can be specified by the user or selected automatically (in the case of bacteria) 

##Output Files
The output directory will contain several files and directories. Among these files are the summary files that report the completeness status of the genome in BUSCO notation. There are two types of summary files - a generic summary and a specific summary. The generic format utilizes a less specific lineage to analyze the BUSCO genes while the specific format utilizes a more specific lineage for the analysis. The summaries come in two formats: JSON format and plain text format. The *.txt* files will be used to generate the plot.  

##Using *generate_plot.py*
This script allows users to view their BUSCO summary results in an easy-to-view bar chart. This script comes with the package when you install it via Conda. To run *generate_plot.py*:
````
# make a directory where you will move the BUSCO summaries
mkdir BUSCO_summaries

# copy the short summary files from each of the runs that you want to plot into the folder you just made
cp genomeX_busco/short_summary.specifc_enterobacterales_odb10.genomeX.txt BUSCO_summaries/.
cp genomeY_busco/short_summary.specific.enterobacterales_odb10.genomeY.txt BUSCO_summaries/.

# In your active BUSCO environment, run this command to find the full path to the script
whereis generate_plot.py

# Now run the script on your BUSCO_summaries directory
~/miniconda/envs/busco/bin/generate_plot.py -wd BUSCO_summaries/ -rt <generic|specifc> 
````
Running this script will produce a png image in the BUSCO_summaries/ directory

![BUSCO Graph](/Users/erintheiller/Downloads/busco/busco_figure.png)

For more information, see their documentation: https://busco.ezlab.org/
