## Usage and example inputs

__*Note:__ These examples are raw inputs. To use the Google Colab Forms UI, open the (included .ipynb markdown)[https://github.com/RichardMSBS/NUDIMAX/blob/main/nudimax/(prerelease)_NUDIMAX_v0_16_2.ipynb] in Google Drive.

### export_to_drive() (COLAB ONLY) 
export_to_drive() takes up to three inputs:
* destination   (Optional) is the target file path on Google Drive. Defaults to NUDIMAX_backup_<timestamp>
* source        is the file or folder to back up
* copy_all      (Optional) will copy the entire NUDIMAX scratch folder.

```
copy_all = False # @param {"type":"boolean"}
nudimax.export_to_drive(destination = destination = "NUDIMAX_backup",
                        source      = 'Tenellia_backup',
                        copy_all    = False)
```

### voucher_matrix_to_genbank()
The input to voucher_matrix_to_genbank() is a CSV spreadsheet that looks like this. It can take any number of genes or voucher IDs.

| Voucher       | 16S      | COI      | H3       | gene4    |
| :---          | :---     | :---     | :---     | :---     |
| CASIZ 174485  | KY128712 | KY128917 | KY128504 | AB123456 |
| CASIZ 179463a | KY128713 | KY128918 | KY128505 | AB789012 |

```
voucher_matrix_to_genbank(Tenellia_corrected.csv)
```

### genbank_to_fastas()
genbank_to_fasta() creates a .zip archive containing one folder. This subfolder will contain one FASTA file per gene in the original table input CSV. 

genbank_to_fastas() takes three inputs:
* file_prefix       is a file prefix or project name common to all input files
* assignment_matrix is the clean CSV file produced by voucher_matrix_to_genbank()
* genbank_input     is the .gb download from genbank

```
nudimax.genbank_to_fastas(file_prefix       = "Tenellia",
                          assignment_matrix = "Tenellia_corrected_clean.csv",
                          genbank_input     = "Tenellia_corrected.gb")
```

### BLAST_wrapper()
BLAST_wrapper() creates a .zip archive containing contains one TSV file for each FASTA. For each sequence in that FASTA, it will display the top n hits (including descriptions) to help identify sequences that may be assigned to the incorrect gene in the input CSV. 

BLAST_wrapper() takes three inputs:
* input_type is 'file' or 'folder'
* input_name is the FASTA or folder name (containing FASTAs)
* max_hits   is the maximum matches to return per query (Recommmended: 10)

```
nudimax.BLAST_wrapper(input_type = 'folder',
                      input_name = 'Tenellia_out',
                      max_hits   = 10)
```

### MAFFT_wrapper()
MAFFT_wrapper() creates at least one output.
* if input_type = 'file', this will be a single aligned FASTA (.afa)
* if input_type = 'folder', it will be a .ZIP archive containing...
- a  "logs" folder, containing MAFFT logs (.log)
- an "afa"  folder, containing aligned FASTAs (.afa)
- a  "nex"  folder, containing single NEXUS alignments (.nex)
* if "concat" = True, then there will be a combined file (.supermatrix.nex)

MAFFT_wrapper() takes up to five inputs:
* input_type is 'file' or 'folder'
* input_name is a FASTA filename, or the name of a folder containing FASTAs
* prefix          (optional, default: input name) is the project name
* adjustdirection (optional, default: True) MAFFT reverse-complement detection
* concat          (optional, default: True) creates a concatenated Nexus supermatrix (.supermatrix.nex) for use in MrBayes

```
nudimax.MAFFT_wrapper(input_type      = "folder",
                      input_name      = "Tenellia_concat",
                      prefix          = "",
                      adjustdirection = True,
                      concat          = False)
```

### IQTREE_wrapper()
iqtree_wrapper() takes an input alignment and runs a maximum-likelihood (ML) phylogenetic analysis with bootstrapping and substitution model selection (defaults are 1000 and AUTO, respectively).

iqtree_wrapper() takes at least two inputs...
* prefix     is your project name
* input      is an alignment, or folder containing multiple aligned FASTAs
* bootstraps (optional) is the number of bootstraps (default: 1000)
* model      (optional) is the model to use (default: MFP)
* redo       (optional) will re-run an existing analysis if one is detected (default: False)
* nthreads   (optional) is the number of CPU threads to assign (default: AUTO)

nudimax.iqtree_wrapper(prefix = "Tenellia",
                               input  = "Tenellia_concat_aligned/afa",
                               bootstraps = 1000,
                               model = "MFP',
                               nthreads = "AUTO",
                               redo = False)
