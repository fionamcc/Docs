==========
**Intro**
==========
- Pathannotator annotates proteins with KEGG and Flybase pathways. It does this through the use of KofamScan, KEGG API and Flybase.org.
- KofamScan is a gene functional annotation tool based on KEGG Orthology and hidden Markov model (HMM). It is provided by the KEGG (Kyoto Encyclopedia of Genes and Genomes) project. The online version is available here: https://www.genome.jp/tools/kofamkoala/ .
- This pipeline pulls annotation directly from the KEGG API when possible. When that isn't possible the pipeline impliments Kofamscan to identify homologous KEGG objects (KO). The pathways annotated to these KEGG objects can then be transfered to the corresponding proteins in your species of interest.
- If specified, the pipeline will also provide annotations to Flybase pathways.


**Where to Find Pathannotator**
=========================================

Pathannotator is provided as a Docker container for use on the command line.


- `Docker Hub <https://hub.docker.com/r/agbase/pathannotator>`_

The Dockerfile and scripts are available on `GitHub <https://github.com/AgBase/pathannotator>`_


**Getting the KOfam Databases**
===============================

The KOfam 'profiles' and 'ko_list' databases are required to run the pipeline. If you don't already have these databases the pipeline will pull them during the first run.
If you want to download them beforehand they are available from the KEGG website.

    Using wget:

    .. code-block:: bash

        wget https://www.genome.jp/ftp/db/kofam/profiles.tar.gz

        wget https://www.genome.jp/ftp/db/kofam/ko_list.gz

If your species of interest has been annotated by the KEGG project you can provide this tool with the corresponding KEGG species code to pull those annotations directly. If your species of interest is not listed you should choose a closely related species and use that code.
KEGG species codes can be found here: https://www.genome.jp/brite/br08611


**Help and Usage Statement**
============================
On the command line the following help statement can be displayed with 'help'.

.. code-block:: none

    Help and Usage:
        There are 4 positional arguments.
        1: KEGG species code (NA or related species code if species not in KEGG; 'help' to see this help and usage statement)
           KEGG species codes can be found here: https://www.genome.jp/brite/br08611
        2: input file (protein FASTA without header lines)
        3: output directory (must be an existing directory)
        4: 'FB' for flybase annotations, 'NA' for none

        KofamScan is used under an MIT License:

        Copyright (c) 2019 Takuya Aramaki

        Permission is hereby granted, free of charge, to any person obtaining a copy
        of this software and associated documentation files (the "Software"), to deal
        in the Software without restriction, including without limitation the rights
        to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
        copies of the Software, and to permit persons to whom the Software is
        furnished to do so, subject to the following conditions:

        The above copyright notice and this permission notice shall be included in all
        copies or substantial portions of the Software.

        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
        IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
        FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
        AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
        LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
        OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
        SOFTWARE."
