# Particiones linux

1. **Veo los discos que tengo en mi maquina** `lsblk -f`

2. **Creamos las particiones** con *fdisk*:
* `fdisk /dev/sdb`
	* OP: n
	* OP: p
	* OP: 
	* OP: 
	* OP: +150M

	* OP: n
	* OP: e
	* OP:
	* OP: 
	* OP:

	* OP: w