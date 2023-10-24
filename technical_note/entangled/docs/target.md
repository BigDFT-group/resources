# Calculation Target and Parameters
For the next step, we will define what systems to calculate and how. In this case, we can just use a random set of molecules from the BigDFT built in database. Though really for this study you might expect conformers.

```{.python file=min_ferm/__main__.py #build-dataset}
from BigDFT.Database.Molecules import get_molecule
systems = {}
for g in geoms:
  systems[g] = get_molecule(g)
```

An input file will define how the calculation will be performed by BigDFT. In this case, we will configure it to use NLCC pseudopotentials, the PBE approximation, and a grid spacing of 0.45 Bohr. I also want to write the orbitals to disk so that we can restart the calculation later.

```{.python file=min_ferm/__main__.py #input-file}
from BigDFT.Inputfiles import Inputfile
inp = Inputfile()
inp.set_xc("PBE")
inp.set_hgrid(0.45)
inp.set_psp_nlcc()
inp.write_orbitals_on_disk()
```
