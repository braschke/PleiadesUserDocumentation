---
title: "Hardware"
---

## Hardware
The cluster consists of 268 workernodes with 14848 Cores in total.
Additionally, there are 5 GPU nodes with 8 NVidia A100 GPUs and 128 Cores each.
931 TB of parallel storage are provided with BeeGFS.


### Node Types
- **Login**: 
  - `fugg1` and `fugg2`: Virtual machine on Intel Xeon Gold 6238R CPUs
    - The fugg login nodes are intended for job submission and light interactive workloads.
    - Because of architecture differences and limited resources, you should try to compile your programs through our batch system or work in interactive slurm sessions.
    - `fugg2` has infiniband-enabled access to BeeGFS
  - `top` and `higgs` (only available to whep users)
- **wn21[001-268]**: 
  - 2 sockets with AMD EPYC 7452 32-Core processor. 64 Cores in total
  - Hyperthreading disabled
  - 256GB memory, 4GB per thread
- **gpu21[001-005]**:
  - 8 GPUs: NVidia HGX A100
  - 2 sockets with AMD EPYC 7763 64-Core processor. 128 Cores in total
  - Hyperthreading disabled
  - 2TB memory, 16GB per thread
  - More details here: [Using GPUs]({{ site.baseurl }}{% link gpu.md %})
- **wn19[01-08]**: Intel(R) Xeon(R) Gold 6152 CPU @ 2.10GHz
  - Hyperthreading **enabled**
  - 176GB memory, 2GB per thread


### Network
The CPU Worker nodes wn21[001-268] are connected to each other and to the BeeGFS servers via infiniband and ethernet.
All GPU nodes (gpu21[001-005]) are in a separate infiniband network and access BeeGFS via ethernet.
The login nodes are currently connected to all other nodes via ethernet.

[![Hardware and network layout of PLEIADES](assets/img/pleiades_layout.jpg)](assets/img/pleiades_layout.jpg)
