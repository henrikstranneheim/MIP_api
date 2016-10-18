MIP Analysis
=============

You can modify all parameters to MIP in order of precedence using:

1. Command line
2. Pedigree file
3. Config file
4. Definitions file (typically not done by user)

Start standard analysis
------------------------

	.. code-block:: console
	
	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1

``-rio 1`` means that two blocks will be performed at the nodes without transfer of files between HDS and SLURM nodes within the block. These two blocks are:

[BAMCalibrationBlock]

-  	[PicardTool MergeSamFiles]
-  	[Sambamba Markduplicates]
-  	[GATK ReAlignerTargetCreator/IndelRealigner]
-  	[GATK BaseRecalibrator/PrintReads]


[VariantAnnotationBlock]

- 	[PrepareForVariantAnnotationBlock]
-  	[VT]
-  	[VariantEffectPredictor]
-  	[VCFParser]
-  	[SnpEff]
-   [RankVariants]

``-rio 0`` means that MIP will copy in and out files from HDS and SLURM nodes between each module. Thus increasing the network traffic.

Excluding a program from the analysis
--------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 --pSampleCheck 0

Skipping a already processed module i.e expect that the ouput has already been generated
----------------------------------------------------------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 --pSampleCheck 2

Simulate standard analysis
--------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 -dra 2

``-dra`` means that MIP will run in dru run mode i.e simulation mode. MIP will execute everything except the final sbatch submission to SLURM and updates to qc_sampleInfo.yaml.

``-dra 2`` will include simulation of downloading/building of references.

``-dra 1`` will simulate analysis without downloading/building of references.

``-dra 0`` no simulation

One can use ``-dra 1`` or ``-dra 2`` to generate sbatch scripts which then can be submitted manually by the user individually or sequentially using ``sbatch --dependency``. Note that this will not update qc_sampleInfo.yaml as this is done at MIP runtime.

Rerun analysis using exactly the same parameters as last analysis run
---------------------------------------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -c /mnt/hds/proj/cust003/develop/exomes/0/0_config.yaml

Rerun analysis using exactly the same parameters as last analysis run, but in simulation mode
---------------------------------------------------------------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -c /mnt/hds/proj/cust003/develop/exomes/0/0_config.yaml -dra 2

Generate all supported standard programs
----------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 -pp

This will print a string with programs in mode 2 (expect ouput) in chronological order (as far as possible, some things are processed in parallel):

	.. code-block:: console

	  $ --pGZipFastq 2 --pFastQC 2 --pBwaMem 2 --pPicardToolsMergeSamFiles 2 --pSambambaMarkduplicates 2 --pGATKRealigner 2 --pGATKBaseRecalibration 2 --pChanjoSexCheck 2 --pSambambaDepth 2 --pGenomeCoverageBED 2 --pPicardToolsCollectMultipleMetrics 2 --pPicardToolsCalculateHSMetrics 2 --pRCovPlots 2 --pCNVnator 2 --pDelly 2 --pManta 2 --pFindTranslocations 2 --pCombineStructuralVariantCallSets 2 --pSVVariantEffectPredictor 2 --pSVVCFParser 2 --pSVRankVariants 2 --pSamToolsMpileUp 2 --pFreebayes 2 --pGATKHaploTypeCaller 2 --pGATKGenoTypeGVCFs 2 --pGATKVariantRecalibration 2 --pGATKCombineVariantCallSets 2 --pPrepareForVariantAnnotationBlock 2 --pVT 2 --pGATKPhaseByTransmission 2 --pGATKReadBackedPhasing 2 --pGATKVariantEvalAll 2 --pGATKVariantEvalExome 2 --pVariantEffectPredictor 2 --pVCFParser 2 --pSnpEff 2 --pSampleCheck 2 --pEvaluation 2 --pRankVariants 2 --pQCCollect 2 --pMultiQC 2 --pRemoveRedundantFiles 2 --pAnalysisRunStatus 2 --pSacct 2

Thus you will always have the actual program names that are supported facilitating starting from any step in the analysis for instance updating qc_sampleInfo.yaml and rerunning module in [BAMCalibrationBlock] skipping [Sambamba Markduplicates]:

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 --pGZipFastq 2 --pFastQC 2 --pBwaMem 2 --pPicardToolsMergeSamFiles 2 --pSambambaMarkduplicates 0 --pGATKRealigner 1 --pGATKBaseRecalibration 1 --pChanjoSexCheck 2 --pSambambaDepth 2 --pGenomeCoverageBED 2 --pPicardToolsCollectMultipleMetrics 2 --pPicardToolsCalculateHSMetrics 2 --pRCovPlots 2 --pCNVnator 2 --pDelly 2 --pManta 2 --pFindTranslocations 2 --pCombineStructuralVariantCallSets 2 --pSVVariantEffectPredictor 2 --pSVVCFParser 2 --pSVRankVariants 2 --pSamToolsMpileUp 2 --pFreebayes 2 --pGATKHaploTypeCaller 2 --pGATKGenoTypeGVCFs 2 --pGATKVariantRecalibration 2 --pGATKCombineVariantCallSets 2 --pPrepareForVariantAnnotationBlock 2 --pVT 2 --pGATKPhaseByTransmission 2 --pGATKReadBackedPhasing 2 --pGATKVariantEvalAll 2 --pGATKVariantEvalExome 2 --pVariantEffectPredictor 2 --pVCFParser 2 --pSnpEff 2 --pSampleCheck 2 --pEvaluation 2 --pRankVariants 2 --pQCCollect 2 --pMultiQC 2 --pRemoveRedundantFiles 2 --pAnalysisRunStatus 2 --pSacct 2

You can of course start or skip any number of modules as long as it is sane to do so (MIP will not check this but just execute)

You can also modulate the mode of '-pp' using -ppm:
---------------------------------------------------

	.. code-block:: console

	  $ perl develop/modules/MIP/mip.pl -f 0 -c develop/references/CMMS_Rasta_Config.v1.4.yaml -rio 1 -pp -ppm 1	
	  $ --pGZipFastq 1 --pFastQC 1 --pBwaMem 1 --pPicardToolsMergeSamFiles 1 --pSambambaMarkduplicates 1 --pGATKRealigner 1 --pGATKBaseRecalibration 1 --pChanjoSexCheck 1 --pSambambaDepth 1 --pGenomeCoverageBED 1 --pPicardToolsCollectMultipleMetrics 1 --pPicardToolsCalculateHSMetrics 1 --pRCovPlots 1 --pCNVnator 1 --pDelly 1 --pManta 1 --pFindTranslocations 1 --pCombineStructuralVariantCallSets 1 --pSVVariantEffectPredictor 1 --pSVVCFParser 1 --pSVRankVariants 1 --pSamToolsMpileUp 1 --pFreebayes 1 --pGATKHaploTypeCaller 1 --pGATKGenoTypeGVCFs 1 --pGATKVariantRecalibration 1 --pGATKCombineVariantCallSets 1 --pPrepareForVariantAnnotationBlock 1 --pVT 1 --pGATKPhaseByTransmission 1 --pGATKReadBackedPhasing 1 --pGATKVariantEvalAll 1 --pGATKVariantEvalExome 1 --pVariantEffectPredictor 1 --pVCFParser 1 --pSnpEff 1 --pSampleCheck 1 --pEvaluation 1 --pRankVariants 1 --pQCCollect 1 --pMultiQC 1 --pRemoveRedundantFiles 1 --pAnalysisRunStatus 1 --pSacct 1 
