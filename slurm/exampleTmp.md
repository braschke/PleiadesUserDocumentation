```bash
#!/usr/bin/env bash
#SBATCH --nodes 1
#SBATCH --ntasks 1

workdir="/tmp/${USER:?}_${SLURM_JOB_ID:?}"
submitdir="${SLURM_SUBMIT_DIR:?}"

mkdir -p "${workdir}"

function clean_up {
    cd "${submitdir}" || exit
    rm -rf "${workdir:?}"
    exit
}

# Always call "clean_up" when script ends
# This even executes on job failure/cancellation
trap 'clean_up' EXIT

cd "${workdir}" || exit

# Start the workload
echo "hello world" > "test.log"
cat test.log
sleep 10
```
