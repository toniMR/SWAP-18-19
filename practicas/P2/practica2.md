# PRACTICA 2

**Crear un tar con ficheros locales en un equipo remoto**
- tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P2/imagenes/Tar-Pipe.png)


**Instalar la herramienta rsync**
- sudo apt-get install rsync

hacer que el usuario sea el dueño de la carpeta donde residen los archivos que hay en el espacio web (en ambas máquinas):

- sudo chown pedro:pedro –R /var/www


**Copiar una carpeta de una maquina a otra**
- rsync -avz -e ssh ipmaquina1:ruta-archivo ruta-destino

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P2/imagenes/Rsync-Carpeta.png)


**Acceso sin contraseña para ssh**
Mediante ssh-keygen podemos generar la clave, con la opción -t para el tipo de clave.
Así, en la máquina secundaria ejecutaremos:
- ssh-keygen -b 4096 -t rsa
- chmod 600 ~/.ssh/authorized_keys
- ssh-copy-id ipMaquina

**Ya podemos hacer shh sin contraseña**


**Programar tareas con crontab**
![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P2/imagenes/Crontab.png)

Así, cada hora, se clonará el contenido de la carpeta /var/html en la maquina Clonada.


