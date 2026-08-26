## changelogs
#  Features to include by Tuesday:
#    [ ] query GenBank automatically
#    [ ] flag suspiciously long/short sequences
#    [ ] test Forms UX for portability to HPC

### 0.17-alpha (upcoming)
##   general
#    [ ] add alerts for non-IUPAC characters
#    [ ] add dynamic terminal padding
#    [ ] add OR statements to improve handling default parameters in Colab forms

### 0.16.2 patch <-- you are here
##   export_to_drive()
#    [X] added user-friendly way to export scratch directory to Google Drive

### 0.16.1 patch
##   MrBayes_wrapper()
#    [X] added "resume" function to check for partial runs (e.g., server timeout)
#    [X] added ability to upload pre-defined block files instead of generating them

### 0.16-alpha
##   general
#    [X] improved Google Colab forms UX
#    [X] improved readme/documentation, including reference info
#    MrBayes_converter()
#    [X] added functionality converting Biopython .nex blocks to MrBayes format
#    [X] translated matrix now saved to new folder (v. overwrite)
#    [X] switched to file-streaming (.readline) vs line-by-line to save memory
#    MrBayes_wrapper()
#    [X] added MrBayes functionality

### 0.15-alpha (not standalone; rolled into 0.16)
##   general
#    MAFFT_wrapper()
#    [X] add nexus conversion/concatenation (for MrBayes)
#    [X] added ability to toggle reverse complement detection on/off
#    MAFFT_single()
#    [X] improved reverse-complement handling/corrections
#    sanitize_fasta_headers()
#    [X] collapsed input FASTA header renaming into a single shared function
#    voucher_matrix_to_genbank()
#    [X] collapsed input FASTA header renaming into sanitize_fasta_headers()
#    leaf_renamer()
#    [X] collapsed input FASTA header renaming into sanitize_fasta_headers()

### 0.14-alpha
##   general
#    [X] phase out condacolab (curl/wget) w/fixed versions
#    [X] add limited UX using Colab Forms
#    [X] switch to an input folder of aligned fastas (.afa) instead of a concatenated nexus
#    [X] correct and update documentation
##   voucher_matrix_to_genbank():
#    [X] added voucher/accession string sanitization (illegal character removal)
#    [X] improved efficiency by sanitizing in place and writing with .stack()
#    [X] automated FASTA concatenation
##   fasta_writer()
#    [X] removed; functionality incorporated into join_single_gene_FASTA()
#    rename_fastas()
#    [X] removed; functionality incorporated into join_single_gene_FASTA()

### 0.13-alpha
#    [X] make a list of Genbank descriptions to verify sequences
#    [X] check for packages before installing them (to save time)
#    [X] include BLAST to identify sequences needing reassignment
#    [X] align using MAFFT for automatic RC detection

### 0.12-alpha
#    [X] combine genbank_reader() and genbank_to_fasta() into one function
#    [X] expand renaming support
#    [X] rename several functions and variables

### 0.11-alpha
#    [X] combine legacy pipelines
