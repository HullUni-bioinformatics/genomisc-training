## Solutions

Note that there are usually several ways to solve the problems and the ones stated below represent just examples. If you found another way to do it – Congratulations! Lines starting with “$” represent commands to be executed in the terminal window. Italicized text after the “#” gives some extra info on what the command is doing. 

1. 
```bash
cd ~/your_directory
mkdir exercise_1
cd exercise_1
cp ~/Downloads/testdata_1.tar .
```

2.
```bash
tar xvf testdata_1.tar #this will produce the directory “testdata_1”, which contains the gzipped file testdata_interleaved.fastq.gz
ls –hlrt testdata_1/ #look whats in the directory and have the content listed with some information on filesize in human readable format
```

3. $ cd testdata_1
$ gunzip testdata_interleaved.fastq.gz #decompresses the “gzipped” file. Note that per default a new decompressed file is created, while the gzipped version disappears. This behavior can be modified in various ways. See manual.
$ less testdata_interleaved.fastq #less is a useful program to look at large text files. It does not have to read the entire text file before starting so it is much faster to open large files than your standard text editor. Navigate with up/down key. Many, many functions including pattern search. Look at manual for details. Quit with “q”.
$ more testdata_interleaved.fastq #similar to less but slightly different functionality
$ head testdata_interleaved.fastq #writes the first 15 lines of the file to your screen (usually called standard output or STDOUT. Number of lines to be written out can be controlled (see man)
$ tail testdata_interleaved.fastq #writes the last 15 lines of the file to STDOUT.
$ cat testdata_interleaved.fastq #writes the entire content of the file to STDOUT); not very helpful at first glance (stop the process by pressing “CTRL-c”), but you can use “pipe” (“|”) to forward the content of the STDOUT directly to another program without displaying -> very powerful!! See below..

You may perform all these actions also directly on compressed files.

$ gzip testdata_interleaved.fastq
$ gunzip –c testdata_interleaved.fastq.gz #gunzippes the file, but instead of creating a new file this command writes the content of the file to the STDOUT.
$ zcat testdata_interleaved.fastq.gz #writes the content of a compressed file to STDOUT.
$ zcat testdata_interleaved.fastq.gz | head #head can not directly display the content of a gzipped file in human readable form (try it). It can however be combined with other commands using pipe.
$ gunzip –c testdata_interleaved.fastq.gz | tail


a) The quality scores in the file are encoded in Phred+33 (Sanger) format.
b) The header of the third read (forward read) is:
@DHKW5DQ1:324:C2G0EACXX:4:2308:19447:41921 1:N:0:TGAACTGG
c) The header of the last read (reverse read) is:
@DHKW5DQ1:324:C2G0EACXX:4:2316:21327:100822 2:N:0:TGAACTGG
	

Note: This data file contains both se and pe reads in what is sometimes referred to as interleaved format. That means that se and pe reads from a given DNA fragment occur in the file in consecutive order. Quite often se and pe reads are provided in separate files. In this case the first read in the se (often named something_1.fastq) file corresponds to the first read in the pe file (e.g. something_2.fastq) and so on.

4. $ wc –l testdata_interleaved.fastq
$ cat testdata_interleaved.fastq | wc -l
$ zcat testdata_interleaved.fastq.gz | wc –l

The file contains 6705040 lines.

5. You already know that the standard fastq format per definition has 4 lines per read, so the number of reads in the file is simply the number of lines/4, i.e. 6705040/4 = 1676260. But you could also simply count all lines containing headers in the file. Can you find a pattern you could search for that is true for every header and does not occur in any other line? 
Per definition the header of a fastq file starts with “@” so if you would count all lines in the file that start with a “@”.

$ grep “^@” testdata_interleaved.fastq | wc –l #grep is a very powerful command for pattern search. The pattern in this case is “@”. The “^” in the command is a special character and defines the start of the line. So by “^@” you search for lines starting with @. 

The result of the linecount is: 2159438, i.e. not the expected 1676260. What’s wrong? By chance some of the quality lines might also start with @. Have a look:

$ grep “^@” testdata_interleaved.fastq | less

So, our pattern has to be more specific. We know that a fastq header usually starts with an id specific to the machine, which was used to generate the data. In our case “@DHKW5DQ1” should thus be a pattern that occurs only in headers.

$ grep “^@DHKW5DQ1” testdata_interleaved.fastq | wc –l
or
$ grep –c “^@DHKW5DQ1” testdata_interleaved.fastq

Result: 1676260

You can try to find other patterns that are specific to headers.

6. Se/pe reads can be identified immediately by looking at their header. In our case a typical se read would be identified by a header containing “ 1”, while a pe read would contain “ 2”. There are different conventions. Sometimes se/pe reads are identified by headers that end in “/1” and “/2”, respectively. Use grep to count the number of se/pe reads, by e.g.:

$ grep –c “ 1:” testdata_interleaved.fastq
$ zcat testdata_interleaved.fastq.gz | grep –c “ 1:”
$ grep “ 2:” testdata_interleaved.fastq |wc –l

The file contains 838130 se/pe reads respectively.

7. Grep enables to you to search for a pattern and then output the line containing the pattern plus a given number of lines after the pattern using the –A flag. While building a complex command you may want to limit your data to only a subset of the actual data file, and only run the full command once you are sure it is correct, e.g.:

$ head –n 20 testdata_interleaved.fastq | grep “ 1:” –A 3 #displays just the first 20 lines of your result

Examining the output of the above command you will see that the –A option separates every hit by lines containing “--“. Another pattern that you can look for. However in this case you will want to invert the pattern search and only display lines that do not contain the pattern.

$ head –n 20 testdata_interleaved.fastq | grep “ 1:” –A 3 | grep –v “^--$“ #Note the use of the special characters “^” for “the beginning of the line” and “$” for “the end of the line”.

Does the result look ok? If yes you can simply replace head by cat to output the result for the full data file. Instead of writing the result to the STDOUT you may save it directly to a file (using “>”).

$ cat testdata_interleaved.fastq | grep “ 1:” –A 3 | grep –v “^--$“ > testdata_1.fastq

for the pe reads:
$ grep “ 2:” –A 3 | grep –v “^--$“ > testdata_2.fastq

directly from a compressed data file:
$ zcat testdata_interleaved.fastq.gz | grep “ 1:” –A 3 | grep –v “^--$“ > testdata_1.fastq

8. A simple pattern search as before should do here: 

a) 
$ grep –c “TGCACTAC” testdata_1.fastq
$ grep “TGCACTAC” testdata_1.fastq | wc -l

This identifies 11565 reads.

b)
$ grep –c “^TGCACTAC” testdata_1.fastq
$ grep “^TGCACTAC” testdata_1.fastq | wc -l 

This identifies 9582 reads.

9. A very useful program to find and replace patterns in text files is sed. Sed is a very powerful tool. A basic usage for find and replace would be: 
$ sed ‘s/pattern/replace/g’ file. This would output the content of the file to STDOUT. Each occurrence of “pattern” in the file would be replaced by “replace”. Sed is case sensitive, so for example you may want to replace every “C” in your file with a “c”. Try it:

$ sed ‘s/C/c/g’ testdata_1.fastq | less 

Note that in Unix some characters have a special meaning. You are already familiar with “^” for “the beginning of a line” and “$” for “the end of a line”. Other examples: “.” is interpreted by sed as “any character”, while “.*” is interpreted as “any character and everything that might follow after this character until the end of the line”. These special characters are very useful in the context of regular expressions (google “Unix regular expressions” to find out more). However, you can tell sed (and also grep) to interpret also these characters in their literal meaning by prepending “/” to the character. For example if you want to replace every “.” in a file, instead of replacing “every character” in the file:
$ sed ‘s/\./REPLACE/g’ testdata_1.fastq |less

A solution to our problem could thus be:

$ sed ‘s/ 1:.*/\/1/g’  testdata_1.fastq > testdata_1_newheader.fastq #Note the use of “.*” to specify “everything that follows after “ 1:” and the use of “\” before “/1” to force the literal interpretation of “/”, instead of specifying the end of the pattern as part of sed. 

10. $ head –n 4000 testdata_1_newheader.fastq > testdata_1_sub1000.fastq
$ gzip testdata_1_sub1000.fastq

11. $ sed ‘s/ 2:.*/\/2/g’ testdata_2.fastq | head –n 4000 | gzip > testdata_2_sub1000.fastq.gz

12. $ zcat testdata_1_sub1000.fastq.gz | grep “^TGCACTAC” –A 2 –B 1 | grep “^--$“ –v > sample_TGCACTAC_sub.1.fastq #in addition to using –A for “lines after pattern” you can also specify the number of lines before the pattern that you will want with “-B”

The resulting file should contain 11 sequences.

13. The current problem will require a complex command, which we will try to build step by step. The only relevant lines for this task are those containing the nucleotide sequence. Per definition fastq format contains the nucleotide sequence in the second line. Every 4th line after that will be another nucleotide sequence. 
A very handy function of sed is to print only a specified line followed by every nth line after that. In our case we will want to print every 4th line starting from the second line of the file. 

$ sed –n ‘2~4p’ testdata_1.fastq | less

The command cut can be used to only print selected characters of a line. In our case we will want to reduce our file to only the first 8 (i.e. characters 1-8) of the line, because we know that the in-line barcode is specified there. Build on the previous command:

$ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | less

The command sort can be used to sort given data in particular ways, for example numerically.

$ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | sort –n | less

The command uniq can be used to collapse consecutive identical lines and thus reduce redundancy in files.

$ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | sort –n | uniq | less

With a little extra flag uniq can at the same time count the number of occurrences for each line.

$ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | sort –n | uniq -c| less

The next step will be to sort the result numerically and at the same time to reverse the sorting order, so that the highest number appears on top.

 $ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | sort –n | uniq -c| sort –nr | less

Finally, we simply limit our output to the first 24 lines, which in our case corresponds to the 24 most abundant barcodes.

$ sed –n ‘2~4p’ testdata_1.fastq | cut –c 1-8 | sort –n | uniq -c| sort –nr | head –n 24 

Extra question:
From our sequencing provider we know that the sequence “CCGTCTAC” is not a barcode that has been used during library preparation, yet it comes up among the 24 most abundant sequences. What could be an explanation for its high abundance?

14. You know that the headers of corresponding se and pe reads are largely identical. They only differ in the part that identifies the read as se/pe. So, one can use the ids of the se reads in a pattern search to identify the corresponding pe reads. This could be done manually, by copying the header of the first read in sample_TGCACTAC_sub.1.fastq, remove the se/pe specific part and use it in a pattern search:

$ zcat testdata_2_sub1000.fastq.gz | grep “^@DHKW5DQ1:324:C2G0EACXX:4:2308:2072:42111” –A 3 > sample_TGCACTAC_sub.2.fastq

Then do the same for the second header in sample_TGCACTAC_sub.1.fastq and append to result to sample_TGCACTAC_sub.2.fastq with the “>>” command.

$ zcat testdata_2_sub1000.fastq.gz | grep “^@DHKW5DQ1:324:C2G0EACXX:4:2308:8096:42248” –A 3 >> sample_TGCACTAC_sub.2.fastq

.. and so on for the remaining 9 sequences. A much more efficient way would be to assign the header ID to a variable and to use this variable as a dynamically changing search pattern.

You can assign the search pattern as a value (in this case a string) to a variable, e.g. named header ..

$ header=”@DHKW5DQ1:324:C2G0EACXX:4:2308:2072:42111”

.. and subsequently use the variable in the search in place of the text. To tell Unix to search for the string stored in the variable instead of the literal “header” you identify header as a variable by prepending “$”, like “$header”. Try the following:

$ echo header #this will write header to your screen

$ echo $header #this identifies header as a variable and the string assigned to the variable will be written to STDOUT

You can now assign a different value to the variable, e.g.:

$ header=”I am learning Unix”

and display the new content of the variable using the same command:

$ echo $header

In the context of our search you can try:

$ header=”@DHKW5DQ1:324:C2G0EACXX:4:2308:2072:42111”
$ zcat testdata_2_sub1000.fastq.gz | grep “^$header” –A 3 > sample_TGCACTAC_sub.2.using_variable.fastq

$ header=“@DHKW5DQ1:324:C2G0EACXX:4:2308:8096:42248”
$ zcat testdata_2_sub1000.fastq.gz | grep “^$header” –A 3 >> sample_TGCACTAC_sub.2.using_variable.fastq

Instead of manually assigning new search strings to the variable you can do this automatically.
First produce a simple command that lists the IDs of the 11 sequences in sample_TGCACTAC_sub.1.fastq and remove the se specific part of the header, e.g. replace it with an empty string.

$ grep “/1$” sample_TGCACTAC_sub.1.fastq | sed ‘s/\1//g’ 

Write the resulting list to a file:

$ grep “/1$” sample_TGCACTAC_sub.1.fastq | sed ‘s/\1//g’ > list

Now read this file line by line and assign the content of each line in turn to the variable $header. The content of the variable is then used as search pattern as before. This can be done via a “for loop” (google “Unix for loop” for many useful examples).

$ for header in $(cat list); do zcat testdata_2_sub1000.fastq.gz | grep "^$header" -A 3 | grep "^--$" -v;  done > sample_TGCACTAC_sub.2.fastq

You could even do it all in one go:

$ for header in $(grep “/1$” sample_TGCACTAC_sub.1.fastq | sed ‘s/\1//g’); do zcat testdata_2_sub1000.fastq.gz | grep "^$header" -A 3 | grep "^--$" -v;  done > sample_TGCACTAC_sub.2.fastq


16. Using principally the same approach as in the previous exercise we can loop line by line through the barcodes file and in turn use each line as search pattern. For every search pattern we simply count the occurrences. Finally, we sort the barcodes in descending order, just for convenience. 

$ for line in $(cat barcodes); do sed -n '2~4p' testdata_1.fastq | grep "^$line" | cut -c 1-8 | uniq -c;  done | sort -rn
