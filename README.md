# NUDIMAX: An accessible data-wrangling pipeline and wrapper for partitioned phylogenetic workflows
Richard Baker,¹ Lynn J. Bonomo,² Paola Guzmán² ³, Terry Gosliner²

¹Center for Comparative Genomics, California Academy of Sciences

²Department of Invertebrate Zoology & Geology 

³University of Puerto Rico at Cayey

## Overview
This Python notebook is designed to facilitate data-wrangling for partitioned maximum-likelihood phylogenetic analysis projects that follow a typical workflow of the [Gosliner Slug Lab](https://sluglab.wordpress.com/) at the California Academy of Sciences (hence, NUDIMAX = NUDIbranch MAXimum-likelihood*). This is all packaged as an interactive Python notebook for use in [Google Colab](https://colab.research.google.com) (and includes a basic user interface using their Forms feature, but relies on an Internet connection) or a [Jupyter notebook](https://jupyter.org/) (which requires interacting directly with the code, but can be installed and run locally). It is also portable to an HPC cluster, if one is available.

> **\*Note:** Since v0.16, NUDIMAX supports both maximum-likelihood and Bayesian inference.

## Features
This is designed to:
- Receive a CSV matrix of accession numbers and GenBank IDs as input (see example Table 1)
- Process that CSV and output a list of GenBank accession numbers for submission to [BatchEntrez](https://www.ncbi.nlm.nih.gov/sites/batchentrez).
- Process the GenBank download and produce separate FASTA files for each gene
- Remotely query the NCBI BLAST database to verify that GenBank accessions were assigned correctly*
- Align those sequences (using MAFFT), returning both FASTA and NEXUS alignment files for partitioned maximum-likelihood analysis (ML/IQ-TREE)
- Combine the alignment files into a partitioned NEXUS for use in Bayesian analysis (BI/MrBayes)
- Run both the ML and BI phylogenetic analyses packages
- Rename Newick branches (e.g., adding species names)

> **\*Note:** While this feature is included, a remote BLAST query is much slower than the BLAST web interface or using a local database.

## Methodology
This is accomplished via a number of custom functions, which primarily use Biopython and Pandas. Importantly, these custom functions only wrangle and convert data; NUDIMAX does not perform any analysis or computation directly. Instead, it includes user-friendly wrappers that allow users to call peer-reviewed packages (including MAFFT, IQ-TREE, MrBayes, and optionally NCBI BLAST) without requiring command-line interaction, root access, or macOS/Windows/Linux compatibility issues.

## Disclaimer
This notebook is provided "as-is" with no warranty. While this tool is designed to automate and streamline a standard workflow, it remains the user's responsibility to validate all data entering and exiting the program. Users are **strongly** encouraged to read the code and comments to ensure that this pipeline meets the specific needs of their project.

NUDIMAX is still in active development, and should be considered to be in an alpha state. While it has been tested with a number of inputs, it may contain bugs as yet unseen (and therefore, unsquashed).

## Installation
To install NUDIMAX, simply copy the following into a Python environment. The setup step will take about two minutes.
```
!pip install git+https://github.com/RichardMSBS/nudimax.git
import nudimax
nudimax.setup()
```
Alternatively, save the included .ipynb file to Google Drive. Drive will automatically open in the Colab environment.

## Usage and example inputs
<details><summary>Click to expand/collapse</summary>

__*Note:__ These examples are raw inputs. To use the Google Colab Forms UI, open the [included .ipynb markdown](https://github.com/RichardMSBS/NUDIMAX/blob/main/nudimax/(prerelease)_NUDIMAX_v0_16_2.ipynb) in Google Drive.

### export_to_drive() (COLAB ONLY) 
export_to_drive() takes up to three inputs:
* destination   (Optional) is the target file path on Google Drive. Defaults to NUDIMAX_backup_<timestamp>
* source        is the file or folder to back up
* copy_all      (Optional) will copy the entire NUDIMAX scratch folder.

```
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
genbank_to_fastas() converts a GenBank download (in GenBank format, .gb) and voucher assignment matrix (i.e., from voucher_matrix_to_genbank()) to a bundle of FASTAs organized by gene. The FASTA headers will be set according to the assignment matrix; in the table above, this means that the GenBank IDs in each row (e.g., KY128712, KY128917, KY128504, and AB123456) will share the assigned voucher name in the first column (e.g., CASIZ 174485).

genbank_to_fastas() takes three inputs:
* file_prefix       is a file prefix or project name common to all input files
* assignment_matrix is the clean CSV file produced by voucher_matrix_to_genbank()
* genbank_input     is the .gb download from genbank

```
nudimax.genbank_to_fastas(file_prefix       = "Tenellia",
                          assignment_matrix = "Tenellia_corrected_clean.csv",
                          genbank_input     = "Tenellia_corrected.gb")
```

### genbank)concatenator()
genbank_concatenator() combines the above three functions, and additionally allows the user to upload their own FASTAs for each gene specified in the input CSV. Note that each fasta must end in the gene name specified in the CSV; a valid example file set based on the table above would be my_samples_16S.fasta, my_samples_COI.fasta, and my_samples_H3.fasta.

genbank_concatenator() takes 4 arguments:
* file_prefix        is a file prefix (e.g., the project name)
* assignment_matrix  is the clean CSV file produced by voucher_matrix_to_genbank()
* genbank_input      is the .gb download from genbank
* input_archive_name is the name of the .zip archive being uploaded

```
nudimax.genbank_concatenator(file_prefix        = "Tenellia",
                             assignment_matrix  = "Tenellia_corrected_clean.csv",
                             genbank_input      = "Tenellia_download.gb",
                             input_archive_name = "my_samples.zip");
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
MAFFT_wrapper() uses MAFFT to align DNA/RNA/AA sequences.
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

### iqtree_wrapper()
iqtree_wrapper() takes an input alignment and runs a maximum-likelihood (ML) analysis with bootstrapping and substitution model selection (defaults are 1000 and AUTO, respectively) to produce phylogenetic trees.

iqtree_wrapper() takes at least two inputs...
* prefix     is your project name
* input      is an alignment, or folder containing multiple aligned FASTAs
* bootstraps (optional) is the number of bootstraps (default: 1000)
* model      (optional) is the model to use (default: MFP)
* redo       (optional) will re-run an existing analysis if one is detected (default: False)
* nthreads   (optional) is the number of CPU threads to assign (default: AUTO)

```
nudimax.iqtree_wrapper(prefix     = "Tenellia",
                       input      = "Tenellia_concat_aligned/afa",
                       bootstraps = 1000,
                       model      = "MFP',
                       nthreads   = "AUTO",
                       redo       = False)
```

### MrBayes_wrapper()
MrBayes_wrapper() takes an input alignment and runs a Bayesian Markov Chain Monte Carlo (BI or MCMC) analysis with adjustable parameters to produce phylogenetic trees. 

MrBayes_wrapper() takes several inputs:
* prefix      is the desired output file prefix (i.e., project name)
* input_nex   is the path to the input Nexus alignment file (.nex)
* input_block (Optional) is a pre-written MrBayes parameter block provided by the user; if none is provided, MrBayes_wrapper will attempt to create one
* runs        (Optional, default 2) is the number of MCMC analyses to run
* nchains     (Optional, default: 4) is the number of MCMC chains to use
* burnin      (Optional, default: 0.25) is the burn-in time. This is either an integer (n generations) OR a decimal (0.25)
* generations (Optional, default: 1000000) is the number of generations
* samplefreq  (Optional, default: 1000) is the MCMC sample frequency
* partitioned (Optional, default: False) toggles partitioned analysis on/off
* resume      (Optional, default: True) will resume a partially completed run if one is detected

```
nudimax.MrBayes_wrapper(prefix      = "Tenellia",
                        input_nex   = "Tenellia_concat_aligned.supermatrix.nex",
                        input_block = "Tenellia_concat_aligned.bayesblock.nex",
                        runs        = 2, 
                        nchains     = 4,
                        burnin      = 0.25,
                        generations = 1000000,
                        samplefreq  = 1000,
                        partitioned = True,
                        resume      = True)
```

</details>

## Citation
Although NUDIMAX was written to handle the Gosliner Lab's specific use case, it should be helpful for anyone using a similar workflow. If you use NUDIMAX in your research, please cite the project as shown below.

Baker, R., Bonomo, L., Guzmán, P., & Gosliner, T. (2026). NUDIMAX: An accessible data-wrangling pipeline for partitioned phylogenetic workflows (Version 0.16.2-alpha). Retrieved from https://github.com/RichardMSBS/NUDIMAX

```bibtex
@software{NUDIMAX,
  author = {Baker, Richard and Bonomo, Lynn and Guzmán, Paola and Gosliner, Terry},
  title = {NUDIMAX: An accessible data-wrangling pipeline for partitioned phylogenetic workflows},
  version = {0.16.2-alpha},
  year = {2026},
  url = {https://github.com/RichardMSBS/NUDIMAX}
}
```

> **Important:** Since NUDIMAX is a wrapper, you should also cite [MAFFT](https://mafft.cbrc.jp/alignment/software/), [IQ-TREE](https://iqtree.github.io/), and/or [MrBayes](https://nbisweden.github.io/MrBayes/).
