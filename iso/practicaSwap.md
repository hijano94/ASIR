# Practica SWAP
## Condiciones iniciales?
1. Particion SWAP

* Anado un **nuevo disco** a la maquina virtual desde la configuracion de **virtualbox**
* `lsblk -f` Con este comando puedo visualizar todos los discos de mi maquina, slecciono el que acabo de crear "sdb" (que es sr0?)
* `fdisk /dev/sdb`
	* **Orden** ===============> 'n'
	* **Tipo de particion** ===> 'p' (primaria)
	* **Numero de particion** => '1' (por defecto)
	* **Primer sector** =======> '2048' (por defecto)
	* **Ultimo sector** =======> '' (por defecto)
	* **Orden** ===============> 't' (asignar etiqueta, valor '82')
	* **Orden** ===============> 'w' (w; guarda | q; no guarda)
* `partprobe` #obliga a actualizar el estado de particiones (paquete "parted")
* `mkswap -L swap1 /dev/sdb1`
* `swapon /dev/sdb1` (activamos la swap)
* `swapon -s` (compruebo si la swap esta funcionando)
* `stress -m 4 -t 20` 
* `htop`

2. Fichero SWAP
* dd if=/dev/zero of=/swap bs=1024 count=65536
* mkswap /swap
* chmod 600 /swap
* swapon –v /swap
* free | grep swap
* /etc/fstab
	`/swap		swap	swap	defaults		0	0`