## Creación de sistemas de ficheros

Una vez que tenemos creados los dispositivos multidisco, creamos un SF en cada uno de ellos

``` bash 
/tmp$ mkfs.ext3 /dev/md0
mke2fs 1.35 (28−Feb−2004)
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
32000 inodes, 127872 blocks
6393 blocks (5.00%) reserved for the super user
First data block=1
Maximum filesystem blocks=67371008
16 block groups
8192 blocks per group, 8192 fragments per group
2000 inodes per group
Superblock backups stored on blocks:
8193, 24577, 40961, 57345, 73729
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
This filesystem will be automatically checked every 25 mounts or
180 days, whichever comes first. Use tune2fs −c or −i to override.
```



## Montaje 

Después de crear el sistema de ficheros, procedemos a montar el dispositivo

```bash
/tmp$ mount /dev/md0 /mnt/
/tmp$ mount
/dev/hda2 on / type ext3 (rw)
none on /proc type proc (rw)
none on /sys type sysfs (rw)
none on /dev/pts type devpts (rw,gid=5,mode=620)
usbfs on /proc/bus/usb type usbfs (rw)
/dev/hda1 on /boot type ext3 (rw)
none on /dev/shm type tmpfs (rw)
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw)
/dev/md0 on /mnt type ext3 (rw)
```
