[< Volver al menú principal](README.md)

# GIT Y GITHUB - GUÍA COMPLETA

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

- [1. Introducción](#1-introducción)
- [2. Git - Instalación y Configuración](#2-git---instalación-y-configuración)
  - [2.1 ¿Qué es Git?](#21-qué-es-git)
  - [2.2 Instalación](#22-instalación)
  - [2.3 Configuración inicial](#23-configuración-inicial)
  - [2.4 Verificación](#24-verificación)
- [3. Conceptos Fundamentales](#3-conceptos-fundamentales)
  - [3.1 Repositorio](#31-repositorio)
  - [3.2 Área de staging](#32-área-de-staging)
  - [3.3 Commit](#33-commit)
  - [3.4 Ramas](#34-ramas)
  - [3.5 Remoto](#35-remoto)
- [4. Buenas Prácticas](#4-buenas-prácticas)
  - [4.1 Archivo .gitignore](#41-archivo-gitignore)
  - [4.2 Mensajes de commit](#42-mensajes-de-commit)
  - [4.3 Estrategias de branching](#43-estrategias-de-branching)
- [5. Gestión de Repositorio - Terminal](#5-gestión-de-repositorio---terminal)
  - [5.1 Inicializar o clonar repositorio](#51-inicializar-o-clonar-repositorio)
  - [5.2 Estado e historial](#52-estado-e-historial)
  - [5.3 Añadir cambios y hacer commit](#53-añadir-cambios-y-hacer-commit)
  - [5.4 Trabajar con ramas](#54-trabajar-con-ramas)
  - [5.5 Fusionar ramas (merge)](#55-fusionar-ramas-merge)
  - [5.6 Gestionar conflictos](#56-gestionar-conflictos)
  - [5.7 Conectar con remoto](#57-conectar-con-remoto)
  - [5.8 Sincronizar cambios (push/pull/fetch)](#58-sincronizar-cambios-pushpullfetch)
  - [5.9 Deshacer cambios](#59-deshacer-cambios)
  - [5.10 Etiquetas (tags)](#510-etiquetas-tags)
  - [5.11 Comandos avanzados](#511-comandos-avanzados)
- [6. Gestión de Repositorio - NetBeans](#6-gestión-de-repositorio---netbeans)
  - [6.1 Inicializar o clonar repositorio](#61-inicializar-o-clonar-repositorio)
  - [6.2 Estado e historial](#62-estado-e-historial)
  - [6.3 Añadir cambios y hacer commit](#63-añadir-cambios-y-hacer-commit)
  - [6.4 Trabajar con ramas](#64-trabajar-con-ramas)
  - [6.5 Fusionar ramas (merge)](#65-fusionar-ramas-merge)
  - [6.6 Gestionar conflictos](#66-gestionar-conflictos)
  - [6.7 Conectar con remoto](#67-conectar-con-remoto)
  - [6.8 Sincronizar cambios](#68-sincronizar-cambios)
  - [6.9 Deshacer cambios](#69-deshacer-cambios)
  - [6.10 Etiquetas (tags)](#610-etiquetas-tags)
- [7. GitHub - Plataforma y Colaboración](#7-github---plataforma-y-colaboración)
  - [7.1 ¿Qué es GitHub?](#71-qué-es-github)
  - [7.2 Crear un repositorio](#72-crear-un-repositorio)
  - [7.3 Autenticación SSH](#73-autenticación-ssh)
  - [7.4 Releases y Tags](#74-releases-y-tags)
  - [7.5 Colaboración](#75-colaboración)
    - [7.5.1 Añadir colaboradores](#751-añadir-colaboradores)
    - [7.5.2 Pull Requests](#752-pull-requests)
    - [7.5.3 Protección de ramas](#753-protección-de-ramas)
  - [7.6 Borrar un repositorio](#76-borrar-un-repositorio)
- [8. Flujos de Trabajo Completos](#8-flujos-de-trabajo-completos)
  - [8.1 Proyecto nuevo (local → GitHub)](#81-proyecto-nuevo-local--github)
  - [8.2 Proyecto existente (GitHub → local)](#82-proyecto-existente-github--local)
  - [8.3 Desarrollo con ramas](#83-desarrollo-con-ramas)
  - [8.4 Preparar una release](#84-preparar-una-release)

---

## 1. Introducción

**Git** es un sistema de control de versiones distribuido gratuito y de código abierto que se utiliza para rastrear los cambios en el código fuente durante el desarrollo de aplicaciones. Permite guardar historial de cambios, trabajar en paralelo con ramas, colaborar con otros desarrolladores y revertir a versiones anteriores sin perder el historial.

**GitHub** es una plataforma en la nube para alojar repositorios Git. Facilita la colaboración, revisión de código y gestión de proyectos. Se usa tanto en local como con plataformas como GitHub o GitLab para gestionar repositorios.

---

## 2. Git - Instalación y Configuración

### 2.1 ¿Qué es Git?

Git es un sistema de control de versiones que permite:
- Guardar cambios y trabajar en proyectos de forma organizada
- Trabajar en paralelo con ramas
- Colaborar con otros desarrolladores
- Revertir a versiones anteriores
- Crear ramas y volver a versiones anteriores sin perder historial

### 2.2 Instalación

**Descargar:**
Accede al sitio oficial de Git: https://git-scm.com/downloads

Haz clic en **Windows** y descarga la versión acorde con tu equipo. Existe una versión de escritorio y una portable. Al pinchar en el enlace de la versión que se quiera, la descarga comenzará automáticamente.

<!-- Captura sugerida: página de descarga de Git mostrando las opciones de Windows -->

**Proceso de instalación (Windows):**

1. Ejecuta el instalador descargado
2. Acepta los términos de licencia y sigue los pasos recomendados
3. **Carpeta de instalación:** Puedes dejar la que viene por defecto → Next
4. **Select Components:** Puedes dejar las opciones por defecto → Next
5. **Start Menu Folder:** Puedes dejar lo que viene por defecto → Next
6. **Editor por defecto:** Selecciona el editor de texto que prefieras. Se recomienda **Use NotePad as Git's default editor** o **Visual Studio Code** → Next

<!-- Captura sugerida: ventana del instalador mostrando la selección del editor por defecto -->

7. **Name of the initial branch:** Selecciona "Override the default branch name for new repositories" y escribe `master` (es el nombre que se creará por defecto al iniciar un repositorio nuevo) → Next

<!-- Captura sugerida: ventana del instalador para configurar el nombre de la rama inicial -->

8. **PATH environment:** Se recomienda seleccionar `Git from the command line and also from 3rd-party software` → Next
9. **SSH executable:** Puedes dejar la que viene por defecto (Use bundled OpenSSH) → Next
10. **HTTPS transport backend:** Puedes dejar la que viene por defecto para poder utilizar la librería de Windows → Next
11. **Line ending conversions:** Dejar por defecto → Next
12. **Terminal emulator:** Selecciona `Use Windows' default console window` → Next
13. El resto de opciones puedes dejarlas por defecto → Next hasta llegar a **Install**
14. Haz clic en **Install** para comenzar la instalación
15. Finaliza la instalación haciendo clic en **Finish**

<!-- Captura sugerida: ventana final del instalador mostrando la instalación completada -->

### 2.3 Configuración inicial

Para poder usar Git primero hay que configurar el nombre de usuario y el email. Estos datos se asocian a los commits.

Abre **Git Bash** desde el inicio de Windows, **Símbolo del sistema (CMD)**, **PowerShell** o la **terminal** y ejecuta:
```bash
git config --global user.name "Tu Nombre Completo"
git config --global user.email "tu.email@ejemplo.com"
```

**Opcional - Configurar editor:**
```bash
git config --global core.editor "nombre_editor"
```

### 2.4 Verificación

Para verificar que Git está correctamente instalado, abre una terminal nueva y ejecuta:
```bash
git --version
```

Deberías ver algo como:
```
git version 2.47.1.windows.1
```

Para verificar la configuración:
```bash
git config --list
```

Verifica que `user.name` y `user.email` estén configurados correctamente.

---

## 3. Conceptos Fundamentales

### 3.1 Repositorio
Carpeta que contiene tu proyecto y el historial de cambios (carpeta oculta `.git`).

### 3.2 Área de staging
Zona intermedia donde preparas los cambios antes de confirmarlos. Es el área donde se encuentran los archivos preparados para el siguiente commit.

### 3.3 Commit
Instantánea del proyecto en un momento dado, con un mensaje descriptivo que explica los cambios realizados.

### 3.4 Ramas
Líneas de desarrollo independientes que permiten trabajar en paralelo sin afectar la rama principal.

### 3.5 Remoto
Versión del repositorio alojada en un servidor externo (por ejemplo: GitHub, GitLab).

---

## 4. Buenas Prácticas

### 4.1 Archivo .gitignore

**¿Qué es?**  
Es un archivo que sirve para indicarle a Git qué archivos y carpetas no queremos que se haga seguimiento ni se suban a GitHub.

**Ubicación:** Raíz del proyecto

**¿Cómo crearlo?**  
Lo creamos con el nombre `.gitignore` y escribimos dentro esos archivos y carpetas que no queremos rastrear.

**Ejemplo de contenido:**
```gitignore
# Dependencias
node_modules/
vendor/

# Archivos de sistema
.DS_Store
Thumbs.db

# Configuración local
.env
config.local.php

# Archivos temporales
*.log
*.tmp
```

Al observar el explorador de archivos del IDE verás que se ponen en gris los archivos y carpetas listados en el `.gitignore`, lo que indica que Git está ignorándolos.

<!-- Captura sugerida: explorador de archivos mostrando archivos en gris después de añadir .gitignore -->

**Nota importante:** Este archivo tiene que estar en la raíz del proyecto.

---

### 4.2 Mensajes de commit

Se recomienda usar el estándar de [Commits Convencionales](https://www.conventionalcommits.org/es/v1.0.0/).

**Estructura:**
```
<tipo>(<ámbito>): <descripción>

[cuerpo opcional]

[notas de pie opcionales]
```

**Reglas importantes:**

1. **Usar lenguaje imperativo**: Los commits deben verse como "instrucciones" que cambian el proyecto más que como cosas que se han hecho. Ejemplo: "Add feature" no "Added feature"
2. **NO usar punto final**: El mensaje corto del commit (la primera línea) es el encabezado/título del commit, y al igual que en el periódico o las noticias, los títulos no llevan punto final
3. **NI puntos suspensivos**: Si vemos los commits como instrucciones, las instrucciones deben ser claras y estar completas. No deben crear duda a quien lo lea después
4. **Usar como máximo unos 50 caracteres**: Si tienes que explicar demasiado, tu commit probablemente hace demasiadas cosas. Si es posible dividirlo en varios commits, hazlo
5. **Línea en blanco entre título y cuerpo**: Si añades alguna explicación necesaria se pondrá en el cuerpo del commit. En él se puede explicar el qué y el por qué, no el cómo. Porque el mensaje puede mentir, pero el código no
6. **Usar prefijos para mejor legibilidad**: Estructura `<tipo>(<scope>): <descripción>`
   - El scope es opcional y sirve para indicar la parte del proyecto afectada (por ejemplo un módulo, componente o funcionalidad específica)

**Tipos de commit más comunes:**

| Tipo | Uso |
|------|-----|
| `feat` | Añade una nueva característica |
| `fix` | Arregla errores en el código |
| `refactor` | Refactorización del código como cambios de nombre de variables o funciones |
| `docs` | Cambios en la documentación |
| `style` | Cambios de formato, tabulaciones, espacios, etc en el código; no afectan al funcionamiento |
| `perf` | Cambios que mejoran el rendimiento del sitio |
| `build` | Cambios en el sistema de construcción, tareas de despliegue o instalación |
| `test` | Añade tests o refactoriza uno existente |
| `ci` | Cambios en la integración continua |

**Breaking Changes:**  
Si el commit contiene cambios que rompen la compatibilidad con las versiones anteriores (por ejemplo: eliminar o renombrar funciones o clases, cambiar parámetros obligatorios, etc.), se pondrá un signo de exclamación `!` antes de los dos puntos.

En el cuerpo del commit se podría poner `BREAKING CHANGE:` y explicar exactamente qué se ha eliminado o cambiado que rompe la compatibilidad.

**Ejemplos correctos:**
```bash
feat(auth): add Google OAuth login
fix(api): resolve null pointer exception in user endpoint
docs(readme): update installation instructions
refactor(utils): extract validation logic to separate module
```

---

### 4.3 Estrategias de branching

**Ramas principales:**
- `master` o `main`: Código en producción, estable
- `develop` o `developer`: Desarrollo activo

**Ramas de características (feature branches):**
- `feature/nombre-funcionalidad`
- Se crean desde `develop`
- Se fusionan de vuelta a `develop`

**Estrategias de merge:**

Normalmente se hace para unir la rama de desarrollo con la principal.

- **Merge con `--no-ff` (no fast-forward)**: Crea un commit de merge → **Usar para fusionar develop → master**. Esto une a la rama principal solo el último commit en ese momento, sin incluir todos los commits intermedios de la rama de desarrollo.

- **Merge con `--ff` (fast-forward)**: Mueve el puntero sin crear commit → **Usar para fusionar master → develop**. Esto mantiene ambas ramas en el mismo punto.

---

## 5. Gestión de Repositorio - Terminal

### 5.1 Inicializar o clonar repositorio

**Inicializar nuevo repositorio:**

Si tienes un proyecto solo en local y quieres hacer control de versiones:
```bash
cd /ruta/al/proyecto
git init
```

Esto creará una carpeta oculta `.git` en el proyecto que contendrá toda la configuración de Git para ese proyecto.

**Clonar repositorio existente:**

Si ya tienes un repositorio en GitHub y quieres usarlo en un ordenador nuevo:
```bash
git clone https://github.com/usuario/repositorio.git

# O especificar directorio:
git clone https://github.com/usuario/repositorio.git nombre-carpeta
```

Si no se pone directorio, se crea uno con el nombre que tiene el repositorio en GitHub. Si se pone uno, este tiene que estar vacío.

---

### 5.2 Estado e historial

**Ver estado actual:**

Para ver el estado actual del repositorio (qué archivos están modificados, cuáles no se han añadido al área de staging y cuáles están listos para hacer commit):
```bash
git status
```

**Ver historial de commits:**

Para ver el historial de commits del repositorio, con su hash, autor, fecha y mensaje:
```bash
git log
git log --oneline          # Versión compacta (una línea por commit)
git log --graph --oneline  # Con gráfico de ramas (recomendado)
```

Se pueden añadir opciones como `--oneline` para ver solo una línea por commit o `--graph` para ver un esquema más gráfico de las ramas. Se recomienda usar ambos para una mejor legibilidad.

**Ver cambios en archivos:**

Para ver los cambios en los archivos línea por línea (qué se ha añadido o eliminado desde el último commit):
```bash
git diff                   # Cambios no staged
git diff --staged          # Cambios en staging
git diff archivo.txt       # Cambios en archivo específico
```

`git diff` muestra los cambios que no han sido añadidos al área de staging. Para ver los cambios del área de staging usa la opción `--staged`.

---

### 5.3 Añadir cambios y hacer commit

**Añadir al staging:**

Para añadir los cambios al área de staging:
```bash
git add archivo.txt        # Archivo específico
git add .                  # Todos los cambios del directorio actual
```

**Quitar del staging (mantener cambios):**

Para quitar los cambios del área de staging pero mantenerlos modificados:
```bash
git reset archivo.txt
git reset .
```

**Deshacer cambios (volver al último commit):**

Para deshacer los cambios hechos y volver al archivo como estaba en el último commit:
```bash
git restore archivo.txt
git restore .
```

**Hacer commit:**

Para confirmar los cambios con un mensaje descriptivo:
```bash
# Con mensaje simple
git commit -m "feat: add user authentication"
```

Si quieres añadir un cuerpo al mensaje:
```bash
git commit
# Se abre un editor de texto:
# 1. Pulsar 'i' para escribir
# 2. Primera línea: título
# 3. Línea en blanco
# 4. Resto: cuerpo (explicación del qué y por qué)
# 5. Pulsar 'Esc' → escribir ':wq' → Enter
```

---

### 5.4 Trabajar con ramas

**Ver ramas:**

Para ver todas las ramas del repositorio:
```bash
git branch           # Ramas locales (la rama actual aparece con *)
git branch -a        # Todas las ramas (incluye remotas)
```

**Crear rama:**

Para crear una nueva rama:
```bash
git branch nombre-rama
```

Esto solo crea la rama, no cambia a ella automáticamente.

**Cambiar de rama:**

Para cambiar a otra rama:
```bash
git checkout nombre-rama

# Crear y cambiar en un solo paso:
git checkout -b nombre-rama
```

**Eliminar rama:**

Para eliminar una rama:
```bash
git branch -d nombre-rama    # Solo se borra si todos sus cambios están incluidos en otra rama
git branch -D nombre-rama    # Forzar borrado aunque tenga cambios no integrados
```

---

### 5.5 Fusionar ramas (merge)

**Merge básico:**

Para fusionar otra rama en la rama actual:
```bash
# Cambiarse a la rama que RECIBE los cambios
git checkout master

# Traer los cambios remotos (recomendado)
git pull origin master

# Fusionar otra rama
git merge nombre-rama
```

La rama en la que estás **recibe** los cambios de la rama que indicas.

Si Git puede combinar los cambios automáticamente, el merge se hace solo y solo mueve el puntero de la rama.

**Merge sin fast-forward (recomendado para master):**

Si queremos asegurar que cree un commit de merge en todo caso:
```bash
git checkout master
git pull origin master
git merge developerVG --no-ff -m "Merge branch 'developerVG' into master"
git push origin master
```

Para añadir un cuerpo al mensaje lo haríamos sin el `-m "mensaje"` y sería lo mismo que con los commits normales.

**Merge con fast-forward (recomendado para develop):**
```bash
git checkout developerVG
git pull origin master
git merge master --ff
git push origin developerVG
```

---

### 5.6 Gestionar conflictos

Si hay cambios que no se pueden combinar automáticamente, se genera un conflicto. Los archivos en conflicto aparecen modificados y marcados para resolver.

**Para resolver los conflictos:**

1. Ver qué archivos están en conflicto usando `git status`
2. Abrir los archivos y buscar las marcas `<<<<<<<`, `=======` y `>>>>>>>`
```
<<<<<<< HEAD
Contenido de la rama actual
=======
Contenido de la rama que se está fusionando
>>>>>>> nombre-rama
```

3. Elegir qué cambios conservar editando el archivo a como lo queramos dejar y eliminando las marcas
4. Guardar los archivos y añadir los cambios con `git add <archivo o '.'>`
5. Terminar el merge con `git commit -m "mensaje"` o `git commit` si Git no lo hace automáticamente

---

### 5.7 Conectar con remoto

**Añadir remoto:**

Para enlazar el repositorio local con uno remoto:
```bash
git remote add origin https://github.com/usuario/repo.git
```

El nombre "origin" es un estándar casi universal para el remoto principal. Esto crea la conexión entre tu repositorio local y el repositorio de GitHub (o cualquier otro servicio).

**Ver remotos configurados:**

Para comprobar qué remotos están configurados:
```bash
git remote -v
```

Muestra las URLs asociadas al remoto.

**Eliminar remoto:**

Para eliminar un remoto:
```bash
git remote remove origin
```

Esto borra la conexión con ese repositorio remoto.

**Cambiar URL del remoto:**
```bash
git remote set-url origin https://github.com/usuario/nuevo-repo.git
```

---

### 5.8 Sincronizar cambios (push/pull/fetch)

**Subir cambios (push):**

Para subir los cambios al remoto:
```bash
git push origin nombre-rama
git push origin master
```

**Descargar y fusionar (pull):**

Para descargar los cambios del remoto y fusionarlos con la rama en la que estamos:
```bash
git pull origin nombre-rama
git pull  # Usa la rama configurada por defecto
```

`git pull` trae los cambios de la rama remota correspondiente y los fusiona con la rama en la que estamos.

**Solo descargar sin fusionar (fetch):**

Si solo queremos descargar los cambios sin fusionarlos todavía:
```bash
git fetch origin
git fetch  # Todas las ramas
```

`git fetch` trae los commits nuevos del remoto y actualiza su información si no hay conflictos, sin modificar nuestros archivos ni nuestra rama actual.

**Diferencia:**
- `fetch`: Descarga cambios, NO los aplica
- `pull`: Descarga cambios Y los fusiona (fetch + merge)

**Opción force:**

Puedes usar la opción `--force`:
- Al usarla en `push` sobrescribes el remoto con tu versión local
- En `pull` fuerzas que lo local se reemplace por lo del remoto
- En `fetch` fuerzas que Git actualice lo que sabe del remoto aunque sobrescriba la información que había anteriormente
```bash
git push --force  # ⚠️ Usar con precaución
```

---

### 5.9 Deshacer cambios

**Deshacer último commit (mantener cambios):**

Si queremos deshacer un commit ya hecho pero aún no se ha subido al remoto:
```bash
git reset --soft HEAD~1
```

Esto mantiene los cambios en los archivos.

**Deshacer último commit (borrar cambios):**

Para borrar también los cambios en los archivos y volver al estado del commit indicado:
```bash
git reset --hard HEAD~1
```

**Deshacer varios commits:**
```bash
git reset --soft HEAD~3  # Últimos 3 commits
```

**Volver a un commit específico:**
```bash
git reset --hard abc1234  # Hash del commit
```

**Si el commit ya se ha subido al remoto:**

Puedes hacer los mismos pasos que si no se hubiera subido y luego forzar el push para sobrescribir el remoto con tu versión corregida:
```bash
git reset --hard abc1234
git push --force
```

⚠️ **Advertencia:** Esto puede afectar a otros que ya hayan descargado esos commits, por lo que se debe usar con cuidado.

---

### 5.10 Etiquetas (tags)

**Crear etiqueta simple:**

Para crear una etiqueta que marca un commit concreto sin añadir información extra:
```bash
git tag nombre-etiqueta
git tag v1.0.0
```

**Crear etiqueta anotada (recomendado):**

Para crear una etiqueta con mensaje que marca un commit y guarda información adicional:
```bash
git tag -a nombre-etiqueta -m "mensaje"
git tag -a v1.0.0 -m "Release version 1.0.0"
```

**Ver etiquetas:**

Para ver todas las etiquetas del repositorio:
```bash
git tag
```

**Subir etiquetas:**

Para subir una etiqueta al remoto:
```bash
git push origin nombre-etiqueta
git push origin v1.0.0

# Subir todas las etiquetas existentes:
git push origin --tags
```

**Borrar etiquetas:**

Para borrar una etiqueta local:
```bash
git tag -d nombre-etiqueta
git tag -d v1.0.0
```

Para borrarla del remoto:
```bash
git push origin --delete nombre-etiqueta
git push origin --delete v1.0.0
```

---

### 5.11 Comandos avanzados

**Remover archivo del seguimiento de Git:**

`git rm` elimina un archivo tanto del directorio de trabajo como del área de preparación de Git, marcando su eliminación para el próximo commit. Este comando combina la eliminación del archivo con `git add` para que los cambios se preparen y se registren en el historial.

**Uso básico:**
```bash
git rm nombre_archivo              # Elimina un archivo del proyecto y lo marca para ser eliminado en el siguiente commit
git rm archivo1 archivo2           # Elimina varios archivos a la vez
git rm -r nombre_directorio        # Elimina un directorio completo y todo su contenido
```

**Opciones útiles:**

Para solo eliminar un archivo del seguimiento de Git sin borrarlo del disco:
```bash
git rm --cached nombre_archivo     # Elimina el archivo del índice pero lo conserva en tu directorio de trabajo
```

Esto hace que Git deje de rastrear el archivo, pero sigue estando en tu disco.

Para forzar la eliminación incluso si hay cambios no guardados:
```bash
git rm --force nombre_archivo      # Fuerza la eliminación
git rm -f nombre_archivo           # Forma corta
```

Se usa con precaución para evitar la pérdida de datos.

**Borrar proyecto y/o seguimiento:**

Para borrar el proyecto entero vas en el explorador de archivos de Windows a la carpeta del proyecto y la borras.

<!-- Captura sugerida: explorador de archivos de Windows mostrando la carpeta del proyecto -->

En caso de que solo quieras quitar el seguimiento de Git lo que haces es borrar la carpeta `.git`.

**Nota:** Para mostrar la carpeta `.git` en caso de que no se vea, en Windows 11, ve al menú superior y da a ver/mostrar/elementos ocultos.

---

## 6. Gestión de Repositorio - NetBeans

### 6.1 Inicializar o clonar repositorio

**Inicializar:**

Si tienes un proyecto solo en local y quieres hacer control de versiones:

1. Abre el proyecto en NetBeans
2. Ve al menú **Team** → **Git** → **Initialize Repository**
3. Confirma la carpeta del proyecto
4. NetBeans inicializará el repositorio Git

<!-- Captura sugerida: menú Team > Git > Initialize Repository en NetBeans -->

Esto creará una carpeta oculta `.git` en tu proyecto que contendrá toda la configuración de Git para ese proyecto.

**Clonar:**

Si ya tienes un repositorio en GitHub y quieres usarlo en un ordenador nuevo:

1. Copia la URL del repositorio desde GitHub
2. En NetBeans, ve al menú **Team** → **Git** → **Clone**
3. Pega la URL del repositorio
4. Configura el directorio destino
5. Haz clic en **Next** y luego **Finish**
6. NetBeans te preguntará si quieres abrir el proyecto clonado

<!-- Captura sugerida: ventana de clonado de Git en NetBeans mostrando el campo de URL -->

---

### 6.2 Estado e historial

**Ver cambios:**

En NetBeans, los archivos modificados se muestran con diferentes colores en el explorador de proyectos:
- **Azul**: Archivos modificados
- **Verde**: Archivos nuevos
- **Gris**: Archivos ignorados (en `.gitignore`)
- **Rojo**: Archivos en conflicto

<!-- Captura sugerida: explorador de proyectos de NetBeans mostrando archivos con diferentes colores -->

Para ver los cambios en un archivo específico:
1. Haz clic derecho en el archivo
2. Selecciona **Git** → **Diff**
3. NetBeans mostrará una vista lado a lado con los cambios

<!-- Captura sugerida: ventana de diff en NetBeans mostrando cambios lado a lado -->

**Ver historial:**

Para ver el historial de commits:
1. Ve al menú **Team** → **Git** → **Show History**
2. O haz clic derecho en el proyecto → **Git** → **Show History**
3. Se abrirá una ventana mostrando todos los commits con su información

<!-- Captura sugerida: ventana de historial de Git en NetBeans -->

---

### 6.3 Añadir cambios y hacer commit

**Añadir al staging:**

En NetBeans, cuando modificas archivos, automáticamente se preparan para el commit. Sin embargo, puedes gestionar qué incluir:

1. Ve al menú **Team** → **Git** → **Commit**
2. En la ventana de commit, verás la lista de archivos modificados
3. Marca o desmarca los archivos que quieres incluir en el commit

<!-- Captura sugerida: ventana de commit en NetBeans mostrando la lista de archivos -->

**Hacer commit:**

1. Ve al menú **Team** → **Git** → **Commit**
2. En el campo **Commit Message**, escribe el mensaje del commit (siguiendo las buenas prácticas)
3. Si quieres añadir un cuerpo al mensaje, escríbelo en las líneas siguientes
4. Haz clic en **Commit**

<!-- Captura sugerida: ventana de commit en NetBeans con el campo de mensaje -->

**Nota:** Para añadir un cuerpo al mensaje, simplemente escribe varias líneas. La primera línea será el título, deja una línea en blanco, y el resto será el cuerpo.

---

### 6.4 Trabajar con ramas

**Ver ramas:**

Para ver las ramas disponibles:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Manage Branches**
2. Se abrirá una ventana mostrando todas las ramas locales y remotas

<!-- Captura sugerida: ventana de gestión de ramas en NetBeans -->

**Crear rama:**

Para crear una nueva rama:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Create Branch**
2. Escribe el nombre de la nueva rama
3. Selecciona desde qué rama o commit quieres crearla
4. Marca **Checkout Branch** si quieres cambiar a ella inmediatamente
5. Haz clic en **Create**

<!-- Captura sugerida: ventana de creación de rama en NetBeans -->

**Cambiar de rama:**

Para cambiar a otra rama:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Switch to Branch**
2. Selecciona la rama a la que quieres cambiar
3. Haz clic en **Switch**

<!-- Captura sugerida: ventana de cambio de rama en NetBeans -->

**Eliminar rama:**

Para eliminar una rama:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Manage Branches**
2. Selecciona la rama que quieres eliminar
3. Haz clic derecho → **Delete Branch**
4. Confirma la eliminación

---

### 6.5 Fusionar ramas (merge)

**Merge básico:**

Para fusionar otra rama en la rama actual:

1. Asegúrate de estar en la rama que RECIBE los cambios (usa **Switch to Branch**)
2. Ve al menú **Team** → **Git** → **Branch/Tag** → **Merge Revision**
3. Selecciona la rama que quieres fusionar
4. Haz clic en **Merge**

<!-- Captura sugerida: ventana de merge en NetBeans mostrando la selección de rama -->

NetBeans realizará el merge automáticamente si no hay conflictos.

**Nota sobre estrategias de merge:**

NetBeans por defecto intenta hacer un merge fast-forward cuando es posible. Para forzar un merge sin fast-forward (crear un commit de merge explícito), es recomendable usar la terminal:
```bash
git merge nombre-rama --no-ff -m "Merge branch 'nombre-rama'"
```

---

### 6.6 Gestionar conflictos

**Cuando hay conflicto:**

Si hay cambios que no se pueden combinar automáticamente, NetBeans mostrará los archivos en conflicto marcados en rojo en el explorador de proyectos.

**Para resolver conflictos:**

1. Haz clic derecho en el archivo en conflicto
2. Selecciona **Git** → **Resolve Conflicts**
3. NetBeans abrirá una ventana de resolución de conflictos con tres paneles:
   - Panel izquierdo: tu versión
   - Panel derecho: versión de la otra rama
   - Panel central: resultado final

<!-- Captura sugerida: ventana de resolución de conflictos en NetBeans mostrando los tres paneles -->

4. Para cada conflicto, puedes:
   - Hacer clic en **Accept** (flecha) en el panel izquierdo o derecho para aceptar esa versión
   - Editar manualmente el panel central
5. Una vez resueltos todos los conflictos, haz clic en **OK**
6. El archivo se añadirá automáticamente al staging
7. Haz commit de los cambios para completar el merge

---

### 6.7 Conectar con remoto

**Añadir remoto:**

Para conectar tu repositorio local con uno remoto:

1. Ve al menú **Team** → **Git** → **Remote** → **Push to Upstream**
2. Si es la primera vez, NetBeans te pedirá la URL del repositorio remoto
3. Pega la URL de GitHub (por ejemplo: `https://github.com/usuario/repo.git`)
4. Introduce tus credenciales de GitHub si es necesario
5. NetBeans configurará el remoto con el nombre "origin" automáticamente

<!-- Captura sugerida: ventana de configuración de remoto en NetBeans -->

**Ver remotos:**

Para ver qué remotos están configurados:
1. Ve al menú **Team** → **Git** → **Remote** → **Manage Remotes**
2. Verás una lista de los remotos configurados con sus URLs

**Eliminar remoto:**

Para eliminar un remoto:
1. Ve al menú **Team** → **Git** → **Remote** → **Manage Remotes**
2. Selecciona el remoto que quieres eliminar
3. Haz clic en **Remove**

---

### 6.8 Sincronizar cambios

**Push (subir cambios):**

Para subir los cambios al remoto:

1. Ve al menú **Team** → **Git** → **Remote** → **Push to Upstream**
2. NetBeans subirá los commits de la rama actual al remoto
3. Si es la primera vez que subes una rama nueva, NetBeans te preguntará si quieres crear la rama en el remoto

<!-- Captura sugerida: ventana de push en NetBeans -->

**Pull (descargar y fusionar):**

Para descargar los cambios del remoto y fusionarlos:

1. Ve al menú **Team** → **Git** → **Remote** → **Pull from Upstream**
2. NetBeans descargará los cambios y los fusionará automáticamente con tu rama actual
3. Si hay conflictos, NetBeans te lo indicará y deberás resolverlos

<!-- Captura sugerida: ventana de pull en NetBeans -->

**Fetch (solo descargar):**

Para solo descargar los cambios sin fusionarlos:

1. Ve al menú **Team** → **Git** → **Remote** → **Fetch from Upstream**
2. NetBeans descargará la información del remoto sin modificar tus archivos
3. Puedes ver los cambios y decidir cuándo fusionarlos manualmente

---

### 6.9 Deshacer cambios

**Deshacer cambios en un archivo (antes de commit):**

Para deshacer los cambios en un archivo y volver a su estado en el último commit:

1. Haz clic derecho en el archivo modificado
2. Selecciona **Git** → **Revert Modifications**
3. Confirma la acción

<!-- Captura sugerida: menú contextual de Git en NetBeans mostrando Revert Modifications -->

**Deshacer último commit:**

NetBeans no tiene una interfaz gráfica directa para `git reset`. Para deshacer commits, es recomendable usar la terminal:
```bash
git reset --soft HEAD~1  # Mantener cambios
git reset --hard HEAD~1  # Borrar cambios
```

---

### 6.10 Etiquetas (tags)

**Crear etiqueta:**

Para crear una etiqueta:

1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Create Tag**
2. Escribe el nombre de la etiqueta (por ejemplo: `v1.0.0`)
3. Opcionalmente, escribe un mensaje
4. Haz clic en **Create**

<!-- Captura sugerida: ventana de creación de etiqueta en NetBeans -->

**Ver etiquetas:**

Para ver todas las etiquetas:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Manage Branches**
2. Cambia a la pestaña **Tags**
3. Verás la lista de todas las etiquetas

**Subir etiquetas:**

Para subir una etiqueta específica al remoto:
1. Ve al menú **Team** → **Git** → **Remote** → **Push to Upstream**
2. En la ventana de push, expande **Advanced Options**
3. Marca la opción **Push Tags**
4. Haz clic en **Push**

<!-- Captura sugerida: ventana de push en NetBeans con la opción de Push Tags -->

Para subir todas las etiquetas, usa la terminal:
```bash
git push origin --tags
```

**Borrar etiqueta:**

Para borrar una etiqueta local:
1. Ve al menú **Team** → **Git** → **Branch/Tag** → **Manage Branches**
2. Cambia a la pestaña **Tags**
3. Haz clic derecho en la etiqueta → **Delete Tag**

Para borrar una etiqueta del remoto, usa la terminal:
```bash
git push origin --delete nombre-etiqueta
```

---

## 7. GitHub - Plataforma y Colaboración

### 7.1 ¿Qué es GitHub?

GitHub es una plataforma en la nube que aloja repositorios Git. Permite:
- Almacenar y gestionar repositorios Git
- Colaborar con otros desarrolladores
- Revisar código mediante Pull Requests
- Gestionar proyectos con Issues y Projects
- Automatizar tareas con CI/CD (GitHub Actions)
- Crear documentación con wikis
- Gestionar releases y tags

---

### 7.2 Crear un repositorio

Para crear un repositorio en GitHub:

1. Entra en https://github.com
2. Haz clic en el icono `+` en la esquina superior derecha
3. Selecciona **New repository**

<!-- Captura sugerida: menú desplegable mostrando la opción New repository -->

4. Configura el repositorio:
   - **Repository name:** Escribe el nombre del proyecto
   - **Description:** (opcional) Descripción breve del proyecto
   - **Public / Private:** Elige la visibilidad
   - **Add a README file:** (opcional) Marca si quieres crear un README inicial
   - **Add .gitignore:** (opcional) Selecciona una plantilla según el lenguaje
   - **Choose a license:** (opcional) Selecciona una licencia

<!-- Captura sugerida: formulario de creación de repositorio mostrando todas las opciones -->

5. Haz clic en **Create repository**

Una vez creado, si no añadiste README, verás instrucciones para:
- Crear un nuevo repositorio desde línea de comandos
- Subir un repositorio existente

**Opciones después de crear:**

Si el repositorio está vacío, GitHub te mostrará la URL que puedes usar para:
- **Opción A:** Clonar el repositorio vacío y empezar a trabajar
- **Opción B:** Conectar un repositorio local existente con este remoto

<!-- Captura sugerida: página de GitHub después de crear un repositorio vacío mostrando las instrucciones -->

---

### 7.3 Autenticación SSH

La autenticación SSH con claves privadas permite conectarte a GitHub sin tener que introducir tu usuario y contraseña cada vez que haces push o pull.

**Documentación oficial:**  
https://docs.github.com/es/authentication/connecting-to-github-with-ssh

**Pasos básicos:**

1. **Generar clave SSH:**

Abre Git Bash o terminal y ejecuta:
```bash
ssh-keygen -t ed25519 -C "tu.email@ejemplo.com"
```

Pulsa Enter para aceptar la ubicación por defecto. Opcionalmente, puedes poner una contraseña (passphrase).

2. **Copiar la clave pública:**
```bash
# En Windows (Git Bash):
cat ~/.ssh/id_ed25519.pub | clip

# En macOS:
pbcopy < ~/.ssh/id_ed25519.pub

# En Linux:
cat ~/.ssh/id_ed25519.pub
# Copia el contenido manualmente
```

3. **Añadir la clave a GitHub:**

- Ve a GitHub → **Settings** (tu perfil)
- En el menú lateral, haz clic en **SSH and GPG keys**
- Haz clic en **New SSH key**
- Pega la clave pública en el campo **Key**
- Dale un título descriptivo (por ejemplo: "Mi portátil")
- Haz clic en **Add SSH key**

<!-- Captura sugerida: página de GitHub SSH keys mostrando el botón New SSH key -->

4. **Probar la conexión:**
```bash
ssh -T git@github.com
```

Deberías ver un mensaje como: `Hi usuario! You've successfully authenticated...`

5. **Usar URL SSH en repositorios:**

A partir de ahora, usa URLs SSH en lugar de HTTPS:
```bash
git remote set-url origin git@github.com:usuario/repo.git
```

O al clonar:
```bash
git clone git@github.com:usuario/repo.git
```

---

### 7.4 Releases y Tags

**¿Qué son?**

- **Tag:** Marcador de un commit específico que representa un punto importante en el tiempo (por ejemplo, una versión)
- **Release:** Empaquetado de código asociado a un tag, con notas de cambios, archivos descargables y documentación

Los releases permiten a los usuarios descargar el código fuente en un punto específico y ver el historial de versiones con sus cambios.

**Crear Release desde GitHub:**

1. Ve a tu repositorio en GitHub
2. Haz clic en **Releases** en la barra lateral derecha

<!-- Captura sugerida: barra lateral de GitHub mostrando el enlace a Releases -->

3. Haz clic en **Draft a new release**

<!-- Captura sugerida: página de Releases mostrando el botón Draft a new release -->

4. Configura el release:
   - **Choose a tag:** Selecciona un tag existente o crea uno nuevo escribiendo el nombre (por ejemplo: `v1.0.0`)
   - **Target:** Selecciona la rama (normalmente `master`)
   - **Release title:** Escribe un título (por ejemplo: `v1.0.0 - Initial Release`)
   - **Description:** Escribe el changelog explicando qué cambió desde la versión anterior

Ejemplo de descripción:
```markdown
## Features
- Add user authentication
- Add dashboard page

## Bug Fixes
- Fix null pointer in login form

## Breaking Changes
- Remove support for Internet Explorer
```

   - **Attach binaries:** (opcional) Puedes subir archivos adicionales como .zip, .tar.gz, ejecutables, etc.

5. Haz clic en **Publish release**

Una vez publicado, el release aparecerá en la página de Releases y los usuarios podrán:
- Ver el changelog
- Descargar el código fuente en ese punto
- Descargar archivos adjuntos

<!-- Captura sugerida: página de Releases mostrando un release publicado con su información -->

---

### 7.5 Colaboración

#### 7.5.1 Añadir colaboradores

**Para repositorios públicos:**
- Cualquier persona puede hacer fork del repositorio y crear Pull Requests
- Solo necesitas añadir colaboradores directos si quieres darles permisos de push

**Para repositorios privados:**

Para añadir colaboradores que puedan hacer push directamente:

1. Ve a tu repositorio en GitHub
2. Haz clic en **Settings**

<!-- Captura sugerida: barra superior del repositorio mostrando el botón Settings -->

3. En el menú lateral izquierdo, haz clic en **Collaborators** o **Manage access**
4. Haz clic en **Add people**
5. Busca por nombre de usuario o email de GitHub
6. Envía la invitación

<!-- Captura sugerida: página de Collaborators mostrando el botón Add people -->

7. La persona recibirá un email y deberá aceptar la invitación

Una vez aceptada, tendrá acceso para:
- Clonar el repositorio
- Crear ramas
- Hacer push de cambios
- Abrir Pull Requests

---

#### 7.5.2 Pull Requests

**¿Qué es un Pull Request?**

Un Pull Request (PR) es una propuesta de cambios desde una rama hacia otra. Permite:
- Revisar código antes de integrarlo
- Comentar línea por línea
- Solicitar cambios
- Discutir implementaciones
- Aprobar o rechazar cambios

**Flujo de trabajo con Pull Requests:**

1. **Colaborador crea una rama y desarrolla:**
```bash
git checkout -b feature/nueva-funcionalidad
# Hacer cambios...
git add .
git commit -m "feat: add new feature"
git push origin feature/nueva-funcionalidad
```

2. **Abrir Pull Request en GitHub:**

- Ve al repositorio en GitHub
- Verás un mensaje: **"feature/nueva-funcionalidad had recent pushes"** con un botón **Compare & pull request**
- Haz clic en **Compare & pull request**

<!-- Captura sugerida: banner en GitHub mostrando el botón Compare & pull request -->

- Configura el PR:
  - **Base:** Rama que recibirá los cambios (por ejemplo: `master` o `develop`)
  - **Compare:** Rama con los cambios (por ejemplo: `feature/nueva-funcionalidad`)
  - **Title:** Título descriptivo
  - **Description:** Explicación detallada de los cambios
  - **Reviewers:** (opcional) Selecciona personas para revisar
  - **Assignees:** (opcional) Asigna responsables
  - **Labels:** (opcional) Añade etiquetas

<!-- Captura sugerida: formulario de creación de Pull Request -->

- Haz clic en **Create pull request**

3. **Revisión del código:**

Los revisores pueden:
- Ver todos los cambios en la pestaña **Files changed**
- Comentar líneas específicas
- Dejar comentarios generales
- Aprobar o solicitar cambios

<!-- Captura sugerida: pestaña Files changed mostrando la vista de diff con comentarios -->

Opciones de revisión:
- **Comment:** Dejar comentario sin aprobar
- **Approve:** Aprobar los cambios
- **Request changes:** Solicitar modificaciones

4. **Realizar cambios si es necesario:**

Si el revisor solicita cambios, el colaborador puede:
```bash
# Hacer las correcciones en la misma rama
git add .
git commit -m "fix: address review comments"
git push origin feature/nueva-funcionalidad
```

Los nuevos commits se añaden automáticamente al PR existente.

5. **Fusionar el Pull Request:**

Una vez aprobado:
- Haz clic en **Merge pull request**
- Elige el tipo de merge:
  - **Create a merge commit:** Crea un commit de merge (recomendado)
  - **Squash and merge:** Combina todos los commits en uno solo
  - **Rebase and merge:** Reaplica los commits uno a uno
- Haz clic en **Confirm merge**

<!-- Captura sugerida: botones de merge en el Pull Request -->

6. **Borrar rama (opcional):**

Después del merge, GitHub te ofrecerá la opción de borrar la rama. Es buena práctica hacerlo para mantener el repositorio limpio.

---

#### 7.5.3 Protección de ramas

**¿Por qué proteger ramas?**

La protección de ramas previene:
- Push directos a ramas importantes (como `master`)
- Borrado accidental de ramas
- Merges sin revisión

Y asegura:
- Que todos los cambios pasen por Pull Requests
- Que el código sea revisado antes de integrarse
- Que se ejecuten tests automáticos (CI/CD)

**Configurar protección de ramas:**

1. Ve a tu repositorio en GitHub
2. Haz clic en **Settings**
3. En el menú lateral izquierdo, haz clic en **Branches**

<!-- Captura sugerida: menú lateral de Settings mostrando la opción Branches -->

4. En la sección **Branch protection rules**, haz clic en **Add rule** o **Add branch protection rule**

<!-- Captura sugerida: sección Branch protection rules mostrando el botón Add rule -->

5. Configura la regla:

   - **Branch name pattern:** Escribe `master` (o el nombre de tu rama principal)
   
   Esto aplicará la regla a cualquier rama que coincida con el patrón.

   - **Opciones recomendadas:**
   
     ✅ **Require a pull request before merging**  
     Esta es la opción clave para obligar a usar Pull Requests.
     
     - ✅ **Require approvals:** Marca cuántos revisores deben aprobar (1 o más)
     
     ✅ **Require status checks to pass before merging** (si usas CI/CD)  
     Obliga a que los tests pasen antes de permitir el merge.
     
     ✅ **Require linear history** (opcional, recomendado)  
     Mantiene un historial limpio sin merges fast-forward innecesarios.
     
     🗙 **Allow force pushes** → Dejar DESMARCADO  
     Evita que se sobreescriba el historial.
     
     🗙 **Allow deletions** → Dejar DESMARCADO  
     Evita borrar la rama accidentalmente.
     
     🗙 **Do not allow bypassing the above settings** → Puedes dejarlo desmarcado si quieres que los administradores puedan saltarse las reglas cuando sea necesario, o activarlo si quieres que nadie pueda saltárselas.

<!-- Captura sugerida: opciones de protección de rama mostrando las casillas marcadas -->

6. Haz clic en **Create** o **Save changes**

**Flujo de trabajo con ramas protegidas:**

Una vez configurada la protección:

1. Los colaboradores NO pueden hacer push directo a `master`
2. Deben:
   - Crear una rama nueva
   - Desarrollar en esa rama
   - Hacer push de la rama
   - Abrir un Pull Request
   - Esperar aprobación de un revisor
3. Solo después de la aprobación, un administrador puede hacer merge a `master`

**Ejemplo de flujo:**
```bash
# Colaborador crea rama
git checkout -b feature/login
# Desarrolla...
git add .
git commit -m "feat: add login form"
git push origin feature/login
# Abre Pull Request en GitHub
# Espera aprobación
# Administrador hace merge desde GitHub
```

Esto asegura que todo el código pase por revisión antes de llegar a producción.

---

### 7.6 Borrar un repositorio

⚠️ **Advertencia:** Esta acción es irreversible. Una vez borrado, no hay vuelta atrás.

**Pasos para borrar un repositorio:**

1. Ve a tu repositorio en GitHub
2. Haz clic en **Settings**

<!-- Captura sugerida: barra superior del repositorio mostrando el botón Settings -->

3. Baja hasta el final de la página, a la sección **Danger Zone**
4. Haz clic en **Delete this repository**

<!-- Captura sugerida: sección Danger Zone mostrando el botón Delete this repository -->

5. Aparecerá una ventana de confirmación. Lee la advertencia.

<!-- Captura sugerida: primera ventana de confirmación de borrado -->

6. En la siguiente ventana te pedirá que escribas el nombre completo del repositorio: `usuario/nombre-repositorio`

<!-- Captura sugerida: ventana pidiendo escribir el nombre del repositorio -->

7. Haz clic en **I understand the consequences, delete this repository**

8. Te pedirá autenticación:
   - Si tienes habilitada la autenticación de dos factores (2FA), introduce el código de tu app de autenticación (por ejemplo: Google Authenticator, Authy, etc.)
   - Si no tienes 2FA, introduce tu contraseña de GitHub

<!-- Captura sugerida: ventana de autenticación solicitando código 2FA o contraseña -->

9. Una vez verificado, el repositorio se borrará permanentemente

10. GitHub te redirigirá a la página principal con un mensaje de confirmación

<!-- Captura sugerida: mensaje de confirmación de borrado en la página principal -->

**Nota:** Si tienes clones locales del repositorio, esos NO se borran automáticamente. Seguirán en tu ordenador hasta que los borres manualmente.

---

## 8. Flujos de Trabajo Completos

### 8.1 Proyecto nuevo (local → GitHub)

**Escenario:** Tienes un proyecto en local y quieres subirlo a GitHub.

**Pasos:**

1. **Inicializar Git en el proyecto:**

Abre la terminal en la carpeta del proyecto y ejecuta:
```bash
git init
```

2. **Crear archivo .gitignore (si es necesario):**

Crea un archivo `.gitignore` en la raíz del proyecto y añade los archivos/carpetas que no quieres rastrear.

3. **Hacer el primer commit:**
```bash
git add .
git commit -m "feat: initial commit"
```

4. **Crear repositorio en GitHub:**

Ve a GitHub y crea un nuevo repositorio (ver [sección 7.2](#72-crear-un-repositorio)). NO marques "Add a README file" ni ".gitignore" ya que ya tienes el proyecto localmente.

5. **Conectar con el remoto:**

Copia la URL del repositorio de GitHub y ejecuta:
```bash
git remote add origin https://github.com/usuario/nombre-repo.git
```

O si usas SSH:
```bash
git remote add origin git@github.com:usuario/nombre-repo.git
```

6. **Subir el código:**
```bash
git push -u origin master
```

El flag `-u` establece `origin master` como la rama upstream, para que en futuros push solo tengas que hacer `git push`.

7. **Verificar en GitHub:**

Actualiza la página de tu repositorio en GitHub y deberías ver todos tus archivos.

---

### 8.2 Proyecto existente (GitHub → local)

**Escenario:** Hay un proyecto en GitHub que quieres descargar para trabajar en él.

**Pasos:**

1. **Copiar URL del repositorio:**

Ve al repositorio en GitHub y copia la URL (HTTPS o SSH).

2. **Clonar el repositorio:**

Abre la terminal en la carpeta donde quieres guardar el proyecto y ejecuta:
```bash
git clone https://github.com/usuario/nombre-repo.git
```

O con SSH:
```bash
git clone git@github.com:usuario/nombre-repo.git
```

Opcionalmente, puedes especificar un nombre de carpeta:
```bash
git clone https://github.com/usuario/nombre-repo.git mi-proyecto
```

3. **Entrar en la carpeta del proyecto:**
```bash
cd nombre-repo
```

4. **Verificar el estado:**
```bash
git status
```

5. **Listo para trabajar:**

Ahora puedes empezar a trabajar en el proyecto. Si quieres trabajar en una rama de desarrollo:
```bash
git checkout -b develop
```

O si la rama `develop` ya existe en el remoto:
```bash
git checkout develop
```

---

### 8.3 Desarrollo con ramas

**Escenario:** Quieres desarrollar una nueva funcionalidad sin afectar la rama principal.

**Flujo recomendado:**

**Estructura de ramas:**
- `master`: Código en producción (estable)
- `develop` o `developer`: Desarrollo activo
- `feature/nombre`: Ramas para funcionalidades específicas

**Pasos:**

1. **Crear rama de desarrollo (si no existe):**
```bash
git checkout -b develop
git push origin develop
```

2. **Para cada nueva funcionalidad:**

Asegúrate de estar en `develop` actualizada:
```bash
git checkout develop
git pull origin develop
```

Crea una rama para la funcionalidad:
```bash
git checkout -b feature/login
```

3. **Desarrollar la funcionalidad:**
```bash
# Hacer cambios en el código...
git add .
git commit -m "feat(auth): add login form"

# Más cambios...
git add .
git commit -m "feat(auth): add validation"
```

4. **Subir la rama:**
```bash
git push origin feature/login
```

5. **Abrir Pull Request en GitHub:**

Ve a GitHub y abre un Pull Request de `feature/login` → `develop` (ver [sección 7.5.2](#752-pull-requests)).

6. **Después de la aprobación, fusionar en develop:**
```bash
git checkout develop
git pull origin develop  # Asegurarse de tener los últimos cambios
git merge feature/login --no-ff -m "Merge feature/login into develop"
git push origin develop
```

7. **Borrar la rama feature (opcional):**
```bash
git branch -d feature/login
git push origin --delete feature/login
```

8. **Cuando esté listo para producción:**

Fusionar `develop` en `master`:
```bash
# Fusionar develop → master (con commit de merge)
git checkout master
git pull origin master
git merge develop --no-ff -m "Release v1.1.0"
git push origin master

# Fusionar master → develop (fast-forward para mantener sincronización)
git checkout develop
git merge master --ff
git push origin develop
```

De esta forma, ambas ramas quedan en el mismo punto, pero `master` solo tiene un commit de merge que representa la release, mientras que `develop` mantiene todo el historial de desarrollo.

---

### 8.4 Preparar una release

**Escenario:** Has terminado de desarrollar una versión y quieres publicarla oficialmente.

**Pasos:**

1. **Asegurarse de que master está actualizado:**
```bash
git checkout master
git pull origin master
```

2. **Crear tag anotado:**
```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

3. **Subir el tag:**
```bash
git push origin v1.0.0
```

O subir todos los tags:
```bash
git push origin --tags
```

4. **Crear Release en GitHub:**

Ve a GitHub y crea un release asociado al tag (ver [sección 7.4](#74-releases-y-tags)):

- Ve a **Releases** → **Draft a new release**
- Selecciona el tag `v1.0.0`
- Escribe el título: `v1.0.0 - Initial Release`
- Escribe el changelog en la descripción
- Publica el release

5. **Actualizar develop con los cambios de master:**
```bash
git checkout develop
git merge master --ff
git push origin develop
```

Esto asegura que `develop` esté sincronizada con `master` después de la release.

6. **Continuar desarrollando:**

Ahora puedes continuar desarrollando nuevas funcionalidades en `develop` o en ramas `feature/`, y cuando estés listo para la siguiente versión, repites el proceso.

---

> **Enrique Nieto Lorenzo**  
> Curso: 2025/2026  
> 2º Curso CFGS Desarrollo de Aplicaciones Web  
> Despliegue de Aplicaciones Web