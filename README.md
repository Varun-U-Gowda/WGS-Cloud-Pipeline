# Cloud-Enabled Microbial WGS Pipeline on AWS

## Overview
This project implements an end-to-end microbial whole genome sequencing (WGS) analysis workflow using Nextflow on AWS. The pipeline performs read quality control, trimming, genome assembly, assembly quality assessment, genome annotation, and taxonomic classification.

The workflow was executed on an AWS EC2 instance, uses Amazon S3 for scalable data storage, and securely accesses cloud resources using IAM role-based authentication.

---

## Workflow Steps
1. FastQC — raw read quality assessment  
2. Trimming — removal of adapters and low-quality bases  
3. SPAdes — genome assembly  
4. QUAST — assembly quality evaluation  
5. Prokka — genome annotation  
6. Kraken2 — taxonomic classification  

---

## Tools Used

- FastQC  
- Trimmomatic  
- SPAdes  
- QUAST  
- Prokka  
- Kraken2  
- Nextflow  

---

## Cloud Architecture

- **Compute:** AWS EC2  
- **Storage:** Amazon S3  
- **Authentication:** IAM role attached to EC2  
- **Workflow Engine:** Nextflow  

### Architecture Flow

S3 (input data)
↓
EC2 (Nextflow pipeline execution)
↓
S3 (results)

IAM Role → secure access between EC2 and S3
---

## Security and Authentication

The pipeline uses an IAM role attached to the EC2 instance to access Amazon S3. This eliminates the need for storing static AWS credentials and follows AWS best practices for secure, role-based access control.

---

## Sample Outputs

Example outputs from a completed run are included in the `sample_output/` directory:

- `ERR13818447_kraken_report.txt` — Kraken2 taxonomic classification report  
- `my_genome_ERR13818447.txt` — Prokka annotation summary  
- `my_genome_ERR13818447.faa` — predicted protein sequences from Prokka  

---

## Pipeline File

- `main.nf` — main Nextflow workflow script  

---

## Prerequisites

- Nextflow installed  
- AWS account with access to S3  
- IAM role attached to EC2 instance with S3 permissions  
- Input FASTQ files stored in S3

---

## How to Run

```bash
nextflow run main.nf \
  --reads "s3://varun-wgs-2026-0413/data/*_{1,2}.fastq.gz" \
  --outdir "s3://varun-wgs-2026-0413/Results" \
  --kingdom "Bacteria" \
  --genus "Escherichia" \
  -resume
