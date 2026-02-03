[< Volver al menú principal](README.md)

# ENTORNO DE EXPLOTACIÓN - PLESK

| CFGS Desarrollo de Aplicaciones Web |
|:-----------------------------------:|
| ![Portada](doc/images/portada.jpg)      |
| **Instalación, Configuración y Documentación del Cliente de Desarrollo** |

> **Autor:** Enrique Nieto Lorenzo 
> **Curso:** 2024/2025  
> **Módulo:** Despliegue de Aplicaciones Web  
> **Centro:** IES Los Sauces

---

## Índice

- [1. Introducción](#1-introducción)
- [2. Primeros Pasos](#2-primeros-pasos)
  - [2.1 Acceso a Plesk](#21-acceso-a-plesk)
  - [2.2 Panel de control](#22-panel-de-control)
- [3. Gestión de Dominios](#3-gestión-de-dominios)
  - [3.1 Crear subdominios](#31-crear-subdominios)
  - [3.2 Configuración DNS](#32-configuración-dns)
- [4. Despliegue de Aplicaciones](#4-despliegue-de-aplicaciones)
  - [4.1 Método 1: Subida manual (FTP/SFTP)](#41-método-1-subida-manual-ftpsftp)
  - [4.2 Método 2: Desde GitHub Release](#42-método-2-desde-github-release)
  - [4.3 Método 3: Integración GitHub (CI/CD automático)](#43-método-3-integración-github-cicd-automático)
- [5. Gestión de Bases de Datos](#5-gestión-de-bases-de-datos)
  - [5.1 Crear base de datos en Plesk](#51-crear-base-de-datos-en-plesk)
  - [5.2 Importar datos (volcado SQL)](#52-importar-datos-volcado-sql)
  - [5.3 Gestión con phpMyAdmin](#53-gestión-con-phpmyadmin)
  - [5.4 Configuración de conexión](#54-configuración-de-conexión)
- [6. Acceso Avanzado](#6-acceso-avanzado)
  - [6.1 Conexión SFTP](#61-conexión-sftp)
  - [6.2 Conexión SSH](#62-conexión-ssh)
- [7. Configuración Avanzada](#7-configuración-avanzada)
  - [7.1 Archivo .htaccess](#71-archivo-htaccess)

---

## 1. Introducción

**¿Qué es Plesk?**

Plesk es un panel de control web para administrar servidores y sitios web fácilmente. Permite gestionar dominios, correos, bases de datos y archivos desde una interfaz gráfica. Incluye opciones para instalar aplicaciones, configurar seguridad y automatizar tareas del servidor.

---

## 2. Primeros Pasos

### 2.1 Acceso a Plesk

Para acceder a Plesk vamos a la URL donde esté el servidor web, pero por el puerto 8443.

En nuestro caso la URL es: https://ieslossauces.es:8443

<!-- Captura sugerida: formulario de inicio de sesión de Plesk -->

**Pasos:**

1. Accede al enlace: https://ieslossauces.es:8443/login_up.php
2. Introduce tu **usuario** y **contraseña**
3. Haz clic en **Iniciar sesión**

<!-- Captura sugerida: página principal de gestión del hosting después del login -->

Una vez dentro, llegarás a la página principal de gestión del hosting.

---

### 2.2 Panel de control

El panel de control de Plesk está organizado en secciones accesibles desde el menú lateral izquierdo:

- **Sitios web y dominios**: Gestión de dominios, subdominios y configuración de sitios
- **Archivos**: Gestor de archivos para subir contenido
- **Bases de datos**: Creación y gestión de bases de datos
- **FTP**: Configuración de acceso FTP/SFTP
- **Git**: Integración con repositorios Git
- **SSH Terminal**: Acceso a la consola del servidor

<!-- Captura sugerida: menú lateral de Plesk mostrando todas las opciones -->

---

## 3. Gestión de Dominios

### 3.1 Crear subdominios

Los subdominios permiten organizar diferentes proyectos o secciones de tu sitio web bajo el mismo dominio principal.

**Pasos:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **Añadir subdominio**

<!-- Captura sugerida: botón "Añadir subdominio" en la parte superior -->

3. Completa la configuración:
   - **Nombre del subdominio**: Escribe el nombre del subdominio (normalmente el del proyecto que quieres subir)
   - **Raíz del documento**: Elige el directorio para guardar el proyecto

<!-- Captura sugerida: formulario de creación de subdominio mostrando campos a completar -->

**Importante sobre la ruta del documento:**

En nuestro caso será dentro de `httpdocs`. Por ejemplo, si queremos poner la aplicación al tema 4 de DWES, la ruta sería:
```
/httpdocs/JTGDWESProyectoTema4
```

**Nota crítica:** Asegúrate de que el nombre de la carpeta sea solo el del proyecto, ya que por defecto Plesk pone el dominio entero a esta carpeta. Tendrás que borrarlo antes de darle aceptar.

<!-- Captura sugerida: campo "Raíz del documento" mostrando la ruta correcta -->

4. Haz clic en **Aceptar**

Una vez creado, el subdominio estará disponible y apuntará a la carpeta especificada.

---

### 3.2 Configuración DNS

Para crear registros DNS personalizados (por ejemplo, para sitios virtuales):

**Pasos:**

1. En **Sitios web y dominios**, selecciona tu dominio principal
2. Ve al apartado **Hosting y DNS**
3. Haz clic en **DNS**

<!-- Captura sugerida: sección DNS dentro de Hosting y DNS -->

4. Haz clic en **Añadir registro**
5. Configura el registro:
   - **Tipo de registro**: Selecciona **A**
   - **Nombre de dominio**: Escribe el nombre que quieras para el subdominio
   - **TTL**: Dejar por defecto
   - **Dirección IP**: Introduce la dirección IP de tu servidor

<!-- Captura sugerida: formulario de creación de registro DNS tipo A -->

6. Haz clic en **Aceptar**
7. Plesk te indicará que hay que actualizar los registros DNS. Haz clic en **Actualizar**

El registro DNS ya estará activo. Este método es útil cuando necesitas configurar sitios virtuales en el servidor de desarrollo.

---

## 4. Despliegue de Aplicaciones

### 4.1 Método 1: Subida manual (FTP/SFTP)

Este método es ideal para subidas rápidas o cuando no trabajas con control de versiones.

**Configurar acceso FTP:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **FTP**
3. Selecciona tu cuenta de usuario (haz clic en tu nombre)

<!-- Captura sugerida: lista de cuentas FTP con el nombre de usuario -->

4. Configura los datos de conexión:
   - **Nombre de usuario**: Visualiza tu nombre de usuario
   - **Contraseña**: Establece o cambia la contraseña
   - **Acceso SSH**: Verifica que sea `/bin/bash`
   - **Dirección IP**: Anota la IP para la conexión

<!-- Captura sugerida: formulario de configuración de cuenta FTP mostrando todos los campos -->

5. Haz clic en **Guardar**

**Conexión con cliente FTP/SFTP:**

Puedes usar cualquiera de estos clientes para conectarte:
- **FileZilla**
- **WinSCP**
- **MobaXterm**

**Datos de conexión:**
- **Protocolo**: SFTP (recomendado) o FTP
- **Host**: Dirección IP anotada anteriormente
- **Usuario**: Tu nombre de usuario
- **Contraseña**: La contraseña configurada
- **Puerto**: 22 (SFTP) o 21 (FTP)

**Subir archivos:**

1. Conecta con el cliente FTP/SFTP usando los datos anteriores
2. Navega hasta la carpeta destino en el servidor (por ejemplo: `/httpdocs/nombre-proyecto`)
3. Selecciona los archivos y carpetas de tu proyecto local
4. Arrástralos o súbelos según el cliente que uses

<!-- Captura sugerida: interfaz de FileZilla o similar mostrando la transferencia de archivos -->

**Alternativa desde Plesk:**

También puedes subir archivos directamente desde la interfaz de Plesk:

1. Ve al menú lateral izquierdo y haz clic en **Archivos**

<!-- Captura sugerida: menú de Plesk con "Archivos" resaltado -->

2. Navega hasta la carpeta `httpdocs` del directorio principal

<!-- Captura sugerida: gestor de archivos mostrando la carpeta httpdocs -->

3. Entra en la carpeta `httpdocs`
4. Arrastra las carpetas desde tu carpeta del proyecto local directamente a la interfaz web

<!-- Captura sugerida: gestor de archivos de Plesk con archivos siendo arrastrados -->

---

### 4.2 Método 2: Desde GitHub Release

Este método es recomendable cuando quieres subir versiones estables y específicas de tu proyecto.

**Pasos:**

1. **Descargar release desde GitHub:**
   - Ve a tu repositorio en GitHub
   - Accede a la sección **Releases**
   - Localiza la versión estable que quieres desplegar
   - Descarga el **Source code (zip)**

<!-- Captura sugerida: sección Releases de GitHub mostrando el botón de descarga del zip -->

2. **Preparar los archivos:**
   - Extrae el archivo ZIP en tu PC
   - Si es necesario, modifica archivos de configuración (como credenciales de base de datos, rutas, etc.) antes de subirlos

3. **Subir al servidor:**
   - Conecta por FTP/SFTP como se explicó en el [Método 1](#41-método-1-subida-manual-ftpsftp)
   - Sube los archivos extraídos a la carpeta del proyecto en el servidor
   - Sobrescribe archivos si es necesario

Este método asegura que despliegas exactamente la versión testeada y marcada como release en GitHub.

---

### 4.3 Método 3: Integración GitHub (CI/CD automático)

Este método permite que cada vez que hagas `push` a la rama principal, los cambios se desplieguen automáticamente en Plesk. Es el más profesional y eficiente para desarrollo continuo.

**Requisitos previos:**
- Tener un repositorio en GitHub con tu proyecto
- Tener acceso a Plesk con permisos de configuración Git

**Pasos:**

**1. Configurar repositorio en Plesk:**

- Abre tu repositorio en GitHub (lo necesitarás para copiar URLs y configurar claves)
- En Plesk, ve a **Sitios web y dominios**
- Selecciona tu dominio principal
- Haz clic en el icono de **Git**

<!-- Captura sugerida: icono de Git en Sitios web y dominios -->

**2. Copiar URL SSH de GitHub:**

- En tu repositorio de GitHub, haz clic en **Code**
- Selecciona la pestaña **SSH**
- Copia la URL (formato: `git@github.com:usuario/repositorio.git`)

<!-- Captura sugerida: menú desplegable de GitHub mostrando la URL SSH -->

**3. Añadir repositorio en Plesk:**

- En la página de Git de Plesk, haz clic en **Añadir repositorio**
- Pega la URL SSH de GitHub

<!-- Captura sugerida: formulario de Plesk para añadir repositorio Git -->

**4. Crear y configurar clave SSH:**

- Al pegar la URL, aparecerá una sección para crear una **clave SSH pública**
- Haz clic en **Generar clave** (debes crear una clave para cada repositorio)
- Copia la clave SSH generada

<!-- Captura sugerida: ventana de Plesk mostrando la clave SSH generada -->

**5. Añadir Deploy Key en GitHub:**

- Ve a tu repositorio en GitHub
- Ve a **Settings** → **Deploy keys**
- Haz clic en **Add deploy key**
- Pega la clave SSH copiada de Plesk
- Ponle un nombre significativo (por ejemplo: "Plesk Deploy Key - Proyecto X")
- **Importante**: Deja **desmarcado** "Allow write access"
- Haz clic en **Add key**

<!-- Captura sugerida: formulario de GitHub para añadir Deploy Key -->

**6. Completar configuración en Plesk:**

- Vuelve a Plesk
- Configura los siguientes campos:
  - **Nombre del repositorio**: Modifica el nombre como quieras (será visible en el panel de Git)
  - **Modo de despliegue**: Asegúrate de que está en **Automático**
  - **Ruta de acceso**: Elige la carpeta de tu proyecto dentro de `httpdocs`
    - Si no existe, créala (recomendable que esté vacía)
    - Ejemplo: `/httpdocs/JTGDWESProyectoTema4`

<!-- Captura sugerida: formulario de configuración del repositorio en Plesk mostrando todos los campos -->

- Haz clic en **Crear**

Plesk clonará el repositorio de GitHub en la ruta especificada.

**7. Configurar Webhook para despliegue automático:**

Si no ha habido errores, verás tu repositorio en la lista de repositorios Git en Plesk.

- Haz clic en el **icono de configuración** del repositorio (icono del medio abajo a la derecha de la tarjeta)

<!-- Captura sugerida: tarjeta del repositorio en Plesk con el icono de configuración resaltado -->

- En la página de configuración, localiza y copia el **URL de webhook**

<!-- Captura sugerida: campo de URL webhook en la configuración del repositorio -->

**8. Añadir Webhook en GitHub:**

- Ve a tu repositorio en GitHub
- Ve a **Settings** → **Webhooks**
- Haz clic en **Add webhook**
- Configura el webhook:
  - **Payload URL**: Pega el URL copiado de Plesk, pero añade `plesk.` al principio del dominio
    - Ejemplo: Si el URL es `https://ejemplo.com/git-deploy`, cámbialo a `https://plesk.ejemplo.com/git-deploy`
  - **Content type**: Selecciona `application/json`
  - **Which events would you like to trigger this webhook?**: Asegúrate de que esté marcada la opción **Just the push event**
  - **Active**: Dejar marcado

<!-- Captura sugerida: formulario de configuración de webhook en GitHub -->

- Haz clic en **Add webhook**

**Listo:**

Cada vez que hagas `push` a la rama principal (master o main), GitHub enviará una notificación a Plesk mediante el webhook, y Plesk automáticamente descargará y desplegará los cambios en el servidor.

Para verificar que funciona, haz un cambio en tu proyecto, súbelo a GitHub y observa si se actualiza automáticamente en el servidor.

---

## 5. Gestión de Bases de Datos

### 5.1 Crear base de datos en Plesk

**Pasos:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **Bases de datos**

<!-- Captura sugerida: menú mostrando la opción "Bases de datos" -->

3. Haz clic en **Añadir base de datos**

<!-- Captura sugerida: botón "Añadir base de datos" -->

4. Completa el formulario:

   - **Nombre de la base de datos**: Introduce el nombre de la base de datos
   - **Sitio relacionado**: Selecciona el subdominio asociado al proyecto (opcional pero recomendado)
   - **Usuario de la base de datos**: Crea un nuevo usuario
     - **Nombre de usuario**: Introduce el nombre del usuario
     - **Contraseña**: Crea una contraseña segura que cumpla con los requisitos de Plesk
     - **Confirmar contraseña**: Repite la contraseña

<!-- Captura sugerida: formulario de creación de base de datos mostrando todos los campos -->

5. Haz clic en **Aceptar** o **Crear**

Una vez creada, la base de datos aparecerá en la lista y tendrás la opción de abrirla en **phpMyAdmin**.

---

### 5.2 Importar datos (volcado SQL)

Una vez creada la base de datos, puedes importar la estructura y los datos iniciales.

**Preparar archivos:**

1. Crea un archivo `.sql` con los scripts de:
   - Creación de tablas
   - Carga inicial de datos (si la hay)
   - (Opcionalmente, script de borrado)

2. Comprime el archivo `.sql` en formato **ZIP**

**Importar en Plesk:**

1. En la lista de bases de datos, selecciona la base de datos que acabas de crear
2. Haz clic en **Importar volcado**

<!-- Captura sugerida: botón "Importar volcado" en la gestión de base de datos -->

3. Configura la importación:
   - **Ubicación**: Selecciona **Desde el ordenador local**
   - Haz clic en **Seleccionar un archivo .sql .zip**
   - Selecciona el archivo ZIP que preparaste

<!-- Captura sugerida: ventana de importación mostrando opciones y botón de selección de archivo -->

4. Haz clic en **Importar volcado**

Plesk procesará el archivo y ejecutará los scripts SQL en la base de datos.

**Verificar importación:**

1. En la lista de bases de datos, haz clic en **phpMyAdmin**

<!-- Captura sugerida: botón de acceso a phpMyAdmin -->

2. En phpMyAdmin, verás la base de datos en el panel lateral izquierdo
3. Haz clic en la base de datos y verifica que las tablas se hayan creado correctamente

<!-- Captura sugerida: phpMyAdmin mostrando la base de datos y sus tablas en el panel lateral -->

---

### 5.3 Gestión con phpMyAdmin

phpMyAdmin es una herramienta web para gestionar bases de datos MySQL/MariaDB.

**Acceso:**

1. Desde Plesk, ve a **Bases de datos**
2. Selecciona la base de datos que quieres gestionar
3. Haz clic en **phpMyAdmin**

<!-- Captura sugerida: botón de phpMyAdmin en la página de gestión de bases de datos -->

Se abrirá phpMyAdmin ya autenticado en la base de datos seleccionada. Solo podrás gestionar esa base de datos específica, no verás todas las bases de datos del servidor.

**Ejecutar consultas SQL:**

1. Ve a la pestaña **SQL** en la parte superior

<!-- Captura sugerida: pestaña SQL en phpMyAdmin -->

2. Escribe o copia el código SQL que quieras ejecutar en el área de texto
3. **Guardar consulta en favoritos (recomendado):**
   - Antes de ejecutar, en la parte inferior de la página marca **Guardar esta consulta en favoritos**
   - Ponle un nombre descriptivo (por ejemplo: "Crear tablas", "Carga inicial", "Borrar datos")

<!-- Captura sugerida: área de texto SQL con la opción de guardar en favoritos marcada -->

4. Haz clic en **Continuar** en la parte inferior derecha para ejecutar la consulta

**Reutilizar consultas guardadas:**

Una vez guardadas, aparece una nueva sección en la parte inferior: **Consulta guardada en favoritos**

<!-- Captura sugerida: sección de consultas guardadas mostrando la lista desplegable -->

- Despliega la lista y selecciona la consulta que quieras
- Haz clic en **Continuar** para ejecutarla de nuevo

Esto es muy útil para scripts que ejecutas frecuentemente, como resetear la base de datos o cargar datos de prueba.

---

### 5.4 Configuración de conexión

**Datos de conexión:**

Para conectar tu aplicación PHP con la base de datos creada en Plesk, utiliza los siguientes datos:

- **Host**: `localhost`
- **Nombre de la base de datos**: El nombre que indicaste al crear la base de datos
- **Usuario**: El usuario que creaste al configurar la base de datos
- **Contraseña**: La contraseña que estableciste para ese usuario

<!-- Captura sugerida: resumen de datos de conexión visible en Plesk -->

**Configuración con variables de entorno (.htaccess):**

Para mayor seguridad, es recomendable **no incluir las credenciales directamente en el código** de tu aplicación, especialmente en entornos de producción.

**Método recomendado: Variables de entorno**

**Paso 1: Crear variables de entorno en .htaccess**

Crea o edita el archivo `.htaccess` en la raíz de tu proyecto (dentro de `httpdocs/nombre-proyecto`) y añade las siguientes líneas:
```apache
SetEnv DB_DSN "mysql:host=localhost;dbname=DBGJLDWESProyectoTema4"
SetEnv DB_USERNAME "userGJLDWESProyectoTema4"
SetEnv DB_PASSWORD "5813Libro-Puro"
```

**Importante**: Reemplaza los valores con tus datos reales de conexión.

**Paso 2: Crear archivo de configuración PHP**

Crea un archivo de configuración (por ejemplo: `conf/confDBPDO.php`) con el siguiente contenido:
```php
<?php
// Lee las variables de entorno para DSN, USERNAME y PASSWORD
define('DSN', getenv('DB_DSN'));
define('USERNAME', getenv('DB_USERNAME'));
define('PASSWORD', getenv('DB_PASSWORD'));

// Verificar si las variables se cargaron correctamente (opcional pero recomendado)
if (!DSN || !USERNAME || !PASSWORD) {
    // Aquí puedes lanzar una excepción o registrar un error si faltan las credenciales
    error_log("ERROR: Las variables de entorno de la base de datos no están definidas.");
}
?>
```

**Ventajas de este método:**

- Las credenciales no aparecen en el código fuente
- El archivo de configuración es el mismo en desarrollo y producción
- Puedes usar diferentes credenciales en cada entorno sin modificar el código
- Mayor seguridad si el código se sube a repositorios públicos

**Separación de entornos:**

El objetivo es que el archivo de configuración en el entorno de **explotación** (rama `master`) no tenga las contraseñas, usuarios ni datos de la base de datos directamente en el código, a diferencia del entorno de **desarrollo** (rama `developer` o `develop`) que puede tenerlos para facilitar el trabajo local.

Como el resto del código PHP es el mismo en los dos entornos, ambos usan el mismo archivo de configuración (por ejemplo: `confDBPDO.php`).

**Gestión con Git:**

**Antes de empezar:** Guarda una copia de seguridad del archivo de configuración por si hay que volver a restaurarlo.

**Paso 1: Excluir del repositorio**

Añade el archivo de configuración al `.gitignore` (hazlo desde la rama de desarrollo):
```
conf/confDBPDO.php
```

**Paso 2: Quitar del seguimiento de Git**

Ejecuta el siguiente comando para dejar de rastrear el archivo (pero mantenerlo en el disco):
```bash
git rm --cached conf/confDBPDO.php
```

Si quieres estar más seguro, puedes borrarlo completamente del repositorio sin usar `--cached` y luego volver a crearlo al final.

**Paso 3: Verificar que se eliminó de GitHub**

En este punto, el archivo no debería estar en GitHub en ninguna rama.

**Paso 4: Crear versiones específicas**

- **Para desarrollo**: Crea el archivo con las credenciales directamente en el código (más cómodo para trabajar en local)
- **Para producción**: Crea el archivo con `getenv()` para leer variables de entorno del `.htaccess`

**Paso 5: Subir manualmente a producción**

El archivo de configuración para producción (el que usa `getenv()`) debe subirse manualmente al servidor por FTP/SFTP, ya que no estará en el repositorio.

De esta forma:
- El código en GitHub no contiene credenciales sensibles
- Cada entorno tiene su propia configuración
- El despliegue automático no sobrescribe la configuración de producción

---

## 6. Acceso Avanzado

### 6.1 Conexión SFTP

SFTP (SSH File Transfer Protocol) permite transferir archivos de forma segura usando el protocolo SSH.

**Obtener datos de conexión:**

Si no tienes los datos de conexión, sigue estos pasos:

1. En Plesk, ve a **Sitios web y dominios**
2. Haz clic en **FTP**
3. Selecciona tu cuenta (haz clic en tu nombre de usuario)
4. En la ventana de configuración verás:
   - **Nombre de usuario**
   - **Dirección IP del servidor**
   - **Acceso SSH**: Debería ser `/bin/bash`

5. Configura o cambia la contraseña si es necesario
6. Haz clic en **Guardar**

**Conectar con MobaXterm (u otro cliente SFTP):**

1. Abre MobaXterm
2. Haz clic en **Session** → **SFTP**
3. Introduce los datos:
   - **Remote host**: Dirección IP del servidor
   - **Username**: Tu nombre de usuario
   - **Port**: 22
4. Haz clic en **OK**
5. Introduce la contraseña cuando te la pida

Ya estarás conectado y podrás transferir archivos de forma segura.

---

### 6.2 Conexión SSH

SSH permite acceder a la línea de comandos del servidor para ejecutar comandos directamente.

**Opción 1: Desde la terminal de Plesk**

Esta es la forma más rápida y no requiere configuración adicional.

1. En Plesk, ve a **Sitios web y dominios**
2. Haz clic en **SSH Terminal**

<!-- Captura sugerida: botón SSH Terminal en el menú de Plesk -->

3. Se abrirá la consola de comandos directamente en el navegador
4. Ya puedes ejecutar comandos según los permisos que tengas

<!-- Captura sugerida: terminal SSH abierta en el navegador mostrando el prompt -->

**Opción 2: Desde un cliente SSH externo (MobaXterm u otro)**

1. Abre MobaXterm
2. Haz clic en **Session** → **SSH**
3. Introduce los datos:
   - **Remote host**: Dirección IP del servidor
   - **Username**: Tu nombre de usuario (el mismo de SFTP)
   - **Port**: 22
4. Haz clic en **OK**
5. Introduce la contraseña cuando te la pida

Ya estarás conectado a la terminal del servidor y podrás ejecutar comandos.

---

## 7. Configuración Avanzada

### 7.1 Archivo .htaccess

El archivo `.htaccess` es un archivo de configuración de Apache que permite modificar el comportamiento del servidor web para un directorio específico.

**Ubicación:** Raíz del proyecto (dentro de `httpdocs/nombre-proyecto`)

**Usos comunes:**

#### **1. Redirección a índice personalizado**

Especifica qué archivo debe cargarse por defecto cuando se accede al directorio:
```apache
directoryindex indexProyectoCIB.php
```

#### **2. Habilitar autenticación HTTP en PHP**

Para que funcione el método `header('WWW-Authenticate: Basic realm="Contenido restringido"')` en PHP:
```apache
<IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
</IfModule>
```

#### **3. Páginas de error personalizadas**

Redirige los errores HTTP a páginas personalizadas en la carpeta `error` de la raíz del dominio/subdominio:
```apache
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
ErrorDocument 500 /error/500.html
```

**Nota:** Debes crear previamente las páginas HTML en la carpeta `/error/` de tu proyecto.

#### **4. Redirección HTTP a HTTPS**

Fuerza el uso de HTTPS para todas las peticiones:
```apache
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://10.199.8.153/$1 [R,L]
```

**Importante:** Cambia la dirección IP por la de tu servidor.

#### **5. Variables de entorno para bases de datos**

Como se explicó en la [sección 5.4](#54-configuración-de-conexión), puedes definir credenciales de base de datos como variables de entorno:
```apache
SetEnv DB_DSN "mysql:host=localhost;dbname=DBGJLDWESProyectoTema4"
SetEnv DB_USERNAME "userGJLDWESProyectoTema4"
SetEnv DB_PASSWORD "5813Libro-Puro"
```

#### **Ejemplo completo de .htaccess**
```apache
# Redirección a índice personalizado
directoryindex indexProyectoCIB.php

# Para que funcione el método header('WWW-Authenticate: Basic realm="Contenido restringido"') en PHP
<IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
</IfModule>

# Redirección de errores a páginas personalizadas en carpeta error de raíz del dominio/subdominio
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
ErrorDocument 500 /error/500.html

# Redirección http a https (cambiar IP por la de tu servidor)
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://10.199.8.153/$1 [R,L]

# Variables de entorno para BBDD
SetEnv DB_DSN "mysql:host=localhost;dbname=DBGJLDWESProyectoTema4"
SetEnv DB_USERNAME "userGJLDWESProyectoTema4"
SetEnv DB_PASSWORD "5813Libro-Puro"
```

**Recursos adicionales:**

Para más información sobre configuraciones avanzadas de `.htaccess`, consulta:  
https://www.ionos.es/digitalguide/hosting/cuestiones-tecnicas/los-mejores-trucos-para-archivos-htaccess/

---

> **Enrique Nieto Lorenzo**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  
> Despliegue de Aplicaciones Web