# DMTCP scripts to get a Python number checker working in SLURM.

## Operation
'sbatch slurm_counter.job' and let it run for 60 seconds. Have a peak at the output so you know where it was, then requeue the job, or kill and run it again. You'll see the job pickup from the last save point, likely within 10 seconds.

## Files
slurm_counter.job is a SLURM script that 1) Submits a new Python job through DMTCP. However, if it finds a dmtcp_restart_script.sh in the same directory, it helpfully will run dmtcp_restart on the latest checkpoint that the created. This is handy on clusters where jobs can be requeued when a higher priority job comes along.

IMPORTANT: Within slurm_counter.job you'll see dmtcp_restart and dmtcp_launch with a "-i 10". That means create a checkpoint every 10 seconds. This is a lot of overhead so I'd recommend changing that to 3600 (once an hour) when you are ready.

counter.py is a simple Python counter. Output will be sent to counter.\<jobid\>.out as it counts.

