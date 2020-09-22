# Introduction to miniconda on the Neufeld lab servers
Copyright Neufeld Research Group, 2020

This introductory tutorial covers installation of miniconda and usage of miniconda to run an example search on BLAST.

## Installation of miniconda
Go to your downloads folder:
```bash
# If you don't already have a Downloads folder, then you can make one
mkdir -p ${HOME}/Downloads

cd Downloads
```

Download the miniconda3 installer:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

Run the installer:
```bash
bash Miniconda3-latest-Linux-x86_64.sh
# A license agreement will come up; you can read and agree to it
# On neufeldserver: you can use the default install location for your miniconda directory
# ****On mellea: put your miniconda directory at /Analysis/[your_username]/miniconda3
```
Then log out and log back in, and conda should work! Test it by running `conda list` (a short list of tools should appear).


## Tutorial: running a BLAST search

### Install BLAST using conda
We will create a conda environment containing BLAST:
```bash
conda create --name blast --channel bioconda blast
```

Activate the environment to be able to use BLAST:
```bash
conda activate blast
```

You should now be able to see the BLAST help page:
```bash
blastn -help
```

### Download some example data

```bash
# Create an analysis directory and go there
mkdir -p "${HOME}/analysis/2020_09_22_blast_test"
cd "${HOME}/analysis/2020_09_22_blast_test"

# Query sequences
wget 

# Subject sequences
wget 
```

Note that FASTA files are just text files. You can view them using some built-in text editors on the server. For example, try:
```bash
# Print the whole file to the screen
cat query_sequences.fasta

# Open an interactive text editor to see the file. You can exit afterwards by pressing control + x.
nano query_sequences.fasta
```

### Run the example BLAST command

It's pretty simple to run jobs on command line. With your BLAST conda environment turned on, run:
```bash
blastn -query query_sequences.fasta -subject subject_sequences.fasta
```
A bunch of results should be printed to the screen.


Let's break down the above command:
- `blastn`: the name of the program that was installed via conda
- `-query`: this is called a *flag*. It is basically a way to set a parameter on a software tool run on the server. In this case, if you looked at the help file for BLAST, you'd see that this flag is used to specify the filename of the query FASTA sequence
- `query_sequences.fasta`: this is the filename of the query FASTA file on the server
- `-subject`: another flag
- `subject_sequences.fasta`: another file

There are lots of other flags you can set for a BLAST search as well, which you can see via `blastn -help`.

For example, we can change the output format of BLAST:
```bash


```
