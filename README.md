# TransMCL

TransMCL is a tool which enables reference genome-free reconstruction of full-length transcriptome invovled in phylogenetic reasearch. It utilizes the genome from the closely relative species to guide the assembly of the transcripts derived from Trinity. Then it further processes the assembled transcripts to eliminate the redudant transcripts by invoking IsoSVM tool to automated separate the isoforms from paralogs based on alignment pattern of protein sequences.



## Requirements

we currently recommand using Python 3.8 from [http://www.python.org](http://www.python.org/) . TransMCL is currently supported and tested on the following Python implementations:

- Perl
- Python3.6, 3.7, 3.8 -- see [http://www.python.org](http://www.python.org/)
- pip3.6 --  see https://pypi.org/project/pip

Make sure these sequence alignment softwares have been installed in your environment. if not, you can install muscle and diamond by "conda install", and install Numpy and Scipy dependencies by " pip install".

 - muscle --see http://www.drive5.com/muscle/manual/install.html
 - diamond --see https://github.com/bbuchfink/diamond
 - Numpy --see https://numpy.org
 - Scipy --see https://scipy.org
 - phyloMCL(optional) --see https://sourceforge.net/projects/phylomcl/ 

## installation instructions

- **install and configure the IsoSVM**

1. download and unzip isoSVM.zip. The main Perl scripts invoked in the packages, 'isosvm.pl' and 'isosvm_wrapper.pl' are resided in this directory. Add the path of isoSVM directory to your $PATH so that the main Perl scripts can be automatically executed when run from a shell or invoked in package. (for example, if you put both IsoSVM scripts in "/home/user/bin/isosvm/" you can add that path to $PATH by:
   `export PATH="$PATH:/home/user/bin/isosvm"  # for users of BASH`

2. Add the path of corresponding subdirectory IsoSVM, which resided in isoSVM directory, to your $PATH so that the Perl scripts "svm_learn.pl" and "svm_classify.pl" can be automatically executed when run from a shell or invoked in package. You can add that path to $PATH by:

   `export path="PATH:/home/user/bin/isosvm/IsoSVM"  #for users of BASH`

3. Configure the IsoSVM modules. the isoSVM scripts need a couple of modules:

   - IFG::Alignments
   - IFG:NutsnBolts
   - IsoSVM:Evaluate
   - IsoSVM::GetFeatures
   - IsoSVM::SVMLight

   All theses modules reside in the subdirectories IFG and IsoSVM. Adjust the corresponding path in both IsoSVM scripts "**isosvm.pl**" **line 7** and **"isosvm_wrapper.pl"** **line 9**. In detail, change the line 

   `use lib "<put IsoSVM module installation path here>"`;

   to

   `use lib "/home/usr/bin/isosvm"`;

4. Install the SVM model. If you like it to be found automatically by IsoSVM, create a directory named **".isosvm"** in **your home directory** and copy the model "isosvm.model" there. IsoSVM will also find the model automatically if "isosvm.pl" is called from the same directory where ".isosvm" is located.

- **install TransMCL package**

`pip3 install TransMCL  ` 

​		or you can download "TransMCL.tar.gz" from this website and unzip it. Then add it to your python site-packages.

## Usage 

After the installation is successful, the main program command TransMCL could be installed in your python package directory. you can check whether TransMCL is installed successfully by running the following command:

`$ python3 -m TransMCL -h`

or

```
$ python3 -m TranMCL help
```

Detailed usage instructions can be displayed after the command:

```
Usage: python3 -m TransMCL [option] [parameter]

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
```



Here are the detailed description of this **neccessary parameters**:

| option          | parameter                       | Description                                                  |
| --------------- | ------------------------------- | ------------------------------------------------------------ |
| --alignment     | input all-to-all blast file     | the all-to-all alignment file by diamond                     |
| --abc           | input abc file (.abc)           | the output file with the ".abc" suffix from PhyloMCL clustering results |
| --info          | input info file (.info)         | the output file with the ".info" suffix from PhyloMCL clustering results |
| --fasta         | input transcriptome file (.pep) | the protein sequences of the transcriptome derived by Trinity or other de-novo transcriptome assembly tools (Oases, Soapdenovo-Trans, et.al) (.fasta/fa format) |
| --group         | input mcl group file            | the hierarchical orthogroups clustered by PhyloMCL           |
| --length        | input gene length file          | the gene length file in the format "gene_name	gene_lenth" |
| --species       | input gene species file         | the gene species file in the format "gene_name	species"   |
| --transcriptome | input transcriptome list file   | the transcriptome species name list                          |
| --MSA           | input MSA tool                  | assign the multi-sequence alignment tool (Mafft/Muscle. default: Mafft) |
| --tree          | input tree file (.nwk)          | the phylogenetic tree file of selected species (.nwk format) |
| --threads       | threads that to be used         | set number of threads (default:1)                            |
| --out           | Output directory name           | set the output directory name                                |





## Testing

The directory test_dataset stored in https://sourceforge.net/projects/transmcl1/files/test_dataset/ contain the necessary input datasets to help you test TransMCL. The file as following:

-  phymcl.input.tree

```
(((Ath_trinity,Alyrata),Ahalleri),(Esa_trinity,Brapa));
```

This is the phylogenetic tree file containing all the selected species **in Newick Format**. Among which the A.thaliana and E.salsugineum are transcriptome derived by Trinity, the rest are genome. The name of A.thaliana and E.salsugineum must be **labeled with suffix** such as "Ath_trinity" and "Esa_trinity".  



- Info_tra.txt	 

```
Ath_trinity
Esa_trinity
```

This file is to clarify the species of transcriptome to help the program identify the protein sequences to be assembled. It must be same in the phymcl.input.tree.

 

- Info_trinity.fa

```
>Ath_TRINITY_DN10000_c0_g1_i1.p1 Ath_TRINITY_DN10000_c0_g1~~Ath_TRINITY_DN10000_c0_g1_i1.p1  ORF type:complete len:472 (-),score=76.73 Ath_TRINITY_DN10000_c0_g1_i1:261-1676(-)
MTAKRAIGRHESLADKVHRHRGLLLVISIPIVLIALVLLLMPGTSTSVSVIEYTMKNHEG.......

>Esa_TRINITY_DN0_c0_g1_i1.p1 Esa_TRINITY_DN0_c0_g1~~Esa_TRINITY_DN0_c0_g1_i1.p1  ORF type:complete len:744 (+),score=113.40 Esa_TRINITY_DN0_c0_g1_i1:329-2233(+)
MGHAVHVTSAASGEITFWSRSAENLYHWYAEEVVGYRTIDLLVTEEYRNCLMSIRNRVCR
```

This file contains all the protein sequences of transcriptome for A.thaliana and E.salsugineum. **The gene id must have the prefix same as info_tra.txt**. otherwise the program will not recognize the transcriptome sequence.



- all_sample2fa.list

```
evm.model.supercontig_0.1       Cpapaya
.......
Ath_TRINITY_DN10000_c0_g1_i1.p1 Ath_trinity
.......
Esa_TRINITY_DN0_c0_g1_i1.p1     Esa_trinity
.......
```

This file contains two columns: the gene id and the species to which it belongs.



- all_sample.faa.length

```
evm.model.supercontig_0.1       62
......
Ath_TRINITY_DN10000_c0_g1_i1.p1 472
......
Esa_TRINITY_DN0_c0_g1_i1.p1     635
......
```

This file contains two columns: the gene id and the length of the protein sequence.



- all_blastp.out 

```
evm.model.supercontig_0.1       evm.model.supercontig_0.1       100.0   62      0       0       1       62      1       62      1.7e-27 117.9
.......
Ath_TRINITY_DN10000_c0_g1_i1.p1 evm.model.supercontig_676.2     60.3    468     103     3       4       470     89      474     3.3e-147        518.5
.......
```

This is all-to-all protein blast file outputted by diamond. The proteins from all the species (genome/transcriptome) are included.



- allmcl.out.info

```
315052  34641275
1       evm.model.supercontig_0.1
2       evm.model.supercontig_40.87
3       evm.model.supercontig_109.2
4       evm.model.supercontig_10.175
```

This first line of this file contains total number of all genes and total lines of blast file (all_blastp.out), and the below lines set up an id for each gene simply by numbers.



- allmcl.out.abc

```
1       1       117.9
1       2       57.4
1       3       47.0
1       4       44.7
1       5       44.3
1       6       43.9
1       7       41.6
1       8       40.8
2       1       56.6
2       2       391.7
.......
```

This file contains three columns: the query gene id, the subject gene id, and the bitscore value of this two gene. 

*NOTE: This file and the above allmcl.out.info can be automaticly output by PhyloMCL if you have already installed this software.*



You can run this command to help test TransMCL.

```
$ python3 -m TransMCL --abc allmcl.out.abc --info allmcl.out.info --alignment all_blastp.out --fasta info_trinity.fa --group allmcl.out.OGs.group --length all_sample.faa.length --species all_sample2fa.list --transcriptome info_tra.txt --tree phymcl.input.tree --threads 6 --out trans
```

If successful, you can obtain an output directory named as "trans_output".



## Output

The output directory -- "trans_output" contains two subdirectory "Isosvm_1st" and "Isosvm_2st" and extra files. Both the two file contains subdirectory "Ath_trinity" and "Esa_trinity".

- Isosvm_1st

This directory contain protein sequences proceessed by first-round isoSVM tool to filter isoforms.

- Isosvm_2st

This directory contain final protein sequences processed by second-round isoSVM tool filter isoforms. The "clean_fasta.txt" resided in "Ath_trinity" and "Esa_trinity" are the full-length non-redundant transcriptome we want. 



if you have any questions please contact "20110700125@fudan.edu.cn".
