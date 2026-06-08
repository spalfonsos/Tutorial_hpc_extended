# Command line
# Alliance resources

Alliance systems provide different storage spaces designed for different purposes.

- **HOME (`$HOME`)**: Intended for source code, job submission scripts, small parameter files, and other important personal files. Home directories have relatively small quotas and are not well suited for reading and writing large amounts of data.

- **PROJECT (`/project`)**: Shared storage linked to a research group's allocation rather than an individual user account. It is well suited for datasets, software, and files that need to be shared among group members. Data stored in project space should be relatively static.

- **SCRATCH (`$SCRATCH`)**: High-performance storage intended for intensive read/write operations on large files and active computations. Scratch is not backed up, files are subject to purge policies, and it should only be used for transient data such as job outputs, checkpoint files, and data that can be recreated.

- **NEARLINE (`/nearline`)**: Tape-based storage intended for inactive data that are not expected to be accessed for extended periods of time.

Alliance clusters also distinguish between **login nodes** and **compute nodes**. Login nodes are used to prepare and submit jobs, while compute nodes are where the jobs actually execute.


For this class we are going to use the file flights.zip (the shell introduction course (https://staff.sharcnet.ca/tyson/Shell-20231002-IntroARC/html)). 




# Data storage
# Getting around
# Transfering files

