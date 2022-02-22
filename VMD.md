# VMD Basics

## Genral

- VMD uses bond information for visualization, (angles, dihedrals, improper are not used)
- Therefore, bond information is treated and stored differently

### How to read syntax in documentation

- **[item]** Indicates items that are optional. 

- **{value | value}** Indicates a choice of items or values. You can usually only choose one of the values in the braces.

- **item [, item ]...** Indicates that the item preceding the ellipsis (three dots) can be repeated.

- **\<item\>** Angle brackets and italics indicate identifiers, parameters, or arguments that are provided by users.

## Supported Formats

- VMD supports lots of formats which may contain redundant information and can be loaded in different combinations
- PDB
- CHARMM, NAMD, X-PLOR style PSF
- CHARMM, NAMD, X-PLOR style DCD
- NAMD Binary restart file (Coordinates)
- AMBER Structure (PARM)
- AMBER trajectory (CRD)
- Gromacs (GRO, G96, XTC, TRR)

## Loading PDB and writing PSF

- The protein data bank is available at [RCSB PDB](https://rcsb.org)
- It contains proteins crystallized with x ray diffraction and nmr spectroscopy
- **PDB ID :** 4 letter word that identifies each structure in the database
- We can load a Molecule file from the VMD "Molecule file browser" by selecting the appropriate file name.
- We can load a new molecule from:
  > File > New Molecule 

- When a PDB is loaded it contains:
  - atom names
  - coordinates
- When a PDB is loaded it does not contain
  - atom types
  - masses
  - charges
- The missing information of atom types and masses are guessed.
- the missing information of charge is filled with 0.0
- Now we can generate a PSF(Protein Structrue File) file using VMD automatic psf loader.
  > Menu > Extension > Modeling > Auto PSF
- Here we need to load the PDB file and topology files. 

## Loading PSF File

- When loading a PSF file it does not contain:
  - atom names
  - coordinates
- Hence it cannot be loaded independently. It should be loaded with a PDB or DCD
- If PDB and PSF is given together there is no missing data and hence no assumptions.

## TopoTools

### General Info

- Can be used for building topology data files for different kinds of MD simulation packages. Eg: LAMMPS Data File

### Syntax
- Syntax to access the API:

```s
topo <command> [args...] <flags>
```
- Commom **flags** (flags which can be used with all commands)

```s

-molid <num>|top          molecule id (default: 'top')
-sel <selection>          atom selection function or text (default: 'all')

```
- Flags for bond operation commands
```s

-bondtype  <typename>   bond type name (default: unknown)
-bondorder <bondorder>  bond order parameter (default: 1)

```
- Commands: [Command Documentation](https://www.ks.uiuc.edu/Research/vmd/plugins/topotools/)
