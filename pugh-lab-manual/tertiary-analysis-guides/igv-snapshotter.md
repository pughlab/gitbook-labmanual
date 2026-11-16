# IGV Snapshotter



## Snap-shots on H4H

In order to capture screen shots of bam files that are residing on H4H, you need to download the following:

IGV command line exectuables

{% embed url="https://data.broadinstitute.org/igv/projects/downloads/2.16/IGV_2.16.2.zip" %}

And the python code `make_IGV_snapshots_modified.py`  from pughlab src directory:

The command line to run the snap-shotter is&#x20;

`python /cluster/projects/pughlab/src/IGVSnapshot/make_IGV_snapshots_modified.py -r regions.bed -g hg38 -o ./outputdir -b /path/to/igv.sh /path/to/bamfile`

