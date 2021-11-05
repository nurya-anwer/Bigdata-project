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
   
Part-3: Indexing reference genome
   The "star-index.sh" shell script indexes (prepares) a subset of your genome 
   
Part-4: Using STAR short read alignment program   
   The "star-align.sh" shell script performs the alignment of reads from sample SRR1039508 to the partial genome indexed in the previous step.
        nohup sh star-align.sh &
  nohup command explanation: The nohup command executes another program specified as its argument and ignores all SIGHUP (hangup) signals. SIGHUP is a signal that is sent to a process when its controlling terminal is closed. Usually, when you run a program over SSH, if your connection drops or you log out, the session is terminated, and all the processes executed from the terminal will stop. This is where the nohup command comes handy. It ignores all hangup signals, and the process will continue to run.
Part-5: Understanding SAM and BAM formats
Answer the following questions for your output sam file
i/$username-aln-SRR1039508.Aligned.out.sam)
   
   
a)	What are the headers(columns) in SAM format?
Col	Read 1	      Field	         Description
1	   SRR1039508.7.1	QNAME	         Query template NAME The name of the query sequence "
   
2	   99	            FLAG	         A bitwise set of information describing the alignment It  is not a number, but it is a binary encoding of a hexadecimal value. Therefore, "99" was entered in SAM identifier web sites https://www.samformat.info/sam-format-flag https://broadinstitute.github.io/picard/explain-flags.html and indicated read paired (0x1); read mapped in proper pair (0x2); mate reverse strand (0x20); first in pair (0x40) In other words: A read that is paired (1), in a proper pair (2), with a mate on the reverse strand (32) and is the first in a pair (64) has a flag of 99 because: 1+2+32+64=99
   
   
3	   2	           RNAME	References sequence NAME The name of the reference contig the sequence is aligned to e.g. 2 (chromosome 2)
   
4	107315003	     POS	   Leftmost position of where this alignment maps to the reference The read aligns starting at position 107315003 on the reference
   
5	255	          MAPQ	  Mapping quality of read to reference The mapping quality of the read; maximum value is usually 60. This number is Phred scaled, meaning that they’re logarithmically scaled. It equals −10 log10 Pr{mapping position is wrong}, rounded to the nearest integer. A value 255 indicates that the mapping quality is not available.
   
   
6	63M	        CIGAR	Concise Idiosyncratic Gapped Alignment Report Standard CIGAR description of pairwise alignment defines three operations: ‘M’ for match/mismatch, ‘I’ for insertion compared with the reference and ‘D’ for deletion. In our case the CIGAR says that the first 63 bases in the read sequence align (match) with the reference.
   
   
7	 =	           RNEXT	Ref. name of the mate/next read Reference sequence name of the primary alignment of the NEXT read in the template. In other words, the reference sequence name of the next alignment in this group, MRNM or RNEXT. In paired alignments, it is the mate's reference sequence name. The name of the reference contig that the other read in the pair aligns to. If identical to this read, an equals sign (“=”) is used
   
8	107315026	  PNEXT	Paired Mate Position The position on the reference contig that the other read in the pair aligns to. In our case, 107315026
   
9	84	          TLEN	The length of the template fragment. This is the distance from the leftmost base of the first read to the rightmost base of the second read of a pair. In our case it is 84.
   
10	TCTCAAATTTTTCAATGGTTCTTTTGTCGATGCCACCGCATTTATAGATCAGATGGCCAGTAG	SEQ	The DNA sequence of the query sequence. This will be identical to the sequence in the FASTQ file that was aligned to the reference genome
   
   
11	HJJJJJJJJJJJJJJJJ JIJJJJJ JJJJJJJJJJJJJJJJJ JJJJ JJJJJJJJJJJJHH HH HF	QUAL	Query quality It is an indicator for how accurate each base in the query sequence (SEQ) is. Quality is calculated based on the probability that a base is wrong. The ASCII PHRED-encoded base qualities. Since a human readable format is desired for SAM, 33 is added to the calculated quality in order to make it a printable character ranging from ! - ~. According to the American Standard Code for Information Interchange (ASCII), the binary encoding of a hexadecimal values H=48; J= 4A and F=46 https://en.wikipedia.org/wiki/ASCII
   
   
12	NH:i:1 HI:i:1 AS:i:114 nM:i:4	TAGs	They are a set of predefined tags that are general used in alignments. TAGs are optional fields on a SAM/BAM Alignment.
They are translated through https://www.samformat.info/sam-format-alignment-tags NH:i: Count number of reported alignments that contain the query in the current record. HI:i:i Query hit index, indicating the alignment record is the i-th one stored in SAM. AS:i: Score alignment score generated by aligner. NM:i: Count number of differences (mismatches plus inserted and deleted bases) between the sequence and reference, counting only (case-insensitive) A, C, G and T bases in sequence and reference as potential matches, with everything else being a mismatch.
