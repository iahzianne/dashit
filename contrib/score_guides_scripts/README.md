# DASH score_guides analysis
`score_guides_wrapper.sh` illustrates a sample batch workflow for running score_guides to examine if a set of samples would undergo depletion by a given guide set or if a DASHed library contains any remaining material which should have been depleted. 

It performs subsampling, converting to fasta, and running DASHit score_guides on a directory of fastq.gz files downloaded from AWS. Recommend to run in tmux/screen.

The wrapper also will format your standard output txt file from score_guides using a Python script. 

### Installation
Copy the score_guides_wrapper.sh into the directory with the files you wish to perform the analysis on.

### Dependencies
seqtk, Python (script will import pandas), DASHit

### Running the script

Use bash to run the script and follow with 3 arguments
1. the AWS path to your files
2. the path to the DASH guide library you want use to score your files (must be in the format of the output of optimize_guides)
3. the prefix of your output file (outputfile)


```
bash score_guides_wrapper.sh s3://czbiohub-seqbot/fastqs/180907_A00111_0206_BH7W5WDSXX/rawdata/Amy_Lyden_AIH /mnt/data/specialops/DASH_Analysis/nribo2_150_V2.csv AIH_Plate_02_1xPCR_DASHed
```

### Your formatted output file
Your formatted CSV (outputfile.csv) will contain five columns
1. Guide library
2. Your filename
3. Total Reads DASHed (hit by scoreguides)
4. Total Reads in sample
5. Percent DASHed

### Other files generated
`outputfile.txt` is the redirected output from score_guides. Subsampled fastqs and fastas, as well as raw data files, are kept after running the wrapper.

### Running the Python script independently to format score_guides output
If you ran score_guides on a batch of files, and redirected output to a txt file (outputfile or outputfile.txt), you can run the Python scripts to reformat outputfile/outputfile.txt into a csv named outputfile.csv (see above for structure of formatted output file)

```
python DASH_csv_format.py outputfile.txt
```

A version of the script will also parse your filename by underscores to create new columns and prompt you if you would like to keep this information column by column. This will create a file which is identical to the formatted output file, but has additional columns defined by you.
 
All of your filenames must have the same number of variables in the same order, and separated by underscores (ex: sample_treatmentgrp_date.fastq.gz). Use DASH_csv_format.py if this is not true.

Run the following command on outputfile. Respond y/n depending on if you would like that column to be in your final file. If you respond y, you will be shown an example variable (ex: treatmentgrp) and prompted to enter a column name for the variable.

```
python DASH_csv_format_interactive.py outputfile.txt
```
