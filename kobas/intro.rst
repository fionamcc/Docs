==========
**Intro**
==========

- KEGG Orthology Based Annotation System (KOBAS) is a standalone Python application.
- Consists of two modules:
    1. annotate--Assigns appropriate KO terms for queried sequences based on a similarity search.
    2. identify--Discovers enriched KO terms among the annotation results by frequency of pathways or statistical significance of pathways. 


Results and analysis from the application of KOBAS annotation to the Official gene set v3.0 protein set from *Diaphorina citri* followed by a differential expression analysis was presented at a seminar in the University of Arizona Animal and Comparative Biomedical Sciences in Fall 2020. The `slides <https://www.slideshare.net/suryasaha/functional-annotation-of-invertebrate-genomes>`_ and `video <https://arizona.zoom.us/rec/play/tZZ-fuutrj43T9fBtASDAaR9W9S0fP6s1XQbrvQOz0e0VnYHYVL1MOMaZ-F4v45qOmXQkV1MUXQ7tufD>`_ are available online.


**Where to Find KOBAS** 
=======================

KOBAS is provided as a Docker container for use on the command line and as a group of apps in the CyVerse Discovery Environment. 


- `Docker Hub <https://hub.docker.com/r/agbase/kobas>`_

- `KOBAS annotate 3.0.3 <https://de.cyverse.org/apps/de/70cdfb64-e83a-11ec-9ecf-008cfa5ae621>`_

- `KOBAS identify 3.0.3 <https://de.cyverse.org/apps/de/7c7c242c-e83a-11ec-9ecf-008cfa5ae621>`_

- `KOBAS annotate and identify 3.0.3 <https://de.cyverse.org/apps/de/77330af8-e83a-11ec-9ecf-008cfa5ae621>`_

- `KOBAS annotate and summarize  <https://de.cyverse.org/apps/de/71cb43ba-cd8a-11ed-90f2-008cfa5ae621>`_

- `KOBAS summary <https://de.cyverse.org/apps/de/2a0d0e7c-c417-11ed-b4a3-008cfa5ae621>`_

.. NOTE::

    Each of these tools accepts a protein FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/apps/de/74828a18-f351-11e8-be2b-008cfa5ae621>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

**Getting the KOBAS Databases**
===============================

.. IMPORTANT::

    **As of version 3.0.3_3 you no longer need to download the seq_pep and sqlite3 databases** before you run KOBAS on the command line.

    **If you would like to download them** they are still available in the CyVerse Data Store. The CyVerse files can be downloaded with wget, curl or `iCommands <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step2.html>`_.


    With wget (copy the **public link** from the three dots menu to the right of the file name in the Discovery Environment):

    .. code-block:: bash

        wget https://data.cyverse.org/dav-anon/iplant/projects/iplantcollaborative/protein_blast_dbs/kobas/sqlite3.tar

    **Or** with curl (copy the **public link** from the three dots menu to the right of the file name in the Discovery Environment):

    .. code-block:: bash

        curl -o sqlite3.tar https://data.cyverse.org/dav-anon/iplant/projects/iplantcollaborative/protein_blast_dbs/kobas/sqlite3.tar

    **Or** with iCommnads copy the file **path** from the three dots menu to the right of then file name in the Discovery Environment

    .. code-block:: bash

        iget /iplant/home/shared/iplantcollaborative/protein_blast_dbs/kobas/sqlite3.tar

    Once you have the tar files you can extract all the contents or only those you wish using tar. There is no need to unzip the .gz files before you run the tool.


**Help and Usage Statement**
============================
On the command line the following help statement can be displayed with the option '-h'.

.. code-block:: none

    Options:
    [-h prints this help statement]

    [-a runs KOBAS annotate]
    KOBAS annotate options:
        -i INFILE can be FASTA or one-per-lineidentifiers. See -t intype for details.
        -s SPECIES 3 or 4 letter species abbreviation (can be found here: ftp://ftp.cbi.pku.edu.cn/pub/KOBAS_3.0_DOWNLOAD/species_abbr.txt or here: https://www.kegg.jp/kegg/catalog/org_list.html)
        -o OUTPUT file (Default is stdout.)
        -t INTYPE (fasta:pro, fasta:nuc, blastout:xml, blastout:tab, id:ncbigi, id:uniprot, id:ensembl, id:ncbigene), default fasta:pro
        [-l LIST available species, or list available databases for a specific species]
        [-e EVALUE expect threshold for BLAST, default 1e-5]
        [-r RANK rank cutoff for valid hits from BLAST result, default is 5]
        [-C COVERAGE subject coverage cutoff for BLAST, default 0]
        [-z ORTHOLOG whether only use orthologs for cross-species annotation or not, default NO (if only using orthologs, please provide the species abbreviation of your input)]
        [-k KOBAS HOME The path to kobas_home, which is the parent directory of sqlite3/ and seq_pep/. This is the absolute path in the container.]
        [-v BLAST HOME The path to blast_home, which is the parent directory of blastx and blastp. This is the absolute path in the container.]
        [-y BLASTDB The path to seq_pep/. This is the absolute path in the container.]
        [-q KOBASDB The path to sqlite3/, This is the absolute path in the container.]
        [-p BLASTP The path to blastp. This is the absolute path in the container.]
        [-x BLASTX The path to blastx. This is the absolute path in the container.]
        [-T number of THREADS to use in BLAST search. Default = 8]

    [-g runs KOBAS identify]
        KOBAS identify options:
        -f FGFILE foreground file, the output of annotate
        -b BGFILE background file, species abbreviation, see this list for species codes: https://www.kegg.jp/kegg/catalog/org_list.html
        -o OUTPUT file (Default is stdout.)
        [-d DB databases for selection, 1-letter abbreviation separated by "/": K for KEGG PATHWAY, n for PID, b for BioCarta, R for Reactome, B for BioCyc, p for PANTHER,
               o for OMIM, k for KEGG DISEASE, f for FunDO, g for GAD, N for NHGRI GWAS Catalog and G for Gene Ontology, default K/n/b/R/B/p/o/k/f/g/N/]
        [-m METHOD choose statistical test method: b for binomial test, c for chi-square test, h for hypergeometric test / Fisher's exact test, and x for frequency list,
               default hypergeometric test / Fisher's exact test
        [-n FDR choose false discovery rate (FDR) correction method: BH for Benjamini and Hochberg, BY for Benjamini and Yekutieli, QVALUE, and None, default BH
        [-c CUTOFF terms with less than cutoff number of genes are not used for statistical tests, default 5]
        [-k KOBAS HOME The path to kobas_home, which is the parent directory of sqlite3/ and seq_pep/. This is the absolute path in the container.]
        [-v BLAST HOME The path to blast_home, which is the parent directory of blastx and blastp. This is the absolute path in the container.]
        [-y BLASTDB The path to seq_pep/. This is the absolute path in the container.]
        [-q KOBASDB The path to sqlite3/. This is the absolute path in the container.]
        [-p BLASTP The path to blastp. This is the absolute path in the container.]
        [-x BLASTX The path to blastx. This is the absolute path in the container.]

    [-j runs both KOBAS annotate and identify]
