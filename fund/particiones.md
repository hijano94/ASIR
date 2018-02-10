# Particionar
> 24/1/18 -FUND

## Sistemas de particiones:

1.	MBR: (Master Boot Record)(antiguo) Primer sector de toda 		esa pista, contiene codigo para arrancar el disco e 			informacion de las particiones.

2.	GPT: (Tabla de Particiones GUID)(actual) 
	*	Acceso LBA 
	*	Utiliza UEFI para crearse su propia particion usada por 	la bios para arrancar las particiones, la informacion		de las particiones se guarda en la particion propia de 		UEFI.
	*	Para un sistema de 64 bits necesita minimo 4 particiones
	*	Normalmente sistema de ficheros FAT-32
*	Alinear particiones con multiplos de 2048
*	ChainLoader: arranque en cadena

## Arquitecturas de disco:

	*	CHS: arquitectura de disco? (antigua)
	
## LBA: (moderna)
	
	*	 

## Sistema de ficheros comunes

1. FAT
	*	Es tan sencillo que cabe en un disquete <==\
	*	Se usa por el poco espacio que ocupa ======/ 
2. NTFS
	*		
3. EXT
	*		
4. ISO-9660

5. UDF

6. ZFS
	* Se tiende a usar en servidores de grandes prestaiones
	* Se recomienda para usarlo un minimo de 8 GB de RAM