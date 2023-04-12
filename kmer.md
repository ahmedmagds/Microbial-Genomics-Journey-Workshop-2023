# Microbial Genomics Journey Workshop 2023
## Session 9: Kmer analysis and Recombination
### Kmer analysis
So far we have covered multiple comparative genomics approaches such as:
1. reference based analysis - identifying single nucleotide polymorphisms in a query genome after mapping to a reference.
2. Pangenome - Identifying the total set of genes in a species or dataset.
3. Panallelome - Even if the protein is present in all isolates, how different the alleles of each protein are in the isolates, which may have functional variations downstream.

Genomic data repositories for bacteria offer immense potential to understand evolutionary responses to environmental changes like antibiotic use. Genome-wide association studies (GWAS) for bacterial phenotypes have only recently emerged, with standard methods applicable to core genome mutations. However, these methods may not fully capture genetic determinants of phenotypic variation. Instead, alignment-free methods using k-mers have been introduced to address various types of genomic variation. Now capturing group-specific sequences between two groups of genomic/metagenomic sequences will complement these previous approaches. One important advantage of this approach will be identifying unique kmers in the intergenic regions. Sequence element enrichment analysis (SEER), a scalable method for thousands of genomes, can accurately detect associations with antibiotic resistance and discover novel invasiveness factors. A more recent comprehensive tool "pyseer" brings these techniques together in one package tailored to microbial pangenome-wide association studies.<br/>

#### pyseer
##### Installation
```
mamba install -c bioconda pyseer
conda activate pyseer
mamba install -c bioconda fsm-lite
```
##### Usage
We will use the 4 S. aureus genomes (2 NAE and 2 SAE) from the previous sessions.<br/>
In the first step, We will Use fsm-lite to count k-mers in populations of genomes (supplied as fasta files).
```
for f in ./*.fna; do id=$(basename "$f" .fna); echo $id $f; done > input.list
fsm-lite -l input.list -v -t fsm_kmers | gzip -c - > ../USA300_fsm_kmers.txt.gz
```
Population structure and kinship can confound genome-wide association studies and other genetic research, leading to false positive associations between genotypes and phenotypes. Accounting for relatedness between individuals helps differentiate true associations from false positives caused by genetic similarity. To correct for population structure, we will need to run roary for these 4 genomes and produce an alignment and then produce a phylogenetic tree that we will use for the pyseer main command.
```
roary -e gff/*.gff
FastTree -gtr -nt core_gene_alignment.aln > core_gene_alignment.tree
python3 phylogeny_distance.py --midpoint --lmm core_gene_alignment.tree > phylogeny_K.tsv
```
* We will use the 4 NAE and SAE genomes from last session. As these genomes from last session are not annotated. The first step is to annotate the genomes using Prokka using a command like
```
pyseer --lmm --phenotypes phenotypes.txt --kmers USA300_fsm_kmers.txt.gz --similarity phylogeny_K.tsv --output-patterns kmer_patterns.txt --cpu 8 > SAu_kmers.txt
cat <(head -1 SAu_kmers.txt) <(awk '$4<8.33E-03 {print $0}' SAu_kmers.txt) > significant_kmers.txt
```
* We can annotate these k-mers with the genes they are found in, or are near. To try and map every k-mer we can include a number of different reference annotations, as well as all the draft annotations of the sequences the k-mers were counted from.
```
annotate_hits_pyseer significant_kmers.txt references.txt Sau_annotated_kmers.txt
python3 ../summarise_annotations.py --unadj-p Sau_annotated_kmers.txt > gene_hits.txt
```
### Recombination

#### ClonalFrameML
It is a software package that performs efficient inference of recombination in bacterial genomes. It can be applied to any type of aligned sequence data, but is especially aimed at analysis of whole genome sequences. It is able to compare hundreds of whole genomes in a matter of hours on a standard Desktop computer. There are three main outputs from a run of ClonalFrameML: a phylogeny with branch lengths corrected to account for recombination, an estimation of the key parameters of the recombination process, and a genomic map of where recombination took place for each branch of the phylogeny.
```
mamba create -n clonalframeml -c conda-forge -c bioconda -c defaults clonalframeml
conda activate clonalframeml
ClonalFrameML Saureus.phyML.newick Saureus.fasta example.output -kappa 4.967695 -emsim 100 -ignore_user_sites Saureus.non-core-sites.txt > example.log.txt
```
**Very important to include all sites including invariable sites in your alignment. The input alignment usually comes from a reference based analysis. A concatenation of code genes is not ideal as input for ClonalFrameML, because it messes up with the linkage of the genes. The software will assume that they occur next to each other which affects linkage and recombination detection.**

## Further Readings
* [PySeer K-mer association with mixed effects model tutorial](https://pyseer.readthedocs.io/en/master/tutorial.html#k-mer-association-with-mixed-effects-model)
* [Publication: Sequence element enrichment analysis to determine the genetic basis of bacterial phenotypes](https://www.nature.com/articles/ncomms12797)
* [ClonalFrameML tutorial](https://github.com/xavierdidelot/clonalframeml/wiki)
