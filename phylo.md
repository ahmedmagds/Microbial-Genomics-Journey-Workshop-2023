# Microbial Genomics Journey Workshop 2023
## Session 10: Phylogenetic analysis
## IQ-Tree
## Background
### IQ-Tree was originally released in 2014 as an alternative methods of ML tree inference. Since, an updated version of the tool was assembled, IQ-Tree v2 (REF:https://doi.org/10.1093/molbev/msaa015), which we'll be using for this class. IQ-Tree is a fast, user-friendly, and **extremely** well documented tool that has been growing in popularity evolutionary sciences since it's initial release. It works on most data sets, including nucleotide/amino acid alignments in any format, traits, or gene presence/absence (both of which are binary data). It runs quickly your computer for lightweight analyses, and can be threaded to work on a cluster if you have a larger dataset. There is also an online GUI to help you get started, but I really prefer the CLI. We'll just scratch the surface of what it can do, but if you want to know more, check out the documentation here (http://www.iqtree.org/doc/).

### Imporantly, IQ-Tree does two things better than any other maximum-likelihood tool I've used.  First, it uses another tool, ModelFinderPlus, that goes through your data and finds the substitution model that minimizes your ability to make meaningful conclusions from your data. Secondly, by using Ultrafast bootstapping, you can get a sense for the branch supports in a fraction of the time and on your laptop (if you ask most phylogenetists, they hate running boots because they are massively time consuming).

### We'll just scratch the surface of what it can do, but if you want to know more, check out the documentation here (http://www.iqtree.org/doc/).

### Things I've used IQ-Tree for:
    - Phylogenetic inference from binary data
    - Building trees based on amino acid alignments
    - Ancestral state reconstruction of common ancesetors
    - Statistical analyses of tree congruence
    - Substitution model selection
    - Tree topology statistical test
    - Approximating evolutionary rates at specific sites

***Remember*** Building phylogenies is time and energy intensive and requires your attention as a scientist. There are multiple steps you have to complete and check before you make the tree. Once you have the tree, it can be hard to analyze or understand. It can feel overwhelming at first, but it gets easier with practice.

## Data

### Part 1: Build a tree from a bunch of genomes.
#### Let's say you want to understand the population structure of a population of K. pneumoniae you recovered from a hospital. You have collected ~110 genomes (I'm downloading them from NCBI for this example). The first thing you need to do is align them to a reference, which is a big choice to make. We're going to align them with Snippy. Then I'll use the core genome alignment to infer a tree with 1000 bootstraps and using automatic model selection. This looks like this at the command line:

```
#Genome accession list [GCA_000240185.2, GCA_023658165.1, GCA_029541645.1, GCA_010450855.1, GCA_003203435.1, GCA_028223855.1, GCA_023658145.1, GCA_017916195.1, GCA_009755705.1, GCA_028221125.1, GCA_002967795.2, GCA_004120095.1, GCA_002812665.1, GCA_004119955.1, GCA_001902475.1, GCA_015992305.1, GCA_004119975.1, GCA_012029945.2, GCA_021138155.1, GCA_002935085.1, GCA_020911785.1, GCA_019444195.1, GCA_028219495.1, GCA_015992425.1, GCA_028831855.1, GCA_009833945.1, GCA_028219375.1, GCA_027595485.1, GCA_021535105.1, GCA_011022255.1, GCA_003367555.1, GCA_021249265.1, GCA_009856585.1, GCA_904865755.1, GCA_021535085.1, GCA_021137635.1, GCA_023715985.1, GCA_024299245.1, GCA_002197225.1, GCA_904866445.1, GCA_024918475.1, GCA_023538995.1, GCA_013423825.1, GCA_004119995.1, GCA_004120075.1, GCA_904863135.1, GCA_904866325.1, GCA_003073335.1, GCA_003432165.1, GCA_904865815.1, GCA_904864465.1, GCA_904865715.1, GCA_023703935.1, GCA_027595685.1, GCA_009834285.1, GCA_028219935.1, GCA_027942175.1, GCA_016801375.1, GCA_009931015.1, GCA_023658225.1, GCA_023658185.1, GCA_019823835.1, GCA_027942195.1, GCA_017809835.1, GCA_018128465.1, GCA_002903005.1, GCA_019823875.1, GCA_002202195.1, GCA_027942215.1, GCA_011066505.1, GCA_018604345.1, GCA_002156725.1, GCA_027595645.1, GCA_002156745.1, GCA_019823915.1, GCA_019823855.1, GCA_029203205.1, GCA_010183685.1, GCA_003574255.1, GCA_003574275.1, GCA_028220015.1, GCA_002752775.1, GCA_008121515.1, GCA_022699325.1, GCA_019823955.1, GCA_904866545.1, GCA_024918055.1, GCA_004120115.1, GCA_015243235.1, GCA_023573805.1, GCA_023658205.1, GCA_019915305.1, GCA_001902435.1, GCA_016772575.1, GCA_911728565.1, GCA_001902515.1, GCA_009661535.2, GCA_020520185.1, GCA_029448435.1, GCA_023573825.1, GCA_010183645.1, GCA_013735895.1, GCA_904863065.1, GCA_023653925.1, GCA_018140905.1, GCA_013305325.1, GCA_002848545.1, GCA_024762135.1, GCA_001902235.1, GCA_001644765.1, GCA_015992325.1] > list.txt
```
#### I'm using this to download a bunch of genomes really fast
```
conda activate WhatsGNU
WhatsGNU_get_GenBank_genomes.py -c -r list.txt ./genomes
conda deactivate
```
#### Run Snippy-multi then snippy
```
conda activate snippy
snippy-multi input.tab --ref Reference.gbk --cpus 8 > runme.sh
sh ./runme.sh
```
#### Run Snippy clean on the core genome to remove weird characters
```
snippy-clean_full_aln core.full.aln > clean.full.aln
conda deactivate
```

#### Run IQ Tree with model finder
```
conda activate iqtree
iqtree -s clean_core_full.aln -m MFP -bb 1000
```

#### You did it! The program outputs a bunch of files, and you should read about what each one is. But the <NAME>.contree can be opened in Itol or Figtree, and it includes the branch support values.

### Part 2: Build a tree from a bunch of protein sequences.
#### Another common analysis you may run into is analyzing protein sequence diversity in a population. You can do this a lot of different ways (concatenating or partitioning the sequences, site specific, or in at the single protein level). For simplicity, we're going to take a single protein and collect 100 homologs from the NCBI database using BlastP.

#### Protein sequence:
```
>RpoB
MSYTFTGKKRIRKSFAKRETILEVPYLLTTQLESYSHFLQQNRSADKRVDEGLQAAFSSIFPILSNNGYAELQFDQYILGEPEFDIPECQLRGITYAAPLRAKIKLVIFNKEGAKPEDREIKEVRENTVYMGEIPLMTPSGSFVINGTERVIVSQLHRSPGVFFEHDRGKTHSSGKLLFSARIIPYRGSWLDFEFDPKDLLYFRIDRRRKMPVTILLRALGYDNEQILDMFYDAETYYLSGQTVQAEVVAERLRGETAKFDIVDNDGKVLVTAGKRINAKNIRDIQAAGLTRLNVEPETLVGKTLAKDIVASATGEVLVVANTEITEELLAQFDINGITQIQTLFTNETEAGGYISATLRTDETRDKQAARIAIYRMMRPGEPPTEEAVEALFNRLFFQEESYDLSRVGRMKFNTRTYEQKLLPAQENNWYGRLVNQTFAGVGEKEAKLHILSVEDIVVTIATLVELRNGHGEVDDIDHLGNRRVRSVGELVENQFRSGLARVERAVKERLNQAESEGLMPTDLINAKPVSAAIKEFFGSSQLSQFMDQTNPLSEVTHKRRVSALGPGGLTRERAGFEVRDVHPTHYGRVCPIETPEGPNIGLINSLSVYARTNEYGFLETPYRRVVDGKVTNEIDYLSAIEEGRYVIAQANSDLDENGCLKDELITCREKGETILATADRVQYMDVATGQVVSVAASLIPFLEHDDANRALMGANMQRQAVPCLRAEKPMVGTGIERSVAVDSATAIVARRGGVVEYVDANRVVIRVHDAEATAEQGGVDIYNLVKFTRSNQSTNINQRPAVNAGDVLQKGDLIADGASTDLGELALGQNMTIAFMPWNGYNYEDSILISERVAADDRYTSIHIEELNVVARDTKLGAEDITRDIPNLSEKMQNRLDESGIVYIGAEVEAGDVLVGKVTPKGETQLTPEEKLLRAIFGEKASDVKDTSLRMPTGMSGTVIDVQVFTREGIQRDKRAQSIIEVELKRYRQDLNDQIRIFDNDAFDRIERMIVGQVANGGPNKLAKGKEVTAEYLRELPSRHDWFDVRIADEDIAKQMELIKLSLQQKRQEAEELYEVKKRKMTQGDELPPGVQKMVKVFIAIKRRLQAGDKMAGRHGNKGVVSRILPVEDMPYMADGRTVDIVLNPLGVPSRMNIGQILEVHLGWAAKGIGQRIDRMLAEQRKVGEIRQFLDKLYNHSGKQEDLDSLTDDEILTLAHNLKRGATFASPVFDGAKEKEIFDMLELAYPSDDPEMQKLDFNDTKTQIRLFDGRSGEPFDRRVTVGVMHYLKLHHLVDEKMHARSTGPYSLVTQQPLGGKAQFGGQRFGEMEVWALEAYGAAYTLQEMLTVKSDDVTGRTKMYENIVKGEHKIDAGMPESFNVIVKEIRSLGLDIDLEQN*
```
#### Align with your favorite protein alignment tool (I like ClustalO because its easy)
#### Run IQ Tree with model finder
```
conda activate iqtree
iqtree -s ALIGNMENT.aln -m MFP -bb 1000
```
#### You did it! The program outputs a bunch of files, and you should read about what each one is. But the <NAME>.contree can be opened in Itol or Figtree, and it includes the branch support values.

### Part 3: Tree visualization!
There are multiple tree visualization tools.
* Interactive Tree Of Life (iTOL) is an online tool for the display, annotation and management of phylogenetic and other trees. You can manage and visualize your trees directly in the browser, and annotate them with various datasets. You can share a live interactive version of the tree with others.<br/>
Let's have a look at this [tree](https://itol.embl.de/tree/15914228161273581674704515)in iTol from a project we are currently working on.

## Cladebreaker


## Further Readings
* [WhatsGNU Publication](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-01965-w)
* [An example of using WhatsGNU to evaluate the sequence conservation](https://elifesciences.org/articles/66657)
* [Pre-epidemic evolution of the MRSA USA300 clade and a molecular key for classification](https://www.frontiersin.org/articles/10.3389/fcimb.2023.1081070/full)
