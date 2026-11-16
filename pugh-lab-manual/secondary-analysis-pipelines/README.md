# Secondary Analysis Pipelines



## Gatk tools and Genomic Pipelines

The [GATK](https://gatk.broadinstitute.org/hc/en-us) is the industry standard for identifying SNPs and indels in germline DNA and RNAseq data. Its scope is now expanding to include somatic short variant calling, and to tackle copy number (CNV) and structural variation (SV). In addition to the variant callers themselves, the GATK also includes many utilities to perform related tasks such as processing and quality control of high- throughput sequencing data, and bundles the popular Picard toolkit. These tools were primarily designed to process exomes and whole genomes generated with Illumina sequencing technology, but they can be adapted to handle a variety of other technologies and experimental designs. And although it was originally developed for human genetics, the GATK has since evolved to handle genome data from any organism, with any level of ploidy



### Mutect2

Call somatic short mutations via local assembly of haplotypes. Short mutations include single nucleotide (SNA) and insertion and deletion (indel) alterations. The caller uses a Bayesian somatic genotyping model that differs from the original MuTect by Cibulskis et al., 2013 and uses the assembly-based machinery of HaplotypeCaller. Of note, Mutect2 v4.1.0.0 onwards enables joint analysis of multiple samples.

Usage:

```
gatk Mutect2 \
     -R reference.fa \
     -I tumor.bam \
     -I normal.bam \
     -normal normal_sample_name \
     --germline-resource af-only-gnomad.vcf.gz \
     --panel-of-normals pon.vcf.gz \
     -O somatic.vcf.gz
```

Mutect2 also generates a stats file names \[output vcf].stats. That is, in the above example the stats file would be named somatic.vcf.gz.stats and would be in the same folder as somatic.vcf.gz. As of GATK 4.1.1 this file is a required input to FilterMutectCalls. As of v4.1 Mutect2 supports joint calling of multiple tumor and normal samples from the same individual. The only difference is that -I and -normal must be specified for the extra samples.

```
gatk Mutect2 \
     -R reference.fa \
     -I tumor1.bam \
     -I tumor2.bam \
     -I normal1.bam \
     -I normal2.bam \
     -normal normal1_sample_name \
     -normal normal2_sample_name \
     --germline-resource af-only-gnomad.vcf.gz \
     --panel-of-normals pon.vcf.gz \
     -O somatic.vcf.gz
```

Tumor-only mode:

```
gatk Mutect2 \
  -R reference.fa \
  -I sample.bam \
  --germline-resource af-only-gnomad.vcf.gz \
  --panel-of-normals pon.vcf.gz \
  -O single_sample.vcf.gz
```

### References on Pughlab for Mutect2

**Panel of normals vcf** `/cluster/projects/pughlab/references/panel_of_normals/hg38/Mutect2/v3.8/mutect2_v3.8_hg38_panel_of_normals_20210330.vcf`

**`gnomad file for germline resource`**

`/cluster/projects/pughlab/references/ExAC/af-only-gnomad.hg38.vcf.gz`



### HaplotypeCaller

Call germline SNPs and indels via local re-assembly of haplotypes The HaplotypeCaller is capable of calling SNPs and indels simultaneously via local de-novo assembly of haplotypes in an active region. In other words, whenever the program encounters a region showing signs of variation, it discards the existing mapping information and completely reassembles the reads in that region. This allows the HaplotypeCaller to be more accurate when calling regions that are traditionally difficult to call, for example when they contain different types of variants close to each other. It also makes the HaplotypeCaller much better at calling indels than position-based callers like UnifiedGenotyper. In the GVCF workflow used for scalable variant calling in DNA sequence data, HaplotypeCaller runs per-sample to generate an intermediate GVCF (not to be used in final analysis), which can then be used in GenotypeGVCFs for joint genotyping of multiple samples in a very efficient way. The GVCF workflow enables rapid incremental processing of samples as they roll off the sequencer, as well as scaling to very large cohort sizes (e.g. the 92K exomes of ExAC).

In addition, HaplotypeCaller is able to handle non-diploid organisms as well as pooled experiment data. Note however that the algorithms used to calculate variant likelihoods is not well suited to extreme allele frequencies (relative to ploidy) so its use is not recommended for somatic (cancer) variant discovery. For that purpose, use Mutect2 instead.

Usage:

```
gatk --java-options "-Xmx4g" HaplotypeCaller  \
   -R Homo_sapiens_assembly38.fasta \
   -I input.bam \
   -O output.g.vcf.gz \
   -ERC GVCF
```

##





## Pughlab Pipeline-Suite

This is a collection of pipelines to be used for DNA- and RNA- sequencing analyses.The  pipeline-suite can be used for any whole-genome, whole-exome, whole-transcriptome or targeted panel sequencing project, including projects with tumour/normal pairs, tumour-only or germline-only samples. For further details visit the GitHub wiki page: [https://github.com/pughlab/pipeline-suite/wiki](https://github.com/pughlab/pipeline-suite/wiki).







## Pughlab Panels

#### 38-Gene Panel:&#x20;

&#x20; Size of this panel is 137,022 bp and consists of 2 subgroups of genes:&#x20;

1\.     MM target/resistance genes (529 probes) – 63,476 bp. Gene list below:&#x20;

| _KRAS_   | _IGF1R_  | _LTB_   |
| -------- | -------- | ------- |
| _NRAS_   | _PTPN11_ | _CRBN_  |
| _BRAF_   | _PRKD2_  | _CUL4B_ |
| _PIK3CA_ | _RASA2_  | _IKZF1_ |
| _RAF3_   | _NFKB1A_ | _IKZF3_ |
| _IDH1_   | _NFKB2_  | _XBP1_  |
| _KDM6A_  | _CYLD_   |         |

2\.     MM-prognosis genes (613 probes) – 73,546 bp. Gene list below:&#x20;

| _IRF4_    | _FGFR3_  | _CDKN2C_  |
| --------- | -------- | --------- |
| _TP53_    | _DIS3_   | _PTEN_    |
| _ATM_     | _TRAF2_  | _CDK4_    |
| _ATR_     | _TRAF3_  | _MAX_     |
| _ZNFHX4_  | _RB1_    | _PRDM1_   |
| _CCND1_   | _EGR1_   | _FAM46C_  |

Location of Bed file for 38-Gene Panel:&#x20;

/mnt/work1/users/pughlab/projects/MiseqQC/targeted\_panels/pughlab\_panels/MYLSTONE2\_Gene\_GRCh38.bed

#### CapIG-Seq:&#x20;

This panel is used for clonal rearrangements of VDJ regions in Ig genes (_IgH_, _IgLκ,_ and _IgLλ_). This panel consists of 3 different probes.

1\.           Ig J (57 probes) – 3,231 bp&#x20;

2\.           Ig V (588 probes) – 68,994 bp&#x20;

3\.           Ig C (118 probes) – 14,211 bp

#### MYLESTONE2:&#x20;

Size of this panel is 174,072 bp and consists of 7 components listed below:&#x20;

1\.     Chr1,17\_SNPs (210 probes) – 25,180 bp&#x20;

2\.     MM-specific CNV regions (151 probes) – 18,104 bp&#x20;

| _TP53_  | _FMA46C_ |
| ------- | -------- |
| _CKS18_ | _CDKN2C_ |

3\.      MM IgH Translocations (172 probes) – 20,626 bp&#x20;

| _FGFR3/ MMSET_ | _MYC translocations_ |
| -------------- | -------------------- |
| _MAF_          | _CCND3_              |
| _MAFB_         | _CCND1_              |

4\.     Ig Hot2 (200 probes) – 23,726 bp&#x20;

5\.     Ig J (57 probes) – 3,231 bp&#x20;

6\.     Ig V (588 probes) – 68,994 bp&#x20;

7\.     Ig C (118 probes) – 14,211 bp&#x20;

Location of Bed file for MYELSTONE2 Panel: &#x20;

/mnt/work1/users/pughlab/projects/MiseqQC/targeted\_panels/pughlab\_panels/MYLSTONE2\_Backbone\_Ig\_regions\_picard.bed

&#x20;

\*\*NOTE\*\* MYLESTONE2 panel can be combined with 38-Gene Panel and is referred to MYL2FULL Panel. The Bed file for MYL2FULL panel is located here:&#x20;

/mnt/work1/users/pughlab/projects/MiseqQC/targeted\_panels/pughlab\_panels/MYLSTONE2\_Gene\_Backbone\_Ig\_regions\_picard.bed



#### CapWAL:

The size is this panel is 146,280 bp and consists of 1219 probes.&#x20;

| _ARID1A_ | _BCL10_   | _NFKBIZ_ |
| -------- | --------- | -------- |
| _ARID1B_ | _MYBBP1A_ | _MALT1_  |
| _CD79A_  | _KMT2D_   | _ATM_    |
| _CD79B_  | _TP53_    | _TRRAP_  |
| _CDKN1B_ | _TBL1XR1_ | _BRAF_   |
| _CXCR4_  | _PTPN13_  | _PLCG2_  |
| _KMT2C_  | _NFKB2_   | _KDM6A_  |
| _MCL1_   | _NFKBIB_  | _UFD1L_  |
| _MYD88_  | _NFKB1_   | _BTK_    |

Location of Bed file for WALPanel:&#x20;

/mnt/work1/users/pughlab/projects/MiseqQC/targeted\_panels/pughlab\_panels/WAL\_signy\_design.bed

#### CapTCRseq:&#x20;

&#x20;           This panel is used to hybrid capture the T-cell repertoire. Specifically, this panel looks at the V and J rearrangements in the T-cell receptor genes. This panel consists for 3 probes:

1\.     TCR J region (94 probes)&#x20;

2\.     TCR D region (238 probes)&#x20;

3\.     TCR V region (299 probes)&#x20;



