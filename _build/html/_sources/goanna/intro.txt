**Intro**
==========

- GOanna performs a BLAST search, allows you to filter based on BLAST match parameters and transfers Gene Ontology (GO) functional annotations from the BLAST matches to your input genes.	
- GOanna accepts a protein FASTA file as input.
- BLAST databases are created by AgBase based upon proteins that have GO available and subsetted by phyla. We recommend selecting the database most closely related to the sequence used as input.
- We strongly recommend selecting only GO annotations based on experimental evidence codes. This will ensure the best quality annotations for your data.
- The remaining parameters are standard BLAST parameters. More information on determining the best BLAST parameters for your specific data set can be found in the section below.


**Where to Find GOanna** 
^^^^^^^^^^^^^^^^^^^^^^^^
`Docker Hub <https://hub.docker.com/r/agbase/goanna>`_
----------------------------------------------------------

`CyVerse Discovery Environment <https://de.cyverse.org/de/?type=apps&app-id=354731ae-71ab-11e9-b82a-008cfa5ae621&system-id=de>`_
-----------------------------------------------------------------------------------------------------------------------------------

`AgBase <https://agbase.arizona.edu/cgi-bin/tools/GOanna.cgi>`_
-----------------------------------------------------------------


**Inputs**
^^^^^^^^^^

1. GOanna accepts a FASTA file of protein sequences. Nucleotide sequences must be translated first. 

.. NOTE::

    If you do not have a preferred method of translating your sequences **TransDecoder**  is available in the CyVerse Discovery Environment **INSERT LINK HERE** and as a `Docker container <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ at quay.io.


.. IMPORTANT::

    This tool uses stand alone BLAST and interprets FASTA deflines accordingly. NCBI ‘bar’-delimited deflines can be interpreted correctly using the ‘parse-deflines’ option. If the ‘parse-deflines’ option is not checked then BLAST will interpret the ID to be everything before the first space.


2. AgBase provides BLAST databases for each of the following phylogentic sets: 

- arthropod
- bacteria
- bird
- crustacean
- fish
- fungi
- human
- insecta
- invertebrates
- mammals
- nematode
- plants
- rodents
- uniprot_sprot
- uniprot_trembl


3. Output file basename. This will be the prefix of all your output files.

**Determining BLAST Parameters**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- BLAST parameters are contingent on the BLAST database used and the composition of the input file, and so will change for each analysis. 
- We recommend making a subset of 100 randomly selected sequences from your larger data set and using this as the input for GOanna to test for parameters that give good alignment. 

.. NOTE:: 

    The `Split FASTA file  <https://de.cyverse.org/de/?type=apps&app-id=c7e10a48-f5e6-4db8-8169-825cf62bd09d&system-id=de>`_ app, available in the CyVerse Discovery Environment can be used to make a test file. 

- To test for good parameters use the same database you will use and set relaxed parameters.
- Once you have run your subsetted file, use the html file to view alignments, select good alignments and note the parameters for these.


**Outputs**
^^^^^^^^^^^

GOanna generates 4 output files. Three of these are different formats of the BLASTp output and the final file is a gene association file (GAF) that contains to Gene Ontology (GO) annotations.

1. <basename>.asn

2. <basename>.html

3. <basename>.tsv

4. <basename>_goanna_gaf.tsv
