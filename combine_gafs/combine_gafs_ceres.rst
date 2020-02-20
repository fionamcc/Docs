=====================================
**Combine GAFs on the ARS Ceres HPC**
=====================================

**About Ceres/Scinet**
===============================
- The Scinet VRSC has installed combine_gafs for ARS use.
- For general information on Scinet/Ceres, how to access it, and how to use it, visit `https://usda-ars-gbru.github.io/scinet-site/ <https://usda-ars-gbru.github.io/scinet-site/>`_.

**Running GOanna on Ceres**
===========================
.. admonition:: Running programs on Ceres/Scinet

    - You'll need to run combine_gafs either in interactive mode or batch mode.
    - For interactive mode, use the `salloc` command.
    - For batch mode, you'll need to write a batch job submission bash script.

**Running combine_gafs in interactive mode**
--------------------------------------------

**Loading the module**
^^^^^^^^^^^^^^^^^^^^^^

The Scinet VRSC has installed the combine_gafs program. To load the module in interactive mode, run the command

.. code-block:: bash

    module load agbase


**Running Combine GAFs**
^^^^^^^^^^^^^^^^^^^^^^^^

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

   combine_gafs -i CFLO_1.fa_gaf.txt -g clfo1_v_insecta_goanna_gaf.tsv -o complete_gaf 


**Command Explained**
""""""""""""""""""""""""

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added 

**Running combine_gafs in batch mode**
--------------------------------------
.. admonition:: Running programs on Ceres/Scinet in batch mode

    - Before using batch mode, you should review Scinet/Ceres' documentation first, and decide what queue you'll want to use. See `https://usda-ars-gbru.github.io/scinet-site/guide/ceres/ <https://usda-ars-gbru.github.io/scinet-site/guide/ceres/>`_.

**Example batch job submission bash script (e.g. combine_gafs-job.sh):**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #! /bin/bash
    module load agbase
    combine_gafs -i CFLO_1.fa_gaf.txt -g clfo1_v_insecta_goanna_gaf.tsv -o complete_gaf

**Submitting the batch job:**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: bash

    sbatch combine_gafs-job.sh

**Command Explained**
""""""""""""""""""""""""

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added