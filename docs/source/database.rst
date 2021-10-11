Public MySQL Database
======================

Rfam provides a public read-only `MySQL <https://www.mysql.com/>`_ database
containing the latest version of Rfam data. The database will be updated with each release.
To access old versions of the database download SQL dumps
from the `FTP archive <https://ftp.ebi.ac.uk/pub/databases/Rfam/>`_.

.. HINT::
  For advanced examples of using the public database, please see our `paper in Current Protocols in Bioinformatics <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6754622>`_.

Connection details
------------------

+--------------+------------------------------+
| Parameter    |  Value                       |
+==============+==============================+
| host         |  mysql-rfam-public.ebi.ac.uk |
+--------------+------------------------------+
| user         | rfamro                       |
+--------------+------------------------------+
| password     | none                         |
+--------------+------------------------------+
| port         | 4497                         |
+--------------+------------------------------+
| database     | Rfam                         |
+--------------+------------------------------+

You can connect to the database on command line:

.. code-block:: bash

  mysql --user rfamro --host mysql-rfam-public.ebi.ac.uk --port 4497 --database Rfam

or use MySQL clients such as `MySQL Workbench <http://dev.mysql.com/downloads/workbench/>`_ or `Sequel Pro <http://www.sequelpro.com/>`_.

If your computer is behind a firewall, please ensure that outgoing TCP/IP connections to the corresponding ports are allowed.

Main tables
-------------

The most important tables are listed below and can be used as starting points for exploring the schema:

=================    ================================================================================================================================
Table                 Description
=================    ================================================================================================================================
family                 a list of all Rfam families and family specific information (family accession, family name, description etc.)
rfamseq                a list of all analysed sequences including INSDC accessions, taxonomy id etc.
full_region            a list of all sequences annotated with Rfam families including  INSDC accessions, start and end coordinates, bit scores etc.
clan                   description of all Rfam clans
clan_membership        a list of all Rfam families per clan
taxonomy               NCBI taxonomy identifiers
=================    ================================================================================================================================

.. image:: images/core-schema.png
   :alt: Rfam core tables

Example queries
----------------

Retrieve all rat sequence coordinates annotated with Rfam families
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

While it is possible to get a list of Rfam families found in a species
using the `taxonomy search <http://rfam.org/search?tab=searchTaxBlock#tabview=tab3>`_,
with an SQL query one can access sequence coordinates of each ncRNA:

.. code-block:: sql

  SELECT fr.rfam_acc, fr.rfamseq_acc, fr.seq_start, fr.seq_end
  FROM full_region fr, rfamseq rf, taxonomy tx
  WHERE rf.ncbi_id = tx.ncbi_id
  AND fr.rfamseq_acc = rf.rfamseq_acc
  AND tx.ncbi_id = 10116 -- NCBI taxonomy id of Rattus norvegicus
  AND is_significant = 1 -- exclude low-scoring matches from the same clan

Example output:

.. code-block:: none

  RF01942	AABR05000009.1	211	327
  RF00005	AABR05000052.1	4940	5008


Retrieve all snoRNA families found in Mammals
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: sql

  SELECT fr.rfam_acc, fr.rfamseq_acc, fr.seq_start, fr.seq_end, f.type
  FROM full_region fr, rfamseq rf, taxonomy tx, family f
  WHERE
  rf.ncbi_id = tx.ncbi_id
  AND f.rfam_acc = fr.rfam_acc
  AND fr.rfamseq_acc = rf.rfamseq_acc
  AND tx.tax_string LIKE '%Mammalia%'
  AND f.type LIKE '%snoRNA%'
  AND is_significant = 1 -- exclude low-scoring matches from the same clan

Example output:

.. code-block:: none

  RF00012	AAYZ01671298.1	83	298	Gene; snRNA; snoRNA; CD-box;
  RF00012	AAYZ01122278.1	302	87	Gene; snRNA; snoRNA; CD-box;
