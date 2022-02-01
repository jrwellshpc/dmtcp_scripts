# DMTCP scripts to get a Python number checker working in SLURM.

## Operation
'sbatch slurm_counter.job' and let it run for 60 seconds. Have a peak at the output so you know where it was, then requeue the job, or kill and run it again. You'll see the job pickup from the last save point, likely within 10 seconds.

## Files
slurm_counter.job is a SLURM script that 1) Submits a new Python job through DMTCP. However, if it finds a dmtcp_restart_script.sh in the same directory, it helpfully will run dmtcp_restart on the latest checkpoint that the created. This is handy on clusters where jobs can be requeued when a higher priority job comes along.

counter.py is a counter. Output will be sent to counter.\<jobid\>.out. 

