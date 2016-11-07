Glossary
========
These are some of the commonly used terms in the Rfam website.

ClustalW
--------

A general purpose multiple sequence alignment program for DNA(RNA) which we use while building our SEED alignments. See the `Clustal web server <http://www.clustal.org>`_.

Covariance model (CM)
---------------------

A secondary structure profile for a RNA structural alignment (also called profile stochastic context-free grammars).

Family
------

A group of RNA sequences which we believe are evolutionarily related in sequence or secondary structure.

Full alignment
--------------

An alignment of the set of related sequences which score higher than the manually set threshold values for the CMs of a particular Rfam family.

Gathering cutoff
----------------

The bit score gathering threshold (GA cutoff), set by our curators when building the family. All sequences that score at or above this threshold will be included in the full alignment.

Infernal
--------

`Infernal <http://eddylab.org/infernal/>`_  is the core software that enables us to make consensus RNA secondary structure profiles (covariance models (CMs)) for our families. We also use Infernal for searching sequence databases for homologous RNAs. See the Infernal website.

MFOLD
-----

RNA structure prediction algorithm which utilises minimum free energy information. See the `MFOLD <http://unafold.rna.albany.edu/?q=mfold>`_ website.

Pfold
-----

RNA folding software which folds alignments using a Stochastic Context-Free Grammars (SCFG) trained on rRNA alignments. It takes an alignment of RNA sequences as input and predicts a common structure for all sequences. See the `Pfold <http://www.daimi.au.dk/~compbio/rnafold/>`_ website.

rfamseq
-------

The underlying nucleotide sequence database on which Rfam is based. It is derived from the `EMBL nucleotide database <http://www.ebi.ac.uk/ena>`_.

RNAalifold
----------

Folds pre-computed alignments using a combination of free-energy and covariation measures. Part of the `Vienna package <http://www.tbi.univie.ac.at/RNA/>`_.

Seed alignment
--------------

A manually curated sample of representative sequences for a family. These sequences are aligned and annotated with a consensus secondary structure. This alignment is used to build the covariance model for the family.

Sequence region
---------------

A single segment of nucleotide sequence in our alignments. Multiple sequence regions from a single EMBL sequence may be in the same family.

Type
----

A simple functional classification we use for our families. This is our own ontology and does not current directly relate to the ontologies used by other databases. For a full list of our current ontologies see the help section on `searching by family type <TO DO>`_.

Stockholm format
----------------

A multiple sequence alignment format used by Rfam (and Pfam) for the dissemination of protein and RNA sequence alignments. For more information see the `Wikipedia article on Stockholm format <https://en.wikipedia.org/wiki/Stockholm_format>`_.

WAR
---

A software tool that enables us to simultaneously run several different methods for performing multiple alignment and secondary structure prediction for non-coding RNA sequences. See the `WAR  <http://genome.ku.dk/resources/war/>`_ website.
