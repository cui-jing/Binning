Binning
=======

Scripts required to calculate tetramer frequencies and create input files for ESOM.<br>
See: Dick, G.J., A. Andersson, B.J. Baker, S.S. Simmons, B.C. Thomas, A.P. Yelton, and J.F. Banfield (2009).  Community-wide analysis of microbial genome sequence signatures. Genome Biology, 10: R85How to ESOM

How to ESOM?
============

These instructions are for ESOM-based for binning: see http://databionic-esom.sourceforge.net/ for software download and manual.

1.	Generate input files.
-------------------------
	1.	Use the "esomWrapper.pl" script to create the relevant input files for ESOM. In order to run this script, you'll need to have all your sequences(in fasta format) files with the same extension in the same folder. For example:
	perl esomWrapper.pl -path fasta_folder -ext fa
	
	For more help and examples, type:
	perl esomWrapper.pl -h
	
	2.	The script will use the fasta file to produce three tab-delimited files that ESOM requires: 
		Learn file = a table of tetranucleotide frequencies (.lrn)
		Names file = a list of the names of each contig (.names)
		Class file = a list of the class of each contig, which can be used to color data points, etc. ( .cls)
		**class number: The esom mapping requires that you define your sequences as classes. We generally define all the sequences that belong to your query (meatgenome for example) as 0 and all the others 1, 2 and so on. think of these as your predefined bins, each sequence that has the same class number will be assigned the same color in the map.

	3. These files are generated with Anders Anderssons perl script "tetramer_freqs _esom.pl" which needs to be in the same folder as the "esomWrapper.pl". To see how to use the "tetramer_freqs _esom.pl" independent of the wrapper, type:
	perl tetramer_freqs _esom.pl -h

2.	Run ESOM
------------
	1. on you termial, run w/ following command from anywhere (X11 must be enabled):
		./esomana

	2. load .lrn, .names, and .cls files (File > load .lrn etc.)
	3. Normalize the data if desired: under data tab, see Z-transform, RobustZT, or To[0,1] as described in the users manual.  I find that this RobustZT makes the map look cleaner although a similar result is obtained no matter what preprocessing is done (even none).
	4. Train the data:

	5. Tools > Training
		- Parameters: use default parameters with the following exceptions.  Note this is what seems work best for AMD datasets but the complete parameter space has not been fully optimized.  David Soergel (Brenner Lab) is working on this:
		- Training algorithm: K-batch
		- Number of rows/columns in map: I use ~5-6 times more neurons than there are data points.  E.g. for 12000 data points (window, NOT contigs) I use 200 rows x 328 columns (~65600 neurons).
		- Start value for radius = 50 (increase/decrease for smaller/larger maps). 
		- I've never seen a benefit to training for more than 20 epochs for the AMD data.
		- Hit 'START' -- training will take 10 minutes to many hours depending on the size of the data set and parameters used.

	6. Analyzing the output:
		- Best viewed (see VIEW tab) with UMatrix background, tiled display.  Use Zoom, Color, Bestmatch size to get desired view.  Also viewing without data points drawn (uncheck "Draw bestmatches") helps to see the underlying data structure.
		- Use CLASSES tab to rename and recolor classes.
		- To select a region of the map, go to DATA tab then draw a shape with mouse (holding left click), close it with right click.  Data points will be selected and displayed in DATA tab.
		- To assign data points to bins, use the CLASS MASK tab to draw a class mask (e.g. using the data structure as a guide -- see also "contours" box in VIEW tab which might help to delineate bins) then > Tools > Classify.  This will assign each data point to a class (bin).  The new .cls file can be saved for further analysis (along with .names file which has the window IDs).
		Note: to load a saved project: File > load .wts
