---
description: >-
  Welcome to the Pugh Lab fragmentomics pipeline page! On this page, you'll find
  scripts to complete fragmentation features analysis.
---

# Fragmentomics

Fragmentation features refer to properties of DNA fragments which have been sequenced via next-generation sequencing. These analyses are typically completed on cell-free DNA samples, since the cutting of DNA fragments _in vivo_ is **not random.** Some examples of features of fragments which can be evaluated include:&#x20;

* the length of fragments (insert-size),
* the bases at the end of the fragments (end-motifs),
* the distance of the nucleosome-bound fragments to known nucleosome positions (nucleosome peak distances),
* the proportion of short vs long fragments across regions in the genome (short/long fragment ratio),&#x20;
* the frequency of dinucleotides in the fragments (dinucleotide context), &#x20;
* the DNA bases which were at the breakpoint of the DNA fragment, including those which were adjacent to the fragment (fragment breakpoint), and
* the coverage at transcription start sites (nucleosome occupancy).

These features may be useful for inferring if a sample is likely to be a tumour or healthy sample, the origin of a tumour, and tumour burden.&#x20;

To run these tools, whole-genome sequencing is recommended with a depth of at least 1X, with a depth of at least 30X for nucleosome occupancy. Higher depth yields the ability to focus on more nuanced regions of the genome which may be especially relevant to the disease of interest.&#x20;

More information on each of these features and how to run them will be discussed in the subpages below. Additionally, these fragmentomics analyses are available within the Pughlab Pipeline-Suite (see the Pipeline-Suite wiki page for further details: [https://github.com/pughlab/pipeline-suite/wiki/Plasma-WGS-Fragmentomics](https://github.com/pughlab/pipeline-suite/wiki/Plasma-WGS-Fragmentomics)).



