# Microbial Genomics Journey Workshop 2023
## Session 5: Blast database and Typing/MLST

## Intro

There are.....

## blast
Let's make our blast database!
```
conda activate blast
cat ../outputs/annot_genome3/genome3.faa ../outputs/annot_genome4/genome4.faa > genomes_3_4_combined.faa
blastp -query Two_genes.faa -db genomes_3_4_combined.faa -max_target_seqs 5 -outfmt '6 qseqid sacc evalue qcovs pident' -out Two_genes_output.txt
less Two_genes_output.txt
```
## MLST
[MLST](https://github.com/tseemann/mlst) is a tool for screening contig files against traditional [PubMLST](https://pubmlst.org/) typing schemes.
```
conda activate mlst
cd ~/MGJW/problem_set4/abricate_genomes
mlst genome4.fasta
```

## Further Readings
* [Misunderstood parameter of NCBI BLAST impacts the correctness of bioinformatics workflows](https://academic.oup.com/bioinformatics/article/35/9/1613/5106166?login=true) Take care of how you use max_target_seqs.
* [Review: Contamination detection in genomic data](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-022-02619-9)
