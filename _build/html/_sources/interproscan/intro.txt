=========
**Intro**
=========

`InterPro <http://www.ebi.ac.uk/interpro/>`_ is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains.

**Basic functions of this tool**
================================

- removes special characters from FASTA sequences
- splits FASTA into groups of 1000 sequences
- runs InterProScan with user-specified options on each of the 1000-sequence files in parallel
- re-combines output files from all groups of 1000
- parses the XML output from InterProScan to generate a gene association file (GAF) (and several other files)

.. NOTE::

    This tool accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

.. NOTE:: 

    As both GOanna and InterProScan provide GO annotations, their outputs are provided in GAF format. The **'Combine GAFs'** tool can then be used to make a single GAF of GO annotations, if desired.

**Where to Find InterProScan**
==============================

`Docker Hub <https://hub.docker.com/r/agbase/interproscan>`_

`InterProScan 5.35-74.0 <https://de.cyverse.org/de/?type=apps&app-id=Interproscan-5.35.74u1&system-id=agave>`_

.. Important::

    InterProScan version 5.35-74.0 does not includ the XML parser to generate GAF file and associated count files. To get these files the user must run the `InterProScan Results function <https://de.cyverse.org/de/?type=apps&app-id=714e28fa-6580-44fc-9756-8b019c192449&system-id=de>`_ app.

.. Coming Soon!::

    InterProScan 5.36-75.0 will be available soon in the Discovery Environment. This app will included the XML parser and run it automatically with InterProScan. There will be no need to use the InterProScan Results Function app.
    

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


