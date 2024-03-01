AmpliconSeq

All files in this repo must be located in ~/Desktop/Live_analysis.
Your active directory must be the folder, where incoming .fastq files are saved.
Start 'Live.sh'

1. 	'Live.sh'
		1. 	'Live.sh' will create a directory /Analyzed, where all previously analyzed .fastq files are moved to prevent repeated analysis.

   		2. 	Directories to store the results for each analyzed marker are created. (If directories from previous analyses are present, they will be deleted first).
		3. 	'plot_markers.py' is executed (this enables a continuous graphical presentation of variant allele frequency during analysis).
		4. 	While 'plot_markers' is active, the current directory is searched for .fastq files. If .fastq files are found the function 'check_markers' iterates through all .fastq files. 
		5. 	'check_markers' executes 6 shell scripts with the active .fastq file as input ('IDH1_variants...sh','IDH2_variants...sh','pTERT_variants...sh','H3F3A_variants...sh','HIST1H3B_variants...sh' and 'BRAF_variants...sh') 
			afterwards it moves the analyzed .fastq file to the '/Analyzed' directory.
		6.	When every .fastq file is processed and the while loop is active because 'plot_markers.py' is running, newly generated .fastq-files will be processed.

3.	Example IDH1: 'IDH1_variants...sh' 
		1. 	Each sequence in a given .fastq file is mapped to a .fasta file of the amplified IDH1 target using 'minimap2'. In this .fasta file the nucleotides of interest are deleted as indicated by the filename.
		2.	The alignment results are saved as 'alignment_(PreviousFilename).sam' in the previously created directory for IDH1 results.
		3.	Afterwards it is converted to .paf format using 'PAFtools.js' and sorted with sort -k6,6 -k8,8n.
		4.	Variants are called using 'PAFtools.js'. Results are sorted and stored as 'aln.var_(PreviousFilename).csv.srt.csv'.
		5.	The script 'IDH1_count_variants...py' is called.

4.	'IDH1_count_variants...py'
		1.	'aln.var_(PreviousFilename).csv.srt.csv' comprises all mismatches between sequences of one .fastq file and the reference they were mapped to. 
			Due to the removed bases at the point of interest, an insertion mutation should be present in sequences carrying the SNV site.
		2.	The recorded insertion mutations are tallied and categorized into Wt or (one or several) Variants.
		3.	'IDH1_graph.csv' is read and existing data is added to the tallied results.
		4.	The created sum is appended to 'IDH1_graph.csv'. 

5.	'plot_markers.py'
		1.	'IDH1_graph.csv' is read and the data in each row is used for calculation of variant allele frequency at each given timepoint (represented by each analyzed .fastq file in this case)
		2.	The calculated data is visualized with graphs. The X-Axis represents analyzed .fastq files. Y-Axis represents variant allel frequency. 
		3.	As more data is appended to the 'IDH1_graph.csv' during analysis, the drawn graphs are updated.
		4.	Upon closing 'plot_markers.py', the drawn graphs are saved.

This example is concerning IDH1. During analysis all six markers are evaluated in the same manner.
A 'Live_HiRes.sh' variation of the scripts exist. They increase the temporal resolution from .fastq file, a batch of several sequences, to single sequence level. 
