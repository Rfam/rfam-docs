.. highlight:: console

Genome annotation
=================

Before trying to annotate your own genome sequences on your local
hardware or submitting lots of sequences to Rfam via the website, please
check that the following resources do not provide the annotation for you:

* `Ensembl <http://www.ensembl.org>`_
* `Ensembl Genomes <http://ensemblgenomes.org/>`_
* `UCSC Genome Browser <http://www.genome.ucsc.edu/>`_

The Rfam library of covariance models can be used to search sequences
(including whole genomes) for homologues to known non-coding RNAs, in
conjunction with the `Infernal software <http://eddylab.org/infernal/>`_.

Example of using Infernal and Rfam to annotate RNAs in an archaeal genome
-------------------------------------------------------------------------

.. .. note:: This example is from pages 27 and 28 of the Infernal v1.1.2
          `User's Guide <http://eddylab.org/infernal/Userguide.pdf>`_

The instructions below will walk you through how to annotate the
*Methanobrevibacter ruminantium* genome (NC_013790.1) for non-coding
RNAs using Rfam and Infernal. The files needed are included in the
Infernal software package, which you will download in step 1.

1. Download, build and install Infernal from `<http://eddylab.org/infernal/>`_

::

  $ wget eddylab.org/infernal/infernal-1.1.2.tar.gz
  $ tar xf infernal-1.1.2.tar.gz
  $ cd infernal-1.1.2
  $ make
   
(If you do not have wget installed and in your path, download infernal-1.1.2.tar.gz `here:
<http://eddylab.org/infernal/infernal-1.1.2.tar.gz>`_.)

To compile and run a test suite to make sure all is well, you can
optionally do::
  
  $ make check

You don’t have to install Infernal programs to run them. The newly
compiled binaries are now in the src directory. You can run them
from there. To install the programs and man pages somewhere on your
system, do::
    
  $ make install

By default, programs are installed in /usr/local/bin and man pages
in /usr/local/share/man/man1/. You can change the /usr/localprefix to
any directory you want using the ``./configure
--prefix`` option, as in ``./configure --prefix /the/directory/you/want``.

Additional programs from the Easel library are available in
easel/miniapps/. You can install these too if you'd like, but this is
not done by default, in case you already have a copy of Easel
separately installed::

  $ cd easel; make install

Step 4 below involves the use of one of these Easel programs (esl-seqstat).
For more information on customizing the Infernal installation, see
the Userguide that comes with Infernal

2. Download the Rfam library of CMs from `<ftp://ftp.ebi.ac.uk/pub/databases/Rfam/12.1/Rfam.cm.gz>`_.

::

   $ wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/12.1/Rfam.cm.gz
   $ gunzip Rfam.cm.gz

(If you do not have wget installed and in your path, download the
file from a browser.)

3. Use the Infernal program cmpress to index the Rfam.cm file
   
::

   $ cmpress Rfam.cm

This step is required before cmscan can be run in step 5.
   
4. Determine the total database size for the genome you are annotating. 

For the purposes of Infernal, the total database size is the number of
nucleotides that will be searched, in units of megabases (Mb, millions
of nucleotides). So, it is the total number of nucleotides in all
sequences that make up the genome, multiplied by two (because both
strands will be searched), and divided by 1,000,000 (to convert to
millions of nucleotides).

You will need to supply this number to Infernal to assure that the
E-values reported by the cmscan program run in the next step are
accurate. 

You can use the esl-seqstat program from the Easel
library that you built along with Infernal in step 1 to help with
this. For this example, we will be annotating the genome of 
*Methanobrevibacter ruminantium*, an archaeon. The sequence file with
this genome can be found in infernal-1.1.2/tutorial/, which you
created in step 1. To determine the total size of this genome, do::
  
  $ esl-seqstat infernal-1.1.2/mrum-genome.fa

The output will include a line reporting the total number of
nucleotides::

  Total # of residues: 2937203

Because we want millions of nucleotides on both strands, we multiple
this by 2, and divide by 1,000,000 to get 5.874406. This number will
be used in step 5.

5. Use the cmscan program to annotate RNAs represented in Rfam in the *Methanobrevibacter ruminantium* genome.
   
::
   
   $ cmscan -Z 5.874406 --cut_ga --rfam --nohmmonly --tblout mrum-genome.tblout --fmt 2 --clanin testsuite/Rfam.12.1.clanin Rfam.cm tutorial/mrum-genome.fa > mrum-genome.cmscan

.. note:: The above cmscan command assumes you are in the
          infernal-1.1.2 directory from step 1. If not, you'll need to
          supply the paths to the tutorial/mrum-genome.fa and
          testsuite/Rfam.12.1.clanin files within the infernal-1.1.2
          directory.
   
Explanations of the command line options used in the above command are as follows:

:``-Z 5.874406``:
   the sequence database size in millions of nucleotides is 5.874406, it is the
   number computed in step 4. This option
   ensures that the reported E-values are accurate.
 
:``--cut_ga``:
   specifies that the special Rfam GA (gathering) thresholds be used
   to determine which hits are reported. These thresholds are stored
   in the Rfam.cm file. Each model has its own GA bit score threshold,
   which was determined by Rfam curators as the bit score at and above
   which all hits are believed to be true homologs to the model. These
   determinations were made based on observed hit results against the
   large Rfamseq database used by Rfam (`Nawrocki et al., 2015 <http://nar.oxfordjournals.org/content/43/D1/D130>`_).
   
:``--rfam``:
   run in "fast" mode, the same mode used for
   Rfam annotation and determination of GA thresholds
   
:``--nohmmonly``:
   all models, even those with zero basepairs, are run in CM mode (not
   HMM mode). This ensures all GA cutoffs, which were determined in CM
   mode for each model, are valid.
   
:``--tblout``:
   a tabular output file will be created.

:``--fmt 2``:
   the tabular output file will be in format 2, which includes
   annotation of overlapping hits.

:``--clanin``:
   Clan information should be read from the file
   ``testsuite/Rfam.12.1.claninfo``. This file lists which models belong
   to the same clan. Clans are groups of models that are homologous and
   therefore it is expected that some hits to these models will
   overlap. For example, the LSU rRNA archaea and LSU rRNA bacteria
   models are both in the same clan.


Expected Running Times
-------

CM searches are computationally expensive and searching large multi-Gb
genomes with the roughly 2500 models in Rfam takes hundreds of CPU
hours. However, you can parallelize by splitting up the input genome
sequence file into multiple files (if the genome has multiple
chromosomes) and running cmscan separately on each individual
file. Also, you can run cmscan with multiple threads, as explained
more below.

The following timings are from Table 2 of (`Nawrocki et al., 2015
<http://nar.oxfordjournals.org/content/43/D1/D130>`_). All searches
were run as single execution threads on 3.0 GHz Intel Xeon
processors. 

+------------------------------------+------------+------------------+---------+
| Genome                             | Size (Mb)  | CPU time (hours) | Mb/hour |
+====================================+============+==================+=========+
| *Homo sapiens*                     | 3099.7     | 650              | 4.8     |
+------------------------------------+------------+------------------+---------+
| *Sus scrofa (pig)*                 | 2808.5     | 460              | 6.1     |
+------------------------------------+------------+------------------+---------+
| *Caenorhabditis elegans*           | 100.3      | 20               | 5.2     |
+------------------------------------+------------+------------------+---------+
| *Escherichia coli*                 |        4.6 | 0.46             |10.2     |
+------------------------------------+------------+------------------+---------+
| *Methanocaldococcus jannaschii*    |        1.7 | 0.31             | 5.6     |
+------------------------------------+------------+------------------+---------+

.. | *Drosophila melanogaster*       | 168.7      | 30               | 5.7     |
.. | Human immunodeficiency virus (HIV) |        1.7 | 0.31             | 5.6     |

cmscan will run in multithreaded mode by default, if
multiple processors are available. Running with 8 threads with 8 cores
should reduce the running times listed in the table above by about
4-fold (reflecting about 50% efficiency versus single threaded).

Specificity
-----------

The Rfam/Infernal approach aims to be sufficiently generic to cope with
**all types of RNAs**. A sequence can be searched using every model in
exactly the same way.

In contrast, several tools are available that search for specific types of
RNA, such as

* `tRNAscan-SE <http://lowelab.ucsc.edu/tRNAscan-SE/>`_ for tRNAs,
* `RNAMMER <http://www.cbs.dtu.dk/services/RNAmmer/>`_ for rRNA,
* `snoscan <http://lowelab.ucsc.edu/snoscan/>`_ for snoRNAs, and
* `SRPscan <http://bio.lundberg.gu.se/srpscan/>`_ for SRP RNA.

The generic Rfam approach has obvious advantages. However, the
specialised programs often incorporate heuristics and family-specific
information which may allow them to out-perform the general method. A
comparison of Infernal versus some of these generic methods is
presented in section 2.2 of a `2014 paper
<https://www.ncbi.nlm.nih.gov/pubmed/24639160>`_ (by one of the
authors of Infernal), `available here
<http://eddylab.org/publications/Nawrocki13/Nawrocki13-preprint.pdf>`_.

Pseudogenes
-----------

ncRNA derived pseudogenes pose the biggest problem for eukaryotic
genome annotation using Rfam/Infernal. Many genomes contain **repeat elements**
that are derived from a non-coding RNA gene, sometimes in
huge copy number. For example, `Alu repeats <https://en.wikipedia.org/wiki/Alu_element>`_
in human are evolutionarily related to `SRP RNA <https://en.wikipedia.org/wiki/Signal_recognition_particle>`_,
and the active `B2 SINE <http://lncrnadb.org/b2sinerna/>`_
in mouse is recently derived from a tRNA.

In addition, specific RNA genes appear to have
undergone massive **pseudogene expansions** in certain genomes. For
example, searching the human genome using the Rfam
`U6 family <http://rfam.xfam.org/family/RF00026>`_
yields over 1000 hits, all with very high score. These are not "false
positives" in the sequence analysis sense, because they are closely
related by sequence to the real U6 genes, but they completely
overwhelm the small number (only 10s) of expected real U6 genes.

At present we don't have computational methods to distinguish the real
genes from the pseudogenes (of course the standard protein coding gene
tricks - in frame stop codons and the like - are useless). The sensible
and precedented method for ncRNA annotation in large vertebrate genomes
is to annotate the easy-to-identify RNAs, such as tRNAs
and rRNAs, and then trust only hits with very high sequence identity
(>95% over >95% of the sequence length) to an experimentally
verified real gene. `tRNAscan-SE <http://lowelab.ucsc.edu/tRNAscan-SE/>`_ has a
very nice method for detecting tRNA pseudogenes.

.. DANGER::
  We recommend that you use Rfam/Infernal for vertebrate genome
  annotation with **extreme caution**!

Nevertheless, Rfam/Infernal does tell us about important
sequence similarities that are effectively undetectable by other means.
However, in complex eukaryotic genomes, it is important to treat hits as
sequence similarity information (much as you might treat BLAST hits),
rather than as evidence of bona fide ncRNA genes.
