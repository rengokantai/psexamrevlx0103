# psexamrevlx0103
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
for example, target in systemd (poweroff.target)= runlevel with sysvinit(0)
