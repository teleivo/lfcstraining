This shows possible ways to achieve the tasks described in [README](README.md).

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
  * ```cut -d':' -f1,7 /etc/passwd > users-shells```
  * ```cut -d':' -f7 /etc/passwd | uniq > shells```
  * ```cut -d':' -f7 /etc/passwd | uniq -c | sort -n -r > shells```
2. ubuntu releases
  * ```wget https://en.wikipedia.org/wiki/List_of_Ubuntu_releases -O ubuntu-releases-web```
  * ```grep -o '<span>.*</span>' ubuntu-releases-web | sed -n 4,23p | cut -d'>' -f2 | cut -d'<' -f1 > ubuntu-releases```
  * ```wc -l ubuntu-releases```

# Filesystem and storage
## Archiving and compressing files and directories
1. backup '/etc'
  * ```sudo cp -a /etc ~/etc_backup```
  * ```sudo tar czvf etc_backup.tar.gz etc_backup```
  * ```sudo tar tf etc_backup.tar.gz```
  * ```sudo tar cjvf etc_backup.tar.bz2 etc_backup```
  * ```sudo tar cvf etc_backup.tar etc_backup && sudo sed -i 's,/home/vagrant,/home/vagrant/new,' etc_backup/passwd && sudo tar uf etc_backup.tar etc_backup && mkdir ~/tmp && sudo tar xf etc_backup.tar etc_backup/passwd```
  * ```sudo tar -v --delete --file=etc_backup.tar etc_backup/passwd```
  * ```sudo tar cf etc_backup.tar etc --exclude=etc_backup/passwd```

## Assembling partitions as LVM devices
1. create an LVM on host lfc01
  * ```sudo apt-get install lvm2```
  * create partition on disk sdb ```sudo fdisk /dev/sdb```, ```sudo vgcreate vg_test /dev/sdb```
  * ```sudo vgrename vg_test vg_lfc01```
  * ```sudo lvcreate -l 100%FREE -n lv_lfc01 vg_lfc01```
  * ```sudo mkfs -t ext4 /dev/vg_lfc01/lv_lfc01```, ```sudo blkid```, ```sudo vim /etc/fstab``` add line like ```UUID=lbQfMo-Qdz6-5hNf-NacD-Ejsr-yd1j-CG1oQh /opt ext4 defaults 0 1```, ```sudo mount -a```
  * ```sudo pvcreate /dev/sdc1 && sudo vgextend vg_lfc01 /dev/sdc1 && sudo lvextend -l 100%VG /dev/vg_lfc01/lv_lfc01 && sudo resize2fs /dev/mapper/vg_lfc01-lv_lfc01```
2. shrink lvm lv_lfc01
  * get current block size ```sudo tune2fs -l /dev/mapper/vg_lfc01-lv_lfc01 | grep -i 'block size'```, shrink filesystem to half of the block size since both pv's are of same size ```sudo umount /opt && sudo resize2fs  /dev/mapper/vg_lfc01-lv_lfc01 523264s && sudo lvreduce -l 511 /dev/vg_lfc01/lv_lfc01 && sudo vgreduce /dev/vg_lfc01 /dev/sdc1 && sudo pvremove /dev/sdc1```

## Configuring swap partitions
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
