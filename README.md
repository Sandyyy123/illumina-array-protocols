# [illumina](http://www.illumina.com/)-array-protocols

**Protocols/scripts for processing illumina SNP arrays**  

A set of scripts and protocols that we use to processing raw Illumina SNP array data.

- BWA Mapping of probe sequences  
- Genomestudio SOP (Manual Calling & QC)    
- Standard QC (PLINK, bash...) and re-calling No-Calls using zCall  

**VERSION: v0.1**  

## The Team 
**Bioinformatics** - Hamel Patel, Amos Folarin & Stephen Newhouse  @ [bluecell.io]()   
**Lab** - Charles Curtis & Team @ [The IoPPN Genomics & Biomarker Core Facility](http://www.kcl.ac.uk/ioppn/depts/mrc/research/The-IoPPN-Genomics--Biomarker-Core-Facility.aspx)  

![illuminaCSPro](./figs/CSProLogo-new-Cropped-313x105.png)

## Illumina Downloads Resource
This links takes you to Illumina's download page, which provides access to product documentation and
manifests.

- [Illumina Downloads](http://support.illumina.com/downloads.html)

## BeadChips
These are the BeadChips we have had experience in processing (so far...).

- [HumanCoreExome-24 v1.0 BeadChip](http://support.illumina.com/downloads/humancoreexome-24-v1-0-product-files.html)  
- [HumanOmniExpressExome-8 v1.1 BeadChip](http://support.illumina.com/downloads/humanomniexpressexome-8v1-1_product_files.html)   

## 1. BWA Mapping of Probe Sequences

Illumina SNP arrays include a lot of probes that map to multiple (>500) sites in the Genome.  

For each array we map the probe sequences to the relevant genome build using BWA (as indicated by Illumina manifests), and
identify probes that map 100% to multiple regions (>1 hit) of the genome.

These probes are either flagged for removal before re-calling, or depending on what the data looks likes in Genomestudio,
are zeroed at the Genomestudio stage before clustering.  

Those familiar with processing Illumina Arrays, will see that a lot of the probes we identify are consitently poorly clustered variants, variants not called for alot of samples, and those messy clouds and/or the always homozygous variants - no matter the population or number of samples. 

More details soon....  

### Illumina Array Annotations

- MEGA\_Consortium\_15063755_B2.csv [download](https://s3-eu-west-1.amazonaws.com/illumina-probe-mappings/mega_array_annotations.txt.gz)  
- HumanCoreExome-24v1-0_A.csv [download](ftp://webdata:webdata@ussd-ftp.illumina.com/Downloads/ProductFiles/HumanCoreExome-24/Product_Files/HumanCoreExome-24v1-0_A.csv)  
- HumanOmniExpressExome-8-v1-1-C.csv [download](ftp://webdata:webdata@ussd-ftp.illumina.com/Downloads/ProductFiles/HumanOmniExpressExome/v1-1/HumanOmniExpressExome-8-v1-1-C.csv)  
- XXX [download]()
- XXX [download]()
- XXX [download]()

### BWA Mapping Results 

**Running BWA**  
Using [NGSeasy](https://github.com/KHP-Informatics/ngseasy) Docker [compbio/ngseasy-bwa](https://registry.hub.docker.com/u/compbio/ngseasy-bwa/) image.

>Program: bwa (alignment via Burrows-Wheeler transformation)  
>Version: 0.7.12-r1039  
>Contact: Heng Li <lh3@sanger.ac.uk>  

```bash
###################################
## BWA > samblaster > samtools
#

array=""
refGenome=""

docker run \
-w /home/pipeman \
-e HOME=/home/pipeman \
-e USER=pipeman \
--user pipeman \
-i \
-t compbio/ngseasy-bwa:1.0 /bin/bash -c \
"bwa mem -t 32 -V -M -a ${refGenome} ${array}.fasta | \
samblaster --addMateTags --excludeDups | \
samtools sort -@ 32 -T temp_ -O sam -o ${array}.sam && \
samtools index ${array}.sam && \
rm ${array}.sam"
```

**MEGA**    
- [mega_array_seq_sorted.bam](https://s3-eu-west-1.amazonaws.com/illumina-probe-mappings/mega_array_seq_sorted.bam)  
- [mega_array_seq_sorted.bam.bai](https://s3-eu-west-1.amazonaws.com/illumina-probe-mappings/mega_array_seq_sorted.bam.bai)  

**OMNI**  
- ...   
- ...     

### Probe Lists  
These lists provide data on probe mappings. We provide Illumina Probe Id's along with the
number of time it maps to the genome. 

- ...
- ...  

## 2. Genomestudio 

More details soon....

## 3. Quality Control and Re-Calling  

More details soon....

******

### Genotypes and Samples Processed  


******
Copyright (C) 2015 Hamel Patel, Amos Folarin & Stephen Jeffrey Newhouse


