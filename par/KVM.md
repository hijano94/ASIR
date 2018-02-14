# KVM
> 23/1/18 -PAR

## Primeros pasos
	
Primero tenemos que crear un bridge virtual para que se puedan conectar nuestras maquinas virtuales y acceder a internet.

1. instalar bridge-utils

	* `apt-get install bridge-utils`

2. modificar network/interfaces

	* `nano /etc/network/interfaces`
		`iface eth0 inet manual`	#opcional para evitar problemas con network manager
		`allow-hotplug br0`
		`iface br0 inet dhcp`
		`bridge_ports eth0`			
		`#allow-hotplug eth0`
		`#iface eth0 inet dhcp`
	
	* `ifdown br0`
	* `ifup br0`

3. Configurar las maquinas

	> brctl show Muestra los bridges en el -equipo

	* `# ip tuntap add mode tap user <user>`
	* `# ip tuntap list`
	* `# brctl addif br0 tap0`	# anado tap0 como puerto de br0
	* `$ MAC0=$(echo "02:"`openssl rand -hex 5 | sed 's/\(..\)/\1:/g; s/.$//'`)`	# asignar una MAC aleatoria

	* Creamos un fichero de la imagen
		`qemu-img create -b jessie-1.qcow2 -f qcow2 jessie1.qcow2 ` (qemu-utils)
		`qemu-img info jessie1.qcow2`
	
	* Levantamos los diferentes taps
		`ip l set tap0 up`	
	> repetir este proceso para cada maquina a crear  -root


4. Arranco la maquina virtual

```kvm -m 512 -hda jessie-1.qcow2 -device virtio-net,netdev=n0,mac=$MAC0 -netdev tap,id=n0,ifname=tap0,script=no,downscript=no```
	
* **`kvm -m 512 -hda jessie-1.qcow2`**
	 Utilizamos el ejecutable “kvm” que es equivalente a “qemu-system-x86_64 -enable-kvm”, asignamos 512 MiB de RAM a la máquina virtual y hacemos que utilice como disco duro el fichero de imagen jessie.qcow2.
	
* **`-device virtio-net,netdev=n0,mac=$MAC0`**
	 Creamos en la máquina virtual un dispositivo de red virtio, denominado n0 y con la dirección MAC anteriormente generada.(repetir para anadir otra tarjeta de red)
	
* **`-netdev tap,id=n0,ifname=tap0,script=no,downscript=no`**
	 Asociamos el dispositivo n0 de la máquina virtual con la interfaz tap0 que existe en el anfitrión, no utilizando ningún tipo de script al levantar ni al bajar dicha interfaz.(repetir para anadir otra tarjeta de red)

	* **`dhclient eth0`**	
	  para levantar la tarjeta de red


## WLANs:
	
1. Casos de uso:
	* Dividir el trafico por WLAN para servicios, politicas, etc. (para servidores)	
	* Cuando el equipo utilice bridges para derivar el trafico a diferentes maquinas virtuales.

2. Ejecucion:
	Tradicionalmente esto se configuraba con el comando "vconfig" del paquete "vlan". Hoy dia se utiliza el comando "ip".

	* Creacion de una maquina virtual asociada a una WLAN:
	
		* **Anadir un bridge por comando** `brctl addbr br01`
		* `ip link add name eth0.110 link eth0 type vlan id 110`
		* **Especificar la WLAN de la maquina** `brctl addif br01 eth0.1` 
		* `brctl delif br0 eth0`
		* `brctl addif br01 eth0`
		* **Anadir el "tap"** `ip tuntap add mode tap user <user>` `brctl addif br01 tap0`

## link aggregation (bonding)

* Para usarlo instalar (ifenslave)
* crear una interfaz `ip link add name ${name} type bond`
* `modprobe mode=4 bonding`
* ip a 
* Calcular ancho de banda (iperf)
	* crear 'servidor' `iperf -6`
	* conectar 'cliente' `iperf -c <IP>`

* crear el bonding, agregar los tap con ifenslave
* ver estado del bonding `cat /proc/net/bonding/bond`


Para conectar por link aggregation a una maquina virtual hay que crear los tap, crear bond, linkear los tap a bond, crear br y linkearle bond. Asignar IP en br0
En la maquina virtual crear el mismo bond y linkear las ethX