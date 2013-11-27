QCCollect
=========
Collects information on MPS analysis from each analysis run. Uses YAML files for input and output.
QCCollect uses a yaml file for matching the outdata produced in each run to another yaml file with 
regular expression used to actually collect the data from the output files. The collected data is then written
to disc in yaml format. 
 
 
MIP produces a sampleInfo yaml file, containing all sample and family information used in each analysis run.

Usage
-----
``perl qcCollect.pl -si [SampleInfoFilePath] -r [regularExpressionFilePath] -o [Outfile]`` 

Installation
------------
qcCollect is written in Perl, so naturally you need to have Perl installed.

SetUp
-----
1. The regular expression file needs to be created. The regExp file used at CMMS can be 
printed from qcCollect using the ``-preg & -prego`` flags

.. csv-table:: qcCollect Parameters
  :header-rows: 1
  :file: qcCollect_parameters.csv