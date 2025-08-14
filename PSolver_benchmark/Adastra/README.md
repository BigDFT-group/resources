# Benchmark of Psolver on Adastra

This directory contains benchmark of Psolver on the following molecules:

| Molecule      |     ndim        |        norb    |
| :------------ | :-------------: | -------------: |
| Aflatoxin     |  [209,191,145]  |        58      |
| H2O-32        |      96         |      128       |
| Graphene-H    |  [216,91,216]   |    [361,360]   |
| UO2_2         |      108        |    [716,716]   |

Output files are of the type: `System_BC_...-S_...-A_...-MPI_...-OMP_...-GPU_...`, where values correspond to:

| Variable      |          Meaning         |
| :------------:|     :-------------:      |
|    **BC**     |    Boundary conditions   |
|    **S**      | Symmetry (True or False) |
|    **A**      |     Type of Acceleration |
|    **MPI**    |    Number of MPI ranks   |
|    **OMP**    | Number of OpenMP Threads |
|    **GPU**    |       Number of GPU      |

These benchmarks have been run on the following platforms:

| Type of partition      |     Cores per nodes        |        Gpu per node   | Memory bandwith per node (TB/s) | 
| :----------------:     |        :-------------:     |     :-------------:   |        :-----------:            |
| CPU (AMD GENOA)        |            192             |            0          |             0.92                |
| GPU (AMD MI250X)       |            64              |            8          |            13.11                |
| APU (AMD MI300A)       |            96              |            4          |            21.23                |
