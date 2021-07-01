===================================
**Functional Annotation Workflow**
===================================

|workflow|

This functional annotation workflow employs three annotation tools:


1. **GOanna:** It performs a BLAST search and transfers gene ontology (GO) annotations from BLAST matches to the query gene products. 

2. **InterProScan:** InterPro is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains. InterProScan can also provide GO and pathway annotations.

3. **KOBAS:** It uses BLAST to annotate the input with KEGG Orthology terms and KEGG pathways

Results and analysis from the application of this functional annotation workflow to the Official gene set v3.0 protein set from *Diaphorina citri* followed by a differential expression analysis was presented at a seminar in the University of Arizona Animal and Comparative Biomedical Sciences in Fall 2020. The `slides <https://www.slideshare.net/suryasaha/functional-annotation-of-invertebrate-genomes>`_ and `video <https://arizona.zoom.us/rec/play/tZZ-fuutrj43T9fBtASDAaR9W9S0fP6s1XQbrvQOz0e0VnYHYVL1MOMaZ-F4v45qOmXQkV1MUXQ7tufD>`_ are available online.


**Citation**

Please cite the following preprint if you use annotation results from the workflow
Saha, S., Cooksey, A. M., Childers, A. K., Poelchau, M. F., & Mccarthy, F. M. (2021). Workflows for rapid functional annotation of diverse arthropod genomes. BioRxiv, 2021.06.12.448177. https://doi.org/10.1101/2021.06.12.448177

.. NOTE::

    Each of these tools accepts a peptide FASTA file. For those users with nucleotide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

.. NOTE:: 

    As both GOanna and InterProScan provide GO annotations, their outputs are provided in GAF format. The **'Combine GAFs'** tool can then be used to make a single GAF of GO annotations, if desired. 

.. |workflow| image:: ../img/i5k_workflow_diagram.png
  :width: 700
