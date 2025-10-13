---
description: This document describes how to properly use the CSL's 53-node computer cluster
---

# Slurm

If you aren't familiar with the layout of the HPC Cluster, it's highly recommended that you read the parent page, [Cluster,](cluster-introduction.md) before delving into Slurm and running jobs, to avoid any confusion over terminology used here. After you have done so, please thoroughly read this page before using the cluster.

## What is Slurm?

Slurm is a free, open-source job scheduler which provides tools and functionality for executing and monitoring parallel computing jobs. It ensures that any jobs which are run have exclusive usage of the requested amount of resources, and manages a queue if there are not enough resources available at the moment to run a job. Your processes won't be bothered by anybody else's processes; you'll have complete ownership of the resources that you request.

## How do you use it?

Slurm is very user-friendly.  You don't necessarily have to have an academic use for the cluster, but keep in mind that any use of the HPC cluster is bound by the FCPS Acceptable Use Policy, just like the rest of TJ's computing resources, and academic jobs will have priority use of Cluster resources. All TJ students are granted cluster accounts in the beginning of the year. If you believe your cluster account does not exist or is broken email [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu)

{% hint style="info" %}
As of 2027, the easiest way to use Slurm is via the "Job Composer" tab at [https://ondemand.tjhsst.edu/](https://ondemand.tjhsst.edu/) - see [ondemand](ondemand/ "mention")for more information
{% endhint %}

### The Login Node

{% hint style="warning" %}
As of October 2025, the old login node, `infocube`, has been decommissioned and replaced with the new login node: `infoprism`.
{% endhint %}

To get started with running jobs on the Cluster you should connect to the **login node**, which is `infoprism` in this case. Any of the following commands while on `ras` or  TJ CSL computer will allow you to connect to `infoprism`:

```bash
ssh infoprism
```

After connecting to `infoprism` you will be placed into your Cluster home directory (`/csl/users/<username>`).  `infoprism` is a virtual machine and does not have nearly the amount of resources as the entire Cluster does, so **do not run programs directly on infocube**. Instead, you want to tell Slurm to launch a **job**

Jobs are how you can tell Slurm what processes you want run, and how many resources those processes should have. Slurm then goes out and launches your program on one or more of the actual HPC cluster nodes. This way, time consuming tasks can run in the background without requiring that you always be connected, and jobs can be queued to run at a later time.

### Viewing information about the Cluster

Our Cluster is split into two partitions: `compute` and `gpu`. All nodes that are in the `gpu` partition are in the `compute` partition, but not vice versa. Nodes in the `gpu` partition have GPUs installed which can be accessed through Slurm.

To see information about the nodes of the cluster, you can run `sinfo`. You should get a table similar to this one:

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
compute*     up   infinite      1    mix hpc9
compute*     up   infinite      8  alloc hpc[1-8]
compute*     up   infinite      3   idle hpc[10-12]
gpu          up   infinite      1  down* hpcgpu
```

* `idle` means that that block of nodes is not currently in use, and will be immediately allocated to any job that requests resources.
* `alloc` means that the node is busy and will not be available for any other jobs until the job is complete
* `mix` means that some of the cores within the node are allocated and others are free. Because this is annoying, it is good etiquette to allocate your jobs in multiples of full nodes (24 cores)
* `down` means that the node cannot currently be used.
* If a state ends with `*` that means those nodes cannot have jobs scheduled on them (useless nodes)
  * i.e if a node is in STATE `idle*`, no jobs will be scheduled onto that node, even though it is marked as `idle`.

To see which jobs are running and who started them, run `squeue`. You should see a table like this:

```
JOBID  PARTITION   NAME     USER    ST      TIME   NODES  NODELIST(REASON)
 882    compute   mpirun  2017ggol   R     44:56    12     hpc[1-12]
 884    compute    echo   2017ggol  PD      0:00     6     (Resources)
```

`ST` stands for state. The two common states are `R`, which means the job is currently running, and `PD`, which stands for pending. If the job is running, the rightmost column displays which nodes the job is running on. If the job is pending, the rightmost column displays why the job is not yet running. In this example, job 884 is waiting for six nodes worth of resources because job 882 is running on all 12 of the available nodes.

### Creating Programs to Run of the HPC Cluster

The HPC Cluster is comprised of 64-bit Ubuntu Linux systems. While you can run any old Linux program on the Cluster, to take advantage of the parallel processing capability that the Cluster has, it's _highly_ recommended to make use of a parallel programming interface. If you're taking or have taken Parallel Computing, you will know how to write and compile a program which uses MPI. If you aren't, [http://condor.cc.ku.edu/\~grobe/docs/intro-MPI-C.shtml](http://condor.cc.ku.edu/~grobe/docs/intro-MPI-C.shtml) is a good introduction to MPI in C. See below for instructions on running an MPI program on the cluster.

When compiling your program, it's best to connect to `infoprism` (the login node explained in the section above), so that your code is compiled in a similar environment to where it will be run. The login node should have all the necessary tools to do so, such as gcc, g++, and mpicc/mpixx.\
**WARNING:** compiling your program on a workstation/any other computer that is not part of the cluster and then transferring the generated executable over to the cluster **WILL NOT WORK**. This is called _CROSS COMPILATION_ and it **WILL NOT WORK**. This is not a challenge, it is a statement of fact&#x20;

### Running a Job

And now the good stuff: running a job! Slurm provides 3 main methods of doing so:

#### `salloc`

[Documentation](https://slurm.schedmd.com/salloc.html)

Salloc allocates resources for a generic job and, by default, creates a shell with access to those resources. You can specify what resources you want to allocate with command line options (run `man salloc` to see them all), but the only one you need for most uses is `-n [number]` which specifies how many cores you want to allocate. You can also specify a command simply by placing it after all command line options (ex: `salloc -n 4 echo "hello world"`). This is currently the suggested way to run MPI jobs on the cluster. To run MPI jobs, first you must load the mpi module, as stated above (`module load mpi`). After that, simply run `salloc -n [number of cores] mpiexec [your program]`. Unfortunately, the displayed name of this job is, by default, just "mpiexec", which is not helpful for anyone. To give it a name, pass salloc (NOT mpirun) `--job-name=[name]`

```bash
$ salloc -n 4 bash # spawns a bash shell with access to 4 cores
$ salloc -n 4 mpiexec ./a.out # runs an mpi program (a.out) with 4 cores
```

#### `srun`

[Documentation](https://slurm.schedmd.com/srun.html)

This is the simplest method, and is probably what you want to start out with. All you have to do is run `srun -n (processes) (path_to_program)`, where `(processes)` is the number of instances of your program that you want to run, and `(path_to_program)` is, you guessed it, the path to the program you want to run. If your program is an MPI program, you should not use `srun`, and instead use the `salloc` method described above.

If your command is successful, you should see "srun: jobid (x) submitted". You can check on the status of your job by running `sacct`. You will receive any output of your program to the console. For more resource options, run `man srun` or use the official Slurm documentation.

#### `sbatch`

[Documentation](https://slurm.schedmd.com/sbatch.html)

`sbatch` allows you to create batch files which specify a job and the resources required for the job and submit that directly to Slurm, instead of passing all the options to `srun`. Here's an example script, and assume you save it as `test.sh`:

```bash
#!/bin/bash
#SBATCH -n 4
#SBATCH --time=00:30:00
#SBATCH --ntasks-per-node=2

srun (path_to_program)
```

You could then submit the program to slurm using `sbatch test.sh`. This would tell Slurm to launch the program at `(path_to_program)`, and to launch 4 tasks, limit the maximum execution time to 30 minutes, and require that no more than two tasks run on a specific system. Here are some other examples: [https://www.hpc2n.umu.se/batchsystem/examples\_scripts.](https://www.hpc2n.umu.se/documentation/batchsystem/basic-submit-example-scripts)
