Setup
======

Filename convention
~~~~~~~~~~~~~~~~~~~~~
The permanent filename should follow the following format::

  {LANE}_{DATE}_{FLOW CELL}_{IDN}_index{BARCODE SEQ}_{DIRECTION 1/2}.fastq.qz

.. note::

   The `familyID` and `sampleID(s)` needs to be unique and that the sampleID supplied should be 
   equal to the {IDN} in the filename.

Dependencies
~~~~~~~~~~~~~~
Make sure you have loaded/installed all dependencies and that they are in your ``$PATH``. You only need to load the dependencies that are required for the modules that you want to run. If you fail to install dependencies for a module, MIP will tell you what dependencies you need to install (or add to your ``$PATH``) and exit.

**Program/Modules**

- Perl `YAML.pm`_ module, since this is not included in the Perl standard
  distribution (if you want to supply config and qc files to MIP)
- Simple Linux Utility for Resource Management (`SLURM`_)
- `FastQC`_
- `Mosaik`_
- `BWA`_
- `SAMTools`_
- `BedTools`_
- `PicardTools`_
- `Chanjo`_
- `GATK`_
- `ANNOVAR`_
- intersectCollect.pl (Supplied with MIP)
- add_depth.pl (Supplied with MIP)
- `mip_family_analysis`_
- `VcfTools`_
- `PLINK`_

Depending on what programs you include in the MIP analysis you also need to add
these programs to your ``$PATH``:

- FastQC
- Mosaik
- BWA
- SAMTools
- BedTools
- PicardTools
- VcfTools
- PLINK

and these to your python ``virtualenvironment``:

- Chanjo
- mip_family_analysis

Databases/References
--------------------

Please checkout `Cosmid`_ to download references and/or databases.

MIP can build certain program prerequisites automatically:

Human Genome Reference Meta Files:
 1. The sequence dictionnary (".dict")
 2. The ".fasta.fai" file

Mosaik:
 1. The Mosaik align format of the human genome {mosaikAlignReference}.
 2. The Mosaik align jump database {mosaikJumpDbStub}.
 3. The Mosaik align network files {mosaikAlignNeuralNetworkPeFile} and {mosaikAlignNeuralNetworkSeFile}. These will be copied from your MOSAIK installation to the MIP reference directory.

BWA:
 1. The BWA index of the human genome. 

.. note::

   If you do not supply these parameters (Mosaik/BWA) MIP will create these from scratch using the supplied
   human reference genom as template. 

Capture target files:
 1. The "infile_list" and .pad100.infile_list files used in {pPicardToolsCalculateHSMetrics}
 2. The ".pad100.interval_list" file used in by some GATK modules.

.. note::

   If you do not supply these parameters MIP will create these from scratch using the supplied
   latest supported capture kit ".bed" file and the supplied
   human reference genome as template.
   
ANNOVAR:
The choosen Annovar databases are downloaded before use if lacking in the annovar/humandb 
directory.

.. note::
   
   This applies only to the supported annovar databases.
   
On UPPMAX
---------
Add modules::

  module load java/sun_jdk1.7.0_25 FastQC mosaik-aligner bwa BEDTools plink vcftools annovar

You do best installing both GATK and PicardTools yourself. Make sure to load the correct Java environment to run these tools.

Moving files from INBOX
~~~~~~~~~~~~~~~~~~~~~~~
FASTQ-files should be moved from `/INBOX` to a backed up directory.

1. Check the :doc:`pedigree_file` and move it to a backed up directory ("exomes")
2. Move and rename gzipped FASTQ-files into ``{SEQ TYPE}/{IDN}/fastq``, where :doc:`IDN` is your sample ID.


.. _YAML.pm: http://search.cpan.org/~mstrout/YAML-0.84/lib/YAML.pm
.. _Mosaik: https://github.com/wanpinglee/MOSAIK
.. _BWA: http://bio-bwa.sourceforge.net/
.. _FastQC: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
.. _SAMtools: http://samtools.sourceforge.net/
.. _BedTools: http://bedtools.readthedocs.org/en/latest/
.. _SLURM: http://slurm.schedmd.com/
.. _PicardTools: http://picard.sourceforge.net/
.. _Chanjo: https://chanjo.readthedocs.org/en/latest/
.. _GATK: http://www.broadinstitute.org/gatk/
.. _ANNOVAR: http://www.openbioinformatics.org/annovar/
.. _mip_family_analysis: https://github.com/moonso/Mip_Family_Analysis
.. _VcfTools: http://vcftools.sourceforge.net/
.. _PLINK: http://pngu.mgh.harvard.edu/~purcell/plink/data.shtml
.. _Cosmid: https://github.com/robinandeer/cosmid