Setup
======

Filename convention
~~~~~~~~~~~~~~~~~~~~~
The permanent filename should follow the following format::

  {LANE}_{DATE}_{FLOW CELL}_{IDN}_{BARCODE SEQ}_{DIRECTION 1/2}.fastq.qz

.. note::

   The `familyID` and `sampleID(s)` needs to be unique and the sampleID supplied should be 
   equal to the {IDN} in the filename.

Dependencies
~~~~~~~~~~~~~~
Make sure you have loaded/installed all dependencies and that they are in your ``$PATH``. 
You only need to load the dependencies that are required for the modules that you want to 
run. If you fail to install dependencies for a module, MIP will tell you what dependencies 
you need to install (or add to your ``$PATH``) and exit. MIP comes with an install script ``install.pl``,
which will install all necessary programs to execute models in MIP via bioconda and/or $SHELL.
Version after the software name are tested for compatibility with MIP. 

**Program/Modules**

- Perl modules: YAML.pm, Log4perl.pm, List::MoreUtils, DateTime, DateTime::Format::ISO8601, 
  DateTime::Format::HTTP, DateTime::Format::Mail, Set::IntervalTree from CPAN, since these
  are not included in the perl standard distribution
- Simple Linux Utility for Resource Management (`SLURM`_)
- `FastQC`_ (version: 0.11.5)
- `Mosaik`_ (version: 2.2.24)
- `BWA`_ (version: 0.7.13)
- `Sambamba`_ (version: 0.6.1)
- `SAMTools`_ (version: 1.3)
- `BedTools`_ (version: 2.25.0)
- `PicardTools`_ (version: 2.3.0)
- `Chanjo`_ (version: 3.4.1)
- `Manta`_ (version: 0.29.6)
- `GATK`_ (version: 3.5-0)
- `freebayes`_ (version: 1.0.2)
- `VT`_ (version: 0.5)
- `VEP`_ (version: 84) with plugin "UpDownDistance, LoFtool, LoF"
- vcfParser.pl (Supplied with MIP; see :doc:`vcfParser`)
- `SnpEff`_ (4.2)
- `ANNOVAR`_ (version: 2013-08-23)
- `GENMOD`_ (version: 3.5.2)
- `VcfTools`_ (version: 0.1.14)
- `BcfTools`_ (version: 1.3)
- `PLINK`_ (version: 1.90b3x)
- `MultiQC`_ (version: 0.6)

Depending on what programs you include in the MIP analysis you also need to add
these programs to your ``$PATH``:

- FastQC
- Mosaik
- BWA
- SAMTools
- Tabix
- BedTools
- VcfTools
- PLINK

and these to your python ``virtualenvironment``:

- Chanjo
- GENMOD
- `Cosmid`_ (version: 0.4.9.1) for automatic download

To make sure that you use the same commands to work on the virtualenvironment, you need to
install a virtual environment wrapper. We recommend `pyenv`_ and `pyenv-virtualenvwrapper`_. 
To enable the virualenvwrapper add: ``pyenv virtualenvwrapper`` to your ``~/.bash_profile``. 

Databases/References
--------------------

Please checkout `Cosmid`_ to download references and/or databases on your own or via MIP.

MIP can build/download many program prerequisites automatically:

.. note::

   Download is only enabled when using the default parameters of MIP and requires a Cosmid 
   installation in your python virtualenvironment.
   
**Automatic Download:**

1. Human Decoy Genome Reference (1000G)
2. The Consensus Coding Sequence project database (CCDS)
3. Relevant references from the 1000G FTP Bundle (mills, omni, dbsnp etc)

**Automatic Build:**

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
directory using Annovars built-in download function.

.. note::
   
   This applies only to the supported annovar databases. Supply flag "--annovarSupportedTableNames"
   to list the MIP supported databases.

.. _Mosaik: https://github.com/wanpinglee/MOSAIK
.. _BWA: http://bio-bwa.sourceforge.net/
.. _FastQC: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
.. _SAMtools: http://samtools.sourceforge.net/
.. _Sambamba: http://lomereiter.github.io/sambamba/
.. _BedTools: http://bedtools.readthedocs.org/en/latest/
.. _SLURM: http://slurm.schedmd.com/
.. _PicardTools: http://picard.sourceforge.net/
.. _Chanjo: https://chanjo.readthedocs.org/en/latest/
.. _GATK: http://www.broadinstitute.org/gatk/
.. _freebayes: https://github.com/ekg/freebayes
.. _Manta: https://github.com/Illumina/manta
.. _VT: https://github.com/atks/vt
.. _VEP: http://www.ensembl.org/info/docs/tools/vep/index.html
.. _SnpEff: http://snpeff.sourceforge.net/
.. _ANNOVAR: http://www.openbioinformatics.org/annovar/
.. _GENMOD: https://github.com/moonso/genmod/
.. _Score_mip_variants: https://github.com/moonso/score_mip_variants
.. _VcfTools: http://vcftools.sourceforge.net/
.. _BcfTools: https://samtools.github.io/bcftools/bcftools.html
.. _PLINK: http://pngu.mgh.harvard.edu/~purcell/plink/data.shtml
.. _MultiQC: https://github.com/ewels/MultiQC
.. _Cosmid: https://github.com/robinandeer/cosmid
.. _Tabix: http://samtools.sourceforge.net/tabix.shtml
.. _pyenv: https://github.com/yyuu/pyenv
.. _pyenv-virtualenvwrapper: https://github.com/yyuu/pyenv-virtualenvwrapper