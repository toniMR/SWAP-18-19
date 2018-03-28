# PRACTICA 1 SWAP

Tras instalar las maquinas, con el servidor LAMP, tenemos que configurar las redes. En mi caso lo he hecho con una red solo-anfitrion. 

Con sudo nano /etc/network/interfaces modificamos la configuracion de las interfaces de nuestras maquina:

Maquina -> UbuntuServer

auto lo
iface lo inet loopback

auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
address 192.168.10.100
netmask 255.255.255.0
gateway 192.168.10.1
network 192.168.10.0
broadcast 192.168.1.255




Maquina -> Clonada

auto lo
iface lo inet loopback

auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
address 192.168.10.150
netmask 255.255.255.0
gateway 192.168.10.1
network 192.168.10.0
broadcast 192.168.1.255




Ahora podemos acceder por ssh de una maquina a otra. Ademas crearemos un archivo.html y lo pondremos en /var/www/html/ para verlo con curl desde la otra máquina.

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P1/imagenes/ssh1.png)
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P1/imagenes/ssh2.png)


