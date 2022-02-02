---
title: "Hardware"
---

## Hardware
The cluster consists of 232 workernodes, with , there are 14848 Cores in total.
Each node has 256 GB of memory.
Additionally, there are 5 GPU nodes with 8 NVidia A100 GPUs and 2 AMD EPYC 7763 64-Core CPUs each.
There is 931 TB of parallel storage (BeeGFS).


### Node Types
- **Login**: 
  - `fugg1`: Virtual machine on Intel Xeon Gold 6238R CPUs
    - The fugg login nodes are intended for job submission and light interactive workloads.
    - Because of architecture differences and limited resources, you should try to compile your programs through our batch system or work in interactive slurm sessions.
  - `top` and `higgs` (only available to whep users)
- **wn21[001-232]**: 
  - 2 sockets with AMD EPYC 7452 32-Core processor. 64 Cores in total
  - Hyperthreading disabled
  - 256GB memory, 4GB per thread
- **gpu21[001-005]**:
  - 2 sockets with AMD EPYC 7763 64-Core processor. 128 Cores in total
  - Hyperthreading disabled
  - 2TB memory, 16GB per thread
  - More details here: [Using GPUs]({{ site.baseurl }}{% link software/gpu.md %})
- **wn19[01-08]**: Intel(R) Xeon(R) Gold 6152 CPU @ 2.10GHz
  - Hyperthreading **enabled**
  - 176GB memory, 2GB per thread


### Network
The CPU Worker nodes wn21[001-232] are connected to each other and to the BeeGFS servers via infiniband and ethernet.
All GPU nodes (gpu21[001-005]) are in a separate infiniband network and access BeeGFS via ethernet.
The login nodes are currently connected to all other nodes via ethernet.

[![Hardware and network layout of PLEIADES](assets/img/pleiades_layout.jpg)](assets/img/pleiades_layout.jpg)
