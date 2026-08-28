# Changelog
### 0.16.3-alpha patch
* removed reliance on shell=True 

### 0.16.2-alpha patch
* added user-friendly way to export scratch directory to Google Drive

### 0.16.1-alpha patch
* added "resume" function to check for partial runs (e.g., server timeout)
* added ability to upload pre-defined block files instead of generating them

### 0.16.0-alpha
* improved Google Colab forms UX
* improved readme/documentation, including reference info
* added functionality converting Biopython .nex blocks to MrBayes format
* translated matrix now saved to new folder (v. overwrite)
* switched to file-streaming (.readline) vs line-by-line to save memory
* added MrBayes functionality

### 0.15.0-alpha
* added nexus conversion/concatenation (for MrBayes)
* added ability to toggle reverse complement detection on/off
* improved reverse-complement handling/corrections
* collapsed input FASTA header renaming into a single shared function
* collapsed input FASTA header renaming into sanitize_fasta_headers()
* collapsed input FASTA header renaming into sanitize_fasta_headers()

### 0.14.0-alpha
* phased out condacolab (curl/wget) w/fixed versions
* added limited UX using Colab Forms
* switched to an input folder of aligned fastas (.afa) instead of a concatenated nexus
* corrected and updated documentation
* added voucher/accession string sanitization (illegal character removal)
* improved efficiency by sanitizing in place and writing with .stack()
* automated FASTA concatenation
* removed; functionality incorporated into join_single_gene_FASTA()
* removed; functionality incorporated into join_single_gene_FASTA()

### 0.13.0-alpha
* make a list of Genbank descriptions to verify sequences
* added checks for packages before installing them (to save time)
* added BLAST to identify sequences needing reassignment
* added alignments using MAFFT for automatic RC detection

### 0.12.0-alpha
* combined genbank_reader() and genbank_to_fasta() into one function
* expanded renaming support
* renamed several functions and variables

### 0.11-alpha
* combined legacy pipelines
