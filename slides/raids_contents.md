## Definición

* RAID: **R**edundant **A**rray of **I**ndependent **D**isks
* Es una forma de combinar el almacenamiento disponible en múltiples discos y proporcionar al usuario un dispositivo virtual único
* Los datos se divide en trozos que se almacenan entre los distintos discos que forman en RAID.

note:
* The term RAID is an acronym for the phrase, Redundant Array of Independent Disks. 
* RAID is a way of combining the storage available across multiple disks and supplying users a single, unified virtual device.
* RAID then breaks down the data into consistently-sized chunks (commonly 32K or 64k).
* Each chunk is then written to a hard drive in the RAID array according to the RAID level employed. When the data is read, the process is reversed, giving the illusion that the multiple drives in the array are actually one large drive.



## Ventajas

* Integridad en los datos
* Tolerancia a fallos
* Mejorar el rendimiento 
* Gran capacidad de almacenamiento

note:

* RAID can be used to provide:
 * data integrity
 * fault tolerance
 * improved performance
 * greater storage capacity



## Tipos de Raid (I)

* Los niveles de RAID más usados son:

 * [Raid 0](#/3/3)
 * [Raid 1](#/3/4)
 * [Raid 4](#/3/5)
 * [Raid 5](#/3/6)
 * [Otros tipos de RAID](#/3/7)

note:
* Multiple physical locations according to several different schemas, known as "RAID Levels".



## Tipos de Raid (II)
### Raid 0 (striping)

<a class="fancybox" href="img/raid0.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid0.png" alt="Introducción">
</a>

note:
* This form of RAID combines multiple disks to present the illusion of a single storage area as large as all the combined disks. 
* The disks are combined in an interleaved manner so that a single large access to the RAID device (for instance, when reading or writing a large file) results in accesses to all the component devices. 
* This configuration can improve overall disk performance; however, if any one disk fails, data on the remaining disks will become useless. 
* Thus, reliability actually decreases when using RAID 0, compared to conventional partitioning.



## Tipos de Raid (III)
### Raid 1 (mirroring)

<a class="fancybox" href="img/raid1.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid1.png" alt="Introducción">
</a>

note:
* This form of RAID creates an exact copy of one disk’s contents on one or more other disks. 
* If any one disk fails, the other disks can take over, thus improving reliability. 
* The drawback is that disk writes take longer, since data must be written to two or more disks. 
* Additional disks may be assigned as hot standby or hot spare disks, which can automatically take over from another disk if one fails. (Higher RAID levels also support hot standby disks.)



## Tipos de Raid (IV)
### Raid 4 (block striping)

<a class="fancybox" href="img/raid4.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid4.png" alt="Introducción">
</a>

note:
* Data are striped in a manner similar to RAID 0; but one drive is dedicated to holding checksum data. 
* If any one disk fails, the checksum data can be used to regenerate the lost data.
* The checksum drive does not contribute to the overall amount of data stored;
* If you have n identically sized disks, they can store the same amount of data as n – 1 disks of the same size in a non-RAID or RAID 0 configuration. 
* You need at least three identically sized disks to implement a RAID 4 array.
* RAID4 is used on a limited basis due to the storage penalty and data corruption vulnerability of dedicating an entire disk to parity.



## Tipos de Raid (V)
### Raid 5 (block striping with striped parity)

<a class="fancybox" href="img/raid5.png" data-fancybox-group="gallery" title="Introducción">
<img height="550px" src="img/raid5.png" alt="Introducción">
</a>

note:
* This form of RAID works just like RAID 4, except the checksum data are interleaved on all the disks in the array. 
* RAID 5’s size computations are the same as those for RAID 4; you need a minimum of three disks, and n disks hold n – 1 disks worth of data.
* RAID5 is the most commonly used level as it provides the best combination of benefits and acceptable costs.



## Tipos de Raid (VI)
### Otros tipos de RAID: RAID 6

<a class="fancybox" href="img/raid6.png" data-fancybox-group="gallery" title="RAID 6">
<img height="500px" src="img/raid6.png" alt="RAID 6">
</a>

note:
* What if two drives fail simultaneously? In RAID 4 and RAID 5, the result is data loss. 
* RAID 6, though, increases the amount of checksum data, therefore increasing resistance to disk failure. 
* The cost, however, is that you need more disks: four at a minimum. With RAID 6, n disks hold n – 2 disks worth of data.



## Tipos de Raid (VII)
### Otros tipos de RAID: RAID 10

<a class="fancybox" href="img/raid10.png" data-fancybox-group="gallery" title="RAID 10">
<img height="500px" src="img/raid10.png" alt="RAID 10z">
</a>

note:
* RAID 10 A combination of RAID 1 with RAID 0, referred to as RAID 1 + 0 
* Provides benefits similar to those of RAID 4 or RAID 5. 
* Linux provides explicit support for this combination to simplify configuration.



## Tipos de Raid (VIII)
### Otros tipos de RAID: Lineal (JBOD)

<a class="fancybox" href="img/JBOD.png" data-fancybox-group="gallery" title="RAID 10">
<img height="500px" src="img/JBOD.png" alt="RAID 10z">
</a>

note:
* Is a simple grouping of drives to create a larger virtual drive. 
* The chunks are allocated sequentially from one member drive, going to the next drive only when the first is completely filled. 
* This grouping provides no performance benefit, as it is unlikely that any I/O operations will be split between member drives. 
* Offers no redundancy and, in fact, decreases reliability — if any one member drive fails, the entire array cannot be used. 
* The capacity is the total of all member disks. 



## Comparativa RAID (I)

<div class="table-responsive">
    <table class="table table-hover table-condensed table-bordered">
        <thead>
            <tr>
                <th>Features</th>
                <th>0</th>
                <th>1</th>
                <th>5</th>
                <th>6</th>
                <th>10</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>`Minimun # Drivers`</td>
                <td> <span class="fragment fade-in">2</span></td>
                <td><span class="fragment fade-in">2</span></td>
                <td><span class="fragment fade-in">3</span></td>
                <td><span class="fragment fade-in">4</span></td>
                <td><span class="fragment fade-in">4</span></td>
            </tr>

            <tr>
                <td>`Data Protection`</td>
                <td><span class="fragment fade-in">No protection</span></td>
                <td><span class="fragment fade-in">Single-drive  failure</span></td>
                <td><span class="fragment fade-in">Single-drive  failure</span></td>
                <td><span class="fragment fade-in">Two drive failure</span></td>
                <td><span class="fragment fade-in">Up to one disk failure in each sub-array</span></td>
            </tr>
                 <tr>
                <td>`Read Performance`</td>
                <td><span class="fragment fade-in">High</span></td>
                <td><span class="fragment fade-in">High</span></td>
                <td><span class="fragment fade-in">High</span></td>
                <td><span class="fragment fade-in">High</span></td>
                <td><span class="fragment fade-in">High</span></td>
           </tr>
        </tbody>
    </table>
</div>



## Comparativa RAID (II)

<div class="table-responsive">
    <table class="table table-hover table-condensed table-bordered">
        <thead>
            <tr>
                <th>Features</th>
                <th>0</th>
                <th>1</th>
                <th>5</th>
                <th>6</th>
                <th>10</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>`Write Performance`</td>
                <td><span class="fragment fade-in">High</span></td>
                <td><span class="fragment fade-in">Medium</span></td>
                <td><span class="fragment fade-in">Low</span></td>
                <td><span class="fragment fade-in">Low</span></td>
                <td><span class="fragment fade-in">Medium</span></td>
            </tr>
            <tr>
                <td>`Capacity Utilization`</td>
                <td><span class="fragment fade-in">100%</span></td>
                <td><span class="fragment fade-in">50%</span></td>
                <td><span class="fragment fade-in">67%-94%</span></td>
                <td><span class="fragment fade-in">50%-88%</span></td>
                <td><span class="fragment fade-in">50%</span></td>
            </tr>
            <tr>
                <td>`Typical Applications`</td>
                <td><span class="fragment fade-in">High end workstations, real time, rendering</span></td>
                <td><span class="fragment fade-in">Operating System, transaction databases</span></td>
                <td><span class="fragment fade-in">Data warehousing, web serving</span></td>
                <td><span class="fragment fade-in">Data archive,high availabilly solutions</span></td>
                <td><span class="fragment fade-in">Fast databases, application servers</span></td>
            </tr>
        </tbody>
    </table>
</div>



## Implementaciones de RAID (I)

* [RAID software](#/3/14)
* [Firmware RAID](#/3/15)
* [RAID hardware](#/3/15)

note:
* RAID can be provided by either dedicated, specialized hardware, by the operating system at a virtual layer, or by firmware software.



## Implementaciones de RAID (II)
### Software RAID

* Es una solución barata y eficiente (depende de la CPU)
* El SO implementa la lógica de los distintos niveles RAID (MD driver)
 * Proceso reconstrucción multihebra en segundo plano
 * Configuración del kernel
 * Portabilidad de RAID entre máquinas
 * Intercambio en caliente
 * Adaptación a CPU

note:
* Software RAID implements the various RAID levels in the kernel disk (block device) code. 
* It offers the cheapest possible solution, itworks with cheaper IDE disks as well as SCSI disks.
* The performance of a software-based array depends on the server CPU performance and load.
* The Linux kernel contains an MD driver that allows the RAID solution to be completely hardware independent. 
* Features:
    * Threaded rebuild process
    * Kernel-based configuration
    * Portability of arrays between Linux machines without reconstruction
    * Backgrounded array reconstruction using idle system resources
    * Hot-swappable drive support
    * Automatic CPU detection to take advantage of certain CPU optimizations



## Implementaciones de RAID (III)
### Firmware RAID

<div class="row">
    <div class="col-xs-6 col-sm-6 col-md-6 col-lg-6 col-centered">
        <ul>
            <li>Configuración de RAID basado en firmware
            <li>Existe un enlace en la BIOS
            <li>[Intel Matrix RAID](http://en.wikipedia.org/wiki/Intel_Matrix_RAID)
        </ul>
    </div>
    <div class="col-xs-6 col-sm-6 col-md-6 col-lg-6 col-centered">
        <a class="fancybox" href="img/raid_firmware.png" data-fancybox-group="gallery" title="Firmware RAID">
            <img height="500px" src="img/raid_firmware.png" alt="Firmware RAID">
        </a>
    </div>
</div>

note: 
* Firmware RAID (also known as ATARAID) is a type of software RAID where the RAID sets can be configured using a firmware-based menu. 
* The firmware used by this type of RAID also hooks into the BIOS, allowing you to boot from its RAID sets. 
* Different vendors use different on-disk metadata formats to mark the RAID set members. The Intel Matrix RAID is a good example of a firmware RAID system.



## Implementaciones de RAID (IV)
### Hardware RAID

<div class="row">
    <div class="col-xs-6 col-sm-6 col-md-6 col-lg-6 col-centered">
        <ul>
            <li>Independiente del host
            <li>El RAID es controlado por un hardware externo
            <li>Para el host, este hardware es como un controlador de disco
            <li>Estas controladoras tiene su propio software de configuración
        </ul>
    </div>
    <div class="col-xs-6 col-sm-6 col-md-6 col-lg-6 col-centered">
        <a class="fancybox" href="img/raid_hardware.png" data-fancybox-group="gallery" title="Raid Hardware">
            <img height="500px" src="img/raid_hardware.png" alt="Hardware">
        </a>
    </div>
</div>

note:
* The hardware-based array manages the RAID subsystem independently from the host. 
* A hardware RAID device connects to the SCSI controller and presents the RAID arrays as a single SCSI drive. 
* An external RAID system moves all RAID handling “intelligence” into a controller located in the external disk subsystem. 
* The whole subsystem is connected to the host via a normal SCSI controller and appears to the host as a single disk.
* RAID controller cards function like a SCSI controller to the operating system, and handle all the actual drive communications. The user plugs the drives into the RAID controller (just like a normal SCSI controller) and then adds them to the RAID controllers configuration, and the operating system won't know the difference.
* In a true hardware RAID, the operating system simply writes data to the hardware RAID controller which handles the multiplicitous reads and writes to the associated disks. 
* Other so−called hardware RAIDs rely on special drivers to the operating system; these act more like software RAIDs in practice.
* With current technology, hardware RAID configurations are generally chosen for very large RAIDs.



## RAID en Linux

* Subsistemas:
 * ⁠Linux Hardware RAID controller drivers
 * mdraid
 * dmraid

note:
* RAID in Linux is composed of the following subsystems:



## RAID en Linux
### ⁠Linux Hardware RAID controller drivers

* Hardware RAID controllers no tiene un subsistema RAID específico
* Necesitamos los drivers del chipset de la controladora
* Estos drivers me permiten detectar los RAID

note:
* Hardware RAID controllers have no specific RAID subsystem in Linux. 
* Because they use special RAID chipsets, hardware RAID controllers come with their own drivers; 
* these drivers allow the system to detect the RAID sets as regular disks.



## RAID en Linux
### mdraid (MD devices)

* Diseñado como solución software en Linux
* Usa su propio formato de metadatos aunque soporta otros
* Se gestiona mediante la utilidad [`mdadm`](http://linux.die.net/man/8/mdadm)
* RAID soportados: JBOD, RAID0, RAID1, RAID4, RAID5, RAID6, RAID10, MULTIPATH, FAULTY y CONTAINER

note:
* The mdraid subsystem was designed as a software RAID solution for Linux; 
* This subsystem uses its own metadata format, generally referred to as native mdraid metadata.
* mdraid also supports other metadata formats, known as external metadata. 
* Red Hat Enterprise Linux 6 uses mdraid with external metadata to access ISW / IMSM (Intel firmware RAID) sets. 
* mdraid sets are configured and controlled through the mdadm utility.



## RAID en Linux
### dmraid

* Device-mapper RAID hace referencia al módulo del kernel que permite "unir trozos" de disco en un RAID dmraid
* Se gestionan mediante la utilidad [`dmraid`](http://linux.die.net/man/8/dmraid)
* Todo se gestionar en el espacio de usuario
* Es usado por algunas implementaciones firmware RAID 

note:
* Device-mapper RAID or dmraid refers to device-mapper kernel code that offers the mechanism to piece disks together into a RAID set. 
* This same kernel code does not provide any RAID configuration mechanism.
* dmraid is configured entirely in user-space, making it easy to support various on-disk metadata formats. 
* As such, dmraid is used on a wide variety of firmware RAID implementations.
* dmraid also supports Intel firmware RAID, although Red Hat Enterprise Linux 6 uses mdraid to access Intel firmware RAID sets.



## RAID y particiones (I)

* Linux permite crear RAID software basados en particiones

<div class="row">
    <div class="col-xs-9 col-sm-9 col-md-9 col-lg-9 col-centered">
        <a class="fancybox" href="img/raid_particiones.png" data-fancybox-group="gallery" title="RAID con particiones">
        <img src="img/raid_particiones.png" alt="RAID con particiones">
        </a>
    </div>
</div>

note:
* Linux’s implementation of RAID is usually applied to partitions rather than to whole disks. 



## RAID y particiones (II)

* Las particiones se combinan danto lugar a nuevos dispositivos `/dev/md#`

<div class="row">
    <div class="col-xs-7 col-sm-7 col-md-7 col-lg-7 col-centered">
        <a class="fancybox" href="img/raid_particiones_2.png" data-fancybox-group="gallery" title="Dispositivos md">
        <img height="550px" src="img/raid_particiones_2.png" alt="Dispositivos md">
        </a>
    </div>
</div>

note:

* Partitions are combined by the kernel RAID drivers to create new devices, with names of the form /dev/md#, 
* This configuration enables you to combine devices using different RAID levels, to use RAID for only some partitions



## Raid y partciones (III)
### Grub y RAID

* GRUB Legacy NO soporta RAID salvo el RAID 1
* GRUB 2 soporta RAID 
* Lo mejor es que la partición de `/boot` no esté en RAID   

note:
* When using Linux’s software RAID, you should realize that boot loaders are sometimes unable to read RAID arrays.
* GRUB Legacy, in particular, can’t read data in a RAID array. (RAID 1 is a partial exception; because RAID 1 partitions are duplicates of each other, GRUB Legacy can treat them like normal partitions for read-only access.)
* GRUB 2 includes Linux RAID support, however. 
* Because of this limitation, you may want to leave a small amount of disk space in a conventional partition or used as RAID 1, for use as a Linux /boot partition.



## RAID y particiones (IV)
### Requisitos

* El kernel lo debe soportar (Device Drivers ⇒ Multiple Devices Driver Support (RAID and LVM) ⇒ RAID Support area))
* Tener instaladas las utilidades [`mdadm`](http://linux.die.net/man/8/mdadm)

note:
* Most distributions ship with kernels that have the necessary support. 
* If you compile your own kernel you should be sure to activate the RAID features you need. These can be found in the Device Drivers ⇒ Multiple Devices Driver Support (RAID and LVM) ⇒ RAID Support area. 
* Utilidades mdadm



## Raid y particiones (V)
### Pasos a dar

1. Crear las particiones
2. Agrupar las particiones
3. Crear el sistema de ficheros
4. Montaje automático
