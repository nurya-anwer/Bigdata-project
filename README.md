# Bigdata-project
Part-I. Understanding FASTQ format&Part-2: Understanding reference genome (sequence and annotation)
1) Using a toy fastq example file “/fastq/toy.fastq” 
   where every four lines correspond to one sequence read:
a) What is given in the first line?
@SRR1039508.1 HWI-ST177:290:C0TECACXX:1:1101:1225:2130 length=63
Abbreviation 	                        Meaning
SRR1039508.1 HWI-ST177:               The unique instrument name
290:	                                The run id
C0TECACXX:	                          the flow cell id
1:	                                  Flow cell lane
1101:	                                Tile number within the flow cell lane
1225:	                                'x'-coordinate of the cluster within the tile
2130:	                                'y'-coordinate of the cluster within the tile
length=63	                             Read length

b)What is given in the second line?
 CATTGCTGATACCAANNNNNNNNGCATTCCTCAAGGTCTTCCTCCTTCCCTTACGGAATTACA                                     
 The raw sequence letters. The base calls: A, C, T, G and N 
 A = adenine C = cytosine G = guanine T = thymine 
 N . It represents ambiguity which are used when more than one kind of the 4 basic nucleotides (A, T, G, C) could occur at that position.
 
c)What is given in the third line?
  +SRR1039508.1 HWI-ST177:290:C0TECACXX:1:1101:1225:2130 length=63
 It begins with a '+' character and is optionally followed by the same sequence identifier ）
 
 d)What is given in the fourth line?
   HJJJJJJJJJJJJJJ########00?GHIJJJJJJJIJJJJJJJJJJJJJJJJJHHHFFFFFD
It encodes the quality values for the sequence in line 2. 
It must contain the same number of symbols as letters in the sequence. 
Quality scores are encoded into a compact form, which uses only 1 byte per quality value. 
In this encoding, the quality score is represented as the character with the American Standard Code for Information Interchange (ASCII) code equal to its value + 33. 

 Part-2: Understanding reference genome (sequence and annotation)
 1) Using a real gtf example file “/gtf/Homo_sapiens.GRCh37.75.gtf”:
  a) With the help of online documentation for gtf file, list each column and give description.
  
Col Field 	    Read 1	Description
1	seqname	     1	      seqname - name of the chromosome or scaffold; chromosome                             names can be given with or without the 'chr' prefix.
2	source	    pseudogene	Indicating where the annotations came from --- typically                            the name of either a prediction program or a public                                  database.
3	featuretype	 gene	     Feature type name, e.g. Gene, Variation, Similarity
4	start	       11869	  Start coordinate of the feature relative to the beginning                           of the sequence named in <seqname>.
5	end	       14412	  End coordinates of the feature relative to the beginning                             of the sequence named in <seqname>.
6	score	.	              Indicates a degree of confidence in the feature's       existence and coordinates. The value of this field has no global scale but may have relative significance when the <source> field indicates the prediction program used to create this annotation. It may be a floating point number or integer, and not necessary and may be replaced with a dot.
7	strand	    +	        defined as + (forward) or - (reverse).
8	frame	.	              One of '0', '1' or '2'. '0' indicates that the first base of the feature is the first base of a codon, '1' that the second base is the first base of a codon, and so on.
9	attribute	gene_id	  All nine features have the same two mandatory attributes at the end of the record.
                          gene_id value; A globally unique identifier for the genomic locus of the transcript. If empty, no gene is associated with this feature. 
                          transcript_id value; A globally unique identifier for the predicted transcript. If empty, no transcript is associated with this feature. "ENSG00000223972"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene";
