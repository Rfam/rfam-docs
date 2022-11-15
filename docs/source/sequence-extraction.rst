.. highlight:: console

Extract ncRNA sequences
===================================================

All Rfam ncRNA sequences become available on the `ftp <https://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/fasta_files>`_ with every new release.
The following is a tutorial on how to extract sequences using the public instance of the `MySQL database <http://rfam.readthedocs.io/en/latest/database.html>`_ and ``esl-sfetch`` tool.

**Requirements:**


1. MySQL Community Server, freely available `here <https://dev.mysql.com/downloads/>`_

2. ``esl-sfetch`` from Infernal's easel `miniapps`


=====================================================



1. Download and install the `Infernal software <http://eddylab.org/infernal/>`_. You can find additional information in the `Infernal User's Guide <http://eddylab.org/infernal/Userguide.pdf>`_.

.. seealso:: `Genome annotation <http://rfam.readthedocs.io/en/latest/genome-annotation.html#example-of-using-infernal-and-rfam-to-annotate-rnas-in-an-archaeal-genome>`_ section

2. Add Infernal tools to your **$PATH** using the following command:
::
	> export PATH="/path/to/infernal-1.1.x/bin:$PATH"

3. Download Rfam.fa.gz (combined file of all the fasta files) from the FTP using ``wget`` and then unzip :
::
	> wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/fasta_files/Rfam.fa.gz
	> gunzip Rfam.fa.gz

4. Index the unified sequence file using ``esl-sfetch`` :
::
	> esl-sfetch --index Rfam.fa

.. note:: If the above command is successful, you should see a ``.ssi`` file generated in your current directory

5. Create a ``.sql`` file with a SQL command that fetches the regions of interest.


Example query to retrieve all human ncRNAs:

::

	select concat(fr.rfamseq_acc,'/',fr.seq_start,'-',fr.seq_end)
	from full_region fr, genseq gs
	where gs.rfamseq_acc=fr.rfamseq_acc
	and fr.is_significant=1
	and fr.type='full'
	and gs.upid='UP000005640' -- human upid
	and gs.version=14.0;

..

Example query to retrieve all human snoRNAs:

::

	select concat(fr.rfamseq_acc,'/',fr.seq_start,'-',fr.seq_end)
   	from full_region fr, genseq gs, family f
	where gs.rfamseq_acc=fr.rfamseq_acc
	and f.rfam_acc=fr.rfam_acc
	and fr.is_significant=1
	and fr.type='full'
	and gs.upid='UP000005640' -- human upid
	and f.type like '%snoRNA%'
	and gs.version=14.0;

Example query to retrieve all Mammalian 5S ribosomal RNAs (RF00001):

::

	select concat(fr.rfamseq_acc,'/',seq_start,'-',seq_end)
	from full_region fr, rfamseq rs, taxonomy tx
	where fr.rfamseq_acc=rs.rfamseq_acc
	and tx.ncbi_id=rs.ncbi_id
	and fr.rfam_acc='RF00001'
	and tx.tax_string like '%Mammalia%'
	and is_significant=1;

.. note:: In order for ``esl-sfetch`` to work with the Rfam fasta file, the regions need to be in the format: **rfamseq_acc/seq_start-seq_end**.

6.  Fetch a list of accessions to extract from the database and save them in a ``.txt`` file using the MySQL database :

::

	> mysql -u rfamro -h mysql-rfam-public.ebi.ac.uk -P 4497 --skip-column-names --database Rfam < query.sql > accessions.txt

..

7. Extract the ncRNA sequences in the ``.txt`` file generated in **step 6** from the unified Rfam fasta file from **step 3** using ``esl-fetch``:

::

	> esl-sfetch -f Rfam.fa /path/to/accessions.txt > Rfam_ncRNAs.fa
