# KVM
> 23/1/18 -PAR

## Primeros pasos
	
	Primero tenemos que crear un bridge virtual para que se puedan conectar 
	nuestras maquinas virtuales y acceder a internet.

1. instalar bridge-utils

	* `apt-get install bridge-utils`

2. modificar network/interfaces

	* `nano /etc/network/interfaces`
	```	
	iface eth0 inet manual	#opcional para evitar problemas con network manager
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

	* Creamos un fichero de la imagen
		`qemu-img create -b jessie-min.qcow2 -f qcow2 jessie1.qcow2 `
		`qemu-img info jessie1.qcow2`
	
	* Levantamos los diferentes taps
		`ip l set tap0 up`	
	> repetir este proceso para cada maquina a crear  -root


4. Arranco la maquina virtual

	``` 
	kvm -m 512 -hda jessie-1.qcow2 -device virtio-net,netdev=n0,mac=$MAC0 -netdev tap,id=n0,ifname=tap0,script=no,downscript=no
	```
	* `kvm -m 512 -hda jessie-1.qcow2`
	 Utilizamos el ejecutable “kvm” que es equivalente a “qemu-system-x86_64 -enable-kvm”, asignamos 512 MiB de RAM a la máquina virtual y hacemos que utilice como disco duro el fichero de imagen jessie.qcow2.
	* `-device virtio-net,netdev=n0,mac=$MAC0`
	 Creamos en la máquina virtual un dispositivo de red virtio, denominado n0 y con la dirección MAC anteriormente generada.(repetir para anadir otra tarjeta de red)
	* `-netdev tap,id=n0,ifname=tap0,script=no,downscript=no`
	 Asociamos el dispositivo n0 de la máquina virtual con la interfaz tap0 que existe en el anfitrión, no utilizando ningún tipo de script al levantar ni al bajar dicha interfaz.(repetir para anadir otra tarjeta de red)

	* dhclient eth0	# para levantar la tarjeta de red

	>25/1/18 -Me he quedado haciendo el aprovisionamiento ligero para arrancar varias maquinas virtuales. configurado nuevo tap y nueva MAC

5. Trabajando con la maquina virtual
	* `dhclient eth0` #conectarse a internet
6. Apagar la maquina
	* `poweroff`