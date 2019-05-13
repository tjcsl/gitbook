# MPI

We don't really use **MPI** for any day-to-day tasks in the CSL, but it's important to know how it works to support some of the high-powered Senior Research projects going on [The Cluster](../../services/cluster/README.md)

## Example MPI Program

Copied from a tutorial somewhere.

```c
// hello_mpi_world.c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    // Initialize the MPI environment
    MPI_Init(NULL, NULL);

    // Get the number of processes
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    // Get the rank of the process
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    // Get the name of the processor
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);

        // Print off a hello world message
        printf("Hello world from processor %s, rank %d out of %d processors\n",
               processor_name, world_rank, world_size);

        // Finalize the MPI environment.
        MPI_Finalize();
    }
```

Then, compile with `mpicc --std=c99 hello_mpi_world.c -o hello_mpi_world`
And run with [Slurm](../../services/cluster/slurm.md) using `srun -N4 ./hello_mpi_world` (Or locally using `mpirun -n4 ./hello_mpi_world`)

## What MPI software we use

 * **PMIX 2.2.2**
 * **OpenMPI 3.1.3**

See the relevant [Ansible](../tools/ansible.md) plays in [Cluster Administration](../../services/cluster/administration.md) for more details on how it is installed.
