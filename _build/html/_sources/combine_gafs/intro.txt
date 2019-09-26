==========
**Intro**
==========

This tool can be used to combine the gene association file (GAF) outputs from GOanna and InterProScan. 

The tool accepts two input files:

1. GOanna GAF output
2. InterProScan GAF output

.. Note:: 

    InterProScan itself does not produce a GAF file. The    `AgBase InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ parses the XML output from InterProScan to produce the GAF file.