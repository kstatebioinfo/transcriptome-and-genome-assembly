transcriptome-and-genome-assembly
=================================

Scripts in this repository have been used to assemble de novo transcriptomes and genomes 

KSU_bioinfo_lab folder
----------------------
**generalizable_script_writer.sh** - This script takes the full path for the input data (e.g. reads) and makes output directories and scripts with rational names. For example, the current "call program" section assumes one input file ends in "_single.fastq" and one in "_paired.fastq" and creates directories based on the name of the data and the parameter. The script can also find a file extension in the argument you pass to it, this is stored as the ${ext} variable. Change lines 36 to 41 to call your program.

```
USAGE: generalizable_script_writer.sh [base_filename_for_reads]
```

**run_Trinity.sh** - This script calls Trinity to assemble with a single k-mer. After read cleaning broken pairs can be loaded as (now single end reads) as "--single".

```
USAGE: bash run_Trinity.sh
```

**run_abyss.pl** - This script runs an installation of ABySS-1.3.4 on beocat. Step 1 is the assembly of multiple single k-mer assemblies. Step 2 is the merging of these single k-mer assemblies by running ABySS on reads and the unitigs from the multi-kmer assemblies. Step 3 writes a script to calculate N25,N50,N75, cumulative scaffold length, number of scaffolds for all assemblies.

```
USAGE: perl run_abyss.pl
```

**run_mira_454.sh** - Runs Mira 3.4 for 454 data alone. If reads have been cleaned than the trace .xml file extracted from the .sff file will be missing so the parameters in this script are set with mxti=no to reflect the lack of xml trace files.

```
USAGE: bash run_mira_454.sh
```

**run_velvet_oases_k-mer_assemblies.sh** - This script will write single k-mer transcriptome assembly scripts and a list of qsubs that can be used to call them on beocat. Comment out line 10 if your sequences are already shuffled. After read cleaning I load my broken pairs (now single end reads) as "-short" and pairs as "-shortPaired". The script calculates memory request for each assembly as "Ram required for velvetg = -109635 + 18977*ReadSize + 86326*GenomeSize + 233353*NumReads - 51092*K" which is taken from http://listserver.ebi.ac.uk/pipermail/velvet-users/2009-June/000359.html. You should change the ReadSize, GenomeSize, and NumReads to reflect your data.

```
USAGE: bash run_velvet_oases_k-mer_assemblies.sh
```

**run_oases_merge.sh** - This script calls velvet and oases to merge many single k-mer assemblies (see velvet_oases_k-mer_assemblies.sh).

```
USAGE: qsub -l h_rt=100:00:00,mem=100G run_oases_merge.sh
```

**stats.sh** - see prinseq.sh
 
 





