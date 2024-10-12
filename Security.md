# Características de Seguridad del Kernel de Linux

## Introducción al Kernel de Linux

El **kernel** es el núcleo central del sistema operativo, encargado de gestionar y coordinar el acceso a los recursos del hardware, como el procesador (CPU), la memoria RAM, y los dispositivos de entrada/salida. A diferencia de un sistema operativo completo, el kernel no incluye interfaces gráficas o aplicaciones de usuario, pero sí controla el acceso a ellas y actúa como intermediario entre el hardware y el software.

### Tipos de Kernels

Existen dos grandes categorías de kernels:

1. **Microkernels**: Estos kernels son extremadamente reducidos, gestionando solo las funciones más esenciales como el manejo del procesador, la memoria y los dispositivos de entrada/salida. Ejemplos:
   - **Minix**: Famoso por ser el microkernel que inspiró a Linus Torvalds para crear Linux.
   - **Redox OS**: Un sistema operativo moderno basado en microkernel.

   Los microkernels delegan el resto de las funciones del sistema (como el manejo de archivos y drivers) a programas en **espacio de usuario** que se comunican con el kernel mediante llamadas a procesos intermedios (servidores).

2. **Macrokernels o Kernels Monolíticos**: A diferencia de los microkernels, los macrokernels incluyen todas las funciones del sistema en un único bloque, integrando tanto el manejo del hardware como los controladores de dispositivos. Estos kernels tienden a ser más grandes pero también más eficientes en términos de velocidad y acceso directo al hardware. Ejemplos:
   - **Linux**
   - **FreeBSD**
   - **Unix**

Los **macrokernels** cargan los controladores de dispositivos y otras funcionalidades directamente en el kernel, lo que permite un acceso más rápido pero también genera kernels de mayor tamaño. Por otro lado, los **microkernels** pueden ser más seguros y estables, ya que un fallo en un módulo del sistema no afecta al núcleo del sistema.

### Modularidad del Kernel de Linux

El kernel de Linux es un **kernel modular**, lo que significa que no necesita tener todos sus controladores y funciones incorporados directamente. En su lugar, puede cargar y descargar **módulos** según sea necesario. Esto permite que el kernel mantenga un tamaño reducido en la memoria y se vuelva más flexible.

Los módulos son pequeñas piezas de software que pueden ser cargadas en tiempo de ejecución, y permiten añadir funcionalidades como nuevos controladores de dispositivos sin necesidad de recompilar todo el kernel. Puedes ver los módulos cargados en tu sistema con el comando:

_Comando:_  
`lsmod`

Y cargar uno manualmente usando el comando:

_Comando:_  
`modprobe`

---

## Sistema de Permisos en Linux

### El Control de Acceso Discrecional (DAC)

El **Control de Acceso Discrecional (DAC)** es el sistema de permisos más básico y utilizado en Linux y en sistemas Unix. Este sistema permite que los **propietarios** de archivos y directorios definan los permisos de acceso para otros usuarios. Los permisos básicos son:

1. **Lectura (r)**: Permite ver el contenido del archivo o la lista de archivos dentro de un directorio.
2. **Escritura (w)**: Permite modificar el contenido del archivo o crear/eliminar archivos dentro de un directorio.
3. **Ejecución (x)**: Permite ejecutar un archivo (si es un programa o script) o acceder al contenido de un directorio.

Estos permisos se aplican a tres tipos de usuarios:
- **Propietario (user)**: El usuario que creó el archivo o directorio.
- **Grupo (group)**: Un grupo de usuarios que tienen permisos definidos.
- **Otros (others)**: Todos los demás usuarios del sistema.

### Ejemplos de Permisos y el Comando `ls -l`

El comando `ls -l` en Linux muestra los permisos de los archivos en formato de letras o números. Por ejemplo:

_Comando:_  
`ls -l`

Salida:  
`drwxr-xr-x 2 anderson users 4096 Oct 10 12:34 documentos`  
`-rw-r--r-- 1 anderson users 2048 Oct 10 12:30 archivo.txt`

Explicación:
- **`drwxr-xr-x`**: El primer carácter indica si es un directorio (`d`) o archivo regular (`-`).
  - **`rwx`**: El propietario tiene permisos de lectura, escritura y ejecución.
  - **`r-x`**: El grupo tiene permisos de lectura y ejecución.
  - **`r-x`**: Otros usuarios solo tienen permisos de lectura y ejecución.

#### Permisos Numéricos

Los permisos también pueden representarse numéricamente:
- **Lectura (r) = 4**
- **Escritura (w) = 2**
- **Ejecución (x) = 1**

Entonces, los permisos `rwxr-xr-x` equivalen a **755**. Puedes cambiar los permisos con el comando `chmod`:

_Comando:_  
`chmod 755 archivo.txt`

### Directorios como Punteros

Es importante destacar que los **directorios en Linux no contienen directamente los archivos**. En realidad, son punteros o referencias que indican dónde están los archivos en el disco. Cuando el kernel accede a un directorio, busca en este índice los archivos correspondientes. Los directorios actúan como un mapa que señala la ubicación de los archivos.

---

## Sistemas de Seguridad Avanzados en Linux

### SELinux (Security Enhanced Linux)

**SELinux** es un sistema de seguridad avanzado desarrollado originalmente por la **NSA** y mantenido por **Red Hat**. SELinux es un tipo de sistema **MAC (Mandatory Access Control)** que aplica políticas muy estrictas sobre qué usuarios y aplicaciones pueden acceder a determinados archivos, dispositivos o realizar ciertas acciones.

#### Funcionamiento de SELinux

SELinux utiliza un sistema de **etiquetas (labels)**. Cada archivo, proceso o recurso en el sistema tiene una etiqueta que indica qué permisos tiene. Cuando una aplicación intenta acceder a un recurso, SELinux verifica si las reglas lo permiten.

SELinux funciona en dos modos:
- **Enforced**: Aplica las políticas de forma estricta, bloqueando cualquier acción no permitida.
- **Permissive**: Solo monitorea las acciones y registra lo que se habría bloqueado, sin aplicar las políticas.

Las políticas de SELinux se almacenan en `/usr/share/selinux/targeted`.

SELinux es utilizado en distribuciones como **Fedora**, **Red Hat Enterprise Linux**, **CentOS** y **Android**, y es considerado uno de los sistemas de seguridad más robustos disponibles.

### AppArmor

**AppArmor** es otro sistema **MAC**, desarrollado por **Canonical** y utilizado en distribuciones como **Ubuntu**, **Debian**, **SUSE**, y **Arch Linux**. A diferencia de SELinux, AppArmor se basa en la ruta de los archivos para aplicar políticas de seguridad, lo que facilita su administración pero ofrece menos granularidad en el control.

Las reglas de AppArmor se almacenan en `/etc/apparmor.d`.

### Tomoyo y Otros Sistemas MAC

**Tomoyo** es un sistema **MAC** más ligero que SELinux, desarrollado por **NTT Data** en Japón. Aunque ofrece control sobre los recursos del sistema, no tiene el mismo nivel de desarrollo que SELinux. Es ideal para entornos embebidos donde la seguridad sigue siendo importante pero el rendimiento es crucial.

Otros sistemas MAC incluyen:
- **YAMA**: Enfocado en limitar las llamadas entre procesos (ptrace) para mejorar la seguridad entre procesos.

---

# Contenedores en Linux

Linux es conocido por su capacidad de soportar contenedores, que permiten aislar aplicaciones y sus dependencias en un entorno controlado, utilizando los recursos del sistema de manera eficiente.

## ¿Qué son los Contenedores?
Los contenedores son entornos ligeros y aislados que permiten ejecutar aplicaciones y sus dependencias sin interferir con otros procesos o el sistema host. Se apoyan en las capacidades del kernel, como **cgroups** (control groups) y **namespaces**, para gestionar los recursos y garantizar el aislamiento.

### Tipos de Aislamiento en Contenedores
1. **Aislamiento de Almacenamiento**: Permite que cada contenedor tenga su propio sistema de archivos independiente.
2. **Aislamiento de Red**: Cada contenedor tiene su propia interfaz de red virtual, separada de la del host.
3. **Aislamiento de Memoria RAM**: Los contenedores no pueden acceder a la memoria de otros contenedores o del sistema host.

## Diferencia con la Virtualización
A diferencia de las máquinas virtuales, los contenedores no requieren un sistema operativo completo por cada instancia. En su lugar, comparten el mismo kernel del sistema operativo host. Esto hace que los contenedores sean mucho más ligeros y eficientes.

## Principales Engines de Contenedores en Linux

### Docker
**Docker** es uno de los motores de contenedores más populares. Permite crear, gestionar y ejecutar contenedores de forma sencilla. A través de comandos como `docker run` y `docker pull`, puedes ejecutar aplicaciones con todas sus dependencias en contenedores aislados.

### Podman
**Podman** es una alternativa a Docker, creada por Red Hat. A diferencia de Docker, Podman no requiere permisos de root, lo que mejora la seguridad. Los contenedores se ejecutan con los permisos del usuario que los crea.

### CRI-O
**CRI-O** es un motor diseñado para ser usado en **Kubernetes**. Funciona como un backend para Kubernetes, gestionando los contenedores de manera eficiente.

## Comandos Básicos para Contenedores
- **Iniciar un contenedor**:
  
  Comando:  
  `docker run [imagen]:[tag]`
  
  O usando **Podman**:

  Comando:  
  `podman run [imagen]:[tag]`

- **Listar contenedores en ejecución**:

  Comando:  
  `docker ps`

- **Listar todos los contenedores (incluyendo los detenidos)**:

  Comando:  
  `docker ps -a`

- **Inspeccionar un contenedor**:

  Comando:  
  `docker inspect [ID_contenedor]`

- **Copiar archivos desde un contenedor**:

  Comando:  
  `docker cp [ID_contenedor]:[ruta_origen] [ruta_destino]`

- **Guardar el estado de un contenedor**:

  Comando:  
  `docker commit [ID_contenedor]`

---

## Medidas de Seguridad en Contenedores
La seguridad en los contenedores es crucial, ya que cualquier vulnerabilidad en el contenedor podría comprometer el sistema host. Algunas medidas de seguridad recomendadas incluyen:

- **Uso de perfiles de AppArmor y SELinux**: Es posible aplicar políticas de seguridad a los contenedores usando **AppArmor** o **SELinux**, restringiendo las acciones que pueden realizar dentro del sistema.
- **Deshabilitar la ejecución de privilegios adicionales**: Evitar el uso de permisos elevados dentro de los contenedores.
- **Actualizaciones periódicas de las imágenes de contenedores**: Mantener las imágenes actualizadas para evitar vulnerabilidades.
