============================================
**InterProScan on the Command Line**
============================================

**Getting the InterProScan Data** 
=================================

Partner data are available from the InterProScan FTP site. These data are available as two separate downloads and can be obtained following these instructions:

**1. Partner Data excluding Panther**

.. code-block:: none

    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.36-75.0/alt/interproscan-data-5.36-75.0.tar.gz 
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.36-75.0/alt/interproscan-data-5.36-75.0.tar.gz.md5 
    md5sum -c interproscan-data-5.36-75.0.tar.gz.md
    tar -pxvzf interproscan-data-5.36-75.0.tar.gz

.. admonition:: tar options

   - p = preserve the file permissions
   - x = extract files from an archive
   - v = verbosely list the files processed
   - z = filter the archive through gzip
   - f = use archive file

**2. Getting the Panther data**

.. code-block:: none

    cd interproscan-5.36-75.0/data
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz.md5 
    md5sum -c panther-data-14.1.tar.gz.md5
    tar -pxvzf panther-data-14.1.tar.gz


These data will be unarchived into this directory structure. They will need to be mounted to the container at run time.

.. code-block:: none

    <current_working_directory>/interproscan-5.36-75.0/data


**Container Technologies**
==========================
GOanna is provided as a Docker container. 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity**. 

Docker containers can be run with either technology.

**Running InterProScan using Docker**
=====================================

.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine (such as CyVerse Atmosphere) etc. Docker can be installed quickly on an Atmosphere instance by typing ‘ezd’.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://learning.cyverse.org/projects/container_camp_workshop_2019/en/latest/docker/dockerintro.html>`_.


.. Important::

    We have included this basic documentation for running InterProScan with Docker. However, InterProScan requires quite a lot of compute resources and may need to be run on an HPC system. If you need to use HPC see 'Singularity' below. 



**Getting the InterProScan Container**
---------------------------------------
The InterProScan tool is available as a Docker container on Docker Hub: 
`InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    docker pull agbase/interproscan:5.36-75.0_0

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/interproscan:5.36-75.0_0



**Running InterProScan with Data**
----------------------------------
.. tip::

    There is one directory built into this container. This directory should be used to mount your working directory.
    
    - /data

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm -v $(pwd):/work-dir agbase/interproscan:5.36-75.0_0 -h

See :ref:`iprsusage`


**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    -v /rsgrps/shaneburgess/amanda/i5k/interproscan:/data \
    -v /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data \
    agbase/interproscan:5.36-75.0_0 \
    -i /rsgrps/shaneburgess/amanda/i5k/interproscan/pnnl_10000.fasta \
    -d outdir_10000 \
    -f tsv,json,xml,html,gff3,svg \
    -g \
    -p \
    -n Amanda \
    -x 109069 \
    -D AgBase

**Command Explained**
""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes container when analysis finishes (image will remain for furture analyses)

**-v /rsgrps/shaneburgess/amanda/i5k/interproscan:/data:** mount my working directory on the host machine into the /data directory in the container. The syntax for this is <absolute path on host machine>:<absolute path in container>

**-v /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data:** mounts the InterProScan partner data (downloaded from FTP) on the host machine into the /opt/interproscan/data directory in the container

**agbase/interproscan:5.36-75.0-0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are GOanna options
    
**-i /rsgrps/shaneburgess/amanda/i5k/interproscan/pnnl_10000.fasta:** local path to input FASTA file. You can also use the mounted file path: /data/pnnl_10000.fasta


**-d outdir_10000:** output directory name


**-f tsv,json,xml,html,gff3,svg:** desired output file formats


**-g:** tells the tool to perform GO annotation 


**-p:** tells tool to perform pathway annoation


**-n Amanda:** name of biocurator to include in column 15 of GAF output file


**-x 109069:** taxon ID of query species to be used in column 13 of GAF output file

**-D AgBase:** database of query accession to be used in column 1 of GAF output file


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


**Running InterProScan with Singularity (HPC)**
===============================================
.. admonition:: About Singularity

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where is is installed
    - more information about `installing Singularity <https://singularity.lbl.gov/docs-installation>`_
    - This tool was tested using Singularity 3.0. Users with Singularity 2.x will need to modify the commands accordingly.


.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a PBSPro system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the InterProScan Data** 
---------------------------------
Partner data are available from the InterProScan FTP site. These data are available as two separate downloads and can be obtained following these instructions:

**1. Partner Data excluding Panther**

.. code-block:: none

    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.36-75.0/alt/interproscan-data-5.36-75.0.tar.gz 
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.36-75.0/alt/interproscan-data-5.36-75.0.tar.gz.md5 
    md5sum -c interproscan-data-5.36-75.0.tar.gz.md
    tar -pxvzf interproscan-data-5.36-75.0.tar.gz

.. admonition:: tar options

   - p = preserve the file permissions
   - x = extract files from an archive
   - v = verbosely list the files processed
   - z = filter the archive through gzip
   - f = use archive file

**2. Getting the Panther data**

.. code-block:: none

    cd interproscan-5.36-75.0/data
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz.md5 
    md5sum -c panther-data-14.1.tar.gz.md5
    tar -pxvzf panther-data-14.1.tar.gz


These data will be unarchived into this directory structure. They will need to be mounted to the container at run time.

.. code-block:: none

    <current_working_directory>/interproscan-5.36-75.0/data


**Getting the InterProScan Container**
--------------------------------------
The InterProScan tool is available as a Docker container on Docker Hub: 
`InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    singularity pull docker://agbase/interproscan:5.36-75.0_0

    
**Running InterProScan with Data**
----------------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
**Example PBS script:**


.. code-block:: bash

    #!/bin/bash
    #PBS -N 10000j100
    #PBS -q standard
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -W group_list=fionamcc
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0

    module load singularity

    cd /rsgrps/shaneburgess/amanda/i5k/interproscan
    
    singularity pull docker://agbase/interproscan:5.36-75.0_0

    singularity run \
    interproscan_5.36-75.0_0.sif \
    -h

See :ref:`iprsusage`

.. tip::

    There is one directory built into this container. This directory should be used to mount your working directory.
    
    - /data

**Example PBS Script**
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #PBS -N 10000j100
    #PBS -q standard
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -W group_list=fionamcc
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0

    module load singularity

    cd /rsgrps/shaneburgess/amanda/i5k/interproscan
    
    singularity pull docker://agbase/interproscan:5.36-75.0_0

    singularity run \
    -B /rsgrps/shaneburgess/amanda/i5k/interproscan:/data \
    -B /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data \
    interproscan_5.36-75.0_0.sif \
    -i /rsgrps/shaneburgess/amanda/i5k/interproscan/pnnl_10000.fasta \
    -d outdir_10000 \
    -f tsv,json,xml,html,gff3,svg \
    -g \
    -p \
    -n Amanda \
    -x 109069 \
    -D AgBase
    
**Command Explained**
""""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /rsgrps/shaneburgess/amanda/i5k/interproscan:/data:** mounts my working directory on the host machine into the /data directory in the container the syntax for this is <aboslute path on host machine>:<aboslute path in container>

**-B /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data:** mounts he InterProScan data directory that was downloaded from the FTP site into the InterProScan data directory in the container

**interproscan_5.36-75.0_0.sif:** name of the image to use

.. tip::

    All the options supplied after the image name are options for this tool

**-i /rsgrps/shaneburgess/amanda/i5k/interproscan/pnnl_10000.fasta:** input FASTA file


**-d outdir_10000:** output directory name


**-f tsv,json,xml,html,gff3,svg:** desired output file formats


**-g:** tells the tool to perform GO annotation 


**-p:** tells tool to perform pathway annoation


**-n Amanda:** name of biocurator to include in column 15 of GAF output file


**-x 109069:** taxon ID of query species to be used in column 13 of GAF output file

**-D AgBase:** database of query accession to be used in column 1 of GAF output file

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
