# Raw sequencing reads

I figured man might not have a great deal of free hard drive space to download these all, let alone analyze the full files because of the large file sizes. I have pulled the first reads from the raw files and uploaded them here so everyone can still follow along even if you don't want to download the full files. The only thing that will be greatly impacted here are going to be the results, but the motions to do the steps will be the same.

These are the top 1,000,000 lines from each of the files. This should be the first 250,000 reads.

This was created using:
>`cat 'SRR_Acc_List - Copy.txt' | parallel -j 1 "gzip -cd rawReads/{}_1.fastq.gz | head -1000000 > shortReads/{}_1.fastq"` #forward reads
>`cat 'SRR_Acc_List - Copy.txt' | parallel -j 1 "gzip -cd rawReads/{}_2.fastq.gz | head -1000000 > shortReads/{}_2.fastq"` #reverse reads

>`pigz *.fastq` #zipping up each file

To extract:
>`gunzip *.gz`
in the directory with the files.

If these don't work please let me know through opening an issue.

v/R

Alex
