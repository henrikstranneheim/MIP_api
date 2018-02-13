vcfParser
==========
Parses vcf files to reformat/add INFO fields and metaData headers and/or select entries 
belonging to a subgroup e.g. a list of genes. Input can be piped or supplied as an infile.

Usage
------
``vcfParser.pl infile.vcf > outfile.vcf``

``vcfParser.pl infile.vcf --parseVEP 1 -rf External_Db.txt -rf_ac 3 -sf genes.v1.0.txt -sf_mc 3 -sf_ac 3,4,11,15,17,20 -sof selected_genes.vcf > outfile.vcf``

Installation
-------------
vcfParser is written in Perl, so naturally you need to have Perl installed. The perl 
module `Set::IntervalTree`_ is required and are used to add "ranged" annotations. 

VEP
~~~~
Parses the output from VEP to include RefSeq transcripts. The transcript and protein 
annotations, moste severe consequence and gene annotations are also included in the output
. Transcript protein predictions (Sift and Polyphen) can also be included.

Select Mode
~~~~~~~~~~~
A list of genes and their corresponding HGNC Symbol can be used to fork the analysis into
"selected" genes and "orphan" genes.

GuideLines on format for database of genes
*******************************************
 - The database file should contain a header line starting with "#".
 - The number of headers should match the number of field elements for each entry.
 - Do not use whitespace in headers.
 - Do not use ";" in file.
 - Separate elements in fields with ",". Do not use ", ".
 - No whitespace in the beginning or end within fields.
 - No entries should be duplicated within database.
 - Length of gene coordinates should be greater than 0
 - Only digits in gene coordinate entries
 
Range Annotations
~~~~~~~~~~~~~~~~~
vcfParser can also add range annotations to the vcf by using the `Set::IntervalTree`_ perl
cpan module and a file with chromosomal coordinates and features to be annotated.
 
 .. _Set::IntervalTree: https://metacpan.org/pod/Set::IntervalTree