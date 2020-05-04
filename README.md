# Genetic Data 4 Us
## Introduction

This project was done for an assignment for VCU's EGRB 301 Engineering Design Practicuum class. 

Genetics can be a scary field to go into. Looking at a whole genome can be scary for the untrained eye, as it just looks like a bunch of letters randomly put together, when in reality each letter has a purpose to the organism it comes from, and if a single letter is changed, it could spell death for that organism. There exist tools today that can analyze full genomes, but all data is not created equal, and all genome sequencing sources do not output their data the same way. This adds to the difficulty of analyzing genomics, as not only do you have to find changes, you have to make the data comparable. With the rise of COVID-19, it is of upmost importance that we can analyze the genomics data quickly and accurately. Therefore, having a similar standard for analyzing genomics is necessary

Genetics Data 4 Us is a program solution that takes in genome data to compare against a template, and outputs all the mutations present for that strain in an easy-to-read JSON file, which can be integrated into almost any language and subsequently analyzed (including programs like MATLAB). This program will be made using the wide variety of genome data present in the [Next Strain data repository](https://github.com/nextstrain) full of genomic data from various viruses, such as dengue, measles, mumps, and the current COVID-19 virus. Even the data present on this website is not free of irregularities, and my goal for this project is to make this program work for this repository of data. 

## Final Process

1. Download the desired genome data and place it all in a single directory
2. Download the necessary genomic analysis tools from the NextStrain website. The preferred method is with a local installation, and instructions on how to do that can be found [here](https://nextstrain.org/docs/getting-started/local-installation). The tools require conda and Python. It is easier to use on macOS or on Linux, but can by done on Windows using a Linux subsystem
3. Download the necessary Python scripts from this project repository. They are found in the Scripts folder
4. Right now, this process only works with FASTA files. If your files are in that format, continue
5. In the FASTA file, go through the information present in the header line before the genome. Edit makeMeta.py by adding a function that parses the header line and obtains the necessary information from the header. The necessary information is in the header variable in the script
6. Run makeMeta.py. This will output Metadata files in .tsv format
7. Run makeNewFasta.py. This will output genomic files in .fasta format
8. Move all new FASTA files and metadata files to a new directory. In this new directory, make a config directory where all the references will be placed
9. Make sure that there are four files in total for each disease you want to analyze: 
   - A new FASTA file made from makeNewFasta.py. These have the convention "-virus name-_new.fasta"
   - A new Metadata file made from makeMeta.py. These have the convention "-virus name-_meta.tsv" 
   - A reference file in the config folder. These are taken from the raw NextStrain download, and have the convention "-virus name-_reference.gb"
   - A text document with excluded strains. These are text files that have the case names of cases that you want excluded from analysis, and have the convention "-virus name-_dropped_strains.txt". They are also placed in the config directory
10. Run runAugur.py. This step may take a while depending on the number of cases in the FASTA file
11. Edit ParseVirusData.py to include the analyzed virus names
12. Run ParseVirusData.py. This step will make the final JSON files with all the necessary information
  
## Final JSON Format

The final JSON format will have the following form: 

```
<case name>: {"virus": <virus name>, "date": <date recorded>, "region": <geographic region>, "country": <country recorded>, "aa_muts": <list with all amino acid mutations>, "nt_muts": <list with all nucleotide mutations>}
```

The final JSON will be a sigle object containing all the cases for a specific disease. 

The amino acid list will be organized by protein (E for envelope, S for spike, etc. Check the results folder and the aa_muts JSON file to see what protein each letter refers to) in the format of "-original amino acid--mutation position--new amino acid-" (ex: A234K)

The nucleotide list will be just a plain list containing all the individual mutations in the format of "-original nucleotide--mutation position--new nucleotide-" (ex: T273G)

## Repository Organization

* __Raw Data__ - Contains all the raw data downloaded from the NextStrain repository. It can also be downloaded directly from their repository
* __Pre-processed Data__ - Contains all the edited data before any steps were performed. This data will need to be reformatted using steps 6 and 7 above
* __Processed Data__ - Contains all the edited data after steps 6 and 7 were completed. This is here to check and see if you data was reformatted correctly, and shows the format your directory should be before steps 10 and 11 are to be completed
* __Analyzed Data__ - Contains the final data processed after all the steps above are completed. This directory has three separate directories: 
  - __results__ - Each strain has its own results directory, with the files needed for the intermediary steps. This is also where you can find the aa_muts.json and nt_muts.json files for reference if needed
  - __final__ - Contains the final JSON file for each disease analyzed
  - __data__ - Contains the data that match the Processed Data folder
* __Scripts__ - Contain all the scripts mentioned in the procedure above.
