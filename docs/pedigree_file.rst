Pedigree File
=============

Family meta data file. Records important metrics for tracking samples and find biases in 
isolation of DNA or subsequent sequence analysis.

Among other things, the file enables:

1. Automatic coverage specification (correct target file(s))

2. Application of mendelian filtering models, e.g. `autosomal dominant`, based on pedigree, sex and disease status

3. Collection of analysis info for the sequence analysis pipeline 

The pedigree file format is defined by `PLINK`_, although we currently only support tab-sep pedigree files. 

The first row should start with a "#" (hash) and contain relevant headers separated by tabs describing each column.
The first six columns are mandatory. The name and order of the headers should follow:

.. csv-table:: Mandatory Columns
  :header-rows: 1
  :widths: 1 1 4
  :file: tables/pedigree_file_mandatory_columns.csv

In addition to these mandatory columns we use the pedigree file to record meta data on each individual.
Entries within each column should be separated with ";" (semi-colon) and entered in consecutive order.  
Each individual recorded in the pedigree file is written on one line and a tab should 
separate each entry. No individual should be recorded twice. The order of individuals below
the header line does not matter.

If there is no information on the parents or the grandparents they should be encoded as “0”. 

An example pedigree file can be found `here`_.

The pedigree file should named: ``<FDN>_pedigree.txt``.

.. csv-table:: Additional columns in the pedigree file
  :header-rows: 1
  :file: tables/pedigree_file_optional_columns.csv


Pedigree capture kits aliases
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Agilent Sure Select

  * Agilent_SureSelect.V2 => Agilent_SureSelect.V2.GenomeReferenceSourceVersion_targets.bed
  * Agilent_SureSelect.V3 => Agilent_SureSelect.V3.GenomeReferenceSourceVersion_targets.bed
  * Agilent_SureSelect.V4 => Agilent_SureSelect.V4.GenomeReferenceSourceVersion_targets.bed
  * Agilent_SureSelect.V5 => Agilent_SureSelect.V5.GenomeReferenceSourceVersion_targets.bed
  * Agilent_SureSelectCRE.V1 => Agilent_SureSelectCRE.V1.GenomeReferenceSourceVersion_targets.bed
  * Latest => Agilent_SureSelect.V5.GenomeReferenceSourceVersion_targets.bed
  
* NimbleGen

  * Nimblegen_SeqCapEZExome.V2 => Nimblegen_SeqCapEZExome.V2.GenomeReferenceSourceVersion_targets.bed
  * Nimblegen_SeqCapEZExome.V3 => Nimblegen_SeqCapEZExome.V3.GenomeReferenceSourceVersion_targets.bed

.. note::
 You can use other target region files with MIP but then you have to supply the complete filename with ".bed" ending.


Abbrevations
--------------
.. csv-table:: 
  :header-rows: 1
  :file: tables/pedigree_file_abbreviations.csv


.. _PLINK: http://pngu.mgh.harvard.edu/~purcell/plink/data.shtml
.. _here: https://github.com/henrikstranneheim/MIP/blob/develop/templates/1_pedigree.txt
