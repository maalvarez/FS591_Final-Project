# Final Project (FS - 591)

This is a draft of the pipeline used to obtain data and process it using the different programs we learned in class. 

# Data processing protocol for Final Project - FS 590

## Get the .fna (genome assembly sequenece) from [NCBI_assembly](https://www.ncbi.nlm.nih.gov/assembly/)

-Search for your organism.

-Press the "Download Assembly" button. 

-Select "GeneBank" from dropdown menu in Source database (GenBank or RefSeq) and "Genomic fasta (.fna)" from dropdown menu in File type. 

-Press the Download button.

## Submit the .fna file to [RAST](https://rast.nmpdr.org). 

Let us know if you will need an account to submit your file. 

-Use the following options when prompted:
  * Use NCBI's Taxonomy ID to fillout all the data of the organism
  * Rasttk
  * Automatic error option checked 
  * Automatic frameshift unchecked

-Wait until the job is completed. It may take 1 - 2 days for the job to be completed. 

-Once the job is completed, download the "Aminoacid-Fasta file" and "Spreadsheet (Excel XLS format)" from the dropdown menu of "Available downloads for this job"

## Working in the cluster

-Create a folder in /peptide/SANGER with the name of your organism with `mkdir <organism name>`. 

-Copy the aminoacid fasta file to the folder you created with 

```
scp <fasta file> <username>@scholar.rcac.purdue.edu:/depot/lindems-class/peptide/SANGER/<organism name>
```

-Run TIGRFam

```
hmmsearch –o <organism name>.YOUR_INITIALS.TIGR –-tblout <organism name>.YOUR_INITIALS.TIGR.tsv --cut_tc ../../../data/TIGRFAMs_14.0_HMM.LIB <your RAST output amino acid fasta file>
```

-Run PFamm

```
hmmsearch –o <organism name>.YOUR_INITIALS.Pfam –-tblout <organism name>.YOUR_INITIALS.Pfam.tsv--cut_tc ../../../data/Pfam-A.hmm <your RAST output amino acid fasta file
```

-Run CAZy

```
hmmscan --domtblout <organism name>.YOUR_INITIALS.CAZy.out.dm ../../../data/dbCAN-HMMdb-V9.txt <your_RAST_output_amino_acid_fasta_file > <organism name>.YOUR_INITIALS.CAZy.out
```

```
sh ../../../data/hmmscan-parser.sh <organism name>.YOUR_INITIALS.CAZy.out.dm > <organism name>.YOUR_INITIALS.CAZy.out.dm.ps
```

```
cat <organism name>.YOUR_INITIALS.CAZy.out.dm.ps | awk '$5<1e-15&&$10>0.35' > <organism name>.YOUR_INITIALS.CAZy.out.dm.ps.stringent
```

***This code assumes that you are located in your organism folder in the cluster, have the amino acid file in this folder, started interactive mode, and loaded the required modules: bioinfo and HMMER. (You can use the script file "Final_project.sb" to run CAZy commands as a batch. Run TIGRFam and PFam one by one, it seems there is an issue when trying to run them in a batch file). If the batch file does not work for you, run the commands one by one and double check the names you are using.***

-Use `scp` to copy the results of TIGRFam (.tsv), PFamm (.tsv), and CAZy (.stringent) to your computer.

## Running BLASTKoala

-Submit your aminoacid file downloaded from RAST to [BlastKOALA](https://www.kegg.jp/blastkoala/)

-Use the following options:

 * Bacteria
 * genus_prokaryotes

It may take at least 1 day for the job to be completed after submission. 

-After receiving the email with the link to the results of your job, go to "view" and click in "Download detail". This will show you more info than the other option: Query, KO, Definition, Score, and Second best

## Gathering the data and data organization

-At this point you should have the following files in your computer:

 * Spreadsheet (Excel XLS format) from RAST
 * TIGRFam (.tsv) results
 * PFamm (.tsv) results
 * CAZy (.stringent) results
 * BlastKoala results
 * TIGRFam Info. You can donwload this file :[TIGRFAMs_improvements_2018-10-11.tsv](https://ftp.ncbi.nlm.nih.gov/hmm/TIGRFAMs/TIGRFAMs_improvements_2018-10-11.tsv)

-In the excel file downloaded from RAST, add more columns and copy the results of each analysis usin the function `vlookup`, as showed in class.

-Once you have all of the results organized in columns, save your file as .csv and copy it to your folder in the cluster: /depot/lindems-class/peptide/SANGER/organism_name

## Strategy for anotation

TBD
