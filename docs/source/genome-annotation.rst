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
the specialised programs are likely to incorporate heuristics and
family-specific information, which ensure that they out-perform the
general method, often requiring only a fraction of the compute time.

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
