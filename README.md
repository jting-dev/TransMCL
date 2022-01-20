# TransMCL 

TransMCL is a tool which enables reference genome-free reconstruction of full-length transcriptome invovled in phylogenetic reasearch. It utilizes the genome from the closely relative species to guide the assembly of the transcripts derived from Trinity. Then it further processes the assembled transcripts to eliminate the redudant transcripts by invoking IsoSVM tool to automated separate the isoforms from paralogs based on alignment of protein sequences.

Please direct bug reports and feature suggestions to xxxx

## Requirements

- Perl
- Python 

Make sure these sequence alignment softwares have been installed in your environment. 

 - muscle
 - diamond
 - Numpy
 - Scipy

## installation instructions

**install and configure the IsoSVM**

1. The main Perl scripts invoked in the package is 'isosvm.pl' and 'isosvm_wrapper.pl' resided in isoSVM home directory. Add the path of isoSVM directory to your $PATH so that the main Perl scripts can be automatically when run from a shell or invoked in package. (for example, if you put both IsoSVM scripts in "/home/user/bin/isosvm/" you can add that path to $PATH by:
   `export PATH="$PATH:/home/user/bin/isosvm"  # for users of BASH`
2. Add the path of corresponding subdirectory IsoSVM to your $PATH so that the Perl scripts "svm_learn.pl" and "svm_classify.pl" can be automatically when run from a shell or invoked in package.

3. Configure the IsoSVM modules. the isoSVM scripts need a couple of modules:

   - IFG::Alignments
   - IFG:NutsnBolts
   - IsoSVM:Evaluate
   - IsoSVM::GetFeatures
   - IsoSVM::SVMLight

   All theses modules reside in the corresponding subdirectories of the IsoSVM home directory. Adjust the corresponding path in both IsoSVM scripts "isosvm.pl" and "isosvm_wrapper.pl" in line 7. for example, change the line 

   `use lib "<put IsoSVM module installation path here>"`;

   to

   `use lib "/home/usr/bin/isosvm"`;

4. Install the SVM model. If you like it to be found automatically by IsoSVM, create a directory named ".isosvm" in **your home directory** and copy the model "isosvm.model" there. IsoSVM will also find the model automatically if "isosvm.pl" is called from the same directory where ".isosvm" is located.

**install TransMCL package**

`pip3 install TransMCL  ` 

## Usage 

Detailed usage instructions can be displayed with the swith '-h' or "--help" as an option after the command:

`python3 -m TransMCL`

the output will be like this:

`Usage: python3 -m TransMCL [option] [parameter]
    --abc       input mcl abc file
    --info      input mcl abc file
    --alignment input all-to-all blast file
    --fasta     input transcriptome file(.pep)
    --group     input mcl group file
    --length    input gene length file
    --species   input gene species file
    --transcriptome input transcriptome list
    --tree      input tree file(.nwk)
    --threads   threads
    --out       output file name`

The necessary options are as follows:

--abc <file> : the output file with the ".abc" suffix from PhyloMCL clustering results 

--info <file> : the output file with the ".info" suffix from PhyloMCL clustering results 

--alignment <file>: the all-to-all alignment file by diamond 

--fasta <file> : the fasta file of the transcriptome derived by Trinity or other de-novo transcriptome assembly tools

--group <file> : the hierarchical orthogroups clustered by PhyloMCL

--length <file> : the gene length file in the format "gene_name	gene_lenth"

--species <file> : the gene species file in the format "gene_name	species"

--transcriptome <file> : the transcriptome name list

--tree <file> : the newick file 

--threads : set the number of threads (Default:1) 

--out <directory> : set the output directory name



## Output

TransMCL has the following output files:

