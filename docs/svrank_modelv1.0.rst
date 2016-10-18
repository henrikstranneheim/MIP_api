svrank_modelv1.0
================

Genmod score uses the `weighted sum model`_ (WSM) approach to rank the most likely
pathogenic variant.

Generally, the higher value the more likely pathogenic variant. 

Genmod_score uses config files to define the rank model, which enables customized
set-up and versioning of rank models.

The WSM uses the following alternatives and weights in the rank model:

Rank score range: -33 <= rs <= 31

`Consequence`_
~~~~~~~~~~~~~~
Each alleles variant effect on individual transcripts are evaluated using a rule-based approach
defined by `SO-terms`_. The SO-terms themselves are ranked in order of severity and this ranking
is used to defined the weight of the consequence alternative. The performance score is based
on the most severe consequence within each gene.

Performance value for the SO-terms:
 - transcript_ablation = 10
 - initiator_codon_variant = 9
 - frameshift_variant = 8
 - stop_gained = 8
 - start_lost = 8
 - stop_lost = 8
 - splice_acceptor_variant = 8
 - splice_donor_variant = 8
 - inframe_deletion = 5
 - transcript_amplification = 5
 - splice_region_variant = 5
 - missense_variant = 5
 - protein_altering_variant = 5
 - inframe_insertion = 5
 - incomplete_terminal_codon_variant = 5
 - non_coding_transcript_exon_variant = 3
 - synonymous_variant = 2
 - mature_miRNA_variant = 1
 - non_coding_transcript_variant = 1
 - regulatory_region_variant = 1
 - upstream_gene_variant = 1
 - regulatory_region_amplification = 1
 - TFBS_amplification = 1
 - 5_prime_UTR_variant = 1
 - intron_variant = 1
 - 3_prime_UTR_variant = 1
 - feature_truncation = 1
 - TF_binding_site_variant = 1
 - stop_retained_variant = 1
 - feature_elongation = 1
 - regulatory_region_ablation = 1
 - TFBS_ablation = 1
 - coding_sequence_variant = 1
 - downstream_gene_variant = 1
 - NMD_transcript_variant = 1
 - intergenic_variant = 0
 - not_reported = 0

Frequency
~~~~~~~~~
The alternative allele frequency (AF) in public databases (`1000G`_). The highest reported 
alternative frequency reported from the database is used to calculate the performance value.

Definitions:
 - Not reported: AF Na
 - Very Rare: AF < 0.0005
 - Rare: 0.0005 <= AF < 0.005
 - Intermediate: 0.005 <= AF < 0.02
 - Common:  AF >= 0.02

Performance value for maximum AF:
 - Not reported = 4
 - Very rare = 3
 - Rare = 2
 - Intermediate = 1
 - Common = -12

For *imprecise* variants each confidence interval (SVR1000G and SVL1000G) around the breakpoints are scored:

Definitions:
 - Not reported: AF Na
 - Rare: 0 <= AF < 0.005
 - Intermediate: 0.005 <= AF < 0.02
 - Common:  AF >= 0.02

Performance value for maximum AF:
 - Not reported = 4
 - Rare = 2
 - Intermediate = 1
 - Common = -12

SV Type
~~~~~~~
PRECISE variants are scored higher than IMPRECISE variants.

Definitions:
 - Precise

Performance value for maximum AF:
 - Precise = 3

SV Length
~~~~~~~~~
Length of the structural variant (SVLen). Shorter SVs are scored higher.

Definitions:
 - Not reported
 - Long: 1000001 <= SVLen <= 100000000
 - Medium: 50001 <= SVLen <= 1000000
 - Short: 1 <= SVLen <= 50000

Performance value for SVLen:
 - Not reported = 0
 - Long = -3
 - Medium = 3
 - Short = 8

Gene Intolerance Score
~~~~~~~~~~~~~~~~~~~~~~
EXAC gene intolerance score - calculated by VEP's LoFtool plugin.

Definitions:
 - Not reported: LoFtool Na
 - Low: LoFtool < 0.0001
 - Medium: 0.0001 <= LoFtool < 0.01
 - High LoFtool < 0.01

Performance value for gene intolerance score:
 - Not reported = 0
 - Low = 2
 - Medium = 1
 - High = 0

Inheritance Model(s)
~~~~~~~~~~~~~~~~~~~~
The segregation pattern for the variant within the family. These models are currently annotated
using `genmod`_ models. A variant that is annotated as autosomal compound with no compound partner 
with a rank score greater than 10 will receive a penalty of -6 to the variants rank score.
For single samples this rule will be enforced for variants with inheritance model autosomal dominant,
autosomal dominant denovo in addition to the autosomal compound annotation.

Definitions:
 - Autosomal Recessive, denoted 'AR_hom'
 - Autosomal Recessive denovo, denoted 'AR_hom_dn'
 - Autosomal Dominant, 'AD'
 - Autosomal Dominant denovo, 'AD_dn'
 - Autosomal Compound Heterozygote, 'AR_comp'
 - X-linked dominant, 'XD'
 - X-linked dominant de novo, 'XD_dn'
 - X-linked Recessive, 'XR'
 - X-linked Recessive de novo, 'XR_dn'

Performance value for inheritance models:
 - Valid model = 1
 - No model = -12
 - AR_comp penalty = -6
 
Variant Quality Filter
~~~~~~~~~~~~~~~~~~~~~~
Each variant call has a filter tranche attached to it indicating the quality of the actual
variant call.

Definitions:
 
 - PASS 
 - Other (Tranches e.g. For GATK [`3`_]: "VQSRTrancheBOTH99.90to100.00"

We also evaluate the combined GQ score called a Model score for reducing the impact of poor
quality genotypes across a case.

Definitions:

 - Low quality (GQ => 20)
 - High quality (GQ > 20)

Performance value for variant quality filter:

- Filter tranche:

   - PASS = 3
   - Other = 0

- Model score:
   
   - Low quality = -5
   - High quality = 0
 
 
.. _weighted sum model: http://en.wikipedia.org/wiki/Weighted_sum_model
.. _Consequence: http://www.ensembl.org/info/genome/variation/predicted_data.html
.. _SO-terms: http://www.sequenceontology.org/
.. _1000G: http://www.1000genomes.org/
.. _genmod: https://github.com/moonso/genmod
.. _1: http://www.ncbi.nlm.nih.gov/pubmed/?term=22689647
.. _2: http://www.ncbi.nlm.nih.gov/pubmed/?term=20354512
.. _3: http://www.ncbi.nlm.nih.gov/pubmed?term=20644199
.. _4: http://www.ncbi.nlm.nih.gov/pubmed/?term=16024819
.. _5: http://www.ncbi.nlm.nih.gov/pubmed/?term=14660683
.. _6: http://www.ncbi.nlm.nih.gov/pubmed/?term=21278375
.. _7: http://www.ncbi.nlm.nih.gov/pubmed/?term=15965027
.. _8: http://www.ncbi.nlm.nih.gov/pubmed/?term=24487276
.. _9: http://www.ncbi.nlm.nih.gov/pubmed/?term=24234437