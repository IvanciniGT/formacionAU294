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

---

# GIT es un sistema de control de versiones.

## COMMIT

Es una FOTO COMPLETA (ZIP COMPLETO) del estado del proyecto en un momento dado del tiempo. -> VERSION

Los commits llevan una secuencia:
  C1 -> C2 -> C3

LPuedo tener varias lineas de evolución temporal independientes: RAMAS / BRANCHES

  C1 -> C2 -> C3
   \
   C1 -> C4 -> C5

Siempre trabajamos con una rama llamada "main".
Lo que hay en esta rama se considera APTO PARA PRODUCCION.

Los desarrolladores, que trabajamos en proyectos de SOFTWARE COMPLEJOS, con muchos archivos, usamos muchas ramas para trabajar.

Muy habitual es tener una rama "desarrollo" (dev, desa, develop, development)... aquí se van haciendo cambios... hasta que está el sistema MADURO como para liberar una versión a producción. Cuando se quiere liebrar, la ultima foto que hay ahi, se copia a la rama "main".

De hecho los desarrolladores no trabajamos solo con esas 2 ramas.
Para cada cambio que queremos hacer en el código creamos una rama nueva.. a este le llamamos rama de feature o rama de trabajo.

Esto tiene sentido cuando trabajo en un proyecto MUY GRANDE, con un EQUIPO MUY GRANDE, lo que es habitual en desarrollo de SOFTWARE COMPLEJO.

Cuando trabajo con playbook, somos 2/3 como mucho, y trabajamos con pocos archivos, y no tenemos un sistema con 50 features diferentes, crear ramas de feature para cada cambio es innecesario.

En la realidad tiene sentido 2 cosas cuando trabajo con ansible:
- MAS REGULERA .. pero en muchos casos suficiente: O bien tengo solo rama "main" y voy poniendo todo ahí
- GUAY ! O bien tengo desarrollo (dev, desa, develop, development) y voy poniendo los cambios ahí antes de pasarlos a main

Qué pasa en AWX?

NO TENDRE EN LA EMPRESA SOLO UN AWX!
Tendré uno para producción y otro para pruebas al menos!

  En cada uno, daré de alta el REPO de GIT (Proyecto).
  Pero en uno PRODUCCION, trabajaré sobre ALGO ESTABLE.
  En el de PRUEBAS, trabajaré sobre una versión... que estoy provbando, que puede contener errores.

  PERO TODO ESTARA EN EL MISMO REPO DE GIT.
  Es decir, lo que trabajo es contra COMMITS diferentes dentro del mismo REPO.

  Si trabajo con 2 ramas, por ejemplo "main" y "desarrollo", en AWX de producción trabajaré sobre los commits de "main" y en AWX de pruebas trabajaré sobre los commits de "desarrollo".

  El problema es si no trabajo con 2 ramas... Si solo trabajo con una.
  Entonces? en producción pongo main y en desarrollo pongo main también?
  Esto significa que cualquier cambio que haga (que lo tengo que subir a main para probarlo) en automático se reflejará también en producción, lo cual puede ser peligroso si el cambio contiene errores. NO VALE!

  En este caso, lo que hacemos es:
  - DESARROLLO: main
  - PRODUCCION: tags (versiones estables)

Cuando genero Commits, a alguno de esos commits les puedo poner etiquetas.

  C1 (alta del proyecto) -> trabajo un poquito y digo.. voy a probar -> C2 (sin etiqueta)
  Lo pruebo.. y veo que falla algo. Lo arreglo y genero un nuevo commit -> C3 (sin etiqueta)
  Y así voy madurando la solución (playbook). Cuando estoy satisfechop: C20 
  Ese commit le pongo una etiqueta de versión: v1.0.0

  En desarrollo pongo en el repo rama main! Y que vaya cogiendo lo último de esa rama: C1.. luego el C2, C3, hasta C20.
  En producción, en cambio, solo cogeré los commits que tengan etiquetas, por ejemplo v1.0.0.

Trabajar con 2 ramas da lugar a una configuración más sencilla en AWX:
- Producción: main
- Desarrollo: desarrollo (o la rama que use para pruebas)
Y según salgan versiones nuevas en rama main, NO NECESITO CAMBIAR NADA en la configuración de AWX, ya que siempre estará apuntando a la rama main en su último commit.
Pero me toca estar copiando cosas de una rama a otra en GIT.

Por contra, trabajar con una sola rama en git, requiere configuración más compleja en AWX:
- AWX de producción apunte a los tags (versiones estables)
- AWX de desarrollo apunte a la rama main.

Pero cada vez que genero una nueva versión estable, debo crear una etiqueta en git y asegurarme de que AWX de producción apunte a esa etiqueta. Esto añade un paso adicional en el flujo de trabajo, pero garantiza que los cambios en desarrollo no afecten directamente a la producción.




registry.ivanosuna.com/curso/ee-servidor-web:v1.2.0

Hay etiquetas, como la que yo he puesto que es FIJA, apunta a una versión concreta: v1.2.0
Pero muchas veces creamos etiquetas VARIABLES: latest

Esa etiqueta ahora  apunta a la v1.2.0, pero mañana puede apuntar a la v1.2.1 o cualquier otra versión que se publique como "latest".

Si pongo extraer siempre, siempre descargará la imagen... por si tiene cambios.
Si pongo extraer si no existe, solo descargará la imagen si no la tengo localmente. 
  Peroesto puede dar problemas si uso tags variables.
  Puede ser que tenga ya la imagen "latest" en local.. pero que esté apuntando a una v1.2.0... cuando en el registry ya existe
  una version "latest" que apunta a la versión v1.3.0.. pero a mi no se me descargaría automáticamente si ya tengo la versión anterior en local.

EXTRAER SIEMPRE lo ponemos cuando trabajo con tags VARIABLES
Cuando trabajo con tags FIJOS (v1.2.0) preferimos poner solo si no existe.. así va más rápido.