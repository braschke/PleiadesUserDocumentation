---
title: "Best Practices"
---

## Best Practise

### BeeGFS and /tmp
Many and frequent accesses to files on `/beegfs` can produce significant load on the metadata servers.
In consequence, the responsiveness of the shared filesystem goes bad for all users.
There are a couple of things you should avoid when working on `/beegfs`, because of this:
* **Too many files in a single directory**. Each directory is managed by one of the metadata servers and having very many files (e.g. 1000+) in one directory can result in unbalanced blocking operations, if the directory is used in your jobs.
* Frequent lookups in a directory, e.g. through `ls` or implicitly through any `readdir` operation in your program or programming language. The lookup results in a locked operations in the metadata server. This could happen if you frequently check for file status in your job scripts.
* Starting many short running processes (seconds), with the software installed on BeeGFS. Each process is creating a new data stream to read the program data, which can overwhelm the storage system.
* Using `/beegfs` as your working directory for frequent file I/O in your job. Please consider using the local `/tmp` storage. Every worker node is equipped with fast 2TB SSDs for exactly this purpose.
  * Afterwards you can transfer your results to a permanent storage on `/beegfs`
  * If you want to store logfiles etc., consider packing everything in a `.tar` file, since a single large file is better to digest on a parallel filesystem than many small files
  * If you use /tmp in your jobs, please make sure that you clean up the directories you created. Also consider what happens to these files, if your job gets canceled or crashes.

If your job is aborted for any reason, you probably left files in `/tmp` that you want to rescue or remove.
Right now, the best approach is to book an interactive shell on the node and resolve it manually, via:
```bash
srun -p short -w wn21053 -n1 -t 60 --pty /bin/bash
```

> **Note:**
> Your files on BeeGFS are not backed up!
> Be sure to regularly store important results and data on another device.
> In case of a catastrophic failure or event, we won't be able to restore the data.


### Slurm
Every job submission in Slurm introduces some overhead to the batch system.
If you have many short jobs of the same kind, e.g. 2000 x 30 minutes, you should combine your workload in fewer submission scripts or consider using Slurms job arrays.
This way you bundle all of these jobs in a single job submission, but still can treat the items individually as job steps.

Please try to estimate a maximum execution time and set your job time limits accordingly.
  * Specify it with `-t`, shorter than the partitions maximum time
  * This will also increase the likelihood of your job starting early!
  * For longer job times than three days, **if your program regularly saves states/snapshots**:
    * Schedule a first job to the normal partition
    * Use `sbatch -d <jobid>` to set a job dependency to the original job, to continue processing
  * As an exception, the `long` partition allows for longer jobs, but there are a couple of risks involved:
    * Jobs naturally have to wait longer to start, since other (long) jobs may block the partition resources
    * A power or cooling failure might kill your jobs prematurely, wasting all of the computation and energy up to that point
    * Planned maintenance has to wait longer until your jobs are finished. In case of critical security interventions we might be even forced to kill you jobs.
    * All issues are mitigated if your programs have some form of checkpointing to save intermediate states. In this case, use the normal partition as described above.


### Moving Data
The login nodes, especially fugg1 and 2, are mostly intended for job submissions.
If you expect to move larger amounts of data, e.g. to a local computer, consider submitting a job that moves the data from a worker node to your system.
This way, you can shift the workload away from login nodes.
