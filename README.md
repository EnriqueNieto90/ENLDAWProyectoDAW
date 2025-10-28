
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
    - [Instalación de PHP 8.3-FPM](#instalación-de-php-83-fpm)
      - [Añadir el repositorio PPA](#añadir-el-repositorio-ppa)
    - [1.2. Instalar los paquetes](#12-instalar-los-paquetes)
    - [Configuración de Apache2](#configuración-de-apache2)
      - [Desactivar Módulos Antiguos](#desactivar-módulos-antiguos)
      - [Activar Módulos Nuevos](#activar-módulos-nuevos)
      - [Activar la Configuración de PHP-FPM](#activar-la-configuración-de-php-fpm)
      - [Reiniciar Apache](#reiniciar-apache)
    - [Configuración de PHP-FPM](#configuración-de-php-fpm)
      - [Archivos de Configuración Clave](#archivos-de-configuración-clave)
      - [Configuración del Pool (`www.conf`)](#configuración-del-pool-wwwconf)
      - [Ajustar `php.ini` para Desarrollo](#ajustar-phpini-para-desarrollo)
      - [Reiniciar el Servicio FPM](#reiniciar-el-servicio-fpm)
    - [Verificación del Sistema](#verificación-del-sistema)
      - [Comprobar Estado de los Servicios](#comprobar-estado-de-los-servicios)
      - [Comprobar Módulo MPM de Apache](#comprobar-módulo-mpm-de-apache)
      - [Comprobar el Socket de FPM](#comprobar-el-socket-de-fpm)
      - [Prueba final con `phpinfo()`](#prueba-final-con-phpinfo)
    - [(Avanzado) Métodos de Configuración Alternativos](#avanzado-métodos-de-configuración-alternativos)
      - [Opción A: `ProxyPassMatch`](#opción-a-proxypassmatch)
      - [Opción B: `SetHandler` (dentro de `FilesMatch`)](#opción-b-sethandler-dentro-de-filesmatch)
      - [1.1.4 MariaDB](#114-mariadb)
  - [Instalación](#instalación-1)
    - [1. Verificar la instalación existente](#1-verificar-la-instalación-existente)
    - [2. Instalar MariaDB](#2-instalar-mariadb)
  - [Configuración](#configuración)
    - [1. Editar el fichero de configuración](#1-editar-el-fichero-de-configuración)
    - [2. Añadir la configuración](#2-añadir-la-configuración)
      - [1.1.5 XDebug](#115-xdebug)
  - [Instalación](#instalación-2)
    - [1. Verificar la instalación existente](#1-verificar-la-instalación-existente-1)
    - [2. Instalar Xdebug](#2-instalar-xdebug)
  - [Configuración](#configuración-1)
    - [1. Editar el fichero de configuración](#1-editar-el-fichero-de-configuración-1)
    - [2. Añadir la configuración](#2-añadir-la-configuración-1)
  - [Reiniciar Servicios](#reiniciar-servicios)
    - [Opción 1: Reiniciar Apache](#opción-1-reiniciar-apache)
    - [Opción 2: Reiniciar PHP-FPM](#opción-2-reiniciar-php-fpm)
      - [1.1.6 Servidor web seguro (HTTPS)](#116-servidor-web-seguro-https)
      - [1.1.7 DNS](#117-dns)
      - [1.1.8 SFTP](#118-sftp)
      - [1.1.9 Apache Tomcat](#119-apache-tomcat)
      - [1.1.10 LDAP](#1110-ldap)
    - [1.2 Windows 11](#12-windows-11)
      - [**Conexión al servidor desde Windows**](#conexión-al-servidor-desde-windows)
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
### Instalación de PHP 8.3-FPM

Estos pasos instalan PHP 8.3 y FPM usando el repositorio PPA de Ondřej Surý, que proporciona las versiones más actualizadas.

#### Añadir el repositorio PPA

Primero, actualizamos el sistema e instalamos las herramientas para gestionar repositorios.

```
# Actualizar la lista de paquetes
sudo apt update

# Instalar herramientas para añadir repositorios
sudo apt install software-properties-common -y
````

Añadimos el PPA de Ondřej Surý para PHP.

```
# Añadir el PPA
sudo add-apt-repository ppa:ondrej/php -y

# Opcional: Verificar que el repositorio se añadió
ls /etc/apt/sources.list.d/ | grep ondrej
```



### 1.2. Instalar los paquetes

Actualizamos la lista de paquetes nuevamente tras añadir el PPA e instalamos PHP 8.3, FPM y el módulo de Apache (necesario para las dependencias y archivos de configuración).

```
# Actualizar la lista de paquetes con el nuevo PPA
sudo apt update

# Instalar PHP 8.3, FPM y el módulo de Apache
sudo apt install php8.3-fpm php8.3 libapache2-mod-php8.3 -y
```



-----

### Configuración de Apache2

Ahora, configuraremos Apache para que deje de usar el módulo PHP tradicional (`mod_php`) y en su lugar redirija las peticiones a PHP-FPM.

#### Desactivar Módulos Antiguos

Desactivamos `mpm_prefork` (el módulo de multiprocesamiento antiguo, incompatible con FPM) y el módulo `php8.3` (`mod_php`).

```
# Desactivar mpm_prefork
sudo a2dismod mpm_prefork

# Desactivar el módulo PHP tradicional
sudo a2dismod php8.3
```

#### Activar Módulos Nuevos

Activamos `mpm_event` (el módulo de multiprocesamiento moderno y eficiente), `proxy_fcgi` (para actuar como proxy FastCGI) y `setenvif` (usado por la configuración de FPM).

```
# Activar mpm_event
sudo a2enmod mpm_event

# Activar los módulos de proxy FastCGI y setenvif
sudo a2enmod proxy_fcgi setenvif
```

#### Activar la Configuración de PHP-FPM

El paquete `php8.3-fpm` incluye un archivo de configuración listo para Apache. Solo necesitamos activarlo.

```
sudo a2enconf php8.3-fpm
```

Este comando activa el archivo `/etc/apache2/conf-available/php8.3-fpm.conf`, que generalmente contiene lo siguiente:

```
<FilesMatch ".+\.ph(?:ar|p|tml)$">
    SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost"
</FilesMatch>
```

**Explicación de este bloque:**

  * `<FilesMatch ".+\.ph(?:ar|p|tml)$">`: Aplica esta regla a cualquier archivo que termine en `.php`, `.phtml` o `.phar`.
  * `SetHandler "..."`: Indica a Apache cómo manejar estos archivos.
  * `proxy:unix:/run/php/php8.3-fpm.sock`: Esta es la parte clave.
      * `proxy`: Le dice a Apache que use `mod_proxy`.
      * `unix:/run/php/php8.3-fpm.sock`: Especifica que debe conectarse a PHP-FPM a través de un **Socket UNIX** local (un archivo especial en el sistema). Esta es la configuración por defecto y la más eficiente si Apache y PHP están en la misma máquina.
      * `|fcgi://localhost`: Define el protocolo a usar (FastCGI).

#### Reiniciar Apache

Aplicamos todos los cambios reiniciando el servicio de Apache.

```
sudo systemctl restart apache2
```



-----

### Configuración de PHP-FPM

El servicio PHP-FPM también tiene sus propios archivos de configuración, que definen *cómo* se ejecutan los procesos PHP.

#### Archivos de Configuración Clave

La configuración de FPM se encuentra principalmente en `/etc/php/8.3/fpm/`:

  * `/etc/php/8.3/fpm/php.ini`: Configuración de PHP específica para FPM (diferente de `/etc/php/8.3/cli/php.ini`). Aquí se ajustan directivas como `memory_limit`, `upload_max_filesize`, etc.
  * `/etc/php/8.3/fpm/php-fpm.conf`: Configuración general del servicio FPM.
  * `/etc/php/8.3/fpm/pool.d/`: Directorio que contiene las configuraciones de los "pools" de procesos.
  * `/etc/php/8.3/fpm/pool.d/www.conf`: Es el pool por defecto.

#### Configuración del Pool (`www.conf`)

El archivo `www.conf` define cómo se comporta el pool de procesos principal. Los parámetros más importantes son:

  * `[www]`: El nombre del pool.
  * `user` y `group`: El usuario y grupo con el que se ejecutarán los procesos PHP (usualmente `www-data`).
  * `listen`: Define cómo escucha FPM. Es crucial que esto coincida con la configuración de Apache.
      * **Socket UNIX (Por defecto):** `listen = /run/php/php8.3-fpm.sock`
      * **Socket TCP:** `listen = 127.0.0.1:9000` (Útil si FPM está en otra máquina).
  * `pm`: El gestor de procesos.
      * `dynamic`: (Por defecto) Crea y destruye procesos según la carga.
      * `static`: Mantiene un número fijo de procesos.
      * `ondemand`: Crea procesos solo cuando se necesitan.
  * Otras directivas `pm.*`: `pm.max_children`, `pm.start_servers`, etc., que controlan cuántos procesos se pueden ejecutar.

#### Ajustar `php.ini` para Desarrollo

Para un entorno de desarrollo, es útil modificar el `php.ini` de FPM para mostrar errores y aumentar los límites de subida.

Primero, hacemos una copia de seguridad:

```
sudo cp /etc/php/8.3/fpm/php.ini /etc/php/8.3/fpm/php.ini.backup
```

Luego, editamos el archivo:

```
sudo nano /etc/php/8.3/fpm/php.ini
```

Busca (con `Ctrl+W` en nano) y modifica las siguientes directivas (ejemplos):

```
display_errors = On
memory_limit = 256M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 300
```

#### Reiniciar el Servicio FPM

Para que los cambios en `php.ini` o en la configuración del pool surtan efecto, reinicia el servicio PHP-FPM.

```
sudo systemctl restart php8.3-fpm
```

-----

### Verificación del Sistema

Comprobemos que todos los servicios están funcionando correctamente.

#### Comprobar Estado de los Servicios

Verifica que tanto Apache como PHP-FPM estén activos (`active (running)`).

```
sudo systemctl status apache2
sudo systemctl status php8.3-fpm
```

[Insertar captura de pantalla del estado de los servicios aquí]

#### Comprobar Módulo MPM de Apache

Confirma qué módulo de multiprocesamiento (MPM) está usando Apache. Debería ser `mpm_event_module`.

```
apache2ctl -M | grep mpm
```

**Resultado esperado:**

```
 mpm_event_module (shared)
```

#### Comprobar el Socket de FPM

Verifica cómo está escuchando PHP-FPM para asegurarte de que coincide con la configuración de Apache.

```
grep '^listen' /etc/php/8.3/fpm/pool.d/*.conf
```

**Resultado esperado (Socket UNIX):**

```
/etc/php/8.3/fpm/pool.d/www.conf:listen = /run/php/php8.3-fpm.sock
```

**Resultado alternativo (Socket TCP):**

```
/etc/php/8.3/fpm/pool.d/www.conf:listen = 127.0.0.1:9000
```

#### Prueba final con `phpinfo()`

Crea un archivo de prueba en el directorio raíz de tu servidor web.

```
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Ahora, visita `http://<tu_direccion_ip>/info.php` en tu navegador.

Busca la línea **"Server API"**. Debería mostrar **"FPM/FastCGI"**. Esto confirma que Apache está pasando la petición a PHP-FPM correctamente.

[Insertar captura de pantalla de phpinfo() mostrando 'FPM/FastCGI' aquí]

**¡Importante\!** Borra este archivo después de verificar, ya que expone información sensible de tu servidor.

```
sudo rm /var/www/html/info.php
```

-----

### (Avanzado) Métodos de Configuración Alternativos

Si no deseas usar la configuración global (`a2enconf php8.3-fpm`), puedes configurar el proxy manualmente dentro de tu archivo de VirtualHost (ej: `/etc/apache2/sites-available/000-default.conf`).

#### Opción A: `ProxyPassMatch`

Esta directiva usa una expresión regular para redirigir las peticiones.

**Si FPM escucha en Socket UNIX:**

```
<VirtualHost *:80>
    DocumentRoot /var/www/html
    # ... otras directivas ...

    ProxyPassMatch ^/(.*\.php)$ unix:/run/php/php8.3-fpm.sock|fcgi://127.0.0.1/var/www/html
</VirtualHost>
```

**Si FPM escucha en Socket TCP (ej: 127.0.0.1:9000):**

```
<VirtualHost *:80>
    DocumentRoot /var/www/html
    # ... otras directivas ...
    
    # Nota el $1 al final para pasar el nombre del archivo
    ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/html/$1
</VirtualHost>
```

#### Opción B: `SetHandler` (dentro de `FilesMatch`)

Esta es otra forma de lograr lo mismo, considerada por algunos más limpia.

**Si FPM escucha en Socket UNIX:**

```
<VirtualHost *:80>
    DocumentRoot /var/www/html
    # ... otras directivas ...
    
    <FilesMatch "\.php$">
        SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>
```

**Si FPM escucha en Socket TCP (ej: 127.0.0.1:9000):**

```
<VirtualHost *:80>
    DocumentRoot /var/www/html
    # ... otras directivas ...
    
    <FilesMatch "\.php$">
        SetHandler "proxy:fcgi://127.0.0.1:9000"
    </FilesMatch>
</VirtualHost>
```
#### 1.1.4 MariaDB
Documentación para la instalación y configuración de MariaDB en un entorno PHP 8.3.

---

## Instalación

### 1. Verificar la instalación existente

Primero, comprueba si MariaDB ya está instalado ejecutando el siguiente comando en tu terminal:
```
sudo php -m | grep mariadb
```

Si este comando muestra información sobre MariaDB, puedes pasar directamente a la sección de configuración.

### 2. Instalar MariaDB

Si el comando anterior no mostró resultados, instala MariaDB con:
```
sudo apt install mariadb-server -y
```

## Configuración

Una vez instalado, MariaDB necesita ser configurado.

### 1. Editar el fichero de configuración

Abre el archivo de configuración de . La ruta se encuentra en:
```
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

### 2. Añadir la configuración

Localiza la línea bind-address = 127.0.0.1 y cámbiala a :
```
bind-address = 0.0.0.0
```

Guarda los cambios.
```
sudo systemctl restart mariadb
```

#### 1.1.5 XDebug

Documentación para la instalación y configuración de Xdebug en un entorno PHP 8.3.

---

## Instalación

### 1. Verificar la instalación existente

Primero, comprueba si Xdebug ya está instalado ejecutando el siguiente comando en tu terminal:
```
sudo php -m | grep xdebug
```

Si este comando muestra información sobre Xdebug, puedes pasar directamente a la sección de configuración.

### 2. Instalar Xdebug

Si el comando anterior no mostró resultados, instala Xdebug para PHP 8.3:
```
sudo apt install php8.3-xdebug
```

## Configuración

Una vez instalado, Xdebug necesita ser configurado.

### 1. Editar el fichero de configuración

Abre el archivo de configuración de Xdebug. La ruta se encuentra en:
```
sudo nano /etc/php/8.3/fpm/conf.d/20-xdebug.ini
```

### 2. Añadir la configuración

Añade las siguientes líneas al final del archivo `20-xdebug.ini`:
```
xdebug.mode=develop,debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
xdebug.log=/tmp/xdebug.log
xdebug.log_level=7
xdebug.idekey="netbeans-xdebug"
xdebug.discover_client_host=1
```

Guarda los cambios y cierra el editor.

## Reiniciar Servicios

Para que los cambios en la configuración surtan efecto, debes reiniciar el servidor web o el servicio PHP-FPM.

### Opción 1: Reiniciar Apache

Si usas Apache:
```
sudo systemctl restart apache2
```

### Opción 2: Reiniciar PHP-FPM

Si usas PHP-FPM:
```
sudo systemctl restart php8.3-fpm
```
#### 1.1.6 Servidor web seguro (HTTPS)
#### 1.1.7 DNS
#### 1.1.8 SFTP
#### 1.1.9 Apache Tomcat
#### 1.1.10 LDAP

### 1.2 Windows 11
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

