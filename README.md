#### psexamrevlx0103
#####hardware setting
```
lspci
lsusb -v
lsblk
```
folders:
```
/proc
/sys
```
######using virtual file
```
man procfs
```
```
ls -l /dev/cdrom
```
######working with USB
detect
```
dmesg   (/dev/kmsg)
```
clear buffer
```
dmesg -C
```
######kernel module
list modules
```
lsmod |grep sr_mod  (cdrom)  =/proc/modules
modprobe -r sr_mod  //remove module = rmmod
modprobe -rv sr_mod    //verbose
modprobe sr_mod  //load a module  = insmod
modinfo sr_mod
```
block hardware:
in
```
vim /etc/modprobe.d
```
edit
```
blacklist sr_mod
```

######load unloading module
```
lsmod | grep sr_mod
```
eject
```
eject /dev/cdrom
```
```
vi  /etc/modprobe.d/modesetting.conf
```
edit
```
options sr_mod xa_test=1
```

#####boot the system
######reading kernel
```
cat /proc/cmdline
```
######understand sysvinit
```
sysvinit
/etc/inittab
```
######using sysvinit
```
/etc/sshd status
/etc/rc3.d   //level3 process
```
######using systemd
```
systemd-analyze
cat /lib/systemd/system/ssh.service
```
######use dmesg
```
dmesg -H  //human-readable
```


#####change runlevels
######gaining control
0=halt 6=reboot  
2=ubuntu 3,5=redhat suse  
in centos
```
grep initdefault /etc/inittab
```
```
chkconfig atd on/off
```

######systemd
for example, target in systemd 
(poweroff.target)= runlevel with sysvinit(0)  
rescue.target=1  
multiuser.target=2,3,4   
graphal.target=5   
reboot.target=6  
```
systemctl get-default
systemctl set-default multi-user.target
```
######change runlevels
```
who -r
(centos) systemctl isolate multiuser.target
(debian) talinit 3
```
######rebooting
shutdown, reboot,poweroff
```
shutdown -h now //halt
shutdown -h +10  //10min
shutdown -h +10 "mess"
shutdown -c  //acncel
```
using wall,send to all user
```
echo "x" | wall
```
#####design hard disk layout
######why partition
/boot: the kernel must be accessible to bootloader  
SWAP virtual memory is normally is partition
command reivew
```
pvcreate /dev/xvda
vgcreate vg /dev/xvda
lvextend -L +100m /dev/vg/name
resize2fs /dev/vg/name
```
retrieve swap
```
swapon -s
free -m
cat /proc/meminfo
```
#####install boot manager
######
```
sudo cat /boot/grub/menu.lst   (legacy, =./grub.conf)
/etc/grub.conf-> /boot/grub/grub.conf
```

grub legacy
```
grub-install /dev/sda
grub > setup(hd0)
grub > root(hd0,0)
```
######grub2 on debian8
```
/boot/grub/grub.cfg
```
however, changes will be performed on 
```
/etc/default/grub
/etc/grub.d
```
if you change either of above file,
```
grub-mkconfig -o /boot/grub/grub.cfg
```
######resetting password for root
```
```

######process text streams using filters


#####perform basic file
######file copies delections
```
cp -u old new   //only copy when newer
cp -i iteractive mode
```
######summary
copy output
```
find /var/log -type f -mtime +1 |cpio -o >log.cpio
```


#####use streams pipes
######howto redirection
```
strace ls -l
```

######pipelines, arguments
standard input
```
apio -id --absolute-filenames < ../filename
while read H; do ping -c3 $H; done<x.txt
```









#####create monitor
######managing process
```
sleep 3  (ctrl z to bg)
fg 1 
bg  (return)
jobs
```

######using screen
```
screen sleep 100
ctrl+a d   //exit
screen -ls
screen -r id      //(retach)
scrren -S easyname
```
######using ps
```
ps aux  (BSD style without dash)
ps -l 
ps -f
```
```
sleep 1000&
pgrep sleep
pkill sleep
killall -u bob
```










#####modify process execution priorities
######using nice
```
nice -n 5 sleep 1005
```
######renice
```
renice -n 10 -p 1234
```

#####create partitions and file systems
######format
portable drive:

```
fdisk /dev/sdc
```

```
mkfs -t vfat /dev/sdd1    = mkdosfs
```
```
apt-get install xfsprogs
```
```
mke2fs -t ext4 /dev/sdc1    //note: not /dev/sdc/sdc1
```
```
mkfs.xfs /dev/sdc2
```
```
mkswap /dev/sdc3
```



#####maintain integrity file system
######using fsck
```
fsck
e2fsck
xfs_check
xfs_repair
```
automated checks
```
tune2fs -i 0 -c 0
tune2fs -i /dev/xda1
```

```
fsck /dev/sda1
fsck -A
fsck -AR   //exculding root
fsck -ARM  //excliding mounted file systems
```
```
e2fsck -f /dev/sdc1
```
```
xfs_repair -n /dev/sda2  //not check
xfs_repair /dev/sda2   //check
btrfsck /dev/sdb
```

######file system metadata
```
dumpe2fs
xfs_info
xfs_metadump
```
futher exploer
```
debugfs
```
show size of dic
```
du -sh /home   //-s summary
```

#####control mounting and unmounting

```
/etc/mtab      //current mount
/proc/mounts
```

#####manage file permission
######default mask
umask 0 ->rw-rw-rw-
umask 077 -> rw-------
#####create link
######hard link
ex:  
drwxr-xr-x 4 ubuntu ubuntu 4096 Apr  3 20:07 .  
4 means number of hard links.  

######setting permission
login new group

```
newgrp kename
```
then
```
touch c
```
we can see  
-rw-r--r-- 1 root newgrp 0 Apr  4 01:46 c  

######dict permisson
```
stat -c %A name //show symbolic permission
stat -c %a name //octal permission
stat -c %G gidname
stat -c %g gid
```
######special permission
```
mkdir -m 770 /dir
```
!$means last command's argument.  

######symblic link
find link type
```
find -xtype l
```

#####findsystem
```
find -name "x" -o -type f           //-o  or
find -iname         //case insensitive
find -type f NOT -name ...     //not
locale -i wd
```
