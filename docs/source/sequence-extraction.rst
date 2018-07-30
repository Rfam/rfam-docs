.. highlight:: console

Extract ncRNA sequences using Infernal's esl-sfetch
===================================================

All Rfam ncRNA sequences become available on the `ftp <ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/fasta_files>`_ with every new release. 

The following is a tutorial on how to extract sequences using the public instance of the `MySQL database <http://rfam.readthedocs.io/en/latest/database.html>`_.

1. Download and install the `Infernal software <http://eddylab.org/infernal/>`_. You can find additional information in the `Infernal User's Guide <http://eddylab.org/infernal/Userguide.pdf>`_. 

.. seealso:: `Genome annotation <http://rfam.readthedocs.io/en/latest/genome-annotation.html#example-of-using-infernal-and-rfam-to-annotate-rnas-in-an-archaeal-genome>`_ section

2. Add Infernal tools to your $PATH using the following command:
:: 
	> export PATH="/path/to/infernal-1.1.x/bin:$PATH"

3. Create a new directory and download all fasta files from the FTP using ``wget`` :
::  
	> mkdir rfam_sequences && cd rfam_sequences
	> wget ftp://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/fasta_files/* .

4. Decompress all sequence files and merge them in a single fasta file:
:: 
	> gunzip *.gz
	> cat *.fa > Rfam.fa

5. Index the unified sequence file using ``esl-sfetch`` :

:: 

	> esl-sfetch --index Rfam.fa

.. note:: If the above command is successfull, you should see a ``.ssi`` file generated in your current directory

6. Fetch the list of accessions you want to extract and save them in a ``.txt`` file using the MySQL database :

.. 

Here's an example query on how to retrieve all Human ncRNAs:

::

	select concat(fr.rfamseq_acc,'/',fr.seq_start,'-',fr.seq_end) 
	from full_region fr, genseq gs
	where gs.rfamseq_acc=fr.rfamseq_acc
	and fr.is_significant=1
	and fr.type='full'
	and gs.upid='UP000005640' --human upid
	and gs.version=14.0

.. 

Here's an example query on how to retrieve all Human snoRNAs:

::	

	select concat(fr.rfamseq_acc,'/',fr.seq_start,'-',fr.seq_end) 
   	from full_region fr, genseq gs, family f
	where gs.rfamseq_acc=fr.rfamseq_acc
	and f.rfam_acc=fr.rfam_acc
	and fr.is_significant=1
	and fr.type='full'
	and gs.upid='UP000008827'
	and f.type like '%snoRNA%'
	and gs.version=14.0


.. note:: In order for ``esl-sfetch`` to work with the Rfam fasta file, the accessions need to be in the format: **rfamseq_acc/seq_start-seq_end**.

7. Extract the ncRNA sequences in the ``.txt`` file you generated in **step 6** using ``esl-fetch``, and the unified Rfam fasta file from **step 4**:

:: 

	> esl-sfetch -f Rfam.fa /path/to/accession_file.txt > Rfam_ncRNAs.fa

