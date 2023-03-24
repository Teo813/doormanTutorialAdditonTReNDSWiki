---
layout: default
title: FAQ
nav_order: 7
last_modified_date: 10/25/2022 11:00
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Common cluster errors and remedies

### matlab: command not found

-   Load the Matlab module

`$ module load matlab/R2022a`

### Lmod has detected the following error:  The following module(s) are unknown

-   Set the `MODULEPATH` variable manually

```
$ source /usr/share/lmod/lmod/init/bash
$ module use /application/ubuntumodules/localmodules
```

### Error using parpool ... Parallel pool failed to start with the following error ...

-   Delete contents of `~/.matlab/local_cluster_jobs`

`$ rm -r ~/.matlab/local_cluster_jobs`

-   Delete contents of `~/.matlab`

`$ rm -r ~/.matlab`

### No space left on device

-   The storage system you are trying to use is full. Try to identify
    the particular storage system and delete some unneeded files from
    there.

### No such file or directory

-   The path you specified is not accessible from the node where the job
    is running. Identify the node by putting `echo $HOSTNAME` at the
    beginning of your `sbatch` script. Try to start an interactive
    session on the said node and see if the directory is accessible from
    the command line.

### Job does not run and there is no \*.out or \*.err file

-   Make sure the directory where the out/err files are supposed to be
    written exists.
-   It is possible that the node where the job step was scheduled has
    dropped storage mount. Try to identify the node where the job was
    scheduled using either one of the following commands:

```
$ squeue -j <jobID>
$ sacct -j <jobID> --format=jobid,elapsed,ncpus,ntasks,state,NodeList
```

### setfacl: permission not supported

-   It might be an NFS4 volume, use `nfs4_setfacl` instead: [Permissions
    on NFS volumes](Storage_guide#Permissions_on_NFS_volumes)

## Other questions

### How can I get information about CPU/memory of the cluster

```
$ sinfo -o "%24n %7P %.11T %.4c %.8m %14C %10e"
HOSTNAMES                PARTITI       STATE CPUS   MEMORY CPUS(A/I/O/T)  FREE_MEM
trendscn001.rs.gsu.edu   qTRD          mixed   32   768697 2/30/0/32      716707
trendscn004.rs.gsu.edu   qTRD          mixed   32   768697 15/17/0/32     409574
trendscn007.rs.gsu.edu   qTRD          mixed   32   768697 16/16/0/32     653037
trendscn008.rs.gsu.edu   qTRD          mixed   32   768697 1/31/0/32      562276
trendscn011.rs.gsu.edu   qTRD          mixed   32   768697 16/16/0/32     461971
```

Above example shows the nodes, which partition each of them belongs to,
their state (state of the CPUs, idle/allocated/mixed), total CPUSs and
memory, number of CPUs allocated/idle/other/total (A/I/O/T), and free
memory.

On a general purpose compute or high memory node, you can get
information (configuration and usage) about the CPUs by running `htop`
command. On a GPU node, you can get information about the GPUs by
running `nvidia-smi` command.

### How do I check which nodes are down?

```
$ sinfo -R
```

### How do I allocate/avoid allocating a particular node?

Using the following argument in `srun`:

```
# allocate on a particular node
$ srun ... --nodelist <hostname> ...
$ srun ... -w <hostname> ...

# avoid a particular node
$ srun --exclude <hostname> ...
$ srun -x <hostname> ...
```
Or use the following `#SBATCH` directives:

```
# allocate on a particular node
#SBATCH --nodelist <hostname>
#SBATCH -w <hostname>

# avoid a particular node
#SBATCH --exclude <hostname>
#SBATCH -x <hostname>
```

### How can I limit the number of simultaneously running tasks?

If you want to run 1000 tasks but not use the whole cluster and leave
some resources for others to use, you can use
`sbatch --array=1-1000%100 JobSubmit.sh` to limit the number of
simultaneously running jobs to 100.

### What is the max time-limit for jobs?

Currently 5 days and 8 hours. Use `sinfo` command to view time limit for
different queues.

### How can I increase time-limit of running jobs?

The following command does it:

`$ scontrol update jobid=<job_id> TimeLimit=<new_timelimit>`

### How can I update task limit of a running job array?

The following command does it:

`$ scontrol update ArrayTaskThrottle=<count> JobId=<jobID>`

### Who are the users with administrative privilege?

Vince `<vcalhoun>`, Sergey `<splis>`, Suranga `<neranjan>`.

### Why should I not run any CPU/memory intensive task on the login node?

The login node is used by *all* the users in TReNDS for submitting jobs,
starting interactive sessions and hopping on to the DEV nodes. As a
result, any CPU/memory intensive task (such as running a computation in
Matlab for hours) disrupts all of these processes and slows *everyone*
down.

### Why should I not ssh directly on to one of the compute nodes and run my analysis?

The SLURM job scheduler allocates resources based on the requested
CPU/memory of all users. If someone bypasses the scheduler and run
analysis on the worker nodes, it may exhaust resources and cause
unpredictable issues.

### What kind of unpredictable issues can be caused?

So far we have seen jobs terminating without any error, jobs terminating
with segmentation faults and out-of-memory errors, corrupted files being
written to the disk, very slow response to any kind of command etc.

### Why am I getting "error: Unable to allocate resources: Invalid account or account/partition combination specified"

You may have specified invalid project code. 
Please take a note of your slurm account code from [https://elpis.rs.gsu.edu/](https://elpis.rs.gsu.edu/).

### My job completed without any error, but there is nothing in the output!

Did you write the job submission script in a Windows system, then
transferred it to the cluster? You may need to convert the line-endings
in that file using `dos2unix `<filename> command.

Also make sure the job script and the error file locations are accessible on the node where the job was scheduled. 
You can find the erroneous node by putting `echo $HOSTNAME >&2` in the job submission script.

### Why does my job not start?

Use `squeue -u `<campusID> to check the state and reason. To find what
the state means, [see
this](https://slurm.schedmd.com/squeue.html#SECTION_JOB-STATE-CODES). To
find what the reason means, [see
this](https://slurm.schedmd.com/squeue.html#SECTION_JOB-REASON-CODES).
Some common states:

- CG COMPLETING: Job is in the process of completing. Some processes on some nodes may
still be active.
- F FAILED: Job terminated with non-zero exit code or other failure condition.
- OOM OUT_OF_MEMORY: Job experienced out of memory error.
- PD PENDING Job is awaiting resource allocation.
- PR PREEMPTED: Job terminated due to preemption.
- R RUNNING: Job currently has an allocation.
- S SUSPENDED: Job has an allocation, but execution has been suspended and CPUs have
been released for other jobs.

### Why is my job in PD (pending) state?

SLURM uses fair tree algorithm to decide the priority of pending jobs.
Once resource is available, the pending job with highest priority is
immediately allocated. To view the priority, use `sprio -l` command. To
learn more about the fair tree algorithm, [see
this](https://slurm.schedmd.com/fair_tree.html).

