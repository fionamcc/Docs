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

    InterProScan version 5.35-74.0 does not include the XML parser to generate GAF file and associated count files. To get these files the user must run the `InterProScan Results function <https://de.cyverse.org/de/?type=apps&app-id=714e28fa-6580-44fc-9756-8b019c192449&system-id=de>`_ app.

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

.. _iprsusage:

**Help and Usage Statement**
============================

.. _iprsusage:

.. code-block:: none

    Options:
 -a  <ANALYSES>                             Optional, comma separated list of analyses.  If this option
                                            is not set, ALL analyses will be run.

 -b, <OUTPUT-FILE-BASE>                     Optional, base output filename (relative or absolute path).
                                            Note that this option, the output directory (-d) option and
                                            the output file name (-o) option are mutually exclusive.  The
                                            appropriate file extension for the output format(s) will be
                                            appended automatically. By default the input file
                                            path/name will be used.

 -d,<OUTPUT-DIR>                            Optional, output directory. Note that this option, the
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
 Available analyses:
                      TIGRFAM (XX.X) : TIGRFAMs are protein families based on Hidden Markov Models or HMMs
                         SFLD (X.X) : SFLDs are protein families based on Hidden Markov Models or HMMs
                        ProDom (XXXX.X) : ProDom is a comprehensive set of protein domain families automatically generated from the UniProt Knowledge Database.
                        Hamap (XXXXXX.XX) : High-quality Automated and Manual Annotation of Microbial Proteomes
                        SMART (X.X) : SMART allows the identification and analysis of domain architectures based on Hidden Markov Models or HMMs
                          CDD (X.XX) : Prediction of CDD domains in Proteins
              ProSiteProfiles (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
              ProSitePatterns (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
                  SUPERFAMILY (X.XX) : SUPERFAMILY is a database of structural and functional annotation for all proteins and genomes.
                       PRINTS (XX.X) : A fingerprint is a group of conserved motifs used to characterise a protein family
                      PANTHER (X.X) : The PANTHER (Protein ANalysis THrough Evolutionary Relationships) Classification System is a unique resource that classifies genes by their functions, using published scientific experimental evidence and evolutionary relationships to predict fu$
                       Gene3D (X.X.X) : Structural assignment for whole genes and genomes using the CATH domain structure database
                        PIRSF (X.XX) : The PIRSF concept is being used as a guiding principle to provide comprehensive and non-overlapping clustering of UniProtKB sequences into a hierarchical order to reflect their evolutionary relationships.
                         Pfam (XX.X) : A large collection of protein families, each represented by multiple sequence alignments and hidden Markov models (HMMs)
                        Coils (X.X) : Prediction of Coiled Coil Regions in Proteins
                   MobiDBLite (X.X) : Prediction of disordered domains Regions in Proteins

 OPTIONS FOR XML PARSER OUTPUTS

 -F <IPRS output directory>              This is the output directory from InterProScan.
 -D <database>                           Supply the database responsible for these annotations.
 -x <taxon>                              NCBI taxon ID of the ID being annotated
 -y <type>                               Transcript or protein
 -n <biocurator>                         Name of the biocurator who made these annotations
 -M <mapping file>                       Optional. Mapping file.
 -B <bad seq file>                       Optional. Bad input sequence file.