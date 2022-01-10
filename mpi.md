---
title: "MPI on PLEIADES"
---


### MPI on Pleiades
There are multiple MPI environments available:
1. OpenMPI4: `module load 2021a GCC/10.3.0 OpenMPI/4.1.1`
1. Intel MPI: `module load 2021a iimpi/2021a` or [through Parallel Studio](software/intel)
1. Local package OpenMPI3 in `/lib64/openmpi3` on our worker nodes
1. Compiling your own MPI libraries

If you need more information about the locally installed OpenMPI3 version, you can look around a worker node by
```bash
# Look into local openmpi3 manually on WN
$ salloc -N1 -n1
$ ssh wn21123 # replace with correct wn number of your interaction session
$ yum info openmpi3
$ ls /lib64/openmpi3
$ <ctrl-d to exit salloc>
```

Alternatively, if you are just interested in file locations:
```bash
$ srun -N1 -n1 tree /lib64/openmpi3 | less
```

The PMI library acts as an interface between Slurm and various MPI versions.
This way Slurm can manage MPI processes, if the MPI library has been compiled correctly, e.g. by using `--with-pmi` and `--with-slurm`.
If you intend to compile your own MPI versions, you may have to mention the location of PMI libraries:
```
# find /lib64/ -name libpmi*
/lib64/libpmi.so.0
/lib64/libpmi.so
/lib64/libpmi2.so.0.0.0
/lib64/libpmi.so.0.0.0
/lib64/libpmi2.so
/lib64/libpmi2.so.0
```

**ATTENTION: We consider these libraries as outdated and plan to update them with PMIx during the next scheduled downtime.
Expect changes to these paths in the future.**


###  Using MPI in Slurm Jobs
There are two approaches to use MPI in your Slurm batch scripts:
1. `mpirun`
2. ~~`srun --mpi=pmix_v3`~~

**`srun --mpi=pmix_v3` is currently not available. It will work as soon as PMIx is replacing the current PMI2 libraries on our workernodes.**

Applications that implicitly ship MPI may need additional configuration.
The application may want to do its own resource management and might need informations about PMI libraries or Slurm itself.
This is a case-by-case situation where you should study the corresponding documentation of your application.


#### Example Job Script
```bash
#!/bin/bash

#SBATCH -p short
#SBATCH -t 60
#SBATCH -N 4 # 4 Nodes
#SBATCH -n 4 # 4 processes in total

module load 2021a GCC/10.3.0 OpenMPI/4.1.1

# Once PMIx libraries have replaced the outdated PMI2 libraries on our workernodes:
srun --mpi=pmix_v3 /path/to/mpiapplication <arguments>

# Alternatively using mpirun directly:
mpirun /path/to/mpiapplication <arguments>
```

It is possible to pass `--mca` options in these commands as well.


### Additional Resources
- Use `srun --cpu-bind=help` to show available options to bind CPU resources managed by Slurm to your (MPI-)processes
- `srun --mpi=list` provides a list available MPI interfaces of Slurm
- https://slurm.schedmd.com/mpi_guide.html
