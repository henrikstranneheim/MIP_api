Individual Identification Number (IDN)
======================================

Ensure that each individual are anonymized and unique. The IDN also facilitates tracking 
of operations and analyses performed upon the individual.

.. note::
 Changes to the IDN format will be recorded in this document.

IDN Definition
-------------- 
The IDN consists of a three digits connected by a dash and a disease status (DS) letter after
the last digit. The disease status letter can be either an “A” denoting affected subjects,
a “U” denoting unaffected subjects or "X" for unknown phenotype. Each subject can only have 1 IDN
and once set it should never be changed.

- The first digit represents the family identification number (FamilyID/FDN). The two first numbers,
  in the first digit, represent the year that the first individual of the family was submitted 
  to massively parallel sequencing, MPS. The attached year is determined by the first individual 
  to be submitted to MPS and this year should then be used for all family members irrespective 
  of how many times a sample or individual is resequenced. This rule is enforced to not create 
  multiple IDNs for the same individual and to make sure that all family members are grouped 
  and analyzed in the proper family. The following three numbers are the a continuous number 
  for each family that has been submitted for the year.
- The second digit represents the generation identification number (GenerationID/GDN) within 
  that family. The generation with the affected child is the defined as GDN = "I". Older 
  generation are numbered in ascending order from GDN I, starting with II. Younger generations
  are numbered in descending order from GDN I starting with "N" (=0 in Roman numerals). 
- The last digit represents the subject identification number (SubjectID/SDN) within the 
  family and generation. Male subjects will have odd SDN numbers and female subjects even 
  numbers. The lowest subject IDs will be given to oldest subject within each family and 
  generation and then in ascending order (both even and odd numbers are counted). However, 
  since there can be later additions in the pedigree this is not strictly enforced.
- The letter after the SDN is the disease status (DS) letter, which can be either of three 
  possible letters. A = affected, U = unaffected and X = unknown.

Example
~~~~~~~
FamilyID.GenerationID.SubjectID(DS) or FDN.GDN.SDN(DS)

A child in the affected child generation being the second oldest male sibling in family 1 
and the first to be submitted to sequencing within the family in 1998 would be written as: 98001-I-3A (Figure 1). 

.. image:: IDN_figur2.png
    :width: 600px
    :align: center
    :height: 500px