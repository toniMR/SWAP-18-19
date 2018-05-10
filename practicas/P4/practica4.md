# PRACTICA 4 SWAP

**Objetivos de la práctica**

-Instalar un certificado SSL para configurar el acceso HTTPS a los servidores.
-Configurar las reglas del cortafuegos para proteger la granja web.

**Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS**

a2enmod ssl
service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

Editamos el archivo de configuración del sitio default-ssl:
nano /etc/apache2/sites-available/default-ssl

Y agregamos estas lineas debajo de donde pone SSLEngine on:
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key

Activamos el sitio default--ssl y reiniciamos apache:
a2ensite default-ssl
service apache2 reload

Una vez reiniciado Apache, accedemos al servidor web mediante el protocolo HTTPS
y veremos, si estamos accediendo con un navegador web, que en la barra de
dirección sale en rojo el https, ya que se trata de un certificado autofirmado.
Para hacer peticiones por HTTPS utilizando la herramienta curl, ejecutaremos:
curl –k https://ipmaquina1/index.html

**Configurando el balanceador**

Copiamos los certificados y la configuracion de Apache a las demás máquinas.

Para configurar Nginx tenemos que escribir estás líneas en /etc/nginx/conf.d/default.conf

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P4/imagenes/confNginx-recortada.png)

**Configurando el firewall de las máquinas**
Hacemos un script para reralizar nuestra configuración:

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P4/imagenes/scriptIptables-recortada.png)

Para que se ejecute al iniciar la máquina, tenemos que mover nuestro script a /etc/init.d/<NombeAchivo>, darle permisos de ejecución con chmod +x y actualizar el registro de Script de inicio con **sudo update-rc.d <nombeScript> defaults**

Comprobaremos que se ha realizado correctamente con **sudo iptables -L -n -v**

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P4/imagenes/comprobacion-recortada.png)
