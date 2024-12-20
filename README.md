# 👩🏻‍💻LOFI (Locally run Operon Finder based on Integrated metrics)  <a href=""><img src ='https://drive.google.com/uc?export=view&id=1c0OLbJXtGd3ZQvNOSGUGb6lvS2ngdevB' width =300 align="right"></a>
> *This repository is for a project to create a tool for searching for operons*

\
Unfortunately, the tool <u>does not currently work</u> on the **macOS** operating system.

All performance tests were carried out on **Ubuntu 22.04 LTS**, 16 GB RAM, 16 threads.

The tool will take up about **12 GB** of disk space.
<br>
<br>

## Description

This tool is designed to predict the location of bacterial genes in an operon based on 3 metrics: 
**intergenic distance**, **score from the [STRING](https://string-db.org) database** and information about metabolic pathways 
from **[KEGG](https://www.kegg.jp) database**.

## Installation:

Clone this repo and go to created folder: 

```shell
git clone https://github.com/JuliGen/LOFI.git && \
cd LOFI
```

Create mamba environment from `environment.yaml` and activate it:

```shell
mamba env create -f environment.yaml && \
mamba activate lofi
```

## Usage

The first run will take longer than subsequent runs due to the loading of the databases required for analysis.

As input, you must provide a `FASTA file` with the **nucleotide sequence** of the bacterial genome in the `.fna` format 
(if you have a different extension, please <u>change it manually</u>). 
Contigs within a FASTA file are expected to be in the **correct order**.

**File with sequence must be in the `genomes` folder**.

It is also necessary to provide the `taxid` of the species being researched. 
For proper operation of the tool, this id must be in the **STRING database**. 
To check if a species is listed in STRING, try looking for it in the file `data/species.v12.0.txt`.
If the id is <u>not found</u>, then use the name of the species to find the species closest to yours, available 
in the STRING database and provide its `taxid`.

All commands are integrated in the `snakemake`. To start the analysis execute the command from the **template below**. 

You have to change 2 variables:
- `taxid`
- `genome`

Also, if you want to launch **kofam_scan** in parallel while doing pipeline, go to the appropriate rule in `Snakefile`
and change the `params.threads` variable to the desired one. Default value is 8.

```shell
snakemake --cores=all -p results/{taxid}/predictions/{genome}_predictions.tsv
```

Remove `-p` flag to see less comments while analysing.

### Example usage

You can download the databases and check if everything is installed correctly using the command below 
with the [_E. coli_ K-12](https://www.ncbi.nlm.nih.gov/datasets/taxonomy/511145/) genome taken from NCBI 
(it is already uploaded to the genomes folder).

```shell
snakemake --cores=all -p results/511145/predictions/GCF_000005845.2_ASM584v2_genomic_predictions.tsv
```

## Troubleshooting

Make sure **taxid** is in the file `data/species.v12.0.txt`.

Make sure that the **genome** of the species is located in the `genomes/` folder.

If you encounter any issues or have questions, ~~try to use _E. coli_ K-12~~ directly contact the authors for support.
