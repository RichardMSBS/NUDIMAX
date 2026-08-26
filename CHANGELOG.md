# changelogs
##  Features to include by Tuesday:
* [ ] query GenBank automatically
* [ ] flag suspiciously long/short sequences
* [ ] test Forms UX for portability to HPC

### 0.17-alpha (upcoming)
* [ ] add alerts for non-IUPAC characters
* [ ] add dynamic terminal padding
* [ ] add OR statements to improve handling default parameters in Colab forms

### 0.16.2 patch <-- you are here
#### export_to_drive()
* added user-friendly way to export scratch directory to Google Drive

### 0.16.1 patch
####   MrBayes_wrapper()
* added "resume" function to check for partial runs (e.g., server timeout)
* added ability to upload pre-defined block files instead of generating them

### 0.16-alpha
####   general
* improved Google Colab forms UX
* improved readme/documentation, including reference info
#### MrBayes_converter()
* added functionality converting Biopython .nex blocks to MrBayes format
* translated matrix now saved to new folder (v. overwrite)
* switched to file-streaming (.readline) vs line-by-line to save memory
#### MrBayes_wrapper()
* added MrBayes functionality

### 0.15-alpha (not standalone; rolled into 0.16)
#### general
#### MAFFT_wrapper()
* add nexus conversion/concatenation (for MrBayes)
* added ability to toggle reverse complement detection on/off
#### MAFFT_single()
* improved reverse-complement handling/corrections
#### sanitize_fasta_headers()
* collapsed input FASTA header renaming into a single shared function
#### voucher_matrix_to_genbank()
* collapsed input FASTA header renaming into sanitize_fasta_headers()
#### leaf_renamer()
* collapsed input FASTA header renaming into sanitize_fasta_headers()

### 0.14-alpha
#### general
* phase out condacolab (curl/wget) w/fixed versions
* add limited UX using Colab Forms
* switch to an input folder of aligned fastas (.afa) instead of a concatenated nexus
* correct and update documentation
#### voucher_matrix_to_genbank():
* added voucher/accession string sanitization (illegal character removal)
* improved efficiency by sanitizing in place and writing with .stack()
* automated FASTA concatenation
#### fasta_writer()
* removed; functionality incorporated into join_single_gene_FASTA()
#### rename_fastas()
* removed; functionality incorporated into join_single_gene_FASTA()

#### 0.13-alpha
* make a list of Genbank descriptions to verify sequences
* check for packages before installing them (to save time)
* include BLAST to identify sequences needing reassignment
* align using MAFFT for automatic RC detection

#### 0.12-alpha
* combine genbank_reader() and genbank_to_fasta() into one function
* expand renaming support
* rename several functions and variables

### 0.11-alpha
* combine legacy pipelines
