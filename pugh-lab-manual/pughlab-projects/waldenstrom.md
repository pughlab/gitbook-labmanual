# WALDENSTROM

## Data type

WALDENSTROM project is the study of serial plasma samples some with matching bone marrow.

The panel is designed by Sign Chow a former graduate student. A subset of data has cap-Ig and shallow 1X whole genome sequencing data.

The analysis pipeline is the following:

## Processing and alignment

The alignment is done using BWA  after barcode extractio followed by indelrealigner. The realigned bams then go through consensus making pipeline. The "all.unique" bam files are use for further analysis steps.

