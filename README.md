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

## File attributes
## Finding files on the filesystem
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
