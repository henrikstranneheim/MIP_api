The Code
========

Subroutines
-----------

FIDSubmitJob
~~~~~~~~~~~~
Handles all communication with SLURM. All jobIDs and SLURM dependencies for all programs 
are set and submitted here. Each program in MIP belongs to a "path" and together with the 
sampleID and/or familyID creates a chain of dependencies determining the execution order in SLURM. 


**Paths**

The central flow in MIP is called the *MAIN* path. MIP supports branching from the MAIN 
path for both familyIDs and sampleIDs. It is also possible to create completely separate 
paths, which are not associated at all with the MAIN path. However, once branched of from 
the MAIN path there is currently no support to merge the branch to the trunk (i.e. MAIN path) again. 

Each program is supplied with a dependency flag, which determines its dependencies in the path.

**Dependency Flags**::

 -1 = Not dependent on earlier scripts, and are self cul-de-sâcs
 0 = Not dependent on earlier scripts
 1 = Dependent on earlier scripts (within sampleID_path or familyID_path)
 2 = Dependent on earlier scripts (within sampleID_path or familyID_path), but are self cul-de-sâcs. 
 3 = Dependent on earlier scripts and executed in parallel within step
 4 = Dependent on earlier scripts and parallel scripts and executed in parallel within step 
 5 = Dependent on earlier scripts both family and sample and adds to both familyID and sampleId jobs
 
**Hash**

All jobIDs are saved to the jobID hash using ``$jobID{FAMILYID_PATH}{CHAINKEY}``. Only the 
last jobID(s) required to set the downstrem dependencies are saved.  