# Slurm

If you aren't familiar with the layout of the HPC Cluster, it's highly recommended that you read the parent page, [Cluster,](./) before delving into Slurm and running jobs, to avoid any confusion over terminology used here. After you have done so, please thoroughly read this page before using the cluster.

## What is Slurm?

Slurm is a free, open-source job scheduler which provides tools and functionality for executing and monitoring parallel computing jobs. It ensures that any jobs which are run have exclusive usage of the requested amount of resources, and manages a queue if there are not enough resources available at the moment to run a job. Your processes won't be bothered by anybody else's processes; you'll have complete ownership of the resources that you request.

## How do you use it?

Slurm is very user-friendly. However, it requires that you have an account on the HPC Cluster, which you'll need to ask a Student Systems Administrator for. You can either do this in-person in Room 200 \(the CSL\), or by emailing [sysadmins@tjhsst.edu](mailto:sysadmins@tjhsst.edu). You don't necessarily have to have an academic use for the cluster, but keep in mind that any use of the HPC cluster is bound by the FCPS Acceptable Use Policy, just like the rest of TJ's computing resources, and academic jobs will have priority use of Cluster resources. Once you have had an account created for you, you can begin.

### The Login Node

If you want to compile and/or run a program, either that you have created or one created by somebody else, you will connect to the **login node**. The login node is a virtual machine with not very many resources relative to the rest of the HPC cluster, so you _don't_ want to run programs directly on the login node. Instead, you want to tell Slurm to launch a **job**.

Jobs are how you can tell Slurm what processes you want run, and how many resources those processes should have. Slurm then goes out and launches your program on one or more of the actual HPC cluster nodes. This way, time consuming tasks can run in the background without requiring that you always be connected, and jobs can be queued to run at a later time.

The login node's name is **infosphere**. To connect to it, use SSH from remote.tjhsst.edu or a CSL workstation \(`ssh infosphere` while on remote.tjhsst.edu or a workstation\). If you don't want to remember "infosphere", "hpc" is aliased to infosphere and works just the same \(`ssh hpc`\). Connecting should be simple - you shouldn't have to enter a password as it should use your session from the computer you already logged in to. If not, just reenter your password that you use to log in to TJ resources \(such as Ion\).

Something important to note is that your home directory on the HPC Cluster is separate from your home directory on remote.tjhsst.edu and CSL workstations; this is because it's a different system optimized for speed. But don't worry, your Cluster home directory is shared across all HPC Cluster resources, so a program running on a compute node can access files that you create on the login node.

### Viewing information about the Cluster

To see information about the nodes of the cluster, you can run `sinfo`. You should get a table similar to this one:

```text
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
compute*     up   infinite      1    mix hpc9
compute*     up   infinite      8  alloc hpc[1-8]
compute*     up   infinite      3   idle hpc[10-12]
gpu          up   infinite      1  down* hpcgpu
```

* `idle` means that that block of nodes is not currently in use, and will be immediately allocated to any job that requests resources.
* `alloc` means that the node is busy and will not be available for any other jobs until the job is complete
* `mix` means that some of the cores within the node are allocated and others are free. Because this is annoying, it is good etiquette to allocate your jobs in multiples of full nodes \(24 cores\)
* `down` means that the node cannot currently be used.

To see which jobs are running and who started them, run `squeue`. You should see a table like this:

```text
JOBID  PARTITION   NAME     USER    ST      TIME   NODES  NODELIST(REASON)
 882    compute   mpirun  2017ggol   R     44:56    12     hpc[1-12]
 884    compute    echo   2017ggol  PD      0:00     6     (Resources)
```

`ST` stands for state. The two common states are `R`, which means the job is currently running, and `PD`, which stands for pending. If the job is running, the rightmost column displays which nodes the job is running on. If the job is pending, the rightmost column displays why the job is not yet running. In this example, job 884 is waiting for six nodes worth of resources because job 882 is running on all 12 of the available nodes.

### Creating Programs to Run of the HPC Cluster

The HPC Cluster is comprised of 64-bit CentOS Linux systems. While you can run any old Linux program on the Cluster, to take advantage of the parallel processing capability that the Cluster has, it's _highly_ recommended to make use of a parallel programming interface. If you're taking or have taken Parallel Computing, you will know how to write and compile a program which uses MPI. If you aren't, [http://condor.cc.ku.edu/~grobe/docs/intro-MPI-C.shtml](http://condor.cc.ku.edu/~grobe/docs/intro-MPI-C.shtml) is a good introduction to MPI in C. See below for instructions on running an MPI program on the cluster.

When compiling your program, it's best to connect to infosphere \(the login node explained in the section above\), so that your code is compiled in a similar environment to where it will be run. The login node should have all the necessary tools to do so, such as gcc, g++, and mpicc/mpixx.

**Important note: You won't be able to run mpicc or other special compilation tools until you load the appropriate programs into your environment. For MPI, the command to do so is** `module load mpi`**.** The reason for this is different compiler systems can conflict with each other, and the module system gives you the flexibility to use whatever compiler you want by loading the appropriate modules.

### Running a Job

And now the good stuff: running a job! Slurm provides 3 main methods of doing so:

#### `salloc`

Salloc allocates resources for a generic job and, by default, creates a shell with access to those resources. You can specify what resources you want to allocate with command line options \(run `man salloc` to see them all\), but the only one you need for most uses is `-n [number]` which specifies how many cores you want to allocate. You can also specify a command simply by placing it after all command line options \(ex: `salloc -n 4 echo "hello world"`\). This is currently the suggested way to run MPI jobs on the cluster. To run MPI jobs, first you must load the mpi module, as stated above \(`module load mpi`\). After that, simply run `salloc -n [number of cores] mpirun [your program]`. Unfortunately, the displayed name of this job is, by default, just "mpirun", which is not helpful for anyone. To give it a name, pass salloc \(NOT mpirun\) `--job-name=[name]`

#### `srun`

This is the simplest method, and is probably what you want to start out with. All you have to do is run `srun -n (processes) (path_to_program)`, where `(processes)` is the number of instances of your program that you want to run, and `(path_to_program)` is, you guessed it, the path to the program you want to run. If your program is an MPI program, Slurm will automatically pass the appropriate hosts and task amount to your program; you should not use mpirun.

If successful, you should see "srun: jobid \(x\) submitted". You can check on the status of your job by running `sacct`. You will receive any output of your program to the console. For more resource options, run `man srun` or use the official Slurm documentation.

#### `sbatch`

`sbatch` allows you to create batch files which specify a job and the resources required for the job and submit that directly to Slurm, instead of passing all the options to `srun`. Here's an example script, and assume you save it as `test.sh`:

```bash
#!/bin/bash
#SBATCH -n 4
#SBATCH --time=00:30:00
#SBATCH --ntasks-per-node=2

srun (path_to_program)
```

You could then submit the program to slurm using `sbatch test.sh`. This would tell Slurm to launch the program at `(path_to_program)`, and to launch 4 tasks, limit the maximum execution time to 30 minutes, and require that no more than two tasks run on a specific system. Here are some other examples: [https://www.hpc2n.umu.se/batchsystem/examples\_scripts](https://www.hpc2n.umu.se/batchsystem/examples_scripts).

### X forwarding

Sorry, it doesn't work yet.

If it did, and your program outputs graphics, then you need to X-forward \(X is linux graphics\) from the remote machine \(infosphere\) to your machine. To do this, use the `-X` flag when `ssh`-ing all the way to infosphere. Then, you need to use the `--x11` flag when running a slurm command. An example series of commands is listed below:

```text
[you@yourmachine:~]$ ssh -X 20xxyyou@remote.tjhsst.edu
[20xxyyou@ras2:~]$ ssh -X infosphere
[20xxyyou@infosphere:~]$ srun --x11 --other_flags ./your_program
```

