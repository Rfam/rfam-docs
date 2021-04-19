Searching Rfam
==============

In addition to the quick links on the home page, every page in the Rfam
site includes a `Search <http://rfam.org/search>`_ link in the page header, which you can use to
access all of the search methods that we offer:

.. contents::
  :local:

.. HINT::
  For more details about searching Rfam, please see our `paper in Current Protocols in Bioinformatics <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6754622>`_.

Text search
------------

.. image:: images/text-search.png
   :alt: Screenshot of Rfam text search

The Rfam text search, available on the `Rfam homepage <http://rfam.org>`_
or at the top of any Rfam page, replaces the older search options, such as
*Keyword search*, *Taxonomy search*, and *browsing* entries by type.

Using the Rfam text search one can:

* explore Rfam by category using **facets**
* **sort** results
* **bookmark and share** search URLs

**Examples**:

* `families <http://rfam.org/search?q=entry_type:%22family%22>`_
* `clans <http://rfam.org/search?q=entry_type:%22clan%22>`_
* `motifs <http://rfam.org/search?q=entry_type:%22motif%22>`_
* `families with 3D structures <http://rfam.org/search?q=entry_type:%22Family%22%20AND%20has_3d_structure:%22Yes%22>`_
* `snoRNA families that match human sequences <http://rfam.org/search?q=rna_type:%22snoRNA%22%20AND%20TAXONOMY:%229606%22>`_

Text search API
^^^^^^^^^^^^^^^

Text search is powered by the `EBI search <http://www.ebi.ac.uk/ebisearch/overview.ebi>`_
which supports a `REST API <http://www.ebi.ac.uk/ebisearch/documentation.ebi>`_
that can be used to access the Rfam data programmatically in addition to the :ref:`api:Rfam API`.

Here is an example query that retrieves riboswitch families as well as their descriptions
and the number of sequences in seed alignments::

    https://www.ebi.ac.uk/ebisearch/ws/rest/rfam?query=riboswitch&format=json&fields=num_seed,description

Here is `full list of fields <http://www.ebi.ac.uk/ebisearch/metadata.ebi?db=rfam>`_ that can be retrieved
using the text search API.

-------------------------

Sequence search
---------------

Searching a nucleotide sequence (DNA or RNA) against the Rfam library
of covariance models will identify any regions in your sequence we
would classify as belonging to one our RNA families.

Single sequence search
^^^^^^^^^^^^^^^^^^^^^^

The `Rfam sequence search <https://rfam.org/search#tabview=tab1>`_ is integrated with the `RNAcentral sequence search <https://rnacentral.org/sequence-search>`_ so that in addition to the Rfam classification each query sequence is also searched against a comprehensive collection of ncRNA sequence from RNAcentral. The predicted secondary structure, if available, is visualised in standard orientations using `R2DT <https://rnacentral.org/r2dt>`_.

.. image:: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7779021/bin/gkaa1047fig4.jpg
   :alt: Screenshot of Rfam sequence search

Medium scale batch searches (less than 1,000 sequences)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you have multiple nucleotide sequences to search, you can use our
batch upload facility to upload a file of your sequences in FASTA
format. Information on the format for this file can be found under the
more link `here <http://rfam.org/search>`_. We will
search your sequences against the Rfam library of covariance models and email the results
back to you, usually within 48 hours. We request that you search a
maximum of 1000 sequences in each file. Each sequence may be up to 200kb
in length.

Large scale batch searches (more than 1,000 sequences)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you have a large number of nucleotide searches, it may be more
convenient to run Infernal searches locally (see section :ref:`genome-annotation:Genome annotation`).

EBI cmscan search
^^^^^^^^^^^^^^^^^

The `EBI cmscan service <https://www.ebi.ac.uk/Tools/rna/infernal_cmscan/>`_ uses the same version of Infernal and the same set of Rfam covariance models as the Rfam website.

-------------------------

Other ways to search Rfam
-------------------------

Search by entry type
^^^^^^^^^^^^^^^^^^^^

.. WARNING::

  Entry type search will soon be replaced by the text search.

You can `search by entry type <http://rfam.org/search#tabview=tab4>`_
to view or download a list of families by type.

Here is a list of Rfam ncRNA types:

* Cis-reg;

  * Cis-reg; IRES;
  * Cis-reg; frameshift_element;
  * Cis-reg; leader;
  * Cis-reg; riboswitch;
  * Cis-reg; thermoregulator;

* Gene;

  * Gene; CRISPR;
  * Gene; antisense;
  * Gene; miRNA;
  * Gene; rRNA;
  * Gene; ribozyme;
  * Gene; sRNA;
  * Gene; snRNA;
  * Gene; snRNA; snoRNA; CD-box;
  * Gene; snRNA; snoRNA; HACA-box;
  * Gene; snRNA; snoRNA; scaRNA;
  * Gene; snRNA; splicing;
  * Gene; tRNA;

* Intron;

.. TIP::

  If you would like to download results as text, click **Show the unformatted list**
  at the bottom of the `search results page <http://rfam.org/search#tabview=tab4>`_.

-----------------------------

Taxonomy search
^^^^^^^^^^^^^^^

.. WARNING::

  Taxonomy search search will soon be replaced by text search.

This is one of the more interesting and powerful ways to search Rfam.
Using the taxonomy search form, you can identify families
that are specific to a given taxonomic level or those found in a given
set of  taxonomic levels. You can also limit your queries to those
families which are found only in a single species or taxonomic
level. Please read the information under the "More..." link on the
`taxonomy search page <http://rfam.org/search#tabview=tab3>`_
for details on how to use this search.
