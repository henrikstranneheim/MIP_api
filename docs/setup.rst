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
you need to install (or add to your ``$PATH``) and exit. Version after the software name
are tested for compatibility with MIP. 

**Program/Modules**

- Perl `YAML.pm`_ module and `Log::Log4perl.pm`_ since this is not included in the Perl standard
  distribution
- Simple Linux Utility for Resource Management (`SLURM`_)
- `FastQC`_ (version: 0.11.2)
- `Mosaik`_ (version: 2.2.24)
- `BWA`_ (version: 0.7.12)
- `SAMTools`_ (version: 1.1)
- `BedTools`_ (version: 2.20.1)
- `PicardTools`_ (version: 1.137)
- `Chanjo`_ (version: 2.3.0)
- `GATK`_ (version: 3.4-46)
- `VT`_ (version: 0.5)
- `VEP`_ (version: 81)
- vcfParser.pl (Supplied with MIP; see :doc:`vcfParser`)
- `SnpEff`_ (4.1)
- `ANNOVAR`_ (version: 2013-08-23)
- `GENMOD`_ (version: 3.0.1)
- `VcfTools`_ (version: 0.1.12b)
- `BcfTools`_ (version: 1.1)
- `PLINK`_ (version: 1.07)

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

.. _YAML.pm: http://search.cpan.org/~mstrout/YAML-0.84/lib/YAML.pm
.. _Log::Log4perl.pm: http://search.cpan.org/~mschilli/Log-Log4perl-1.46/lib/Log/Log4perl.pm
.. _Mosaik: https://github.com/wanpinglee/MOSAIK
.. _BWA: http://bio-bwa.sourceforge.net/
.. _FastQC: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
.. _SAMtools: http://samtools.sourceforge.net/
.. _BedTools: http://bedtools.readthedocs.org/en/latest/
.. _SLURM: http://slurm.schedmd.com/
.. _PicardTools: http://picard.sourceforge.net/
.. _Chanjo: https://chanjo.readthedocs.org/en/latest/
.. _GATK: http://www.broadinstitute.org/gatk/
.. _VT: https://github.com/atks/vt
.. _VEP: http://www.ensembl.org/info/docs/tools/vep/index.html
.. _SnpEff: http://snpeff.sourceforge.net/
.. _ANNOVAR: http://www.openbioinformatics.org/annovar/
.. _GENMOD: https://github.com/moonso/genmod/
.. _Score_mip_variants: https://github.com/moonso/score_mip_variants
.. _VcfTools: http://vcftools.sourceforge.net/
.. _BcfTools: https://samtools.github.io/bcftools/bcftools.html
.. _PLINK: http://pngu.mgh.harvard.edu/~purcell/plink/data.shtml
.. _Cosmid: https://github.com/robinandeer/cosmid
.. _Tabix: http://samtools.sourceforge.net/tabix.shtml
.. _pyenv: https://github.com/yyuu/pyenv
.. _pyenv-virtualenvwrapper: https://github.com/yyuu/pyenv-virtualenvwrapper