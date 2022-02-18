# VMD Basics

## Genral

- VMD uses bond information for visualization, (angles, dihedrals, impropoers are not used)
- Therefore, bond information is treated and stored differently

### How to read syntax in documentation

- **[item]** Indicates items that are optional. 

- **{value | value}** Indicates a choice of items or values. You can usually only choose one of the values in the braces.

- **item [, item ]...** Indicates that the item preceding the ellipsis (three dots) can be repeated.

- **\<item\>** Angle brackets and italics indicate identifiers, parameters, or arguments that are provided by users.

## Loading PDB and writing PSF

- The protein data bank is available at [RCSB PDB](https://rcsb.org)
- It contains proteins crystallized with x ray diffraction and nmr spectrosocpy
- **PDB ID :** 4 letter word that identifies each structure in the database
- We can load a Molecule file from the VMD "Molecule file browser" by selecting the appropriate file name.
- We can load a new molecule from:
  > File > New Molecule 
- Now we can generate a PSF(Protein Structrue File) file using VMD automatic psf loader.
  > Menu > Extension > Modeling > Auto PSF
- Here we need to load the PDB file and topology files. 

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
