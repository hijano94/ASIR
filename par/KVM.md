# KVM
> 23/1/18 -PAR

## Primeros pasos
1. instalar bridge-utils
* `apt-get install bridge-utils`
2. modificar network/interfaces
* `nano /etc/network/interfaces`
``` 	iface eth0 inet manual	#opcional para evitar problemas con network manager
	allow-hotplug br0
	iface br0 inet dhcp
		bridge_ports eth0

	#allow-hotplug eth0
	#iface eth0 inet dhcp
```
