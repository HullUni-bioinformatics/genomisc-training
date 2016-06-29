# HINTS

The following hints suggest commands that might be useful to solve these problems. Not all commands will be needed in all cases, but different combinations and applications of different subsets of commands can be applied.

1. `cd`, `mkdir`, `cp`
2. `tar` 
3. gunzip; cat; zcat; less; more; head; tail;
b) Is the offset of the quality encoding Phred+33 or Phred+64? visit http://en.wikipedia.org/wiki/FASTQ_format for help.
4. gunzip; cat; wc; zcat;
5. cat; zcat; grep; wc; |;
6. cat; zcat; grep; wc; |;
7. cat; zcat; grep (-A, -v); >; |;
8. cat; zcat; grep; wc; |;
For many applications it might be useful/applicable to sequence DNA from several DNA extracts (different individuals, species, etc) during the same run of a NGS instrument (often termed multiplexing). Specific short DNA sequences (barcodes) are ligated to the DNA fragments of different samples during NGS library preparation. After sequencing reads can be assigned back to individual DNA samples based on this barcode. In our case individuals are identified by an 8bp in-line barcode, i.e. the first 8 bp of the se reads. 
9. cat; sed; >; |; 
(example would be to convert a header, such as `@DHK1:324:C2:4:23:19:41 1:N:0:TGCAACTGG` to `@DHK1:324:C2:4:23:19:41/1`)
10. head; >; gzip; 
11. head; |; gzip; >;
12. cat; zcat; grep (-A, -B); >;

Advanced:

13. cat; zcat; sed -n; cut -c; sort; uniq; head; |;
14. for loop; cat; grep; |; >;
15. for loop; cat; sed; grep; cut; uniq; sort; |;

