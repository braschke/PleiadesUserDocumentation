---
title: "Best Practices"
---

On this page we are collecting a couple of best practices of how to use our systems.
Please 


### Best Practices: BeeGFS and /tmp
Many and frequent accesses to files on `/beegfs` can produce significant load on the metadata servers.
In consequence, the responsiveness of the shared filesystem goes bad for all users.
There are a couple of things you should avoid when working on `/beegfs`, because of this:
* **Too many files in a single directory**. Each directory is managed by one of the metadata servers and having very many files (e.g. 1000+) in one directory can result in unbalanced blocking operations, if the directory is used in your jobs.
* Frequent lookups in a directory, e.g. through ls or implicitly through any readdir operation in your program or programming language. The lookup results in a locked operations in the metadata server. This could happen if you frequently check for file status in your job scripts.
* Using `/beegfs` as your working directory for frequent file I/O in your job. Please consider using the local `/tmp` storage. Every worker node is equipped with fast 2TB SSDs for exactly this purpose.
  * Afterwards you can transfer your results to a permanent storage on `/beegfs`
  * If you want to store logfiles etc., consider packing everything in a `.tar` file, since a single large file is better to digest on a parallel filesystem than many small files
  * If you use /tmp in your jobs, please make sure that you clean up the directories you created. Also consider what happens to these files, if your job gets canceled or crashes.


### Best Practices: Slurm
Every job submission in Slurm introduces some overhead to the batch system.
If you have many short jobs of the same kind, e.g. 2000 x 30 minutes, you should combine your workload in a fewer submission scripts or consider using Slurms job arrays.
This way you bundle all of these jobs in a single job submission, but still can treat the items individually as job steps.

Please always consider the maximum execution time and set your job time limits accordingly.
  * Try to always specify maximum time with `-t` that is shorter than the partitions default time
  * You can submit long running jobs to the `long` partition, but there are a couple of risks involved:
    * A power or cooling failure might kill your jobs prematurely, wasting all of the computation and energy up to that point
    * Planned maintenance has to wait longer until your jobs are finished. In case of critical security interventions we might be even forced to kill you jobs.
    * Both issues are mitigated if your programs have some form of checkpointing to save intermediate states
