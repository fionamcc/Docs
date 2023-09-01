======================================
**KOBAS on the Command Line**
======================================


**Getting the databases**
==========================

`No longer required as of version 3.0.3_3 <file:///home/amcooksey/Documents/USDA_i5K/Docs/_build/kobas/intro.html#getting-the-kobas-databases>`_.


**Container Technologies**
===========================
KOBAS is provided as a Docker container.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Apptainer (Singularity)**.

Docker containers can be run with either technology.

**Running KOBAS using Docker**
==============================
.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ (admin) permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine etc. 
    - For more information on installing Docker on other systems:  `Installing Docker <https://docs.docker.com/engine/install/>`_.


**Getting the KOBAS container**
-------------------------------
The KOBAS tool is available as a Docker container on Docker Hub:
`KOBAS container <https://hub.docker.com/r/agbase/kobas>`_

The container can be pulled with this command:

.. code-block:: bash

    docker pull agbase/kobas:3.0.3_3

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/kobas:3.0.3_3




**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm agbase/kobas:3.0.3_3 -h


.. tip::

    The /work-dir directory is built into this container and should be used to mount your data.

KOBAS can perform two tasks
- annotate (-a)
- identify (enrichment) (-g)

KOBAS can also run both tasks with a single command (-j).

**Annotate Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run \
    --rm \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_3 \
    -a \
    -i GCF_001298625.1_SEUB3.0_protein.faa \
    -s sce \
    -t fasta:pro \
    -o GCF_001298625.1

**Command Explained**
""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' inside the container

**agbase/kobas:3.0.3_3:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-a:** Tells KOBAS to run the 'annotate' process.

**-i GCF_001298625.1_SEUB3.0_protein.faa:** input file (protein FASTA).

**-s sce:** Enter the species code for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o GCF_001298625.1:** prefix for the output file names

Reference `Understanding results`_.

**Identify Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run \
    --rm \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_3 \
    -g \
    -f GCF_001298625.1_SEUB3.0_protein.faa \
    -b sce \
    -o ident_out

**Command Explained**
"""""""""""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/kobas:3.0.3_3:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-g:** Tells KOBAS to runt he 'identify' process.

**-f GCF_001298625.1_SEUB3.0_protein.faa:** output file from KOBAS annotate

**-b sce:** background; enter the species code for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-o ident_out:** basename of output file

Reference `Understanding results`_.

**Annotate and Identify Pipeline Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run \
    --rm \
    -v $(pwd):/work-dir \
    agbase/kobas:3.0.3_3 \
    -j \
    -i GCF_001298625.1_SEUB3.0_protein.faa \
    -s sce \
    -t fasta:pro
    -o GCF_001298625.1

**Command Explained**
"""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/kobas:3.0.3_3:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-j:** Tells KOBAS to run both the 'annotate' and 'identify' processes.

**-i GCF_001298625.1_SEUB3.0_protein.faa:** input file (protein FASTA)

**-s sce:** Enter the species code for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o GCF_001298625.1:** basename of output files

.. NOTE::

    This pipeline will automatically use the output of 'annotate' as the -f foreground input for 'identify'.
    This will also use your species option as the -b background input for 'identify'.

Reference `Understanding results`_.

**Running KOBAS using Singularity**
============================================
.. admonition:: About Singularity (now Apptainer)

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where it is installed
    - more information about `installing Singularity <https://apptainer.org/docs-legacy>`_
    - This tool was tested using Singularity 3.10.2.

.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a Slurm system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the KOBAS container**
-------------------------------
The KOBAS tool is available as a Docker container on Docker Hub:
`KOBAS container <https://hub.docker.com/r/agbase/kobas>`_

**Example Slurm script:**

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=kobas
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularity

    cd /location/where/your/want/to/save/file

    singularity pull docker://agbase/kobas:3.0.3_3


**Running KOBAS with Data**
---------------------------

.. tip::

    There /work-dir directory is built into this container and should be used to mount data.


**Example Slurm Script for Annotate Process**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=kobas
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularity

    cd /directory/you/want/to/work/in

    singularity run \
    -B /directory/you/want/to/work/in:/work-dir \
    /path/to/your/copy/kobas_3.0.3_3.sif \
    -a \
    -i GCF_001298625.1_SEUB3.0_protein.faa \
    -s sce \
    -t fasta:pro \
    -o GCF_001298625.1



**Command Explained**
"""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /project/nal_genomics/amanda.cooksey/protein_sets/saceub/KOBAS:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**/path/to/your/copy/kobas_3.0.3_3.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-a:** Tells KOBAS to run the 'annotate' process.

**-i GCF_001298625.1_SEUB3.0_protein.faa:** input file (protein FASTA)

**-s sce:** Enter the species for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o GCF_001298625.1:** name of output file

Reference `Understanding results`_.

**Example Slurm Script for Identify Process**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=kobas
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularity

    cd /location/where/your/want/to/save/file

    singularity pull docker://agbase/kobas:3.0.3_3

    singularity run \
    -B /directory/you/want/to/work/in:/work-dir \
    kobas_3.0.3_3.sif \
    -g \
    -f GCF_001298625.1_SEUB3.0_protein.faa \
    -b sce \
    -o ident_out


**Command Explained**
"""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /project/nal_genomics/amanda.cooksey/protein_sets/saceub/KOBAS:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**kobas_3.0.3_3.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-g:** Tells KOBAS to run the 'identify' process.

**-f GCF_001298625.1_SEUB3.0_protein.faa:** output file from 'annotate'

**-b sce:** background; enter the species for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-o ident_out:** name of output file

Reference `Understanding results`_.

**Example Slurm Script for Annotate and Identify Pipeline**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=kobas
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularity

    cd /location/where/your/want/to/save/file

    singularity pull docker://agbase/kobas:3.0.3_3

    singularity run \
    -B /directory/you/want/to/work/in:/work-dir \
    kobas_3.0.3_3.sif \
    -j \
    -i GCF_001298625.1_SEUB3.0_protein.faa \
    -s sce \
    -t fasta:pro \
    -o GCF_001298625.1


**Command Explained**
""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /rsgrps/shaneburgess/amanda/i5k/kobas:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**kobas_3.0.3_3.sif:** the name of the Singularity image to use

.. tip::

    All the options supplied after the image name are KOBAS options

**-j:** Tells KOBAS to runt he 'annotate' process.

**-i GCF_001298625.1_SEUB3.0_protein.faa:** input file (protein FASTA)

**-s sce:** Enter the species for the species of the sequences in your input file.

.. NOTE::

    If you don't know the code for your species it can be found here: https://www.kegg.jp/kegg/catalog/org_list.html

    If your species of interest is not available then you should choose the code for the closest-related species available

**-t:** input file type; in this case, protein FASTA.

**-o GCF_001298625.1:** name of output file

.. NOTE::

    This pipeline will automatically use the output of 'annotate' as the -f foreground input for 'identify'.
    This will also use your species option as the -b background input for 'identify'.

.. _Understanding results:

**Understanding Your Results**
==============================


**Annotate**
------------

If all goes well, you should get the following:

- **<species>.tsv:** This is the tab-separated output from the BLAST search. It is unlikely that you will need to look at this file.
- **<basename>:** KOBAS-annotate generates a text file with the name you provide. It has two sections (detailed below).
- **<basename>_KOBAS_acc_pathways.tsv:** Our post-processing script creates this tab-separated file. It lists each accession from your data and all of the pathways to which they were annotated.
- **<basename>_KOBAS_pathways_acc.tsv:** Our post-processing script creates this tab-separated file. It lists each pathway annotated to your data with all of the accessions annotated to that pathway.

The <basename> file has two sections.
The first section looks like this:

.. code-block:: bash

    #Query	Gene ID|Gene name|Hyperlink
    XP_018220118.1	sce:YMR059W|SEN15|http://www.genome.jp/dbget-bin/www_bget?sce:YMR059W
    XP_018221352.1	sce:YJR050W|ISY1, NTC30, UTR3|http://www.genome.jp/dbget-bin/www_bget?sce:YJR050W
    XP_018224031.1	sce:YDR513W|GRX2, TTR1|http://www.genome.jp/dbget-bin/www_bget?sce:YDR513W
    XP_018222559.1	sce:YFR024C-A|LSB3, YFR024C|http://www.genome.jp/dbget-bin/www_bget?sce:YFR024C-A
    XP_018221254.1	sce:YJL070C||http://www.genome.jp/dbget-bin/www_bget?sce:YJL070C

The second section follows a dashed line and looks like this:

.. code-block:: bash

    ////
    Query:                  XP_018222878.1
    Gene:                   sce:YDL220C     CDC13, EST4
    Entrez Gene ID:         851306
    ////
    Query:                  XP_018219412.1
    Gene:                   sce:YOR204W     DED1, SPP81
    Entrez Gene ID:         854379
    Pathway:                Innate Immune System    Reactome        R-SCE-168249
                            Immune System   Reactome        R-SCE-168256
                            Neutrophil degranulation        Reactome        R-SCE-6798695


<basename>_KOBAS_acc_pathways.tsv looks like this:

.. code-block:: bash

    XP_018220118.1	BioCyc:PWY-6689
    XP_018221352.1	Reactome:R-SCE-6782135,KEGG:sce03040,Reactome:R-SCE-73894,Reactome:R-SCE-5696398,Reactome:R-SCE-6782210,Reactome:R-SCE-6781827
    XP_018224031.1	BioCyc:GLUT-REDOX-PWY,BioCyc:PWY3O-592


<basename>_KOBAS_pathways_acc.tsv looks like this:

.. code-block:: bash

    BioCyc:PWY3O-0  XP_018222002.1,XP_018222589.1
    KEGG:sce00440   XP_018222406.1,XP_018219751.1,XP_018222229.1
    Reactome:R-SCE-416476   XP_018223583.1,XP_018221814.1,XP_018222685.1,XP_018220832.1,XP_018219073.1,XP_018218776.1,XP_018223466.1,XP_018223545.1,XP_018222256.1
    Reactome:R-SCE-418346   XP_018220070.1,XP_018221774.1,XP_018221826.1,XP_018220071.1,XP_018222218.1,XP_018220541.1,XP_018219550.1


.. _identifyresults:

**Identify**
------------

If all goes well, you should get the following:

- **<output_file_name_you_provided>:** KOBAS identify generates a text file with the name you provide.

.. code-block:: bash

    ##Databases: PANTHER, KEGG PATHWAY, Reactome, BioCyc
    ##Statistical test method: hypergeometric test / Fisher's exact test
    ##FDR correction method: Benjamini and Hochberg

    #Term   Database        ID      Input number    Background number       P-Value Corrected P-Value       Input   Hyperlink
    Metabolic pathways      KEGG PATHWAY    sce01100        714     754     0.00303590229485        0.575578081959  XP_018221856.1|XP_018220917.1|XP_018222719.1|...link
    Metabolism      Reactome        R-SCE-1430728   419     438     0.0147488189928 0.575578081959  XP_018221856.1|XP_018221742.1|XP_018219354.1|XP_018221740.1|...link
    Immune System   Reactome        R-SCE-168256    304     315     0.0267150787723 0.575578081959  XP_018223955.1|XP_018222962.1|XP_018223268.1|XP_018222956.1|...link


`Contact us <agbase@email.arizona.edu>`_.


