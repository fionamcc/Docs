==========
**Intro**
==========

- GOanna performs a BLAST search, allows you to filter based on BLAST match parameters and transfers Gene Ontology (GO) functional annotations from the BLAST matches to your input genes.	
- GOanna accepts a protein FASTA file as input.
- BLAST databases are created by AgBase based upon proteins that have GO available and subsetted by phyla. We recommend selecting the database most closely related to the sequence used as input.
- We strongly recommend selecting only GO annotations based on experimental evidence codes. This will ensure the best quality annotations for your data.
- The remaining parameters are standard BLAST parameters. More information on determining the best BLAST parameters for your specific data set can be found in the section below.

Results and analysis from the application of GOanna to the Official gene set v3.0 protein set from *Diaphorina citri* followed by a differential expression analysis was presented at a seminar in the University of Arizona Animal and Comparative Biomedical Sciences in Fall 2020. The `slides <https://www.slideshare.net/suryasaha/functional-annotation-of-invertebrate-genomes>`_ and `video <https://arizona.zoom.us/rec/play/tZZ-fuutrj43T9fBtASDAaR9W9S0fP6s1XQbrvQOz0e0VnYHYVL1MOMaZ-F4v45qOmXQkV1MUXQ7tufD>`_ are available online.


**Where to Find GOanna** 
========================
- `Docker Hub <https://hub.docker.com/r/agbase/goanna>`_


- `CyVerse Discovery Environment <https://de.cyverse.org/de/?type=apps&app-id=354731ae-71ab-11e9-b82a-008cfa5ae621&system-id=de>`_


- `AgBase <https://agbase.arizona.edu/cgi-bin/tools/GOanna.cgi>`_


**Getting the GOanna Databases**
================================

To run the tool you need some public data. These files are now available as gzipped files to aid downloading. The directories are best downloaded with `iCommands <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html>`_. Once iCommands is `setup <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html#icommands-first-time-configuration>`_ you can use ‘iget’ to download the data.

1) agbase_database: species subset to run BLAST against  (this command will download the entire directory)

.. code-block:: bash

    iget -rPT /iplant/home/shared/iplantcollaborative/protein_blast_dbs/agbase_database


2) go_info: Uniprot GO annotations (this command will download the entire directory)

.. code-block:: bash

    iget -rPT /iplant/home/shared/iplantcollaborative/protein_blast_dbs/go_info


.. NOTE::

    Each of these tools accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

.. _goannausage:

**Help and Usage Statement**
============================

.. code-block:: none

    Options:
    -a BLAST database basename ('arthropod', 'bacteria', 'bird', 'crustacean', 'fish', 'fungi', 'human', 'insecta',
       'invertebrates', 'mammals', 'nematode', 'plants', 'rodents' 'uniprot_sprot', 'uniprot_trembl', 'vertebrates'
        or 'viruses')
    -c peptide fasta filename
    -o output file basename
    [-b transfer GO with experimental evidence only ('yes' or 'no'). Default = 'yes'.]
    [-d database of query ID. If your entry contains spaces either substitute and underscore (_) or,
        to preserve the space, use quotes around your entry. Default: 'user_input_db']
    [-e Expect value (E) for saving hits. Default is 10.]
    [-f Number of aligned sequences to keep. Default: 3]
    [-g BLAST percent identity above which match should be kept. Default: keep all matches.]
    [-h help]
    [-m BLAST percent positive identity above which match should be kept. Default: keep all matches.]
    [-s bitscore above which match should be kept. Default: keep all matches.]
    [-k Maximum number of gap openings allowed for match to be kept.Default: 100]
    [-l Maximum number of total gaps allowed for match to be kept. Default: 1000]
    [-q Minimum query coverage per subject for match to be kept. Default: keep all matches]
    [-t Number of threads.  Default: 8]
    [-u 'Assigned by' field of your GAF output file. If your entry contains spaces (eg. firstname lastname)
        either substitute and underscore (_) or, to preserve the space, use quotes around your entry (eg. "firstname lastname")
        Default: 'user']
    [-x Taxon ID of the query species. Default: 'taxon:0000']
    [-p parse_deflines. Parse query and subject bar delimited sequence identifiers]
