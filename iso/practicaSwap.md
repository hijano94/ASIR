# Practica SWAP
## Condiciones iniciales?
	* Anado un **nuevo disco** a la maquina virtual desde la configuracion de **virtualbox**
	* `lsblk -f` Con este comando puedo visualizar todos los discos de mi maquina, slecciono el que acabo de crear "sdb" (que es sr0?)
	* `fdisk /dev/sdb`
		* Orden => 'm'
		* Tipo de particion => 'e' (logica)
		* Numero de particion => '1' (por defecto)
		* Primer sector => '2048' (por defecto)
		* Ultimo sector => '' (por defecto)
		* Orden => 't' (asignar etiqueta, valor '82')
		* Orden => 'w' (w; guarda | q; no guarda)
	* `partprobe`
	* `mkswap -L swap1 /dev/sdb1`
	* `swapon /dev/sdb1` (activamos la swap)
	* `swapon -s` (compruebo si la swap esta funcionando)
	* stress y htop