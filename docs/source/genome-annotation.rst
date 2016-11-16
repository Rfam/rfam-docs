.. highlight:: console

Genome annotation
=================

Annotating non-coding RNAs in complete genomes
----------------------------------------------

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

.. note:: This example is from pages 27 and 28 of the Infernal v1.1.2
          `User's Guide <http://eddylab.org/infernal/Userguide.pdf>`_

The instructions below will walk you through how to annotate the
Methanobrevibacter ruminantium genome (NC_013790.1) for non-coding
RNAs using Rfam and Infernal. The files needed are included in the
Infernal software package, which you will download in step 1.

1. Download, build and install Infernal from `<http://eddylab.org/infernal/>`_

::

  $ wget eddylab.org/infernal/infernal-1.1.2.tar.gz
  $ tar xf infernal-1.1.2.tar.gz
  $ cd infernal-1.1.2
  $ make
   
(If you do not have wget installed and in your path, download the infernal-1.1.2.tar.gz `here:
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
any directory you want using the ./configure
--prefix option, as in ./configure --prefix /the/directory/you/want.

Additional programs from the Easel library are available in
easel/miniapps/. You can install these too if you'd like, but this is
not done by default, in case you already have a copy of Easel
separately installed::

  $ cd easel; make install

Step 4 below involves the use of one of these Easel programs (esl-seqstat).
For more information on customizing the Infernal installation, see
the Userguide that comes with Infernal

2. Download Rfam CMs from `<ftp://ftp.ebi.ac.uk/pub/databases/Rfam/12.1/Rfam.cm.gz>`_.

::

   $ wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/12.1/Rfam.cm.gz
   $ gunzip Rfam.cm.gz

(If you do not have wget installed and in your path, download the
file from a browser.)


3. Use the Infernal program cmpress to index the Rfam.cm file
   
::

   $ cmpress Rfam.cm

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

Since we want millions of nucleotides on both strands, we multiple
this by 2, and divide by 1,000,000 to get 5.874406. This number will
be used in step 5.

5. Use Infernal's cmscan program to annotate RNAs represented in Rfam in the *Methanobrevibacter ruminantium* genome.

   $ cmscan -Z 5.874406 --rfam --cut_ga --nohmmonly --tblout mrum-genome.tblout --fmt 2 --clanin testsuite/Rfam.12.1.clanin Rfam.cm tutorial/mrum-genome.fa > mrum-genome.cmscan
  
The command line options used in the above command are as follows:

  * ``-Z 5.874406`` this specifies the sequence size in millions of
      nucleotides, it is the number computed in step 4, divided by 1
      million. This option specifies that the E-values are accurate.

  * *--rfam* Specifies that the filter pipeline run in fast mode, with
       the same strict filters that are used for Rfam searches and for
       other sequence databases larger than 20 Gb     

Further reference
-----------------

There are two publications related to genome annotation using Rfam
and Infernal:

* `Annotating functional RNAs in genomes using Infernal. EP
  Nawrocki. In RNA sequence, structure and function: computational
  and bioinformatic methods, J Gorodkin, WL Ruzzo, eds. Chapter
  9. Springer Humana Press, 2014. <http://eddylab.org/publications/Nawrocki13/Nawrocki13-preprint.pdf>`_.

* `Rfam 12.0: updates to the RNA families database. EP Nawrocki, SW
  Burge, A Bateman, J Daub, RY Eberhardt, SR Eddy, EW Floden, PP
  Gardner, TA Jones, J Tate, RD Finn. Nucleic Acids Res. 43:D130-7,226-32,
  2015.

The Rfam 12.0 publication includes a section "Using Rfam 12.0 for
Genome Annotation" and Table 2 includes some running times for
annotating various sized genomes using Infernal. 

Compute
-------

Covariance model searches are extremely computionally intensive. A small
model (like tRNA) can search a sequence database at a rate of around
300 bases/sec.

The compute time scales roughly to the 4th power of the
length of the RNA, so searching larger models quickly become infeasible
without significant compute resources.

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

The generic Rfam approach has obvious advantages. However,
the specialised programs often incorporate heuristics and
family-specific information which may allow them to out-perform the
general method. A comparison of Infernal versus some of these generic
methods is presented in section 2.2 of a `2014 paper
<https://www.ncbi.nlm.nih.gov/pubmed/24639160>`_ (by one of the authors
of Infernal), `available here
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
