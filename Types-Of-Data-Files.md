# Types of Data Files

## General Cassification

- **PDB:** 
  - standard textual file describling the three-dimensional structure of molecules 
  - They are used to represent atom connectivity
- **DCD:** are typically large binary files that contain the time varying atomic coordinates for the system. Each set of coordinats correspond to one frame in time. 
- **PSF:** contains all of the molecule-specific information needed to applya particular force field to a molecular system. The PSF file conatins six main sections of interest
  - atoms
  - bonds 
  - angles
  - dihedrals
  - impropers (dihedral force term used to maintain planarity)
  - cross-terms

- **Topology Files:** Contains all of the information needed to convert a list of residue names into a complete PSF structure file.

## PDB Files

- Poular PDB archives
  - [wwPDB](https://www.wwpdb.org/)
  - [RCSB](https://www.rcsb.org/)

```s
$ grep ATOM 2hac.pdb | tail
ATOM   1080  O   ASP B  30   15.852   9.308  20.637  1.00  0.00 O
ATOM   1081  CB  ASP B  30   14.621   8.686  18.383  1.00  0.00 C
ATOM   1082  CG  ASP B  30   14.115   8.214  17.018  1.00  0.00 C
ATOM   1083  OD1 ASP B  30   14.307   8.938  16.054  1.00  0.00 O
ATOM   1084  OD2 ASP B  30   13.543   7.138  16.960  1.00  0.00 O
ATOM   1085  OXT ASP B  30   17.814   9.632  19.808  1.00  0.00 O
ATOM   1086  H   ASP B  30   17.125   6.964  18.700  1.00  0.00 H
ATOM   1087  HA  ASP B  30   16.405   9.587  17.597  1.00  0.00 H
ATOM   1088  HB2 ASP B  30   14.359   7.957  19.136  1.00  0.00 H
ATOM   1089  HB3 ASP B  30   14.164   9.634  18.625  1.00  0.00 H
```
- It consist of following sections
  - Title Section : General details about moleculs in the file
  -  

### Title Section

- Header Record (HEADER)
  ```s
  HEADER    MEMBRANE PROTEIN                        12-JUN-06   2HAC
  ```
   the structure in this file is a membrane protein; it was deposited on the 12th of June, 2006; and its PDB ID is 2HAC. the first six characters are the record's name ("HEADER"), characters 11-50 are reserved for the "classification" field (membrane protein), characters 51-59 are used for the deposition date, and characters 63-66 hold the structure's ID.

- Compound Record (COMPND)
  ```s
    COMPND    MOL_ID: 1;
    COMPND   2 MOLECULE: T-CELL SURFACE GLYCOPROTEIN CD3 ZETA CHAIN;
    COMPND   3 CHAIN: A, B;
    COMPND   4 FRAGMENT: TRANSMEMBRANE REGION (28-60);
    COMPND   5 SYNONYM: T-CELL RECEPTOR T3 ZETA CHAIN;
    COMPND   6 ENGINEERED: YES
  ```
     Each line consists of a token/value pair

     Multi-line records use numbers to allow continuation of a single record. So, for example, the line beginning with "COMPND   2" simply continues the compound record, by giving another token/value pair. In this case, the token (or key) is MOLECULE and the value is T-CELL SURFACE GLYCOPROTEIN CD3 ZETA CHAIN.

- Remark Record (REMARK)
  A PDB remark is similar to comments in Python or other languages. 
  In PDB files, a remark starts with REMARK and a number, which simply identifies the remark. So, for example, remark #3 starts like this:
  ```s
  REMARK   3
    REMARK   3 REFINEMENT.
    REMARK   3   PROGRAM     : XPLOR-NIH 2.11
    REMARK   3   AUTHORS     : CHARLES SCHWIETERS
    [etc.]
  ```

### Primary Structure Section

- The primary structure section lists, among other things, the amino-acid sequence of the protein. The main record type for doing this is the **SEQRES**

```s
SEQRES   1 A   33  ASP SER LYS LEU CYS TYR LEU LEU ASP GLY ILE LEU PHE
SEQRES   2 A   33  ILE TYR GLY VAL ILE LEU THR ALA LEU PHE LEU ARG VAL
SEQRES   3 A   33  LYS PHE SER ARG SER ALA ASP
SEQRES   1 B   33  ASP SER LYS LEU CYS TYR LEU LEU ASP GLY ILE LEU PHE
SEQRES   2 B   33  ILE TYR GLY VAL ILE LEU THR ALA LEU PHE LEU ARG VAL
SEQRES   3 B   33  LYS PHE SER ARG SER ALA ASP
```
- 1, 2, ... is serial number

- A, B, ... is Chain ID
- 33, 33, ... is number of residues in a chain from N- to C-terminus (i.e., from amino to carboxy terminus)
  
### Secondary Structure Section

- Gives info about secondary structure of proteins (helices and sheets)
- The secondary structures are determined by various algorithms used by the authors or wwPDB. Note that these algorithms do not always agree, so it is possible for different databases to give conflicting information about secondary structure

### Connectivity Annotation Section

This section gives information about the bonds or linkages present in the protein structure, but which is not given in the primary structure. 
Eg: a disulfide bond, specified with the SSBOND record

```s
SSBOND   1 CYS A    2    CYS B    2                          1555   1555  2.02 
```

- CYS : Short of residue name (Cysteine)
- A, B, ... are chain IDs
- 2, ... redisue number within the chain
- 1555 1555 is summetry operators
- 2.02 length of disulfide bond

### Crystallographic and Coordinates Section

- Information about crystal structure the atom coordinates are based on (CRYST1, ORIGXn, SCALEn)
- The CRYST1 record describes the shape of the unit cell
```s
CRYST1    1.000    1.000    1.000  90.00  90.00  90.00 P 1           1
````
- This is simple quibic unit cell 
- 1.000 1.000 1.000 ... unit cell edge length is 1 Amstong
- 90.00 90.00 90.00 ... means all angles are 90 degree
- P 1 ... is space group


- When a protein structure is first submitted to the PDB archive, it is given in coordinates that may be different from those used in the PDB file. The original coordinates are called the submitted coordinates, and the coordinates used in the PDB file are called orthogonal coordinates. The ORIGXn records give the transformation matrix necessary to obtain the submitted coordinates from the orthogonal coordinates.

```s
ORIGX1      1.000000  0.000000  0.000000        0.00000
ORIGX2      0.000000  1.000000  0.000000        0.00000
ORIGX3      0.000000  0.000000  1.000000        0.00000
```

- "n" in ORIGXn is nth row of transformation matrix
- SCALEn gives transformation matrix needed to go from orthogonal coordinates to fractional coordinates, which are a fraction of the unit cell's length

```s
SCALE1      1.000000  0.000000  0.000000        0.00000
SCALE2      0.000000  1.000000  0.000000        0.00000
SCALE3      0.000000  0.000000  1.000000        0.00000
```
### Coordinate Section

- This is largest section
- Contains coordinates of every atom
- Model Record
  - Different protein conformations are represented with different models
```s
MODEL        [model_num]
ATOM      1  [...]
ATOM      2  [...]
ATOM      3  [...]
[...]
ENDMDL
```
  -   Atom Record: A single line that occurs multiple times for each atom record
  ```s
  ATOM      2  CA  ASP A  -3     -24.877   1.931  -4.644  1.00  0.00           C
  ```
  -  CA ... is atom name
  -  ASP ... is residue name
  -  A is chain ID
  -  -3 is residue ID
  -  -24.877 1.931 -4.644 ...orthogonal coordinates of the atom
  -  1.00 ... occupancy (propotion of time atom spends occupying a praticualr position in case of flexible molecules). If all atoms take only one conformation, so their occupacy is 100%(1.00)
  -  0.00 ... temperature factor(B-factor)
    -  C ... elemental symbol on periodic table
  - A **TER record** is given after all atoms of a chain are listed 
```s
ATOM    543  HB2 ASP A  30      13.952  -5.133 -14.399  1.00  0.00           H
ATOM    544  HB3 ASP A  30      13.434  -6.033 -12.973  1.00  0.00           H
TER     545      ASP A  30
ATOM    546  N   ASP B  -3     -24.040  -6.834   4.973  1.00  0.00           N
ATOM    547  CA  ASP B  -3     -24.918  -6.297   3.894  1.00  0.00           C
``` 
  - The TER record indicates the name of the last residue and chain for the atom that was just given

### Bookkeeping Section

- There are two records at the end of a PDB file: **MASTER** and **END**

```s
MASTER      124    0    0    2    0    0    0    616320   30    2    6
```
- In the above example, we can see that there are 124 REMARK records and 16320 atomic coordinate records. At first, it appears to say there are 616320 atoms, but this is because there are only 5 characters reserved for the field indicating the number of atoms. The initial "6" refers to the number of coordinate transformation records
- The final record is simply **END**

### Hetrogen Section

Heterogens are "non-standard" residues
```s
HET    BGC  A 901      12
```
- This record tells us that heterogen #901 is named BGC, is part of chain A, and has 12 corresponding HETATM records. 
- A few lines later, a HETNAM record tells us the full name of BGC
```s
HETNAM     BGC BETA-D-GLUCOSE
```
- HETATM records are analogous to ATOM records, except that they give information about chemicals that aren't standard amino acids or nucleotides. For example, both water and modified amino acids are considered heterogens.
- Below is the first heterogen atom entry:
```s
HETATM10783  C2  BGC A 901      11.313  82.102 123.399  1.00195.30           C 
```

## PSF Files

PSF files are required becuse in PDB 

- **CHARMM PSF:**  