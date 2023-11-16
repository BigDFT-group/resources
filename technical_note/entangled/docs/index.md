# Introduction
The goal of this project project will be to demonstrate how [remotemanager](https://l_sim.gitlab.io/remotemanager/index.html)
might be used with a different literate programming system than a notebook.
We will use [entangled](https://entangled.github.io/) as our example.

The plan of this study is as follows. First, we will build a small dataset
of molecules to compute. Then we will construct an input file with the
settings we like for the calculation. After that, we will use a Dataset
to run BigDFT on them. We will find the molecule which had the lowest energy,
and run another calculation on it that writes cubefiles of the states around
the fermi-level.


```{.python file=min_ferm/__main__.py}
if __name__ == "__main__":
  geoms = ["H2O", "CO", "CO2"]
  <<build-dataset>>
  <<input-file>>
  <<build-comp>>
  <<remote-calculation>>
  <<find-lowest>>
  <<write-lowest-cubefile>>
```

Being able to write outlines like this is a strong point of using alternatives
to notebooks. On the other hand, the weak point will be the lack of
interactivity. Let's continue to fill in the details. We will of course have
to create a computer first. Here we use a custom computer that we will define
in the next lesson, and set it up to run with 2 MPI and 2 OpenMP threads.

```{.python file=min_ferm/__main__.py #build-comp}
from remotemanager import Dataset
from computer import Comp
Dataset.default_url = Comp(user="wddawson")
Dataset.default_url.mpi = 2
Dataset.default_url.omp = 2
```
