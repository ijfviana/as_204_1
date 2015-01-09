## Simular rotura de un disco (I)

<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
<a class="fancybox" href="img/raid1_failure.png" data-fancybox-group="gallery" title="Introducción">
<img height="450px" src="img/raid1_failure.png" alt="Introducción">
</a>
    </div>
</div>

note:



## Simular rotura de un disco (II)

* Comprobamos que el RAID funciona correctamente

```bash
/tmp$ cat /proc/mdstat
Personalities : [raid0] [raid1] 
md1 : active raid1 sdb2[0] sdc2[1]
      56128 blocks super 1.2 [2/2] [UU]

/tmp$ mdadm --detail /dev/md1
/dev/md1:
        Version : 1.2
  Creation Time : Sat Nov 16 22:19:20 2013
     Raid Level : raid1
     Array Size : 56128 (54.82 MiB 57.48 MB)
  Used Dev Size : 56128 (54.82 MiB 57.48 MB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Sat Nov 16 23:31:30 2013
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



## Simular rotura de un disco (III)

* Creamos un fichero en el RAID 
``` bash
/tmp$ mount /dev/md1
/tmp$ echo "Hola Mundo" >> /mnt/test.txt
```



## Simular rotura de un disco  (IV)

* Simulamos fallo de uno de los dispositivos del RAID usando `mdadm --failure`

``` bash
/tmp$ mdadm --fail /dev/md1 /dev/sdb2
md/raid1/md1: Disk failure on /dev/sdb2, disabling device.
md/raid1/md1: Operation continuing on 1 devices.
mdadm: set /dev/sdb2 faulty in /dev/md0
```



## Simular rotura de un disco  (V)

* Al mostrar el estado del RAID: solo hay un dispositivo activo de los dos , el estado del RAID está degradado.
 
```bash
/tmp$ cat /proc/mdstat 
Personalities : [raid0] [raid1] 
md1 : active (auto-read-only) raid1 sdc2[1]
      56128 blocks super 1.2 [2/1] [_U]
      
/tmp$ mdadm --detail /dev/md1
/dev/md1:
        Version : 1.2
  Creation Time : Sat Nov 16 22:19:20 2013
     Raid Level : raid1
     Array Size : 56128 (54.82 MiB 57.48 MB)
  Used Dev Size : 56128 (54.82 MiB 57.48 MB)
   Raid Devices : 2
  Total Devices : 1
    Persistence : Superblock is persistent

    Update Time : Sat Nov 16 23:45:07 2013
          State : clean, degraded 
 Active Devices : 1
Working Devices : 1
 Failed Devices : 0
  Spare Devices : 0

           Name : localhost.localdomain:1  (local to host localhost.localdomain)
           UUID : 21b7c369:db704f85:9585806a:c5de15c7
         Events : 17

    Number   Major   Minor   RaidDevice State
       0       0        0        0      removed
       1       8       34        1      active sync   /dev/sdc2
       0       8       18        -      faulty spare  /dev/sdb2
``` 



## Simular rotura de un disco  (VI)

* Seguimos usando el raid, creamos un nuevo fichero con datos
``` bash
/tmp$ mount /dev/md0
/tmp$ dd if=/dev/zero of=sal bs=40M
```
* Si añadiéramos un nuevo dispositivo al raid, este fichero debería copiarse en este nuevo dispositivo.



## Restauración y recuperación (I)

* Cuando uno de los dispositivos de un RAID deja de funcionar hay que reemplazarlo

<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
<a class="fancybox" href="img/raid_nuevo_dispositivo.png" data-fancybox-group="gallery" title="Introducción">
<img height="450px" src="img/raid_nuevo_dispositivo.png" alt="Introducción">
</a>
    </div>
</div>




## Restauración y recuperación (II)

* Creamos una partición adecuada en `/dev/sdd`

```bash
/tmp$ fdisk /dev/sdd
fdisk /dev/sdd

WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
         switch off the mode (command 'c') and change display units to
         sectors (command 'u').

Orden (m para obtener ayuda): n
Acción de la orden
e   Partición extendida
   p   Partición primaria (1-4)
p
Número de partición (1-4): 1
Primer cilindro (1-16, valor predeterminado 1): 1
Last cilindro, +cilindros or +size{K,M,G} (1-16, valor predeterminado 16): +64M

Orden (m para obtener ayuda): t
Se ha seleccionado la partición 1
Código hexadecimal (escriba L para ver los códigos): fd
Se ha cambiado el tipo de sistema de la partición 1 por fd (Linux raid autodetect)

Orden (m para obtener ayuda): w
¡Se ha modificado la tabla de particiones!

Llamando a ioctl() para volver a leer la tabla de particiones.
Se están sincronizando los discos.
```



## Restauración y recuperación (III)

* Añadimos la nueva partición, inicialmente se activa como *hot spare*, al RAID que volverá a regenerarse 

``` bash
/tmp$ mdadm −−add /dev/md1 /dev/sdd1
mdadm: hot added /dev/sdd1

/tmp$ cat /proc/mdstat
Personalities : [raid1]
md0 : active raid1 sdd1[2] sdc1[0]
127872 blocks [2/1] [U_]
[======>...

/tmp$ mdadm --detail /dev/md1
/dev/md1:
        Version : 1.2
  Creation Time : Sat Nov 16 22:19:20 2013
     Raid Level : raid1
     Array Size : 56128 (54.82 MiB 57.48 MB)
  Used Dev Size : 56128 (54.82 MiB 57.48 MB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Sun Nov 17 18:04:24 2013
          State : clean, degraded, recovering 
 Active Devices : 1
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 1

 Rebuild Status : 40% complete

           Name : localhost.localdomain:1  (local to host localhost.localdomain)
           UUID : 21b7c369:db704f85:9585806a:c5de15c7
         Events : 76

    Number   Major   Minor   RaidDevice State
       2       8       49        0      spare rebuilding   /dev/sdd1
       1       8       34        1      active sync   /dev/sdc2
       0       8       18        -      faulty spare  /dev/sdb2
```



## Restauración y recuperación (III)

* El dispositivo dañado `/dev/sdb2` lo eliminados del RAID 

``` bash
/tmp$ mdadm --remove /dev/md1 /dev/sdb2
```
