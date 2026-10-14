# Tarea2.2

## Práctica: instalar tres SGBD

### 1. Desplegar Oracle Database 23ai Free con Podman

#### Primero deberemos instalar Podman y descargar la imagen de Oracle
Para ello utilizaremos los siguientes comando
* sudo apt update
  ![sudo apt update](images/02.png)
* sudo apt install podman
  ![instalamos podman](images/03.png)
* sudo podman pull container-registry.oracle.com/database/free:23.5.0.0
  ![descargamos la imagen](images/04.png)
* sudo podman images
  ![Lo comprobamos](images/05.png)

### Crearemos y comprobaremos el contenedor
Ahora crearemos el contenedor y lo comprobaremos con los siguientes comandos

* sudo podman volume create oracle_23ai_datos
* sudo podman run -d --name cont-oracle -p 1521:1521 --restart=always -v oracle_23ai_datos:/opt/oracle/oradata:U container-registry.oracle.com/database/free:23.5.0.0
* sudo podman ps
* sudo podman ps -a
* sudo podman logs cont-oracle

### Entramos en Oracle y creamos el usuario de trabajo
Para ello ejecutaremos los siguientes comandos en la terminal

* sudo podman exec -it cont-oracle/bin/bash
* exit
* sudo podman exec -it cont-oracle sqlplus / as sysdba

##### Dentro de SQL, comprobaremos la PDB y crearemos el usuario de FREEPDB1:

* SHOW PDBS
* ALTER SESSION SET CONTAINER = FREEPDB1:
* CREATE USER orauser IDENTIFIED BY "TU_CLAVE";
* ALTER USER orauser QUOTA 100M ON USERS;
* EXIT

- Donde sustituiremos TU_CLAVE por una contraseña propia.

Por último comprobaremos la conexión como usuario de trabajo con el siguiente comando:
* sudo podman exec -it cont-oracle sqlplus orauser@//localhost:1521/FREEPDB1


### Desplegar PostgreSQL

##### Primero instalaremos PostgreSQL

