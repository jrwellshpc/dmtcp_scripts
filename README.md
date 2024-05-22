# DMTCP Scripts to Get Python Working in SLURM

## Theory of Operation
counter\slurm_counter.job, and gpu\tf.job, are SLURM scripts that submit a new Python job through the Distributed MultiThreaded CheckPointing (DMTCP) package that has been installed on a cluster. However, if it finds a dmtcp_restart_script.sh in the same directory (a pointer to the dmtcp_restart_script_\*.sh files), it helpfully will run dmtcp_restart on the latest checkpoint (ckpt_platform-\*.dmtcp files) that was created. This is important on clusters where jobs can be requeued when a higher priority job comes along. Restarting the job without checkpointing will increase the researchers usage, which only lowers their priority even further. Checkpointing is the way to escape that loop.

IMPORTANT: Within the sbatch files you'll see dmtcp_restart and dmtcp_launch with a "-i 10". That means create a checkpoint every 10 seconds. This is a lot of overhead so I'd recommend changing that to 600 (every 10 minutes) when you are ready.

## Instructions
Download this repository and run the non .py files in these directories through sbatch and let them run for 2 minutes. Have a peak at the output ('ls -alt' to find the files) so you know where it was, then requeue the job, or kill and run it again. You'll see the job pickup from the last save point, likely within 10 seconds if you kept the '-i 10' in place.

## Files
Within GitHub:  
counter\counter.py is a simple Python counter.  
counter\slurm_counter.job is the sbatch file that submits counter.py to the scheduler through DMTCP.  
gpu\tf.py is a Python script that uses Tensorflow to do a model.fit over 500 epochs.  
gpu\slurm_tf.job is the sbatch file that submits tf.py to the scheduler through DMTCP.  

Created at Runtime:  
ckpt_platform-\*.dmtcp The checkpointing file.  
dmtcp_restart_script_\*.sh Makes submiting the checkpoint file to DMTCP easier.  
dmtcp_restart_script.sh A pointer to the other dmtcp_restart file. 
