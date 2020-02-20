======================================
**GOanna on the ARS Ceres HPC**
======================================

**About Ceres/Scinet**
===============================
- The Scinet VRSC has installed GOanna for ARS use.
- For general information on Scinet/Ceres, how to access it, and how to use it, visit `https://usda-ars-gbru.github.io/scinet-site/ <https://usda-ars-gbru.github.io/scinet-site/>`_.

**Accessing the databases**
===========================
GOanna requires access to some public databases that are already available on Ceres. These need to be in your working directory when you run the program. The best way to set this up is to create symbolic links to the databases from your working directory.

1) agbase_database: species subset to run BLAST against

.. code-block:: bash

    ln -s /reference/data/iplant/2019-09-16/agbase_database


2) go_info: Uniprot GO annotations

.. code-block:: bash

    ln -s /reference/data/iplant/2019-09-16/go_info


**Running GOanna on Ceres**
===========================
.. admonition:: Running programs on Ceres/Scinet

    - You'll need to run GOanna either in interactive mode or batch mode.
    - For interactive mode, use the `salloc` command.
    - For batch mode, you'll need to write a batch job submission bash script.

**Running GOanna in interactive mode**
--------------------------------------

**Loading the module**
^^^^^^^^^^^^^^^^^^^^^^

The Scinet VRSC has installed the GOanna module. To load the module in interactive mode, run the command

.. code-block:: bash

    module load agbase


**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    goanna -h

See :ref:`goannausage`

GOanna has three required parameters:

.. code-block:: bash

    -a BLAST database basename (acceptable options are listed in the help/usage)
    -c peptide FASTA file to BLAST
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

   goanna -c AROS_10.faa -a invertebrates -o goanna_output -p -g 70 -s 900 -d RefSeq -u Monica -x 37344

**Running GOanna in batch mode**
--------------------------------

.. admonition:: Running programs on Ceres/Scinet in batch mode

    - Before using batch mode, you should review Scinet/Ceres' documentation first, and decide what queue you'll want to use. See `https://usda-ars-gbru.github.io/scinet-site/guide/ceres/ <https://usda-ars-gbru.github.io/scinet-site/guide/ceres/>`_.

**Example batch job submission bash script (e.g. goanna-job.sh):**

.. code-block:: bash

    #! /bin/bash
    module load agbase
    goanna -c AROS_10.faa -a invertebrates -o goanna_output -p -g 70 -s 900 -d RefSeq -u Monica -x 37344

**Submitting the batch job:**

.. code-block:: bash

    sbatch goanna-job.sh

**GOanna Commands Explained**
-----------------------------

**-a invertebrates:** GOanna BLAST database to use--first of three required options.

**-c AROS_10.faa:** input file (peptide FASTA)--second of three required options

**-o goanna_output:** output file basename--last of three required options

**-p:** our input file has NCBI deflines. This specifies how to parse them.

**-g 70:** tells GOanna to keep only those matches with at least 70% identity

**-s 900:** tells GOanna to keep only those matches with a bitscore above 900

**-d RefSeq:** database of query ID. This will appear in column 1 of the GAF output file.

**-u "Monica":** name to appear in column 15 of the GAF output file

**-x 37344:** NCBI taxon ID of input file species will appear in column 13 of the GAF output file


**Understanding Your Results**
------------------------------

If all goes well, you should get 4 output files:

**<basename>.asn:** This is standard BLAST output format that allows for conversion to other formats. You probably won’t need to look at this output.

**<basename>.html:** This output displays in your web browser so that you can view pairwise alignments to determine BLAST parameters.

**<basename>.tsv:** This is the tab-delimited BLAST output that can be opened and sorted in Excel to determine BLAST parameter values. The file contains the following columns:

- Query ID
- query length
- query start<
- query end
- subject ID
- subject length
- subject start
- subject end
- e-value
- percent ID
- query coverage
- percent positive ID
- gap openings
- total gaps
- bitscore
- raw score

For more information on the BLAST output parameters see the `NCBI BLAST documentation <https://www.ncbi.nlm.nih.gov/books/NBK279684/#_appendices_Options_for_the_commandline_a_.>`_.

**<basename>_goanna_gaf.tsv:** This is the standard tab-separated `GO annotation file format <http://geneontology.org/docs/go-annotation-file-gaf-format-2.1>`_  that is used by the GO Consortium and by software tools that accept GO annotation files to do GO enrichment.

If you see more files in your output folder there may have been an error in the analysis or there may have been no GO to transfer. `Contact us <agbase@email.arizona.edu>`_.
