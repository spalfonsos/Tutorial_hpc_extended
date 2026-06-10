# Command line
# Alliance resources

Alliance systems provide different storage spaces designed for different purposes.

- **HOME (`$HOME`)**: Intended for source code, job submission scripts, small parameter files, and other important personal files. Home directories have relatively small quotas and are not well suited for reading and writing large amounts of data.

- **PROJECT (`/project`)**: Shared storage linked to a research group's allocation rather than an individual user account. It is well suited for datasets, software, and files that need to be shared among group members. Data stored in project space should be relatively static.

- **SCRATCH (`$SCRATCH`)**: High-performance storage intended for intensive read/write operations on large files and active computations. Scratch is not backed up, files are subject to purge policies, and it should only be used for transient data such as job outputs, checkpoint files, and data that can be recreated.

- **NEARLINE (`/nearline`)**: Tape-based storage intended for inactive data that are not expected to be accessed for extended periods of time.

Alliance clusters also distinguish between **login nodes** and **compute nodes**. Login nodes are used to prepare and submit jobs, while compute nodes are where the jobs actually execute.

<img width="1431" height="803" alt="storage_by_folders" src="https://github.com/user-attachments/assets/ca44797a-140c-4d50-9d28-1ff3c9af5c4e" />



For this class we are going to use the file flights.zip ( extract of the data set in the shell introduction course (https://staff.sharcnet.ca/tyson/Shell-20231002-IntroARC/html)). 

It is important to know that the computation nodes do not have access to internet so in case we require download files from internet we need to do it from a login node. So, for our first class lets download the data_1 from the following link
https://drive.google.com/file/d/1Kl3zyxhw2Yzupm8Zg4egxP3D6eMmfq6m/view?usp=drive_link
# Data storage

#### 1. Let's make a directory in the path where we are going to store the files we are going to use in the class1
- In the home directory go to the directory projects/def-cbravo/salfonso (you also can store it in the home/user folder)
- cd projects/def-cbravo/salfonso
- mkdir tutorial_hpc_bcp
- cd tutorial_hpc_bcp
- mkdir class1
- Then change directory to class1 and there download the data 
- wget -O data_1.zip "https://drive.google.com/uc?export=download&id=1Kl3zyxhw2Yzupm8Zg4egxP3D6eMmfq6m"
- then unzip the data_1.zip 
- unzip data_1.zip 

# Getting around
- We can see the structure of the folder with
- tree
- to see the list of documents
- ls
- chmod u+w
- ls -l (give the liss of documment with permissions)
```text
d rwx r-s r-x
│ │   │   │
│ │   │   └── others
│ │   └────── group
│ └────────── owner
└──────────── directory
```

 Permission | Regular file | Directory |
|---|---|---|
| `r` (read) | Read the contents of the file (`cat`, `less`, `head`) | List the names of entries in the directory (`ls`) |
| `w` (write) | Modify or overwrite the file (`nano`, `vim`, `echo >>`) | Create, delete, or rename entries in the directory (`touch`, `rm`, `mv`) |
| `x` (execute) | Run the file as a program or script (`./script.sh`) | Traverse/search the directory (`cd`, access files by name) |

- pwd (current path)
- The command line is very different from Graphical User Interface (GUI). It helps to make automatization (e.g., changing the name of a lot of files)
- if you mistakenly write sth you could cancel with ctrl+C (here it is different from copy :S)
- And also we have autocomplete option with the tap key.
- cd .. takes us again to the home directory
- cd ~ makes the same as before
- with man command we see the definition of the commands

## Copy, viewing and editing files

- man cp
- mkdir examples
- cd data_1
- [salfonso@narval1 class1]$ cp -T data_1/0144f5b1.igc examples/0144f5b1.igc
- [salfonso@narval1 class1]$ cp data_1/0d1edd25.igc data_1/48da3f0c.igc examples
- [salfonso@narval1 class1]$ cp -t examples data_1/9f615499.igc data_1/99f1f386.igc
- [salfonso@narval1 class1]$ cd examples
- [salfonso@narval1 examples]$ cat 0144f5b1.igc
- [salfonso@narval1 examples]$ head 0144f5b1.igc
- For editing the file
- [salfonso@narval1 examples]$ nano 0144f5b1.igc
- Rename a file
- [salfonso@narval1 examples]$ mv 0144f5b1.igc ourfile1.igc
- remove the file
- [salfonso@narval1 examples]$ rm ourfile1.igc

# Transfering files





