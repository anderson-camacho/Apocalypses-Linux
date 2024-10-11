# Sistemas Operativos Linux - Informática a prueba del apocalipsis

## Introducción a Linux

Un sistema operativo **Linux** es un sistema de código abierto (open source). Esto significa que su código fuente está disponible para que cualquiera lo use, modifique y lo mejore. Al ser open source, muchas distribuciones son gratuitas (excepto algunas, como Red Hat Enterprise Linux). Además, la mayoría de estas distribuciones usan componentes del proyecto GNU.

### Historia de Linux

La historia de Linux comienza en 1983 cuando **Richard Stallman** (sí, el mismo de GNU) inició el proyecto para crear una versión de un sistema operativo similar a Unix, pero **open source**. Este proyecto se llamó GNU y su objetivo era que el software fuera libre para todos.

Aunque GNU avanzó, le faltaba el componente más importante: el kernel (el núcleo del sistema). Y aquí entra nuestro amigo **Linus Torvalds**. En 1991, Linus, durante sus estudios en la Universidad de Helsinki, desarrolló el kernel **Linux**. Al combinar GNU con el kernel Linux, nació lo que hoy conocemos como un **sistema operativo Linux**.

---

## Arquitectura de un sistema Linux

Un sistema Linux sigue una estructura de **directorios** muy organizada, basada en el antiguo esquema de Unix. Aquí algunos directorios clave que te interesa conocer:

| Directorio | Descripción |
|------------|-------------|
| **/boot**  | Almacena el kernel y el cargador de arranque. |
| **/bin**   | Contiene los programas esenciales del sistema. |
| **/home**  | Carpeta personal de cada usuario (ejemplo: /home/anderson). |
| **/lib**   | Guarda las bibliotecas necesarias para los programas. |
| **/proc**  | Información sobre los procesos en ejecución. |
| **/usr**   | Aquí van los programas instalados por los usuarios. |

---

## Kernel Linux

El **kernel** es el corazón del sistema operativo, el que se encarga de gestionar los recursos (memoria, CPU, etc.). Linux usa un **macrokernel**, lo que significa que se ocupa de un montón de tareas, desde la gestión de archivos hasta la seguridad del sistema. Para evitar que el kernel sea pesado, se usan **módulos** que se cargan solo cuando son necesarios.

Los módulos del kernel se guardan en `/lib/modules/[versión_del_kernel]`.

---

## Librería C estándar

Esta librería (C standard library) permite que el sistema y los programas interactúen fácilmente. Básicamente, es un conjunto de funciones ya listas para usar, que hacen que el sistema sea más fácil de programar.

En Linux, hay varias versiones de la librería C: **MUSL** (ligera y rápida), **GLibC** (la más completa) y **uClibc** (optimizada para sistemas embebidos).

---

## Gestores de paquetes

En Linux, para instalar o actualizar programas, se usan **gestores de paquetes**. Dependiendo de la distribución, tendrás uno u otro. Aquí algunos ejemplos:

| Distribución | Gestor de paquetes |
|--------------|--------------------|
| Debian, Ubuntu, Mint | apt |
| Fedora, Rocky Linux  | dnf |
| Arch, Manjaro        | pacman |
| Alpine Linux         | apk |

Con estos gestores, puedes instalar programas fácilmente desde la terminal. Por ejemplo, en Ubuntu, para instalar algo usarías:  
```bash
sudo apt install nombre_del_programa
```
## Uso básico de la terminal

Linux se puede usar tanto desde la **terminal** como con un entorno gráfico (escritorio). Aquí algunos comandos básicos para empezar:

- **Ver el contenido de un archivo**:
  ```bash
  cat archivo.txt
  ```
