# PRACTICA 5 SWAP

**Objetivos de la práctica**

- Clonar BD entre máquinas

- Configurar la estructura maestro-esclavo entre 2 máquinas para realizar el clonado automátio de la información.

**Crear una BD e insertar datos**

- `mysql -root -p`
- `mysql> create database contactos`;
- `mysql> use contactos`;

- `mysql> insert into datos(nombre,tlf) values ("pepe",95834987)`;



**Replicar una BD MySQL con mysqldump**

Bloqueamos la BD para que no se modifique nada mientras se copia

- `mysql -u root –p`
- `mysql> FLUSH TABLES WITH READ LOCK;`
- `mysql> quit;`

- `mysqldump contactos -u root -p > /tmp/contactos.sql`


Desde la segunda máquina hacemos:

- `scp máquina1:/tmp/ejemplodb.sql /tmp/

- `mysql -u root –p`
- `mysql> CREATE DATABASE ‘ejemplodb’;`
- `mysql> quit;`

- `mysql -u root -p contactos < /tmp/ejemplodb.sql`


**Replicación de BD mediante una configuración maestro-esclavo**
Editamos /etc/mysql/mysql.conf.d/mysqld.cnf

Comentamos el parámetro bind-address que sirve para que escuche a un servidor:

-#bind-address 127.0.0.1

-log_error = /var/log/mysql/error.log

-server-id = 1

-log_bin = /var/log/mysql/bin.log

-/etc/init.d/mysql restart

En la otra máquina haremos lo mismo pero cambiaremos el server-id.



A continuación, en el maestro haremos:

`mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';`

`mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo@'%' IDENTIFIED BY 'esclavo';`

`mysql> FLUSH PRIVILEGES;`

`mysql> FLUSH TABLES;`

`mysql> FLUSH TABLES WITH READ LOCK;`

`mysql> SHOW MASTER STATUS;`

Ahora en la otra máquina 

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P5/imagenes/Change-Master.png)

`mysql> START SLAVE;`
`mysql> UNLOCK TABLES;`
`mysql> SHOW SLAVE STAUTS\G;`

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P5/imagenes/show-slave-status.png)

Ahora insertamos el usuario probando y veremos como sin hacer nada cambia automáticamente en la otra máquina.

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P5/imagenes/insertar-maestro.png)

![img](https://raw.githubusercontent.com/toniMR/SWAP-18-19/master/practicas/P5/imagenes/insertar-esclavo.png)


**NOTA**
Si nos da un error 1593 se soluciona borrando el archivo /var/lib/mysql/auto.cnf
