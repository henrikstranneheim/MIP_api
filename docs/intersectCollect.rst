IntersectCollect
================
Intersects and collects information (in any order) based on 1-4 keys present (mandatory) in each database file to be investigated.
The set of elements to be interrogated are decided by the first db file elements unless the merge option is used.
The db files are supplied using the ``-db`` flag, which should point to a database master file (tab-sep) with the format:

.. code-block:: console

  {DATABASE PATH}\t{SEPARATOR}\t{COLUMN KEYS}\t{CHROMOSOME COLUMN}\t{MATCHING}\t{COLUMNS TO COLLECT}\t{FILE SIZE}

.. note::

  That matching should be either range or exact. Currently the range option only supports 3-4 keys i.e. only 3 keys are used to define the range look up (preferbly chromosome,start,stop). Range db file should be sorted ``-k1,1 -k2,2n`` if it contains chromosome information. 

If the merge option is used then all overlapping and unique elements are added to the final list. 
Beware that this option is memory demanding.

IntersectCollect requires a tab-separated plain text file (the database master file) describing:

1. The headers and columns to collect from the databases. 


2. The path to the database that holds the elements that subsequent database elements are to be merged to (if the keys match). 


3. The subsequent database(s) to match keys and merge elements from. 

Meta-information (Optional)
---------------------------

Meta-information is not mandatory and exists only to aid in the interpretation of the headers 
and data in the template master file and annotated file. The metadata does not need to be ordered and can be present 
without a corresponding header. However, the metadata {COLUMN NAME} must be identical to the 
corresponding header {COLUMN NAME} to link the correct information. 

Format::

##{COLUMN NAME}={String}\t{TYPE}={String}\t{VERSION}={String}\t{DESCRIPTION}={String}\t{dBNAME}={String}

File meta-information is included after the ``##`` string and must be *key=value* pairs.


.. note::

  An example database master file can be found at `MIP's`_ github repository. 


Usage
-----
``perl intersectCollect.pl -db [DatabaseMasterFilePath] -o [Outfile]`` 

Installation
------------
IntersectCollect is written in Perl, so naturally you need to have Perl installed.
IntersectCollect also supports `Tabix`_ formated files.

Tabix
~~~~~
1. `Download`_ and install `Tabix`_. 
2. Add the C/java version of tabix to your ``$PATH``. 
3. Install the perl wrapper that comes with Tabix by:

.. code-block:: console

  $cd {PATH}/tabix-0.2.6/perl/
  $perl Makefile.PL
  $make test
  $make install

SetUp
-----

Database Master File
~~~~~~~~~~~~~~~~~~~~
* Line 1: ``outinfo:ColumnName=>{DATABASE LINE NR IN FILE}_{COLUMN IN DATABASE AT LINE NR IN FILE},..,ColumnNameN``
* Line 2: ``{DATABASE PATH}\t{SEPARATOR}\t{COLUMN KEYS}\t{CHROMOSOME COLUMN}\t{MATCHING}\t{COLUMNS TO COLLECT}\t{FILE SIZE}``
	\.
	
	\.
* Line N: ``{DATABASE PATH}\t{SEPARATOR}\t{COLUMN KEYS}\t{CHROMOSOME COLUMN}\t{MATCHING}\t{COLUMNS TO COLLECT}\t{FILE SIZE}``

Explanation
~~~~~~~~~~~
#. Separator = Any separator that can be handled by Perls split function. 
#. Column keys = 1-4 column numbers designating the keys to be used in matching. Entry is comma-separated.
#. Chromosome column = The column number of the chromosome element.
#. Matching = range (3-4 keys) or exact (1-4 keys).
#. Columns to collect = The columns number that are to be collected (1..N). Entry is comma-separated.
#. File size = tabix, small (read whole database to RAM) or large (handle database one chromosome at a time). 

Dynamic Template File used with MIP
-----------------------------------
To avoid having to update the master template file each time MIP is executed for different families
the database path are dynamically updated by MIP for the following "dynamic strings":

  * FDN! = '-f familyID' (from command line)
  * IDN! = '-s sampleIDs' (from command line), configuration file or supplied pedigree file
  * RD!= '-rd' referencesDir (from command line), configuration file or supplied pedigree file
  * ODF!= '-odd' outDataDir (from command line), configuration file or supplied pedigree file
  * ALIGNER! = '-aligner' (from command line), configuration file or supplied pedigree file

.. csv-table:: intersectCollect Parameters
  :header-rows: 1
  :file: tables/intersectCollect_parameters.csv
  
.. _MIP's: https://github.com/henrikstranneheim/MIP/tree/master/templates
.. _Tabix: http://samtools.sourceforge.net/tabix.shtml
.. _Download: http://sourceforge.net/projects/samtools/files/tabix/