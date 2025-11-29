# **Antibiotic Resistance Gene Detection in *Klebsiella pneumoniae***

**Dataset:** SRR24819222
**Author:** Danish Alvi

---

## 📌 **Overview**

This project presents a complete antimicrobial resistance (AMR) gene detection workflow using whole-genome sequencing (WGS) data from a multidrug-resistant (MDR) *Klebsiella pneumoniae* clinical isolate (SRA Run: **SRR24819222**).
The pipeline demonstrates raw data preprocessing, genome assembly, and resistance gene identification using the **CARD – Resistance Gene Identifier (RGI)** tool.

This project is designed as a clean, reproducible workflow suitable for early-career microbiologists, genomic researchers, and applicants building a professional bioinformatics portfolio.

---

## 🧬 **Dataset Information**

* **Organism:** *Klebsiella pneumoniae*
* **SRA ID:** **SRR24819222**
* **Study:** Carbapenem-resistant Enterobacterales Genome Sequencing and Assembly
* **Sequencing Platform:** Illumina NovaSeq 6000
* **Library:** Paired-end (150 bp × 2)
* **Size:** 3.9M spots, ~364 MB download

---

## 🎯 **Project Objectives**

* Download *K. pneumoniae* WGS data from SRA
* Perform FASTQ quality assessment (FastQC)
* Trim adapters and low-quality reads (fastp)
* Assemble the genome (SPAdes)
* Detect antibiotic resistance genes using **CARD RGI**
* Generate comprehensive, publication-grade AMR reports

---

## 🚀 **Workflow Summary**

### **1. Download Sequencing Reads**

```bash
prefetch SRR24819222
fasterq-dump SRR24819222 --split-files -O data/raw/
```

### **2. Raw Quality Control**

```bash
fastqc data/raw/*.fastq -o results/fastqc_raw/
```

### **3. Read Trimming (fastp)**

```bash
fastp \
  -i data/raw/SRR24819222_1.fastq \
  -I data/raw/SRR24819222_2.fastq \
  -o data/trimmed/SRR24819222_1.trimmed.fastq \
  -O data/trimmed/SRR24819222_2.trimmed.fastq \
  --html results/fastp_report.html
```

### **4. Genome Assembly (SPAdes)**

```bash
spades.py \
  -1 data/trimmed/SRR24819222_1.trimmed.fastq \
  -2 data/trimmed/SRR24819222_2.trimmed.fastq \
  -o data/assembly/
```

### **5. Antibiotic Resistance Gene Identification (CARD RGI)**

```bash
rgi main \
  --input_sequence data/assembly/contigs.fasta \
  --output_file results/rgi/kpneumoniae_amr \
  --input_type contig \
  --local \
  --clean
```

---

## 📁 **Repository Structure**

```
Klebsiella-pneumoniae-AMR-Gene-Detection/
│
├── data/
│   ├── raw/             # Downloaded FASTQ files
│   ├── trimmed/         # Cleaned FASTQ files
│   └── assembly/        # SPAdes assembly
│
├── results/
│   ├── fastqc_raw/
│   ├── fastqc_trimmed/
│   ├── rgi/             # CARD RGI results
│   └── multiqc/         # Summary reports (optional)
│
├── scripts/
│   ├── download_data.sh
│   ├── fastqc.sh
│   ├── trimming.sh
│   ├── assembly.sh
│   └── rgi_analysis.sh
│
├── docs/
│   ├── workflow_diagram.png
│   └── project_overview.pdf
│
└── README.md
```

---

## 📊 **Expected Outputs**

* FastQC quality reports (HTML)
* Trimmed reads
* Assembled genome (contigs.fasta)
* RGI AMR detection outputs (JSON, TXT, TSV)
* List of detected resistance genes including:

  * β-lactamases
  * Carbapenemases
  * Aminoglycoside resistance
  * Efflux pump genes
  * Mobile genetic elements

---



