Structure
=======================================

mip.pl
---------------------------------------
Central hub and likely the only script most users will ever interact directly with.

.. code-block:: console
  
  $ echo "Running MIP on Uppmax, analyzing all samples in family 10"
  $ mip.pl -c CMMS_Uppmax_config.yaml -f 10

Sequence QC
-----------
Raw sequence quality control: `FastQC`_

Alignment
---------
Currently MIP supports these aligners:

#. `Mosaik`_ (WES, WGS)
#. `BWA`_ (WES, WGS, Rapid WGS)

BAM file manipulation
---------------------

- Sorting and indexing: `PicardTools`_ (SortSam)
- Duplicate marking: `PicardTools`_ (MarkDuplicates)
- Realignment and base recalibration: `GATK`_ (Realigner & BaseRecalibration)

Coverage QC
-----------

- Coverage Report and QC metrics: `Chanjo`_ & `BedTools`_
- QC metrics: `PicardTools`_ (MultipleMetrics & HSmetrics)

Variant calling
---------------

- Variant discovery and recalibration: `GATK`_ (HaploTypeCaller, GenoTypeGVCFs & VariantRecalibration)

Variant QC
----------

- All variants: `GATK`_ (VariantEval)
- Exonic variants: `GATK`_ (VariantEval)


Variant Selection
-----------------
Select transcripts that overlap a gene list: :doc:`vcfParser`

Variant annotation
------------------
Collect transcript and amino acid information and information from external 
databases as well as annotation of inheritance models: `VEP`_, :doc:`vcfParser`, `SnpEff`_, `ANNOVAR`_, `GENMOD`_


Variant evaluation
---------------------------------------
Score and rank each variant using weighted sums according to disease causing potential: `Score_mip_variants`_
(see :doc:`score_mip_variants`)
  
qcCollect.pl
---------------------------------------
Collects QC data from the MPS analysis in YAML format. (see :doc:`qcCollect`).


covplots_exome.R / covplots_genome.R
---------------------------------------
Plots coverage across chromosomes.

.. _FastQC: http://www.bioinformatics.babraham.ac.uk/projects/fastqc/
.. _Mosaik: https://github.com/wanpinglee/MOSAIK
.. _BWA: http://bio-bwa.sourceforge.net/
.. _SAMtools: http://samtools.sourceforge.net/
.. _PicardTools: http://picard.sourceforge.net/
.. _BedTools: http://bedtools.readthedocs.org/en/latest/
.. _Chanjo: https://chanjo.readthedocs.org/en/latest/
.. _GATK: http://www.broadinstitute.org/gatk/
.. _mip_family_analysis: https://github.com/moonso/Mip_Family_Analysis
.. _VEP: http://www.ensembl.org/info/docs/tools/vep/index.html
.. _SnpEff: http://snpeff.sourceforge.net/
.. _ANNOVAR: http://www.openbioinformatics.org/annovar/
.. _GENMOD: https://github.com/moonso/genmod/
.. _Score_mip_variants: https://github.com/moonso/score_mip_variants