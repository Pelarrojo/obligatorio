README:

# Taller Julio 2024

### Obligatorio - Repositorio del Taller de Linux, Julio de 2024

Este repositorio contiene los PLaybooks necesarios en Ansible para desplegar los siguientes servicios:

CentOS 9 Stream:
- Una Aplicación Web llamada “Todo” anteriormente utilizada en el semestre pasado con su respectiva configuración a la Base de Datos. 
- Instalación de OpenJDK 11 de Java.
- Instalación de Tomcat.
- Apertura de puerto 8080/tcp en firewalld.

Ubuntu:
- Instalación de Base de Datos con su configuración correspondiente para la App "Todo"
- Instalación del Servidor "MariaDB".
- Apertura de puerto 3306/tcp en UFW.

Se incluye además la habilitación necesaria de cada uno de los Servicios en el Firewall.

---
# Playbook webserver

#### A continuación se detallan los pasos necesarios para desplegar la aplicación web:

- Instalación de OpenJDK 11.
- Creación del directorio /opt/tomcat para alojar la webapp.
- Creación de usuario y grupo “tomcat”.
- Descarga de “apache-tomcat v10.1.26”.
- Extracción de “apache-tomcat-10.1.26.tar.gz” y copia al directorio “/opt/tomcat”.
- Apertura de puerto 8080/tcp en firewalld para el acceso a través de la web.
- Creación del archivo para servicio en systemd para Tomcat.
- Cambio de propietario y grupo “tomcat:tomcat /opt/tomcat”.
- Iniciar y habilitar el servicio tomcat.service.
- Reinicio de firewalld.

# Playbook database

#### Los pasos para el despliegue de la base de datos se detallan a continuación:

- Instalación de UWF como firewall.
- Habilitar firewall.
- Instalación de mariadb-server.
- Configuración del servicio para que escuche en todas las interfaces 'bind-address=0.0.0.0'.
- Verificación del servicio mariadb ejecutándose.
- Apertura de puerto 3306/tcp en el firewall.
- Creación de la base de datos
- Importar base de datos desde “./files/todo.sql”
- Reinicio del servicio mariadb

# Requisitos

- Se requieren al menos 2 servidores para realizar el despliegue
- Se recomiendan tres equipos desplegados de la siguiente manera:
 
  -Servidor CentOS con rol de “Controller” que utilizará Ansible
 
  -Servidor Ubuntu donde se desplegará la base de datos

  -Servidor CentOS que será utilizara para la aplicación web en Tomcat

- Se requiere la creación de un usuario ansible con permisos de sudo.
- Copiado de claves SSH a los servidores Centos y Ubuntu donde se desplegará la aplicación web y base de datos. 

  Se hace con los siguientes comandos:
```
ssh-copy-id 192.168.56.113
```
```
ssh-copy-id 192.168.56.105
```

- En el servidor Centos (Controller) se requiere instalar:
```
dnf install python3-pip
```
```
pip install pipx
```
```
pipx ensurepath
```
```
pipx install ansible-core
```
```
pipx inject ansible-core argcomplete
```
```
pipx inject ansible-core ansible-lint
```
```
activate-global-python-argcomplete --user
```
# Ejecución

#### Para la ejecución del Playbook Webserver, se hace de la siguiente manera:

```
ansible-playbook -i inventory/servidores.toml webserver.yml --ask-become-passs
```
#### Para ejecutar el Playbook database, se hace de la siguiente manera:
```
ansible-playbook -i inventory/servidores.toml database.yml --ask-become-pass
```
Una vez que comienza la ejecucion, se muestran en pantalla los pasos mencionados anteriormente y su correspondiente resultado de ejecución. 
El Playbook finaliza su ejecución obteniendo como resultado un resumen de las tareas realizadas y su estado.

## Distribuciones soportadas

#### Los Playbooks mencionados fueron ejecutados y puesta en funcionamiento en las siguientes distribuciones:

* RedHat/CentOS Stream 9
* Ubuntu 24.04 LTS

La utilizacion de este directorio/playbooks sera responsabilidad del consumidor si se hace uso de lo anteriormente nombrado en distribuciones que no se detallaron anteriormente.