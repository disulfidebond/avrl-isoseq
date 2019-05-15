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

The PacBio IsoSeq pipeline can be run manually, using *pbsmrtpipe*, or using the SMRT Analysis GUI.  The first option will be described here, and is applicable to the *pbsmrtpipe* command, which runs a pre-created pipeline. 

### Step 1: CCS
This step converts the CCS subreads into consensus reads.  The following commands have been tested successfully:

        ccs --noPolish --numThreads=16 --minLength=300 --minPasses=1 --minZScore=-999 --maxDropFraction=0.8 --minPredictedAccuracy=0.8 --minSnr=4 /path/to/file/file.subreads.bam outputFile.ccs.bam
        dataset create --type ConsensusReadSet outputFile-Step1.ccs.xml outputFile.ccs.bam
        
Page 83 of the PacBio SMRT Tools User Manual has a detailed explanation of the usage.

This step takes about 96 hours to complete, with the above parameters.

### Step 2: Classify
This step classifies reads into full length or non-full length reads, and checks for chimeric reads.  The following commands have been tested successfully:

        pbtranscript classify outputFile-Step1.ccs.xml isoseq_draft.fasta --flnc=flnc_outputFile.isoseq_flnc.fasta --nfl=nfl_outputFile.isoseq_nfl.fasta

After the step is completed, ensure that the number of artificial concatemers is low, by verifying that the number of full-length, non-chimeric (flnc) reads is slightly less than the number of full-length reads (SMRT_Tools User Guide page 85). 

This step takes about 5 hours to complete, with the above parameters.

### Step 3: Clustering

The Clustering step can be run with or without polishing.  Polishing is the process of examining quality values to further refine consensus sequences of clusters.

Running the classify step without polishing can be done with the following command, and takes about 5 hours:

        pbtranscript cluster flnc_outputFile.isoseq_flnc.fasta outputFile.unpolished_clustered.fasta

Running the classify step with polishing can be done with the following command, and takes over 1 week:

        pbtranscript cluster flnc_outputFile.isoseq_flnc.fasta outputFile.polished_clustered.fasta --quiver --nfl_fa=nfl_outputFile.isoseq_nfl.fasta --bas_fofn=file.subreadset.xml
        
* Other Comments

Note that there is a typo in the User guide; use the option *nfl_fa=* and not *nfl=*

The file *file.subreadset.xml* is the file described above under 'Requirements and Setup'
