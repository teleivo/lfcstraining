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
3. create a snapshot lvm lv_lfc01_snapshot of lv_lfc01
  * write some data to the lv_lfc01
  * create a snapshot lvm lv_lfc01_snapshot of lv_lfc01
  * mount the snapshot
  * unmount and delete the lvm lv_lfc01_snapshot after the backup finished

4. create an LVM specifying the exact physical extents which should be used

5. create a striped LVM out of lv_lfc01
  * remove lvm lv_lfc01 and lv_lfc02
  * add physical volume /dev/sdc1 to vg_lfc01
  * create a striped LVM with stripes=2 stripe_size=xxx
  * print details of LVMs to see that the logical volume lv_lfc01 is used in striped fashion

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
  * search for files in john's home directory which have been modified in the last twenty-four hours and print modification time followed by file path
  * search for files which have read and write permission for their owner, and group, but which other users can read but not write to
  * search for files which have read and write permission for their owner and group, and which other users can read, without regard to the presence of any extra permission bits
  * search for files which are writable by both their owner and their group
  * find where program passwd is executed from
  * find all files owned by user john
  * find all files owned by user john excluding "/home/john"
  * find all directories containing "log" in their name but ignore case
  * find all character devices
  * as user john create files "file name01.txt" "file name02.txt" "file name03.txt" in '/home/john/find', run find on the directory for type file and remove then by combining find exec and xargs
2. locate
  * create a new file and use locate to find it

## Partitioning storage devices
1. fdisk
  * list all available disks
  * add a primary partition on /dev/sde using half the available sectors
  * add an extended partition on /dev/sde
  * add a logical partition on /dev/sde using the remaining sectors

## Formatting filesystems
1. mkfs
  * make an ext3 on /dev/sde1
  * make an ext4 on /dev/sde5

## Mounting filesystems automatically at boot time
1. /etc/fstab
* mount /dev/sde1 automatically at boot time at /mnt/diske1
* mount /dev/sde5 automatically at boot time at /mnt/diske5

## Mounting networked filesystems
1. create samba share on host lcf01
  * share directory /opt/share/sales and allow write operations, add samba password for user john
2. mount samba share on host lfc02
  * mount /opt/share/sales from host lfc01 at host lfc02 at /mnt/sales
3. mount nfs filesystem

## Troubleshooting filesystem issues
1. tune2fs

# Local system administration
## Creating backups
1. use dd to create an image of a partition

## Creating local user groups
* create group marketing
* add user john to group marketing by manipulating the file containing groups and their members
* add user jane to group marketing using a commandline tool
* make jane an administrator of group marketing
* add a password to group marketing as user jane
* create user frank and add him to group marketing as user jane
* login with john and change his current gid to marketing
* login as jane create a new file and change the files group to marketing
* remove john from group marketing via a commandline tool
* rename the group marketing to business
* disable password for group business
* remove group business and all the files owned by it

## Managing file permissions
* covered by [File attributes](#file-attributes)

## Managing fstab entries
* mount filesystem as read-only
* mount filesystem with defaults but atime should not be updated
* mount filesystem with defaults but users should be able to mount it
* mount filesystem with defaults but it should not automatically be mounted at boot time
* find out which fileystems are currently supported by your kernel
* get the UUID's of your block devices via command line or a file on your system

## Managing local users accounts
* create user frank member of group marketing with default home dir, but do not enable his login
* disable user john from login in (others users might still be able), and re-enable
* disable login with user john entirely, and re-enable
* print password status info on user john
* print password aging info on user john
* set john's account expiration to 100days from today, he should be able to keep his password for up to 30days, he should have to keep his password for a minimum of 10 days, be warned 10 days before password expiration and his account should be de-activated 10 days after password expiration.
* set janes password as expired so she has to change it the next time she logs in
* create user francis but do not create the default group francis, instead primary group should be sales
* create user jessica with shell /bin/false and no home directory

## Managing user accounts
* covered by [Managing local users accounts](#managing-local-users-accounts)

## Managing user account attributes
* covered by [Managing local users accounts](#managing-local-users-accounts)

## Managing the startup process and related services
* get status of ssh
* stop and start cron
* disable cron from starting at boot
* create an upstart configuration for service 'testjob'
  * service should start on runlevels 2345
  * write a message with current timestamp to /var/log/testjob.log as user and group vagrant
* modify puppet (system-v) runlevels
  * prevent puppet from starting on runlevel 5
  * prevent puppet from starting entirely (do not remove the init script itself)
  * ensure puppet runs as before

## Managing user processes
* find all processes of user vagrant
* reduce the priority of a process
* increase the priority of a process
* start a process from the shell with a specific nice value
* start a process from the shell in background, bring it to the foreground, halt it, resume it, kill it
* send a stop signal to a process
* send a kill signal to a process

## Restoring backed up data
## Setting file permissions and ownership
* covered by [File attributes](#file-attributes)

# Local security
## Accessing the root account
## Using sudo to manage access to the root account

# Shell scripting
## Basic bash shell scripting
* write a script
  * exporting environment variables 
  * which exits if called by root with return value 10
  * takes command line arguments
  * uses if
  * uses function
  * uses while
  * uses for loop
  * test if directory exists and if we have write permission, create a file

# Software management
## Installing software packages
