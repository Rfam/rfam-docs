Rfam API
========

.. contents::
  :local:

Most data in Rfam can be accessed programmatically using a RESTful API
allowing for integration with other resources.

.. HINT::
  You can also access the data using a :ref:`database:Public MySQL Database`
  that contains the latest Rfam release.

Data access
-----------

The data can be accessed in several formats which can be specified in the URL:

* HTML
  https://rfam.org/family/RF00360

* JSON
  https://rfam.org/family/RF00360?content-type=application/json

* XML
  https://rfam.org/family/RF00360?content-type=text/xml

Using *curl*
^^^^^^^^^^^^

Here is how to retrieve an XML description of an Rfam family
using `curl <https://curl.haxx.se>`_:

.. code-block:: bash

  curl https://rfam.org/family/snoZ107_R87?content-type=text%2Fxml

Output:

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <!-- information on Rfam family RF00360 (snoZ107_R87), generated: 12:57:01 31-Oct-2016 -->
  <rfam xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://rfam.sanger.ac.uk/"
        xsi:schemaLocation="http://rfam.sanger.ac.uk/
                            http://rfam.sanger.ac.uk/static/documents/schemas/entry_xml.xsd"
        release="12.1"
        release_date="2016-04-26">
    <entry entry_type="Rfam" accession="RF00360" id="snoZ107_R87">
      <description>
  <![CDATA[
  Small nucleolar RNA Z107/R87
  ]]>
      </description>
      <comment>
  <![CDATA[
  Z107 and R87 are members of the C/D class of snoRNA which contain the C (UGAUGA) and D (CUGA) box motifs. Most of the members of the box C/D family function in directing site-specific 2'-O-methylation of substrate RNA
  ]]>
      </comment>
      <curation_details>
        <author>Moxon SJ</author>
        <seed_source>Moxon SJ</seed_source>
        <num_seqs>
          <seed>9</seed>
          <full>144</full>
        </num_seqs>
        <num_species>37</num_species>
        <type>Gene; snRNA; snoRNA; CD-box;</type>
        <structure_source>Predicted; RNAfold; Moxon SJ, Daub J, Gardner PP</structure_source>
      </curation_details>
      <cm_details num_states="">
        <build_command>cmbuild -F CM SEED</build_command>
        <calibrate_command>cmcalibrate --mpi CM</calibrate_command>
        <search_command>cmsearch --cpu 4 --verbose --nohmmonly -T 19 -Z 549862.597050 CM SEQDB</search_command>
        <cutoffs>
          <gathering>50.0</gathering>
          <trusted>50.2</trusted>
          <noise>49.8</noise>
        </cutoffs>
      </cm_details>
    </entry>
  </rfam>

Using a script
^^^^^^^^^^^^^^

Rfam API can also be used from a script written in any programming language,
for example Python or Perl.

**Python example script**

.. code-block:: python

  import json
  import requests

  r = requests.get('https://rfam.org/family/RF00360?content-type=application/json')
  print r.json()['rfam']['acc']

**Perl example script**

.. code-block:: perl

  #!/usr/bin/perl

  use strict;
  use warnings;

  use LWP::UserAgent;

  my $ua = LWP::UserAgent->new;
  $ua->env_proxy;

  my $res = $ua->get(' https://rfam.org/family/snoZ107_R87?content-type=text%2Fxml' );

  if ( $res->is_success ) {
    print $res->content;
  }
  else {
    print STDERR $res->status_line, "\n";
  }

------------------------------------------

Endpoints
---------

Family
^^^^^^

Family description
++++++++++++++++++

Returns general information about an Rfam family, such as curation details, search parameters, etc.

**Examples:**

* https://rfam.org/family/RF00360?content-type=text/xml
* https://rfam.org/family/snoZ107_R87?content-type=application/json

Accession to ID
+++++++++++++++

Returns the ID for the family with the given Rfam accession or ID.

**Example:**

https://rfam.org/family/snoZ107_R87/acc

**Example output:**

.. code-block:: bash

  RF00360

ID to accession
+++++++++++++++

**Example output:**

https://rfam.org/family/RF00360/id

**Output:**

.. code-block:: bash

  snoZ107_R87

Secondary structure images
++++++++++++++++++++++++++

Returns the schematic secondary structure image for the family.
The following types of secondary structure diagrams are supported:

* *cons* (sequence conservation)
* *fcbp* (basepair conservation)
* *cov* (covariation)
* *ent* (relative entropy)
* *maxcm* (maximum CM parse)
* *norm* (normal)
* *rscape* (`R-scape`_ analysis of Rfam SEED alignment)
* *rscape-cyk* (secondary structure predicted by `R-scape`_ based on Rfam SEED alignment)

**Examples:**

* https://rfam.org/family/snoZ107_R87/image/norm
* https://rfam.org/family/RF00360/image/cov
* https://rfam.org/family/RF00360/image/rscape
* https://rfam.org/family/RF00360/image/rscape-cyk

Covariance models
+++++++++++++++++

Returns the covariance model for the specified family.

**Example:** https://rfam.org/family/RF00360/cm

Sequence regions
++++++++++++++++

Returns the list of all sequence regions for the specified families in tab-delimited format.

.. NOTE::

  Some families have too many regions to list. The server will return a status of ``403 Forbidden`` in these cases.

**Examples:**

* https://rfam.org/family/snoZ107_R87/regions (plain text)
* https://rfam.org/family/RF00360/regions?content-type=text%2Fxml

---------------------------

Phylogenetic trees
^^^^^^^^^^^^^^^^^^

Tree data
+++++++++

Returns the raw data for the phylogenetic tree in NHX format based on seed alignment.

Example: https://rfam.org/family/RF00360/tree/

Tree image
++++++++++

Returns a PNG image showing the phylogenetic tree for the specified family based on seed alignment.
The image can be labelled either using **species names** or **sequence accessions**.

**Examples:**

* https://rfam.org/family/RF00360/tree/label/species/image
* https://rfam.org/family/RF00360/tree/label/acc/image

Tree image map
++++++++++++++

Returns the `HTML image map <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/map>`_
that is used in conjunction with the tree image to highlight tree nodes
in the Rfam website.

**Example:**

* https://rfam.org/family/RF00360/tree/label/acc/map
* https://rfam.org/family/RF00360/tree/label/species/map

.. NOTE::

  The HTML snippet contains an ``<img>`` tag that automatically loads the tree image.

---------------------------

Structure mapping
^^^^^^^^^^^^^^^^^

Returns the mapping between an Rfam family, EMBL sequence regions and PDB residues.
The plain text file has a tab-delimited format.

**Examples:**

* https://rfam.org/family/RF00002/structures (HTML)
* https://rfam.org/family/RF00002/structures?content-type=application/json
* https://rfam.org/family/RF00002/structures?content-type=text/xml

---------------------------

Alignments
^^^^^^^^^^

The following methods can be used to return family alignments in various formats.

.. HINT::

  You can request a compressed version of the alignment by adding ``gzip=1`` to the URL.

Stockholm-format alignment
++++++++++++++++++++++++++

Returns the Stockholm-format seed alignment for the specified family.

**Examples:**

* https://rfam.org/family/RF00360/alignment
* https://rfam.org/family/RF00360/alignment?gzip=1

Formatted alignment
+++++++++++++++++++

Returns the seed alignment for the specified family in one of the following formats:

* *stockholm* (standard Stockholm format - default)
* *pfam* (Stockholm with sequences on a single line conservation)
* *fasta* (gapped FASTA format)
* *fastau* (ungapped FASTA format)

**Examples:**

* https://rfam.org/family/RF00360/alignment/stockholm
* https://rfam.org/family/RF00360/alignment/pfam
* https://rfam.org/family/RF00360/alignment/fasta
* https://rfam.org/family/snoZ107_R87/alignment/fastau

---------------------------

Sequence searches
-----------------

In addition to a `sequence search <https://rfam.org/search>`_ user interface,
it is possible to run single-sequence Rfam searches programmatically.

Running a search is a two step process:

1. submit the search sequence
2. retrieve search results

The reason for separating the operation into two steps rather than
performing a search in a single operation is that the time taken to
perform a sequence search will vary according to the length of the
sequence searched. Most web clients, browsers or scripts, will simply
time-out if a response is not received within a short time period,
usually less than a minute. By submitting a search, waiting and then
retrieving results as a separate operation, we avoid the risk of a
client reaching a time-out before the results are returned.

The following example uses simple command-line tools to submit the search
and retrieve results, but the whole process is easily transferred to a
single script or program.

Save your sequence to file
^^^^^^^^^^^^^^^^^^^^^^^^^^

It is usually most convenient to save your sequence into a plain text
file, something like this:

.. code-block:: bash

  $ cat test.seq
  AGTTACGGCCATACCTCAGAGAATATACCGTATCCCGTTCGATCTGCGAA
  GTTAAGCTCTGAAGGGCGTCGTCAGTACTATAGTGGGTGACCATATGGGA
  ATACGACGTGCTGTAGCTT

The sequence should contain only valid sequence characters. You can break
the sequence across multiple lines to make it easier to handle.

Submit the search
^^^^^^^^^^^^^^^^^

When you send a request to the server, you can specify the format of the
response. The server supports `JSON <http://en.wikipedia.org/wiki/JSON>`_
(application/json) and `XML <http://en.wikipedia.org/wiki/XML>`_ (text/xml) output.
In the examples below we'll
use the JSON output format by adding an ``Accept`` header to the
request, specifying the media type ``application/json``.
You could use the "content-type" parameter on the URL, rather
than setting a header.

.. code-block:: bash

  curl -X 'POST' 'https://batch.rfam.org/submit-job' -H 'accept: application/json' -F 'sequence_file=@test.seq'

**Example output:**

.. code-block:: json

  {
    "resultURL": "https://batch.rfam.org/result/infernal_cmscan-R20240522-154022-0777-68207895-p1m",
    "jobId": "infernal_cmscan-R20240522-154022-0777-68207895-p1m"
  }

Wait for the search to complete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Having submitted the search, you now need to check the ``resultURL``
given in the response.

Although you can check for results immediately, if you poll before your
job has completed you won't receive a full response. Instead, the HTTP
response will have its status set appropriately and the body of the
response will contain only string giving the status. You should ideally
check the HTTP status of the response, rather than relying on the body of
the response. See below for a table showing the response status codes
that the server may return.

When writing a script to submit searches and retrieve results, **please add
a short delay** between the submission and the first attempt to retrieve
results. Most search jobs are returned within four to five seconds of
submission, depending greatly on the length of the sequence to be
searched.

Retrieve results
^^^^^^^^^^^^^^^^

The response that was returned from the first query includes a URL from
which you can now retrieve results:

.. code-block:: bash

  curl -X 'GET' 'https://batch.rfam.org/result/infernal_cmscan-R20240522-154022-0777-68207895-p1m' -H 'accept: application/json'

.. code-block:: json

  {
  "searchSequence": "AGTTACGGCCATACCTCAGAGAATATACCGTATCCCGTTCGATCTGCGAAGTTAAGCTCTGAAGGGCGTCGTCAGTACTATAGTGGGTGACCATATGGGAATACGACGTGCTGTAGCTT",
  "numHits": 1,
  "jobId": "infernal_cmscan-R20240522-154022-0777-68207895-p1m",
  "opened": "2024-05-22 15:40:27",
  "started": "2024-05-22 15:40:27",
  "closed": "2024-05-22 15:42:16",
  "hits": {
    "5S_rRNA": [
      {
        "id": "5S_rRNA",
        "acc": "RF00001",
        "start": 1,
        "end": 119,
        "strand": "+",
        "GC": 0.49,
        "score": 104.9,
        "E": 4.5e-24,
        "alignment": {
          "nc": "#NC                                                                                                                                       ",
          "ss": "#SS               (((((((((,,,,\u003C\u003C-\u003C\u003C\u003C\u003C\u003C---\u003C\u003C--\u003C\u003C\u003C\u003C\u003C\u003C______\u003E\u003E--\u003E\u003E\u003E\u003E--\u003E\u003E----\u003E\u003E\u003E\u003E\u003E--\u003E\u003E\u003C\u003C\u003C-\u003C\u003C----\u003C-\u003C\u003C-----\u003C\u003C____\u003E\u003E-----\u003E\u003E-\u003E--\u003E\u003E-\u003E\u003E\u003E))))))))): ",
          "hit_seq": "#CM 1 gccuGcggcCAUAccagcgcgaAagcACcgGauCCCAUCcGaACuCcgAAguUAAGcgcgcUugggCcagggUAGUAcuagGaUGgGuGAcCuCcUGggAAgaccagGugccgCaggcc 119",
          "match": "#MATCH               :: U:C:GCCAUACC ::G:GAA ::ACCG AUCCC+U+CGA CU CGAA::UAAGC:C:: +GGGC: :G  AGUACUA  +UGGGUGACC+  UGGGAA+AC:A:GUGC:G:A ::+",
          "user_seq": "#SEQ 1 AGUUACGGCCAUACCUCAGAGAAUAUACCGUAUCCCGUUCGAUCUGCGAAGUUAAGCUCUGAAGGGCGUCGUCAGUACUAUAGUGGGUGACCAUAUGGGAAUACGACGUGCUGUAGCUU 119",
          "pp": "#PP               *********************************************************************************************************************** "
        }
      }
    ]
  }
}

.. WARNING::

  Old search results are regularly cleared out but results will be visible
  for **one week** after completion of the original search.

Server responses
^^^^^^^^^^^^^^^^

Server responses include a standard HTTP status code giving information
about the current state of your job. These are the possible status
codes:

+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| HTTP method  |  HTTP status code | Status description    | Response body  | Notes                                                                                                                                                                                                                                       |
+==============+===================+=======================+================+=============================================================================================================================================================================================================================================+
| POST         | 202               | Accepted              | PEND / RUN     | The job has been accepted by the search system and is either pending (waiting to be started) or running. After a short delay, your script should check for results again.                                                                   |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| POST         | 502               | Bad gateway           | Error message  | There was a problem scheduling or running the job. The job has failed and will not produce results. There is no need to check the status again.                                                                                             |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| POST         | 503               | Service unavailable   | Error message  | Occasionally the search server may become overloaded. If the error message suggests that the search queue is full, try submitting your search later.                                                                                        |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GET          | 200               | OK                    | Search results | The job completed successfully and the results are included in the response body.                                                                                                                                                           |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GET          | 410               | Gone                  | DEL            | Your job was deleted from the search system. This status will not be assigned by the search system, but by an administrator. There was probably a problem with the job and you should contact the help desk for assistance with it.         |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GET          | 503               | Service unavailable   | HOLD           | Your job was accepted but is on hold. This status will not be assigned by the search system, but by an administrator. There is probably a problem with the job and you should contact the help desk for assistance with it.                 |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GET, POST    | 500               | Internal server error | Error message  | There was some problem accepting or running your job, but it does not fall into any of the other categories. The body of the response will contain an error message from the server. Contact the help desk for assistance with the problem. |
+--------------+-------------------+-----------------------+----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Links
-----

.. target-notes::

.. _`R-scape`: http://eddylab.org/R-scape/
