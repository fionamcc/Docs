===========================
**GOanna on CyVerse**
===========================

.. Coming Soon!::

    InterProScan 5.36-75.0 will be available soon in the Discovery Environment. This app will included the XML parser and run it automatically with InterProScan. There will be no need to use the InterProScan Results Function app.

**Accessing InterProScan in the Discovery Environment**
========================================================

1. `Create an account on CyVerse <user.cyverse.org>`_ (free)
2. Open the CyVerse Discovery Environment (DE) and login with your CyVerse credentials.
3. If you are new to the Discovery Environment (DE) the user guide can be found `here <https://learning.cyverse.org/projects/discovery-environment-guide/en/latest/>`_.

4. Click on the ‘Data’ button at the left side of the screen to access your files/folders. Upload your data to the DE.
5. To access the `InterProScan Sequence Search 5.35-74.0 <https://de.cyverse.org/de/?type=apps&app-id=Interproscan-5.35.74u1&system-id=agave>`_ app click on the ‘Apps’ button at the left side of the DE. 
6. Search for 'interproscan" in the search bar at the top of the ‘apps’ window (see below). The contents of the folder will appear in the main pane of the window. The GOanna app is called ‘InterProScan Sequence Search 5.35-74.0’; click on the name to open the app.

|find_iprs|


**Using the InterProScan App**
==============================
**Launching the App**
---------------------

|iprs|

**InterProScan_Sequence_Search_5.35.74_analysis1:**
This menu is used to name the job you will run so that you can find it later.
Analysis Name: The default name is "InterProScan_Sequence_Search_5.35.74_analysis1". We recommend changing the 'analysis1' portion of this to reflect the data you are running.

**Comments:**
(Optional) You can add additional information in the comments section to distinguish your analyses further.

**Select output folder:**
This is where your results will be placed. The default (recommended) is your 'analyses' folder.

**Retain Inputs:**
Enabling this flag will copy all the input files into the analysis result folder. 

.. WARNING:: 

    Selecting this option will rapidly consume your allocated space. It is not recommended. Your inputs will always remain available in the folder in which you stored them.

**Input**
^^^^^^^^^

**Peptide FASTA file:** Use the Browse button on the right hand side to navigate to your Data folder and select your protein sequence file. 

**Parameters**
^^^^^^^^^^^^^^

**Annotate each peptide with Gene Ontology information:** Be sure this box is checked. This will ensure that you get GO annotations

**Annotate each peptide with biological pathway information:** This is optional. However, if you want pathways annotations be it is checked.

.. Important::

    InterProScan version 5.35-74.0 does not include the XML parser to generate GAF file and associated count files. To get these files the user must run the `InterProScan Results Function <https://de.cyverse.org/de/?type=apps&app-id=714e28fa-6580-44fc-9756-8b019c192449&system-id=de>`_ app.


**Understanding Your Results**
==============================
**InterProScan Outputs**
------------------------

**:** This is standard BLAST output format that allows for conversion to other formats. You probably won’t need to look at this output.

**:** This output displays in your web browser so that you can view pairwise alignments to determine BLAST parameters.

**:** This is the tab-delimited BLAST output that can be opened and sorted in Excel to determine BLAST parameter values. The file contains the following columns:

**:** This is the standard tab-separated `GO annotation file format <http://geneontology.org/docs/go-annotation-file-gaf-format-2.1>`_  that is used by the GO Consortium and by software tools that accept GO annotation files to do GO enrichment. 

**Using the InterProScan App**
==============================
**Launching the App**
---------------------

|resultsfunc|

**InterProScan_Results_Function_analysis1:**
This menu is used to name the job you will run so that you can find it later.
Analysis Name: The default name is "InterProScan_Results_Function_analysis1". We recommend changing the 'analysis1' portion of this to reflect the data you are running.

**Comments:**
(Optional) You can add additional information in the comments section to distinguish your analyses further.

**Select output folder:**
This is where your results will be placed. The default (recommended) is your 'analyses' folder.

**Retain Inputs:**
Enabling this flag will copy all the input files into the analysis result folder. 

.. WARNING:: 

    Selecting this option will rapidly consume your allocated space. It is not recommended. Your inputs will always remain available in the folder in which you stored them.

**Input Parameters**
^^^^^^^^^^^^^^^^^^^^^

**InterProScan XML Results File:** Here you should use the 'browse' button on the right side of the field to navigate to the XML output file from your InterProScan analysis.

**Database:** Use the database that sequences were obtained from (Genbank), or a recognizable project name if these sequences are not in a database (e.g., i5k project or Smith Lab).

**Sequence:** Type of sequence in your original FASTA file. For this pipeline you should always select protein.

**Taxon Number:** Enter the NCBI taxon number for your species. This can be found by searching for your species name (common or scientific) in the `NCBI taxon database <https://www.ncbi.nlm.nih.gov/taxonomy>`_.

**Biocurator Name (Assigned by):** Enter your name. This field is used to track who made the annotations.

**InterProScan Results Function Outputs**
------------------------------------------

If you output doesn't look like you expect please check the 'condor_stderr' file in the analysis output 'logs' folder. If that doesn't clarify the problem contact us at agbase@email.arizona.edu or support@cyverse.org.

.. |find_iprs| image:: ../img/find_iprs.png
  :width: 700

.. |iprs| image:: ../img/iprs.png
  :width: 700

.. |resultsfunc| image:: ../img/resultsfunc.png
  :width: 700