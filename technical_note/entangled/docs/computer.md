# Build the Computer Definition

Before we can run on our remote machine, we need to build a computer. This can be split into defining first the overall computer and its options and then a parser to create the necessary jobscript.

```{.python file=min_ferm/computer.py}
from remotemanager.connection.computers.base import BaseComputer, optional
<<parser-imp>>
<<comp-imp>>
```

The computer class is built by deriving from BaseComputer. The only options we need for this study are the amount of MPI and OpenMP.

```{.python file=min_ferm/computer.py #comp-imp}
class Comp(BaseComputer):
   def __init__(self, host='alphachem.r-ccs27.riken.jp', **kwargs):
     super().__init__(**kwargs, host=host)
     self.mpi = optional("mpi", 1)
     self.omp = optional("omp", 1)
     self._parser = my_parser
```

Custom computers need a parser routine to create the needed jobscript. Specifically, on this machine we need to export some environment variables for running BigDFT. I also need to activate a conda environment because I used conda to install bigdft-suite and pip for PyBigDFT on the remote machine.

```{.python file=min_ferm/computer.py #parser-imp}
def my_parser(resources):
  output = []
  output.append('eval "$(~/miniconda3/bin/conda shell.bash hook)"')
  output.append(f'conda activate bigdft')
  output.append(f'export OMP_NUM_THREADS={resources["omp"].value}')
  output.append(f'export BIGDFT_MPIRUN="mpirun -np {resources["mpi"].value}"')

  return output 
```
