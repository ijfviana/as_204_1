## ¿Qué son las particiones?

<a class="fancybox" href="img/partitions_intro.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/partitions_intro.png" alt="Introducción">
</a>

note:
* Hard disks are typically broken into segments, known as partitions.
* They are composed of contiguous sets of sectors. Thus, if you want to change the way partitions are laid out, you may need to move all the data on one or more partitions
* In Linux, most partitions (or, to be more precise, the filesystems they contain) are mounted at specific directories. 
* Swap partitions are an exception to this rule; they are accessed as an adjunct to system memory. 
* The next few slides describe this topic, including both the important principles and partition types and the basic operation of the tools used to create partitions.



## ¿Qué es la tabla de particiones? (I)

* Estructuras que describen las características de las particiones (tipo, tamaño...)
* Las más conocidas:
 * Master Boot Record (MBR)
 * Apple Partition Map (APM)
 * GUID Partition Table (GPT)

note:
* Partitions are described in a data structure that is known generically as a partition table. 
* The partition table is stored in one or more sectors of a hard disk, in locations that are defined by the partition table type. 
* Over the years, several different partition table types have been developed:
 * Master Boot Record (MBR)
 * Apple Partition Map (APM)
 * GUID Partition Table (GPT)



## ¿Qué es la tabla de particiones? (II)
### Master Boot Record (I)

<div class="row">
    <div class="col-xs-5 col-sm-5 col-md-5 col-lg-5 col-centered">
<a class="fancybox" href="img/partition_table_mbr.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/partition_table_mbr.png" alt="Introducción">
</a>
    </div>
</div>

note:
* It is the most common one on disks under 2 TiB in size. 
* It was used by Microsoft’s Disk Operating System (DOS) and has been adopted by most OSs that run on the same hardware as DOS and its successor, Windows. 
* MBR is known by various other names, including MS-DOS partitions and BIOS partitions.
* Unfortunately, MBR suffers from many limitations, as described shortly, and so is being slowly abandoned. 



## ¿Qué es la tabla de particiones? (III)
### Master Boot Record (II)

* Usa apuntadores de 32 bits
* Restricciones tipo y número de particiones
<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
<a class="fancybox" href="img/particiones_primaria_logica_extendida.png" data-fancybox-group="gallery" title="Introducción">
<img height="250px" src="img/particiones_primaria_logica_extendida.png" alt="Introducción">
</a>
    </div>
</div>
* Muchos SO necesitan arrancar desde una primaria

note:
* It uses 32-bit pointers to refer to disk sectors.
* The original MBR specification provided for just four partitions. 
* When this limit became troublesome, a workaround was devised: One of the original four partitions (now known as primary partitions) was allocated as a placeholder (an extended partition) for an arbitrary number of additional partitions (logical partitions
* Limitations
 * All logical partitions must reside within a single extended partition.
 * Furthermore, some OSs, such as Microsoft Windows, must boot from a primary partition. (Linux is not so limited.)
* In Linux, primary partitions are numbered from 1 to 4, while logical partitions are numbered 5 and up.



## ¿Qué es la tabla de particiones? (IV)
### Apple Partition Map (APM)

* Usada en sistemas 680x0 y PowerPC
* Usa apuntadores de 32 bits

note:
* Apple used this partition table type on its 680x0- and PowerPC-based Macintoshes, and it’s been adopted by a few other computer types. 
* Because Mac OS has never dominated the marketplace,
* APM is uncommon except on older Mac hardware; however, you may occasionally run into a removable disk that uses APM.
* It uses 32-bit pointers to refer to disk sectors



## ¿Qué son las particiones? (VI)
### GUID Partition Table (GPT) (I)

<a class="fancybox" href="img/partition_table_gpt.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/partition_table_gpt.png" alt="Introducción">
</a>

note:
* It is described in the Extensible Firmware Interface (EFI) definition, but it can be used on non-EFI systems. 
* Apple switched to GPT for its Macintosh computers when it adopted Intel CPUs.
* GPT overcomes many of the problems of the older MBR and APM partition tables, particularly their disk size limits, 
* GPT seems likely to become increasingly important as disk sizes rise.



## ¿Qué son las particiones? (VII)
### GUID Partition Table (GPT) (II)

* Usa apuntadores de 64 bits
* Permite hasta 128 particiones
* Permite asociar nombres a las particiones

<a class="fancybox" href="img/particiones_uefi.png" data-fancybox-group="gallery" title="Introducción">
<img height="350px" src="img/particiones_uefi.png" alt="Introducción">
</a>

note:
* GPT uses 64-bit sector pointers, 
* GPT supports a fixed number of partitions (128 by default), all of which are defined in the main partition table. 
* GPT and MBR support slightly different meta-data—for instance, GPT supports a partition name, which MBR doesn’t support.



## Creando paticiones (I)

* [libparted tools](#/4/8)
* [fdisk Family](#/4/9)
* [GPT fdisk](#/4/13)



## Creando paticiones (II)
### Libparted Tools

* [The GNU Project’s libparted](http://www.gnu.org/software/parted/)
* Aplicaciones que hacen uso de esta librería 
 * Aplicación de linea de comandos [`parted`](http://linux.die.net/man/8/parted)
 * Aplicación gráfica [`gparted`](http://gparted.sourceforge.net)

note:
The libparted Tools The GNU Project’s libparted (http://www.gnu.org/software/parted/), which comes with the
parted text-mode program, is a popular tool that can handle MBR, GPT, APM, and several other partition table
formats. GUI tools, such as GNOME Partition Editor (aka GParted; http://gparted.sourceforge.net), have been
built upon libparted. The greatest strength of these tools is the ability to move and resize both partitions and
the filesystems they contain. They can also create filesystems at the same time you create partitions.



## Creando paticiones (III)
### fdisk Family

* [`sfdisk`](http://linux.die.net/man/8/sfdisk): herramienta no interactiva útil para creación de scripts
* [`cfdisk`](http://linux.die.net/man/8/cfdisk): herramienta sofisticada  basada en un interfaz de texto
* [`fdisk`](http://linux.die.net/man/8/fdisk):  herramienta interactiva

note:
* fdisk is the basic program, with a simple text-mode interactive user interface. 
* The `sfdisk` program  it’s designed to be used in a non-interactive way via command-line options. Uuseful in scripts. 
* The `cfdisk` program uses a more sophisticated text-mode interface similar to that of a text editor.
 These programs ship with the standard util-linux or util-linux-ng packages.



## Creando paticiones (III)
### fdisk Family: cfdisk

<a class="fancybox" href="img/cfdisk.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/cfdisk.png" alt="Introducción">
</a>



## Creando paticiones (III)
### fdisk Family: opciondes fdisk

<div class="table-responsive">
    <table class="table table-hover table-bordered table-condensed">
        <thead>
            <tr>
                <th>Opción</th>
                <th>Descripción</th>
            </tr>
        </thead>
        <tbody>
        <tr>
        <td>d</td>
        <td>Deletes a partition</td>
        </tr>
        <tr>
        <td>l</td>
        <td>Displays a list of partition type codes</td>
        </tr>
        <tr>
        <td>n</td>
        <td>Creates a new partition</td>
        </tr>
        <tr>
        <td>o</td>
        <td>Destroys the current partition table, enabling you to start fresh</td>
        </tr>
        <tr>
        <td>p</td>
        <td>Displays the current partition table</td>
        </tr>
        <tr>
        <td>q</td>
        <td>Exits without saving changes</td>
        </tr>
        <tr>
        <td>t</td>
        <td>Changes a partition’s type code</td>
        </tr>
        <tr>
        <td>u</td>
        <td>Toggles units between sectors and cylinders</td>
        </tr>
        <tr>
        <td>v</td>
        <td>Performs checks on the validity of the disk’s data structures</td>
        </tr>
        <tr>
        <td>w</td>
        <td>Saves changes and exits</td>
        </tr>
        </tbody>
    </table>
</div>

note:
| Comando | Explicación
| --------| ------------
| d       | Deletes a partition
| l       | Displays a list of partition type codes
| n       | Creates a new partition
| o       | Destroys the current partition table, enabling you to start fresh
| p       | Displays the current partition table
| q       | Exits without saving changes
| t       | Changes a partition’s type code
| u       | Toggles units between sectors and cylinders
| v       | Performs checks on the validity of the disk’s data structures
| w       | Saves changes and exits

note:
* The l and t commands deserve elaboration: MBR supports a 1-byte type code for each partition.
*  This code helps identify what types of data are supposed to be stored on the partition.
* For instance, in hexadecimal, 0x07 refers to a partition that holds High Performance Filesystem (HPFS) or New Technology Filesystem (NTFS) data, 0x82 refers to a Linux swap partition, and 0x83 refers to a Linux data partition.
*  For the most part, Linux ignores partition type codes; however, Linux installers frequently rely on them, as do other OSs. Thus, you should be sure your partition type codes are set correctly. 
* Linux fdisk creates 0x83 partitions by default, so you should change the code if you create anything but a Linux partition.



## Creando paticiones (IV)
### GPT fdisk

* [`gdisk`](http://linux.die.net/man/8/gdisk)
* [`sgdisk`](http://linux.die.net/man/8/sgdisk)

note:
* Consisting of the gdisk and sgdisk programs, is designed as a workalike to fdisk but for GPT disks. 
* The gdisk program is modeled on fdisk. 
* Although sgdisk is designed for shell-based interaction, it bears little resemblance to sfdisk in its operational details. 
* You can learn more at http://www.rodsbooks.com/gdisk/.
* GPT also supports partition type codes, but these codes are 16-byte GUID values rather than 1-byte MBR type codes. 
* GPT fdisk translates the 16-byte GUIDs into 2-byte codes based on the MBR codes;
* Unfortunately, Linux and Windows use the same GUID code for their partitions, so GPT fdisk translates both to 0x0700. 
* Programs based on libparted don’t give direct access to partition type codes, although they use them internally.
* Several GPT type codes are referred to as “flags” in for instance, the “boot flag” refers to a partition with the type code for an EFI System Partition on a GPT disk.



## Particiones para RAID (I)

<a class="fancybox" href="img/example_particionado_raid.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/example_particionado_raid.png" alt="Introducción">
</a>

note:
Ejemplo de creación de particiones



## Particiones para RAID (II)
### Buscamos dispositivos

```bash
ijfviana-dev@vagalume:~$ fdisk -l

Disco /dev/sda: 8589 MB, 8589934592 bytes
255 heads, 63 sectors/track, 1044 cylinders
Units = cilindros of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000801fd

Disposit. Inicio    Comienzo      Fin      Bloques  Id  Sistema
/dev/sda1   *           1          64      512000   83  Linux
La partición 1 no termina en un límite de cilindro.
/dev/sda2              64        1045     7875584   8e  Linux LVM

Disco /dev/sdb: 134 MB, 134846464 bytes
255 heads, 63 sectors/track, 16 cylinders
Units = cilindros of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

Disco /dev/sdc: 134 MB, 134217728 bytes
255 heads, 63 sectors/track, 16 cylinders
Units = cilindros of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000
```

Salen (entre otros) los dos dispositivos 



## Particiones para RAID (III)
### Creamos particiones en /dev/sdb

``` bash
ijfviana-dev@vagalume:~$ fdisk /dev/sdb
El dispositivo no contiene una tabla de particiones DOS válida ni una etiqueta de disco Sun o SGI o OSF
Building a new DOS disklabel with disk identifier 0x23a6b33a.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Atención: el indicador 0x0000 inválido de la tabla de particiones 4 se corregirá mediante w(rite)

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

Orden (m para obtener ayuda): n
Acción de la orden
e   Partición extendida
   p   Partición primaria (1-4)
p
Número de partición (1-4): 2
Primer cilindro (10-16, valor predeterminado 10): 
Se está utilizando el valor predeterminado 10
Last cilindro, +cilindros or +size{K,M,G} (10-16, valor predeterminado 16): 
Se está utilizando el valor predeterminado 16

Orden (m para obtener ayuda):
```

Dos particiones primarias de 64 Megas cada una



## Particiones para RAID (IV)
### Comprobamos particiones creadas /dev/sdb

``` bash
Orden (m para obtener ayuda): p

Disco /dev/sdb: 134 MB, 134846464 bytes
255 heads, 63 sectors/track, 16 cylinders
Units = cilindros of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x23a6b33a

Disposit. Inicio    Comienzo      Fin      Bloques  Id  Sistema
/dev/sdb1               1           9       72261   83  Linux
/dev/sdb2              10          16       56227+  83  Linux

Orden (m para obtener ayuda): 
```



## Particiones para RAID (V)
### Cambia de tipo de partición (I)

``` bash
Orden (m para obtener ayuda): l

 0  Vacía          24  NEC DOS         81  Minix / old Lin bf  Solaris        
 1  FAT12           39  Plan 9          82  Linux swap / So c1  DRDOS/sec (FAT-
 2  XENIX root      3c  PartitionMagic  83  Linux           c4  DRDOS/sec (FAT-
 3  XENIX usr       40  Venix 80286     84  Unidad C: ocult c6  DRDOS/sec (FAT-
 4  FAT16 <32M      41  PPC PReP Boot   85  Linux extendida c7  Syrinx         
 5  Extendida       42  SFS             86  Conjunto de vol da  Datos sin SF   
 6  FAT16           4d  QNX4.x          87  Conjunto de vol db  CP/M / CTOS / .
 7  HPFS/NTFS       4e  QNX4.x segunda  88  Linux plaintext de  Utilidad Dell  
 8  AIX             4f  QNX4.x tercera  8e  Linux LVM       df  BootIt         
 9  AIX bootable    50  OnTrack DM      93  Amoeba          e1  DOS access     
 a  OS/2 Boot Manag 51  OnTrack DM6 Aux 94  Amoeba BBT      e3  DOS R/O        
 b  W95 FAT32       52  CP/M            9f  BSD/OS          e4  SpeedStor      
 c  W95 FAT32 (LBA) 53  OnTrack DM6 Aux a0  Hibernación de eb  BeOS fs        
 e  W95 FAT16 (LBA) 54  OnTrackDM6      a5  FreeBSD         ee  GPT            
 f  W95 Ext'd (LBA) 55  EZ-Drive        a6  OpenBSD         ef  EFI (FAT-12/16/
10  OPUS            56  Golden Bow      a7  NeXTSTEP        f0  inicio Linux/PA
11  FAT12 oculta    5c  Priam Edisk     a8  UFS de Darwin   f1  SpeedStor      
12  Compaq diagnost 61  SpeedStor       a9  NetBSD          f4  SpeedStor      
14  FAT16 oculta <3 63  GNU HURD o SysV ab  arranque de Dar f2  DOS secondary  
16  FAT16 oculta    64  Novell Netware  af  HFS / HFS+      fb  VMware VMFS    
17  HPFS/NTFS ocult 65  Novell Netware  b7  BSDI fs         fc  VMware VMKCORE 
18  SmartSleep de A 70  DiskSecure Mult b8  BSDI swap       fd  Linux raid auto
1b  Hidden W95 FAT3 75  PC/IX           bb  Boot Wizard hid fe  LANstep        
1c  Hidden W95 FAT3 80  Old Minix       be  arranque de Sol ff  BBT            
1e  Hidden W95 FAT1
```

Posibles tipos de particiones



## Particiones para RAID (VI)
### Cambia de tipo de partición (II)

``` bash
Orden (m para obtener ayuda): t
Número de partición (1-4): 1
Código hexadecimal (escriba L para ver los códigos): fd
Se ha cambiado el tipo de sistema de la partición 1 por fd (Linux raid autodetect)

Orden (m para obtener ayuda): t
Número de partición (1-4): 2
Código hexadecimal (escriba L para ver los códigos): fd
Se ha cambiado el tipo de sistema de la partición 2 por fd (Linux raid autodetect)

Orden (m para obtener ayuda): p

Disco /dev/sdb: 134 MB, 134846464 bytes
255 heads, 63 sectors/track, 16 cylinders
Units = cilindros of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x23a6b33a

Disposit. Inicio    Comienzo      Fin      Bloques  Id  Sistema
/dev/sdb1               1           9       72261   fd  Linux raid autodetect
/dev/sdb2              10          16       56227+  fd  Linux raid autodetect

Orden (m para obtener ayuda): 
```

El tipo debe ser 0xFD

note:
* When you partition a disk for RAID, you should be sure to assign the 0xFD type code on an MBR disk.
* If you edit a GPT disk using GPT fdisk, the equivalent code is 0xFD00. 
* If you use a libparted-based tool with either MBR or GPT disks, a RAID partition is identified as one with the RAID flag set.



## Particiones para RAID (VII)
### Hacemos efectivos los cambios

```bash
Orden (m para obtener ayuda): w
¡Se ha modificado la tabla de particiones!

Llamando a ioctl() para volver a leer la tabla de particiones.
Se están sincronizando los discos.
```

note:
* When you partition a disk for RAID, you should be sure to assign the 0xFD type code on an MBR disk.
* If you edit a GPT disk using GPT fdisk, the equivalent code is 0xFD00. 
* If you use a libparted-based tool with either MBR or GPT disks, a RAID partition is identified as one with the RAID flag set.
