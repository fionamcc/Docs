======================================
**Pathannotator on the Command Line**
======================================


**Getting the databases**
==========================
The KOfam 'profiles' and 'ko_list' databases are required to run the pipeline. If you don't already have these databases the pipeline will pull them during the first run.
If you want to download them beforehand they are available from the KEGG website.

    Using wget:

    .. code-block:: bash

        wget https://www.genome.jp/ftp/db/kofam/profiles.tar.gz

        wget https://www.genome.jp/ftp/db/kofam/ko_list.gz

If your species of interest has been annotated by the KEGG project you can provide this tool with the corresponding KEGG species code to pull those annotations directly. If your species of interest is not listed you should choose a closely related species and use that code.
KEGG species codes can be found here: https://www.genome.jp/brite/br08611


**Container Technologies**
===========================
Pathannotator is provided as a Docker container.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Apptainer (Singularity)**.

Docker containers can be run with either technology.

**Running Pathannotator using Docker**
==================================
.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ (admin) permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Apptainer/Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine etc. 
    - For more information on installing Docker on other systems:  `Installing Docker <https://docs.docker.com/engine/install/>`_.


**Getting the Pathannotator container**
------------------------------------
The Pathannotator tool is available as a Docker container on Docker Hub:
`Pathannotator container <https://hub.docker.com/r/agbase/pathannotator>`_

The container can be pulled with this command:

.. code-block:: bash

    docker pull agbase/pathannotator:1.0

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/pathannotator:1.0




**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm agbase/pathannotator:1.0 help


.. tip::

    The /workdir directory is built into this container and should be used to mount your working directory.

    The /data directory is built into this container and should be used to mount the KofamScan database files.


**Example Command**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run \
    --rm \
    -v /path/to/your/input/files:/workdir \
    -v /path/to/kofam/databases/:/data \
    agbase/pathannotator:1.0 \
    tca \
    GCF_031307605.1_icTriCast1.1_protein.faa \
    out_dir \
    FB

**Command Explained**
""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v /path/to/your/input/files:/workdir:** mounts the working directory on the host machine to '/workdir' inside the container

**-v /path/to/kofam/databases/:/data:** mounts the directory with the Kofam database files (or where you want them to be stored) on the host machine to '/data' inside the container

**agbase/pathannotator:1.0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are Pathannotator options

**tca:** KEGG species code for Tribolium casteneum. Can be found here: https://www.genome.jp/brite/br08611 . If your species doesn't have a code choose a closely related species.

**GCF_031307605.1_icTriCast1.1_protein.faa:** input file (protein FASTA, no header lines).

**out_dir:** Directory where you want the pipeline outputs to go. The directory must exist before you run the pipeline.

**FB:** FB indicates that we want to get Flybase pathways annotations in addition to KEGG annotations.

Reference `Understanding results`_.


**Running Pathannotator using Apptainer (formerly Singularity)**
============================================================
.. admonition:: About Apptainer

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Apptainer installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where it is installed
    - more information about `installing Apptainer <https://apptainer.org/docs-legacy>`_
    - This tool was tested using Apptainer 1.3.1

.. admonition:: HPC Job Schedulers

    Although Apptainer can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a Slurm system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the Pathannotator container**
------------------------------------
The Pathannotator tool is available as a Docker container on Docker Hub:
`Pathannotator container <https://hub.docker.com/r/agbase/Pathannotator>`_

**Example Slurm script:**

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=pathannot
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load apptainer

    cd /location/where/you/want/to/save/image/file

    apptainer pull docker://agbase/pathannotator:1.0


**Running Pathannotator with Data**
--------------------------------

.. tip::

    There /workdir directory is built into this container and should be used to mount your local working directory.

    There /data directory is built into this container and should be used to mount the KOfam database files.

**Example Slurm Script**
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=pathannot
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load apptainer

    cd /directory/you/want/to/work/in

    singularity run \
    -B /directory/you/want/to/work/in:/workdir \
    -B /directory/with/kofam/database/files:/data \
    /path/with/image/file/pathannotator_1.0.sif \
    tca \
    GCF_031307605.1_icTriCast1.1_protein.faa \
    out_dir \
    FB



**Command Explained**
"""""""""""""""""""""

**apptainer run:** tells Apptainer to run

**-B /directory/you/want/to/work/in:/workdir:** mounts the working directory on the host machine to '/workdir' in the container

**-B /directory/with/kofam/database/files:/data:** mounts the directory with the kofam database file (or where you want them stored) on the host machine to '/data' in the container

**/path/with/image/file/pathannotator_1.0.sif:** the name of the Apptainer image to use

.. tip::

    All the options supplied after the image name are Pathannotator options

**tca:** KEGG species code for Tribolium casteneum. Can be found here: https://www.genome.jp/brite/br08611 . If you species doesn't have a code choose a closely related species.

**GCF_031307605.1_icTriCast1.1_protein.faa:** input file (protein FASTA, no header lines)

**out_dir:** Directory where you want the outputs of the pipeline to be stored. The directory must exist before you run the pipeline.

**FB:** FB indicates that you want Flybase pathways annotations in addition to KEGG annotations

Reference `Understanding results`_.

.. _Understanding results:

**Understanding Your Results**
==============================

The output files you can expect will differ depending on the circumstances of your run. If you are using the KEGG code for your species of interest and your FASTA protein identifiers are NCBI protein IDs then your annotations will be pulled directly from the KEGG API. In other circumstances (detailed below) KofamScan will be run to identify homologs and transfer annotations to your species of interest. Under all circumstances you may specify whether or not you want to receive Flybase pathways annotations as well.

**KEGG code and NCBI protein IDs**
----------------------------------

**Expected output files:**
^^^^^^^^^^^^^^^^^^^^^^^^^^
- **tca_KEGG_species.tsv:** These are annotations to the species-specific pathways. The filename will begin with the KEGG species code. The pathway identifiers will begin the KEGG species code.

    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |KEGG_species_ID    |Input_species_ID    |KEGG_KO               |KEGG_tca_pathway      | KEGG_tca_pathway_name                                                |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141520          |XP_001813251        |K01540                |tca04820              |Cytoskeleton in muscle cells - Tribolium castaneum (red flour beetle) |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141523          |XP_001812480        |K02268                |tca00190              |Oxidative phosphorylation - Tribolium castaneum (red flour beetle)    |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141526          |XP_008195997        |K04676                |tca04350              |TGF-beta signaling pathway - Tribolium castaneum (red flour beetle)   |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+



- **tca_KEGG_ref.tsv:** These are annotations to the KEGG reference pathways. The pathway identifiers wil begin with 'map'.

    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |KEGG_species_ID |  Input_species_ID |  KEGG_KO  |   KEGG_ref_pathway  |    KEGG_ref_pathway_name                  |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141516       |  XP_015835225     |  K26207   |  map04024           |    cAMP signaling pathway                 |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141516       |  XP_015835225     |  K26207   |  map04261           |    Adrenergic signaling in cardiomyocytes |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141520       |  XP_001813251     |  K01540   |  map04022           |    cGMP-PKG signaling pathway             |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+



- **HMM_flybase.tsv:** If you used the 'FB' option for Flybase pathways annotations you will get this output.

    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    | KEGG_species_ID |Input_species_ID |Flybase_protein |Flybase_pathway_ID |Flybase_pathway_name                                   |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    | CG9885          |NP_001034540.1   |FBpp0077451     |FBgg0001085        |BMP Signaling Pathway Core Components                  |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    |CG10002          |NP_001034503.2   |FBpp0084690     |FBgg0000904        |Insulin-like Receptor Signaling Pathway Core Components|
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    |CG2666           |NP_001034492.1   |FBpp0078442     |FBgg0002045        |CHITIN BIOSYNTHESIS                                    |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+



- **dme_flybase.tsv:** This is an alternative to 'HMM_flybase.tsv' if you used the 'FB' option for Flybase pathways annotations AND your species code was 'dme' (Drosophila melanogaster).

    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |KEGG_species_ID     |Input_species_ID         |KEGG_KO         |Flybase_pathway_ID          |Flybase_pathway_name                     |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------
    |CG34403             |NP_001034490             |K04491          |FBgg0000890                 |Wnt-TCF Signaling Pathway Core Components|
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |CG2666              |NP_001034491             |K00698          |FBgg0002045                 |CHITIN BIOSYNTHESIS                      |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |CG7464              |NP_001034491             |K00698          |FBgg0002045                 |CHITIN BIOSYNTHESIS                      |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+


**KEGG code for a related species**
-----------------------------------

**Expected output files:**
^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **kofam_result_full.txt:** This is the full output from KofamScan. It has not yet been filtered and pathways annotations have not yet been identified.

    +-------------------+-----------------+-----------------+---------------------+---------------------+-------------------+
    |# gene name        |   KO            |thrshld          |score                |E-value              |KO definition      |
    +-------------------+-----------------+-----------------+---------------------+---------------------+-------------------+
    |NP_001034280.2     | K10180          |417.47           | 374.4               |1.2e-113             |T-box protein 6    |
    +-------------------+-----------------+-----------------+---------------------+---------------------+-------------------+
    | NP_001034280.2    | K10177          |886.07           |309.5                |7.2e-94              |T-box protein 3    |
    +-------------------+-----------------+-----------------+---------------------+---------------------+-------------------+
    |  NP_001034280.2   |   K10176        |750.77           |300.4                |4.6e-91              |T-box protein 2    |
    +-------------------+-----------------+-----------------+---------------------+---------------------+-------------------+

- **kofam_filtered_asterisk.txt:** This is the KofsamScan output filtered for the KEGG pre-determined 'asterisk' cutoffs. According to KEGG: "K number assignments with scores above the predefined thresholds for individual KOs are more reliable than other proposed assignments. Such high score assignments are highlighted with asterisks '*' in the output."

    +---+------------------+-------------------+------------------+-------------------------+-------------------------+------------------------------+
    |\* | NP_001034488.1   |  K20232           |99.97             |148.2                    |9.7e-46                  |ETS-domain lacking            |
    +---+------------------+-------------------+------------------+-------------------------+-------------------------+------------------------------+
    |\* | NP_001034489.1   |   K16672          |642.27            |911.6                    |1.2e-276                 |homeobox protein homothorax   |
    +---+------------------+-------------------+------------------+-------------------------+-------------------------+------------------------------+
    |\* | NP_001034490.1   |   K04491          |446.07            |754.9                    |6.8e-229                 |transcription factor 7-like 2 |
    +---+------------------+-------------------+------------------+-------------------------+-------------------------+------------------------------+
    |\* | NP_001034491.1   |   K00698          |133.83            |1083.4                   |0                        |chitin synthase [EC:2.4.1.16] |
    +---+------------------+-------------------+------------------+-------------------------+-------------------------+------------------------------+

- **tca_KEGG_species.tsv:** These are annotations to the species-specific pathways. The pathway identifiers will begin the KEGG species code.

    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |KEGG_species_ID    |Input_species_ID    |KEGG_KO               |KEGG_tca_pathway      | KEGG_tca_pathway_name                                                |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141520          |XP_001813251        |K01540                |tca04820              |Cytoskeleton in muscle cells - Tribolium castaneum (red flour beetle) |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141523          |XP_001812480        |K02268                |tca00190              |Oxidative phosphorylation - Tribolium castaneum (red flour beetle)    |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+
    |100141526          |XP_008195997        |K04676                |tca04350              |TGF-beta signaling pathway - Tribolium castaneum (red flour beetle)   |
    +-------------------+--------------------+----------------------+----------------------+----------------------------------------------------------------------+


- **tca_KEGG_ref.tsv:** These are annotations to the KEGG reference pathways. The pathway identifiers wil begin with 'map'.

    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |KEGG_species_ID |  Input_species_ID |  KEGG_KO  |   KEGG_ref_pathway  |    KEGG_ref_pathway_name                  |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141516       |  XP_015835225     |  K26207   |  map04024           |    cAMP signaling pathway                 |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141516       |  XP_015835225     |  K26207   |  map04261           |    Adrenergic signaling in cardiomyocytes |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+
    |100141520       |  XP_001813251     |  K01540   |  map04022           |    cGMP-PKG signaling pathway             |
    +----------------+-------------------+-----------+---------------------+-------------------------------------------+


- **HMM_flybase.tsv:** If you used the 'FB' option for Flybase pathways annotations you will get this output.

    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    | KEGG_species_ID |Input_species_ID |Flybase_protein |Flybase_pathway_ID |Flybase_pathway_name                                   |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    | CG9885          |NP_001034540.1   |FBpp0077451     |FBgg0001085        |BMP Signaling Pathway Core Components                  |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    |CG10002          |NP_001034503.2   |FBpp0084690     |FBgg0000904        |Insulin-like Receptor Signaling Pathway Core Components|
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+
    |CG2666           |NP_001034492.1   |FBpp0078442     |FBgg0002045        |CHITIN BIOSYNTHESIS                                    |
    +-----------------+-----------------+----------------+-------------------+-------------------------------------------------------+


- **dme_flybase.tsv:** This is an alternative to 'HMM_flybase.tsv' if you used the 'FB' option for Flybase pathways annotations AND your species code was 'dme' (Drosophila melanogaster).

    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |KEGG_species_ID     |Input_species_ID         |KEGG_KO         |Flybase_pathway_ID          |Flybase_pathway_name                     |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------
    |CG34403             |NP_001034490             |K04491          |FBgg0000890                 |Wnt-TCF Signaling Pathway Core Components|
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |CG2666              |NP_001034491             |K00698          |FBgg0002045                 |CHITIN BIOSYNTHESIS                      |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+
    |CG7464              |NP_001034491             |K00698          |FBgg0002045                 |CHITIN BIOSYNTHESIS                      |
    +--------------------+-------------------------+----------------+----------------------------+-----------------------------------------+




**'NA' as KEGG code**
---------------------

**Expected output files:**

If you did not specify a KEGG species code (used 'NA') there will be no species-specific annotations file made.

- **kofam_result_full.txt:** This is the full output from KofamScan. It has not yet been filtered and pathways annotations have not yet been identified.

    +--------------------+----------------------+----------------------+-------------------------+--------------------------+----------------+
    |# gene name         |  KO                  |thrshld               |score                    |E-value                   |KO definition   |
    +--------------------+----------------------+----------------------+-------------------------+--------------------------+----------------+
    |  NP_001034280.2    |  K10180              | 417.47               |374.4                    |1.2e-113                  |T-box protein 6 |
    +--------------------+----------------------+----------------------+-------------------------+--------------------------+----------------+
    |  NP_001034280.2    |  K10177              |886.07                |309.5                    |7.2e-94                   |T-box protein 3 |
    +--------------------+----------------------+----------------------+-------------------------+--------------------------+----------------+
    |  NP_001034280.2    |  K10176              |750.77                |300.4                    |4.6e-91                   |T-box protein 2 |
    +--------------------+----------------------+----------------------+-------------------------+--------------------------+----------------+


- **kofam_filtered_asterisk.txt:** This is the KofsamScan output filtered for the KEGG pre-determined 'asterisk' cutoffs. According to KEGG: "K number assignments with scores above the predefined thresholds for individual KOs are more reliable than other proposed assignments. Such high score assignments are highlighted with asterisks '*' in the output."

    +---+--------------------+----------------------+---------------------+----------------------+---------------------+------------------------------+
    |\* |NP_001034488.1      |K20232                |99.97                |148.2                 |9.7e-46              |ETS-domain lacking            |
    +---+--------------------+----------------------+---------------------+----------------------+---------------------+------------------------------+
    |\* |NP_001034489.1      |K16672                |642.27               |911.6                 |1.2e-276             |homeobox protein homothorax   |
    +---+--------------------+----------------------+---------------------+----------------------+---------------------+------------------------------+
    |\* |NP_001034490.1      |K04491                |446.07               |754.9                 |6.8e-229             |transcription factor 7-like 2 |
    +---+--------------------+----------------------+---------------------+----------------------+---------------------+------------------------------+
    |\* |NP_001034491.1      |K00698                |133.83               |1083.4                | 0                   |chitin synthase [EC:2.4.1.16] |
    +---+--------------------+----------------------+---------------------+----------------------+---------------------+------------------------------+


- **NA_KEGG_ref.tsv:** These are annotations to the KEGG reference pathways. The pathway identifiers wil begin with 'map'.

    +------------------+---------------------+-----------------------+----------------------+------------------------------+
    |KEGG_species_ID   |Input_species_ID     |  KEGG_KO              |KEGG_ref_pathway      | KEGG_ref_pathway_name        |
    +------------------+---------------------+-----------------------+----------------------+------------------------------+
    |NA                |NP_001034489         |K16672                 |map04391              |Hippo signaling pathway - fly |
    +------------------+---------------------+-----------------------+----------------------+------------------------------+
    |NA                |NP_001034490         |K04491                 |map04310              |Wnt signaling pathway         |
    +------------------+---------------------+-----------------------+----------------------+------------------------------+
    |NA                |NP_001034490         |K04491                 |map04390              |Hippo signaling pathway       |
    +------------------+---------------------+-----------------------+----------------------+------------------------------+


- **HMM_flybase.tsv:** If you used the 'FB' option for Flybase pathways annotations you will get this output.

    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+
    |KEGG_species_ID  |Input_species_ID          |Flybase_protein        |Flybase_pathway_ID       |Flybase_pathway_name                                    |
    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+
    |CG9885           |NP_001034540.1            |FBpp0077451            |FBgg0001085              |BMP Signaling Pathway Core Components                   |
    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+
    |CG10002          |NP_001034503.2            |FBpp0084690            |FBgg0000904              |Insulin-like Receptor Signaling Pathway Core Components |
    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+
    |CG2666           |NP_001034492.1            |FBpp0078442            |FBgg0002045              |CHITIN BIOSYNTHESIS                                     |
    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+
    |CG2666           |NP_001034491.1            |FBpp0290640            |FBgg0002045              |CHITIN BIOSYNTHESIS                                     |
    +-----------------+--------------------------+-----------------------+-------------------------+--------------------------------------------------------+


`Contact us <agbase@email.arizona.edu>`_.


