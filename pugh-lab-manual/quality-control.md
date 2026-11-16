# Quality Control

For the samples that are run on capture panels such as MYLSTONE2, WALDENSTROM panels, Cap-tcr and Cap-Ig, the wet lab runs a shallow sequencing to evaluate the quality of the capturedd reads. This is to ensure that the reads are matching up sufficiently on the targeted regions before going ahead with deep sequencing. This step is called Miseq QC as this shallow data is sequenced on the Miseq machine.&#x20;

## Capture panels in pughlab

The pughlab capture panels can be found here on valinor

`/mnt/work1/users/pughlab/projects/MiseqQC/targeted_panels/pughlab_panels`

`The panels are the following:`

<mark style="color:blue;">Hi5 panel: Hi5\_Panel\_0656441\_Covered.bed</mark>

<mark style="color:blue;">MYLSTONE2 panel: MYL2\_Gene\_Backbone\_Ig\_regions.hg38.bed</mark>

<mark style="color:blue;">WALDENSTROM panel: WAL\_signy\_design.bed</mark>



Individual region panel targets can be found in `/mnt/work1/users/pughlab/projects/MiseqQC/targeted_panels/individual_regions`&#x20;

<mark style="color:blue;">1\_17\_SNPs.bed</mark>

<mark style="color:blue;">CNVs\_MM\_panel\_IDT.bed</mark>

<mark style="color:blue;">MM-prognosis-genes.bed</mark>

<mark style="color:blue;">MM-target-resistance-genes.bed</mark>

captcr individual regions are

<mark style="color:blue;">J\_regions.bed</mark>

<mark style="color:blue;">V\_regions.bed</mark>

<mark style="color:blue;">C\_regions.bed</mark>



cap-Ig individual targets are:

<mark style="color:blue;">Ig\_Hot1.bed</mark>

<mark style="color:blue;">Ig\_Hot2.bed</mark>

The Ig targets are also included in the MYLSTONE2 panel







## Miseq QC pipeline

![](<.gitbook/assets/image (1).png>)



