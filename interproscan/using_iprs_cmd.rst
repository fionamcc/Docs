============================================
**InterProScan on the Command Line**
============================================

**Getting the InterProScan Data** 
----------------------------------

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
--------------------------
GOanna is provided as a Docker container. 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity**. 

Docker containers can be run with either technology.

**Running InterProScan using Docker**
-------------------------------

.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine (such as CyVerse Atmosphere) etc. Docker can be installed quickly on an Atmosphere instance by typing ‘ezd’.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://learning.cyverse.org/projects/container_camp_workshop_2019/en/latest/docker/dockerintro.html>`_.


.. Important::

    We have included this basic documentation for running InterProScan with Docker. However, InterProScan requires quite a lot of compute resources and may need to be run on an HPC system. If you need to use HPC see 'Singularity' below. 



**Getting the InterProScan Container**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The InterProScan tool is available as a Docker container on Docker Hub: 
`InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    docker pull agbase/interproscan:5.36-75.0_0

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/interproscan:5.36-75.0_0


**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    sudo docker run --rm -v $(pwd):/work-dir agbase/interproscan:5.36-75.0_0 -h

.. code-block:: none

    Options:
 -a  <ANALYSES>                             Optional, comma separated list of analyses.  If this option
                                            is not set, ALL analyses will be run.

 -b, <OUTPUT-FILE-BASE>                     Optional, base output filename (relative or absolute path).
                                            Note that this option, the output directory (-d) option and
                                            the output file name (-o) option are mutually exclusive.  The
                                            appropriate file extension for the output format(s) will be
                                            appended automatically. By default the input file
                                            path/name will be used.

 -d,<OUTPUT-DIR>                            Optional, output directory. Note that this option, the
                                            output file name (-o) option and the output file base (-b) option
                                            are mutually exclusive. The output filename(s) are the
                                            same as the input filename, with the appropriate file
                                            extension(s) for the output format(s) appended automatically .

 -c                                         Optional.  Disables use of the precalculated match lookup
                                            service.  All match calculations will be run locally.

 -C                                         Optional. Supply the number of cpus to use.

 -e                                         Optional, excludes sites from the XML, JSON output

 -f <OUTPUT-FORMATS>                        Optional, case-insensitive, comma separated list of output
                                            formats. Supported formats are TSV, XML, JSON, GFF3, HTML and
                                            SVG. Default for protein sequences are TSV, XML and
                                            GFF3, or for nucleotide sequences GFF3 and XML.

 -g                                         Optional, switch on lookup of corresponding Gene Ontology
                                            annotation (IMPLIES -l lookup option)

 -h                                         Optional, display help information

 -i <INPUT-FILE-PATH>                       Optional, path to fasta file that should be loaded on
                                            Master startup. Alternatively, in CONVERT mode, the
                                            InterProScan 5 XML file to convert.

 -l                                         Also include lookup of corresponding InterPro
                                            annotation in the TSV and GFF3 output formats.

 -m <MINIMUM-SIZE>                          Optional, minimum nucleotide size of ORF to report. Will
                                            only be considered if n is specified as a sequence type.
                                            Please be aware of the fact that if you specify a too
                                            short value it might be that the analysis takes a very long
                                            time!

 -o <EXPLICIT_OUTPUT_FILENAME>              Optional explicit output file name (relative or absolute
                                            path).  Note that this option, the output directory -d option
                                            and the output file basename -b option are mutually
                                            exclusive. If this option is given, you MUST specify a
                                            single output format using the -f option.  The output file
                                            name will not be modified. Note that specifying an output
                                            file name using this option OVERWRITES ANY EXISTING FILE.

 -p                                         Optional, switch on lookup of corresponding Pathway
                                            annotation (IMPLIES -l lookup option)
 -t <SEQUENCE-TYPE>                         Optional, the type of the input sequences (dna/rna (n)
                                            or protein (p)).  The default sequence type is protein.

 -T <TEMP-DIR>                              Optional, specify temporary file directory (relative or
                                            absolute path). The default location is temp/.

 -v                                         Optional, display version number
 
 -r                                          Optional. 'Mode' required ( -r 'cluster') to run in cluster mode. These options
                                            are provided but have not been tested with this wrapper script. For
                                            more information on running InterProScan in cluster mode:
                                            https://github.com/ebi-pf-team/interproscan/wiki/ClusterMode

 -R                                          Optional. Clusterrunid (crid) required when using cluster mode.
                                            -R unique_id
 Available analyses:
                      TIGRFAM (XX.X) : TIGRFAMs are protein families based on Hidden Markov Models or HMMs
                         SFLD (X.X) : SFLDs are protein families based on Hidden Markov Models or HMMs
                        ProDom (XXXX.X) : ProDom is a comprehensive set of protein domain families automatically generated from the UniProt Knowledge Database.
                        Hamap (XXXXXX.XX) : High-quality Automated and Manual Annotation of Microbial Proteomes
                        SMART (X.X) : SMART allows the identification and analysis of domain architectures based on Hidden Markov Models or HMMs
                          CDD (X.XX) : Prediction of CDD domains in Proteins
              ProSiteProfiles (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
              ProSitePatterns (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
                  SUPERFAMILY (X.XX) : SUPERFAMILY is a database of structural and functional annotation for all proteins and genomes.
                       PRINTS (XX.X) : A fingerprint is a group of conserved motifs used to characterise a protein family
                      PANTHER (X.X) : The PANTHER (Protein ANalysis THrough Evolutionary Relationships) Classification System is a unique resource that classifies genes by their functions, using published scientific experimental evidence and evolutionary relationships to predict fu$
                       Gene3D (X.X.X) : Structural assignment for whole genes and genomes using the CATH domain structure database
                        PIRSF (X.XX) : The PIRSF concept is being used as a guiding principle to provide comprehensive and non-overlapping clustering of UniProtKB sequences into a hierarchical order to reflect their evolutionary relationships.
                         Pfam (XX.X) : A large collection of protein families, each represented by multiple sequence alignments and hidden Markov models (HMMs)
                        Coils (X.X) : Prediction of Coiled Coil Regions in Proteins
                   MobiDBLite (X.X) : Prediction of disordered domains Regions in Proteins

 OPTIONS FOR XML PARSER OUTPUTS

 -F <IPRS output directory>              This is the output directory from InterProScan.
 -D <database>                           Supply the database responsible for these annotations.
 -x <taxon>                              NCBI taxon ID of the ID being annotated
 -y <type>                               Transcript or protein
 -n <biocurator>                         Name of the biocurator who made these annotations
 -M <mapping file>                       Optional. Mapping file.
 -B <bad seq file>                       Optional. Bad input sequence file.

**Running GOanna with Data**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. tip::

    There are one directory built into this container. This directory should be used to mount your working directory.
    
    - /data

**Example Command**
"""""""""""""""""""

.. code-block:: none

    sudo docker run \
    -v /rsgrps/shaneburgess/amanda/i5k/interproscan:/data \
    -v /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data \
    agbase/interproscan:5.36-75.0-0 \
    -i /rsgrps/shaneburgess/amanda/i5k/interproscan/pnnl_10000.fasta \
    -d outdir_10000 \
    -f tsv,json,xml,html,gff3,svg \
    -g \
    -p \
    -n Amanda \
    -x 109069 \
    -D AgBase

**Breakdown of Command**
""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes container when analysis finishes (image will remain for furture analyses)

**-v /rsgrps/shaneburgess/amanda/i5k/interproscan:/data:** mount my working directory on the host machine into the /data directory in the container. The syntax for this is <absolute path on host machine>:<absolute path in container>

**-v /rsgrps/shaneburgess/amanda/i5k/interproscan/interproscan-5.36-75.0/data:/opt/interproscan/data:** mounts the InterProScan partner data (downloaded from FTP) on the host machine into the /opt/interproscan/data directory in the container

**agbase/interproscan:5.36-75.0-0:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are GOanna options
    
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
**InterProScan `outputs <https://github.com/ebi-pf-team/interproscan/wiki/OutputFormats>`_**
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
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
------------------------------------------------
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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The InterProScan tool is available as a Docker container on Docker Hub: 
`InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ 

The container can be pulled with this command: 

.. code-block:: bash

    singularity pull docker://agbase/interproscan:5.36-75.0_0



**Getting the Help and Usage Statement**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
**Example PBS script:**
"""""""""""""""""""""""

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


.. code-block:: none

    **INSERT HELP STATEMENT HERE**
    
**Running InterProScan with Data**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. tip::

    There is one directory built into this container. This directory should be used to mount your working directory.
    
    - /data

**Example PBS Script**
""""""""""""""""""""""

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
    
**Breakdown of Command**
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
**InterProScan `outputs <https://github.com/ebi-pf-team/interproscan/wiki/OutputFormats>`_**
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
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
