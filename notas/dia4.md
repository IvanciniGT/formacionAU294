# Repaso a la instalación de paquetes

Necesitamos instalar paquetes en nuestras máquinas remotas.

En varios momentos del tiempo:
- Dependencias      N paquetes
- Nginx             1 paquete

Añadir un paquete en cualquier caso, implica las mismas operaciones:
- Si me dan repo, configurarlo en el sistema (puede tener clave asociada o no).
  Doble condicion:
    - Repo definido
      - Repo con clave asociada
      - Repo sin clave
    - Repo no definido
- Actualizar los repositorios (yum update o dnf update o apt update)
  Lo que hacemos es descargar a nuestra máquina qué paquetes hay disponibles en los repositorios configurados.
  Generamos una cache local de los paquetes disponibles en los repositorios remotos.
- Instalar el paquete
  Una vez tenemos los repositorios configurados y actualizados, podemos instalar el paquete deseado.
  Dependiendo de si nos han dado una versión específica o no, la instalación puede variar. Y matchings para esas versiones.
  Varios escenarios:
  - Que solo me den el nombre del paquete, sin versión específica:
    dnf install <nombre_del_paquete>    (montará la última versión disponible)
    apt install <nombre_del_paquete>    (montará la última versión disponible)
  - Que me den una versión exacta (matching =)  
    dnf install <nombre_del_paquete>-<versión>
    apt install <nombre_del_paquete>=<versión>
  - Que me den una versión mínima (matching >=)
    dnf install <nombre_del_paquete> >= <versión>
    apt install <nombre_del_paquete> >= <versión>  # Nota, sin espacios alrededor del operador >=

Como lo usaremos en varios sitios, mejor crear un conjunto de tareas reusables que podamos invocar cada vez que necesitemos instalar un paquete. Invocar un listado de tareas es algo sencillo en ansible: Módulo ansible.builtin.include_tasks.

Me pongo a definir esas tareas. Y me doy cuenta que necesitamos variables:
- nombre_del_paquete: Nombre del paquete a instalar
- version: Versión específica del paquete (opcional)
- operador: Operador de comparación para la versión (opcional, puede ser "=" o ">=" o ">"...)
- repo: Repositorio desde el cual instalar el paquete (opcional)
- clave: Clave asociada al repositorio (opcional)

    En humano:
    > Instala git                                                                       # Tiro de repos oficiales
    > Instala git versión 2.30 o superior                                               # Tiro de repos oficiales
    > Instala git desde el repositorio "extras" con clave asociada "clave_extras"       # Tiro de repos específicos
    > Instala git versión 2.30 exacta desde el repositorio "extras" sin clave           # Tiro de repos específicos
    > ...


---
# Ansible ROLES

Un role de Ansible es un conjunto de tareas reusables. Pero resusables no solo dentro de un playbook, sino que también pueden ser compartidos entre distintos playbooks y proyectos.

Hay una herramienta en Ansible que nos permite gestioanr roles: ansible-galaxy.

Una vez creados esos roles tenemos 2 opciones:
- Dejarlos en una subcarpeta de un playbook = RUINA!
- Dejarlos en un repositorio de git independiente y luego incluirlos en los playbooks mediante ansible-galaxy. = GUAY!

> Qué metemos en un PLAYBOOK?

tareas (hadlers)
variables
argumentos
constantes
templates
files
inventario

> Qué es lo que podría yo reusar de eso?

tareas? SI
variables? SI
argumentos? SI
constantes? SI
templates? SI
files? SI
inventario? NO


Básicamentela diferencia entre un playbook y un role es que el playbook define inventario... y los roles no.

# Cómo crear un role:

$ ansible-galaxy role init instalacion_paquetes

Esto da lugar a una carpeta que tiene dentro subcarpetas:

defaults      ARGUMENTOS
files         ARCHIVOS para copiar a los hosts remotos
templates     PLANTILLAS JINJA para generar archivos dinámicamente en los hosts remotos
tasks         TAREAS DEL ROLE
handlers      MANEJADORES DEL ROLE (opcional)
vars          CONSTANTES INTERNAS DEL ROLE
meta          METADATOS DEL ROLE (opcional)
  Título
  Descripción
  Licencia
  Dependencias (opcional)
  Si comparto publicamente el role, incluir información relevante como el autor y la URL del repositorio (opcional)

¿Cómo lo uso ahora?

Cuando tenía las tareas dentrol del playbook en una carpeta, lo que usaba era include_tasks.

¿Dónde busca ansible el role?

Hay muchas rutas en las que lo busca:
- Dentro del playbook, en la carpeta roles/<mi_role_name>
- Dentro del playbook, en la carpeta <mi_role_name>
- ~/.ansible/roles/<mi_role_name>
- /usr/share/ansible/roles/<mi_role_name>
- /etc/ansible/roles/<mi_role_name>

Cuando ejecuto ansible, puedo tener en la misma carpeta un archivo llamado ansible.cfg que me permita definir rutas adicionales para buscar roles.

```ini
[defaults]
roles_path = ./roles:/etc/ansible/roles # Las que quiera poner
```

Variable de entorno:
ANSIBLE_ROLES_PATH
    Permite definir rutas adicionales para buscar roles, similar a la opción roles_path en ansible.cfg.
    Ejemplo:
    export ANSIBLE_ROLES_PATH=./roles:/etc/ansible/roles

Hay 500 formas de indicarle a Ansible dónde buscar roles, ya sea mediante la configuración en ansible.cfg, la variable de entorno ANSIBLE_ROLES_PATH o las rutas por defecto que ya conoce.

Si es un role que tengo publicado en Ansible Galaxy o en otro repositorio, puedo instalarlo directamente usando:

$ ansible-galaxy install <nombre_del_role>

Esto descargará el role en la ruta por defecto o en la ruta que haya definido en ansible.cfg o mediante la variable de entorno ANSIBLE_ROLES_PATH.

Lo normal en mi empresa es que mis roles los guarde en mi sistema de control de versiones (GITHUB, GITLAB, BITBUCKET, etc.)

Y los instalo igual:

$ ansible-galaxy install git+https://github.com/mi_empresa/mi_role.git

Necesito tenerlo instalado donde corre el playbook. Instalado es un eufemismo... realmente es solo descargar los archivos del role.

---

Hemos visto 3 módulos conceptualmente muy similares:
- include_tasks
- include_role
- include_vars

Existen otros 3 módulos en ansible , parecidos a estos... pero que se comportan de forma muy diferente:
- import_tasks
- import_role
- import_vars

Diferencias entre include_ y import_:

Los imports_ se procesan en el momento en que se lee el playbook, es decir, antes de que comience la ejecución. 
Los include_ se procesan en el momento en que se ejecuta la tarea correspondiente, es decir, durante la ejecución del playbook.

Ya que el include se ejecuta durante la ejecución del playbook,puedo cargar cosas dinámicamente, como variables o tareas, dependiendo de las condiciones que se cumplan en ese momento:
- include_tasks: "/micarpeta/{{ variable }}/tareas.yml"
Esto en un import no puede hacerse. El import se procesa antes de que se ejecute el playbook. Y en ese momento la variable aún no tiene un valor definido.

En los imports no puedo usar loops.
En los includes sí puedo usar loops, ya que se procesan durante la ejecución del playbook.

En los imports el when se aplica / hereda a las tareas que se importan.
En los includes el when se aplica solo a la tarea de include, no a las tareas que se incluyen.

En los imports los tags se aplican (heredan) a todas las tareas que se importan.
En los includes los tags se aplican solo a la tarea de include, no a las tareas que se incluye (puedo inyectarlas manualmente si quiero: apply: tags: <tag_name>).

En general usamos mucho más include_ que import_, ya que nos permite mayor flexibilidad durante la ejecución del playbook.

No hay muchas ventajas o casos donde convenga usar import_ en lugar de include_. Qué me ofrecería:
- Visibilidad antes de ejecutar el playbook.
  $ ansible-playbook --list-tasks mi_playbook.yml
    Con import_ se listarán todas las tareas importadas, ya que se procesan antes de la ejecución del playbook.
    Con include_ solo se listará la tarea de include, no las tareas que se incluyen, ya que se procesan durante la ejecución del playbook.

---

# Inventarios

Los inventarios son la colección de hosts/entornos remotos sobre los que potencialmente podré ejecutar mis playbooks.
Cuando ejecuto un playbook diré sobre que máquinas concretas quiero que se ejecute.
Pero esas máquinas tienen que estar definidas en el inventario.

El inventario no contiene solo la relación de máquinas. También incluye:
- La forma de conexión a esos hosts (usuario, clave, método de conexión, etc.)
- Variables específicas de cada host o grupo de hosts.
- La forma en la que escalo privilegios en esos hosts (por ejemplo, usando sudo, su, etc.).
- La ruta en la que está instalado python en esos hosts, si no está en el path.

Las máquinas las puedo ir AGRUPANDO: Grupos de hosts.
Y de hecho puedo tener estructuras jerárquicas a varios niveles:

mi_intentario:
  servidores_web:
    nginx:
      host1.example.com
      host2.example.com
    apache:
      host3.example.com
      host4.example.com
  bbdd:
    postgresql:
      host5.example.com
      host6.example.com
    mariadb:
      host7.example.com
      host8.example.com

Los grupos también admiten variables, que heredan los hosts que pertenecen a esos grupos.

> Cómo se definen inventarios?

Ansible ofrece 3 SINTAXIS diferentes para definir inventarios... Y SOLO UNA ES BUENA!

- Archivos .ini                        RUINA!
- Archivos yaml... aquí ya flipas!     IGUAL DE RUINA!
- Directorios                          BUENA OPCIÓN!

# Que podemos meter en un directorio de inventario?
- Archivos yaml o ini para definir hosts
- Archivos yaml para variables a nivel de host o grupo.
- Subdirectorios para grupos

En las empresas lo normal es tener un CMDB, una BBDD de configuración donde se almacena información sobre los hosts y sus relaciones, y a partir de ahí generar los inventarios de Ansible de forma dinámica.

O hay sistemas/plataformas de gestión de infra que tienen sus propios CMSBs. Ejemplo:
- VMWARE
- Clouds (AWS, Azure, GCP, etc.)

Lo guay es poder usar esos CMDBs.

Dentro de la carpeta del inventario, además de archivos .yaml .ini para definir hosts, podemos tener cualquier archivo ejecutable, que genere en salida estandar un inventario en formato YAML, JSON, INI con las máquinas de un CMDB o cualquier otra fuente de información.

Puedo tener un programa python que conecte con mi cmdb y genere en salida estándar un inventario en formato YAML, JSON o INI, que Ansible pueda consumir directamente. ESTO ES GUAY!

No solo porque me evita escribirlo. Por otro motivo mucho más importante: NO HAY DUPLICIDAD DE LOS DATOS. SOLO HAY UNA FUENTE DE VERDAD: Mi CMDB.

Ahora.. quizás en el cmdb tengo el host, su ip, usuario... tipo de sistema operativo.
La versión de nginx o de postgres o el puerto que abro en el firewall probablemente no esté en el CMDB, y eso lo puedo definir en los archivos de inventario. Parte es dinámica (del cmdb) y parte es estática (definida en los archivos de inventario).

Ansible AUTOMATION PLATFORM (AWX) lleva de serie un huevo de esos programas que permiten generar inventarios dinámicos a partir de diferentes fuentes de información, como CMDBs, nubes públicas, y otros sistemas de gestión de infraestructura.


# Cuando ejecuto un playbook

Dentro del playbook viene una marca "hosts", con un valor por defecto, indicando a qué hosts o grupos de hosts se va a aplicar ese playbook.

Pero siempre queremos limitar eso en tiempo de ejecución:
$ ansible-playbook -i <inventario> <playbook> --limit <hosts_o_grupos>

Podemos también excluir hosts o grupos usando la opción --limit con el prefijo '!':
$ ansible-playbook -i <inventario> <playbook> --limit '!<hosts_o_grupos>'

Puedo combinar grupos:
$ ansible-playbook -i <inventario> <playbook> --limit '<grupo1>:<grupo2>'

También puedo combinar inclusión y exclusión:
$ ansible-playbook -i <inventario> <playbook> --limit '<grupo1>:<grupo2>:!<grupo3>'


La pregunta es .... dónde se ejecuta ese comando?
En mi PC? SI... cuando desarrollo!
Cuando ya está apto para producción lo ejecutaré en un servidor central (AWX/Ansible Automation Platform)
De hecho, lo normal es que tenga varios Ansible Automation Platform distribuidos, por ejemplo uno para desarrollo, otro para preproducción y otro para producción.

Es decir, este comando se ejecutará desde MUCHOS SITIOS.
Y aquí hay un problema! ENORME!

# Qué necesito para ejecutar un playbook?

- Tener instalado ansible... en qué versión?
- Necesito tener ciertas colecciones instaladas, en qué versión?
ansible.builtin.copy              copy, synchronize son MODULOS
ansible.posix.synchronize         ansible.builtin, ansible.posix son COLECCIONES
- Qué necesitaba mi playbook? curl? En qué máquina...
- Y los roles? Necesito que estén descargados y disponibles en la ruta correcta para que el playbook los pueda usar.

En cuantito en uno de los entornos donde vaya a ejecutar el playbook, falte algo o haya una versión de algo distinta, corro el riesgo de que el playbook falle y no se ejecute correctamente.

Necesito asegurar que el entorno donde desarrollo / pruebo el playbook sea IDENTICO al entorno donde se ejecutará en producción.
Lo que paso a producción NO ES SOLO EL PLAYBOOK, SINO EL ENTORNO donde corro el playbook.
Además, que cada PLAYBOOK tendrá su entorno! con sus propias versiones de ansible, colecciones, roles y dependencias.

MADRE DEL AMOR HERMOSO!!!!! La que acabamos de liar!

La gente de REDHAT si se lo ha planteado!
Y le han dado solución: CONTENEDORES!

Los playbooks de ansible se ejecutan dentro de contenedores, lo que permite que cada playbook tenga su propio entorno aislado con las versiones correctas de ansible, colecciones, roles y dependencias.

Redhat sacó hace ya 3/4 años una herramienta llamada ansible-navigator y ansible-builder.
Esta es la forma que usamos HOY EN DIA para crear entornos donde correr los playbooks.

cuando subo mi playbook a un sistema central: AWX, Ansible Automation Platform, no subo solo el playbook... sino que subo también el contenedor (IMAGEN) con el entorno necesario para ejecutarlo.

---

# Contenedores

Es un entorno aislado, dentro de un sistema operativo Linux, donde correr procesos.
Aislado?
- Tiene su propia configuración de red (y su propia IP)
- Tiene su propio sistema de archivos aislado
- Tiene sus propias variables de entorno, como entorno independiente
- Puede tener limitaciones en cuanto a recursos como CPU, memoria o almacenamiento.

Son una alternativa a las VM para tener entornos aislados sobre los que correr procesos.
Tienen muchas ventajas frente a las VM para algunos casos de uso.... lo que si para la inmensa mayoría.
El 90% de las cosas que antes resolvíamos con VM ahora se pueden resolver con contenedores.

Linux? Kernel de SO
Igual que Windows tiene su kernel... el que se usa desde hace más de 20 años ya microsoft se llama NT (New Technology) Kernel.

En las empresas usamos mucho un Sistema Operativo llamado GNU/Linux, que se ofrece habitualmente en forma de distribuciones como Red Hat Enterprise Linux, CentOS, Fedora, Ubuntu, Debian, entre otras. GNU/Linux rueda/ejecuta un kernel Linux... como muchos otros SOs:
- Android
- Chrome OS
- Windows! (WSL2) Esto es característica estandar de Microsoft Windows

La diferencia grande con respecto a VMs es que los contenedores no tienen su propio SO. Ni grande ni chico!
Usan el kernel de SO del host donde se ejecutan. El kernel LINUX tiene funcionaldiades que eprmiten crear entornos aislados para los para ejecutar procesos. De eso hacen uso los contenedores.
El impacto:
- Las imágenes de contenedor (igual que las imágenes ISO de una VM) no llevan un SO... y ocupan nada!
- Los procesos que corren dentro del contenedor (entorno aislado) Están en comunciación directa con el kernel del host, lo que permite un rendimiento igual al nativo. En las MVs el rendimiento se degrada... y chupan recursos por un tubo( Es necesario levantar otro SO completo).
Los contenedores los creamos desde IMAGENES DE CONTENEDOR: Triste archivo comprimido (tar) que tienen dentro PREINSTALADO! un programa y todo lo que ese programa necesita para funcionar. (de hecho , la realidad es que en cualquier imagen de contenedor tenemos cientos-miles de programas preinstalados)

- nginx (nginx, ls, cp, mv, sh, ... cientos de comandos/programas preinstalados)
Para crear un contenedor y ejecutar software dentro, lo único que hacemos es DESCOMPRIMIR EL TAR (la imagen del contenedor) y arrancar el programa que queremos ejecutar dentro del contenedor. La gracia es que ese programa corre en un entorno aislado:
- Sus propias variables de entorno
- Su propio sistema de archivos
- Su propia configuración de red (y su propia IP)
- Limitaciones en cuanto a recursos como CPU, memoria o almacenamiento.

Es decir, LO MISMO QUE CONSIGO CON UNA VM (salvo el poder disponer de un SO propio)

Hoy en día TODO El software empresarial se distribuye mediante IMAGENES DE CONTENEDOR.

Redhat, junto con Microsoft y AWS fueron los primeros que se metieron de cabeza en el mundo de los contenedores.. Año 2013... firmando acuerdos con Docker Inc. cuando surgió como startup.

Ansible Tower / AWX / Ansible platform hacen uso intensivo de contenedores. Los playbooks se ejecutan dentro de contenedores, lo que permite un entorno controlado y reproducible para la automatización de tareas.

El desarrollador del playbook no dice solo cómo es su playbook, parte de su trabajo es DEFINIR EL ENTORNO DONDE ESE PLAYBOOK DEBE EJECUTARSE!
- Qué versión de ansible
- Qué dependencias de SO
- Qué dependencias de python
- Qué colecciones de ansible
- Qué roles de ansible

Y qué versiones de todo ello!

Redhat me lo pone fácil. Me da unos ficheros llamados execution environments. En esos ficheros, el creador del playbook configura lo que requiere el entorno para que el playbook se ejecute correctamente.

Con ese archivo podemos generar en automático una imagen de contenedor:

$ ansible-builder build -f execution-environment.yml -t my-execution-environment:latest

Una vez generada la imagen de contenedor, necesitamos subiarla a un registry de repos de imagenes de contenedor:
- docker hub
- quay.io
- microsoft artifact registry
- Normalmente las empresas usas sus propios registries privados para almacenar las imágenes de contenedor:
  - gitlab registry
  - harbor
  - nexus/artifactory

Si en mi máquina tengo docker instalado, puedo subir la imagen generada a un registry con el comando:

$ docker push my-execution-environment:latest

Lo normal es que yo cree mis imagenes de contenedor para jugar en local, cuando estoy desarrollando o probando playbooks.

Cuando acabo, subo el playbook junto con el execution environment a un repo de git.
Y en el repo de git, correrá un programa (pipeline de cd-jenkins, gitlab ci/cd...) que se encargará de construir la imagen de contenedor a partir del execution environment y subirla al registry correspondiente.

Y después es cuando nos vamos a Ansible container Platform o Ansible Tower, y alli, doy de alta:
- El repo git de mi playbook
- El execution environment asociado a ese playbook