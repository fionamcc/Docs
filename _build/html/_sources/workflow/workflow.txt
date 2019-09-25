===================================
**Functional Annotation Workflow**
===================================

|workflow|

This functional annotation workflow imploys three annotation tools:


1. **GOanna:** performs a BLAST search and transfers gene ontology (GO) annotations from BLAST matches to the query gene products. 

2. **InterProScan:** InterPro is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains. InterProScan can also provide GO and pathway annotations.

3. **KOBAS:** uses BLAST to annotate the input with KEGG Orthology terms and KEGG pathways


.. NOTE::

    Each of these tools accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using TransDecoder (although other tools are also acceptable).

.. NOTE:: 

    As both GOanna and InterProScan provide GO annotations, their outputs are provided in GAF format. The 'combine GAFs' tool can then be used to make a single GAF of GO annotations, if desired. 

.. |workflow| image:: ../img/i5k_workflow_diagram.png
  :width: 700