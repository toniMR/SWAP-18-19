# PRACTICA 3 SWAP

**Instalar ngingx**

Primero desactivaremos apache haciendo:
- systemctl stop apache2
- sudo systemctl disable apache2 (Para que al reiniciar la maquina no se inicie)

sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove
sudo apt-get install nginx
sudo systemctl start nginx
sudo systemctl enable nginx (Para que al reiniciar este  activado)


**Configurar balanceador nginx**
Despues hay que cambiar la configuración, ya que por defecto tiene configuración de servidor web.
Hay que eliminar la informacion de /etc/nginx/conf.d/default.conf (Lo eliminamos todo)

En la parte de upstream indicamos las maquinas que formarán el cluster.

En la sección server indicamos que reparta la carga en las maquinas de upstream.

Es importante para que el proxy_pass funcione correctamente que indiquemos que la conexión entre nginx y los servidores finales sea HTTP 1.1 y especificarle que debe eliminar la cabecera Connection (hacerla vacía) para evitar que se pase al servidor final la cabecera que indica el usuario

**Configuracion /etc/nginx/conf.d/default.conf**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Conf-Nginx.png)

**En el archivo /etc/nginx/nginx.conf** he comentado include /etc/nginx/sites-enabled/*

Para probar el balanceador usaremos el comando curl http://192.168.10.200 varias veces para ver si va alternando lo que nos muestra (es decir, va cargando distintas maquinas de la granja)



**Instalar Haproxy**
Primero desactivaremos apache haciendo:
- systemctl stop apache2
- sudo systemctl disable apache2 (Para que al reiniciar la maquina no se inicie)

Desactivaremos nginx si lo tenemos instalado haciendo:
- systemctl stop nginx
- sudo systemctl disable nginx (Para que al reiniciar la maquina no se inicie)

Instalamos haproxy
sudo apt-get install haproxy

**Configurar balanceador haproxy**

modificar configuracion /etc/haproxy/haproxy.cfg

global
	daemon
	maxconn 256
defaults
	mode http
	contimeout 4000 → Pongo timeout connect
	clitimeout 42000  → Pongo timeout client   → (Las 3 para que no salten WARNINGS)
	srvtimeout 43000 → Timeout server
frontend http-in
	bind *:80
	default_backend servers
backend servers
server m1 192.168.10.100:80 maxconn 32
server m2 192.168.10.150:80 maxconn 32

**Configuracion /etc/nginx/conf.d/default.conf**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Conf-Haproxy.png)


**Lanzamos el servidor** sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg

**Hacemos las pruebas con ab desde otra maquina fuera de la granja**
Instalo el ab en otra maquina con direccion 192.168.10.111


**Ejecutando test de ab en la granja con balanceador Nginx**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Test-Maquinas-Nginx.png)


**Ejecutando test de ab en la granja con balanceador Haproxy**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Test-Maquinas-Haproxy.png)


**Resultado test de ab en la granja con balanceador Nginx**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Test-Maquinas-Nginx.png)

**Resultado test de ab en la granja con balanceador Haproxy**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P3/imagenes/Test-Maquinas-Haproxy.png)


**Comparando resultados del test de ab en la granja con Nginx vs Haproxy**
Como podemos observar, el **tiempo total para realizar el test con Nginx es 0.675 segundos** y el **tiempo por peticion en Nginx es 6.754** contra el **tiempo total para realizar el test con Haproxy es 0.582 segundos** y el **tiempo por peticion en Haproxy es 5.819**.

Por lo que **el que nos dá el mejor resultado es Haproxy**.


























