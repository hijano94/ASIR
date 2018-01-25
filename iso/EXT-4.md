#Sistema de Archivos ==> EXT-4
> 23/1/18 -IMPL

## Buscando sus Propiedades:

```[bash]
	lsblk -f	#lista los dispositivos de bloques

	dumpe2fs /dev/vdb1	#(root) informacion del sistema de archivos
	df -h /dev/vdb1

``` 
### Datos de interes del sistema de archivos:

	* UUID: identificador del sistema de archivos
	* Filesystem state: estado del sistema de archivos (clean=sin fallos, pero no vacio)
	* Inode count: numero maximo de inodos
	* Block count: numero maximo de bloques
	* Block size: tamano de bloque
	* Free inodes: numero de inodos libres
	* Free blocks: numero de bloques libres
	* Inodes per group: numero de inodos por grupo
	* Inodo size: tamano de inodo

> 24/1/18 -IMPL
## crear dispositivo de bloques:
### sistema de ficheros:
```
fdisk  /dev/vdx
command (m for help): n
select: p
partition number: 
last sector: +512M
command: p ?? (print)

	*	q ==> no guarda
	*	w ==> guarda conf	

mkfs. + doble tab 	(muestra los sistemas de archivos soportados)
mkfs.ext4 --help 	(muestra las diferentes opciones para crear el
					sistema de archivos)
mkfs.ext4 /dev/vdd1

# Hay que montar el disco
	* mount -t ext4 /dev/vdd1 /mnt

# Llenar el disco de ceros
	* dd if=/dev/zero of=/mnt bs=4096 count=124719
					         (block size)(num blocks)

```
## Creacion de un sistema de ficheros:

	ya tenemos formadas las particiones y ahora vamos a "hacerlo visible"/"montarlo"/generar el sistema de archivos

1. Instalar los siguientes paquetes:
	
	* dosfstools
	* ntfs-3g
	* xfsprogs 

2. Montar el sistema de archivos:
```
	mkfs.ext4 -L Manolito /dev/vdd1 	 	# Anadimos una etiqueta al sistema de archivos
	mount -U <codigo_UUID> /mnt 			# (/mnt=ubicacion)

	mount									# Lista todos los sistemas de archivos montados

	mount -o ro,remount /dev/vdd1 /mnt 		# Cambiar los permisos de los sistemas de archivos (ro= read only)
	
```