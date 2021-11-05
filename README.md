# Bigdata-project
Part-I. Understanding FASTQ format
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

 
