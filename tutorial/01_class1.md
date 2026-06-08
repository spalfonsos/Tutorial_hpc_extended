# Command line
# Alliance resources

When working on Alliance (formerly Compute Canada) systems, it is important to understand the different types of nodes and storage spaces available.

- **Login nodes** are the entry point to the cluster. They are used to edit files, load software modules, compile code, and submit or monitor jobs. Since login nodes are shared among many users, computationally intensive tasks should not be run there.

- **Compute nodes** are where jobs actually execute. Resources such as CPUs, memory, GPUs, and walltime are requested through the Slurm scheduler (e.g., using `sbatch`), and the scheduler assigns the job to one or more compute nodes. (They do not have access to the internet!)

Alliance systems also provide different storage locations:

- **`$HOME`** is your personal storage space. It is typically used for scripts, source code, configuration files, and other important files. Home directories have relatively small quotas.

- **`/project`** is shared storage available to members of a research group. It is commonly used for shared datasets, software installations, and results that collaborators need to access. But also inside your personal folder you could storage the permanent results!

- **`$SCRATCH`** is high-performance storage intended for active computational work. It is suitable for large datasets, intermediate files, and job outputs. Scratch storage is **not backed up** and is **subject to purge policies**, so important results should be copied to `$HOME` or `/project`.


For this class we are going to use the file flights.zip (the shell introduction course (https://staff.sharcnet.ca/tyson/Shell-20231002-IntroARC/html)). 




# Data storage
# Getting around
# Transfering files

