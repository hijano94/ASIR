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

