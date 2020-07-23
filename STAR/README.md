# STAR: ultrafast universal RNA-seq aligner

Manuscript:
>`https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3530905/`

Manual:
>`https://raw.githubusercontent.com/alexdobin/STAR/master/doc/STARmanual.pdf`

If running on WSL you need to remember to increase the `ulimit -n` to something larger than the default `1024`. I increase mine to `100000` but I'm sure it doesn't have to go that high.

## Genome and annotation files

Download:
>`ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Mus_musculus/NCBI/build37.2/Mus_musculus_NCBI_build37.2.tar.gz`

To decompress and untar:
>`tar xvfz Mus_musculus_NCBI_build37.2.tar.gz`

## Running STAR

Command used the first time to run STAR with the `SRR_Acc_List.txt`:
>`car SRR_Acc_List.txt | parallel -j 2 "gunzip trimmedReads{}_*P.gz && STAR --runThreadN 32 --genomeDir starIndex --readFilesIn trimmedReads/{}_1P trimmedReads/{}_2P --outFilterIntronMotifs RemoveNoncacnonical --outFileNamePrefix starAligned/{} --outSAMtype BAM SortedByCoordinate && pigz trimmedReads/{}_*P"`

Command used in the second video which keeps the genome in RAM, parallel unzipping of the trimmed files, limits the RAM used for sorting the BAM file, and then keeping the unmapped reads so we can use them later:
>`cat SRR_Acc_List.txt | parallel -j 1 "parallel gunzip ::: trimmedReads/{}_*P.gz && STAR --runThreadN 64 --genomeLoad LoadAndKeep --genomeDir starIndex/ --readFilesIn trimmedReads/{}_1P trimmedReads/{}_2P --outFilterIntronMotifs RemoveNoncanonical --outFileNamePrefix starAligned/{} --limitBAMsortRAM 5000000000 --outSAMtype BAM SortedByCoordinate --outReadsUnmapped Fastx && pigz trimmedReads/{}_*P"`
