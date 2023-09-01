=========
**Intro**
=========

`InterPro <http://www.ebi.ac.uk/interpro/>`_ is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains.

**Basic functions of this tool**

- removes special characters from FASTA sequences
- splits FASTA into groups of 1000 sequences
- runs InterProScan with user-specified options on each of the 1000-sequence files in parallel
- re-combines output files from all groups of 1000
- parses the XML output from InterProScan to generate a gene association file (GAF) (and several other files)


Results and analysis from the application of InterProScan annotation to the Official gene set v3.0 protein set from *Diaphorina citri* followed by a differential expression analysis was presented at a seminar in the University of Arizona Animal and Comparative Biomedical Sciences in Fall 2020. The `slides <https://www.slideshare.net/suryasaha/functional-annotation-of-invertebrate-genomes>`_ and `video <https://arizona.zoom.us/rec/play/tZZ-fuutrj43T9fBtASDAaR9W9S0fP6s1XQbrvQOz0e0VnYHYVL1MOMaZ-F4v45qOmXQkV1MUXQ7tufD>`_ are available online.


.. NOTE::

    This tool accepts a peptide FASTA file. For those users with nucloetide sequences some documentation has been provided for using **TransDecoder** (although other tools are also acceptable). 
    The `TransDecoder app <https://de.cyverse.org/de/?type=apps&app-id=74828a18-f351-11e8-be2b-008cfa5ae621&system-id=de>`_ is available through CyVerse or as a `BioContainer <https://quay.io/repository/biocontainers/transdecoder?tab=tags>`_ for use on the command line.

.. NOTE:: 

    As both GOanna and InterProScan provide GO annotations, their outputs are provided in GAF format. The **'Combine GAFs'** tool can then be used to make a single GAF of GO annotations, if desired.

**Where to Find InterProScan**
==============================

`Docker Hub (5.63-95) <https://hub.docker.com/r/agbase/interproscan>`_

`CyVerse (5.36-75) <https://de.cyverse.org/de/?type=apps&app-id=Interproscan-5.36.75u2&system-id=agave>`_
    

**Getting the InterProScan Data** 
=================================
**InterProScan Data (now includes Panther)**

.. code-block:: bash

    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.63-95.0/alt/interproscan-data-5.63-95.0.tar.gz 
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.63-95.0/alt/interproscan-data-5.63-95.0.tar.gz.md5 
    md5sum -c interproscan-data-5.63-95.0.tar.gz.md
    tar -pxvzf interproscan-data-5.63-795.0.tar.gz

.. admonition:: tar options

   - p = preserve the file permissions
   - x = extract files from an archive
   - v = verbosely list the files processed
   - z = filter the archive through gzip
   - f = use archive file


.. _iprsusage:

**Help and Usage Statement**
============================

.. _iprsusage:

.. code-block:: bash

    Options:
 -a  <ANALYSES>                             Optional, comma separated list of analyses.  If this option
                                            is not set, ALL analyses will be run.

 -b <OUTPUT-FILE-BASE>                     Optional, base output filename (relative or absolute path).
                                            Note that this option, the output directory (-d) option and
                                            the output file name (-o) option are mutually exclusive.  The
                                            appropriate file extension for the output format(s) will be
                                            appended automatically. By default the input file
                                            path/name will be used.

 -d <OUTPUT-DIR>                            Optional, output directory. Note that this option, the
                                            output file name (-o) option and the output file base (-b) option
                                            are mutually exclusive. The output filename(s) are the
                                            same as the input filename, with the appropriate file
                                            extension(s) for the output format(s) appended automatically .

 -c                                         Optional.  Disables use of the precalculated match lookup
                                            service.  All match calculations will be run locally.

 -C                                         Optional. Supply the number of cpus to use.

 -e                                         Optional, excludes sites from the XML, JSON output

 -f <OUTPUT-FORMATS>                        Optional, case-insensitive, comma separated list of output
                                            formats. Supported formats are TSV, XML, JSON, GFF3, HTML and
                                            SVG. Default for protein sequences are TSV, XML and
                                            GFF3, or for nucleotide sequences GFF3 and XML.

 -g                                         Optional, switch on lookup of corresponding Gene Ontology
                                            annotation (IMPLIES -l lookup option)

 -h                                         Optional, display help information

 -i <INPUT-FILE-PATH>                       Optional, path to fasta file that should be loaded on
                                            Master startup. Alternatively, in CONVERT mode, the
                                            InterProScan 5 XML file to convert.

 -l                                         Also include lookup of corresponding InterPro
                                            annotation in the TSV and GFF3 output formats.

 -m <MINIMUM-SIZE>                          Optional, minimum nucleotide size of ORF to report. Will
                                            only be considered if n is specified as a sequence type.
                                            Please be aware of the fact that if you specify a too
                                            short value it might be that the analysis takes a very long
                                            time!

 -o <EXPLICIT_OUTPUT_FILENAME>              Optional explicit output file name (relative or absolute
                                            path).  Note that this option, the output directory -d option
                                            and the output file basename -b option are mutually
                                            exclusive. If this option is given, you MUST specify a
                                            single output format using the -f option.  The output file
                                            name will not be modified. Note that specifying an output
                                            file name using this option OVERWRITES ANY EXISTING FILE.

 -p                                         Optional, switch on lookup of corresponding Pathway
                                            annotation (IMPLIES -l lookup option)
 -t <SEQUENCE-TYPE>                         Optional, the type of the input sequences (dna/rna (n)
                                            or protein (p)).  The default sequence type is protein.

 -T <TEMP-DIR>                              Optional, specify temporary file directory (relative or
                                            absolute path). The default location is temp/.

 -v                                         Optional, display version number
 
 -r                                          Optional. 'Mode' required ( -r 'cluster') to run in cluster mode. These options
                                            are provided but have not been tested with this wrapper script. For
                                            more information on running InterProScan in cluster mode:
                                            https://github.com/ebi-pf-team/interproscan/wiki/ClusterMode

 -R                                          Optional. Clusterrunid (crid) required when using cluster mode.
                                            -R unique_id


Available InterProScan analyses:

- CDD
- COILS
- Gene3D
- HAMAP
- MOBIDB
- PANTHER
- Pfam
- PIRSF
- PRINTS
- PROSITE (Profiles and Patterns)
- SFLD
- SMART (unlicensed components only by default - this analysis has simplified post-processing that includes an E-value filter, however you should not expect it to give the same match output as the fully licensed version of SMART)
- SUPERFAMILY
- NCBIFAM (includes the previous TIGRFAM analysis)

 OPTIONS FOR XML PARSER OUTPUTS

 -F <IPRS output directory>              This is the output directory from InterProScan.
 -D <database>                           Supply the database responsible for these annotations.
 -x <taxon>                              NCBI taxon ID of the ID being annotated
 -y <type>                               Transcript or protein
 -n <biocurator>                         Name of the biocurator who made these annotations
 -M <mapping file>                       Optional. Mapping file.
 -B <bad seq file>                       Optional. Bad input sequence file.
