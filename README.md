# Summary

This repository contains the relevant files for the paper [Multispecies transcriptomes reveal core fruit development genes](https://pubmed.ncbi.nlm.nih.gov/36407608) published in Frontiers in Plant Science 2022. The gist of this project was to compare gene expression patterns in multiple flowering plant species as their fruits develop. The repo makes heavy use of publically available data, whose accessions are documented below.

# Methods

## DEG Analysis

For RNA-seq, this will consist of STAR as the read aligner and DESeq2 as the differential expression testing software.  The data are fit to a spline regression model with one fewer degree of freedom than there are timepoints in the specific study. (See supplement of [Sander et al, 2017](https://www.ncbi.nlm.nih.gov/pubmed/27797772) for an implementation of this with DESeq2.)

#### Gene Expression Data

Because the data are a combination of previously published and new data, the downloading of the raw data is admittedly a mess. The previously published data was downloaded with the `1_GetData.sh` script and places its files into `./ExternalData/`. It is parallelized as a SLURM array job to download different accession simultaneously. This script also performs the quality trimming of the FASTQ files immediately following downloading. The data generated in this study were also submitted to NCBI (later) and the script to download that raw data is `1_SRADownload.sh`, which places its files in the `./SRA/` folder. This script is more efficient and also more flexible since it relies on the `SRA_IDs.tsv` file to get a list of the relevant files to download. The quality trimming for this data is done with `1_SRATrim.sh`

TODO: Merge `1_GetData.sh` into `1_SRADownload` and `SRA_IDs.tsv`

| Species | NCBI BioProject ID | Description |
| ------- | ------------------ | ----------- |
| Cultivated Tomato | [PRJNA646747](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA646747) | RNA-seq from a developmental series of pericarps in cultivated tomato (*Solanum lycopersicum*  cv. Ailsa Craig). Generated by our lab. |
| Wild Tomato | [PRJNA646747](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA646747) | RNA-seq from a developmental series of pericarps in wild tomato (*Solanum pimpinellifolium* LA2547). Generated by our lab. |
| Desert Tobacco | [PRJNA646747](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA646747) | RNA-seq from a developmental series of pericarps in wild-type desert tobacco (*Nicotiana obtusifolia*). Generated by our lab. |
| Arabidopsis | [PRJEB25745](https://www.ncbi.nlm.nih.gov/bioproject/PRJEB25745) | RNA-seq of wild-type fruit valve tissue. From [Mizzotti et al, 2018](https://doi.org/10.1104/pp.18.00727) | 
| Cucumis melo | [PRJNA314069](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA314069) | RNA-seq of wild-type melon fruit pericarp tissue fro ma developmental series. From [Chayut N et al, 2017](https://doi.org/10.1104/pp.16.01256) |

#### Genomes

I am currently using the [SL4.0 genome and the ITAG4.0 annotation for tomato](https://solgenomics.net/organism/Solanum_lycopersicum/genome). For tobacco (*N. obtusifolia*) I am using the publicly available [genome and annotation](http://nadh.ice.mpg.de/NaDH/download/overview) but with a slight modification to add in the the MBP20 gene, which was missed in the original annotation. For *Arabidopsis* I'll be using the TAIR10 genome and annotation. In the case of tomato and tobacco these files are symlinked to copies we already have using the `SlyDNA` and `NobtDNA` directories, respectively. For melon, I am using the [DHL92 v3.5.1 genome](http://cucurbitgenomics.org/organism/3).



