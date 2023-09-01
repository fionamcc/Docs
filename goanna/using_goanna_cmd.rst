======================================
**GOanna on the Command Line**
======================================


**Getting the databases**
==========================
To run the tool you need some public data. These files are now available as gzipped files to aid downloading. The directories are best downloaded with `iCommands <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html>`_. Once iCommands is `setup <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html#icommands-first-time-configuration>`_ you can use ‘iget’ to download the data.

1) agbase_database: species subset to run BLAST against  (this command will download the entire directory)

.. code-block:: bash

    iget /iplant/home/shared/iplantcollaborative/protein_blast_dbs/agbase_database


2) go_info: Uniprot GO annotations (this command will download the entire directory)

.. code-block:: bash

    iget /iplant/home/shared/iplantcollaborative/protein_blast_dbs/go_info


**Container Technologies**
==========================
GOanna is provided as a Docker container. 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity**. 

Docker containers can be run with either technology.

**Running GOanna using Docker**
===============================
.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine (such as CyVerse Atmosphere) etc. Docker can be installed quickly on an Atmosphere instance by typing ‘ezd’.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://learning.cyverse.org/projects/container_camp_workshop_2019/en/latest/docker/dockerintro.html>`_.


**Getting the GOanna container**
--------------------------------
The GOanna tool is available as a Docker container on Docker Hub: 
`GOanna container <https://hub.docker.com/r/agbase/goanna>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    docker pull agbase/goanna:2.3

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/goanna:2.3


**Running GOanna with Data**
----------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm -v $(pwd):/work-dir agbase/goanna:2.3 -h

See :ref:`goannausage`

.. tip::

    There are 3 directories built into this container. These directories should be used to mount data.
    
    - /agbase_database
    - /go_info
    - /work-dir

GOanna has three required parameters:

.. code-block:: bash

    -a BLAST database basename (acceptable options are listed in the help/usage)
    -c peptide FASTA file to BLAST
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v /location/of/agbase_database:/agbase_database \
    -v /location/of/go_info:/go_info \
    -v $(pwd):/work-dir \
    agbase/goanna:2.3 \
    -a invertebrates \
    -c AROS_10.faa \
    -o AROS_10_invert_exponly \
    -p \
    -g 70 \
    -s 900 \
    -d RefSeq \
    -u "Amanda Cooksey" \
    -x 37344 \
    -k 9 \
    -q 70

**Command Explained**
""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v /location/of/agbase_database:/agbase_database:** tells docker to mount the 'agbase_database' directory you downloaded to the host machine to the '/agbase_database' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-v /locaiton/of/go_info:/go_info:** mounts 'go_info' directory on host machine into 'go_info' directory inside the container

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/goanna:2.3:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are GOanna options

**-a invertebrates:** GOanna BLAST database to use--first of three required options.

**-c AROS_10.faa:** input file (peptide FASTA)--second of three required options

**-o AROS_10_invert_exponly:** output file basename--last of three required options

**-p:** our input file has NCBI deflines. This specifies how to parse them.

**-g 70:** tells GOanna to keep only those matches with at least 70% identity

**-s 900:** tells GOanna to keep only those matches with a bitscore above 900

**-d RefSeq:** database of query ID. This will appear in column 1 of the GAF output file.

**-u "Amanda Cooksey":** name to appear in column 15 of the GAF output file

**-x 37344:** NCBI taxon ID of input file species will appear in column 13 of the GAF output file

**-k 9:** tells GOanna to keep only those matches with a maximum number of 9 gap openings

**-q 70:** tells GOanna to keep only those matches with query coverage of 70 per subject

**Understanding Your Results**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If all goes well, you should get 4 output files:

**<basename>.asn:** This is standard BLAST output format that allows for conversion to other formats. You probably won’t need to look at this output.

**<basename>.html:** This output displays in your web browser so that you can view pairwise alignments to determine BLAST parameters. 

**<basename>.tsv:** This is the tab-delimited BLAST output that can be opened and sorted in Excel to determine BLAST parameter values. The file contains the following columns:

- Query ID
- query length
- query start
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



**Running GOanna using Singularity**
====================================

.. admonition:: About Singularity

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where is is installed
    - more information about `installing Singularity <https://singularity.lbl.gov/docs-installation>`_
    - This tool was tested using Singularity 3.0. Users with Singularity 2.x will need to modify the commands accordingly.


.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a PBSPro system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the GOanna Container**
--------------------------------
The GOanna tool is available as a Docker container on Docker Hub: 
`GOanna container <https://hub.docker.com/r/agbase/goanna>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    singularity pull docker://agbase/goanna:2.3

    
**Running GOanna with Data**
----------------------------

**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
**Example PBS script:**

.. code-block:: bash

    #!/bin/bash
    #PBS -N goanna
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /where/to/save/singularity/image
    
    singularity pull docker://agbase/goanna:2.3
    
    singularity run \
    goanna_2.0.sif \
    -h

See :ref:`goannausage`

.. tip::

    There are 3 directories built into this container. These directories should be used to mount data.
    
    - /agbase_database
    - /go_info
    - /work-dir
    
GOanna has three required parameters:

.. code-block:: bash

    -a BLAST database basename (acceptable options are listed in the help/usage)
    -c peptide FASTA file to BLAST
    -o output file basename

**Example PBS Script**
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #PBS -N goanna
    #PBS -W group_list=fionamcc
    #PBS -l select=1:ncpus=28:mem=168gb
    #PBS -q standard
    #PBS -l walltime=6:0:0
    #PBS -l cput=168:0:0
    
    module load singularity
    
    cd /where/to/save/singularity/image
    
    singularity pull docker://agbase/goanna:2.3
    
    singularity run \
    -B /location/of/agbase_database:/agbase_database \
    -B /location/of/go_info:/go_info \
    -B /directory/where/you/will/work:/work-dir \
    goanna_2.3.sif \
    -a invertebrates \
    -c AROS_10.faa \
    -o AROS_10_invert_exponly \
    -p \
    -g 70 \
    -s 900 \
    -d RefSeq \
    -u "Amanda Cooksey" \
    -x 37344 \
    -t 28 \
    -q 70 \
    -k 9

**Command Explained**
""""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /location/of/agbase_database:/agbase_database:** tells docker to mount the 'agbase_database' directory I downloaded to the host machine to the '/agbase_database' directory within the container. The syntax for this is: <absolute path on host>:<absolute path in container>

**-B /location/of/go_info:/go_info:** mounts 'go_info' directory on host machine into 'go_info' directory inside the container

**-B /directory/where/you/will/work:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**goanna_2.3.sif:** the name of the Singularity image file to use

.. tip::

    All the options supplied after the image name are GOanna options

**-a invertebrates:** GOanna BLAST database to use--first of three required options.

**-c AROS_10.faa:** input file (peptide FASTA)--second of three required options

**-o AROS_10_invert_exponly:** output file basename--last of three required options

**-p:** our input file has NCBI deflines. This specifies how to parse them.

**-g 70:** tells GOanna to keep only those matches with at least 70% identity

**-s 900:** tells GOanna to keep only those matches with a bitscore above 900

**-d RefSeq:** database of query ID. This will appear in column 1 of the GAF output file.

**-u "Amanda Cooksey":** name to appear in column 15 of the GAF output file

**-x 37344:** NCBI taxon ID of input file species will appear in column 13 of the GAF output file

**-t 28:** number of threads to use for BLAST. This was run on a node with 28 cores.

**-k 9:** tells GOanna to keep only those matches with a maximum number of 9 gap openings

**-q 70:** tells GOanna to keep only those matches with query coverage of 70 per subject


**Understanding Your Results**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If all goes well, you should get 4 output files:

**<basename>.asn:** This is standard BLAST output format that allows for conversion to other formats. You probably won’t need to look at this output.

**<basename>.html:** This output displays in your web browser so that you can view pairwise alignments to determine BLAST parameters. 

**<basename>.tsv:** This is the tab-delimited BLAST output that can be opened and sorted in Excel to determine BLAST parameter values. The file contains the following columns:

- Query ID
- query length
- query start
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


