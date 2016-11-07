Building Rfam families
======================

*rfamseq* database
----------------

The underlying nucleotide sequence database from which we build our
families (known as *rfamseq*) is derived from the `European Nucleotide Archive <http://www.ebi.ac.uk/ena/>`_.

We include Standard (STD) and Whole Genome Shotgun (WGS) data
classes. This includes all the environmental sample sequences (ENV)
but we currently exclude the patented (PAT) and synthetic (SYN)
divisions. You should note that *rfamseq* does NOT include Expressed
Sequence Tag (EST) or Genome Survey Sequence (GSS) data classes.

*rfamseq* is usually updated with each major Rfam release, e.g., 8.0, 9.0.
You can find out the ENA release currently in use in the
`README file <ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/README>`_ on our FTP site.

Seed alignments and secondary structure annotation
--------------------------------------------------

Our **seed alignments** are small, curated sets of representative sequences
for each family, as opposed to an alignment of all known members. The
seed alignment also has as a **secondary structure** annotation, which
represents the conserved secondary structure for these sequences.

The ideal basis for a new family is an RNA element that:

* has some known functional classification
* is evolutionarily conserved
* has evidence for a secondary structure

In order to build a new family, we
must first obtain at least one **experimentally validated example** from
the published literature. If any other homologues are identified in the
literature, we will add these to the seed. Alternatively, if these are
not available, we will try to identify others members either by
similarity searching (using BLAST) or manual curation.

Where possible we will use a multiple sequence alignment and
secondary structure annotation provided in the literature. If this is
the case, we will cite the source of both the alignment and the
secondary structure. You should note that the structure annotations
obtained from the literature may be experimentally validated or they
may be RNA folding predictions (commonly `Mfold <http://unafold.rna.albany.edu/?q=mfold>`_).
Unfortunately, we do not discriminate between these two cases when we
site the PubMed Identifier (PMID) and you will need to refer to the
original publications to clarify.

Alternatively, where this information is not available from the
literature, we will generate an alignment and secondary structure
prediction using various software, such as `WAR <http://genome.ku.dk/resources/war>`_. This
software allows us to cherry pick the best alignment and secondary
structure prediction. Historically, the methods used to
make these alignments and folding predictions have varied.
Author names added to the list indicate that the published or predicted
secondary structure has been manually curated in some way. The last
author on the list will be the most recent editor of the secondary
structure. You can
find the method we have used for the seed alignment or the secondary
structure annotation in the **SE** and **SS**
lines of the `Stockholm format <https://en.wikipedia.org/wiki/Stockholm_format>`_
or in the curation information pages.

Covariance Models
-----------------

From the seed alignment, we use the `Infernal software <http://eddylab.org/infernal/>`_ to build a
probabilistic model (covariance model or CM) for this family. Useful
references on stochastic free grammars and covariance models can be
found in the :ref:`Citing Rfam`
section. This model is then used to search the *rfamseq*
database for other possible homologs.

Searching a nucleotide database as larger as *rfamseq* with a covariance
model is hugely computationally expensive. In order to do this in
reasonable time, we use sequence based filters to prune the search
space prior to applying the CMs. Please refer to the recent Rfam
publication for more details on how we implement this.

Expanding the seed (iteration)
------------------------------

If the CM search of *rfamseq* identifies any homologs that we believe
would improve the seed, we use the Infernal software (cmalign) to
add these sequences to the seed alignment. From the new seed, the CM
is re-built and re-searched against *rfamseq*. We refer to this process
of expanding the seed using Infernal searching as "iteration". We
continue to iterate the seed until we have good resolution
between real and false hits and cannot improve the seed membership
further.

Important points to remember about our seed alignments
------------------------------------------------------

* We can only build families using the sequences in *rfamseq*
* We can only build a family where we can identify more than one
  sequence in *rfamseq*
* Sequences in the seed cannot be manually altered in any way,
  e.g. no manual excision of introns, no editing of sequencing errors,
  no marking up modified nucleotides etc
* At least one sequence in the seed will have some experimental
  evidence of transcription, e.g. northern blot or RT-PCR, and,
  preferably, some evidence of function
* The secondary structure should ideally have some experimental
  support (such as structure probing, NMR or crystallography)
  and/or evidence of evolutionary conservation. We do, however, use and
  generate predicted structures
* Each seed sequence will be a significant match to the corresponding
  covariance model. A significant score is generally greater than 20
  bits

Rfam full alignments
--------------------

The Rfam full alignments contain all of the sequences in *rfamseq* that
we can identify as members of the family. The alignment is generated by
searching the covariance model for the family against the *rfamseq*
database. Matches that score above a curated threshold are aligned to
the CM to produce the full alignment. All sequences in the seed will
also be present in the full  alignment. You should read the
`curation information <TODO>`_ pages for details of bit scores and gathering
thresholds.

As of Rfam 12.0, we no longer automatically generate full alignments for
each Rfam family. You may download the Rfam CM and generate your own alignments.

Family annotation
-----------------

In order to provide some background and functional information about
a family, we link to a `Wikipedia <http://www.wikipedia.org/>`_
page that provides relevant background information on
the family. We have either linked to an existing page or we have created
the page ourselves in Wikipedia. As this annotation is hosted by
Wikipedia, you are free to edit, correct and otherwise improve
this annotation and we would encourage you to do so.

Phylogenetic trees
------------------

All our phylogenetic trees are now generated using `fasttree <http://www.microbesonline.org/fasttree/>`_.
