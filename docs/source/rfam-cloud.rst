Rfam cloud pipeline
===================

The Rfam cloud environment provides access to the command-line interface for curating Rfam families. It uses the same software and the sequence database as the ones used by the Rfam team. The pipeline allows one to create a new RNA family or update an existing Rfam entry.

.. contents::
  :local:

Background
----------

The main Rfam family building pipeline is located on the `EMBL-EBI <https://www.ebi.ac.uk/>`_ computational cluster, so that only the EBI account holders can access it. In order to enable more users to contribute to Rfam, a new version of the pipeline was developed using a `cloud infrastructure <https://www.embassycloud.org/>`_ so that the EBI accounts are not needed. All families built using the cloud pipeline are **reviewed** by the :ref:`rfam-team:Rfam team` before the families are added to Rfam.

Requirements
------------

1. A computer with internet access (Mac, Linux, or PC)
2. A command line environment supporting ``ssh`` (for example, bash)
3. An Rfam cloud account

Requesting an Rfam cloud account
--------------------------------

Please :ref:`contact-us:Contact us` to request access to the Rfam family building pipeline. If you intend to use the pipeline for teaching purposes, please let us know in advance to ensure that the pipeline can support the workload.

Connecting to Rfam cloud
------------------------

Use the login and password provided by the Rfam team to ``ssh`` to Rfam cloud::

  ssh username@rfam_ip_address

You should see a command line prompt:

.. figure:: images/rfam-cloud-cli.png
      :alt: Rfam cloud command line prompt
      :width: 600
      :align: center

      Rfam cloud command line prompt

To verify that the system works, try calling the ``rfsearch`` and ``rfmake`` scripts (you should see help messages explaining how to use the scripts)::

  rfsearch.pl -h
  rfmake.pl -h

10 steps for building an Rfam family
------------------------------------

1. Create a new folder
^^^^^^^^^^^^^^^^^^^^^^

📂 Create a new folder, for example *rfam_test* and navigate to it::

  mkdir rfam_test
  cd rfam_test

2. Prepare a SEED file
^^^^^^^^^^^^^^^^^^^^^^

Each family has a :ref:`glossary:seed alignment` file called ``SEED`` that contains a multiple sequence alignment of the confirmed instances of a family. To get started, you will need a :ref:`glossary:Stockholm format` file with at least 1 RNA sequence and a consensus secondary structure, for example see the `tRNA seed alignment <https://xfamsvn.ebi.ac.uk/svn/data_repos/trunk/Families/RF00005/SEED>`_.

If you already have a ``SEED`` file on your local computer, copy it to Rfam cloud using ``scp``::

  scp SEED:username@rfam_ip_address/rfam_test

.. HINT::
  Alternatively, create a ``SEED`` file using the `vi editor <https://www.cs.colostate.edu/helpdocs/vi.html>`_ and paste the file contents from your local computer.

If you have a `FASTA <https://en.wikipedia.org/wiki/FASTA_format>`_ file, convert it to Stockholm format and predict a consensus secondary structure. For a **single sequence** (using RNAfold)::

  predict_ss.pl -infile file.fasta -outfile SEED -r

For **multiple sequences** (using :ref:`glossary:RNAalifold`)::

  create_alignment.pl -fasta file.fasta -mu > align.stockholm
  predict_ss.pl -infile align.stockholm -outfile SEED -ra

Once you have a Stockholm file called ``SEED`` in your working directory, proceed to the next step.

3. Find similar sequences using rfsearch
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Build a :ref:`glossary:Covariance model (CM)` based on your seed alignment and search for similar sequences in the :ref:`glossary:rfamseq` database::

  rfsearch.pl -nodesc -relax -t 30

.. list-table::

    * - Option
      - Meaning
    * - ``-nodesc``
      - creates a required file called ``DESC`` that contains the description of the family. You only need to use the ``-nodesc`` flag the first time you run rfsearch, after that you will get an error if you use ``-nodesc`` because a ``DESC`` file already exists.
    * - ``-relax``
      - allow sequences not found in the :ref:`glossary:rfamseq` database to be included in the seed alignment
    * - ``-t 30``
      - :ref:`glossary:Gathering cutoff` in bits. Usually 30 bits is a good starting point as most families are expected to have a threshold higher than 30.

⚠️ **This step can take a long time** depending on the size of the alignment and the availability of computational resources.

4. Choose a gathering threshold
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The output files (``species``, ``outlist``, and ``taxonomy``) should be used to determine the gathering threshold for this family (the bit score of the last true positive hit). For more information, see :ref:`choosing-gathering-threshold:Choosing gathering threshold`.

5. Add sequences to SEED (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :ref:`glossary:seed alignment` needs to represent the taxonomic diversity and the structural features observed in different instances of the family. A seed alignment needs to have **at least 2 sequences** but a larger seed alignment is preferred.

Find an accession in the ``outlist`` file that you would like to add to the ``SEED`` (for example, ``AB480043.1``)::

  grep AB480043.1 outlist >> addme
  rfseed.pl addme

To remove sequences from ``SEED`` (if added in error, for example), create a file with a list of accessions you want to remove using ``grep`` as described above and call it *removeme*. Make sure the accession is exactly the same as in the ``SEED`` file, for example ``NW_002196667.1/1438869-1438941``. Then run the following command::

  rfseed.pl -d -n removeme

Consider **manually editing the alignment** on your local computer using `RALEE <http://sgjlab.org/ralee/>`_ or `belvu <http://sonnhammer.sbc.su.se/Belvu.html>`_ and re-uploading it as explained in **Step 1**.

6. Repeat rfsearch with a new threshold
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

🔄 Steps 5 and 6 should be repeated until the seed alignment can no longer be improved::

  rfsearch.pl -t new_cutoff

This process is known as `iteration <https://rfam.readthedocs.io/en/latest/building-families.html#expanding-the-seed-iteration>`_.

7. Create required family files with rfmake
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once the cutoff has been chosen, all the required family files can be generated like this::

  rfmake.pl -t gathering_cutoff -a

The ``-a`` option creates an ``align`` file with an alignment of all the sequences above the gathering threshold. Reviewing the ``align`` file can help to adjust the threshold, as the unwanted sequences can be excluded by rerunning rfmake with a higher threshold ``-t``.

8. Add metadata to the DESC file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each family is described using in a ``DESC`` file (see the `tRNA DESC file <https://xfamsvn.ebi.ac.uk/svn/data_repos/trunk/Families/RF00005/DESC>`_ as an example). The following fields are required:

:ID:
    a unique ID, such as *tRNA* or *skipping-rope*. No spaces are allowed.
:DE:
  | a short description of the family.
  | Example: ``DE   GlmZ RNA activator of glmS mRNA``
  | ⚠️ Maximum **75 characters**.

:AU:
    Author name with an `ORCID <https://orcid.org/>`_ id. Multiple ``AU`` lines can be used.
    Example: ``AU   Eddy SR; 0000-0001-6676-4706``
:SE:
    Seed alignment source. Example: ``SE   Published; PMID:21994249;``
:SS:
    Secondary structure source.
    Examples:

    - ``SS   Published; PMID:28977401;``
    - ``SS   Predicted; mfold;``

:TP:
    One of Rfam `RNA types <https://rfam.readthedocs.io/en/latest/searching-rfam.html#search-by-entry-type>`_.
    Example: `TP   Gene; sRNA;`
:DR:
    A reference to a `Gene Ontology <http://geneontology.org/>`_ or `Sequence Ontology <http://sequenceontology.org/>`_ term. Multiple ``DR`` lines can be used. Example:

    - ``DR   SO; 0000253; tRNA;``
    - ``DR   GO; 0030533; triplet codon-amino acid adaptor activity;``

    You may find the `QuickGO <https://www.ebi.ac.uk/QuickGO/>`_ website useful for finding GO terms.
    A link to a website can also be included, for example: ``DR   URL; http://telomerase.asu.edu/;``
:CC:
    A free text comment describing what is known about the RNA (function, taxonomic distribution, experimental validation etc).
    ⚠️ Maximum **80 characters per line**, but multiple ``CC`` lines can be used.
:WK:
    A `Wikipedia <https://en.wikipedia.org/>`_ link (you should create a new Wikipedia article or link to an existing one).
    Example: ``WK   Transfer_RNA``

📚 To add literature references, use the following command that automatically imports information from `PubMed <https://www.ncbi.nlm.nih.gov/pubmed/>`_::

  add_ref.pl pubmed_id

⚠️ The ``GA``, ``TC``, ``NC``, ``BM``, ``CV``, ``SM`` lines are added automatically, please do not change them manually. The ``RN``, ``RM``, ``RT``, ``RA``, and ``RL`` lines are added by the ``add_ref.pl`` script. The ``AC`` field is assigned once the family is stored in the official Rfam database.

9. Perform quality control checks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``rqc-all`` script performs multiple quality controls on the family. It checks the file formats, the accessions, and the ``DESC`` file::

  cd .. && rqc-all.pl rfam_test

10. Send SEED and DESC files for review
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download your ``SEED`` and ``DESC`` files to your local machine::

  scp username@rfam_ip_address/rfam_test/SEED:.
  scp username@rfam_ip_address/rfam_test/DESC:.

`Email <https://rfam.readthedocs.io/en/latest/contact-us.html>`_ the files to the Rfam team for review. 🎉🎉🎉

.. DANGER::
  We encourage you to **always keep a local copy of the important data**!

Updating an existing Rfam family
--------------------------------

The only difference between creating a new family and updating an existing one is that the ``SEED`` and ``DESC`` files are retrieved from Rfam::

  rfco.pl RF0XXXX

After that, follow the family building instructions from **Step 3**.

Questions or comments?
----------------------

:ref:`contact-us:Contact us` or `raise an issue <https://github.com/Rfam/rfam-family-pipeline/issues>`_ on GitHub.
