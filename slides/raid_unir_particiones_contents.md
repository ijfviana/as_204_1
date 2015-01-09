## Crear RAID 0 (I)

<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
<a class="fancybox" href="img/raid0_create.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid0_create.png" alt="Introducción">
</a>
    </div>
</div>

note:



## Crear RAID 0 (II)

* Usamos el comando [`mdadm`](http://linux.die.net/man/8/mdadm)
``` bash
mdadm [mode] <raiddevice> [options] <component-devices>
```
* Ejecutamos `mdadm --create`
```bash
/tmp$ mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```



## Crear RAID 0 (III)

* Comprobamos estado consultando `/proc/mdstat` o ejecutando `mdadm --query`

```bash
/tmp$ cat /proc/mdstat
Personalities : [raid0] 
md0 : active raid0 sdc1[1] sdb1[0]
      144384 blocks super 1.2 512k chunks
      
unused devices: <none>

/tmp$ mdadm --query /dev/md0
/dev/md0: 141.00MiB raid0 2 devices, 0 spares. Use mdadm --detail for more detail.
```



## Crear RAID 0 (IV)

* Comprobamos detalles RAID ejecutabdo  `mdadm --detail` 

``` bash
/tmp$ mdadm --detail /dev/md0
/dev/md0:
        Version : 1.2
  Creation Time : Sat Nov 16 22:09:48 2013
     Raid Level : raid0
     Array Size : 144384 (141.02 MiB 147.85 MB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Sat Nov 16 22:09:48 2013
          State : clean 
 Active Devices : 2
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 0

     Chunk Size : 512K

           Name : localhost.localdomain:0  (local to host localhost.localdomain)
           UUID : 9c20da48:09f729c2:710de065:73c8bbca
         Events : 0

    Number   Major   Minor   RaidDevice State
       0       8       17        0      active sync   /dev/sdb1
       1       8       33        1      active sync   /dev/sdc1
```



## Crear RAID 1 (I)

<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
<a class="fancybox" href="img/raid1_create.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid1_create.png" alt="Introducción">
</a>
    </div>
</div>

note:



## Crear RAID 1 (II)

* Creamos el RAID ejecutando  `mdadm --create`:
``` bash
/tmp$ mdadm --create /dev/md1 --level=1 --raid-devices=2 /dev/sdb2 /dev/sdc2
mdadm: Note: this array has metadata at the start and
may not be suitable as a boot device.  If you plan to
store '/boot' on this device please ensure that
your boot-loader understands md/v1.x metadata, or use
--metadata=0.90
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
```

note:
*



## Crear RAID 1 (III)

* Consultados estado mediante `/proc/mdstat` o `mdadm --query`

```bash
/tmp$ cat /proc/mdstat
Personalities : [raid0] [raid1] 
md1 : active raid1 sdc2[1] sdb2[0]
      56128 blocks super 1.2 [2/2] [UU]
      
md0 : active raid0 sdc1[1] sdb1[0]
      144384 blocks super 1.2 512k chunks
      

/tmp$ mdadm --query /dev/md1
/dev/md1: 54.81MiB raid1 2 devices, 0 spares. 
```



## Crear RAID 1 (IV)

* Consultamos estado detallado usando `mdm --detail`
 
```bash
/tmp$ mdadm --detail /dev/md0
/dev/md1:
        Version : 1.2
  Creation Time : Sat Nov 16 22:19:20 2013
     Raid Level : raid1
     Array Size : 56128 (54.82 MiB 57.48 MB)
  Used Dev Size : 56128 (54.82 MiB 57.48 MB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Sat Nov 16 22:19:22 2013
          State : clean 
 Active Devices : 2
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 0

           Name : localhost.localdomain:1  (local to host localhost.localdomain)
           UUID : 21b7c369:db704f85:9585806a:c5de15c7
         Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       18        0      active sync   /dev/sdb2
       1       8       34        1      active sync   /dev/sdc2
```

note:



## Ensamblado automático (I)

* Linux no recuerda automáticamente:
 * El nombre del dispositivo dado al RAID
 * Los componentes que lo forman
* Esta información la tenemos que almacenar en `/etc/mdadm.conf`
* Por defecto este fichero no está creado.

note:
* Linux system doesn’t automatically remember all the components that are part of RAID set. 
* This information has to be add it on mdadm.conf file under /etc directory. 
* It help to start ,rebuild,re-activate the raid etc.., by default, file will not be available, it has to be created manually.




## Ensamblado automático (II)

* Podemos crear este fichero usando el comando  `mdadm --detail --scan`

``` bash
/tmp$ echo "DEVICE /dev/sdb1 /dev/sdb2 /dev/sdc1 /dev/sdc2" > /etc/mdadm.conf
/tmp$ mdadm --detail --scan | tee −a /etc/mdadm.conf
ARRAY /dev/md0 metadata=1.2 name=localhost.localdomain:0 UUID=9c20da48:09f729c2:710de065:73c8bbca
ARRAY /dev/md1 metadata=1.2 name=localhost.localdomain:1 UUID=21b7c369:db704f85:9585806a:c5de15c7

/tmp$ cat /etc/mdadm.conf
DEVICE /dev/sdb1 /dev/sdb2 /dev/sdc1 /dev/sdc2
ARRAY /dev/md0 metadata=1.2 name=localhost.localdomain:0 UUID=9c20da48:09f729c2:710de065:73c8bbca
ARRAY /dev/md1 metadata=1.2 name=localhost.localdomain:1 UUID=21b7c369:db704f85:9585806a:c5de15c7
```

note:



## Ensamblado automático (III)

* Si no existe `/etc/mdadm.conf` se intentará localizar los RAID creados y ensamblarlos automáticamente
* Si el proceso falla,  podemos "ensamblar" un RAID ya creado ejecutando `mdadm --assemble`

``` 
/tmp$ mdadm --assemble /dev/md0 /dev/sdb1 /devsdb2
mdadm: /dev/md0 has been started with 2 devices
```

note:
