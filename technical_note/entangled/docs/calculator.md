# Routines to Run Remotely
The first function we need to run on the remote machine is fairly straightforward: build a calculator, pass it the arguments, return the energy.

```{.python file=min_ferm/functions.py}
def calculate(sys, inp, name):
  from BigDFT.Calculators import SystemCalculator
  calc = SystemCalculator()
  log = calc.run(sys=sys, input=inp, name=name)
  return log.energy
```

The second remote function will write the cubefiles. Since this is done on only a single system, it would be a waste to make a dataset. In a notebook case, we would use sanzu directives to automatically transform a cell into a remote function. This is not available outside of a notebook, but we can use another low overhead approach based on the `SanzuFunction` decorator. This will generate a function that is automatically run synchronously on the remote machine.

```{.python file=min_ferm/functions.py #+final}
from remotemanager import SanzuFunction
from remotemanager.serialisation import serialjsonpickle
@SanzuFunction(serialiser=serialjsonpickle())
def write_cubefile(sys, inp, name):
  from BigDFT.Calculators import SystemCalculator
```

For this function, we need to remember to modify the input file so that it reads the orbitals we wrote in the dataset.

```{.python file=min_ferm/functions.py #+final}
  inp.read_orbitals_from_disk()
```

As well as to write the cubefiles as desired.

```{.python file=min_ferm/functions.py #+final}
  inp.write_cubefiles_around_fermi_level()
```

Before proceeding as before.

```{.python file=min_ferm/functions.py #+final}
  calc = SystemCalculator()
  _ = calc.run(sys=sys, input=inp, name=name)
```
