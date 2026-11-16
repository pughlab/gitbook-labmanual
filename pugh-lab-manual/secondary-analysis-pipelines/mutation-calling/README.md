# Mutation calling

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
