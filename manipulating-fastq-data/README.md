# Manipulting FASTQ data

## FASTQ basics

## FASTQ file manipulation using command line skills

Try to solve the below tasks using your Unix skills. Do not hesitate to consult Google for help!! Hints for every task can be found [here](). Possible solutions are suggested [here](). Enjoy!


1. Download the testdata from https://www.dropbox.com/s/wcanmej6z03yfmt/testdata_1.tar?dl=0. Create a directory (exercise_1) in your home directory and copy the archive testdata.tar from ~/Downloads/ to this directory.
2. Decompress the archive.
3. Examine the content of the data files. For basic information on fastq format please visit: http://en.wikipedia.org/wiki/FASTQ_format. With this information try to interpret the content of your data files. Do you know what all the lines represent? 
a) Which quality encoding (which offset) do these files use? 
b) What is the header of the third read in the file? Is this a single-end (forward) or a paired-end (reverse) read? 
c) What is the header of the last read in the file? Is this a forward or reverse read?
4. How many lines does the file have?
5. How many reads does the file contain?
6. How many single-end (“se” also referred to as forward reads) reads and how many paired-end (“pe” also reverse reads) reads does the file contain?
7. Extract all se/pe reads and write them to separate files testdata_1.fastq and testdata_2.fastq.
8. a) Count the number of reads that contain the sequence TGCACTAC in testdata_1.fastq. 
b) Count the number of reads that start with TGCACTAC (referred to as in-line barcode) in testdata_1.fastq.
9. Modify all headers in the file testdata_1.fastq. Replace the part of the header, which identifies the read as se by “/1” and write the data to testdata_1_newheader.fastq.
10. Extract the first 1000 reads from testdata_1_newheader.fastq and save them to a file called testdata_1_sub1000.fastq. Gzip this file. 
11. Perform the tasks of 10. (except change to “/2”) and 11. above in one single command using pipes for testdata_2.fastq and write the data into a compressed file called testdata_2_sub1000.fastq.gz.
12. Identify all reads with the in-line barcode TGCACTAC from the file testdata_1_sub1000.fastq.gz and write them to the file sample_TGCACTAC_sub.1.fastq. 

Advanced:

13. Which are the 24 most common barcodes (of length 8bp) in the file testdata_1.fastq and how often do they occur?
14. Extract the pe reads corresponding to the reads in sample_TGCACTAC_sub.1.fastq from testdata_2_sub1000.fastq.gz and write them to sample_TGCACTAC_sub.2.fastq.
15. The file “barcodes” contains a list of the in-line barcodes used during the preparation of the current library. Count the number of reads for every barcode in testdata_1.fastq.



