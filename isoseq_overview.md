## Overview

PacBio RNA Sequencing (RNASeq) leverages the capabilities of Single Molecule Real-Time (SMRT) Sequencing [to create reads that span whole transcripts and isoforms](https://www.pacb.com/products-and-services/analytical-software/rna-sequencing/). These [long read lengths](https://www.pacb.com/wp-content/uploads/Application-Brief-RNA-sequencing-Best-Practices.pdf) allow more accurate variant detection, and the isoseq software allows comparisons of transcript isoforms (Bruijnesteijn_et_al_2018, Audano_et_al_2019).

The [attached user manual](https://github.com/disulfidebond/avrl-isoseq/blob/Workflow1/SMRT_Tools_Reference_Guide_v600.pdf) from PacBio is verbose, but extremely useful, both as a reference, and as a primer on important topics.  

Here, we will describe one possible workflow for transcript isoform detection and characterization using the software applications within SMRT Tools. 

## Requirements and Setup

* ccs output folder, which should have the following files:
  * file.scraps.bam
  * file.scraps.bam.pbi
  * file.sts.xml
  * file.subreads.bam
  * file.subreads.bam.pbi
  * file.subreadset.xml
* these other files should be present as well:
  * file.adapters.fasta
  * file.baz2bam_1.log
  * file.transferdone
* SMRT_tools v. 6 installed and in the PATH or env
* At least 1 TB of free space, regardless of the size of the PacBio files

## Workflow

### 

ccs --noPolish --numThreads=16 --minLength=300 --minPasses=1 --minZScore=-999 --maxDropFraction=0.8 --minPredictedAccuracy=0.8 --minSnr=4 /slipstream/oc/pacbio/pacbio_raw/6_F01/m54178_190126_092450.subreads.bam m54178_190126_092450.ccs.bam
