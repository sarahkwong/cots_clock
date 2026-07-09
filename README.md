# **An epigenetic clock for the crown-of-thorns seastar (Acanthaster cf. solaris)**  

This repository contains the bioinformatics workflow and analysis scripts used to develop an epigenetic clock for estimating the chronological age of crown-of-thorns seastars (CoTS) from DNA methylation data generated using Oxford Nanopore sequencing.

### Citation 
Kwong, S.L.T., Budd, A.M., Gomez Cabrera, M., Hung, J.Y.-H., Pratchett, M., Villacorta-Rath, C. and Uthicke, S. (under review), DNA methylation-based ageing in a deuterostome invertebrate: an epigenetic clock for the crown-of-thorns seastar (Acanthaster cf. solaris)

### Data availability

Raw sequencing data are available from the NCBI Sequence Read Archive (PRJNA1429840).

---

## **1. Post-hoc canonical and modified (5mC) basecalling**  
 
Raw Nanopore Fast5 files are converted into modbam files containing both canonical and 5mC modified basecalls. The super-accurate (SUP) model is used to ensure low error rate. Using a GPU-supported system is highly recommended, as it significantly reduces processing time.

- **Script:** `basecall.sh`
- **Software:** Guppy v6.5.7 (GPU version) 
- **Input:** Fast5 files  
- **Output:** modbam files (BAM format with methylation annotations)  

## **2.  Convert modbam to bedMethyl**  

Methylation data is extracted from modbam files and converted into a bedMethyl format for downstream analysis compatibility.

- **Script:** `modkit.sh`
- **Software:** Modkit v0.2.1 
- **Input:** modbam files  
- **Output:** bedMethyl (methylation scores per CpG sites)

## **3. Epigenetic clock construction**  

### **3.1 Load data**

Extract, clean, and merge methylation data from bedMethyl files across all samples to prepare for downstream analysis.

- **Script:** `load_data.R`

### **3.2 Age prediction models (LOOCV)**  

Main analysis pipeline for constructing the epigenetic clock. The workflow identifies age-associated CpG sites, trains an elastic net regression model using nested leave-one-out cross-validation, evaluates prediction accuracy, and builds the final optimised epigenetic clock.

- **Script:** `epigenetic_clock_loocv.R`

### **3.3 5-fold cross validation**  

Supplementary validation analysis using nested 5-fold cross-validation to confirm that model performance is robust to an alternative cross-validation strategy.

- **Script:** `epigenetic_clock_5fold.R`
