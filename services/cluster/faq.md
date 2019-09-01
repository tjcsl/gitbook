# FAQ

Because I'm tired of explaining the same things over and over again

## Using the Cluster

* How do I get my code on the cluster?
  1. From a [workstation](../workstations.md) with your code on it:

     ```bash
     scp -rp folder_with_your_code/ infosphere:~
     ```

  2. Your code is now on infosphere
* Where do I go to run code on the cluster?
  * From [RAS](../remote-access/) or a [workstation](../workstations.md):

    ```bash
    ssh infosphere
    ```
* How do I compile code on the cluster?
  * If using a single C/C++ file like a pleb:

    ```bash
    mpicc your_file.c # or mpicxx if using C++
    ```

  * If using CMake like a boss:

    ```bash
    cmake . -DCMAKE_C_COMPILER=mpicc -DCMAKE_CXX_COMPILER=mpicxx
    make
    ```
* Do I need to compile my code on the cluster?
  * **YES**, there is no other option, binaries will _**never**_ be compatible. This is not a challenge, this is a statement of fact. Do not complain if you `a.out` from the workstation doesn't work.
* Now that I did all that, how do I run code on the cluster?
  * If using an MPI binary _\(most parallel computing classes\)_, or just running a non-MPI binary `n` number of times:

    ```bash
    srun -N <number_of_machines> -n <number_of_processes> --mpi=pmix_v2 ./your_binary --your-binary-flags
    ```

  * Alternative \(non-preferred\) method of running an MPI binary:

    ```bash
    salloc -N <number_of_machines> -n <number_of_processes> mpiexec ./your_binary --your-binary-flags
    ```
* That gives me some error about not having an account! What gives?
  * Ask a sysadmin if they can make you an account on the cluster. Point them to this document if they don't know how.
* My job just hangs forever!
  * Either someone is inconsiderately using the entire cluster, or the cluster is broken.
  * To check the former, use the `sinfo` and `squeue` commands.

## Managing the Cluster

Or, "I was put in charge of your mess Jack you better have documented everything." Don't worry, I didn't. Keeps you on your toes.

Note: everything here expects you have root access on infosphere, and are running all these commands after getting root with [Kerberos](../../technologies/authentication/kerberos.md) `kinit` + `ksu`.

* What are the basic troubleshooting steps?
  * `sinfo` to view the state of the cluster
  * `squeue` to see what jobs are holding things up
  * Checking files in `/var/log/slurm` on infosphere and the affected nodes
* Someone wants me to make an account for them!
  * Run this in `/root` on infosphere:

    ```bash
    cat your_user_list.txt | ./make_new_users.sh
    ```
* How do I run the [ansible](../../technologies/tools/ansible.md) play?
  * From infosphere \(this is important\):

    ```bash
    cd ansible
    ansible-playbook -i hosts -f 50 --ask-vault-pass cluster.yml --skip-tags=install
    ```

  * The above command basically runs the default, updating-only play on all the cluster nodes, including infosphere.
* I want to upgrade a specific part of the cluster, how do?
  * There are a couple partitions in ansible:
    * `hpc`: All the [HPC](../../machines/hpc-cluster/) nodes
    * `hpcgpu`: Just [Zoidberg](../../machines/hpc-cluster/zoidberg.md)
    * `ibm`: All the [Borg](../../machines/borg-cluster.md) nodes
    * `ibmgpu`: All the Borg nodes with GPUs in them
    * `clustermaster`: infosphere itself
  * Run the corresponding ansible `.yml` playbooks from infosphere
* I would like to install a new version of something on the cluster, how do?
  * The current software names and versions are under `[fullcluster:vars]` in the hosts file in the [ansible](../../technologies/tools/ansible.md) repository.
  * Change the version number to the latest available, and run the following command replacing `<software_name>` with something like `abinit` or `openmpi`.

    ```bash
    ansible-playbook -i hosts -f 50 cluster.yml --tags=install_<software_name>
    ```

  * The plays should automatically uninstall the old version for you when updating.
  * _**IMPORTANT NOTE**_: If you update `pmix`, you also need to reinstall `slurm` and `openmpi`.
* After a slurm upgrade, everything went down. How fix?
  * Check that the slurm version is the same everywhere
  * `ansible fullcluster -i hosts -m command -a "bash -c 'systemctl disable slurmd; systemctl enable slurmd; systemctl restart slurmd'"` \(disable slurmd on infosphere afterwards\)
  * Really, just check the logs yourself, listen to error messages, Google your way to victory.
* How to recreate the `slurmdbd` database \(last resort\)?
  * `systemctl stop slurmdbd slurmctld && systemctl restart mariadb && mysql`
    1. `CREATE DATABASE slurm;`
       * If this command fails, skip to the end of this list
    2. `USE slurm;`
    3. `SHOW TABLES;`
    4. For every table in the database, `DROP TABLE <table_name>;`
    5. `exit`
  * `systemctl start slurmdbd slurmctld`
  * `sacctmgr add cluster hpc`
  * `find /cluster -type d | /root/make_new_users.sh`

