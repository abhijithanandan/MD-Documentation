# General Information


## LAMMPS Data File

The data file is read into LAMMPS:

```s
readdata <filepath>
```

- The data file consists of:
  - size of system
  - initial atomic coordinates
  - molecular topology
  - (optional) force field coefficients
- Balnk lines are important
- There is blank line after header section
- Indentation and space between words are not important
- keywords like Masses, Bond Coeffs must be left-justified and capitalized
- Header section upto box bounds should appear first. Remainig content (Masses, Atoms, Bonds) can have any order
- Mandartory entries:
  - Header Section
  - Masses
  - Atoms
- Optional entries
  - Bonds
  - Angles
  - Dihedrals
  - Impropers
- Force fields can be in data file or read seperately
- Class 2 force-field coefficients can only be included through data files

-  Here is a sample file with annotations in parenthesis and lengthy sections replaced by dots (...). Note that the blank lines are important in this example.

```s
LAMMPS Description           (1st line of file)

100 atoms         (this must be the 3rd line, 1st 2 lines are ignored)
95 bonds                (# of bonds to be simulated)
50 angles               (include these lines even if number = 0)
30 dihedrals
20 impropers

5 atom types           (# of nonbond atom types)
10 bond types          (# of bond types = sets of bond coefficients)
18 angle types         
20 dihedral types      (do not include a bond,angle,dihedral,improper type
2 improper types             line if number of bonds,angles,etc is 0)

-0.5 0.5 xlo xhi       (for periodic systems this is box size,
-0.5 0.5 ylo yhi        for non-periodic it is min/max extent of atoms)
-0.5 0.5 zlo zhi       (do not include this line for 2-d simulations)

Masses

  1 mass
  ...
  N mass                           (N = # of atom types)

Nonbond Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of atom types)

Bond Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of bond types)

Angle Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of angle types)

Dihedral Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

Improper Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of improper types)

BondBond Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of angle types)

BondAngle Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of angle types)

MiddleBondTorsion Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

EndBondTorsion Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

AngleTorsion Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

AngleAngleTorsion Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

BondBond13 Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of dihedral types)

AngleAngle Coeffs

  1 coeff1 coeff2 ...
  ...
  N coeff1 coeff2 ...              (N = # of improper types)

Atoms

  1 molecule-tag atom-type q x y z nx ny nz  (nx,ny,nz are optional -
  ...                                    see "true flag" input command)
  ...                
  N molecule-tag atom-type q x y z nx ny nz  (N = # of atoms)

Velocities

  1 vx vy vz
  ...
  ...                
  N vx vy vz                        (N = # of atoms)

Bonds

  1 bond-type atom-1 atom-2
  ...
  N bond-type atom-1 atom-2         (N = # of bonds)

Angles

  1 angle-type atom-1 atom-2 atom-3  (atom-2 is the center atom in angle)
  ...
  N angle-type atom-1 atom-2 atom-3  (N = # of angles)

Dihedrals

  1 dihedral-type atom-1 atom-2 atom-3 atom-4  (atoms 2-3 form central bond)
  ...
  N dihedral-type atom-1 atom-2 atom-3 atom-4  (N = # of dihedrals)

Impropers

  1 improper-type atom-1 atom-2 atom-3 atom-4  (atom-2 is central atom)
  ...
  N improper-type atom-1 atom-2 atom-3 atom-4  (N = # of impropers)

```
- **atom-tag** : First number on ATOM record line, used to uniquely identify atom in simulation
- **molecule-tag** : A second identifier attached to atom. Can be customized for use, either make it molecule counter, or simply put 0 etc.
- **q** : charge of atom in electron units (+1 for proton)
- **x y z** : initial position of atom
- **nx ny nz** : (optional) (read only with true flag on read data command) it adds "n x box-lengths" along all directions to the atom coordinates to get atoms position. Mostly used in post processing to unwrap system coordinates. The value of n can be positive, negative or zero
- **vx vy vz** - (optional) If initial velocities are to be given. First number on line is atom-tag
- For periodic b.c simulation, the atom coordinates need not be wrapped. LAMMPS will automatically do the wrapping
- The styles(force field models) for bonds, angles etc. should be specificed before "read data", **otherwise default is used**


## Methods to prepare LAMMPS Data File

- Use topology building tools of 
  - Amber
  - CHARMM
  - NAMD
- Then convert to LAMMPS Data File Format using the tools given in LAMMPS Package


## Preparing LAMMPS Data File With TopoTools

Check for available commands:

```s
topo help
```

- To write a LAMMPS data file :
```s
topo writelammpsdata <filename> <atom-style>
```

- The data file generated by TopoTools will not contain
  - pair/bond/angle/dihedral/improper sytle definitions
- It contains comments that match symbolic type names for the above style definitions

**Note**

---

- To visualize LAMMPS dump file like (.dcd or .xtc) first load the lammps data file and write a PSF file.

```s
topo readlammpsdata <data-file-name>
animate write psf <psf-file-name>
```
 - This will help in replacing the numbers which LAMMPS uses internally with labels which are more easily readable
---

