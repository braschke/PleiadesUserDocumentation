---
title: "Slurm example: Job dependencies"
---


## Slurm example: Job dependencies
Job dependencies are useful to only execute a certain job, if a previous job was executed.
More details are available through [`man sbatch`](https://slurm.schedmd.com/sbatch.html).

### Job dependency graph
In the following example, we create the following dependency structure between jobs A, B, ..., F
```
   A
  /|\
 / | \
B  C  D
|  |
E  |
 \ |
  \|
   F
```

A corresponding job-submission script could look like this:
```bash
#!/usr/bin/env bash
# Example of how to submit multiple jobs with job dependencies.

# Submit job "A" with the sbatch command
# sbatch returns the sentence "Submitted batch job <jobid>"
# Use grep to only select the number
jobidA="$(sbatch --job-name=A sleepjob.sh | grep -o -E '[0-9]+')"

# Now we can use the filtered job id to submit dependent jobs
jobidB="$(sbatch --job-name=B --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"
jobidC="$(sbatch --job-name=C --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"
jobidD="$(sbatch --job-name=D --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"

jobidE="$(sbatch --job-name=E --dependency=${jobidB} sleepjob.sh | grep -o -E '[0-9]+')"

jobidF="$(sbatch --job-name=F --dependency=${jobidC},${jobidE} sleepjob.sh | grep -o -E '[0-9]+')"
```

### Singleton job dependency
Another interesting approach is a singleton job.
Here you can submit many jobs with exactly the same job name.
By using `--dependency=singleton`, Slurm will only execute

```bash
#!/usr/bin/env bash

# Use the "singleton" flag to submit multiple jobs
# with the same name. This combination will make sure
# That only a single job runs at any given time.

for I in $(seq 10)
do
    sbatch --job-name=A --dependency=singleton sleepjob.sh
done

# Slurm will only every execute a single job with name "A"
```

This structure can be used to automatically continue computations after the maximum 3 days of our `normal` partition.
