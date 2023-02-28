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
- BWA based alignment workflow for Whole Exome Sequencing (WXS), Whole Genome Sequencing (WGS), Targeted Sequencing, and some other DNA-Seq experimental strategies. The workflow takes either BAM or FASTQ files as input, performs reads mapping, and optional steps of Base Quality Score Recalibration (BQSR), Indel Realignment, MarkDuplicates, and outputs a sorted BAM file, a BAM index file, and various QC metrics.  
- Main CWL: https://github.com/NCI-GDC/gdc-dnaseq-cwl 

### RNA-Seq Alignment
- STAR based RNA-Seq alignment workflow that takes either BAM and FASTQ files as input, and generates 3 BAMs (Genome Aligned BAM, Transcriptome Aligned BAM, Chimeric BAM), STAR Counts, and Splice Junction Quantifications. Among the 3 BAMs generated, Transcriptome aligned BAM is read name sorted instead of coordinate sorted, so it is not companioned by a BAM index file. In addition, the STAR Counts file contains quantification in 3 different ways: strandless mode and two different stranded modes.
- Main CWL: https://github.com/NCI-GDC/gdc-rnaseq-cwl
- Utility scripts: https://github.com/NCI-GDC/gdc-rnaseq-tool 

### miRNA Alignment and Profiling
- miRNA alignment and quantification workflow adopted from https://github.com/bcgsc/mirna. The workflow takes an adaptor-trimmed BAM as input, performs reads mapping, and outputs a sorted BAM file, a BAM index file, miRNA expression, and miRNA isoform expression. 
- Main CWL: https://github.com/NCI-GDC/gdc-mirnaseq-cwl 
- Docker file: https://github.com/NCI-GDC/mirna-profiler-docker 

### RNA-Seq HTSeq Quantification
- RNA-Seq HTSeq quantification workflow takes a GDC aligned RNA-Seq BAM file as input, and outputs HTSeq gene-level qualitifications with various normalization methods. The workflow assumes strandless to maximize compatibility across all GDC projects.
- Main CWL: https://github.com/NCI-GDC/htseq-cwl
- Utility scripts: https://github.com/NCI-GDC/htseq-tool

### WXS Variant Calling
- 4 Tumor-normal paired somatic mutation calling workflow for WXS (or Targeted Sequencing) data. The workflow takes a tumor-tissue BAM and a normal-tissue BAM from the same case, and generates raw calling VCFs, and VCF indexes.  
- As a single combined workflow or run individual callers: GATK3 MuTect2, MuSE, VarScan2, SomaticSniper
- Combined workflow: https://github.com/NCI-GDC/gdc-somatic-variant-calling-workflow 
- MuSE module: https://github.com/NCI-GDC/muse-cwl
- SomaticSniper module: https://github.com/NCI-GDC/somaticsniper-cwl
- VarScan2 module: https://github.com/NCI-GDC/varscan-cwl
- MuTect2 module: https://github.com/NCI-GDC/mutect2-cwl 
- Samtools module: https://github.com/NCI-GDC/samtools-mpileup-cwl
- MuSE docker file: https://github.com/NCI-GDC/muse-tool
- SomaticSniper docker file: https://github.com/NCI-GDC/somaticsniper-tool
- VarScan2 docker file: https://github.com/NCI-GDC/varscan-tool
- MuTect2 docker file: https://github.com/NCI-GDC/mutect2-tool
- Samtools docker file: https://github.com/NCI-GDC/samtools-mpileup-tool

### WXS Variant Filtering
- VCF filtering workflow. GDC internal VCF filtering and formating workflow that runs between variant calling and variant annotation steps. Direct outputs of this workflow are not present in the GDC portal, but rather, used as inputs for the VEP Variant Annotation workflow.  
- Main CWL: https://github.com/NCI-GDC/variant-filtration-cwl 
- Utility scripts: https://github.com/NCI-GDC/variant-filtration-tool

### WGS Variant Calling
- Tumor-normal paired somatic mutation calling workflow for WGS data, adopted from https://github.com/cancerit/dockstore-cgpwgs. The workflow takes a tumor-tissue BAM and a normal-tissue BAM from the same case, and generates simple somatic mutations with CaVEMan and Pindel in VCF format, structural variation with BRASS in VCF and BedPE formats, and copy number variation with ascatNGS in segmentation and gene-level copy number TSV formats.
- Main CWL: https://github.com/NCI-GDC/gdc-sanger-somatic-cwl
- Utility scripts: https://github.com/NCI-GDC/gdc-sanger-somatic-tool

### Tumor-only Variant Calling
- Tumor-only somatic mutation calling workfow for DNA-Seq. The workflow takes a tumor-tissue BAM and generates simple somatic mutations with GATK4 MuTect2 in VCF format. 
- Main CWL: https://github.com/NCI-GDC/gatk4_mutect2_cwl
- Utility scripts: https://github.com/NCI-GDC/gatk4-mutect2-tool

### Tumor-only Variant Filtering
- Tumor-only somatic mutation filtering workfow for DNA-Seq. The workflow takes a tumor-only VCF and tags variants for predicted somatic status by PureCN (https://github.com/lima1/PureCN). The resulting VCF from the tumor-only variant calling workflow can be further annotated using VEP Variant Annotation workflow and converted into MAF files using MAF Generation workflows. 
- Main CWL: https://github.com/NCI-GDC/gdc_tosvc_workflow
- Utility scripts: https://github.com/NCI-GDC/gdc-tosvc-tools

### VEP Variant Annotation
- VEP based variant annotation workflow that provide functional annotations to each variant in the VCFs.
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
- The workflow applies Circular Binary Segmentation to existing BirdSeed probe-level copy numbers, and generates copy number segmentation files and gene-level copy number TSVs.
- Utility scripts: https://github.com/NCI-GDC/dnacopy-tool




