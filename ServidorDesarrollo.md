[< Volver al menú principal](README.md)

# SERVIDOR DE DESARROLLO - UBUNTU SERVER 24.04.3 LTS

| CFGS Desarrollo de Aplicaciones Web |
|:-----------------------------------:|
| ![Portada](images/portada.jpg)      |
| **Instalación, Configuración y Documentación del Entorno de Desarrollo** |

> **Autor:** Enrique Nieto Lorenzo
> **Curso:** 2024/2025  
> **Módulo:** Despliegue de Aplicaciones Web / Ciberseguridad  
> **Centro:** IES Los Sauces

---

## Índice

- [1. Configuración Inicial del Sistema](#1-configuración-inicial-del-sistema)
  - [1.1 Instalación del Sistema Operativo](#11-instalación-del-sistema-operativo)
  - [1.2 Configuración de Red](#12-configuración-de-red)
  - [1.3 Usuarios y Permisos](#13-usuarios-y-permisos)
  - [1.4 Seguridad Básica](#14-seguridad-básica)
- [2. Servidor Web (Apache)](#2-servidor-web-apache)
- [3. Intérprete PHP (PHP-FPM)](#3-intérprete-php-php-fpm)
- [4. Base de Datos (MariaDB)](#4-base-de-datos-mariadb)
- [5. Herramientas de Administración](#5-herramientas-de-administración)
- [6. Herramientas de Desarrollo](#6-herramientas-de-desarrollo)
- [7. Servicios Adicionales](#7-servicios-adicionales)
- [8. Comandos de Referencia Rápida](#8-comandos-de-referencia-rápida)
- [9. Solución de Problemas Comunes](#9-solución-de-problemas-comunes)

---

## Introducción

Este documento es una guía completa para la instalación y configuración de un servidor de desarrollo basado en Ubuntu Server 24.04.3 LTS. El entorno resultante estará optimizado para el desarrollo de aplicaciones web con la pila LAMP (Linux, Apache, MySQL/MariaDB, PHP).

El servidor incluye herramientas esenciales para el desarrollo profesional de aplicaciones web, tales como depuración con XDebug, gestión de bases de datos con phpMyAdmin, transferencia segura de archivos mediante SFTP, y documentación automática con PHPDocumentor.

---

## 1. Configuración Inicial del Sistema

La configuración inicial del sistema establece las bases para un entorno de desarrollo seguro y funcional. Esta sección cubre desde la instalación del sistema operativo hasta la configuración de seguridad básica, incluyendo red, usuarios y cortafuegos.

### 1.1 Instalación del Sistema Operativo

#### Requisitos de Hardware

Para garantizar un rendimiento adecuado del servidor de desarrollo, se recomienda la siguiente configuración mínima:

| Componente | Especificación | Justificación |
|------------|----------------|---------------|
| **Hipervisor** | VirtualBox 7.0+ | Plataforma de virtualización gratuita y multiplataforma |
| **Memoria RAM** | 2 GB | Suficiente para Apache, PHP-FPM y MariaDB en desarrollo |
| **Procesadores** | 2 cores | Permite procesamiento paralelo de peticiones |
| **Almacenamiento** | 500 GB (dinámico) | Espacio para proyectos, bases de datos y logs |
| **Adaptador de Red** | Adaptador puente | Permite acceso desde el equipo host y otros dispositivos en la red |

#### Configuración de la Máquina Virtual en VirtualBox

Antes de comenzar la instalación del sistema operativo, configuramos la máquina virtual:

**Paso 1: Crear nueva máquina virtual**

1. Abrir VirtualBox
2. Click en "Nueva"
3. Nombre: `daw-used` (o el nombre que prefieras)
4. Tipo: Linux
5. Versión: Ubuntu (64-bit)

<!-- Captura sugerida: ventana de creación de nueva máquina virtual en VirtualBox -->

**Paso 2: Asignar memoria RAM**

Configurar 2048 MB (2 GB) de RAM.

<!-- Captura sugerida: deslizador de memoria RAM configurado en 2048 MB -->

**Paso 3: Crear disco duro virtual**

1. Seleccionar "Crear un disco duro virtual ahora"
2. Tipo: VDI (VirtualBox Disk Image)
3. Almacenamiento: Reservado dinámicamente
4. Tamaño: 500 GB

El almacenamiento dinámico significa que el archivo del disco solo ocupará el espacio realmente utilizado.

<!-- Captura sugerida: configuración del disco duro virtual con tamaño 500GB y tipo dinámico -->

**Paso 4: Configurar procesadores y red**

1. Ir a Configuración → Sistema → Procesador
2. Asignar 2 procesadores
3. Ir a Configuración → Red → Adaptador 1
4. Habilitar adaptador de red
5. Conectado a: Adaptador puente

<!-- Captura sugerida: configuración de red con adaptador puente seleccionado -->

#### Descarga de Ubuntu Server

Descargar la imagen ISO de Ubuntu Server 24.04.3 LTS desde:
```
https://releases.ubuntu.com/noble/
```

Seleccionar "Server install image" para la arquitectura AMD64.

#### Instalación de Ubuntu Server

**Paso 1: Iniciar la instalación**

1. En VirtualBox, seleccionar la máquina virtual creada
2. Configuración → Almacenamiento → Controlador IDE
3. Agregar la imagen ISO descargada
4. Iniciar la máquina virtual

<!-- Captura sugerida: pantalla de bienvenida del instalador de Ubuntu Server -->

**Paso 2: Selección de idioma y distribución de teclado**

1. Idioma: English (o el preferido)
2. Distribución de teclado: Spanish (o la correspondiente)

**Paso 3: Configuración de red**

Por ahora, dejar la configuración por DHCP. Más adelante configuraremos una IP estática.

**Paso 4: Configuración de proxy**

Dejar en blanco si no se utiliza proxy.

**Paso 5: Mirror de Ubuntu**

Mantener el mirror por defecto o seleccionar uno más cercano geográficamente.

**Paso 6: Particionado del disco**

Esta es una de las configuraciones más importantes. Crearemos dos particiones separadas:

**Esquema de particiones:**

| Partición | Tamaño | Punto de Montaje | Sistema de Archivos |
|-----------|--------|------------------|---------------------|
| Raíz      | 150 GB | `/`              | ext4                |
| Datos     | Resto (~350 GB) | `/var`    | ext4                |

**Justificación del particionado:**

- **`/` (raíz):** Contiene el sistema operativo base, aplicaciones instaladas y configuraciones. 150 GB es suficiente.
- **`/var`:** Almacena datos variables como logs, bases de datos y archivos web. Separar esta partición proporciona:
  - **Aislamiento:** Crecimiento descontrolado no afectará al sistema base.
  - **Facilidad de backup:** Se puede respaldar independientemente.
  - **Mejor rendimiento:** Accesos frecuentes no fragmentan el sistema base.

**Configuración del particionado:**

1. Seleccionar "Custom storage layout"
2. Seleccionar el disco disponible
3. Agregar GPT Partition:
   - Tamaño: 150GB
   - Formato: ext4
   - Punto de montaje: `/`
4. Agregar GPT Partition:
   - Tamaño: Usar espacio restante
   - Formato: ext4
   - Punto de montaje: `/var`

<!-- Captura sugerida: pantalla de particionado mostrando las dos particiones configuradas -->

**Paso 7: Configuración del perfil**

Configurar el primer usuario administrador del sistema:

- **Tu nombre:** [Nombre completo]
- **Nombre del servidor:** `daw-used`
- **Usuario:** `miadmin`
- **Contraseña:** `paso`

> **Nota de seguridad:** En entornos de producción usar contraseñas robustas. Para desarrollo educativo usamos contraseñas simples por conveniencia.

<!-- Captura sugerida: formulario de configuración del perfil con los datos completados -->

**Paso 8: Configuración SSH**

**Importante:** Marcar la opción "Install OpenSSH server"

Esto permite la conexión remota al servidor desde el equipo host.

<!-- Captura sugerida: pantalla de selección de OpenSSH server con la opción marcada -->

**Paso 9: Paquetes adicionales**

No seleccionar ningún paquete adicional. Instalaremos manualmente lo necesario después.

**Paso 10: Finalizar instalación**

Esperar a que se complete la instalación. Cuando se indique, reiniciar el sistema y extraer el medio de instalación (la ISO).

#### Primer Arranque y Verificación

Tras el reinicio, iniciar sesión con:
```
Usuario: miadmin
Contraseña: paso
```

Verificar la instalación:
```bash
# Ver información del kernel y arquitectura
uname -a

# Ver información detallada del sistema
hostnamectl

# Ver versión del sistema operativo
cat /etc/os-release
lsb_release -a

# Verificar particiones creadas
df -h
lsblk
fdisk -l
```

**Salida esperada de `df -h`:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       148G  2.1G  138G   2% /
/dev/sda3       345G  1.2G  326G   1% /var
```

**Salida esperada de `lsblk`:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk
├─sda1   8:1    0    1M  0 part
├─sda2   8:2    0  150G  0 part /
└─sda3   8:3    0  350G  0 part /var
```

### 1.2 Configuración de Red

Una configuración de red estática es esencial para un servidor de desarrollo, garantizando acceso consistente en la misma dirección IP.

#### Parámetros de Red

Los parámetros dependerán de tu infraestructura. Ejemplo para entorno educativo:

> **Hostname:** daw-used  
> **Interface:** enp0s3  
> **Dirección IP:** 10.199.10.22/22  
> **Gateway:** 10.199.8.1  
> **DNS Primario:** 10.151.123.21  
> **DNS Secundario:** 10.151.126.21  
> **Dominio de búsqueda:** educa.jcyl.es

**Nota:** En entorno doméstico, valores típicos serían:
- **IP:** 192.168.1.X/24
- **Gateway:** 192.168.1.1
- **DNS:** 8.8.8.8, 8.8.4.4

#### Comandos de Verificación de Red
```bash
# Ver todas las interfaces y direcciones IP
ip a

# Ver solo la IP asignada al hostname
hostname -I

# Ver la tabla de enrutamiento (incluye gateway)
ip r

# Ver la configuración DNS
resolvectl status

# Ver el nombre del servidor
hostname
```

#### Cambiar el Nombre del Servidor (si es necesario)
```bash
# Ver nombre actual
hostname
hostnamectl

# Cambiar hostname
sudo hostnamectl set-hostname daw-used

# Editar archivo hosts
sudo nano /etc/hosts
```

En el archivo `/etc/hosts`, modificar o agregar:
```
127.0.1.1    daw-used
```

Para que el cambio se refleje en el prompt, cerrar sesión y volver a iniciar:
```bash
exit
```

#### Configuración de IP Estática

Ubuntu Server 24.04 utiliza **Netplan** para la configuración de red. Los archivos están en `/etc/netplan/`.

**Paso 1: Identificar el archivo de configuración**
```bash
cd /etc/netplan
ls
```

Normalmente encontrarás `50-cloud-init.yaml` o similar.

**Paso 2: Crear copia de seguridad**
```bash
sudo cp 50-cloud-init.yaml 50-cloud-init.yaml.backup
```

**Paso 3: Renombrar el archivo (opcional)**
```bash
sudo mv 50-cloud-init.yaml 01-netcfg.yaml
```

**Paso 4: Editar la configuración**
```bash
sudo nano 01-netcfg.yaml
```

**Contenido para entorno educativo:**
```yaml
# This is the network config written by 'subiquity'
network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
        - 10.199.10.22/22
      nameservers:
        addresses:
          - 10.151.123.21
          - 10.151.126.21
        search: [educa.jcyl.es]
      routes:
        - to: default
          via: 10.199.8.1
```

**Contenido para entorno doméstico:**
```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
        - 192.168.1.100/24
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
      routes:
        - to: default
          via: 192.168.1.1
```

**Importante:** La indentación es crítica en YAML. Usar espacios, no tabulaciones.

<!-- Captura sugerida: archivo de configuración de netplan abierto en nano -->

**Paso 5: Aplicar la configuración**
```bash
sudo netplan apply
```

Si hay errores, Netplan los mostrará indicando la línea problemática.

**Paso 6: Verificar la configuración**
```bash
# Verificar la nueva IP
ip a

# Verificar el gateway
ip r

# Verificar DNS
resolvectl status

# Probar conectividad a internet
ping -c 4 8.8.8.8

# Probar resolución DNS
ping -c 4 google.com
```

**Paso 7: Probar conectividad desde el equipo host**

Desde el equipo anfitrión, abrir terminal y ejecutar:
```bash
ping 10.199.10.22
```

Si responde, la configuración es exitosa.

### 1.3 Usuarios y Permisos

La gestión adecuada de usuarios es fundamental para seguridad y organización, siguiendo el principio de mínimo privilegio.

#### Cuentas Administradoras

> - [X] **root** - Cuenta del sistema (no se usa directamente)
> - [X] **miadmin/paso** - Creado durante la instalación
> - [X] **miadmin2/paso** - Segundo administrador

**Justificación de múltiples administradores:**
- Redundancia en caso de problemas
- Auditoría de acciones
- Permite que diferentes personas administren el servidor

#### Creación de Usuario Administrador Adicional

**Método 1: Comando completo**
```bash
# Crear usuario con todos los grupos necesarios
sudo useradd -m -G sudo,adm,cdrom,dip,plugdev,lxd -s /bin/bash miadmin2

# Establecer contraseña
sudo passwd miadmin2
```

**Explicación de parámetros:**
- `-m`: Crea el directorio home
- `-G sudo,adm,...`: Grupos adicionales
  - `sudo`: Permite ejecutar comandos con privilegios root
  - `adm`: Acceso a logs en `/var/log`
  - `cdrom,dip,plugdev`: Acceso a dispositivos
  - `lxd`: Gestión de contenedores LXD
- `-s /bin/bash`: Shell bash por defecto

**Método 2: Crear y agregar a grupos**
```bash
# Crear usuario básico
sudo adduser miadmin2

# Agregar a grupos administrativos
sudo usermod -aG sudo,adm,cdrom,dip,plugdev,lxd miadmin2
```

#### Verificación de Usuarios
```bash
# Ver información completa
id miadmin2

# Ver grupos
groups miadmin2

# Ver entrada en /etc/passwd
cat /etc/passwd | grep miadmin2

# Ver todos los grupos
cat /etc/group
```

#### Usuario Operador Web

El usuario operador web gestiona el contenido del servidor web con permisos limitados a `/var/www/html`.

> - [X] **operadorweb/paso** - Gestión de contenido web

**Características:**
- Directorio home: `/var/www/html`
- Grupo primario: `www-data`
- Shell: `/bin/bash`
- Permisos: rwx en `/var/www/html`

#### Creación del Usuario Operador Web
```bash
# Crear usuario operador web
sudo useradd -M -d /var/www/html -g www-data -s /bin/bash operadorweb

# Establecer contraseña
sudo passwd operadorweb
```

**Explicación:**
- `-M`: No crear directorio home automáticamente
- `-d /var/www/html`: Directorio home existente
- `-g www-data`: Grupo primario
- `-s /bin/bash`: Shell bash

#### Configurar Propiedad y Permisos
```bash
# Cambiar propietario y grupo
sudo chown -R operadorweb:www-data /var/www/html

# Establecer permisos
sudo chmod -R 775 /var/www/html
```

**Explicación de permisos 775:**
- **7** (Propietario): rwx
- **7** (Grupo): rwx
- **5** (Otros): r-x

**Justificación:**
- `operadorweb` puede crear, modificar y eliminar archivos
- Grupo `www-data` (Apache) puede leer, ejecutar y escribir si necesario
- Otros solo lectura

#### Verificación
```bash
# Ver información
id operadorweb

# Verificar propiedad
ls -la /var/www/

# Verificar permisos
ls -ld /var/www/html
```

**Salida esperada:**
```
drwxrwxr-x 2 operadorweb www-data 4096 nov 30 10:00 /var/www/html
```

#### Comandos Útiles de Gestión
```bash
# Cambiar contraseña
sudo passwd <usuario>

# Cambiar shell
sudo usermod -s /bin/bash <usuario>

# Agregar a grupo
sudo usermod -aG <grupo> <usuario>

# Eliminar de grupo
sudo gpasswd -d <usuario> <grupo>

# Eliminar usuario completamente
sudo userdel -r <usuario>

# Bloquear cuenta
sudo usermod -L <usuario>

# Desbloquear cuenta
sudo usermod -U <usuario>
```

### 1.4 Seguridad Básica

#### A) Cortafuegos (UFW)

UFW (Uncomplicated Firewall) simplifica la gestión del firewall Linux.

##### Instalación
```bash
sudo apt update
sudo apt install ufw
```

##### Configuración
```bash
# Habilitar cortafuegos
sudo ufw enable

# Abrir puerto SSH (importante hacerlo ANTES de habilitar)
sudo ufw allow 22/tcp
```

Confirmación requerida:
```
Command may disrupt existing ssh connections. Proceed with operation (y|n)?
```

Responder `y`.

**Eliminar reglas IPv6 innecesarias:**
```bash
# Ver reglas numeradas
sudo ufw status numbered

# Eliminar regla (ejemplo: número 2)
sudo ufw delete 2
```

<!-- Captura sugerida: salida de ufw status numbered mostrando reglas configuradas -->

##### Monitorización
```bash
# Ver estado y reglas
sudo ufw status

# Ver estado detallado
sudo ufw status verbose

# Ver reglas numeradas
sudo ufw status numbered
```

##### Mantenimiento
```bash
# Deshabilitar temporalmente
sudo ufw disable

# Habilitar
sudo ufw enable

# Restablecer reglas
sudo ufw reset

# Recargar
sudo ufw reload
```

##### Comandos Adicionales
```bash
# Permitir puerto
sudo ufw allow 80/tcp

# Denegar puerto
sudo ufw deny 23/tcp

# Eliminar regla por especificación
sudo ufw delete allow 80/tcp

# Permitir desde IP específica
sudo ufw allow from 192.168.1.100

# Permitir desde red
sudo ufw allow from 192.168.1.0/24

# Limitar conexiones (anti fuerza bruta)
sudo ufw limit 22/tcp
```

#### B) SSH (Acceso Remoto)

##### Instalación

Si no está instalado:
```bash
sudo apt update
sudo apt install openssh-server
```

##### Configuración
```bash
# Crear copia de seguridad
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Editar configuración
sudo nano /etc/ssh/sshd_config
```

**Parámetros importantes:**
```
Port 22
PermitRootLogin no
PasswordAuthentication yes
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
```

Aplicar cambios:
```bash
# Verificar sintaxis
sudo sshd -t

# Reiniciar SSH
sudo systemctl restart ssh
```

##### Monitorización
```bash
# Ver estado
sudo systemctl status ssh

# Ver conexiones activas
sudo ss -punta | grep ssh

# Ver logs (últimas 50 líneas)
sudo journalctl -u ssh -n 50

# Seguir logs en tiempo real
sudo journalctl -u ssh -f
```

##### Mantenimiento
```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload ssh
sudo systemctl enable ssh
sudo systemctl disable ssh
```

##### Conexión desde Cliente

**Desde Windows/Linux/macOS:**
```bash
ssh miadmin@10.199.10.22
```

Primera conexión mostrará mensaje de autenticidad. Escribir `yes`.

**Herramientas recomendadas:**
- Windows: MobaXterm, PuTTY, Windows Terminal
- Linux/macOS: Terminal integrada

#### C) Antivirus (ClamAV)

ClamAV detecta malware en archivos subidos o almacenados en el servidor.

##### Instalación
```bash
sudo apt update
sudo apt install clamav clamav-daemon -y
```

##### Configuración

**Actualizar base de datos de virus:**
```bash
# Detener servicio de actualización
sudo systemctl stop clamav-freshclam

# Actualizar manualmente
sudo freshclam

# Reiniciar servicio
sudo systemctl start clamav-freshclam

# Habilitar inicio automático
sudo systemctl enable clamav-freshclam
```

##### Monitorización
```bash
# Ver estado del demonio
sudo systemctl status clamav-daemon

# Ver estado de actualización
sudo systemctl status clamav-freshclam

# Ver versión
clamscan --version
```

##### Escaneo
```bash
# Escanear archivo
sudo clamscan /ruta/archivo

# Escanear directorio recursivamente
sudo clamscan -r /var/www/html

# Escanear solo infectados
sudo clamscan -ri /var/www/html

# Escanear y eliminar infectados (¡PRECAUCIÓN!)
sudo clamscan -r --remove /var/www/html

# Escanear y mover a cuarentena
sudo clamscan -r --move=/var/quarantine /var/www/html
```

##### Mantenimiento
```bash
sudo systemctl start clamav-daemon
sudo systemctl stop clamav-daemon
sudo systemctl restart clamav-daemon
sudo systemctl enable clamav-daemon
sudo systemctl disable clamav-daemon
sudo freshclam
```

#### D) Actualización del Sistema
```bash
# Actualizar lista de paquetes
sudo apt update

# Actualizar paquetes instalados
sudo apt upgrade -y

# Actualización completa (incluye kernel)
sudo apt full-upgrade -y

# Limpiar paquetes obsoletos
sudo apt autoremove -y
sudo apt autoclean
```

#### E) Configuración de Fecha y Hora
```bash
# Ver fecha actual
date

# Ver configuración detallada
timedatectl
```

**Configurar zona horaria:**
```bash
# Listar zonas disponibles
timedatectl list-timezones

# Para España
sudo timedatectl set-timezone Europe/Madrid

# Verificar
timedatectl
```

**Habilitar sincronización NTP:**
```bash
sudo timedatectl set-ntp true
timedatectl status
```

---

## 2. Servidor Web (Apache)

Apache HTTP Server es el servidor web más utilizado, de código abierto y arquitectura modular.

**Funciones:**
- Recibe peticiones HTTP/HTTPS
- Entrega contenido estático
- Procesa contenido dinámico mediante PHP
- Gestiona múltiples sitios (hosts virtuales)
- Implementa seguridad SSL/TLS

### Instalación
```bash
# Actualizar repositorios
sudo apt update

# Instalar Apache
sudo apt install apache2 -y
```

### Configuración

#### Estructura de Directorios
```
/etc/apache2/
├── apache2.conf          # Configuración principal
├── ports.conf            # Puertos de escucha
├── envvars               # Variables de entorno
├── conf-available/       # Configuraciones disponibles
├── conf-enabled/         # Configuraciones activas (symlinks)
├── mods-available/       # Módulos disponibles
├── mods-enabled/         # Módulos activos (symlinks)
├── sites-available/      # Sitios virtuales disponibles
└── sites-enabled/        # Sitios virtuales activos (symlinks)
```

**Explicación:**

- **apache2.conf**: Archivo principal, cargado primero
- **ports.conf**: Define puertos (80, 443)
- **mods-available/**: Archivos de módulos instalados
- **mods-enabled/**: Enlaces a módulos activos
- **conf-available/**: Configuraciones globales opcionales
- **conf-enabled/**: Enlaces a configuraciones activas
- **sites-available/**: Configuraciones de VirtualHosts
- **sites-enabled/**: Enlaces a sitios activos

#### Configuración Principal
```bash
# Crear copia de seguridad
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.backup

# Editar
sudo nano /etc/apache2/apache2.conf
```

**Agregar ServerName al final:**
```apache
ServerName daw-used
```

Elimina warning de ServerName al iniciar Apache.

<!-- Captura sugerida: apache2.conf mostrando ServerName agregado -->

**Permitir .htaccess:**

Buscar sección `<Directory /var/www/>`:
```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All       # Cambiar de None a All
    Require all granted
</Directory>
```

<!-- Captura sugerida: sección Directory con AllowOverride All -->

**Justificación AllowOverride All:**
- Permite redirecciones personalizadas
- Manejo de errores
- Reescritura de URLs
- Control de acceso por directorio
- Configuraciones sin reiniciar Apache

Guardar con `Ctrl+O`, Enter, salir con `Ctrl+X`.

#### Verificar y Aplicar
```bash
# Verificar sintaxis
sudo apache2ctl configtest
```

Salida esperada: `Syntax OK`
```bash
# Reiniciar Apache
sudo systemctl restart apache2
```

#### Configurar Directorio de Errores
```bash
# Crear directorio
sudo mkdir -p /var/www/html/error

# Asignar permisos
sudo chown -R operadorweb:www-data /var/www/html/error
sudo chmod -R 775 /var/www/html/error
```

#### Configurar Sitio por Defecto
```bash
# Crear copia
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf.backup

# Editar
sudo nano /etc/apache2/sites-available/000-default.conf
```

**Modificaciones:**
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    # Logs personalizados
    ErrorLog /var/www/html/error/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

<!-- Captura sugerida: 000-default.conf con ErrorLog y CustomLog -->

Verificar y reiniciar:
```bash
sudo apache2ctl configtest
sudo systemctl restart apache2
```

#### Configurar .htaccess
```bash
sudo nano /var/www/html/.htaccess
```

**Contenido básico:**
```apache
# Habilitar motor de reescritura
RewriteEngine On

# Páginas de error personalizadas
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
ErrorDocument 500 /error/500.html
```

#### Abrir Puerto HTTP
```bash
# Permitir HTTP
sudo ufw allow 80/tcp

# Verificar
sudo ufw status numbered

# Eliminar IPv6 si no se necesita
sudo ufw delete [número]
```

### Monitorización
```bash
# Estado del servicio
sudo systemctl status apache2

# Módulos activos
apache2ctl -M

# Sitios activos
apache2ctl -S

# Procesos Apache
ps aux | grep apache2

# Conexiones activas
sudo ss -tuln | grep :80

# Ver logs
sudo tail -f /var/log/apache2/error.log
sudo tail -f /var/log/apache2/access.log
```

#### Verificar desde Navegador

Acceder a:
```
http://10.199.10.22
```

Debería aparecer "Apache2 Ubuntu Default Page".

<!-- Captura sugerida: página por defecto de Apache2 en navegador -->

### Mantenimiento
```bash
# Iniciar
sudo systemctl start apache2

# Detener
sudo systemctl stop apache2

# Reiniciar
sudo systemctl restart apache2

# Recargar (sin detener)
sudo systemctl reload apache2

# Habilitar inicio automático
sudo systemctl enable apache2

# Deshabilitar
sudo systemctl disable apache2

# Ver logs
sudo journalctl -u apache2 -n 50
sudo journalctl -u apache2 -f
```

#### Gestión de Módulos
```bash
# Listar disponibles
ls /etc/apache2/mods-available/

# Listar activos
ls /etc/apache2/mods-enabled/

# Ver cargados
apache2ctl -M

# Habilitar módulo
sudo a2enmod [nombre]

# Deshabilitar módulo
sudo a2dismod [nombre]

# Recargar Apache
sudo systemctl reload apache2
```

**Módulos comunes:**
- `rewrite`: Reescritura URLs
- `ssl`: HTTPS
- `headers`: Manipulación cabeceras
- `proxy_fcgi`: Integración PHP-FPM
- `setenvif`: Variables de entorno

#### Gestión de Sitios
```bash
# Listar disponibles
ls /etc/apache2/sites-available/

# Listar activos
ls /etc/apache2/sites-enabled/

# Ver activos
apache2ctl -S

# Habilitar sitio
sudo a2ensite [nombre]

# Deshabilitar sitio
sudo a2dissite [nombre]

# Recargar
sudo systemctl reload apache2
```

### Permisos y Usuarios

Verificar y ajustar:
```bash
# Verificar propietario
ls -la /var/www/

# Establecer si necesario
sudo chown -R operadorweb:www-data /var/www/html
sudo chmod -R 775 /var/www/html
```

**Permisos específicos:**
```bash
# Directorios con escritura
sudo chmod -R 775 /var/www/html/uploads
sudo chmod -R 775 /var/www/html/cache
```

### HTTPS (Certificados SSL/TLS)

HTTPS es esencial para funcionalidades web modernas.

#### Generar Certificado Autofirmado
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/daw-used.key \
  -out /etc/ssl/certs/daw-used.crt
```

**Explicación:**
- `req`: Solicitud de certificado
- `-x509`: Certificado autofirmado
- `-nodes`: No cifrar clave privada
- `-days 365`: Validez 1 año
- `-newkey rsa:2048`: Clave RSA 2048 bits
- `-keyout`: Ubicación clave privada
- `-out`: Ubicación certificado

**Información a completar:**
```
Country Name: ES
State: [Provincia]
Locality: [Ciudad]
Organization: IES Los Sauces
Organizational Unit: Informatica
Common Name: daw-used
Email: [email]
```

<!-- Captura sugerida: proceso de generación del certificado con datos -->

#### Verificar Certificados
```bash
# Verificar certificado
sudo ls -la /etc/ssl/certs/ | grep daw-used

# Verificar clave
sudo ls -la /etc/ssl/private/ | grep daw-used

# Ver contenido
sudo openssl x509 -in /etc/ssl/certs/daw-used.crt -text -noout
```

#### Habilitar SSL
```bash
# Habilitar módulo
sudo a2enmod ssl

# Reiniciar
sudo systemctl restart apache2

# Verificar
apache2ctl -M | grep ssl
```

#### Configurar Sitio HTTPS
```bash
cd /etc/apache2/sites-available/
sudo cp default-ssl.conf daw-used-ssl.conf
sudo nano daw-used-ssl.conf
```

**Contenido:**
```apache
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
        ServerAdmin webmaster@localhost
        ServerName daw-used
        
        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on

        SSLCertificateFile    /etc/ssl/certs/daw-used.crt
        SSLCertificateKeyFile /etc/ssl/private/daw-used.key

        <Directory /var/www/html>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        <FilesMatch "\.(cgi|shtml|phtml|php)$">
            SSLOptions +StdEnvVars
        </FilesMatch>
        
        <Directory /usr/lib/cgi-bin>
            SSLOptions +StdEnvVars
        </Directory>
    </VirtualHost>
</IfModule>
```

<!-- Captura sugerida: daw-used-ssl.conf con rutas de certificados -->

**Habilitar sitio:**
```bash
sudo a2ensite daw-used-ssl.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

#### Abrir Puerto HTTPS
```bash
sudo ufw allow 443/tcp
sudo ufw status numbered
```

#### Verificar HTTPS

Acceder a:
```
https://10.199.10.22
```

El navegador mostrará advertencia (normal con certificado autofirmado).

- Chrome: "Avanzado" → "Continuar a..."
- Firefox: "Avanzado" → "Aceptar el riesgo..."

<!-- Captura sugerida: advertencia de certificado autofirmado en navegador -->

#### Redirección HTTP → HTTPS

**Habilitar módulo rewrite:**
```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

**Configurar .htaccess:**
```bash
sudo nano /var/www/html/.htaccess
```

**Agregar:**
```apache
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://10.199.10.22/$1 [R=301,L]
```

**Explicación:**
- `RewriteEngine On`: Activa reescritura
- `RewriteCond %{SERVER_PORT} 80`: Si puerto 80 (HTTP)
- `RewriteRule`: Redirige a HTTPS
  - `^(.*)$`: Captura URL completa
  - `$1`: Mantiene ruta
  - `R=301`: Redirección permanente
  - `L`: Última regla

### Hosts Virtuales

Apache puede alojar múltiples sitios mediante VirtualHosts.

#### Requisitos Previos

1. DNS configurado (ver sección [DNS](#71-dns))
2. Usuario enjaulado SFTP (ver sección [Usuarios Enjaulados](#usuarios-enjaulados-1))

#### Estructura de Directorios
```
/var/www/usuarioenjaulado1/
├── httpdocs/           # Raíz del sitio
│   ├── index.html
│   └── ...
└── logs/               # Logs (opcional)
```

#### Configuración de Host Virtual

Ejemplo: `sitio1.ejemplo.ieslossauces.es`

**Paso 1: Crear usuario enjaulado**

Ver sección [Usuarios Enjaulados](#usuarios-enjaulados-1).
```bash
# Crear usuario
sudo useradd -g www-data -G sftpusers -m -d /var/www/usuarioenjaulado1 usuarioenjaulado1
sudo passwd usuarioenjaulado1

# Configurar jaula
sudo chown root:root /var/www/usuarioenjaulado1
sudo chmod 555 /var/www/usuarioenjaulado1

# Crear directorio trabajo
sudo mkdir /var/www/usuarioenjaulado1/httpdocs
sudo chown usuarioenjaulado1:www-data -R /var/www/usuarioenjaulado1/httpdocs
sudo chmod 2775 -R /var/www/usuarioenjaulado1/httpdocs
```

**Paso 2: Crear configuración VirtualHost**
```bash
cd /etc/apache2/sites-available/
sudo cp 000-default.conf sitio1-ejemplo-ieslossauces-es.conf
sudo nano sitio1-ejemplo-ieslossauces-es.conf
```

**Contenido:**
```apache
<VirtualHost *:80>
    ServerName sitio1.ejemplo.ieslossauces.es
    ServerAdmin webmaster@ejemplo.ieslossauces.es
    
    DocumentRoot /var/www/usuarioenjaulado1/httpdocs

    ErrorLog ${APACHE_LOG_DIR}/error-sitio1.log
    CustomLog ${APACHE_LOG_DIR}/access-sitio1.log combined

    <Directory /var/www/usuarioenjaulado1/httpdocs>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch "\.php$">
        SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>
```

<!-- Captura sugerida: configuración VirtualHost con ServerName y DocumentRoot -->

**Paso 3: Habilitar sitio**
```bash
sudo a2ensite sitio1-ejemplo-ieslossauces-es.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

**Paso 4: Verificar**
```bash
apache2ctl -S
```

#### HTTPS para Host Virtual
```bash
# Copiar configuración
sudo cp sitio1-ejemplo-ieslossauces-es.conf sitio1-ejemplo-ieslossauces-es-ssl.conf

# Editar para HTTPS
sudo nano sitio1-ejemplo-ieslossauces-es-ssl.conf
```

**Modificar:**
```apache
<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerName sitio1.ejemplo.ieslossauces.es
        ServerAdmin webmaster@ejemplo.ieslossauces.es
        DocumentRoot /var/www/usuarioenjaulado1/httpdocs

        ErrorLog ${APACHE_LOG_DIR}/error-sitio1-ssl.log
        CustomLog ${APACHE_LOG_DIR}/access-sitio1-ssl.log combined

        SSLEngine on
        SSLCertificateFile    /etc/ssl/certs/daw-used.crt
        SSLCertificateKeyFile /etc/ssl/private/daw-used.key

        <Directory /var/www/usuarioenjaulado1/httpdocs>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        <FilesMatch "\.php$">
            SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost/"
        </FilesMatch>
    </VirtualHost>
</IfModule>
```

Habilitar:
```bash
sudo a2ensite sitio1-ejemplo-ieslossauces-es-ssl.conf
sudo systemctl reload apache2
```

---

## 3. Intérprete PHP (PHP-FPM)

PHP-FPM (FastCGI Process Manager) mejora rendimiento y estabilidad en la ejecución de PHP.

**Ventajas sobre mod_php:**
- Mejor rendimiento
- Menor consumo de memoria
- Mayor estabilidad
- Configuración granular por sitio
- Compatible con MPM Event

### Instalación
```bash
# Actualizar
sudo apt update

# Instalar software-properties-common
sudo apt install software-properties-common -y

# Agregar repositorio Ondřej Surý
sudo add-apt-repository ppa:ondrej/php -y

# Verificar
ls /etc/apt/sources.list.d/ | grep ondrej

# Actualizar
sudo apt update

# Instalar PHP 8.3 y PHP-FPM
sudo apt install php8.3 php8.3-fpm php8.3-cli libapache2-mod-php8.3 -y
```

### Configuración

#### Integración con Apache
```bash
# Habilitar proxy_fcgi y setenvif
sudo a2enmod proxy_fcgi setenvif

# Verificar
apache2ctl -M | grep proxy_fcgi
apache2ctl -M | grep setenvif
```

**Deshabilitar mod_php y cambiar MPM:**
```bash
# Deshabilitar mod_php
sudo a2dismod php8.3

# Deshabilitar MPM prefork
sudo a2dismod mpm_prefork

# Habilitar MPM event
sudo a2enmod mpm_event
```

**MPM (Multi-Processing Modules):**
- **prefork**: Compatible mod_php, menos eficiente
- **worker**: Más eficiente, no compatible mod_php
- **event**: Más eficiente, compatible PHP-FPM

**Habilitar configuración PHP-FPM:**
```bash
sudo a2enconf php8.3-fpm

# Verificar enlace
ls -la /etc/apache2/conf-enabled/ | grep php
```

**Reiniciar servicios:**
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl restart apache2
```

#### Verificar Integración
```bash
sudo nano /var/www/html/info.php
```

Contenido:
```php
<?php
phpinfo();
?>
```

Acceder:
```
https://10.199.10.22/info.php
```

<!-- Captura sugerida: phpinfo() mostrando Server API: FPM/FastCGI -->

Buscar "Server API": debe decir "FPM/FastCGI".

**Eliminar archivo:**
```bash
sudo rm /var/www/html/info.php
```

#### Archivo php.ini

El archivo para PHP-FPM está en:
```
/etc/php/8.3/fpm/php.ini
```
```bash
# Crear copia
sudo cp /etc/php/8.3/fpm/php.ini /etc/php/8.3/fpm/php.ini.backup

# Editar
sudo nano /etc/php/8.3/fpm/php.ini
```

#### Configuración para Desarrollo

| Directiva | Desarrollo | Producción | Justificación |
|-----------|------------|------------|---------------|
| **ERRORES Y LOGS** ||||
| `display_errors` | `On` | `Off` | Ver errores inmediatamente en desarrollo |
| `display_startup_errors` | `On` | `Off` | Ver errores al iniciar PHP |
| `error_reporting` | `E_ALL` | `E_ALL & ~E_NOTICE` | Reportar todos los errores |
| `log_errors` | `On` | `On` | Siempre registrar en logs |
| `error_log` | `/var/log/php_errors.log` | `/var/log/php_errors.log` | Ubicación del log |
| **LÍMITES** ||||
| `memory_limit` | `256M` | `128M` | Más memoria para debugging |
| `max_execution_time` | `360` | `30` | Permitir ejecuciones largas |
| `max_input_time` | `360` | `60` | Tiempo recibir datos |
| **SUBIDA** ||||
| `file_uploads` | `On` | `On` | Permitir subida |
| `upload_max_filesize` | `100M` | `10M` | Archivos grandes en desarrollo |
| `post_max_size` | `100M` | `10M` | Debe ser ≥ upload_max_filesize |
| **SEGURIDAD** ||||
| `allow_url_fopen` | `On` | `Off` | Abrir URLs remotas |
| `expose_php` | `On` | `Off` | Ocultar versión en producción |
| **OTROS** ||||
| `date.timezone` | `Europe/Madrid` | `Europe/Madrid` | Zona horaria |

**Buscar y modificar (usar Ctrl+W):**
```ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
log_errors = On
error_log = /var/log/php_errors.log
memory_limit = 256M
max_execution_time = 360
max_input_time = 360
file_uploads = On
upload_max_filesize = 100M
post_max_size = 100M
allow_url_fopen = On
date.timezone = Europe/Madrid
```

<!-- Captura sugerida: php.ini mostrando display_errors = On -->
<!-- Captura sugerida: php.ini mostrando memory_limit = 256M -->
<!-- Captura sugerida: php.ini mostrando upload_max_filesize = 100M -->

**Crear archivo de log:**
```bash
sudo touch /var/log/php_errors.log
sudo chown www-data:www-data /var/log/php_errors.log
sudo chmod 644 /var/log/php_errors.log
```

#### Aplicar Cambios
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl status php8.3-fpm
```

### Monitorización
```bash
# Estado
sudo systemctl status php8.3-fpm

# Versión
php -v

# Módulos instalados
php -m

# Configuración completa
php -i

# Procesos
ps aux | grep php-fpm

# Socket
ls -la /run/php/php8.3-fpm.sock

# Modo de escucha
grep '^listen' /etc/php/8.3/fpm/pool.d/www.conf

# Logs
sudo tail -f /var/log/php8.3-fpm.log
sudo tail -f /var/log/php_errors.log
```

### Mantenimiento
```bash
sudo systemctl start php8.3-fpm
sudo systemctl stop php8.3-fpm
sudo systemctl restart php8.3-fpm
sudo systemctl reload php8.3-fpm
sudo systemctl enable php8.3-fpm
sudo systemctl disable php8.3-fpm
sudo journalctl -u php8.3-fpm -n 50
```

### Módulos PHP Esenciales

#### A) php8.3-mysql

**¿Por qué es necesario?**

Permite conectar PHP con MySQL/MariaDB mediante MySQLi y PDO.
```bash
sudo apt install php8.3-mysql
sudo systemctl restart php8.3-fpm

# Verificar
php -m | grep mysql
```

#### B) php8.3-mbstring

**¿Por qué es necesario?**

Funciones estándar (`strlen`, `substr`) cuentan bytes, no caracteres. En UTF-8:
- "ñ" ocupa 2 bytes
- "á" ocupa 2 bytes
- "😀" ocupa 4 bytes

**Problema sin mbstring:**
```php
$texto = "niño";
echo strlen($texto);      // 5 (bytes)
echo substr($texto, 0, 2); // "ni" (puede cortar mal)
```

**Solución con mbstring:**
```php
$texto = "niño";
echo mb_strlen($texto);      // 4 (caracteres)
echo mb_substr($texto, 0, 2); // "ni" (correcto)
```
```bash
sudo apt install php8.3-mbstring
sudo systemctl restart php8.3-fpm
php -m | grep mbstring
```

**Funciones principales:**

| Función | Descripción | Ejemplo |
|---------|-------------|---------|
| `mb_strlen()` | Cuenta caracteres | `mb_strlen("niño")` → 4 |
| `mb_substr()` | Extrae subcadena | `mb_substr("niño", 0, 2)` → "ni" |
| `mb_strpos()` | Encuentra posición | `mb_strpos("niño", "ñ")` → 2 |
| `mb_strtolower()` | Minúsculas | `mb_strtolower("ESPAÑA")` → "españa" |
| `mb_strtoupper()` | Mayúsculas | `mb_strtoupper("españa")` → "ESPAÑA" |
| `mb_convert_encoding()` | Cambia codificación | Conversión entre codificaciones |

#### C) php8.3-intl

**¿Por qué es necesario?**

Proporciona internacionalización basada en ICU (International Components for Unicode).
```bash
sudo apt install php8.3-intl
sudo systemctl restart php8.3-fpm
php -m | grep intl
```

**Funcionalidades:**

| Característica | Descripción | Ejemplo |
|----------------|-------------|---------|
| **Fechas** | Formato regional | `27 de octubre de 2025` (es) vs `October 27, 2025` (en) |
| **Números** | Separadores correctos | `1.220,66` (es_ES) vs `1,220.66` (en_US) |
| **Monedas** | Símbolos regionales | `1.200,50 €` vs `$1,200.50` |
| **Comparación** | Ordenar con reglas del idioma | Ordenar con acentos |
| **Normalización** | Comparaciones correctas | Caracteres combinados vs precompuestos |

**Ejemplo (fechas):**
```php
<?php
$fecha = new DateTime('2025-10-27');
$formatter_es = new IntlDateFormatter('es_ES', IntlDateFormatter::LONG, IntlDateFormatter::NONE);
$formatter_en = new IntlDateFormatter('en_US', IntlDateFormatter::LONG, IntlDateFormatter::NONE);

echo $formatter_es->format($fecha); // 27 de octubre de 2025
echo $formatter_en->format($fecha); // October 27, 2025
?>
```

#### D) php8.3-xml

**¿Por qué es necesario?**

Requerido por herramientas como PHPDocumentor y Composer.
```bash
sudo apt install php8.3-xml
sudo systemctl restart php8.3-fpm
php -m | grep -E 'xml|SimpleXML|dom'
```

#### E) php8.3-curl
```bash
sudo apt install php8.3-curl
sudo systemctl restart php8.3-fpm
php -m | grep curl
```

#### F) php8.3-gd

Manipulación de imágenes.
```bash
sudo apt install php8.3-gd
sudo systemctl restart php8.3-fpm
php -m | grep gd
```

#### G) php8.3-zip
```bash
sudo apt install php8.3-zip
sudo systemctl restart php8.3-fpm
php -m | grep zip
```

---

## 4. Base de Datos (MariaDB)

MariaDB es un RDBMS de código abierto, fork de MySQL, compatible y con mejor rendimiento.

**Ventajas sobre MySQL:**
- Código abierto garantizado
- Mejor rendimiento
- Más funcionalidades
- Compatible con MySQL
- Opción por defecto en Ubuntu

### Instalación
```bash
sudo apt update
sudo apt install mariadb-server -y
```

### Configuración

#### Permitir Conexiones Remotas
```bash
# Copia de seguridad
sudo cp /etc/mysql/mariadb.conf.d/50-server.cnf /etc/mysql/mariadb.conf.d/50-server.cnf.backup

# Editar
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

**Buscar:**
```ini
bind-address = 127.0.0.1
```

**Cambiar por:**
```ini
bind-address = 0.0.0.0
```

<!-- Captura sugerida: 50-server.cnf mostrando bind-address = 0.0.0.0 -->

**Aplicar:**
```bash
sudo systemctl restart mariadb
sudo ss -punta | grep mariadb
```

**Salida esperada:**
```
tcp   LISTEN 0  80  0.0.0.0:3306  0.0.0.0:*  users:(("mariadbd",pid=1234,fd=10))
```

#### Asegurar Instalación
```bash
sudo mysql_secure_installation
```

<!-- Captura sugerida: asistente mysql_secure_installation -->

**Respuestas:**

1. **Contraseña root actual:** `Enter` (sin contraseña)
2. **Switch to unix_socket:** `n`
3. **Cambiar contraseña root:** `Y` → `paso`
4. **Eliminar usuarios anónimos:** `Y`
5. **Desactivar login remoto root:** `Y`
6. **Eliminar base test:** `Y`
7. **Recargar privilegios:** `Y`

#### Crear Usuario Administrador
```bash
sudo mariadb
```
```sql
GRANT ALL ON *.* TO 'adminsql'@'%' IDENTIFIED BY 'paso' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

**Verificar:**
```bash
sudo mariadb -e "SELECT User, Host FROM mysql.user WHERE User='adminsql';"
```

#### Abrir Puerto
```bash
sudo ufw allow 3306/tcp
```

### Monitorización
```bash
# Estado
sudo systemctl status mariadb

# Versión
mariadb --version

# Procesos
sudo ps aux | grep mariadb

# Puerto
sudo ss -punta | grep mariadb
sudo ss -tuln | grep 3306

# Conexiones activas
sudo mariadb -e "SHOW PROCESSLIST;"

# Bases de datos
sudo mariadb -e "SHOW DATABASES;"

# Usuarios
sudo mariadb -e "SELECT User, Host FROM mysql.user;"

# Tamaño BDs
sudo mariadb -e "
  SELECT 
    table_schema AS 'Database',
    ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
  FROM information_schema.tables
  GROUP BY table_schema;
"
```

#### Ver Logs
```bash
sudo tail -f /var/log/mysql/error.log
sudo tail -f /var/log/mysql/mysql.log
```

### Mantenimiento
```bash
sudo systemctl start mariadb
sudo systemctl stop mariadb
sudo systemctl restart mariadb
sudo systemctl reload mariadb
sudo systemctl enable mariadb
sudo systemctl disable mariadb
sudo journalctl -u mariadb -n 50
```

#### Backups

**Backup específico:**
```bash
sudo mysqldump -u root nombre_bd > backup_$(date +%Y%m%d).sql
mysqldump -u adminsql -p nombre_bd > backup_$(date +%Y%m%d).sql
```

**Todas las BDs:**
```bash
sudo mysqldump -u root --all-databases > backup_all_$(date +%Y%m%d).sql
```

**Restaurar:**
```bash
sudo mariadb nombre_bd < backup.sql
mariadb -u adminsql -p nombre_bd < backup.sql
```

#### Comandos SQL Útiles
```sql
-- Listar BDs
SHOW DATABASES;

-- Seleccionar BD
USE nombre_bd;

-- Listar tablas
SHOW TABLES;

-- Ver estructura
DESCRIBE tabla;

-- Ver usuarios
SELECT User, Host FROM mysql.user;

-- Crear BD
CREATE DATABASE nombre_bd CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Eliminar BD
DROP DATABASE nombre_bd;

-- Crear usuario
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'contraseña';

-- Otorgar privilegios
GRANT ALL PRIVILEGES ON nombre_bd.* TO 'usuario'@'localhost';

-- Revocar privilegios
REVOKE ALL PRIVILEGES ON nombre_bd.* FROM 'usuario'@'localhost';

-- Eliminar usuario
DROP USER 'usuario'@'localhost';

-- Recargar privilegios
FLUSH PRIVILEGES;

-- Ver privilegios
SHOW GRANTS FOR 'usuario'@'localhost';
```

---

## 5. Herramientas de Administración

### 5.1 phpMyAdmin

phpMyAdmin es una herramienta web para administrar MySQL/MariaDB con interfaz gráfica.

**Funcionalidades:**
- Explorar bases de datos y tablas
- Ejecutar consultas SQL
- Importar/exportar
- Gestionar usuarios
- Diseñar estructura
- Ver estadísticas

#### Instalación
```bash
# Guardar módulos antes
php -m | sort > /tmp/modulos_antes.txt

# Instalar
sudo apt update
sudo apt install phpmyadmin -y

# Comparar después
php -m | sort > /tmp/modulos_despues.txt
diff /tmp/modulos_antes.txt /tmp/modulos_despues.txt
```

<!-- Captura sugerida: selección de apache2 con asterisco en instalador -->

**Durante instalación:**

1. **Servidor web:** Seleccionar `apache2` (Espacio para marcar)
2. **Configurar BD con dbconfig-common:** `Sí`
3. **Contraseña phpMyAdmin:** `paso`
4. **Confirmar:** `paso`

<!-- Captura sugerida: pantalla de configuración de BD -->
<!-- Captura sugerida: campos de contraseña completados -->

#### Configuración
```bash
# Crear enlace simbólico
sudo ln -sf /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf

# Verificar
ls -la /etc/apache2/conf-available/ | grep phpmyadmin

# Habilitar
sudo a2enconf phpmyadmin

# Verificar sintaxis
sudo apache2ctl configtest

# Recargar
sudo systemctl reload apache2
```

#### Acceso
```
https://10.199.10.22/phpmyadmin/
```

<!-- Captura sugerida: página de login de phpMyAdmin -->

**Credenciales:**
- Usuario: `adminsql`
- Contraseña: `paso`

<!-- Captura sugerida: interfaz principal de phpMyAdmin -->

---

## 6. Herramientas de Desarrollo

### 6.1 XDebug

XDebug proporciona depuración paso a paso y análisis de rendimiento.

**Funcionalidades:**
- Depuración paso a paso
- Inspeccionar variables
- Ver pila de llamadas
- Análisis de rendimiento
- Trazas de errores mejoradas
- Cobertura de código

#### Instalación
```bash
sudo apt update
sudo apt install php8.3-xdebug -y
```

#### Verificar
```bash
# Ver versión (X mayúscula)
php -v | grep Xdebug

# Ver módulos
php -m | grep xdebug

# Ver información
php -i | grep xdebug
```

**Salida esperada:**
```
PHP 8.3.x (cli) (built: ...)
    with Xdebug v3.3.x, Copyright (c) 2002-2024, by Derick Rethans
```

#### Configuración
```bash
sudo nano /etc/php/8.3/fpm/conf.d/20-xdebug.ini
```

**Contenido:**
```ini
; Cargar extensión
zend_extension=xdebug.so

; Modo de operación
xdebug.mode=develop,debug

; Iniciar automáticamente
xdebug.start_with_request=yes

; IP del cliente (IDE)
xdebug.client_host=127.0.0.1

; Puerto comunicación
xdebug.client_port=9003

; Archivo de log
xdebug.log=/tmp/xdebug.log

; Nivel de log
xdebug.log_level=7

; Clave IDE
xdebug.idekey="netbeans-xdebug"

; Descubrir IP automáticamente
xdebug.discover_client_host=1
```

<!-- Captura sugerida: 20-xdebug.ini con configuración completa -->

**Explicación:**

- **xdebug.mode=develop,debug:**
  - `develop`: Mejora visualización errores
  - `debug`: Depuración paso a paso
  - Otros: `coverage`, `profile`

- **xdebug.start_with_request=yes:**
  - Inicia automáticamente
  - Alternativas: `trigger`, `no`

- **xdebug.client_host:**
  - IP del IDE
  - `127.0.0.1` si IDE en mismo servidor
  - IP del host si IDE en otro equipo

- **xdebug.client_port=9003:**
  - Puerto por defecto XDebug 3.x
  - Era 9000 en versiones antiguas

- **xdebug.idekey:**
  - Identificador para IDE
  - Debe coincidir con IDE

#### Crear Log
```bash
sudo touch /tmp/xdebug.log
sudo chmod 666 /tmp/xdebug.log
sudo chown www-data:www-data /tmp/xdebug.log
```

#### Aplicar
```bash
sudo systemctl restart php8.3-fpm
sudo systemctl restart apache2
```

### Monitorización
```bash
# Verificar cargado
php -m | grep xdebug

# Ver configuración
php -i | grep xdebug

# Ver parámetros activos
php -i | grep xdebug | grep '=>'

# Ver log
tail -n 50 /tmp/xdebug.log
tail -f /tmp/xdebug.log
```

#### Verificar desde phpinfo()
```bash
sudo nano /var/www/html/xdebug_test.php
```
```php
<?php
phpinfo();
?>
```

Acceder:
```
https://10.199.10.22/xdebug_test.php
```

<!-- Captura sugerida: sección xdebug en phpinfo() -->

Buscar sección "xdebug" y verificar parámetros.

**Eliminar:**
```bash
sudo rm /var/www/html/xdebug_test.php
```

### Mantenimiento
```bash
sudo systemctl restart php8.3-fpm
cat /tmp/xdebug.log
sudo truncate -s 0 /tmp/xdebug.log
```

#### Configuración IDE NetBeans

1. Herramientas → Opciones → PHP → Depuración
2. Puerto: `9003`
3. Detener en primera línea: Activar

<!-- Captura sugerida: configuración depuración NetBeans con puerto 9003 -->

**Configurar proyecto:**

1. Click derecho proyecto → Propiedades
2. Ejecutar configuración → Avanzado
3. URL: `https://10.199.10.22/proyecto/`

**Iniciar depuración:**

1. Establecer breakpoints (click en número línea)
2. `Ctrl+F5` o "Depurar proyecto"
3. Navegador se abre y pausa en breakpoint

**Controles:**
- `F7`: Entrar en función
- `F8`: Pasar por encima
- `Shift+F7`: Salir de función
- `F5`: Continuar
- `Shift+F5`: Detener

### 6.2 PHPDocumentor

Genera documentación API automáticamente desde comentarios DocBlock.

**Ventajas:**
- Documentación automática
- Estándar DocBlock
- HTML navegable
- Integración con IDEs

**Ejemplo DocBlock:**
```php
<?php
/**
 * Calcula el área de un círculo.
 *
 * @param float $radio El radio del círculo
 * @return float El área del círculo
 * @throws InvalidArgumentException Si radio negativo
 */
function calcularAreaCirculo(float $radio): float {
    if ($radio < 0) {
        throw new InvalidArgumentException("Radio no puede ser negativo");
    }
    return pi() * pow($radio, 2);
}
?>
```

#### Instalación Dependencias
```bash
sudo apt install php8.3-xml php8.3-mbstring
sudo systemctl restart php8.3-fpm
php -m | grep -E 'xml|mbstring'
```

#### Instalación PHPDocumentor
```bash
# Descargar
wget https://phpdoc.org/phpDocumentor.phar

# O con curl
curl -L -O https://phpdoc.org/phpDocumentor.phar

# Verificar
ls -lh phpDocumentor.phar

# Permisos
chmod +x phpDocumentor.phar

# Probar
./phpDocumentor.phar --version

# Mover a ubicación global
sudo mv phpDocumentor.phar /usr/local/bin/phpdoc

# Verificar
phpdoc --version
```

#### Uso

**Estructura proyecto:**
```
/var/www/html/MiProyecto/
├── src/                    # Código fuente
│   ├── models/
│   ├── controllers/
│   └── views/
├── docs/                   # Documentación generada
└── README.md
```

**Generar documentación:**
```bash
# Posicionarse en código fuente
cd /var/www/html/MiProyecto/src

# Generar
phpdoc --directory . --target ../docs
```

**Explicación:**
- `--directory .`: Buscar en directorio actual (recursivo)
- `--target ../docs`: Generar en carpeta docs

**Alternativa con rutas completas:**
```bash
phpdoc --directory /var/www/html/MiProyecto/src \
       --target /var/www/html/MiProyecto/docs
```

**Opciones adicionales:**
```bash
# Excluir directorios
phpdoc -d src -t docs --ignore="vendor/,tests/"

# Título proyecto
phpdoc -d src -t docs --title="Mi Aplicación Web"
```

#### Ver Documentación

**Navegador local:**
```
https://10.199.10.22/MiProyecto/docs/index.html
```

<!-- Captura sugerida: interfaz documentación generada por PHPDocumentor -->

#### Solución Problemas

**Permisos:**
```bash
sudo chown -R operadorweb:www-data /var/www/html/MiProyecto
sudo chmod -R 775 /var/www/html/MiProyecto
```

**Extensión XML:**
```bash
sudo apt install php8.3-xml
sudo systemctl restart php8.3-fpm
php -m | grep xml
```

---

## 7. Servicios Adicionales

### 7.1 DNS

#### Configuración en Plesk

1. Iniciar sesión en Plesk
2. Seleccionar dominio principal
3. "Hosting y DNS" → "DNS"

<!-- Captura sugerida: panel Plesk DNS Settings -->

**Agregar registro A:**

1. "Añadir registro"
2. Tipo: `A`
3. Configurar:
   - **Nombre:** `sitio1`
   - **IPv4:** `10.199.10.22`
   - **TTL:** Por defecto

<!-- Captura sugerida: formulario registro DNS tipo A completado -->

4. "Aceptar"
5. "Actualizar"

**Verificar:**
```bash
nslookup sitio1.tudominio.ieslossauces.es
dig sitio1.tudominio.ieslossauces.es
```

#### Archivo hosts Local

**Windows:**
```
C:\Windows\System32\drivers\etc\hosts
```

**Linux/macOS:**
```
/etc/hosts
```

**Agregar:**
```
10.199.10.22    sitio1.ejemplo.local
10.199.10.22    sitio2.ejemplo.local
```

### 7.2 SFTP

SFTP (SSH File Transfer Protocol) transfiere archivos de forma segura mediante SSH.

**Ventajas:**
- Cifra comunicación completa
- Un solo puerto (22)
- Integrado con SSH
- Autenticación robusta

#### Instalación

SFTP viene con OpenSSH Server (ya instalado).
```bash
sudo systemctl status ssh
```

Si no está:
```bash
sudo apt install openssh-server
```

#### Configuración
```bash
# Ver configuración SFTP
grep -i sftp /etc/ssh/sshd_config
```

**Debe aparecer:**
```
Subsystem sftp /usr/lib/openssh/sftp-server
```

Si necesitas modificar:
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
sudo nano /etc/ssh/sshd_config
```

Después de cambios:
```bash
sudo sshd -t
sudo systemctl restart ssh
```

#### Monitorización
```bash
# Estado
sudo systemctl status ssh

# Conexiones activas
sudo ss -punta | grep ssh
sudo netstat -antp | grep ssh

# Usuarios conectados
who

# Logs
sudo journalctl -u ssh -f
sudo tail -f /var/log/auth.log
```

#### Mantenimiento
```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload ssh
sudo systemctl enable ssh
sudo systemctl disable ssh
```

#### Conexión desde Cliente

**Línea de comandos:**
```bash
# Conectar
sftp usuario@10.199.10.22

# Con puerto diferente
sftp -P 2222 usuario@10.199.10.22
```

**Comandos dentro de SFTP:**
```bash
ls          # Listar remotos
lls         # Listar locales
cd dir      # Cambiar remoto
lcd dir     # Cambiar local
pwd         # Ver remoto
lpwd        # Ver local
get file    # Descargar
put file    # Subir
mkdir dir   # Crear remoto
rmdir dir   # Eliminar remoto
rm file     # Eliminar archivo
exit        # Salir
```

**Clientes gráficos:**
- FileZilla (multiplataforma)
- WinSCP (Windows)
- Cyberduck (Windows/macOS)
- MobaXterm (Windows)

**Configurar FileZilla:**

1. Archivo → Gestor de sitios → Nuevo sitio
2. Protocolo: SFTP
3. Servidor: 10.199.10.22
4. Puerto: 22
5. Usuario: operadorweb
6. Contraseña: paso

<!-- Captura sugerida: configuración FileZilla con SFTP -->

#### Usuarios Enjaulados

Usuario enjaulado está restringido a su home.

**Justificación:**
- Seguridad
- Aislamiento
- Hosting compartido

**Estructura:**
```
/var/www/usuarioenjaulado1/          # Raíz jaula (root)
├── httpdocs/                         # Usuario puede escribir
│   ├── index.html
│   └── ...
└── logs/                             # Opcional
```

#### Crear Usuario Enjaulado

**Paso 1: Crear grupo**
```bash
sudo groupadd sftpusers
```

**Paso 2: Crear usuario**
```bash
sudo useradd -g www-data -G sftpusers -m -d /var/www/usuarioenjaulado1 usuarioenjaulado1
sudo passwd usuarioenjaulado1
```

**Explicación:**
- `-g www-data`: Grupo primario
- `-G sftpusers`: Grupo adicional
- `-m`: Crear home
- `-d /var/www/usuarioenjaulado1`: Ubicación home

**Paso 3: Configurar jaula**
```bash
# Cambiar propietario a root
sudo chown root:root /var/www/usuarioenjaulado1

# Quitar escritura
sudo chmod 555 /var/www/usuarioenjaulado1
```

**Verificar:**
```bash
ls -ld /var/www/usuarioenjaulado1
```

**Salida esperada:**
```
dr-xr-xr-x 3 root root 4096 nov 30 12:00 /var/www/usuarioenjaulado1
```

**Paso 4: Crear directorio trabajo**
```bash
# Crear httpdocs
sudo mkdir /var/www/usuarioenjaulado1/httpdocs

# Asignar propiedad
sudo chown usuarioenjaulado1:www-data /var/www/usuarioenjaulado1/httpdocs

# Permisos
sudo chmod 2775 /var/www/usuarioenjaulado1/httpdocs
```

**Permisos 2775:**
- **2**: Setgid (archivos heredan grupo)
- **7**: Propietario rwx
- **7**: Grupo rwx
- **5**: Otros r-x

**Crear logs (opcional):**
```bash
sudo mkdir /var/www/usuarioenjaulado1/logs
sudo chown usuarioenjaulado1:www-data /var/www/usuarioenjaulado1/logs
sudo chmod 2775 /var/www/usuarioenjaulado1/logs
```

**Paso 5: Configurar SSH**
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
sudo nano /etc/ssh/sshd_config
```

**Buscar:**
```
Subsystem sftp /usr/lib/openssh/sftp-server
```

**Comentar y agregar:**
```
# Subsystem sftp /usr/lib/openssh/sftp-server
Subsystem sftp internal-sftp
```

**Al final del archivo:**
```
# Usuarios SFTP enjaulados
Match Group sftpusers
    ChrootDirectory %h
    ForceCommand internal-sftp -u 2
    AllowTcpForwarding yes
    PermitTunnel no
    X11Forwarding no
```

<!-- Captura sugerida: sshd_config con Match Group sftpusers -->

**Explicación:**

- **Match Group sftpusers:** Aplicar solo a este grupo
- **ChrootDirectory %h:** Encarcelar en home
- **ForceCommand internal-sftp -u 2:** Forzar SFTP, umask 002
- **AllowTcpForwarding yes:** Permitir túneles TCP
- **PermitTunnel no:** No túneles dispositivo
- **X11Forwarding no:** No reenvío X11

**Paso 6: Verificar y reiniciar**
```bash
sudo sshd -t
sudo systemctl restart ssh
```

#### Verificar Usuario Enjaulado
```bash
sftp usuarioenjaulado1@10.199.10.22
```

**Probar:**
```bash
pwd         # Debe ser /
ls          # Debe ver httpdocs
cd ..       # Debe fallar
pwd         # Debe seguir en /
cd httpdocs # Debe funcionar
put test.txt
ls
```

**Desde servidor:**
```bash
ls -la /var/www/usuarioenjaulado1/httpdocs/
```

**Intentar SSH (debe fallar):**
```bash
ssh usuarioenjaulado1@10.199.10.22
```

**Salida esperada:**
```
This service allows sftp connections only.
Connection closed.
```

### 7.3 Apache Tomcat

Apache Tomcat es servidor de aplicaciones Java para Servlets, JSP y WebSockets.

*(Sección reservada para futura expansión)*

### 7.4 LDAP

LDAP (Lightweight Directory Access Protocol) para autenticación centralizada y directorios corporativos.

*(Sección reservada para futura expansión)*

---

## 8. Comandos de Referencia Rápida

### Sistema Operativo
```bash
# Información sistema
uname -a
hostnamectl
cat /etc/os-release
lsb_release -a

# Fecha y hora
date
timedatectl
timedatectl set-timezone Europe/Madrid

# Recursos
free -h
df -h
top
htop

# Particiones
df -h
lsblk
fdisk -l
du -sh /ruta

# Actualizaciones
sudo apt update
sudo apt upgrade -y
sudo apt full-upgrade -y
sudo apt autoremove -y
sudo apt autoclean

# Apagar/Reiniciar
sudo shutdown -h now
sudo shutdown -r now
sudo reboot
```

### Red
```bash
# Interfaces
ip a
ip addr show enp0s3
hostname -I
hostname
hostnamectl

# Enrutamiento
ip r
ip route show

# DNS
resolvectl status
cat /etc/resolv.conf

# Conectividad
ping -c 4 8.8.8.8
ping -c 4 google.com
traceroute 8.8.8.8
mtr google.com

# Puertos
ss -tuln
ss -punta
ss -tuln | grep :80
netstat -tuln
sudo lsof -i :80

# Configuración
cat /etc/netplan/*.yaml
sudo netplan apply
```

### Usuarios y Permisos
```bash
# Información
whoami
id
id usuario
groups usuario
w
who
last

# Gestión usuarios
sudo useradd -m -s /bin/bash usuario
sudo passwd usuario
sudo usermod -aG grupo usuario
sudo userdel -r usuario
sudo chage -l usuario

# Cambiar usuario
su usuario
su -
exit
sudo -i

# Permisos
ls -l archivo
ls -ld directorio
chmod 755 archivo
chmod -R 775 directorio
chown usuario:grupo archivo
chown -R usuario:grupo dir
chgrp grupo archivo

# Archivos usuarios
cat /etc/passwd
cat /etc/group
cat /etc/shadow
```

### Servicios (systemd)
```bash
# Control
sudo systemctl start servicio
sudo systemctl stop servicio
sudo systemctl restart servicio
sudo systemctl reload servicio
sudo systemctl status servicio
sudo systemctl enable servicio
sudo systemctl disable servicio
sudo systemctl is-active servicio
sudo systemctl is-enabled servicio

# Listar
systemctl list-units --type=service
systemctl list-unit-files --type=service

# Logs
sudo journalctl -u servicio
sudo journalctl -u servicio -n 50
sudo journalctl -u servicio -f
sudo journalctl -u servicio --since "1 hour ago"
sudo journalctl -xe
```

### Apache
```bash
# Control
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl reload apache2
sudo systemctl status apache2

# Verificación
apache2ctl configtest
apache2ctl -M
apache2ctl -S
apache2ctl -V

# Módulos
sudo a2enmod nombre
sudo a2dismod nombre
ls /etc/apache2/mods-available
ls /etc/apache2/mods-enabled

# Sitios
sudo a2ensite nombre
sudo a2dissite nombre
ls /etc/apache2/sites-available
ls /etc/apache2/sites-enabled

# Configuraciones
sudo a2enconf nombre
sudo a2disconf nombre

# Logs
sudo tail -f /var/log/apache2/error.log
sudo tail -f /var/log/apache2/access.log
sudo tail -n 100 /var/log/apache2/error.log
```

### PHP
```bash
# Información
php -v
php -m
php -i
php -i | grep -i "configuration file"
php --ini

# Ejecutar
php -r "echo 'Hola';"
php archivo.php
php -l archivo.php

# PHP-FPM
sudo systemctl start php8.3-fpm
sudo systemctl stop php8.3-fpm
sudo systemctl restart php8.3-fpm
sudo systemctl status php8.3-fpm

# Verificar
php-fpm8.3 -t
ps aux | grep php-fpm

# Cambiar versión
sudo update-alternatives --config php
```

### MariaDB/MySQL
```bash
# Acceso
sudo mariadb
mariadb -u usuario -p
mariadb -u usuario -p -h host

# Control
sudo systemctl start mariadb
sudo systemctl stop mariadb
sudo systemctl restart mariadb
sudo systemctl status mariadb

# Información
mariadb --version
sudo ss -tuln | grep 3306

# Comandos rápidos
sudo mariadb -e "SHOW DATABASES;"
sudo mariadb -e "SHOW PROCESSLIST;"
sudo mariadb -e "SELECT User, Host FROM mysql.user;"

# Backups
mysqldump -u usuario -p bd > backup.sql
mariadb -u usuario -p bd < backup.sql
mysqldump -u usuario -p --all-databases > backup_all.sql
```

### UFW (Cortafuegos)
```bash
# Estado
sudo ufw status
sudo ufw status numbered
sudo ufw status verbose

# Control
sudo ufw enable
sudo ufw disable
sudo ufw reload

# Reglas
sudo ufw allow 80/tcp
sudo ufw allow ssh
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24 to any port 3306

# Eliminar
sudo ufw delete allow 80/tcp
sudo ufw delete 3

# Denegar
sudo ufw deny 23/tcp

# Restablecer
sudo ufw reset
```

### Archivos y Directorios
```bash
# Navegación
pwd
cd /ruta
cd
cd ..
cd -

# Listar
ls
ls -l
ls -la
ls -lh
tree

# Crear
mkdir directorio
mkdir -p ruta/con/subdirs
touch archivo

# Copiar
cp archivo destino
cp -r directorio destino
cp -p archivo destino

# Mover/Renombrar
mv archivo nuevo_nombre
mv archivo /ruta/destino
mv -i archivo destino

# Eliminar
rm archivo
rm -r directorio
rm -rf directorio
rmdir directorio

# Ver contenido
cat archivo
less archivo
more archivo
head archivo
head -n 20 archivo
tail archivo
tail -n 50 archivo
tail -f archivo

# Buscar
find /ruta -name "archivo"
find /var/www -type f -name "*.php"
find /var/www -type d -name "cache"
grep "texto" archivo
grep -r "texto" directorio
locate archivo

# Comprimir
tar -czf archivo.tar.gz directorio/
tar -xzf archivo.tar.gz
zip -r archivo.zip directorio/
unzip archivo.zip
```

### Editor nano
```bash
# Abrir
nano archivo.txt

# Comandos
Ctrl+O      # Guardar
Ctrl+X      # Salir
Ctrl+W      # Buscar
Ctrl+K      # Cortar línea
Ctrl+U      # Pegar
Ctrl+G      # Ayuda
Alt+U       # Deshacer
Alt+E       # Rehacer
```

---

## 9. Solución de Problemas Comunes

### Apache no inicia

**Diagnóstico:**
```bash
sudo systemctl status apache2
sudo journalctl -u apache2 -n 50
sudo tail -n 50 /var/log/apache2/error.log
sudo apache2ctl configtest
```

**Soluciones:**

1. **Error de sintaxis:**
```bash
sudo apache2ctl configtest
sudo nano /ruta/archivo/con/error
```

2. **Puerto en uso:**
```bash
sudo ss -tuln | grep :80
sudo lsof -i :80
sudo killall apache2
sudo systemctl start apache2
```

3. **Módulo o sitio problemático:**
```bash
sudo a2dissite *
sudo a2ensite 000-default.conf
sudo systemctl start apache2
```

4. **Permisos incorrectos:**
```bash
sudo chown root:root /etc/apache2/sites-available/*
sudo chmod 644 /etc/apache2/sites-available/*
```

### PHP no se ejecuta

**Diagnóstico:**
```bash
sudo systemctl status php8.3-fpm
apache2ctl -M | grep -E 'proxy_fcgi|php'
cat /etc/apache2/sites-enabled/000-default.conf | grep -A 5 php
```

**Soluciones:**

1. **PHP-FPM parado:**
```bash
sudo systemctl start php8.3-fpm
sudo systemctl restart apache2
```

2. **Módulo proxy_fcgi:**
```bash
sudo a2enmod proxy_fcgi setenvif
sudo systemctl restart apache2
```

3. **Configuración no habilitada:**
```bash
sudo a2enconf php8.3-fpm
sudo systemctl reload apache2
```

4. **Falta FilesMatch:**
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Agregar:
```apache
<FilesMatch "\.php$">
    SetHandler "proxy:unix:/run/php/php8.3-fpm.sock|fcgi://localhost/"
</FilesMatch>
```
```bash
sudo systemctl reload apache2
```

5. **mod_php interfiriendo:**
```bash
sudo a2dismod php8.3
sudo systemctl restart apache2
```

### Error conexión a base de datos

**Diagnóstico:**
```bash
sudo systemctl status mariadb
sudo ss -tuln | grep 3306
php -m | grep -E 'mysqli|pdo_mysql'
mariadb -u adminsql -p -h 127.0.0.1
```

**Soluciones:**

1. **MariaDB parado:**
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

2. **Módulo PHP MySQL:**
```bash
sudo apt install php8.3-mysql
sudo systemctl restart php8.3-fpm
```

3. **Usuario incorrecto:**
```bash
sudo mariadb -e "SELECT User, Host FROM mysql.user WHERE User='adminsql';"
sudo mariadb -e "GRANT ALL ON *.* TO 'adminsql'@'%' IDENTIFIED BY 'paso' WITH GRANT OPTION;"
```

4. **No acepta conexiones remotas:**
```bash
grep bind-address /etc/mysql/mariadb.conf.d/50-server.cnf
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
# Cambiar a: bind-address = 0.0.0.0
sudo systemctl restart mariadb
```

5. **Firewall:**
```bash
sudo ufw allow 3306/tcp
```

### Permisos denegados

**Diagnóstico:**
```bash
ls -la /var/www/html
ls -ld /var/www/html
```

**Soluciones:**

1. **Restablecer:**
```bash
sudo chown -R operadorweb:www-data /var/www/html
sudo chmod -R 775 /var/www/html
```

2. **Directorios específicos:**
```bash
sudo chmod -R 775 /var/www/html/uploads
sudo chown -R operadorweb:www-data /var/www/html/uploads
```

3. **Si persiste:**
```bash
sudo chmod -R 777 /var/www/html/uploads
```

### Usuario enjaulado puede salir

**Diagnóstico:**
```bash
sftp usuarioenjaulado1@10.199.10.22
cd /
pwd
ls
```

**Soluciones:**

1. **Verificar grupo:**
```bash
groups usuarioenjaulado1
sudo usermod -aG sftpusers usuarioenjaulado1
```

2. **Verificar sshd_config:**
```bash
cat /etc/ssh/sshd_config | grep -A 6 "Match Group sftpusers"
```

3. **Permisos directorio:**
```bash
ls -ld /var/www/usuarioenjaulado1
sudo chown root:root /var/www/usuarioenjaulado1
sudo chmod 555 /var/www/usuarioenjaulado1
```

4. **SSH no reiniciado:**
```bash
sudo systemctl restart ssh
```

### XDebug no conecta

**Diagnóstico:**
```bash
tail -f /tmp/xdebug.log
php -i | grep xdebug | grep -E 'client_port|client_host|mode'
```

**Soluciones:**

1. **Puerto:**
```bash
grep client_port /etc/php/8.3/fpm/conf.d/20-xdebug.ini
# Debe ser 9003
```

2. **IP cliente:**
```bash
sudo nano /etc/php/8.3/fpm/conf.
```

3. **Firewall:**
```bash
sudo ufw allow from 192.168.1.100 to any port 9003
```

4. **IDE no escucha:**

Verificar que el IDE está en modo depuración.

5. **XDebug no activo:**
```bash
php -m | grep xdebug
grep zend_extension /etc/php/8.3/fpm/conf.d/20-xdebug.ini
sudo systemctl restart php8.3-fpm
```

### Sistema de archivos lleno

**Diagnóstico:**
```bash
df -h
du -sh /* | sort -h
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null
```

**Soluciones:**

1. **Limpiar logs:**
```bash
du -sh /var/log
sudo truncate -s 0 /var/log/apache2/*.log
sudo journalctl --vacuum-time=7d
sudo journalctl --vacuum-size=500M
```

2. **Eliminar paquetes:**
```bash
sudo apt autoremove -y
sudo apt autoclean
sudo apt clean
```

3. **Temporales:**
```bash
sudo find /tmp -type f -atime +7 -delete
sudo rm -rf /var/lib/php/sessions/*
```

### Servidor lento

**Diagnóstico:**
```bash
top
htop
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
uptime
```

**Soluciones:**

1. **Reiniciar servicios:**
```bash
sudo systemctl restart apache2
sudo systemctl restart php8.3-fpm
sudo systemctl restart mariadb
```

2. **Ajustar PHP-FPM:**
```bash
sudo nano /etc/php/8.3/fpm/pool.d/www.conf
# Ajustar:
pm.max_children = 10
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

3. **Optimizar MariaDB:**
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
# Ajustar:
innodb_buffer_pool_size = 512M
max_connections = 50
```

---

> **Enrique Nieto Lorenzo**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  
> Despliegue de Aplicaciones Web