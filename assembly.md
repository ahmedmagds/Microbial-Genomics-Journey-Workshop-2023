# Microbial Genomics Journey Workshop 2023
## Session 3: Assembly

## Intro
![Adapter](Illumina_adapters.jpg)
<br/>Illumina Figure showing a paired-end flow cell for MiSeq, HiSeq 2000/2500 and NovaSeq 6000<br/>

**Sample Multiplexing**
Sample multiplexing allows large numbers of libraries to be pooled and sequenced on a single run. This will drastically reduce cost. There are 384 Unique Dual Indexes (Sets A, B, C and D). This means a total of 384 samples can be multiplexed on a single lane.

**Adapter Trimming**
Adapter trimming is the process of removing adapter sequences from the 3’ ends of reads. Adapter sequences should be removed from reads because they interfere with downstream analyses, such as alignment of reads to a reference. it is necessary to eliminate adapter sequences from reads. These adapter sequences contain important elements, including the sequencing primer binding sites, the index sequences, and the sites that facilitate the attachment of library fragments to the flow cell lawn. You can use [trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) for this trimming step.

## shovill
It is a tool to assemble bacterial isolate genomes from Illumina paired-end reads. The SPAdes genome assembler is the standard de novo genome assembler for Illumina whole genome sequencing data of bacteria and other small microbes. [Shovill](https://github.com/tseemann/shovill) is a pipeline which uses SPAdes at its core, but it is faster.<br/>

**If you get an error saying `Cannot open temporary file kmc_00253.bin and Could not determine genome size from ''`, use `ulimit -n 2048` to increase limit for opened files for the kmc tool on MacOS.**<br/>

Let's assemble our first genome.
```
conda activate shovill
shovill --trim --assembler skesa --outdir output_S56 --R1 S56_R1.fastq --R2 S56_R2.fastq
mash screen -w -p 8 ../../problem_set1/RefSeqSketches.msh contigs.fa > S56_screen_winning_contigs.tab
sort -gr S56_screen_winning_contigs.tab > S56_screen_winning_contigs_sorted.tab
less S56_screen_winning_contigs_sorted.tab | head -n 10
```
## quast
QUAST stands for QUality ASsessment Tool. The tool evaluates genome assemblies by computing various metrics. It has an interactive visualizer for the outputs.<br/>
Let's try it! Go to NCBI and download the reference genome and gff file for Pseudomonas aeruginosa (PAO1).
```
conda activate quast
quast.py contigs.fa -r GCF_000006765.1_ASM676v1_genomic.fna.gz
```
## busco
It is a tool to assess the genome assembly and annotation completeness based on evolutionarily informed expectations of gene content (single-copy orthologs).
```
conda activate busco
busco -i ~/MGJW/problem_set3/out_S56/contigs.fa -l bacteria_odb10 -o busco_op -m genome
busco -i ~/MGJW/problem_set1/fasta/genome4.fasta -l bacteria_odb10 -o busco_op2 -m genome
busco -i ~/MGJW/problem_set1/fasta/genome4.fasta -l clostridiales_odb10 -o busco_op3 -m genome
```
## Further Readings
* [Shovill](https://github.com/tseemann/shovill)
* [Quast](https://www.youtube.com/watch?v=ViXzKrQo25k)
* [Busco](https://academic.oup.com/bioinformatics/article/31/19/3210/211866)
* [Busco_lineages](https://busco.ezlab.org/list_of_lineages.html)
