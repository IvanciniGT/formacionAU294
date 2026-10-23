# Creación de imágenes de contenedor

Cuando creamos una imagen de contenedor se usan unos archivos que se llaman Dockerfile.

Esos archivos incluyen detalles de muy bajo nivel.
Lo que hacemos es ejecutar comandos linux.

Ansible nos permite automatizar la creación de imágenes de contenedor sin tener que escribir todos los comandos de bajo nivel en un Dockerfile (lenbguaje imeprativo) . Lo que ofrce son unos ficheros llamados execution-environment.yaml.
En ese archivo solamente DECLARAMOS (lenguaje declarativo) las necesidades de nuestro playbook (que ansible, que roles, que colecciones, que paquetería, etc.)

ansible-builder genera un archivo DOCKERFILE desde el execution-environment.yaml.
Y después desde ese DOCKERFILE se puede construir la imagen de contenedor con herramientas como docker build.

Cuando creamos una imagen de contenedor, lo habitual es partir de otra imagen. Lo que se llaman imágenes base.

    registry.fedoraproject.org/fedora:43                        ESTO ES UNA IMAGEN BASE

Pero no nos equivoquemos. Eso no lleva dentro un fedora.

Estas imágenes (Son archivos comprimidos... tar) lo que contienen es:
- Una estructura de carpetas compatible con POSIX:
  - bin
  - lib
  - etc
  - usr
  - var
  - home
Y dentro de esas carpetas, preinstalados un montón de programas:
  - ls
  - cp
  - cat
  - more
  - mkdir
  - sh
Los comandos habtuales definidos en POSIX.

Las diferencias entre las distintas imágenes base son en los extras que llevan además de eso.
- Ubuntu: además de lo anterior viene: bash, apt, dpkg
- Fedora: además de lo anterior viene: bash, dnf, rpm, yum

Lo que hacemos es descomprimir eso, sobre esas carpetas instalar programas adicionales y volver a comprimir.
Ese es el proceso de generar una imágen de contenedor.
Y eso es lo que definirmos en los ficheros Dockerfile (imágen base de la que parto, y que comandos voy ejecutando para instalar mis cosas adicionales).