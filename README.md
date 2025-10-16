
# CFGS Desarrollo de Aplicaciones Web

|  CFGS DESARROLLO  DE APLICACIONES WEB |
|:-----------:|
|![Alt](images/portada.jpg)|
| DESPLIEGUE DE APLCIACIONES WEB
| CYBERSEGURIDAD
| DAWES Tema 2. INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DE ENTORNO DE DESARROLLO Y DEL ENTORNO DE EXPLOTACIÓN |


- [CFGS Desarrollo de Aplicaciones Web](#cfgs-desarrollo-de-aplicaciones-web)
  - [1. Entorno de Desarrollo](#1-entorno-de-desarrollo)
    - [1.1 Ubuntu Server 24.04.3 LTS](#11-ubuntu-server-24043-lts)
      - [1.1.1 **Configuración inicial**](#111-configuración-inicial)
        - [Nombre y configuración de red](#nombre-y-configuración-de-red)
        - [**Cambiar nombre del servidor**](#cambiar-nombre-del-servidor)
        - [**Actualizar el sistema**](#actualizar-el-sistema)
        - [**Configuración fecha y hora**](#configuración-fecha-y-hora)
        - [**Cuentas administradoras**](#cuentas-administradoras)
        - [**Habilitar cortafuegos**](#habilitar-cortafuegos)
      - [**Conexión al servidor desde Windows**](#conexión-al-servidor-desde-windows)
        - [**Comprobar IP, puerta de enlace y DNS**](#comprobar-ip-puerta-de-enlace-y-dns)
        - [**Particiones**](#particiones)
        - [**Actualización**](#actualización)
        - [**Comprobaciones sistema operativo**](#comprobaciones-sistema-operativo)
      - [1.1.2 Instalación del servidor web](#112-instalación-del-servidor-web)
        - [Instalación](#instalación)
        - [Verficación del servicio](#verficación-del-servicio)
        - [Virtual Hosts](#virtual-hosts)
        - [Permisos y usuarios](#permisos-y-usuarios)
      - [1.1.3 PHP](#113-php)
      - [1.1.4 MySQL](#114-mysql)
      - [1.1.5 XDebug](#115-xdebug)
      - [1.1.6 Servidor web seguro (HTTPS)](#116-servidor-web-seguro-https)
      - [1.1.7 DNS](#117-dns)
      - [1.1.8 SFTP](#118-sftp)
      - [1.1.9 Apache Tomcat](#119-apache-tomcat)
      - [1.1.10 LDAP](#1110-ldap)
    - [1.2 Windows 11](#12-windows-11)
      - [1.2.1 **Configuración inicial**](#121-configuración-inicial)
        - [**Nombre y configuración de red**](#nombre-y-configuración-de-red-1)
        - [**Cuentas administradoras**](#cuentas-administradoras-1)
      - [1.2.2 **Navegadores**](#122-navegadores)
      - [1.2.3 **FileZilla**](#123-filezilla)
      - [1.2.4 **Netbeans**](#124-netbeans)
    - [Información General e Instalación](#información-general-e-instalación)
    - [Creación de un Proyecto PHP desde un Servidor Remoto](#creación-de-un-proyecto-php-desde-un-servidor-remoto)
    - [Eliminación de un Proyecto](#eliminación-de-un-proyecto)
      - [1.2.5 **Visual Studio Code**](#125-visual-studio-code)
  - [2. GitHub](#2-github)
  - [3.Entorno de Explotación](#3entorno-de-explotación)

## 1. Entorno de Desarrollo

### 1.1 Ubuntu Server 24.04.3 LTS

Este documento es una guía detallada del proceso de instalación y configuración de un servidor de aplicaciones en Ubuntu Server utilizando Apache, con soporte PHP y MySQL

#### 1.1.1 **Configuración inicial**

##### Nombre y configuración de red

> **Nombre de la máquina**: daw-used\
> **Memoria RAM**: 2G\
> **Particiones**: 150G(/) y resto (/var)\
> **Configuración de red interface**: enp0s3 \
> **Dirección IP** :10.199.9.184/22\
> **GW**: 10.199.8.1/22\
> **DNS**: 10.151.123.21 10.151.126.21

- Hacemos una copia del archivo por defecto 50-cloud-init.yaml

````
cd /etc/netplan
sudo cp 50-cloud-init.yaml enp0s3.yaml
````

- Editar el fichero de configuración del interface de red  **/etc/netplan**,

```bash

# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      addresses:
       - 10.199.9.184/22
      nameservers:
         addresses:
         - 10.151.123.21
         - 10.151.126.21
         search: [educa.jcyl.es]
         routes:
            - to: default
              via: 10.199.8.1
  version: 2
````

- Actualizar la configuración de red
  
````
sudo netplan apply
````

##### **Cambiar nombre del servidor**

````
sudo hostnamectl set-hostname enl-used
````

- Cambiamos el nombre en el archivo hosts y comprobamos el cambio con un cat

````
sudo nano /etc/hosts
cat /etc/hosts
````

##### **Actualizar el sistema**

```bash
sudo apt update
sudo apt upgrade
```

##### **Configuración fecha y hora**

[Establecer fecha, hora y zona horaria](https://somebooks.es/establecer-la-fecha-hora-y-zona-horaria-en-la-terminal-de-ubuntu-20-04-lts/ "Cambiar fecha y hora")

````
sudo timedatectl set-timezone Europe/Madrid
````

##### **Cuentas administradoras**

> - [X] root(inicio)
> - [X] miadmin/paso
> - [X] miadmin2/paso

- Creamos el usuario miadmin2 como administrador (miadmin se crea al instalar UbuntuServer)
  
````
sudo usermod -aG sudo miadmin2
````
- Para crear nuevos miadmin y que estén en los mismos grupos que miadmin
````
sudo useradd -m -G sudo,adm,cdrom,dip,plugdev,lxd -s /bin/bash miadmin3
````


##### **Habilitar cortafuegos**

- Activamos el cortafuegos

````
sudo ufw enable
````

- Abrimos el puerto 22

````
sudo ufw allow 22
````

- Miramos los puertos que están abiertos (sin número y con número)

````
sudo ufw status
sudo ufw status numbered
````

- Borramos el puerto utilizando su número

````
sudo ufw delete [numPuerto]
````

#### **Conexión al servidor desde Windows**

- Arrancamos el servicio ssh en el servidor para permitir la conexión
````
sudo systemctl start ssh
````
- Comprobamos que está en active (running)
````
sudo systemctl status ssh
````
- Si no está en active (running) lo habilitamos
````
sudo systemctl enable ssh
````
- Para conectarnos a nuestro servidor desde nuestro anfitrión: Abrimos la consola de windows (símbolo del sistema) y escribimos el comando ssh con nuestro nombre de usuario @ y la ip de nuestro servidor. Nos pedirá la clave.
````
ssh miadmin@10.10.199.9.184
````

##### **Comprobar IP, puerta de enlace y DNS**
- Para ver la IP: el nombre de nuestro adaptador de red (enp0s3). Si es dinámica veremos dynamic en la misma línea. Si es estática no veremos nada.
````
ip a
````
- Para ver la puerta de enlace, en la primera línea observaremos la puerta de enlace y el nombre de la tarjeta de red
````
ip r
````
- Para consultar los DNS: en DNS Servers se ve cuáles hay configurados. También vemos a que dominio pertenecemos en DNS Domain
````
resolvectl
````
##### **Particiones**
- Podemos utilizar los dos comandos para ver qué particiones hay y de qué tamaño son. El primero nos proporciona más información del tamaño usado por cada partición.
````
df -h
lsblk
````
##### **Actualización**
- Para comprobar si hay actualizaciones y a continuación que actualice todas las actualizaciones disponibles
````
sudo apt update
sudo apt upgrade
````
##### **Comprobaciones sistema operativo**
- Tipo de sistema operativo
````
uname -a
````
- Ver procesos
````
ps -ef
````

#### 1.1.2 Instalación del servidor web

##### Instalación
- Abrir puerto 80, comprobamos y desactivamos el 80(v6)
````
sudo ufw allow 80
sudo ufw status numbered
sudo ufw delete 3
````
- Instalamos apache
````
sudo apt update
sudo apt install apache2
````
##### Verficación del servicio
- Comprobamos que el servicio esta en ejecucion (running)
````
sudo systemctl status apache2
````
- Comprobamos ubicacion de la carpeta y los archivos web
````
cd /var/www/html
ls
````
##### Virtual Hosts
##### Permisos y usuarios
- Creamos usuario del dominio para administrar la web.
  - Nombre: operadorweb/paso
  - directorio de trabajo: /var/www/html 
  - grupo:www-data
  - shell:/bin/bash
  
````
sudo useradd -M -d /var/www/html -N -g www-data -s /bin/bash operadorweb
````
- Cambiamos la contraseña (paso)
````
sudo passwd operadorweb
````
- Cambiamos el propietario de la carpeta html y el grupo
````
sudo chown -R operadorweb:www-data /var/www/html
````
- Cambiamos los permisos de la carpeta html
````
sudo chmod -R 775 /var/www/html
````
- Para crear un administrador de la web y cambiar propietario, permisos y crear contraseña en una sola línea
````
sudo adduser --home /vaar/www/html --ingroup www-data --shell /bin/bash operadorweb
````

#### 1.1.3 PHP
#### 1.1.4 MySQL
#### 1.1.5 XDebug
#### 1.1.6 Servidor web seguro (HTTPS)
#### 1.1.7 DNS
#### 1.1.8 SFTP
#### 1.1.9 Apache Tomcat
#### 1.1.10 LDAP

### 1.2 Windows 11
#### 1.2.1 **Configuración inicial**
##### **Nombre y configuración de red**
##### **Cuentas administradoras**
#### 1.2.2 **Navegadores**
#### 1.2.3 **FileZilla**
#### 1.2.4 **Netbeans**

### Información General e Instalación

Antes de comenzar, asegurarse de tener la versión correcta del IDE.

* **IDE:** Apache NetBeans
* **Versión Recomendada:** 20
* **Página Oficial:** [https://netbeans.apache.org/](https://netbeans.apache.org/)
* **Descarga Directa (Versión 20):** [https://netbeans.apache.org/front/main/download/nb20/](https://netbeans.apache.org/front/main/download/nb20/)


### Creación de un Proyecto PHP desde un Servidor Remoto

Este proceso configura un proyecto en el ordenador (local) que se sincroniza con una carpeta en un servidor remoto.

**Paso 1: Iniciar el Asistente de Nuevo Proyecto**

* Haz clic en el menú `File > New Project...`
* O pulsa en el icono de la carpeta naranja con un símbolo `+` en la barra de herramientas.

**Paso 2: Seleccionar el Tipo de Proyecto**

1.  En la ventana "Choose Project", selecciona la categoría **PHP**.
2.  En la lista de "Projects", elige **PHP Application from Remote Server**.
3.  Haz clic en **Next**.

![Alt](images/phpRemoteServer.png)

**Paso 3: Nombre y Ubicación Local**

1.  **Project Name:** Asigna un nombre descriptivo a tu proyecto (ej: `ENLDWESProyectoTema5`).
2.  **Sources Folder:** Cambia la ruta por defecto a tu carpeta de trabajo personal donde se guardarán los archivos en **tu ordenador**(ej: `D:\ProyectosNB\ENLDWESProyectoTema5`).
3.  Haz clic en **Next**.

![Alt](images/NameAndLocation.png)

**Paso 4: Configuración de la Conexión Remota (SFTP)**

Esta es la parte más importante, donde se enlaza el proyecto local con el servidor.

1.  **Project URL:** Introduce la URL con la que accederás al proyecto en un navegador (ej: `http://10.199.9.184`).
2.  **Remote Connection:** Haz clic en el botón **Manage...** para configurar los detalles de la conexión SFTP por primera vez.

![Alt](images/RemoteConnection.png)

**Dentro de la ventana "Manage Remote Connections":**

1.  Haz clic en **Add...**
2.  Rellena los datos de la conexión:
    * **Connection Name:** Un nombre para identificar esta conexión (ej: `enl-used`).
    * **Connection Type:** Selecciona **SFTP**.
    * **Host Name:** La dirección IP del servidor remoto.
    * **Port:** El puerto (normalmente `22` para SFTP).
    * **User Name:** Tu nombre de usuario con permisos en el servidor.
    * **Password:** Tu contraseña.
3.  Haz clic en **Test Connection**. Si todo es correcto, verás un mensaje de "Connection successful". Acepta el mensaje.
4.  Haz clic en **OK** para guardar la configuración de la conexión.

![Alt](images/ManageConnection.png)

**De vuelta en la ventana del asistente:**

5.  **Upload Directory:** Especifica la ruta de la carpeta del proyecto **en el servidor**.
    * Si el proyecto está en la raíz del servidor, puedes poner solo `/`.
    * Si está en una carpeta, la ruta sería algo como `/ENLDWESProyectoTema5`.
6.  Haz clic en **Next**. Es posible que aparezca un mensaje de confirmación de la conexión, haz clic en **Yes**.

![Alt](images/RemoteConnection.png)

> **Nota importante:** Para que la conexión inicial funcione, la carpeta del proyecto ya debe existir en el servidor y es recomendable que contenga al menos un archivo (como un `index.php` vacío).

**Paso 5: Sincronización y Finalización**

1.  El asistente te mostrará los archivos que existen en la carpeta remota y te preguntará cuáles quieres descargar a tu carpeta local. Selecciona los que necesites.
2.  Haz clic en **Finish**. Acepta cualquier otro mensaje de confirmación que aparezca.

![Alt](images/Confirmation.png)

**Paso 6: Proyecto Creado**

El proyecto aparecerá en el panel izquierdo del IDE ("Projects"), y los archivos locales estarán sincronizados con los del servidor.

![Alt](images/Creation.png)

---

### Eliminación de un Proyecto

Para eliminar un proyecto de NetBeans:

1.  En el panel "Projects", haz clic derecho sobre el nombre del proyecto que quieres eliminar.
2.  Selecciona la opción **Delete**.

![Alt](images/Delete.png)

3.  Aparecerá un cuadro de diálogo de confirmación.
    * **Importante:** Si marcas la casilla `Also delete sources under [ruta del proyecto]`, se borrarán permanentemente los archivos del proyecto de **tu ordenador local**.
    * Haz clic en **Yes** para confirmar.

![Alt](images/DeleteConfirm.png)

> Este proceso **solo elimina el proyecto de NetBeans y (opcionalmente) los archivos locales**. Los archivos en el **servidor remoto no se ven afectados** y, si quieres eliminarlos, hay que hacerlo manualmente.


#### 1.2.5 **Visual Studio Code**

## 2. GitHub
## 3.Entorno de Explotación

---

> **Enrique Nieto Lorenzo**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  

