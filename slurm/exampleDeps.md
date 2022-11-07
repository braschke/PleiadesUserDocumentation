```bash
#!/usr/bin/env bash

# Example of how to submit multiple jobs with job dependencies.
# The dependency structure here looks like this
#     A
#    /|\
#   / | \
#  B  C  D
#  |  |
#  E  |
#   \ |
#    \|
#     F

# Submit job "A" with the sbatch command
# sbatch returns the sentence "Submitted batch job <jobid>"
jobidA="$(sbatch --job-name=A sleepjob.sh | grep -o -E '[0-9]+')"

# Now we can use the filtered job id to submit dependent jobs
jobidB="$(sbatch --job-name=B --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"
jobidC="$(sbatch --job-name=C --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"
jobidD="$(sbatch --job-name=D --dependency=${jobidA} sleepjob.sh | grep -o -E '[0-9]+')"

jobidE="$(sbatch --job-name=E --dependency=${jobidB} sleepjob.sh | grep -o -E '[0-9]+')"

jobidF="$(sbatch --job-name=F --dependency=${jobidC},${jobidE} sleepjob.sh | grep -o -E '[0-9]+')"
```

```bash
#!/usr/bin/env bash

# Use the "singleton" flag to submit multiple jobs
# with the same name. This combination will make sure
# That only a single job runs at any given time.

for I in $(seq 10)
do
    sbatch --job-name=A --dependency=singleton sleepjob.sh
done
```
