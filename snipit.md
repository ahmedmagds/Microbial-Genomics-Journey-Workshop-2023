# Visualizing Your SNPs


After running *snippy*, run *snippy-core* to generate the core SNP alignment. 


Optionally, if you are running a set of isolates against one reference, you can use the *snippy-multi* script that is packaged with *snippy*. *Snippy-multi* will also run *snippy-core* at the end of the analysis.

There are many tools that can summarize your SNPs relative to a reference sequence. However, we will focus on using *snipit* today.
 
### Installing snipit
```
pip install snipit
```

### Running snipit
```
snipit core_SNP_alignment.aln --solid-background -r Reference 
```

### Looking at the output 

![snipit output](MGJW_SnipitFigure.pdf)

For more information, check out the GitHub page: https://github.com/aineniamh/snipit


