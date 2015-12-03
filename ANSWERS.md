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
  * 

   ```grep -o '<span>.*</span>' ubuntu-releases-web | sed -n 4,23p | cut -d'>' -f2 | cut -d'<' -f1 > ubuntu-releases```
   
     or
   
   ```grep -o '<span>.*</span>'  ubuntu-releases-web | grep -o '^<span>.*</span></span>' | sed 's/<span>//g' | sed 's,</span></span>,,g' > ubuntu-releases```
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
3. create a snapshot lvm lv_lfc01_snapshot of lv_lfc01
  * ```wget https://en.wikipedia.org/wiki/List_of_Ubuntu_releases -O /opt/ubuntu-releases-web```
  * ```sudo pvcreate /dev/sdc1 && sudo vgextend vg_lfc01 /dev/sdc1 && sudo lvcreate -L100M -s -n lv_lfc01_snapshot vg_lfc01/lv_lfc01```
  * ```sudo mkdir /srv/backup/ && sudo mount /dev/vg_lfc01/lv_lfc01_snapshot /srv/backup/```
  * ```sudo umount /srv/backup/ && sudo vgreduce vg_lfc01 /dev/sdc1 && sudo pvremove /dev/sdc1```
4. create an LVM specifying the exact physical extents which should be used
  * ```sudo pvcreate /dev/sdc1 && sudo vgextend vg_lfc01 /dev/sdc1 && sudo lvcreate -n lv_lfc02 -l 511 vg_lfc01```

5. create a striped LVM out of lv_lfc01
  * ```sudo lvremove /dev/vg_lfc01/lv_lfc0[12]```
  * ```sudo pvcreate /dev/sdc1 && sudo vgextend vg_lfc01```
  * ```sudo lvcreate --extents 100%FREE --name lv_lfc01_stripe vg_lfc01 --stripes 2 --stripesize 8```
  * ```sudo lvdisplay -vm```

## Configuring swap partitions
1. create swap partition on /dev/sdd and auto mount on boot
  * create partition with swap id ```sudo fdisk /dev/sdd```, ```sudo mkswap /dev/sdd1```, add fstab entry (get uuid by ```blkid```) ```UUID=ca4a7ff4-ff8d-4384-a10d-cf23a8d9f228 none swap defaults 0 0```, ```sudo swapon -a```
2. list used swap devices
  * ```swapon -s``` or ```cat /proc/swaps```

## File attributes
1. SUID
  * ```sudo adduser john```, ```sudo su - john```, ```echo -e "#!/bin/bash\necho test\n" > johnscript.sh```, ```chmod a+x,u+s johnscript.sh```
2. GUID
  * ```sudo addgroup sales && sudo usermod -aG sales john && sudo adduser jane && sudo usermod -aG sales jane```, ```sudo mkdir /opt/sales && sudo chown root.sales /opt/sales && sudo chmod 3770 /opt/sales```
3. file attributes
  * ```cd /home/vagrant```, ```mkdir attributes && cd attributes```
  * ```echo "content" > immutable && sudo chattr =i immutable```
  * ```echo "first line" > appendonly && sudo chattr =a appendonly```
  * ```echo "first line" > noatimeupdate && sudo chattr =A noatimeupdate```
  * ```lsattr```
  * ```sudo chattr -i immutable && sudo chattr -a appendonly && sudo chattr -A noatimeupdate```

## Finding files on the filesystem
1. find
  * ```sudo find / \( -perm -4000 -fprintf /home/vagrant/setuid.txt '%p \n' \) , \( -size +10M -fprintf /home/vagrant/big.txt '%s %p \n' \)```
  * ```sudo find /etc/ -mtime 0 -printf '%t %p \n'```
  * ```sudo find / -perm 664 -type f```
  * ```sudo find / -perm /664 -type f```
  * ```sudo find / -perm -u+w,g+w -type f```
  * ```which passwd```
  * ```sudo find / -user john```
  * ```sudo find / -user "john" -type f -path "/home/john" -prune```
  * ```sudo find / -iname "log"```
  * ```sudo find / -type c```
  * ```su - john```, ```mkdir find & cd find && touch 'file name'{01,02,03}.txt```, ```find -type f  -name "file name??.txt" -print0 | xargs -0 rm```
2. locate
  * ```touch file01 sudo updatedb && locate file01```

## Partitioning storage devices
1. fdisk
  * ```sudo fdisk -l```
  * ```sudo fdisk /dev/sde```, commands: n, p, enter, enter, 2097153, enter;
  * commands: n, e, enter, enter, enter;
  * commands: n, l, enter, enter, enter; commands: w

## Formatting filesystems
1.mkfs
  * ```sudo fdisk -l /dev/sde```, ```sudo mkfs -t ext3 /dev/sde1```
  * ```sudo mkfs -t ext4 /dev/sde5```

## Mounting filesystems automatically at boot time
1. /etc/fstab
  * ```mkdir /mnt/diske1```, get uuid ```sudo blkid```, add line to /etc/fstab ```UUID=594512ab-6231-471e-97da-e62521478b39 /mnt/diske1 ext3 defaults 0 1```
  * ```mkdir /mnt/diske5```, add line to /etc/fstab ```UUID=1418e78b-ab4c-422a-a1e0-d17ffe6e3e4a /mnt/diske5 ext4 defaults 0 1```

## Mounting networked filesystems
1. create samba share on host lcf01
  * ```mkdir /opt/share/sales && chmod g+w /opt/share/sales && sudo smbpasswd -a john```
  * ```sudo vim /etc/samba/smb.conf```
  add the following to the end of the file


[sales]  
path = /opt/sales/share  
valid users = john  
read only = no  

  * ```sudo service smbd restart```
  * test config ```testparm```

2. mount samba share on host lfc02
  * install necessary packages ```sudo apt-get install cifs-utils && sudo apt-get install smbclient```
  * test connection with ```smbclient -L //10.10.1.10/sales -U john```
  * create mount point ```sudo mkdir /mnt/sales```
  * create credentials file ```su - john```, ```touch /mnt/.smbcredentials && chmod 600 /mnt/.smbcredentials```, add lines

username=john  
password=john

  * automatically mount at boot by adding line to /etc/fstab ```//10.10.1.10/sales /mnt/sales cifs credentials=/mnt/.smbcredentials,defaults 0 0```, test with ```sudo mount -a```

3. mount nfs filesystem
  * TODO

## Troubleshooting filesystem issues

# Local system administration
## Creating backups
## Creating local user groups
* ```sudo addgroup marketing```
* ```sudo vim /etc/group```, ensure 'john' is at end of line ```marketing:x:1003:john```
* ```sudo usermod -aG marketing jane``` or ```sudo gpasswd -a jane marketing```
* ```sudo gpasswd -A jane marketing```
* ```sudo su - jane```, ```gpasswd marketing```
* ```sudo adduser frank```, ```sudo su - jane```, ```gpasswd -a frank```
* ```su - john```, ```newgrp marketing && id -gn```, change back to default group ```newgrp```
* ```su - jane```, ```touch janesfile && chgrp marketing janesfile```
* ```sudo gpasswd -d john marketing```
* ```sudo groupmod -n business marketing```
* ```sudo gpasswd -R business```, note members of group with a password will still be able to newgrp to the group
* ```sudo find /home -group business -delete```, ```sudo groupdel business```

## Managing file permissions
## Managing fstab entries
```sudo mkdir /mnt/{readonly,noatime,users,noauto}```
* /etc/fstab entry: ```UUID=ad31310a-1125-4f96-ac8b-b8f3fb6736e7 /mnt/readonly ext4 ro,suid,dev,exec,auto,nouser,async 0 2```
* /etc/fstab entry: ```UUID=9bd0459c-a9fd-4b0b-84d2-6b83fac5009d /mnt/noatime ext4 defaults,noatime 0 2```
* /etc/fstab entry: ```UUID=b00b3393-37c7-4d07-acb5-52203acc1398 /mnt/users ext4 rw,suid,dev,exec,auto,user,async 0 2```
* /etc/fstab entry: ```UUID=7184b8e8-232a-40a1-998d-3b21e3ee182a /mnt/noauto ext4 rw,suid,dev,exec,noauto,nouser,async 0 2```
* cat /proc/filesystems
* ```sudo blkid``` or ```ll /dev/disk/by-uuid```

## Managing local users accounts
* if frank already exists ```sudo userdel -r frank```, ```sudo useradd -m -d /home/frank frank```
* ```sudo passwd -l john```, unlock ```sudo passwd -u john```
* ```sudo usermod --expiredate 1 john``` sudo usermod --expiredate "" john
* ```sudo passwd -S john```
* ```sudo chage -l john```
* ```sudo chage -E $(date --date="+100 days" +%Y-%m-%d) -M 30 -m 10 -W 10 -I 10 john```
* ```sudo passwd -e jane```, or ```sudo chage -d 0 jane```
* ```sudo useradd -m -d /home/francis -g 1005 francis```
* ```sudo useradd -m -d /home/jessica jessica -s /bin/false```

## Managing user accounts
## Managing user account attributes

## Managing the startup process and related services
* ```sudo service ssh```, or ```status ssh```
* ```sudo service cron stop```, then ```sudo service cron start``` or ```sudo stop cron```, then ```sudo start cron```
* ```echo manual | sudo tee /etc/init/cron.override```
* ```sudo vim /etc/init/testjob.conf```
add
```
description "A test job file"
author "some body"

start on runlevel [2345]

setuid vagrant
setgid vagrant

exec echo Test Job ran at `date` >> /var/log/testjob.log
```

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
