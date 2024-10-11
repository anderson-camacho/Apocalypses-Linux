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

- Listar archivos en un directorio:
  ```bash
  ls
  
- Buscar una palabra en un archivo:
  ```bash
  grep "palabra" archivo.txt
- Mover un archivo:
  ```bash
  mv archivo_origen archivo_destino
- Copiar un archivo:
  ```bash
  cp archivo_origen archivo_destino

##Seguridad en Linux
Hay dos tipos de distribuciones en cuanto a actualizaciones:

- Fixed Release: Distribuciones como Red Hat o Ubuntu LTS sacan nuevas versiones cada cierto tiempo. Son más estables, pero no siempre tienen lo último.
- Rolling Release: Distribuciones como Arch Linux siempre están actualizadas con lo último, pero pueden ser más inestables si algún paquete tiene un error.

  ## Sysinit y el proceso de inicialización en Linux

Cuando un sistema Linux arranca, uno de los primeros archivos que se ejecutan es `/sbin/init`. Este archivo es clave en el proceso de inicialización del sistema, y su principal función es cargar los **servicios** necesarios para que el sistema operativo funcione correctamente. Ahora, desglosaremos qué significa exactamente este proceso y sus variantes.

### ¿Qué es `/sbin/init`?

El directorio `/sbin` es una abreviatura de **"System Binary"**, lo que significa que contiene archivos binarios críticos para el sistema, generalmente utilizados por el administrador (root). En este caso, el archivo `init` que reside aquí es el encargado de iniciar todos los procesos necesarios para que el sistema esté operativo. Este proceso inicializa los **demonios** (programas que se ejecutan en segundo plano) y establece el ambiente donde los usuarios y aplicaciones funcionarán.

### SysVinit: El clásico

**SysVinit** es uno de los sistemas de inicialización más antiguos que se usaron en Unix y Linux. Aunque ha sido reemplazado en muchas distribuciones por sistemas más modernos, todavía sirve de base para varias distribuciones actuales. SysVinit sigue un enfoque simple: usa **scripts de shell** para ejecutar los servicios de forma secuencial (uno después del otro).

A pesar de su antigüedad, algunas distribuciones como **Devuan** y **Slackware** aún lo utilizan, ya que mantiene un enfoque básico y confiable. Muchos de los sistemas de inicialización modernos, como OpenRC o SystemD, descienden de este clásico sistema.

### OpenRC: Un SysVinit mejorado

**OpenRC** es una versión mejorada de SysVinit que agrega capacidades más avanzadas, como **supervisión de procesos** y **paralelismo**. La supervisión permite que el sistema controle si un proceso falla y lo reinicie automáticamente. Además, el paralelismo hace posible que varios servicios se inicien al mismo tiempo, lo que mejora la velocidad de arranque del sistema.

OpenRC es usado en distribuciones como **Nitrux**, **Funtoo**, **Gentoo** y **Alpine Linux**. Su principal ventaja es que es más flexible y eficiente que SysVinit, pero sin romper con la simplicidad de su predecesor.

### SystemD: El gigante de la inicialización

**SystemD** es mucho más que un sistema de inicialización; es un conjunto completo de **demonios**, **bibliotecas** y **herramientas** que administran no solo el arranque del sistema, sino también la comunicación entre procesos, la gestión de dispositivos y más. A diferencia de SysVinit y OpenRC, SystemD no sigue la filosofía **KISS (Keep It Simple, Stupid)**, que aboga por hacer una cosa y hacerla bien.

En lugar de eso, SystemD incluye una gran cantidad de funcionalidades adicionales, lo que ha generado cierta controversia en la comunidad Linux. Aun así, su poder y flexibilidad lo han convertido en el sistema de inicialización predeterminado para muchas distribuciones populares como **Debian**, **Ubuntu**, **Fedora**, **RedHat**, y **Arch Linux**.

#### ¿Qué son los sockets en SystemD?

Una de las características que distingue a SystemD es su manejo de **sockets**. Un socket es un punto de comunicación que permite a un programa enviar o recibir datos. En Linux, un **Unix socket** es un archivo especial que permite a los programas comunicarse directamente con otros procesos o incluso con el kernel, sin necesidad de estar en ejecución constante.

SystemD utiliza los sockets para iniciar servicios bajo demanda. Por ejemplo, cuando un programa intenta conectarse a un servicio que no está corriendo, SystemD puede detectar esta conexión, iniciar el servicio y luego transferirle la conexión. Esto ahorra recursos del sistema, ya que los servicios no tienen que estar corriendo todo el tiempo.

---

En resumen, Linux cuenta con varios sistemas de inicialización, cada uno con sus ventajas y desventajas. Desde el clásico SysVinit, hasta el más moderno y potente SystemD, cada uno cumple con la tarea esencial de preparar el sistema para su funcionamiento y gestionar los procesos que lo componen.


## Documentación

Tener buena documentación es clave en Linux. Aquí te dejo algunas fuentes de información útiles:

- **Red Hat** tiene su propia base de conocimiento (requiere cuenta de pago):  
  [Red Hat Knowledge Base](https://access.redhat.com/search/knowledgebase)

- **Arch Linux** tiene una de las mejores wikis del mundo Linux:  
  [Arch Linux Wiki](https://wiki.archlinux.org/)

- Un video útil de YouTube sobre Linux que te puede interesar:  
  [Linux Video](https://www.youtube.com/watch?v=7Myy3kTnXgo)

- Un artículo detallado sobre Linux que puedes consultar:  
  [Informatica Apocalipsis - Linux](https://shyanjmc.com/informatica-apocalipsis/9-linux.html)

