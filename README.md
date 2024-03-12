# TRS-omix: Search Engine

This code accompanies the paper:

- **Sebastian Sakowski, Marta Majchrzak, Jacek Waldmajer, Pawel Parniewski**: *TRS-omix: a new search engine for trinucleotide flanked sequences*. 2021.

The content of the repository is derived from its predecessor, available at:
[https://github.com/TRS-omix/software](https://github.com/TRS-omix/software)

## ToDo's:

### Simple Tasks

1. **Sequence Flanking Correction**: Add flanking sequences (`*CGACGACGACG*`) analogously on the right side.

2. **Batch File for Genome List**: Upon program startup, the batch file should contain a list of genomes with arbitrary genome file names.

3. **FRG NO Column in Output**: Currently manually generated and needs automation. It should contain a string from the fasta file header (`>..." "`) with a prefix-index.

4. **Sequence Similarity in `interiors.txt` ("Interiors")**: Address similarity of sequences within `interiors.txt`.

## Operating Mechanism
> [!IMPORTANT]
> 1. **Environment Setup with Conda**: The necessary package list for script operation is in `environment.yml`.
   
   Quick installation command in terminal: `conda create env -f environment.yml -n TRS`

> [!IMPORTANT]
> 2. **Activate Environment**: Use `conda activate TRS`. All further operations should be performed in this environment.

> [!WARNING]
> 3. **Compilation of TRS-wrapper**: (Request to Mr. Rafal for the exact script needed for compilation)

> [!NOTE]
> 4. **Usage of `TRS_and_fasta_revised.py`**: This script is used to obtain initial results for subsequent BLAST analysis. Detailed operation described below.

### TRS_and_fasta_revised.py

1. Asks the user for the location of the folder containing `.fasta` files. Full path specification is recommended.

2. Queries additional parameters to be used in TRS-omix (minimum and maximum length, mode).

> [!IMPORTANT]
> 3. Creates a new path in the folder where the script is located, named according to pattern inputdirectory_results. This path will contain all files generated during analysis, dynamically changing to include information about the experiment.

> [!CAUTION]
> 4. Do not modify this folder in any way. Files within it can be copied elsewhere, but the original location and file names must be preserved—at least for now.

> [!NOTE]
> 5. Upon specifying required parameters and creating the folder, TRS-omix operates, typically taking 1-1.5 hours for 7 genomes. The analysis duration also depends on the maximum length parameter.

6. TRS output is saved in the `TRS_output` folder, with an equivalent to `interiors.txt` always named `${\color{red}specified_folder_name}_results.csv`.

7. The script then asks how much of the sequence start and end the user wants to extract, with maximum length limited by the minimum sequence length. If the user specifies a value beyond the accepted maximum, they will be informed, and the length will be adjusted.

8. Users are asked for their email to use Entrez and the `GENOME` column in `${\color{red}specified_folder_name}_results.csv` to obtain organism names. It's best if the `.fasta` files originate from NCBI Nucleotide for compatibility.

9. Extracted sequence fragments are saved to a `.fasta` file named "combined_sequences.fasta", with sequences named according to the scheme: `Species_name_L/R{number}`.

10. Each L/R pair receives a number scheme `Species_name_L/R{number}_{pair_number}`, and sequences are saved to `combined_sequences_unique.fasta`.

11. Subsequent operations include clustering with cd-hit (automated if cd-hit is installed or will prompt for path if not found) and setting the desired identity degree.

12. The script also performs operations on clusters to clean them and obtain sequence IDs to be discarded. cd-hit results are located in the `cd-hit results` folder.

13. Two new fasta files are created in the `filtered_sequences` folder, one containing sequences within clusters and another outside them.

14. These files should then be BLASTed against the nt database with parameters `perc_identity 100 -outfmt 6`.
"""
