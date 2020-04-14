# Overview of GDC Harmonization Workflows

## Description
- Here are the overview of major GDC harmonization workflows.
- The GDC workflow repositories has been tested on GDC data and in the particular environment GDC is running in. 
- Current GDC production workflows are running in the GDC Pipeline Automation System (GPAS).
- Most GDC workflows are developed using [Common Workflow Language (CWL)](https://www.commonwl.org/ "Common workflow Language") with [Dockerized](https://www.docker.com/) tools.
- Please check [GDC Documentation](https://docs.gdc.cancer.gov/Data/Bioinformatics_Pipelines/DNA_Seq_Variant_Calling_Pipeline/#somatic-variant-calling-workflow) for more details. 

## For external users
The GDC workflow repositories has been tested on GDC data and in the particular environment GDC is running in. 
- We have created external CWL entrypoint in some workflows. For the others, you are expecting to modify the workflow to be used in your system. 
- Some of the reference data required for the workflow production are hosted in [GDC reference files](https://gdc.cancer.gov/about-data/data-harmonization-and-generation/gdc-reference-files "GDC reference files"). We can not share some other reference files due to licensing issues. 
  - In particular, we can not share target or bait bed files or interval list of any specific Target Capture Kits. You are encouraged to contact the vendors or project owners to get these files.
- We are not able to share dockers to external users due to licensing issues. You are welcomed to build your own dockers using the docker files provided.
- For any questions related to GDC data, please contact the GDC Help Desk at support@nci-gdc.datacommons.io.

## Production Workflows Include

### DNA-Seq Alignment
- BWA based alignment workflow for Whole Exome Sequencing (WXS), Whole Genome Sequencing (WGS), Targeted Sequencing, and some other DNA-Seq experimental strategies
- Main CWL: https://github.com/NCI-GDC/gdc-dnaseq-cwl 

### RNA-Seq Alignment
- STAR based RNA-Seq alignment workflow that generates 3 BAMs (Genome Aligned BAM, Transcriptome Aligned BAM, Chimeric BAM), STAR Counts, and Splice Junction Quantifications
- Main CWL: https://github.com/NCI-GDC/gdc-rnaseq-cwl
- Utility scripts: https://github.com/NCI-GDC/gdc-rnaseq-tool 

### miRNA Alignment and Profiling
- miRNA alignment and quantification workflow adopted from https://github.com/bcgsc/mirna
- Main CWL: https://github.com/NCI-GDC/gdc-mirnaseq-cwl 
- Docker file: https://github.com/NCI-GDC/mirna-profiler-docker 

### RNA-Seq HTSeq Quantification
- RNA-Seq HTSeq quantification workflow, assuming strandless to maximize compatibility 
- Main CWL: https://github.com/NCI-GDC/htseq-cwl
- Utility scripts: https://github.com/NCI-GDC/htseq-tool

### WXS Variant Calling
- 4 Tumor-normal paired somatic mutation calling workflow for WXS (or Targeted Sequencing) data
- As a single combined workflow or run individual callers: GATK3 MuTect2, MuSE, VarScan2, SomaticSniper
- Combined workflow: https://github.com/NCI-GDC/gdc-somatic-variant-calling-workflow 
- MuSE module: https://github.com/NCI-GDC/muse-cwl
- SomaticSniper module: https://github.com/NCI-GDC/somaticsniper-cwl
- VarScan2 module: https://github.com/NCI-GDC/varscan-cwl
- MuTect2 module: https://github.com/NCI-GDC/mutect2-cwl 
- Samtools module: https://github.com/NCI-GDC/samtools-mpileup-cwl
- MuSE docker file: https://github.com/NCI-GDC/muse-tool
- MuTect2 docker file: https://github.com/NCI-GDC/somaticsniper-tool
- VarScan2 docker file: https://github.com/NCI-GDC/varscan-tool
- SomaticSniper docker file: https://github.com/NCI-GDC/mutect2-tool
- Samtools docker file: https://github.com/NCI-GDC/samtools-mpileup-tool

### WXS Variant Filtering
- VCF filtering workflow 
- Main CWL: https://github.com/NCI-GDC/variant-filtration-cwl 
- Utility scripts: https://github.com/NCI-GDC/variant-filtration-tool

### WGS Variant Calling
- Tumor-normal paried somatic mutation calling workflow for WGS data, adopted from https://github.com/cancerit/dockstore-cgpwgs
- Include CaVEMan (point mutation), Pindel (small indel), BRASS (structural variation), and ascatNGS (copy number variation)
- Main CWL: https://github.com/NCI-GDC/gdc-sanger-somatic-cwl
- Utility scripts: https://github.com/NCI-GDC/gdc-sanger-somatic-tool
- We are still working on these, and will make them public soon

### VEP Variant Annotation
- VEP based variant annotation workflow
- Main CWL: https://github.com/NCI-GDC/vep-cwl 
- Utility scripts: https://github.com/NCI-GDC/vep-tool 

### Mutation Annotation File (MAF)
- MAF generation workflows 
- MAF schema: https://github.com/NCI-GDC/maf-lib/tree/master/src/maflib/resources
- MAF library: https://github.com/NCI-GDC/maf-lib (maf library)
- Main CWL: https://github.com/NCI-GDC/aliquot-maf-cwl
- Utility scripts: https://github.com/NCI-GDC/aliquot-maf-tools

## One-off Workflows Include
### SNP6 Segmentation
- Utility scripts: https://github.com/NCI-GDC/dnacopy-tool




