# Instructions
## Prerequisites
## 1. Prepare the data files.

There are three required inputs for MFAS: the feature file, the annotation file and the ID file. The feature file contains the features to be analyzed formatted in bed6. The annotation file is the gene model in the bed12 format. The ID file includes the ID mapping between the transcripts and the genes.
Please refer to [Examples](Examples.md) for the detailed format.

## 2. Download and install necessary tools.
### a. Python3 
Python can be downloaded from: https://www.python.org/downloads/
Required python packages: Numpy, Scipy, Pandas, and Seaborn. These modules can be installed via pip command.
### b. BedTools
The installation of BEDTools can be found at: https://bedtools.readthedocs.io/en/latest/content/installation.html.

Please make a-b exceutable in your system. 


# Run MFAS
## 1. [Download the source code of MFAS.](Downloads.md) 
MFAS source code can be downloaded from the [Downloads](Downloads.md) page.
## 2. Check help page of MFAS for usage.
MFAS help page can be generated simply by typing:

python MFAS.py -h

in your command window. 

The basic usage of MFAS is as below:

usage: MFAS.py [-h] -b BED12 -i BED6 -t TAB [-l GL] [-w INDICATOR]
                          [-x XTYPE] [-n BIN_NUMBER] [-d D_LS] [-f FILETYPE]
                          [-a ANTI] [-p PLOT] [-s TOPN]

If you get any errors in this command line, please go back to the prerequisites and check.

## 3. Collect MFAS output files.
In the running process of MFAS, it will generate some temporary files. Therefore, please make sure you have the administrative rights of the system.
MFAS generated two files with suffix "_trans.txt" and "trans.txt.anti", which record the input features in the transcript coordinates for the sense strand and anti-sense strand of the annotation, respectively.
If the option of normalized distance is enabled, MFAS will output two files. One of them is the matrix with m rows and n columns, where m is the number of genes of interest, and n is the number of bins defined by the user. Another file is the averaged meta-feature vector with length n, where n is the number of bins. If the option of absolute distance is enabled, MFAS will also output two files. One of them is the matrix with m rows and n columns, where m is the number of genes of interest, and n is the sum of distance list defined by the user. Another file is the averaged meta-feature matrix with n rows and 3 columns, where n is the sum of distance list and the 3 columns are index, distance to reference site, and average score.
If the visualization module is enabled, MFAS will output one line chart and one heatmap for the input feature file. The line chart shows the average across all the interested genes (or regions), and the heatmap shows the individual profiles for the top s genes (regions) ranked by the sum of scores.
If the anti-sense option is enabled, MFAS will automatically output the files for the anti-sense strand of the interested genes (regions) in the same format as the sense strand profiles and visualizations.

Please refer to [Examples](Examples.md) for the detailed output format of MFAS.
