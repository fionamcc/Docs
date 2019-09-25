======================================
**KOBAS on the Command Line**
======================================


**Getting the databases**
==========================
To run the tool you need some public data. The files can be downloaded directly from the `KOBAS homepage <kobas.cbi.pku.edu.cn>`_. These directories are also available as two tar archives in the CyVerse Data Store. The files are best downloaded with `iCommands <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html>`_. Once iCommands is `setup <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html#icommands-first-time-configuration>`_ you can use ‘iget’ to download the data.


1) seq_pep.tar: species-specific BLAST databases used by KOBAS

.. code-block:: bash

    iget /iplant/home/shared/iplantcollaborative/protein_blast_dbs/kobas/seq_pep.tar
    
    tar -xf seq_pep.tar


2) sqlite3.tar: species-specific annotation databases used by KOBAS

.. code-block:: bash

    iget /iplant/home/shared/iplantcollaborative/protein_blast_dbs/kobas/sqlite3.tar
    
    tar -xf sqlite3.tar

.. NOTE::

    The above commands should result in two directories (seq_pep and sqlite3) each containing many files. There is no need to unzip the .gz files.


**Container Technologies**
===========================
KOBAS is provided as a Docker container. 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity**. 

Docker containers can be run with either technology.

**Running KOBAS using Docker**
==============================
.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine (such as CyVerse Atmosphere) etc. Docker can be installed quickly on an Atmosphere instance by typing ‘ezd’.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://learning.cyverse.org/projects/container_camp_workshop_2019/en/latest/docker/dockerintro.html>`_.


**Getting the KOBAS container**
-------------------------------
The KOBAS tool is available as a Docker container on Docker Hub: 
`KOBAS container <https://hub.docker.com/r/agbase/kobas>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    docker pull agbase/kobas:3.0.3_0

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/kobas:3.0.3_0


**Running KOBAS with Data**
---------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm -v $(pwd):/work-dir agbase/kobas:3.0.3_0 -h

See :ref:`kobasusage`

.. tip::

    There are 3 directories built into this container. These directories should be used to mount data.
     - /work-dir
     - /seq_pep
     - /sqlite3

KOBAS can perform two tasks:
- annotate (-a)
- identify (enrichment) (-g)
KOBAS can also run both task with a single command (-j).

**Annotate Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v /home/amcooksey/i5k/seq_pep:/seq_pep \
    -v /home/amcooksey/i5k/sqlite3:/sqlite3 \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_0 \
    -a 
    -i AROS1000.fa \
    -s dme \
    -t fasta:pro
    -o AROS1000

**Command Explained**
""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v /home/amcooksey/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-v /home/amcooksey/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/kobas:3.0.3_0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-a:** Tells KOBAS to runt he 'annotate' process.

**-i AROS1000.fa:** input file (peptide FASTA)

**-s dme:** Enter the species for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o AROS1000:** name of output file

For information on output files see :ref:`Understanding Your Results: Annotate <annotateresults>`

**Identify Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v /home/amcooksey/i5k/seq_pep:/seq_pep \
    -v /home/amcooksey/i5k/sqlite3:/sqlite3 \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_0 \
    -g \
    -f AROS1000 \
    -b dme \
    -o ident_out

**Command Explained**
"""""""""""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v /home/amcooksey/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-v /home/amcooksey/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/kobas:3.0.3_0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-g:** Tells KOBAS to runt he 'identify' process.

**-f AROS1000:** output file from KOBAS annotate

**-b dme:** background; enter the species code for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-o ident_out:** name of output file

For information of outputs see :ref:`Understanding Your Results: Identify <identifyresults>`


**Annotate and Identify Pipeline Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v /home/amcooksey/i5k/seq_pep:/seq_pep \
    -v /home/amcooksey/i5k/sqlite3:/sqlite3 \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_0 \
    -j 
    -i AROS1000.fa \
    -s dme \
    -t fasta:pro
    -o AROS1000

**Command Explained**
"""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v /home/amcooksey/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-v /home/amcooksey/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/kobas:3.0.3_0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-j:** Tells KOBAS to runt he 'annotate' process.

**-i AROS1000.fa:** input file (peptide FASTA)

**-s dme:** Enter the species for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o AROS1000:** basename of output files

.. NOTE::

    This pipeline will automatically use the output of 'annotate' as the -f foreground input for 'identify. 
    This will also use your species option as the -b background input for 'identify'.

For more information on outputs see :ref:`Understanding Your Results: Annotate and Identify <annoident>`

**Running KOBAS using Singularity**
============================================
.. admonition:: About Singularity

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where is is installed
    - more information about `installing Singularity <https://singularity.lbl.gov/docs-installation>`_
    - This tool was tested using Singularity 3.0. Users with Singularity 2.x will need to modify the commands accordingly.


.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a PBSPro system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the KOBAS container**
-------------------------------
The KOBAS tool is available as a Docker container on Docker Hub: 
`KOBAS container <https://hub.docker.com/r/agbase/kobas>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    singularity pull docker://agbase/kobas:3.0.3_0



**Running KOBAS with Data**
----------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
**Example PBS script:**

.. code-block:: bash

    #!/bin/bash
    #PBS -N kobas
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /rsgrps/shaneburgess/amanda/i5k/kobas
    
    singularity pull docker://agbase/kobas:3.0.3_0
    
    singularity run \
    kobas_3.0.3_0.sif \
    -h


See :ref:`kobasusage`

.. tip::

    There are 3 directories built into this container. These directories should be used to mount data.
    
    - /seq_pep
    - /sqlite3
    - /work-dir
    

**Example PBS Script for Annotate Process**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #PBS -N kobas
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /rsgrps/shaneburgess/amanda/i5k/kobas
    
    singularity pull docker://agbase/kobas:3.0.3_0
    
    singularity run \
    -B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep \
    -B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3 \
    -B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir \
    kobas_3.0.3_0.sif \
    -a 
    -i AROS1000.fa \
    -s dme \
    -t fasta:pro \
    -o AROS1000 \


**Command Explained**
"""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**kobas_3.0.3_0.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-a:** Tells KOBAS to runt he 'annotate' process.

**-i AROS1000.fa:** input file (peptide FASTA)

**-s dme:** Enter the species for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o AROS1000:** name of output file 

For information on output files see :ref:`Understanding Your Results: Annotate <annotateresults>`


**Example PBS Script for Identify Process**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #PBS -N kobas
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /rsgrps/shaneburgess/amanda/i5k/kobas
    
    singularity pull docker://agbase/kobas:3.0.3_0
    
    singularity run \
    -B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep \
    -B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3 \
    -B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir \
    kobas_3.0.3_0.sif \
    -g 
    -f AROS1000 \
    -b dme \
    -o ident_out


**Command Explained**
"""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**kobas_3.0.3_0.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-g:** Tells KOBAS to runt he 'identify' process.

**-f AROS1000:** output file from 'annotate'

**-b dme:** background; enter the species for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-o ident_out:** name of output file 

For information on output see :ref:`Understanding Your Results: Identify <identifyresults>`

**Example PBS Script for Annotate and Identify Pipeline**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #PBS -N kobas
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /rsgrps/shaneburgess/amanda/i5k/kobas
    
    singularity pull docker://agbase/kobas:3.0.3_0
    
    singularity run \
    -B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep \
    -B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3 \
    -B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir \
    kobas_3.0.3_0.sif \
    -j 
    -i AROS1000.fa \
    -s dme \
    -t fasta:pro \
    -o AROS1000 \


**Command Explained**
""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /rsgrps/shaneburgess/amanda/i5k/seq_pep:/seq_pep:** tells docker to mount the 'seq_pep' directory I downloaded to the host machine to the '/seq_pep' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-B /rsgrps/shaneburgess/amanda/i5k/sqlite3:/sqlite3:** mounts 'sqlite3' directory on host machine into 'go_info' directory inside the container

**-B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**kobas_3.0.3_0.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-j:** Tells KOBAS to runt he 'annotate' process.

**-i AROS1000.fa:** input file (peptide FASTA)

**-s dme:** Enter the species for the species of the sequences in your input file. 

.. NOTE:: 

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o AROS1000:** name of output file 

.. NOTE::

    This pipeline will automatically use the output of 'annotate' as the -f foreground input for 'identify'. 
    This will also use your species option as the -b background input for 'identify'.

For information on outputs see :ref:`Understanding Your Results: Annotate and Identify <annoident>`

**Understanding Your Results**
==============================

.. _annotateresults:


**Annotate**
------------

If all goes well, you should get the following:

- **seq_pep folder:** This folder contains the BLAST database files used in your analysis.
- **sqlite3 folder:** This folder contains the annotation database files used in your analysis
- **<species>.tsv:** This is the tab-delimited output from the BLAST search. It is unlikely that you will need to look at this file.
- **<output_file_name_you_provided>:** KOBAS-annotate generates a text file with the name you provide. It has two sections. 

The first sections looks like this:

.. code-block:: none

    ##dme	Drosophila melanogaster (fruit fly)
    ##Method: BLAST	Options: evalue <= 1e-05
    ##Summary:	87 succeed, 0 fail

    #Query	Gene ID|Gene name|Hyperlink
    lcl|NW_020311285.1_prot_XP_012256083.1_15	dme:Dmel_CG34349|Unc-13-4B|http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG34349
    lcl|NW_020311286.1_prot_XP_020708336.1_46	dme:Dmel_CG6963|gish|http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG6963
    lcl|NW_020311285.1_prot_XP_020707987.1_39	dme:Dmel_CG30403||http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG30403
    
The second section follows a dashed line and looks like this:

.. code-block:: none

    --------------------

    ////
    Query:              	lcl|NW_020311285.1_prot_XP_012256083.1_15
    Gene:               	dme:Dmel_CG34349	Unc-13-4B
    Entrez Gene ID:      	43002
    ////
    Query:              	lcl|NW_020311286.1_prot_XP_020708336.1_46
    Gene:               	dme:Dmel_CG6963	gish
    Entrez Gene ID:      	49701
    Pathway:            	Hedgehog signaling pathway - fly	KEGG PATHWAY	dme04341
    ////
    Query:              	lcl|NW_020311285.1_prot_XP_020707987.1_39
    Gene:               	dme:Dmel_CG30403	
    Entrez Gene ID:      	246595
    ////
    Query:              	lcl|NW_020311285.1_prot_XP_020707989.1_40
    Gene:               	dme:Dmel_CG6148	Past1
    Entrez Gene ID:      	41569
    Pathway:            	Endocytosis	KEGG PATHWAY	dme04144
                                Hemostasis	Reactome	R-DME-109582
                    	        Factors involved in megakaryocyte development and platelet production	Reactome	R-DME-98323

.. _identifyresults:

**Identify**
------------

If all goes well, you should get the following:

- **sqlite3 folder:** This folder contains the annotation database files used in your analysis

- **<output_file_name_you_provided>:** KOBAS identify generates a text file with the name you provide.

.. code-block:: none

    ##Databases: PANTHER, KEGG PATHWAY, Reactome, BioCyc
    ##Statistical test method: hypergeometric test / Fisher's exact test
    ##FDR correction method: Benjamini and Hochberg

    #Term	Database	ID	Input number	Background number	P-Value	Corrected P-Value	Input	Hyperlink
    Hedgehog signaling pathway - fly	KEGG PATHWAY	dme04341	12	33	3.20002656734e-18	1.76001461204e-16	lcl|NW_020311286.1_prot_XP_012256678.1_51|lcl|NW_020311286.1_prot_XP_025602973.1_48|lcl|NW_020311286.1_prot_XP_012256683.1_52|lcl|NW_020311286.1_prot_XP_012256679.1_55|lcl|NW_020311286.1_prot_XP_012256674.1_54|lcl|NW_020311286.1_prot_XP_020708336.1_46|lcl|NW_020311285.1_prot_XP_012256108.1_32|lcl|NW_020311286.1_prot_XP_012256682.1_53|lcl|NW_020311286.1_prot_XP_025603025.1_47|lcl|NW_020311286.1_prot_XP_020708334.1_49|lcl|NW_020311285.1_prot_XP_012256109.1_33|lcl|NW_020311286.1_prot_XP_020708333.1_50	http://www.genome.jp/kegg-bin/show_pathway?dme04341/dme:Dmel_CG6963%09red/dme:Dmel_CG6054%09red
    Hedgehog signaling pathway	PANTHER	P00025	6	13	3.6166668094e-10	9.94583372585e-09	lcl|NW_020311286.1_prot_XP_025602279.1_78|lcl|NW_020311286.1_prot_XP_025602289.1_76|lcl|NW_020311286.1_prot_XP_025602264.1_79|lcl|NW_020311285.1_prot_XP_012256108.1_32|lcl|NW_020311285.1_prot_XP_012256109.1_33|lcl|NW_020311286.1_prot_XP_012256943.1_77	http://www.pantherdb.org/pathway/pathwayDiagram.jsp?catAccession=P00025
    Signaling by NOTCH2	Reactome	R-DME-1980145	3	8	2.00259649553e-05	0.000275357018136	lcl|NW_020311285.1_prot_XP_012256118.1_28|lcl|NW_020311285.1_prot_XP_012256117.1_27|lcl|NW_020311285.1_prot_XP_012256119.1_26	http://www.reactome.org/cgi-bin/eventbrowser_st_id?ST_ID=R-DME-1980145

.. _annoident:

**Annotate and Identify Pipeline**
----------------------------------

If all goes well, you should get the following:

- **logs folder:** This folder contains the 'conder_stderr' and 'condor_stdout' files. The files record feedback, progress and, importantly, any errors the app encountered during the analysis. You won't normally need to look at these but they are very helpful in figuring out what may have happened if your output doesn't look like you expected.

- **sqlite3 folder:** This folder contains the annotation database files used in your analysis

- **seq_pep folder:** This folder contains the BLAST database files used in your analysis.

- **<species>.tsv:** This is the tab-delimited output from the BLAST search. It is unlikely that you will need to look at this file.

- **<basename>_annotate_out.txt:** KOBAS annotate generates a text file with the name you provide. It has two sections. 

The first sections looks like this:

.. code-block:: none

    ##dme	Drosophila melanogaster (fruit fly)
    ##Method: BLAST	Options: evalue <= 1e-05
    ##Summary:	87 succeed, 0 fail

    #Query	Gene ID|Gene name|Hyperlink
    lcl|NW_020311285.1_prot_XP_012256083.1_15	dme:Dmel_CG34349|Unc-13-4B|http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG34349
    lcl|NW_020311286.1_prot_XP_020708336.1_46	dme:Dmel_CG6963|gish|http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG6963
    lcl|NW_020311285.1_prot_XP_020707987.1_39	dme:Dmel_CG30403||http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG30403
    
The second section follows a dashed line and looks like this:

.. code-block:: none

    --------------------

    ////
    Query:              	lcl|NW_020311285.1_prot_XP_012256083.1_15
    Gene:               	dme:Dmel_CG34349	Unc-13-4B
    Entrez Gene ID:      	43002
    ////
    Query:              	lcl|NW_020311286.1_prot_XP_020708336.1_46
    Gene:               	dme:Dmel_CG6963	gish
    Entrez Gene ID:      	49701
    Pathway:            	Hedgehog signaling pathway - fly	KEGG PATHWAY	dme04341
    ////
    Query:              	lcl|NW_020311285.1_prot_XP_020707987.1_39
    Gene:               	dme:Dmel_CG30403	
    Entrez Gene ID:      	246595
    ////
    Query:              	lcl|NW_020311285.1_prot_XP_020707989.1_40
    Gene:               	dme:Dmel_CG6148	Past1
    Entrez Gene ID:      	41569
    Pathway:            	Endocytosis	KEGG PATHWAY	dme04144
                                Hemostasis	Reactome	R-DME-109582
                    	        Factors involved in megakaryocyte development and platelet production	Reactome	R-DME-98323mel_CG6963
    lcl|NW_020311285.1_prot_XP_020707987.1_39	dme:Dmel_CG30403||http://www.genome.jp/dbget-bin/www_bget?dme:Dmel_CG30403
    

- **<basename>_identify_out.txt:** KOBAS identify generates a text file with the name you provide.

.. code-block:: none

    ##Databases: PANTHER, KEGG PATHWAY, Reactome, BioCyc
    ##Statistical test method: hypergeometric test / Fisher's exact test
    ##FDR correction method: Benjamini and Hochberg

    #Term	Database	ID	Input number	Background number	P-Value	Corrected P-Value	Input	Hyperlink
    Hedgehog signaling pathway - fly	KEGG PATHWAY	dme04341	12	33	3.20002656734e-18	1.76001461204e-16	lcl|NW_020311286.1_prot_XP_012256678.1_51|lcl|NW_020311286.1_prot_XP_025602973.1_48|lcl|NW_020311286.1_prot_XP_012256683.1_52|lcl|NW_020311286.1_prot_XP_012256679.1_55|lcl|NW_020311286.1_prot_XP_012256674.1_54|lcl|NW_020311286.1_prot_XP_020708336.1_46|lcl|NW_020311285.1_prot_XP_012256108.1_32|lcl|NW_020311286.1_prot_XP_012256682.1_53|lcl|NW_020311286.1_prot_XP_025603025.1_47|lcl|NW_020311286.1_prot_XP_020708334.1_49|lcl|NW_020311285.1_prot_XP_012256109.1_33|lcl|NW_020311286.1_prot_XP_020708333.1_50	http://www.genome.jp/kegg-bin/show_pathway?dme04341/dme:Dmel_CG6963%09red/dme:Dmel_CG6054%09red
    Hedgehog signaling pathway	PANTHER	P00025	6	13	3.6166668094e-10	9.94583372585e-09	lcl|NW_020311286.1_prot_XP_025602279.1_78|lcl|NW_020311286.1_prot_XP_025602289.1_76|lcl|NW_020311286.1_prot_XP_025602264.1_79|lcl|NW_020311285.1_prot_XP_012256108.1_32|lcl|NW_020311285.1_prot_XP_012256109.1_33|lcl|NW_020311286.1_prot_XP_012256943.1_77	http://www.pantherdb.org/pathway/pathwayDiagram.jsp?catAccession=P00025
    Signaling by NOTCH2	Reactome	R-DME-1980145	3	8	2.00259649553e-05	0.000275357018136	lcl|NW_020311285.1_prot_XP_012256118.1_28|lcl|NW_020311285.1_prot_XP_012256117.1_27|lcl|NW_020311285.1_prot_XP_012256119.1_26	http://www.reactome.org/cgi-bin/eventbrowser_st_id?ST_ID=R-DME-1980145


`Contact us <agbase@email.arizona.edu>`_.


