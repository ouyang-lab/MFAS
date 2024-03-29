# Examples
All sample datasets can be downloaded from the [Downloads](Downloads.md) page.
## Sample input

There are three required inputs for MFAS: the feature file, the annotation file and the ID file.

### 1. Feature file: example_features.bed

Feature file is required in bed6 format. An eCLIP-seq sample input is as below:
|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
| chr1	| 14943	| 14992	| .  | 5  | - |
| chr1	| 16248	| 16300	| .   | 7   | - |
| chr1	| 16427	| 16479	| .   | 12  | - |
| chr1	| 17461	| 17525	| .   | 16  | - |
| chr1	| 109164	| 109224	| .   | 11  | - |
| chr1	| 109617	| 109656	| .   | 5   | - |

The columns are: chromosome, start, end, ID, score, and strand. The fourth column can be filled with any character as a place holder if there's no specific IDs. The fifth column shows the score of the feature, and we call it as 'bed-like' score when it stands for the total read counts in that region and 'bedgraph-like' score when it stands for the read coverage of every position within that region. The sixth column should be the strand information.

### 2.Annotation file: example_annotation.bed

Annotation file is required in bed12 format. A Refseq annotation sample input is as below:
|   |   |   |   |   |   |   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|chr1	|66999638	|67216822	|NM_001350218	|0   |+   |67000041	|67208778	|0   |23  |413,64,25,57,55,176,12,12,25,52,86,93,75,128,127,60,112,156,133,203,65,165,8067,    |0,91891,99114,105821,108854,109588,126557,133574,137039,137988,139325,143048,145722,155192,156234,161478,185338,195308,199792,205379,206702,207316,209117,
|chr1	|66999638	|67216822	|NM_001350217	|0   |+   |67000041	|67208778	|0   |25  |413,64,25,84,57,55,176,12,12,25,52,86,93,75,501,128,127,60,112,156,133,203,65,165,8067, |0,91891,99114,100124,105821,108854,109588,126557,133574,137039,137988,139325,143048,145722,147913,155192,156234,161478,185338,195308,199792,205379,206702,207316,209117,
|chr1	|66999251	|67216822	|NM_001308203	|0   |+   |67000041	|67208778	|0   |22  |104,123,64,25,57,55,176,25,52,86,93,75,128,127,66,112,156,133,203,65,165,8067,  |0,677,92278,99501,106208,109241,109975,137426,138375,139712,143435,146109,155579,156621,160870,185725,195695,200179,205766,207089,207703,209504,

This example is a standard format of bed12 file for transcripts' annotations in hg19.


### 3. ID file: example_ID.txt

ID file is required in plain text format with two columns. A Refseq annotation sample input is as below:
|   |   | 
| --- | --- |
NM_001350218	|SGIP1
NM_001350217	|SGIP1
NM_001308203	|SGIP1
NM_032291	|SGIP1
NR_136623	|AGBL4
NM_032785	|AGBL4

The first column of this ID file should be the same as the 4th column of the annotation file.

## Sample commands and output

There are 6 groups of meta-feature analysis (defined by option -w) 1 -- Meta-transcript, 2 -- Meta-exon, 3 -- Meta-intron, 4 -- Meta-con_introns, 5 -- Meta-gene, 6 -- Meta customized region.
For each group of analysis, there are two classes of coordinate system (defined by option -x) 1 -- norm, 2 -- abs.
For any kind of meta-feature analysis, there are two types of analysis can be applied (defined by option -a) 1 -- sense strand or 2 -- anti-sense strand.
Therefore, MFAS provides 24 types of meta-feature analysis given customized inputs. Here, we give two examples of the usage.

### 1. Sense-strand Meta-transcript analysis on the normalized length scale.

Command:
    python MFAS.py -b sample_annotation.bed -i sample_feature.bed -t sample_ID.txt -w 1 -x norm -n 50 -f bedlike -a 0 -p 1 -s 100

This line of command performs the meta-transcript analysis (-w 1) on the normalized length scale (-x norm). It uses 50 bins (-n 50) to normalize 5'UTR, CDS and 3'UTR and align all the transcript together by four reference sites: transcription start site, CDS start site, CDS end site, and polyA site. The iput bed6 file is bedlike file (-f bedlike) and no anti-sense analysis is performed but showing the visualization (-p 1) of top 100 transcripts (-s 100) in heatmap.

Output files:
#### (a) example_features_trans.txt

This is base-level feature for all the transcripts.
|   |   |    |   |  |
| --- | --- | --- | --- | --- |
NM_183004	|EIF5    |533	|1829	|0.0,0.0,0.340909090909,0.446808510638,0.0,......,0.0,0.31914893617

There are five columns of this output file. They are transcript ID, gene ID, CDS start site and CDS end site on the transcript coordinate, as well as the feature profile in single-nucleotide resolution separated by comma.

#### (b) example_features_trans_meta_150_bins.txt

This is the averaged meta-feature file with 150 bins in total across all transcripts.

2.755445623027614772e-03
1.435490186165403782e-02
2.409420678601847249e-02
1.290208072510331053e-02
...
2.025815441943625006e-02
4.447263826433938555e-02
2.625942152411957914e-02
1.249974787587827298e-02

This is a single-column file with 150 rows, which shows the final meta-feature of the dataset.

#### (c) example_features_trans_meta_150_bins_individual.txt

This is the features in 150 bins for individual transcript.
|   |   |
| --- | --- |
RNF14	|0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.222222222222  0.25    0.2     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.125331564987  0.173076923077  0.0135746606335 0.0     0.11375 0.151666666666  0.0825    0.012962962963  0.12962962963   0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0
UBE2Q1	|0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.153846153846  0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.152173913043  0.128762541806  0.0     0.0     0.0     0.0     0.162721893491  0.0384615384616 0.0     0.128205128205  0.166666666667  0.0666666666668 0.0     0.0     0.150063051702  0.27868852459   0.256393442623  0.0     0.301538461538  0.362119515965  0.324324324324  0.0518918918918 0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.157142857142  0.0480769230768 0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0685714285715 0.114285714286  0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0     0.0    0.0     0.0     0.0     0.0     0.0     0.0     0.0

#### (d) example_features_trans_150_bins_individual_heatmap.pdf

This is the heatmap for top 100 transcripts ranked by the sum of score.
![alt text](trans_meta_150_bins_heatmap.png)

#### (e) example_features_trans_150_bins.pdf

This is the line chart for all transcripts.
![alt text](trans_meta_150_bins_line.png)

### 2. Sense-strand Meta-transcript analysis on the absolute length scale.

Command:
    python MFAS.py -b sample_annotation.bed -i sample_feature.bed -t sample_ID.txt -w 1 -x abs -d 100,100,100,100,100,100,100,100 -f bedlike -a 0 -p 1 -s 100

This line of command performs the meta-transcript analysis (-w 1) on the absolute length scale (-x abs). It uses 100 bps as the largest distance included: downstream to the TSS, upstrem to the CDS start, downstream to the CDS start, upstrem to the CDS center, downstream to the CDS center, upstream to the CDS end, downstream to the CDS end, and upstream to the polyA site.  Interested regions defined by the distance list are aligned together across all the transcript together. The iput bed6 file is bedlike file (-f bedlike) and no anti-sense analysis is performed but showing the visualization (-p 1) of top 100 transcripts (-s 100) in heatmap.

Output files:
#### (a) example_features_trans.txt
This file is extactly same as that of the First example above.

#### (b) example_features_trans_100_100_100_100_100_100_100_100_bps_individual_heatmap.pdf

This is the heatmap for top 100 transcripts ranked by the sum of score.
![alt text](trans_100_100_100_100_100_100_100_100_bps_individual_heatmap.png)


#### (c) example_features_trans_meta_100_100_100_100_100_100_100_100_bps_line.pdf

This is the line chart for all transcripts.
![alt text](trans_meta_100_100_100_100_100_100_100_100_bps_line.png)


