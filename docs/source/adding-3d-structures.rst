Integrating 3D Structures into a SEED Alignment
===============================================

Updating an Rfam family to include 3D structures involves several steps.
This document outlines the process.

Acronyms
--------

- ncRNA: non-coding RNA
- PDB: Protein Data Bank
- SS: Secondary Structure
- SS_cons: Alignment secondary structure consensus
- RF: Alignment reference sequence
- PK: Pseudoknot

Validation
----------

Validate that the 3D information has the correct identifiers. The
alignments map the PDB sequences to RNAcentral IDs. The format for PDB
sequence reference includes RNAcentral ID plus coordinates. The PDB
secondary structure reference includes the sequence reference plus the
PDB reference.

For example:

::

   URS0000A76344_911092/1-94
   #=GR  URS000080DF35_32630/1-94      2GIS_A_SS

Where ``URS0000A76344_911092`` is the RNAcentral reference:
https://rnacentral.org/rna/URS0000A76344/911092 and ``2GIS_A`` is the
PDB reference: https://www.rcsb.org/structure/2GIS

Review and Update of 3D References
----------------------------------

1. Validate that PDB structures have high resolution (<4.0Å). If the
   added structures are of low resolution or are missing a secondary
   structure, remove the sequence.
2. Review and update base pairs in the sequence. Not all base pairs may
   be annotated correctly. For example, modified nucleotides are not
   annotated in the 2D but may be valid base pairs. Base pairs between
   modified nucleotides are included manually in the PDB SS annotations.
3. Review and update regions involved in contact with other chains, RNA,
   or proteins. In structures that report an interaction between two RNA
   chains, the PDB 2D may include incorrect base pairs. The base pairs
   will be between the two chains but incorrectly assigned to one of
   them. These must be removed. In the case of interactions with
   proteins, this interaction can prevent base pairs in the PDB SS;
   these regions have to reflect the 3D information in the PDB SS but
   may not be considered for the SS_cons.
4. Review ‘canonical’ base pairs. The PDB 2D must include all G:C, A:U,
   and G:U base pairs reported in the structure. If a base pair is
   missed, it is added; if a base pair is not supported, it is removed.
5. Review PKs. The PKs are represented in the PDB SS as open ``{{{`` and
   closed ``}}}`` brackets. This has to be confirmed in the structures
   and updated if needed.

Update the SS_cons Line
-----------------------

The SS_cons attempts to represent what is reported in the structures and
is updated after reviewing each of the PDB SS. The updates can reflect:

- PKs annotated in the PDB SS; PK base pairs need to be included
  considering the base pairs supported in the alignment and the RF
  sequence.
- Single base pairs that are not stem loops but are supported by the
  structures. These base pairs are less easily caught by classical
  alignment methods, but recognizing them in the 3D structures and
  adding them to the SS_cons helps direct the alignment and represent a
  more accurate structure.
- Missing base pairs in stem-loops, for example, stem-loops with
  variable length. Whenever the SS_cons can represent as many base pairs
  in a variable length of the stem-loops, this has to be included to
  provide the best-supported structure.
- Base pairs in riboswitches or RNAs with ligands. Some PDBs with
  ligands prevent the formation of base pairs in their bound
  conformation. If there is support for base pairs in an unbound
  structure, these base pairs have to be included in the SS_cons.
- When the RNA is compromised by interacting with proteins or RNAs, the
  SS_cons needs to be corrected to avoid reflecting extra structural
  elements that are not supported by the structures.

Refine the Alignment
--------------------

As a result of the curation process, the length of the sequences may
need to be adjusted, and the alignment may need to be refined to better
align variable regions. Some supported refinements are:

1. When the 3D structure supports a longer or shorter ncRNA, consider
   extending or trimming the alignment to represent a more accurate
   model. Sequence length has to be reflected in all the alignment
   sequences, and the improvement in the family has to be confirmed in
   terms of keeping or improving the annotation of the family in the
   current annotation dataset of Rfam.
2. Confirm that only canonical Watson-Crick base pairs are represented
   in the RF. In some cases, it may be necessary to refine the
   alignment. This is done with Infernal, providing the reviewed SS_cons
   as a reference.
3. If the 3D structures report new elements, for example, PKs or missed
   base pairs that are not represented in the RF, the alignment has to
   be refined to correctly represent these elements. This is done with
   Infernal, providing the reviewed SS_cons as a reference.

Add Structural Annotations
--------------------------

Annotations are not mandatory in the alignment; they are provided as a
guide to users when the structure has been defined in terms of 3D
structural elements (``#=GC RNA_structural_elements``). Ligands and RNA
motifs can also be annotated as ‘x’ in the positions that form the motif
(example: K-turn) or the position that represents contacts of the
sequence with the ligand (``#=GC RNA_motif`` and ``#=GC RNA_ligand``).
Since authors use several nomenclatures for structural elements, the
structural elements have a free format. Some examples are: stem loop,
SL, stem, S, P, etc.

Update the DESC File
--------------------

The literature reference of each structure in the SEED has to be added
to the family DESC, and the PDB references have to be included in the
DESC CC lines.

Basic References
----------------

- Canonical base pairs: A:U, C:G, and G:U are considered canonical base
  pairs.
- Non-canonical base pairs: Non-canonical base pairs are kept in the PDB
  SS as a reference of the PDB reported sequence but are not in the
  SS_cons reference.
- Pseudoknots: PKs in PDB SS are represented by ``{{{`` and ``}}}``, PKs
  in SS_cons are represented by AAA aaa, BBB bbb, CCC ccc, etc.

Whenever annotations of motifs, ligands, or structural elements can be
included, they can be annotated with the following ``#=GC`` lines:

::

   #=GC RNA_motif
   #=GC RNA_ligand
   #=GC RNA_structural_elements
