---
description: Alignment and processing pipleines
---

# Processing and Alignment using UMI tools

Raw data comes to our lab mostly in the fastq format. For quality control on the fastqs files,[ **fastqc**](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) results is included in the data transfer. The fastqs go through the alignment and processing pipelines. For alignment the industry standard [BWA](https://bio-bwa.sourceforge.net/)  is used across all major pipelines.

### UMI Tools

Unique molecular identifiers (UMIs) are a type of molecular barcoding that provides error correction and increased accuracy during sequencing. In pughlab almost all libraries use unique molecular barcoding to enhance precision and accuracy in genomic analysis. In order to process the barcoded data there exists some codes and pipeline that were mainly developed by Nina Wang [https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gkz474/5498633](https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gkz474/5498633). The [Consensus Cruncher](https://github.com/pughlab/ConsensusCruncher) pipeline developed by N.Wang includes barcode extraction from the raw fastq files and alignment and processing of the UMIs to generate collapsed bam files.&#x20;

There are 3 main steps in the Consensus Cruncher pipeline. We do not always need to run all of them.

1. Barcode extraction
2. Fastq to bam (alignment)
3. Consensus

For **barcode extraction** only one can use the following python code [https://github.com/pughlab/ConsensusCruncher/blob/master/ConsensusCruncher/extract\_barcodes.py](https://github.com/pughlab/ConsensusCruncher/blob/master/ConsensusCruncher/extract_barcodes.py)

`python3 /cluster/projects/pughlab/src/ConsensusCruncher/scripts/extract_barcodes.py --read1 $read1 --read2 $read2 --outfile ${filename}_barcoded --blist /cluster/projects/pughlab/src/UMI-tools/idt_dual_index_barcodes.txt`



If you need to **alignment and barcode extraction** you can use [https://github.com/pughlab/ConsensusCruncher/blob/master/ConsensusCruncher.py](https://github.com/pughlab/ConsensusCruncher/blob/master/ConsensusCruncher.py)

`python3 ConsensusCruncher.py -c CONFIG.ini fastq2bam --fastq1  read1 --fastq2 read2 --output outputdir --blist /cluster/projects/pughlab/src/UMI-tools/idt_dual_index_barcodes.txt`

&#x20;

For running the **Consensus** step:

`python3  ConsensusCruncher.py -c config.ini consensus -i  bamdir/$bamfile -o  outputdir -b cytoband.txt`

The bam directory is what contains alignment files from the previous step.
