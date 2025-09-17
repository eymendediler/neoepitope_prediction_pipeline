# neoepitope_prediction_pipeline
Pipeline for neoepitope prediction from tumor RNA-seq data. It identifies tumor-specific peptides from SNVs, indels, and alternative splicing events, then predicts their MHC binding. Designed to highlight novel, immunogenic peptides for cancer immunotherapy research.

#Description
RNA-seq → variant calling → VEP (with Transcript Support Level / TSL) → pVACseq neoepitopes (MHC Class I / MHCflurry).
This repository contains a Bash pipeline (neoepitope_pipeline.sh) that performs alignment (STAR), RNA-seq variant calling (GATK Best Practices), VEP annotation with TSL, and neoepitope prediction via pVACseq/MHCflurry.
What this pipeline is for
Exploratory neoepitope prediction from RNA-seq only (no matched DNA).
Generates VEP-annotated VCF and pVACseq epitope tables for specified HLA class I alleles.


#Prerequisites
Software (via conda or modules)
STAR
samtools, bcftools, tabix
GATK 4
Ensembl VEP (with offline cache for GRCh38)
pVACtools (pVACseq)
MHCflurry (models/resources downloaded once)
The script activates environments by name:

ENV_ALIGN=star_env      # STAR + samtools
ENV_GATK=bio_env        # gatk4 + samtools
ENV_VEP=neoepi          # ensembl-vep + bcftools/tabix
ENV_PVAC=neoepi         # pvactools + mhcflurry

Reference data (GRCh38)
FASTA: Homo_sapiens.GRCh38.dna.toplevel.fa (+ .fai)
GTF: Ensembl v114 (or matching the FASTA you use)
STAR genome index built with the same FASTA + GTF
VEP cache for GRCh38 (matching your VEP version), accessible at VEP_CACHE
Inputs
Either paired FASTQs (R1, R2, gzipped) or a coordinate-sorted BAM (BAM_SORTED).
HLA alleles (comma-separated, no spaces), e.g.
HLA-A*02:01,HLA-A*24:02,HLA-B*07:02,HLA-B*15:01,HLA-C*07:02,HLA-C*07:01
Compute & storage (ballpark)
CPU: configurable with THREADS (default 16)
RAM: STAR 2-pass typically 32–60 GB; GATK varies with sample size
Disk (shared, one-time): STAR index ~30–40 GB; VEP cache ~20–50 GB; MHCflurry ~1–2 GB



