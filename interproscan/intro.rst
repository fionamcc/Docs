=========================
**Intro to InterProScan**
=========================

`InterPro <http://www.ebi.ac.uk/interpro/>`_ is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains.

**Basic functions of this tool**
--------------------------------

- removes special characters from FASTA sequences
- splits FASTA into groups of 1000 sequences
- runs InterProScan with user-specified options on each of the 1000-sequence files in parallel
- re-combines output files from all groups of 1000
- parses the XML output from InterProScan to generate a gene association file (GAF) (and several other files)

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


