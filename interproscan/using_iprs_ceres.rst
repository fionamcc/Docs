============================================
**InterProScan on the ARS Ceres HPC**
============================================

**About Ceres/Scinet**
===============================
- The Scinet VRSC has installed InterProScan for ARS use.
- For general information on Ceres/Scinet, how to access it, and how to use it, visit `https://usda-ars-gbru.github.io/scinet-site/ <https://usda-ars-gbru.github.io/scinet-site/>`_.


**Running InterProScan on Ceres/Scinet**
========================================

.. Important::

    - InterProScan requires quite a lot of compute resources and should be run in batch mode.
    - Before using batch mode, you should review Ceres/Scinet's documentation first, and decide what queue you'll want to use. See `https://usda-ars-gbru.github.io/scinet-site/guide/ceres/ <https://usda-ars-gbru.github.io/scinet-site/guide/ceres/>`_.


**Running InterProScan with Data**
----------------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    agbase_interproscan -h

See :ref:`iprsusage`

**Example batch job submission bash script (e.g. agbase_interproscan-job.sh):**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #! /bin/bash
    module load agbase
    agbase_interproscan -i AROS_10.faa -d outdir -f tsv,json,xml,html,gff3,svg -g -p -n Monica -x 109069 -D i5k


**Submitting the batch job:**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sbatch agbase_interproscan-job.sh


**Command Explained**
""""""""""""""""""""""""

**-i AROS_10.faa:** local path to input FASTA file. 


**-d outdir:** output directory name


**-f tsv,json,xml,html,gff3,svg:** desired output file formats


**-g:** tells the tool to perform GO annotation 


**-p:** tells tool to perform pathway annoation


**-n Monica:** name of biocurator to include in column 15 of GAF output file


**-x 109069:** taxon ID of query species to be used in column 13 of GAF output file

**-D i5k:** database of query accession to be used in column 1 of GAF output file


**Understanding Your Results**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
**InterProScan outputs:** https://github.com/ebi-pf-team/interproscan/wiki/OutputFormats
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Default**
- <basename>.gff3
- <basename>.tsv
- <basename>.xml

**Optional**
- <basename>.json
- <basename>.html.tar.gz
- <basename>.svg.tar.gz

**Parser Outputs**
""""""""""""""""""
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

If you see more files in your output folder there may have been an error in the analysis or there may have been no GO to transfer. `Contact us <agbase@email.arizona.edu>`_.
