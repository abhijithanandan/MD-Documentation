# Psfgen package for VMD

## Introduction

- For using psfgen we should create separate pdb file for each segments (Chains in case of proteins). For this we can split an existing pdb file using vmd:

```s
# Split a file containing protein and water into separate segments.
# Creates files named myfile_water.pdb, myfile_frag0.pdb, myfile_frag1.pdb,...
mol load pdb myfile.pdb
set water [atomselect top water]
$water writepdb myfile_water.pdb
set protein [atomselect top protein]
set chains [lsort -unique [$protein get pfrag]]
foreach chain $chains {
set sel [atomselect top "pfrag $chain"]
$sel writepdb myfile_frag${chain}.pdb
}
```

    - We can also use VMD atom selection to delete unwanted atoms

```s
# Load a pdb and psf file into both psfgen and VMD.
resetpsf
readpsf myfile.psf
coordpdb myfile.pdb
mol load psf myfile.psf pdb myfile.pdb
# Select waters that are more than 10 Angstroms from the protein.
set badwater1 [atomselect top "name OH2 and not within 10 of protein"]
# Alternatively, select waters that are outside our periodic cell.
set badwater2 [atomselect top "name OH2 and (x<-30 or x>30 or y<-30 or>30
or z<-30 or z>30)"]
# Delete the residues corresponding to the atoms we selected.
foreach segid [$badwater1 get segid] resid [$badwater1 get resid] {
delatom $segid $resid
}
# Have psfgen write out the new psf and pdb file (VMD’s structure and
# coordinates are unmodified!).
writepsf myfile_chopwater.psf
writepdb myfile_chopwater.pdb
```
## Syntax

- To work with psfgen we need separate pdb files for each segment

```s
# (1) Split input PDB file into segments}
grep -v ’^HETATM’ 6PTI.pdb > output/6PTI_protein.pdb
grep ’HOH’ 6PTI.pdb > output/6PTI_water.pd
```
- topology information can be read with the topology command 

```s
topology toppar/top_all22_prot.inp
```

- Then we can build segment by segment reading PDB files for each segment 

```s
segment BPTI {
pdb output/6PTI_protein.pdb
}
```

- while the above code runs, psfgen will auto generate angles and dihedrals. If we need to avoid that we need to specify "auto none". This is usually done in case of water. 

```s
segment SOLV {
auto none
pdb output/6PTI_water.pdb
}
```

- In case of proteins there is a patch command which can do something called patching for different part of the protein chain. Only applicable in case of proteins. 

```s
patch DISU BPTI:5 BPTI:55
patch DISU BPTI:14 BPTI:38
patch DISU BPTI:30 BPTI:51
```
- 

