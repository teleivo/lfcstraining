# lfcstraining

This is a VM setup to play around with for the lfcs sysadmin exam taken on Ubuntu in 2015. Below you'll find tasks for each of the domains and competencies. Check [ANSWERS](ANSWERS.md) for possible solutions to the given tasks.

This is not an official guide or training program!!
Please refer to https://training.linuxfoundation.org/certification/lfcs for an up to date list of the domains and competencies as these might change!!!


####Table of Contents

1. [Command line](#command-line)
2. [Filesystem and storage](#filesystem-and-storage)
3. [Local system administration](#local-system-administration)
4. [Local security](#local-security)
5. [Shell scripting](#shell-scripting)
6. [Software management](#software-management)

# Command line
## Editing text files on the command line
## Manipulating text files from the command line
1. user accounts
  * extract all user accounts and their default shells into file 'users-shells'
  * extract a list of unique shells into file 'shells'
  * get a total of users per shell from 'users-shells', sort from most to least assigned shell and override file 'shells' with the output
2. ubuntu releases
  * download page https://en.wikipedia.org/wiki/List_of_Ubuntu_releases into file 'ubuntu-releases-web'
  * extract all past & current releases from the table of content into file 'ubuntu-releases'
  * get a total of all releases

# Filesystem and storage
## Archiving and compressing files and directories
1. backup '/etc'
  * create a copy of '/etc' in '~/etc_backup' while preserving mode, ownership, timestamps and symbolic links 
  * create a gziped tarball '~/etc_backup.tar.gz'
  * list files in '~/etc_backup.tar.gz'
  * create a bzip2 tarball '~/etc_backup.tar.bz2'
  * create '~/etc_backup.tar', change vagrant users home directory in '~/etc_backup/passwd' and update '~/etc_backup.tar', extract 'passwd' from '~/etc_backup.tar' to 'tmp/etc/passwd'
  * delete '~/etc_backup/passwd' from '~/etc_backup.tar'
  * recreate '~/etc_backup.tar' and exclude '~/etc_backup/passwd' from it

## Assembling partitions as LVM devices
1. create an LVM on host lfc01
  * ensure packages for LVM are installed
  * create volume group vg_test containing physical volume /dev/sdb1
  * rename the volume group vg_test to vg_lfc01
  * create an lvm lv_lfc01 on vg_lfc01 taking its entire space
  * create an ext4 filesystem on lv_lfc01, mount it permanently using its UUID
  * expand disk space on lv_lfc01 by adding /dev/sdc1 to vg_lfc01
2. shrink lvm lv_lfc01
  * remove pv /dev/sdc1 from vg_lfc01
3. create a snapshot of lv_lfc01 to create a backup while the original volume can still be written to
4. create an LVM specifying the exact physical extents which should be used
5. create a striped/linear LVM

## Configuring swap partitions
1. create swap partition on /dev/sdd and auto mount on boot
2. list used swap devices

## File attributes
1. SUID
  * create an executable which when executed runs with permissions of user john no matter who executes it
2. GUID, sticky bit
  * create a directory which is owned by user root and group sales of which user john and jane are members. only root and members of the sales group should have read,write,execute access to this directory. files created in this directory should be readable, writable by the members of the sales group no matter who created the file. files in this directory should only be deletable by its owner or root.
3. file attributes
  * create directory 'attributes' in home directory /home/vagrant of user vagrant
  * create an immutable file 'immutable'
  * create file 'appendonly' which can only be appended to
  * create file 'noatimeupdate' of which atime will not be updated
  * list the file attributes of above files
  * remove the file attributes of above files

## Finding files on the filesystem
1. find
  * traverse the filesystem just once, listing setuid files and directories into /home/vagrant/suid.txt and files bigger than 10MB into /home/vagrant/big.txt
  * search for files in john's home directory which have been modified in the last twenty-four hours
  * search for files which have read and write permission for their owner, and group, but which other users can read but not write to
  * search for files which have read and write permission for their owner and group, and which other users can read, without regard to the presence of any extra permission bits
  * search for files which are writable by both their owner and their group
  * find where program passwd is executed from
  * find all files owned by user john
  * find all files owned by user john excluding "/home/john"
  * find all directories containing "log" in their name but ignore case
  * find all character devices
2. locate
  * create a new file and use locate to find it

## Formatting filesystems
## Mounting filesystems automatically at boot time
## Mounting networked filesystems
## Partitioning storage devices
## Troubleshooting filesystem issues

# Local system administration
## Creating backups
## Creating local user groups
## Managing file permissions
## Managing fstab entries
## Managing local users accounts
## Managing the startup process and related services
## Managing user accounts
## Managing user account attributes
## Managing user processes
## Restoring backed up data
## Setting file permissions and ownership

# Local security
## Accessing the root account
## Using sudo to manage access to the root account

# Shell scripting
## Basic bash shell scripting

# Software management
## Installing software packages
