# Cheat Sheet PAR

1. **Bridges**:
* `# brctl addbr br01`		**primer bridge** repetir por cada bridge
* `# brctl show`			**compruebo que estan creados**

2. **WLAN's**
* `ip link add name eth0.110 link eth0 type vlan id 110` **linkeo las wlans a la tarjeta de red**
* `brctl addif br01 eth0.1`	**Especificar la WLAN de la maquina**

3. **TAP's**
* `ip tuntap add mode tap user <user>` **Anadir el "tap"** 
* `brctl addif br01 tap0`

4. **BOND**
* `ip link add name ${name} type bond`
* `modprobe mode=4 bonding`				error Module mode=4 not found in directory /lib/modules/4.9.0-4-amd64
* `ip l set dev bond0 master br0`
* `ip l set dev tap0 master bond0`