# Introduction to miniconda on the Neufeld lab servers
Copyright Neufeld Research Group, 2020

This introductory tutorial covers installation of miniconda and usage of miniconda to run an example search on BLAST.

## Installation of miniconda
Miniconda is a really helpful tool for installing and managing bioinformatics software on your account on the server. We'll work through how to use miniconda below. For now, install miniconda using the following commands:

1. Go to your downloads folder:
```bash
# If you don't already have a Downloads folder, then you can make one
mkdir -p ${HOME}/Downloads

cd Downloads
```

2. Download the miniconda3 installer:
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

3. Run the installer:
```bash
bash Miniconda3-latest-Linux-x86_64.sh
# A license agreement will come up; you can read and agree to it
# On neufeldserver: you can use the default install location for your miniconda directory
# ****On mellea: put your miniconda directory at /Analysis/[your_username]/miniconda3
```

4. Delete the installer file (no longer needed):
```bash
rm Miniconda3-latest-Linux-x86_64.sh
```

Then log out and log back in, and conda should work! Test it by running `conda list` (a short list of tools should appear).


## Tutorial: running a BLAST search
Now, let's practice using miniconda in a practical example. Say we have a 16S rRNA gene sequence and want to compare it to some reference sequences using BLAST. 
You could do this using BLASTN online, but you can also run BLASTN on the server using the command line version of BLASTN. 
Let's install BLAST on your server account using conda and then try it out.


### Install BLAST using conda
Create a conda environment containing BLAST:
```bash
conda create --name blast --channel bioconda blast
# After waiting a minute, a prompt will ask you if you're okay to install some packages to get blast. Type 'y' and hit enter to agree. Then BLAST will install.
```

Conda stores installed software in "conda environments". To use BLAST now that you've installed it, you need to activate the new conda environment:
```bash
conda activate blast
```

You should now be able to see the BLAST help page:
```bash
# BLASTN is nucleotide BLAST, compared to BLASTP, for example, which is for proteins:
blastn -help
```

### Download some example data
Now, let's get ready to use BLAST practically by downloading two example FastA files containing 16S rRNA gene sequences.

```bash
# Create an analysis directory and go there
mkdir -p "${HOME}/analysis/2020_09_22_blast_test"
cd "${HOME}/analysis/2020_09_22_blast_test"

# Query sequences
wget https://raw.githubusercontent.com/neufeld/lab-server-training/master/tutorials/conda/query_sequence.fasta

# Subject sequences
wget https://raw.githubusercontent.com/neufeld/lab-server-training/master/tutorials/conda/subject_sequences.fasta
```

Note that FastA files are just text files. You can view them using some built-in text editors on the server. For example, try:
```bash
# Print the whole file to the screen
cat query_sequences.fasta

# Open an interactive text editor to see the file. You can exit afterwards by pressing control + x.
nano query_sequences.fasta
```

### Run the example BLAST command

It's pretty simple to run jobs on command line. With your BLAST conda environment turned on, run:
```bash
blastn -query query_sequence.fasta -subject subject_sequences.fasta
```
A bunch of results should be printed to the screen.


Let's break down the above command:
- `blastn`: the name of the program that was installed via conda
- `-query`: this is called a *flag*. It is basically a way to set a parameter on a software tool run on the server. In this case, if you looked at the help file for BLAST, you'd see that this flag is used to specify the filename of the query FASTA sequence
- `query_sequence.fasta`: this is the filename of the query FASTA file on the server
- `-subject`: another flag
- `subject_sequences.fasta`: another file

There are lots of other flags you can set for a BLAST search as well, which you can see via `blastn -help`.

For example, we can make the e-value threshold of the search more strict (e.g., e < 10^-100) using the `-evalue` flag:
```bash
blastn -query query_sequence.fasta -subject subject_sequences.fasta -evalue 1e-100
```

We can also change the output format of BLAST:
```bash
blastn -query query_sequence.fasta -subject subject_sequences.fasta -evalue 1e-100 -outfmt "6 qseqid sseqid pident evalue qcovhsp bitscore"
```

You can direct the output to a file using `>`:
```bash
blastn -query query_sequence.fasta -subject subject_sequences.fasta -evalue 1e-100 -outfmt "6 qseqid sseqid pident evalue qcovhsp bitscore" > blastn_results.tsv
```

## Wrap-up
That's your basic tutorial for using conda and BLAST! In summary (and outlook):
- Now that miniconda is installed on your server account, you can easily install software tools for bioinformatics purposes
- If you want to install a new tool on the server, always check first to see if it has a conda install option (more and more tools do these days). It will save you lots of time.
- Running programs on the server is fairly simple. Type the program name followed by flags and/or arguments, then hit enter. The help page for each software tools will give you guidance on how to use it.
