# Estándares de Contenedores: OCI vs. OSI

## Introducción al Estándar OCI

Para lo que vamos a trabajar, es fundamental entender el **estándar OCI (Open Container Initiative)**. Aquí no hay que confundirlo con **OSI (Open Systems Interconnection)**, que es un modelo de referencia para la interconexión de sistemas en redes. La **OCI**, en cambio, se enfoca en la estandarización de contenedores, definiendo cómo deben funcionar los contenedores a nivel de ejecución. Esto permite que cualquier engine que siga las especificaciones de OCI sea compatible entre sí. Así, sin importar dónde se ejecute el contenedor, siempre funcionará de la misma manera.

### Diferencia entre OCI y OSI

- **OSI**: Modelo de capas para comunicaciones en red.
- **OCI**: Estandarización de contenedores y su ejecución, asegurando compatibilidad entre diferentes implementaciones y motores de contenedores.

### Open Container Initiative (OCI)

La **Open Container Initiative (OCI)** es una organización que se encarga de definir estándares abiertos para contenedores. Estos estándares son importantes porque permiten la interoperabilidad entre diferentes sistemas y tecnologías de contenedores, asegurando que no haya problemas al mover contenedores entre diferentes plataformas o servicios.

El **estándar OCI** establece cómo debe funcionar un contenedor, desde su creación hasta su ejecución. De esta manera, asegura que los engines (programas que crean y ejecutan contenedores) sean retrocompatibles y funcionen de la misma forma, sin importar en qué entorno se ejecuten. Es decir, si creas un contenedor en **Docker**, este será completamente funcional en **Podman** o **CRI-O**, gracias a que todos respetan las especificaciones de OCI.

## Docker y Moby Project

### Docker y su Relación con Moby

Un punto importante que hay que tener en cuenta es que **Docker**, como empresa, creó un proyecto de código abierto llamado **Moby Project**. **Moby** es el software base que permite la contenedorización de aplicaciones, pero **Docker** es la empresa que le da soporte. Es importante mencionar que **Moby** fue donado a la Open Container Initiative, ya que su importancia en la contenedorización sentó un precedente para los estándares actuales.

### Moby y containerd

Aunque mucha gente relaciona directamente a Docker con la creación de contenedores, en realidad el motor que ejecuta los contenedores es **containerd**. Este es el verdadero motor que realiza las funciones más importantes como la ejecución, administración de contenedores y gestión de imágenes.

Es crucial entender que **Docker** es solo una interfaz o frontend que simplifica el uso de **containerd**, haciendo más accesible la creación y manejo de contenedores. Docker fue pionero en este campo y facilitó la adopción masiva de la tecnología de contenedores, pero es containerd quien hace el trabajo pesado.

_Comando para ejecutar un contenedor con Docker:_  
`docker run [imagen]`

_Comando equivalente en containerd:_  
`ctr run [imagen]`

## Podman: La Alternativa a Docker

**Podman** es un motor de contenedores creado por **Red Hat** como respuesta a una limitación importante de Docker: la necesidad de ejecutarlo como root. Esta vulnerabilidad en Docker permite una posible escalada de privilegios, lo cual representa un riesgo de seguridad significativo. En resumen, si alguien logra vulnerar un contenedor ejecutado con Docker, puede obtener permisos de root en el sistema host, lo que comprometería toda la máquina.

### Solución de Podman

Podman resuelve este problema permitiendo la ejecución de contenedores sin necesidad de permisos de root. En lugar de correr como root, los contenedores creados por **Podman** se ejecutan con los privilegios del usuario que los creó. Esto significa que si un atacante vulnera un contenedor, solo obtendrá los permisos del usuario que creó el contenedor (por ejemplo, si el usuario es `sebas`, solo tendrá sus permisos).

Este enfoque añade una capa de seguridad adicional, aunque no es una solución definitiva. Es una barrera más que reduce las posibilidades de comprometer todo el sistema.

_Comando para ejecutar un contenedor con Podman:_  
`podman run [imagen]`

## CRI-O: Contenedores para Kubernetes

**CRI-O** es otro motor de contenedores, pero con un enfoque exclusivo para **Kubernetes**. Al igual que Docker, CRI-O requiere permisos de root para ejecutarse, lo que introduce las mismas vulnerabilidades de escalado de privilegios que mencionamos antes. Sin embargo, al igual que Docker y Podman, CRI-O cumple con los estándares de OCI, lo que asegura que los contenedores creados por cualquiera de estos motores sean compatibles entre ellos.

_CRIO Comando:_  
`crictl runp [imagen]`

Esto significa que los contenedores creados en **Docker**, **Podman** o **CRI-O** son intercambiables y pueden ejecutarse en cualquier plataforma que siga el estándar OCI.

---

## Detalles Técnicos sobre Contenedores

### Cómo Funcionan los Contenedores

A nivel técnico, los contenedores son instancias de procesos que utilizan las capacidades del **kernel de Linux**, como los **cgroups** (para limitar el uso de recursos) y los **namespaces** (para aislar los entornos de los contenedores). Estas tecnologías permiten que múltiples contenedores se ejecuten de manera aislada en un mismo sistema, compartiendo el kernel pero sin interferir entre sí.

Los contenedores no son máquinas virtuales; no necesitan un sistema operativo completo, solo las librerías y binarios esenciales para ejecutar una aplicación. Esto hace que los contenedores sean mucho más ligeros y eficientes en términos de recursos comparados con las máquinas virtuales tradicionales.

### Seguridad en Contenedores

Además de las medidas como el uso de **Podman** para evitar la ejecución de contenedores como root, existen otras prácticas de seguridad importantes en el manejo de contenedores, como:
- Mantener las imágenes actualizadas para evitar vulnerabilidades conocidas.
- Limitar las capacidades de los contenedores mediante herramientas como **seccomp**.
- Aplicar políticas de seguridad de nivel empresarial utilizando perfiles de **AppArmor** o **SELinux**.

### Estandarización OCI y Retrocompatibilidad

Como mencionamos antes, gracias a la estandarización de OCI, todos los motores de contenedores, desde **Docker** hasta **CRI-O**, son compatibles entre sí. Esto asegura que los comandos básicos como `run`, `pull`, `start` funcionen de manera uniforme en cualquier plataforma. Esta retrocompatibilidad es esencial para evitar el bloqueo de proveedores y facilitar la integración de contenedores en infraestructuras multi-cloud o híbridas.

