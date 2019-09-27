===========================
**InterProScan on CyVerse**
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
6. Search for 'interproscan" in the search bar at the top of the ‘apps’ window (see below). The contents of the folder will appear in the main pane of the window. The InterProScan app is called ‘InterProScan Sequence Search 5.35-74.0’; click on the name to open the app.

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


**Using the InterProScan Results Function App**
===============================================
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

**Understanding Your Results**
==============================
**InterProScan Outputs**
------------------------

**:** 

**:** 

**:** 



**InterProScan Results Function Outputs**
------------------------------------------
**<basename>_gaf.txt:**
-This table follows the formatting of a gene association file (gaf) and can be used in GO enrichment analyses.
 
**<basename>_acc_go_counts.txt:**
-This table includes input accessions, the number of GO IDs assigned to each accession and GO ID names. GO IDs are split into BP (Biological Process), MF (Molecular Function) and CC (Cellular Component).

**<basename>_go_counts.txt:**
-This table counts the numbers of sequences assigned to each GO ID so that the user can quickly identify all genes assigned to a particular function.

**<basename>_acc_interpro_counts.txt:**
-This table includes input accessions, number of InterPro IDs for each accession, InterPro IDs assigned to each sequence and the InterPro ID name.

**<basename>_interpro_counts.txt:**
-This table counts the numbers of sequences assigned to each InterPro ID so that the user can quickly identify all genes with a particular motif. 

**<basename>_acc_pathway_counts.txt:**
-This table includes input accessions, number of pathway IDs for the accession and the pathway names. Multiple values are separated by a semi-colon.

**<basename>_pathway_counts.txt:**
-This table counts the numbers of sequences assigned to each Pathway ID so that the user can quickly identify all genes assigned to a pathway.

**<basename>.err:**
-This file will list any sequences that were not able to be analyzed by InterProScan. Examples of sequences that will cause an error are sequences with a large run of Xs.



If you output doesn't look like you expect please check the 'condor_stderr' file in the analysis output 'logs' folder. If that doesn't clarify the problem contact us at agbase@email.arizona.edu or support@cyverse.org.

.. |find_iprs| image:: ../img/find_iprs.png
  :width: 700

.. |iprs| image:: ../img/iprs.png
  :width: 700

.. |resultsfunc| image:: ../img/resultsfunc.png
  :width: 700