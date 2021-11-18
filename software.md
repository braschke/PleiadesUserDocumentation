---
title: "Software on Pleiades"
---

### Modules
We offer many software installations through [modules](/software/modules).
There are also alternative, as described below.


### Different compiler versions using singularity
The container management program singularity is installed on the system. If you need a different compiler version in order to be able to compile your program, you can download them as an image from the docker hub by using the command

```bash
singularity pull docker://gcc:<version>
```

This will create a .sif file in your current directory. You can then use the command

```bash
singularity shell <your-.sif-file>
```

to get an interactive shell using the specified compiler version. Compile your program in the way you need to and log out of the container in the usual way.


### Singularity and MPI
Singularity can be used to execute mpi-processes in a container. This can be done by using

```bash
mpirun singularity exec <your-.sif-file> </path/to/program> 
```

Note the order of calling mpi and singularity. Singularity will connect the mpi runtime environment on the host system with the mpi processes in the container.


### Use LCG software environments for different compiler versions
The SFT project provides multiple LCG releases that bundle many very useful software packages. This also contains different versions of the gcc or clang compiler. If CVMFS is available, you can set up a specific release via

```bash
source /cvmfs/sft.cern.ch/lcg/views/LCG_99/x86_64-centos7-gcc8-opt/setup.sh
```

If you are only interested in a single tool, e.g. gcc, you can source the corresponding setup.sh, e.g. in

```bash
/cvmfs/sft.cern.ch/lcg/releases/gcc/8.1.0/x86_64-centos7/setup.sh
```

Please consider if the version of the operating system (here centos7) matches the architecture in that path. For more info refer to the LCG info web page or the README files in /cvmfs/sft.cern.ch/lcg/


### Other software
Please have a look at the [PGI user guide](/software/pgi) to learn how to use the PGI compiler tools on Pleiades.
Additionally, you have access to many of tools of [Intels parallel studio XE 2020](/software/intel) by sourcing

```bash
source /beegfs/Tools/intel/setup.sh
```

This contains compilers, MPI, libraries and profiling tools like VTune Amplifier, Advisor, etc.
