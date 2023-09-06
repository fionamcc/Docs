====================================
**Combine GAFs on the Command Line**
====================================

**Container Technologies**
==========================
GOanna is provided as a Docker container.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity (Apptainer)**.

Docker containers can be run with either technology.


**Combine GAFs using Docker**
=============================

.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine etc.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://docs.docker.com/engine/install/>`_.


**Getting the Combine GAFs container**
--------------------------------------
The Combine GAFs tool is available as a Docker container on Docker Hub:
`Combine GAFs container <https://hub.docker.com/r/agbase/combine_gafs>`_

The container can be pulled with this command:

.. code-block:: bash

    docker pull agbase/combine_gafs:1.1

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/combine_gafs:1.1

**Running Combine GAFs with Data**
-----------------------------------

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v $(pwd):/work-dir \
    agbase/combine_gafs:1.1 \
    -i CFLO_1.fa_gaf.txt \
    -g clfo1_v_insecta_goanna_gaf.tsv \
    -o complete_gaf 


**Command Explained**
""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/combine_gafs:1.1:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are Combine_GAFs options

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added 


**Combine GAFs using Singularity (Apptainer)**
==============================================


.. admonition:: About Singularity (Apptainer)

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where is is installed
    - more information about `installing Singularity <https://apptainer.org/docs-legacy>`_
    - This tool was tested using Singularity 3.10.2.


.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a PBSPro system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the Combine GAFs Container**
--------------------------------------
The Combine GAFs tool is available as a Docker container on Docker Hub:
`Combine GAFs container <https://hub.docker.com/r/agbase/combine_gafs>`_

The container can be pulled with this command:

.. code-block:: bash

    singularity pull docker://agbase/combine_gafs:1.1

**Running Combine GAFs with Data**
----------------------------------

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example SLURM Script**
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=combine_gafs
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularityCE

    singularity run \
    -B /directory/you/want/to/work/in:/work-dir \
    combine_gafs_1.1.sif \
    -i CFLO_1.fa_gaf.txt \
    -g clfo1_v_insecta_goanna_gaf.tsv \
    -o complete_gaf

**Command Explained**
""""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /directory/you/want/to/work/in:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**combine_gafs_1.1.sif:** the name of the Singularity image file to use

.. tip::

    All the options supplied after the image name are GOanna options

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added 