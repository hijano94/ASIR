# KVM
> 23/1/18 -PAR

## Primeros pasos
	
	Primero tenemos que crear un bridge virtual para que se puedan conectar 
	nuestras maquinas virtuales y acceder a internet.

1. instalar bridge-utils

	* `apt-get install bridge-utils`

2. modificar network/interfaces

	* `nano /etc/network/interfaces`
	```	iface eth0 inet manual	#opcional para evitar problemas con network manager
		allow-hotplug br0
		iface br0 inet dhcp
			bridge_ports eth0	
			
		#allow-hotplug eth0
		#iface eth0 inet dhcp	
	```
	* `ifdown br0`
	* `ifup br0`

3. Configurar las maquinas
> brctl show Muestra los bridges en el -equipo

	* `# ip tuntap add mode tap user <user>`
	* `# ip tuntap list`
	* `# brctl addif br0 tap0`	# anado tap0 como puerto de br0
	* `$ MAC0=$(echo "02:"`openssl rand -hex 5 | sed 's/\(..\)/\1:/g; s/.$//'`)`	# asignar una MAC aleatoria
> repetir este proceso para cada maquina a crear  -root

4. Arranco la maquina virtual

``` kvm -m 512 -hda jessie-1.qcow2 -device virtio-net,netdev=n0,mac=$MAC0 -netdev tap,id=n0,ifname=tap0,script=no,downscript=no
```
	* dhclient eth0	# para levantar la tarjeta de red

>25/1/18 -Me he quedado haciendo el aprovisionamiento ligero para arrancar varias maquinas virtuales. configurado nuevo tap y nueva MAC
