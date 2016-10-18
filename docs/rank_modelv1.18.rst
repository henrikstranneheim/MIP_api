rank_modelv1.18
===============

Genmod score uses the `weighted sum model`_ (WSM) approach to rank the most likely
pathogenic variant.

Generally, the higher value the more likely pathogenic variant. 

Genmod_score uses config files to define the rank model, which enables customized
set-up and versioning of rank models.

The WSM uses the following alternatives and weights in the rank model:

Rank score range: -33 <= rs <= 44

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
The alternative allele frequency (AF) in public databases (`1000G`_, `ExAC`_, MTAF, SWEREF). The highest reported 
alternative frequency and observation count (locusDB) reported from the databases is used to calculate the performance value.

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

**Observation Count**

The observation count (Obs) from the local variant database locusDB.

Definitions:
 - Not reported: Obs Na
 - Very Rare: Obs < 5
 - Rare: 5 <= Obs < 10
 - Intermediate: 10 <= Obs < 20
 - Common:  Obs >= 20

Performance value for maximum Obs:
 - Not reported = 4
 - Very Rare = 3
 - Rare = 2
 - Intermediate = 1
 - Common = -12

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

Protein Functional Prediction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The predicted functional effect on the protein.
Currently 2 protein effect predictors are used (`Sift`_, `PolyPhen2`_).
Each predictors can contribute 1 point each to the overall protein predictor performance score.

SIFT predicts whether an amino acid substitution is likely to affect protein function based
on sequence homology and the physico-chemical similarity between the alternate amino acids [`1`_].

PolyPhen-2 predicts the effect of an amino acid substitution on the structure and function
of a protein using sequence homology, Pfam annotations, 3D structures from PDB where available,
and a number of other databases and tools (including DSSP, ncoils etc [`2`_].

Definitions:

 - Sift Terms:
 
   - "D" Deleterious (score<=0.05)
   - "T" Tolerated (score>0.05) 

 - `PolyPhen2HumVar`_ Terms:

   - "D": Probably damaging (>=0.909)
   - "P": Possibly damaging (0.447<=pp2_hvar<=0.909)
   - "B": Benign (pp2_hvar<=0.446)

Performance value for protein predictors:

 - Sift:
   
   - D = 1

 - PolyPhen2HumVar:
   
   - D or P = 1

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

Conservation
~~~~~~~~~~~~
The level of conservation for a sequence element (`PhastCons`_ [`4`_]), nucleotides or classes of 
nucleotides `PhyloP`_ [`5`_] both from the `Phast`_ [`6`_] package as well as genomic constraint score
`GERP`_ [`7`_] is used. The Phast datasets used in the conservation calculation were generated
by the UCSC/Penn State Bioinformatics comparative genomics alignment pipeline. A description of this analysis can be found at `UCSC`_. Each type of 
conservation can contribute 1 point each to the overall conservation performance score.

Definitions:

 - Conserved
 
   - PhastCons: 0.8 >= Score <= 1
   - GERPRS: Score >= 2
   - PhyloP: Score > 2,5
   
Performance value for conservation:
 - Conserved:
 
   - PhastCons100way_vertebrate = 1
   - PhyloP100way_vertebrate = 1	
   - GERP++RS = 1

Combined Annotation Dependent Depletion (CADD)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
`CADD`_ is a tool for scoring the deleteriousness of single nucleotide variants as well as 
insertion/deletions variants in the human genome. C-scores strongly correlate with allelic
diversity, pathogenicity of both coding and non-coding variants, and experimentally measured
regulatory effects, and also highly rank causal variants within individual genome sequences.
The CADD-score is a pre-calculated for all SNVs and for indel from 1000G-project [`8`_].
 
Definitions:

- Strongly deleterious (CADD > 40)
- deleterious (40 >= CADD > 30)
- Mildly deleterious (30 >= CADD > 20)
- Probably deleterious (20 >= CADD > 10)
- Benign (10 >= CADD >= 0)

Performance value for CADD:

- Strongly deleterious = 5
- Deleterious = 4
- Mildly deleterious = 3
- Probably deleterious = 2
- Benign = 0


ClinVar
~~~~~~~
`ClinVar`_ [`9`_] is a freely accessible, public archive of reports of the relationships
among human variations and phenotypes, with supporting evidence. Each variant in clinvar has a record of 
clinical significance (CLNSIG):

Definitions:

 - Uncertain significance = 0
 - Not provided = 1
 - Benign = 2
 - Likely benign = 3
 - Likely pathogenic = 4
 - Pathogenic = 5
 - Drug response = 6
 - Histocompatibility = 7
 - Other = 255

Performance value for ClinVar:
 - Uncertain significance = 0
 - Not provided = 0
 - Benign = -1
 - Likely benign = 0
 - Likely pathogenic = 2
 - Pathogenic = 5
 - Drug response = 0
 - Histocompatibility = 0
 - Other = 0
 
 **Clinical review status**

 Clinical review status (CLNREVSTAT) is a measure on the certainty of the supporting evidence for the variant's
 clinical significance.
 
 Definitions:
 
 - not_reported
 - no_assertion
 - no_criteria
 - single
 - mult
 - conf
 - exp
 - guideline
 
 Performance value for CLNREVSTAT:
 
 - not_reported = 0
 - no_assertion = 0
 - no_criteria = 0
 - single = 1
 - conf = 1
 - mult = 2
 - exp = 3
 - guideline = 4
 
Spidex
~~~~~~~
Spidex is a database for snvs that have been predicted to affect splicing. 

Definitions:

 - Not reported = 0
 - Low (-1 < dpsi < 1)
 - Medium (-1 <= dpsi > 1 & -2 > dpsi < 2)
 - High (-2 <= dpsi >= 2)
 
Performance value for Spidex:

 - Low = 0
 - Medium = 3
 - High = 5
 
 
.. _weighted sum model: http://en.wikipedia.org/wiki/Weighted_sum_model
.. _Consequence: http://www.ensembl.org/info/genome/variation/predicted_data.html
.. _SO-terms: http://www.sequenceontology.org/
.. _1000G: http://www.1000genomes.org/
.. _ExAC: http://exac.broadinstitute.org/about
.. _MutationTaster: http://www.mutationtaster.org/
.. _genmod: https://github.com/moonso/genmod
.. _Sift: http://sift.jcvi.org/
.. _PolyPhen2: http://genetics.bwh.harvard.edu/pph2/
.. _PolyPhen2HumVar: http://genetics.bwh.harvard.edu/pph2/dokuwiki/overview#prediction
.. _PhastCons: http://compgen.bscb.cornell.edu/phast/help-pages/phastCons.txt
.. _PhyloP: http://compgen.bscb.cornell.edu/phast/help-pages/phyloP.txt
.. _Phast: http://compgen.bscb.cornell.edu/phast/
.. _UCSC: http://genome.ucsc.edu/cgi-bin/hgTrackUi?db=hg19&g=cons100way
.. _GERP: http://mendel.stanford.edu/SidowLab/downloads/gerp/
.. _CADD: http://cadd.gs.washington.edu/
.. _ClinVar: http://www.ncbi.nlm.nih.gov/clinvar/
.. _1: http://www.ncbi.nlm.nih.gov/pubmed/?term=22689647
.. _2: http://www.ncbi.nlm.nih.gov/pubmed/?term=20354512
.. _3: http://www.ncbi.nlm.nih.gov/pubmed?term=20644199
.. _4: http://www.ncbi.nlm.nih.gov/pubmed/?term=16024819
.. _5: http://www.ncbi.nlm.nih.gov/pubmed/?term=14660683
.. _6: http://www.ncbi.nlm.nih.gov/pubmed/?term=21278375
.. _7: http://www.ncbi.nlm.nih.gov/pubmed/?term=15965027
.. _8: http://www.ncbi.nlm.nih.gov/pubmed/?term=24487276
.. _9: http://www.ncbi.nlm.nih.gov/pubmed/?term=24234437