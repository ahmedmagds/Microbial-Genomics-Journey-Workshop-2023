# Microbial Genomics Journey Workshop 2023
## Session 6: Reference based analysis

Many of the phylogenetic and comparative genomics approaches use a reference composed of either core genes or a single reference genome. The mapping softwares like bwa and bowtie2 are specialized for mapping short, paired-end, low-divergent sequence reads against a large reference genome. It was originally crafted for eukaryotic genomes such as human genome, where all the sequences should align to one genome.<br/>

Some important considerations when you pick your reference:
* Whenever you have raw reads, use them before thinking of using the assembled genome. Why? Simply because you lose data once you assemble and you want to use as much as possible in this step.
* Any sequence data not in the reference is lost. This missing data decreases the discriminatory power, and may lead to inaccurate statements about relatedness or critical gene/SNP content.
* If you change your reference, your variants called will be different.
* In many of the cases, pangenome and other approaches will be needed to give a better picture of the dataset.
* The choice of a reference may result in errors and it may affect downstream analyses such as the detection of single nucleotide polymorphisms (SNPs) and phylogenetic inference.

There are multiple use cases for using a reference-based approach but we will cover here two of them.

## Decontaminate

Let’s try using a tool called bwa to decontaminate our samples from phiX reads!
* We have two files, S56_R1.fastq and S56_R2.fastq, with paired-end reads.
* We will download a phiX genome from (NCBI)[https://www.ncbi.nlm.nih.gov/genome/?term=NC_001422.1] and call it phiX.fasta.
* The first step to align the reads to a reference is indexing. In bioinformatics, indexing enhances the efficiency of the tools by enabling rapid access to specific file locations. This is very useful as it allows the tool to directly locate the required information without scanning the entire file. `bwa index` is the command we use for this step in the bwa tool.
* This indexing step can take a long time on large genomes (e.g. human). However, it is needed only once for each reference genome and tool.
* The next step will be mapping the sequence reads to the indexed reference using `bwa mem` command as shown below.
* These programs use an output format called SAM. These files start with a bunch of header lines (beginning with “@”). Then one line for each query sequence against the reference. In each of these lines, you will find the query sequence ID, reference sequence ID, coordinates of the hit, and other info in a tab-separated format.
* If no hit was found, the program writes “*” in place of the reference sequence ID which can be nicely used for filtering these unmapped reads from the rest.
* We will then need to extract mapped reads and count how many reads mapped to phiX.
The commands below summarize the multiple steps we just discussed.
```
conda activate snippy
bwa index phiX.fasta
bwa mem -M -t 2 -o output.sam phiX.fasta ~/MGJW/problem_set3/S56_R1.fastq ~/MGJW/problem_set3/S56_R2.fastq
samtools view -b -F 4 output.sam > mapped.bam
samtools view mapped.bam | wc -l
```

## Snippy
[Snippy](https://github.com/tseemann/snippy) finds SNPs between a haploid reference genome and your NGS sequence reads. It will find both substitutions (snps) and insertions/deletions (indels). It will do that for multiple genomes (raw reads or assembled genomes). It can then take a set of Snippy results using the same reference and generate a core SNP alignment (and ultimately a phylogenomic tree).

Let's produce a core snp alignment for our Staphylococcus  aureus genome from previous sessions (genome3).

* Let's assume we do not know much about this genome. The first question is which genome from the public database I should select to compare this genome to. If you have no knowledge, you may choose the reference that is most commonly used in publication (e.g. N315 for S. aureus and PAO1 for P. aeruginosa). Let's download N315.
* Go to [assembly](https://www.ncbi.nlm.nih.gov/assembly/) page on NCBI. How many assembled genomes are available for S. aureus on GenBank and RefSeq?
* Go to the [genome](https://www.ncbi.nlm.nih.gov/genome/?term=Staphylococcus+aureus) page of S. aureus and then press on Genome Assembly and Annotation report and then search for strain N315. Press on the accession number NC_002745.2. Now let's try to download a fasta file. Press on send to and then select file and then select fasta. This should download the fasta file. Do the same step but for strain Newman (NC_009641.1).

```
conda activate snippy
cd ref_based
snippy --outdir N315_genom3 --ref N315.fasta --ctgs ~/MGJW/problem_set1/fasta/genome3.fasta
```
How many snps were found between our isolate and the reference? (30094 SNPs). Now let's add a new layer of information that will help us choose a better reference and see how would this affect the number of SNPs. From last session, we typed this genome using MLST and it rturned out to be ST398. Now let's do the same steps but with a genome that is ST398. M357 is a ST398 genome. I already know that from previous work.

```
snippy --outdir M357_genome3 --ref M357.fasta --ctgs ../problem_set1/fasta/genome3.fasta
```
How many snps were found between our isolate and the ST398 reference? (1719 SNPs). Huge difference and the reason is now we used a closer genome to our study isolate. N315 is a ST5 genome which is divergent from ST398.

**Can we get even a much closer public genome to our isolate?**
Yes, all what we need is a tool that quickly can compare our genome against the database and report the top hit. [Which tool from our previous sessions can do that?](quality.md/#mash) I used it and it reported genome PA57 as the closest from the public database. There is also another tool "WhatsGNU" that we will study in a coming session that can easily address this issue.
```
snippy --outdir PA57_genome3 --ref PA57.fasta --ctgs ../problem_set1/fasta/genome3.fasta
```
When I compared my genome to this reference, I found only 265 SNPs. Choosing different references resulted in totally different number of SNPs (30094---1719---265). Some Questions:
* Does this mean I need to do that manually and choose the reference for each study isolate?
* What if I compare multiple study genomes, which reference to use then?
[CladeBreaker](https://github.com/andriesfeder/cladebreaker) will find the answer.
* What if I need to make one alignment of all snps in 10 genomes or more compared to one reference? snippy-core is the solution for this.

## Further Readings
* [The documentation for bwa](https://bio-bwa.sourceforge.net/bwa.shtml)
* [Evaluation of SNP calling tools](https://www.microbiologyresearch.org/content/journal/mgen/10.1099/mgen.0.000261#tab2)
