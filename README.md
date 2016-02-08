## CrossHub

CrossHub is a Python tool allowing multi-way analysis of The Cancer Genome Atlas (TCGA) data:
•	Gene expression (RNA-Seq)
•	miRNA expression (miRNA-Seq)
•	Methylation profiling (Illumina Infinium HumanMethylation450 BeadChips)
•	Prediction of regulatory microRNA based on gene-miRNA co-expression analysis and miRNA target prediction algorithms (TargetScan, DIANA microT, etc.)
•	Prediction of regulatory transcription factors (TF) based on gene-TF co-expression analysis and ENCODE ChIP-Seq data.
•	Gene expression – methylation correlation analysis for CpGs located in promoter and enhancer regions
  CrossHub generates pretty Excel workbooks with the results.

## Quick start

### Prerequisites
It is recommended to install Anaconda Python3 since it includes all the needed packages, http://continuum.io/downloads#py35

Otherwise, you can install these packages separately:

	sudo apt-get install python3 python3-dev python3-scipy python3-xlsxwriter python3-pyparsing python3-matplotlib python3-pandas python3-psutil python3-setuptools python3-pip
	sudo easy_install3 -U statsmodels

Download and unpack one of following CrossHub releases:

CrossHub.1.2.2.plus.db.tar.gz (~800 Mb)

CrossHub.1.2.2.plus.db.TCGA.example.tar.gz (~4 Gb)

The second one includes an example of TCGA colon adenocarcinoma (COAD) path-to-data including RNA-Seq, miRNA-Seq and methylation profiling data.


### Download TCGA data
The easiest way to obtain TCGA data is to use DownloadData.py script. This allows easy downloading regularly updated TCGA data from our server

	python3 DownloadData.py

<br>
Otherwise, visit TCGA Data Portal https://tcga-data.nci.nih.gov/tcga/dataAccessMatrix.htm and download data manually.
The example of colon adenocarcinoma (COAD) TCGA data files are provided within CrossHub.*.plus.db.TCGA.example.tar.gz


### Run CrossHub

We have provided several scripts for typical tasks (gene expression, methylation etc.).

General script syntax looks like:

	bash task.sh TCGA.data.folder gene1,gene2,gene3 output-folder

Gene range may be defined with Gene Ontology keywords. This example describes the analysis of differential expression for the genes responsible for apoptosis and some reference genes:

	bash ./RNA-Seq.diff.exp.sh ./COAD [apoptosis],GAPDH,ACTB ./output-folder


Here is the complete list of scripts:

•	Complete analysis - differential expression, co-expression with ENCODE TF + ChIP-Seq, co-expression with miRNA + target prediction databases, methylation profiling + ENCODE genome segmentation:

	bash Complete.Analysis.sh path-to-data genes output-folder
	
•	Gene differential expression profiling
	
	bash RNA-Seq.diff.exp.sh path-to-data genes output-folder

•	Expression correlation between miRNA and genes:

	bash miRNA-Seq-RNA-Seq.co-expr.sh path-to-data genes output-folder
	
In order to save memory, you can launch the analysis without miRNA target prediction databases:
	
	bash miRNA-Seq-RNA-Seq.co-expr.without.DB.sh path-to-data genes output-folder
	
•	Methylation profiling of the genes involved in apoptosis with description of chromatin state of CpG sites according to ENCODE Chromatin State Segmentation database (promoter, enhancer, polycomb repressed etc.). Methylation-expression correlation analysis will be also performed.

	bash Methylation.and.RNA-Seq.sh path-to-data genes output-folder
	
  In order to reduce RAM usage one can run this analysis without ENCODE Chromatin State Segmentation DB:

	bash Methylation.and.RNA-Seq.without.ENCODE.sh path-to-data genes output-folder

•	Expression correlation between genes and transcription factors studied in the ENCODE project. This analysis includes highlighting gene-TF interactions found by ENCODE ChIP-Seq: 

	bash RNA-Seq.co-expr.with.ENCODE.TF.sh path-to-data genes output-folder

•	Expression correlation between genes and transcription factors, cofactors and genes of chromatin remodeling complexes. This analysis includes highlighting gene-TF interactions found by ENCODE ChIP-Seq: 
	
	bash RNA-Seq.co-expr.TF.ChrRem.Cofactors.plus.ENCODE.ChIP-Seq.sh path-to-data genes output-folder
	
  The same analysis supplemented with Jaspar predictions: 

	bash RNA-Seq.co-expr.TF.ChrRem.Cofactors.plus.ENCODE.ChIP-Seq.Jaspar.sh path-to-data genes output-folder
	
  Co-expression analysis without Jaspar and ENCODE ChIP-Seq, to save maximum the memory:
	
	bash RNA-Seq.co-expr.TF.ChrRem.Cofactors.without.DB.sh path-to-data genes output-folder
	
•	Differential expression of miRNA:

	bash miRNA-Seq.diff.exp.sh path-to-data output-folder
	
	

You can use complex queries to define gene range to analyze. In the following example, we reveal genes with GO annotation matching the pattern 'apopto*', e.g. apoptosis, apoptotic, apoptosome:

	bash ./RNA-Seq.diff.exp.sh ./COAD [apopto*]
	
You can use brackets and logical operators 'or', 'and'. This example describes the selection of apoptosis-related and glycolytic process-related genes, but not encoding extracellular proteins:

	bash ./RNA-Seq.diff.exp.sh ./COAD "[(apoptosis or glycolytic) -extracellular]"
	
Also you can use expression-matching:

	bash RNA-Seq.diff.exp.sh ./COAD "['negative regulation of glycolytic process']"


#### Citation

If you have found CrossHub useful, please cite

Krasnov G.S., Dmitriev A.A. et al. CrossHub: a tool for multi-way analysis of The Cancer Genome Atlas (TCGA) in the context of gene expression regulation mechanisms. Nucleic Acids Res. 2016 Jan 14. pii: gkv1478. PMID 26773058
