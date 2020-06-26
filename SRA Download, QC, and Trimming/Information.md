## Mouse Genome and GTF files
Mus musculus from the Johns Hopkins University Center for Computational Biology - TopHat iGenomes

>http://ccb.jhu.edu/software/tophat/igenomes.shtml

To download the genome file
>`wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Mus_musculus/NCBI/build37.2/Mus_musculus_NCBI_build37.2.tar.gz`

However, remember that this is 23GB..compressed. It will also take a very long time to download.

## SRA Downloading
To install SRA-Tools with conda:
>`conda install sra-tools`

Can download the files that you want to use by clicking [here](https://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP061386&o=acc_s%3Aa) or the link below. Once the runs that you want are selected, you can download the accession list (or download it from the [SRA Download, QC, and Trimming folder](github.com/ACSoupir/Bioinformatics_YouTube/)).

>https://trace.ncbi.nlm.nih.gov/Traces/study/?acc=SRP061386&o=acc_s%3Aa

These can than be downloaded like I showed one at a time if wanted using:

>`fastq-dump --gzip --split-files SRR2121770 &`

and replacing `SRR2121770` with other IDs that are located in the downloaded list. Alternatively, parallel can be used to download the files. To do this, you will need to download `parallel`. I forgot about installing this in the environment until the end of the video.. Oops! To install it:

>`conda install parallel`

To run SRA downloading in parallel with, for example, 4 jobs:

>`cat SRR_Acc_List.txt | parallel -j 4 "fastq-dump --gzip --split-files {}"`

## Downloading and running FastQC and MultiQC

In the first video I gave the example of installing a package to the environment but just in case starting here, to install FastQC and MultiQC:

>`conda install fastqc`
>`conda install multiqc`

Once installed, running FastQC with, lets say 8 jobs:

>`fastqc -t 8 rawReads/*.fastq.gz -o rawFastqc/`

This tells FastQC to go to the folder rawReads and select all of the files with the ending `.fastq.gz` and analyze them 8 at a time and then store the outputs in the folder rawFastqc. If you want to run MultiQC:

>`multiqc -o rawFastqc/ rawFastqc/`

This will tell multiQC to go into the output folder from FastQC and then combine all of the outputs into a single file that makes the information easier to digest.

## Trimming with Trimmomatic

To install trimmomatic:

>`conda install trimmomatic`

Now, the location of your adapters may be different. I'm using WSL and an environment so when moving the adapter sequence that we are going to use to the working directory:

>`cp ~/miniconda3/envs/p53/share/trimmomatic-0.39-1/adapters/TruSeq3-PE-2.fa .`

To run Trimmomatic with, say, 2 jobs using 4 cores each:

>`cat SRR_Acc_List.txt | parallel -j 2 "trimmomatic PE -threads 4 rawReads/{}_1.fastq.gz rawReads/{}_2.fastq.gz -baseout trimmedReads/{} ILLUMINACLIP:TruSeq3-PE-2.fa:2:30:10:2:keepBothReads LEADING:3 TRAILING:3 MINLEN:36 2> trimmedReads/{}_trimming.log"`
