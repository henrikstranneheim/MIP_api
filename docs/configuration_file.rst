Dynamic Configuration File
==========================

MIP uses dynamic configuration files in YAML format to load parameters for each analysis run. 
An example configuration file can be found `here`_.

To facilitate using different clusters, projects and tailoring the MIP analysis to each run without having to 
create new configuration files each time you can supply a cluster/project specifc configuration file. Certain paths 
in the configuration file information will be updated to the current analysis when MIP executes.

This requires that these two entries are added to the configuration file:

1. 'clusterConstantPath: {value}', specifying the project path.
2. 'analysisConstantPath: {value}', specifying the analysis directory.

Entries in the configuration file containing the following "dynamic strings" will be updated in MIP:

  * CLUSTERCONSTANTPATH! = 'clusterConstantPath: {value}'
  * ANALYSISCONSTANTPATH! = 'analysisConstantPath: {value}'
  * ANALYSISTYPE! = 'analysisType: {value}'
  * FDN! = '-f familyID' (from command line)

For instance, the pedigree file entry in the configuration file can be supplied like this:

'pedigreeFile: CLUSTERCONSTANTPATH!/ANALYSISTYPE!/FDN!/FDN!_pedigree.txt'

and each path element will be replaced with the corresponding value as specified in the 
configuration file, command line (precedence) or pedigree file. 

.. note::

  Any entries not containing "dynamic strings" will not be modified by MIP. 
  
  Both updated and constant entries will be written to the analysis specific folder if specified by
  '-wc'. 

Capture kits info supplied in the configuration file should be on sampleID level:

.. code-block:: yaml

	FDN:
	  IDN:
	    Flag: Entry

.. _here: https://github.com/henrikstranneheim/MIP/tree/master/templates