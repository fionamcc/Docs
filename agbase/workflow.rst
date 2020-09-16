===================================
**Functional Annotation Workflow**
===================================

|workflow|

This functional annotation workflow employs three annotation tools:


1. **GOanna:** performs a BLAST search and transfers gene ontology (GO) annotations from BLAST matches to the query gene products. 

2. **InterProScan:** InterPro is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains. InterProScan can also provide GO and pathway annotations.

3. **KOBAS:** uses BLAST to annotate the input with KEGG Orthology terms and KEGG pathways


.. NOTE::

    Each of these tools accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

.. NOTE:: 

    As both GOanna and InterProScan provide GO annotations, their outputs are provided in GAF format. The **'Combine GAFs'** tool can then be used to make a single GAF of GO annotations, if desired. 

.. |workflow| image:: ../img/i5k_workflow_diagram.png
  :width: 700
