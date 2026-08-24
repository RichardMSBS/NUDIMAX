# NUDIMAX: An accessible data-wrangling pipeline for partitioned phylogenetic workflows
By Richard Baker, MS, in collaboration with the [Gosliner Slug Lab](https://sluglab.wordpress.com/) at the California Academy of Sciences (Terry Gosliner, PhD; Lynn Bonomo, MS; and Paola Guzmán, BS)

## Overview
This Python notebook is designed to facilitate data-wrangling for partitioned maximum-likelihood phylogenetic analysis projects that follow a typical workflow of the Gosliner Slug Lab at the California Academy of Sciences (hence, NUDIMAX = NUDIbranch MAXimum-likelihood*). This is all packaged as an interactive Python notebook for use in [Google Colab](https://colab.research.google.com) (and includes a basic user interface using their Forms feature, but relies on an Internet connection) or a [Jupyter notebook](https://jupyter.org/) (which requires interacting directly with the code, but can be installed and run locally). It is also portable to an HPC cluster, if one is available.

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

## Citation
Although NUDIMAX was written to handle the Gosliner Lab's specific use case, it should be helpful for anyone using a similar workflow. If you use NUDIMAX in your research, please cite the project as shown below.

Baker, R., Bonomo, L., Guzmán, P., & Gosliner, T. (2026). NUDIMAX: An accessible data-wrangling pipeline for partitioned phylogenetic workflows (Version 0.16a). Retrieved from https://github.com/RichardMSBS/NUDIMAX

```bibtex
@software{NUDIMAX},
  author = {Baker, Richard and Bonomo, Lynn and Guzmán, Paola and Gosliner, Terry},
  title = {NUDIMAX: An accessible data-wrangling pipeline for partitioned phylogenetic workflows},
  version = {0.16a},
  year = {2026},
  url = {https://github.com/RichardMSBS/NUDIMAX}
}
```

> **Important:** Since NUDIMAX is a wrapper, you should also cite [MAFFT](https://mafft.cbrc.jp/alignment/software/), [IQ-TREE](https://iqtree.github.io/), and/or [MrBayes](https://nbisweden.github.io/MrBayes/).
