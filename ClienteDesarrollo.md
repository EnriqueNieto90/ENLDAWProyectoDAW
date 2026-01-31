[< Volver al menú principal](README.md)

# CLIENTE DE DESARROLLO - WINDOWS 11

| CFGS Desarrollo de Aplicaciones Web |
|:-----------------------------------:|
| ![Portada](images/portada.jpg)      |
| **Instalación, Configuración y Documentación del Cliente de Desarrollo** |

> **Autor:** Enrique Nieto Lorenzo    
> **Curso:** 2024/2025  
> **Módulo:** Despliegue de Aplicaciones Web  
> **Centro:** IES Los Sauces

---

## Índice

- [1. Configuración Inicial](#1-configuración-inicial)
  - [1.1 Requisitos del Sistema](#11-requisitos-del-sistema)
  - [1.2 Nombre y Configuración de Red](#12-nombre-y-configuración-de-red)
  - [1.3 Cuentas Administradoras](#13-cuentas-administradoras)
  - [1.4 Verificación de Conectividad](#14-verificación-de-conectividad)
- [2. Navegadores](#2-navegadores)
- [3. MobaXterm](#3-mobaxterm)
- [4. NetBeans](#4-netbeans)

---

## 1. Configuración Inicial

### 1.1 Requisitos del Sistema

**Sistema Operativo:** Windows 11 (compatible con Windows 10)

**Software necesario:**
- Navegador moderno (Firefox o Chrome)
- MobaXterm
- NetBeans IDE 20

### 1.2 Nombre y Configuración de Red

**Verificar configuración de red:**

Abrir CMD (Símbolo del sistema) y ejecutar:
```cmd
ipconfig
```

Anotar los siguientes datos:
- **Dirección IPv4:** (ejemplo: 192.168.1.100)
- **Puerta de enlace:** (ejemplo: 192.168.1.1)
- **Máscara de subred:** (ejemplo: 255.255.255.0)

Estos datos son necesarios para verificar la conectividad con el servidor de desarrollo.

### 1.3 Cuentas Administradoras

**Cuenta local de Windows:**

Verificar que se tienen permisos de administrador en el equipo Windows para poder instalar software.

**Cuentas en el servidor:**

> - **Servidor:** daw-used (Ubuntu Server 24.04)
> - **Usuario administrador:** miadmin/paso
> - **Usuario web:** operadorweb/paso

### 1.4 Verificación de Conectividad

Antes de configurar las herramientas de desarrollo, es importante verificar que existe conectividad con el servidor.

#### Prueba de Conectividad con Ping

Abrir la terminal de Windows (CMD):

<!-- Captura sugerida: CMD de Windows abierto -->

Ejecutar el comando ping con la IP del servidor:
```cmd
ping 10.199.10.22
```

(Reemplazar con la IP de tu servidor)

<!-- Captura sugerida: CMD mostrando respuesta exitosa del comando ping -->

**Salida esperada:**
```
Pinging 10.199.10.22 with 32 bytes of data:
Reply from 10.199.10.22: bytes=32 time<1ms TTL=64
Reply from 10.199.10.22: bytes=32 time<1ms TTL=64
Reply from 10.199.10.22: bytes=32 time<1ms TTL=64
Reply from 10.199.10.22: bytes=32 time<1ms TTL=64

Ping statistics for 10.199.10.22:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

Si todos los paquetes se reciben correctamente, la conexión con el servidor es exitosa.

#### Conexión SSH desde CMD

Una vez verificada la conectividad básica, probar la conexión SSH:
```cmd
ssh miadmin@10.199.10.22
```

Se solicitará la contraseña (`paso`).

<!-- Captura sugerida: CMD mostrando conexión SSH exitosa con prompt del servidor -->

**Prompt esperado después de conectar:**
```
miadmin@daw-used:~$
```

Ya se puede trabajar directamente en el servidor desde la terminal de Windows.

**Para salir de la sesión SSH:**
```bash
exit
```

<!-- Captura sugerida: CMD después de ejecutar exit, volviendo al prompt de Windows -->

---

## 2. Navegadores

### Firefox (Recomendado)

**Descarga:** https://www.mozilla.org/es-ES/firefox/new/

Se puede utilizar cualquier navegador moderno para visualizar los proyectos web que están alojados en el servidor.

**Extensiones recomendadas:**

- **[uBlock Origin](https://addons.mozilla.org/es-ES/firefox/addon/ublock-origin/):** Bloquea anuncios y rastreadores.
- **[Bitwarden](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/):** Gestor de contraseñas seguro.
- **[Web Developer](https://addons.mozilla.org/es-ES/firefox/addon/web-developer/):** Herramientas de desarrollo web.

### Chrome/Edge (Alternativa)

Compatible con extensiones similares disponibles en Chrome Web Store.

### Acceso a Proyectos Web

Para visualizar un proyecto alojado en el servidor, se debe indicar la URL completa en el navegador:
```
http://10.199.10.22/nombre_proyecto/
```

O si el servidor tiene HTTPS configurado:
```
https://10.199.10.22/nombre_proyecto/
```

<!-- Captura sugerida: navegador mostrando un proyecto web accedido mediante la URL del servidor -->

**Herramientas de Desarrollador:**

- Presionar `F12` o `Ctrl+Shift+I` para abrir las herramientas de desarrollador
- **Consola:** Ver errores de JavaScript
- **Red:** Inspeccionar peticiones HTTP
- **Elementos:** Inspeccionar HTML y CSS

---

## 3. MobaXterm

MobaXterm es una terminal avanzada para Windows que integra SSH, SFTP, X11 y otras herramientas en una sola aplicación.

### Instalación

**Descarga:** https://mobaxterm.mobatek.net/download-home-edition.html

Existen dos versiones disponibles:

- **Portable Edition:** No requiere instalación (recomendado)
- **Installer Edition:** Instalación tradicional

**Instalación de la versión portable:**

1. Descargar el archivo ZIP
2. Extraer en una ubicación permanente (ejemplo: `C:\Tools\MobaXterm`)
3. Ejecutar `MobaXterm_Personal_XX.X.exe`

### Configuración de Sesión SSH

**Paso 1:** Click en el botón **Session** (esquina superior izquierda)

<!-- Captura sugerida: ventana principal de MobaXterm con botón Session destacado -->

**Paso 2:** Seleccionar **SSH** en la ventana que aparece

<!-- Captura sugerida: menú de selección de tipo de sesión mostrando SSH -->

**Paso 3:** Configurar los datos de conexión

- **Remote host:** `10.199.10.22` (IP del servidor)
- **Port:** `22`
- **Specify username:** Marcar checkbox e introducir `miadmin`

<!-- Captura sugerida: formulario de configuración SSH con los campos completados -->

**Paso 4:** Click en **OK**

La primera vez que se conecta, se pedirá la contraseña (`paso`).

**Gestión de contraseñas:**

En la primera conexión puede aparecer una ventana para guardar contraseñas:
- Si se acepta, se pedirá una contraseña maestra para MobaXterm
- Si no se acepta, se pedirá la contraseña en cada conexión

**Paso 5:** Conectar a la sesión

La sesión aparecerá en el panel **User sessions** (lado izquierdo). Seleccionarla para abrir la terminal:
```
miadmin@daw-used:~$
```

<!-- Captura sugerida: terminal de MobaXterm conectada mostrando el prompt del servidor -->

### Configuración de Sesión SFTP

**Paso 1:** Click en el botón **Session**

**Paso 2:** Seleccionar **SFTP**

<!-- Captura sugerida: menú de selección con SFTP destacado -->

**Paso 3:** Configurar los datos de conexión

- **Remote host:** `10.199.10.22`
- **Port:** `22`
- **Username:** `operadorweb`

<!-- Captura sugerida: formulario de configuración SFTP completado -->

**Paso 4:** Click en **OK**

Se pedirá la contraseña (`paso`).

<!-- Captura sugerida: ventana solicitando contraseña -->

**Paso 5:** Usar la conexión SFTP

Seleccionar la sesión SFTP en **User sessions**. Se abrirá una ventana con dos paneles:

- **Panel izquierdo:** Archivos locales (Windows)
- **Panel derecho:** Archivos remotos (Servidor)

<!-- Captura sugerida: ventana SFTP mostrando ambos paneles -->

**Funcionalidades disponibles:**

La barra de herramientas del panel derecho incluye los siguientes iconos:

| Icono | Función |
|-------|---------|
| ↑ | Subir un nivel en la estructura de directorios |
| ↻ | Actualizar la vista de archivos |
| 📁+ | Crear nueva carpeta |
| 📄+ | Crear nuevo archivo |
| 📂 | Abrir carpeta seleccionada |
| 🗑️ | Eliminar elemento seleccionado |
| ⬆️ | Subir archivo (abre explorador de Windows) |
| ⬇️ | Descargar archivo (abre explorador de Windows) |

**Operaciones comunes:**

- **Subir archivos:** Arrastrar desde panel izquierdo al derecho
- **Descargar archivos:** Arrastrar desde panel derecho al izquierdo
- **Editar archivos:** Doble click en archivos de texto (HTML, PHP, CSS, etc.) para abrirlos en el editor integrado de MobaXterm
- **Eliminar:** Click derecho en archivo/carpeta → Delete

<!-- Captura sugerida: ventana SFTP con archivo siendo editado en el editor integrado -->

---

## 4. NetBeans

Apache NetBeans es un IDE completo para desarrollo Java, PHP, HTML5 y más. Integra servidor, depurador y control de versiones.

### Instalación

**Descarga:** https://netbeans.apache.org/front/main/download/

**Versión recomendada:** Apache NetBeans 20

**Enlace directo a la descarga:** https://netbeans.apache.org/front/main/download/nb20/

**Requisitos previos:**
- Java JDK 11 o superior (incluido en el instalador)

**Proceso de instalación:**

1. Descargar el instalador para Windows
2. Ejecutar el instalador
3. Seguir el asistente de instalación (Next → Next → Install)
4. Esperar a que finalice la instalación

### Configuración Inicial

**Primera ejecución:**

1. Aceptar la licencia de uso
2. Seleccionar los plugins necesarios (PHP y HTML5 recomendados)
3. Configurar la apariencia (tema claro u oscuro)

**Configurar carpeta de proyectos:**

Es recomendable cambiar la ubicación por defecto donde se guardan los proyectos.

1. Tools → Options → Miscellaneous → Files
2. Cambiar "User Directory" a una ruta personalizada
   - Ejemplo: `D:\Proyectos_NetBeans`

### Creación de Proyecto PHP con Conexión Remota

#### Proceso Completo de Creación

**Paso 1:** Iniciar nuevo proyecto

Click en **File → New Project** o en el icono naranja con símbolo "+"

<!-- Captura sugerida: menú File con New Project destacado -->

<!-- Captura sugerida: icono naranja con + para nuevo proyecto -->

**Paso 2:** Seleccionar tipo de proyecto

En la ventana que aparece:

- **Categories:** Seleccionar `PHP`
- **Projects:** Seleccionar `PHP Application from Remote Server`
- Click en **Next**

<!-- Captura sugerida: ventana de selección de tipo de proyecto con PHP Application from Remote Server seleccionado -->

**Paso 3:** Configurar nombre y ubicación del proyecto

- **Project Name:** Introducir el nombre del proyecto (ejemplo: `MiProyectoPHP`)
- **Project Location:** Cambiar la ruta a la carpeta personal de proyectos
- Click en **Next**

<!-- Captura sugerida: ventana de configuración de nombre y ubicación del proyecto -->

**Paso 4:** Configurar la URL del proyecto

**Run Configuration:**
- **Project URL:** `http://10.199.10.22/MiProyectoPHP`

En la sección **Remote Connection:**
- Click en el botón **Manage...**

<!-- Captura sugerida: ventana de configuración de URL con botón Manage destacado -->

**Paso 5:** Configurar conexión remota SFTP

En la ventana "Manage Remote Connections", hacer click en **Add** para crear una nueva conexión y completar los siguientes datos:

- **Connection Name:** `daw-used` (o el nombre que prefieras)
- **Type:** `SFTP`
- **Host Name:** `10.199.10.22` (IP del servidor)
- **Port:** `22`
- **User Name:** `operadorweb`
- **Password:** `paso`
- **Initial Directory:** `/var/www/html`

<!-- Captura sugerida: formulario de conexión remota SFTP completado -->

**Paso 6:** Probar la conexión

Click en el botón **Test Connection**

<!-- Captura sugerida: botón Test Connection -->

**Mensaje esperado:**
```
Connection succeeded
```

<!-- Captura sugerida: mensaje de confirmación "Connection succeeded" -->

Si la conexión es exitosa, aparecerá un mensaje de confirmación. Click en **Yes** para continuar.

Click en **OK** para guardar la conexión.

**Paso 7:** Seleccionar la conexión y configurar directorio remoto

De vuelta en la ventana de configuración del proyecto:

- **Remote Connection:** Seleccionar `daw-used` de la lista desplegable
- **Upload Directory:** `/var/www/html/MiProyectoPHP`

Click en **Next**

<!-- Captura sugerida: configuración de directorio remoto completada -->

**Paso 8:** Mensaje de confirmación de conexión

Aparecerá nuevamente un mensaje de confirmación de conexión. Click en **Yes**.

<!-- Captura sugerida: mensaje de confirmación de conexión -->

**Paso 9:** Seleccionar archivos a descargar

Si la carpeta remota ya contiene archivos, aparecerá una lista para seleccionar cuáles descargar al proyecto local.

- Marcar los archivos que se deseen descargar
- Click en **Finish**

<!-- Captura sugerida: ventana de selección de archivos remotos para descargar -->

**Paso 10:** Confirmación final

Aparecerá nuevamente el mensaje de confirmación. Click en **Yes**.

El proyecto se creará y aparecerá en el panel **Projects** (lado izquierdo de NetBeans).

<!-- Captura sugerida: proyecto creado visible en el panel Projects de NetBeans -->

### Apertura de Proyecto Existente

**Paso 1:** Abrir el menú de proyectos

Click en **File → Open Project**

O hacer click en el icono de carpeta con flecha verde en la barra de herramientas

<!-- Captura sugerida: menú File con Open Project destacado -->

<!-- Captura sugerida: icono de abrir proyecto en la barra de herramientas -->

**Paso 2:** Navegar a la carpeta de proyectos

En la ventana que aparece, navegar hasta la carpeta donde están almacenados los proyectos de NetBeans.

Seleccionar la carpeta del proyecto deseado. Los proyectos de NetBeans se identifican con un icono especial.

<!-- Captura sugerida: explorador de archivos mostrando proyectos de NetBeans con su icono característico -->

**Paso 3:** Abrir el proyecto

Click en el botón **Open Project**

El proyecto se cargará y aparecerá en el panel **Projects**.

### Eliminación de Proyecto

**Paso 1:** Seleccionar el proyecto

En el panel **Projects**, hacer click derecho sobre el proyecto que se desea eliminar.

**Paso 2:** Seleccionar Delete

En el menú contextual, seleccionar **Delete**

<!-- Captura sugerida: menú contextual mostrando la opción Delete -->

**Paso 3:** Confirmar eliminación

Aparecerá una ventana de confirmación con las siguientes opciones:

- **Also delete sources under [ruta]:** Marcar esta opción si se desean eliminar también los archivos locales del proyecto
- Click en **Yes** para confirmar la eliminación

<!-- Captura sugerida: ventana de confirmación de eliminación con checkbox -->

**Nota importante:** Los archivos en el servidor NO se eliminan automáticamente. Deben borrarse manualmente usando MobaXterm (conexión SFTP) o desde el propio servidor.

### Depuración de Código PHP

#### ¿Qué es Depurar?

Depurar (debug) es el proceso de ejecutar código paso a paso para identificar y corregir errores. Permite:

- Pausar la ejecución en puntos específicos
- Inspeccionar el valor de las variables en tiempo real
- Seguir el flujo de ejecución del programa
- Comprender cómo se comporta el código

Es una herramienta fundamental para mejorar la estabilidad, fiabilidad y rendimiento del software.

#### Requisitos Previos

Para poder depurar código PHP en NetBeans, es necesario:

1. El servidor debe tener **Xdebug** instalado y configurado correctamente
2. NetBeans debe estar configurado para comunicarse con Xdebug

**Configurar NetBeans para Xdebug:**

1. Ir a **Tools → Options → PHP → Debugging**
2. Verificar la configuración:
   - **Debugger Port:** `9003` (puerto por defecto de Xdebug 3.x)
   - **Stop at First Line:** Marcar esta opción (recomendado para empezar)

#### Proceso de Depuración

**Paso 1: Establecer breakpoints**

Los breakpoints son puntos donde se desea pausar la ejecución del código para inspeccionar variables.

1. Abrir el archivo PHP que se desea depurar
2. Hacer click en el número de línea donde se quiere pausar la ejecución
3. Aparecerá un cuadrado rosa o rojo indicando el breakpoint

<!-- Captura sugerida: editor de código PHP con breakpoints marcados en líneas específicas -->

**Paso 2: Iniciar depuración**

Hacer click derecho en el archivo a depurar y seleccionar **Debug File**

O presionar `Ctrl+Shift+F5`

También se puede ir al menú **Debug → Debug File**

<!-- Captura sugerida: menú contextual con la opción Debug File -->

<!-- Captura sugerida: menú Debug con Debug File destacado -->

Se abrirá automáticamente el navegador y la ejecución se pausará en el primer breakpoint o en la primera línea (según configuración).

**Paso 3: Controles de depuración**

Cuando el código está pausado, aparece una barra de herramientas con controles de depuración:

| Botón | Atajo | Función |
|-------|-------|---------|
| ▶️ Continue | F5 | Continuar la ejecución hasta el siguiente breakpoint |
| ⏭️ Step Over | F8 | Ejecutar la línea actual sin entrar en funciones |
| ⏬ Step Into | F7 | Entrar dentro de la función en la línea actual |
| ⏫ Step Out | Ctrl+F7 | Salir de la función actual |
| ⏹️ Stop | Shift+F5 | Detener la depuración completamente |

<!-- Captura sugerida: barra de herramientas de depuración con los controles -->

**Paso 4: Inspeccionar variables**

Durante la depuración, el panel **Variables** (ubicado en la parte inferior) muestra:

- **Local Variables:** Variables del ámbito (scope) actual
- **Superglobals:** $_GET, $_POST, $_SESSION, $_SERVER, etc.
- **Constants:** Constantes PHP definidas

En este panel se puede ver el valor actual de cada variable en el momento en que el código está pausado.

<!-- Captura sugerida: panel de Variables mostrando valores de variables durante la depuración -->

**Paso 5: Continuar o detener**

- Para continuar hasta el siguiente breakpoint: Presionar **Continue** (F5) o el botón play
- Para finalizar la depuración: Presionar **Stop** (Shift+F5) o el botón de detener

**Consejos para depurar eficientemente:**

- Colocar breakpoints estratégicamente (no en cada línea)
- Inspeccionar variables antes y después de operaciones críticas
- Usar **Step Into** para entender funciones complejas
- Usar **Step Over** para funciones ya probadas y funcionales
- Verificar los valores de las superglobales cuando se trabaja con formularios

### Información del IDE

> **Página Oficial:** https://netbeans.apache.org/  
> **Versión Actual:** 20  
> **Descarga Directa:** https://netbeans.apache.org/front/main/download/nb20/  
> **Módulos Instalados:** PHP, HTML5, Git (incluidos por defecto)

---

> **Enrique Nieto Lorenzo**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  
> Despliegue de Aplicaciones Web