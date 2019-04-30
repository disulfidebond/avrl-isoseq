## Overview
This writeup provides a brief explanation of typical PacBio workflows.  It is referenced frequently, and unless specifically noted otherwise, all PacBio workflows follow these general steps.

## CCS mapping
Circular Consensus Sequence (CCS) analysis involves sequencing both the forward and reverse strands on a circular stretch of DNA. A [video tutorial](https://www.pacb.com/videos/tutorial-circular-consensus-sequence-analysis-application-smrt-link-v5-0-0/) from PacBio is available that explains the steps involved. The video uses SMRT Tools version 5, however, the description of CCS is still accurate.

The output from CCS mapping step will contain gzipped fastq and fasta files of the consensus sequences, as well as log files.

## Demultiplex
For most PacBio runs (IsoSeq is an exception), samples will be [barcoded](https://www.dnabarcoding101.org/lab/bioinformatics.html), and the barcodes must be removed as part of the analysis process.

There is a modified PacBio Sequel script that performs this task as a part of the typical AVRL PacBio pipeline.

## Long Amplicon Analysis
Long Amplicon Analysis (LAA) is a step in several PAcBio workflows that generates amplicon sequences from PacBio Sequel data. It requires:

* *subreadsetXML* can be from a single dataset, or merged datasets where new XML files are created using 'dataset create'
* *whitelistFasta* is a file containing sequences that will be analyzed by LAA, typically sequences from a single sample
* defaults are set for typical MHC class I genotyping and should be adjusted depending on target
* the default minLength=3000 should always be changed, because this value has caused analyses to fail
* increasing maxClusteringReads will allow more alleles to be detected at the expense of speed:
  * LAA default of 500 clustering reads runs each sample in ~2 minutes, MHC class I default of 10000 takes ~30 minutes but detects more alleles. Setting even higher values like 100,000 clustering reads causes runtimes of several hours.
  * maxReads can be set very high to ensure that all reads are used to accurately define clusters. This doesn't significantly impact runtime


