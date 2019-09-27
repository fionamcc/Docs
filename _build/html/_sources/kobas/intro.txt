==========
**Intro**
==========

- KEGG Orthology Based Annotation System (KOBAS) is a standalone Python application.
- Consists of two modules:
    1. annotate--Assigns appropriate KO terms for queried sequences based on a similarity search.
    2. identify--Discovers enriched KO terms among the annotation results by frequency of pathways or statistical significance of pathways. 



**Where to Find KOBAS** 
=======================

KOBAS is provided as a Docker container for use on the command line and as a group of apps in the CyVerse Discovery Environment. 


- `Docker Hub <https://hub.docker.com/r/agbase/kobas>`_

- `KOBAS annotate 3.0.3 <https://de.cyverse.org/de/?type=apps&app-id=070f519e-983f-11e9-b659-008cfa5ae621&system-id=de>`_

- `KOBAS identify 3.0.3 <https://de.cyverse.org/de/?type=apps&app-id=9e0a429c-dee0-11e9-948a-008cfa5ae621&system-id=de>`_

- `KOBAS annotate and identify 3.0.3 <https://de.cyverse.org/de/?type=apps&app-id=2959dcb4-d0d0-11e9-9f25-008cfa5ae621&system-id=de>`_

.. NOTE::

    Each of these tools accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

**Getting the KOBAS Databases**
===============================
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

.. _kobasusage:

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
