
# Automatizar!

Automatizar es crear una máquina (o cambiar el comportamiento de una mediante programas) que haga lo que antes hacía un humano con sus manos.

Puedo automatizar el lavado de la ropa (LAVADORA , que incluso le puedo cambiar su comportamiento mediante PROGRAMAS de lavado: frío, prendas delicadas...)

En nuestro caso (mundo IT) la máquina la tenemos (COMPUTADORA). Por ende, en nuestro caso: AUTOMATIZAR = PROGRAMAR

## Cuándo tiene sentido automatizar?

Solo si una tarea la voy a hacer muchas veces, automatizarla trae a cuenta. 
AUTOMATIZAR ES CARO! CREAR PROGRAMAS ES COMPLEJO Y CARO!

En nuestro caso, qué tareas se benefician de automatización:
- Despliegue de aplicaciones (donde iré sacando muchas versiones y necesite redesplegar un sistema muchas veces)
- Configuración de servidores (si solo voy a configurar un servidor... olvídate!)
- ...

## Tareas vs Procesos

> Tengo una persiana, que subo y bajo con una cuerdita. PROCESO/TAREA MANUAL

Le pongo un motor... y un botoncito para automatizar el subir y bajar de la persiana. 
Acabo de automatizar una TAREA (el hecho de subir o bajar la persiana).
El proceso es otra cosa... es un conjunto de tareas para un objetivo más grande (propósito).

El proceso está automatizado o sigue requiriendo intervención humana? Sigue necesitando intervención humana. Para qué?
- Darle al botoncito
- Mantenimiento del motor
- Tomar la decisión de cuando subir y bajar la persiana (darle al botoncito)
- Supervisar la correcta ejecución de la tarea por parte del motor

Así que llegados a este punto hablamos de 2 niveles de automatización: TAREA y PROCESO.
Para completar totalmente la automatización de un proceso, no solo debemos automatizar las tareas individuales, sino también la coordinación entre ellas... y otra cosa: Los potenciales DISPARADORES (TRIGGERS, EVENTOS) que desencadenan la ejecución del proceso también deben estar automatizados.

En nuestro ejemplo de la persiana, para automatizar completamente el proceso, necesitaríamos:
- Sensores de luz o temporizadores para decidir cuándo subir o bajar la persiana automáticamente.
- Supervisión remota para asegurarnos de que la persiana se mueve correctamente sin intervención humana directa.

El objetivo será automatizar PROCESOS!
Pero he de comenzar automatizando tareas.

En nuestro caso vamos a crear programas. Esos programas permitirán automatizar secuencias de TAREAS! llegando en algunos casos a automatizar procesos completos. Otra cosa será CUANDO DEBEN EJECUTARSE ESOS PROCESOS: APRETAR EL BOTONCITO: DARLE A PLAY (RUN) en mi programa.

La configuración del cuándo debe ejecutarse mi programa (AUTOMATIZACIÓN) va por otro lado. Y de hecho puede haber MUCHOS DESENCADENADORES que activen la ejecución de la automatización.

## Niveles de procesos

Muchas veces vamos a automatizar no solo tareas (paso 1) sino procesos... PERO a su vez, esos procesos serán parte de un PROCESO DE MAYOR ENVERGADURA, que atienda a un propósito MAYOR!

> Imaginad que quiero montar un servidor... para desplegarle un nginx. ESO ES UNA TAREA O UN PROCESO?  PROCESO 

Es un proceso que requerirá de MUCHAS TAREAS:
- Disponer de un servidor 
- Instalar SO
- Plataformado de esa máquina (configuración, redes, firewall, usuario, monitorización, claves ssh)
- Instalación del nginx
- Configuración del nginx (virtual hosts, certificados, etc.)
- Verificación del funcionamiento del nginx

Cada una de esas cosas es una tarea... todas juntas, orquestadas correctamente, constituyen un proceso.
QUE TIENE UN OBJETIVO, UN PROPOSITO DE ALTO NIVEL, de GRAN ENVERGADURA: PRODUCTO ENTREGABLE: Un servidor con NGINX operativo

> Ahora bien (y esta es la vuelta de tuerca)... para qué estoy montando ese servidor? Me gusta montar servidores nginx?

Quizás lo que quiero es desplegar un nuevo sistema en producción:
- BBDD
- KAFKA
- NGINX (Proxy reverso)
- TOMCATS 

El trabajo (PROCESO) de montar ese nginx, a su vez será parte de un proceso más amplio de despliegue del nuevo sistema en producción.
Ese NUEVO PROCESO tiene un propósito más elevado (UN PRODUCTO de mayor ENVERGADURA: El sistema completo en producción)

Aquí además, empezamos a darnos cuenta de otro concepto.
Seguramente ese proceso de instalación del nginx lo puedo REUSAR en otros despliegues o proyectos similares.

Como parte de la automatización, estará bien tener esto en cuenta... para dotar a esos PROGRAMAS de cierta FLEXIBILIDAD (que me permitan reusarlos en distintos contextos y adaptarlos a diferentes necesidades)
Sale el concepto de MODULARIDAD (Piezas de un lego que se pueden ensamblar de distintas maneras).

## Sobre herramientas de automatización

Hay un huevo! Muchas solapan entre si.. Muchas se complementan y otras compiten por el mismo espacio en el mercado.

- Terraform: Es una herramienta para automatizar. En general la usamos mucho para automatizar trabajos de infraestructura.
- Ansible:  contiene herramientas para automatizar, especialmente útiles para configuraciones, despliegues (PLANCHADO de infraestructura)

Puedo usar terraform para planchar infra? SI, pero no es su fuerte.
Puedo usar ansible para provisionar infra? SI, pero no es su fuerte.

Por eso hay muchas herramientas.

# Qué es Ansible?

Ansible NO ES UNA HERRAMIENTA... es una suite de herramientas para AUTOMATIZACION IT. Quién está detrás es Red Hat.
Dentro del ecosistema ansible encontramos:
- Ansible Core/engine: Nos permite automatizar procesos, con múltiples tareas.
- AWX/Ansible Automation Platform (antiguamente Ansible Tower): Plataforma web/rest para gestionar y ejecutar centralizadamente playbooks de Ansible (permitiendo ORQUESTAR PROCESOS automatizados y una mínima configuración de triggers de ejecución).
- Ansible galaxy: ???
- Ansible navigator: ???

Nosotros en el curso, principalmente nos enfocaremos en Ansible Core/engine.
Hacia el final del curso veremos también AWX/Ansible Automation Platform y cómo integrarlo con Ansible Core para orquestar procesos más complejos.

## Ansible Core/engine

Es un conjunto de programas que nos permite automatizar procesos (con sus tareas) de TI de forma programática (creando programas). No es un concepto nuevo... Llevamos décadas automatizando trabajos en el mundo de la administración de sistemas. Qué hemos usado tradicionalmente para ello:
- Python
- ps1
- Bash

> Por qué algo nuevo? Si ya con eso nos apañábamos? Algo aportarán

Qué aporta Ansible?
Ansible aporta 2 cosas:
- Lenguaje propio para definir/escribir esos programas (no es algo revolucionario... python es un lenguaje, bash tiene su lenguaje)... Alguna ventaja tendrá frente a los que había!
- Gestión de INVENTARIOS OUT OF THE BOX.
  Si antes quería reusar un script (programa) para distintos entornos, tenía que currarme yo una forma de hacer llegar a ese programa configuraciones para cada entorno (ficheros de texto, con props que YO DEFINIA; variables de entorno... argumentos que pasaba por línea de comandos). LO QUE FUERA, YO ME LO CURRABA!
  Esto tiene varios problemas:
  - FALTA DE ESTANDARIZACION. Si yo lo hago, lo haré como me dé la gana... y será distinto de como lo haga otra persona.
  - DIFICULTAD DE REUSO. Cada persona en la organización, en cada proyecto acababa tomando sus propias decisiones.

El tema de INVENTARIOS lo veremos el jueves (RAZONABLEMENTE SENCILLO).
De aquí al jueves (parte del jueves incluido) nos la vamos a pasar creando SCRIPTS/PROGRAMAS con el lenguaje propio de Ansible.

## Lenguaje propio de Ansible.

Qué aporta frente a los lenguajes tradicionales como Python, ps1 o Bash?

El lenguaje de Ansible usa, PARA CIERTAS PARTES DEL CÓDIGO (TAREAS), un paradigma de programación denominado **declarativo**. Mientras que los lenguajes tradicionales como Python, ps1 o Bash son **imperativos**.

ESTO ES POR LO QUE ANSIBLE LO HA PETADO!
Y no solo ansible:
- Kubernetes (OpenShift): Usa lenguaje declarativo.
- Terraform:              Usa lenguaje declarativo.
- SpringBoot:             Usa lenguaje declarativo.
- Angular:                Usa lenguaje declarativo.

El paradigma declarativo lo ha petado! Lo adoramos... Y en partes iguales, cada día más aborrecemos el paradigma imperativo... Le estamos cogiendo una manía.... ES UN TOSTON.. y nos hemos dado cuenta que estábamos muy mal acostumbrados.

## Paradigmas de programación

Básicamente un paradigma de programación es un nombre hortera que los desarrolladores de software ponemos a las formas en las que usamos un lenguaje. Pero.. ni siquiera es algo propio de los lenguajes de programación. En los lenguajes naturales (los que hablamos los humanos) está el mismo concepto.

> Felipe, pon una silla debajo de la ventana.      IMPERATIVO

Estamos muy acostumbrados a hablar con las máquinas en lenguaje imperativo. Pero es un tostón.

Para conseguir que ese "mandato" (orden) Que estamos dando a Felipe funcione en una variedad de escenarios (dado que nuestro programa lo querremos ejecutar muchas veces y no controlamos totalmente el estado inicial en el que puede estar nuestro sistema/entorno al principio), empezamos con el guarreo típico de los lenguajes imperativos.

> FELIPE, "COMPROBACION DE LA HABITACION" SUBRUTINA/FUNCION/PROCEDIMIENTO
    > FELIPE, IF HAY ALGO QUE NO SEA SILLA DEBAJO DE LA VENTANA THEN
        > QUÍTALO!                                     IMPERATIVO
    > Felipe, IF NOT SILLA (Condicionales) IF SILLA == NULO THEN
        > GOTO IKEA                                    IMPERATIVO
        > COMPRA UNA SILLA                              IMPERATIVO

> FELIPE, "COLOCACION DE LA SILLA"      SUBRUTINA/FUNCION/PROCEDIMIENTO
> Felipe, IF NOT SILLA DEBAJO DE LA VENTANA THEN   CONDICIONAL
    > Felipe, pon una silla debajo de la ventana.  IMPERATIVO

> Felipe, ejecuta: "COMPROBACION DE LA HABITACION" + "COLOCACION DE LA SILLA"  IMPERATIVO

Para conseguir que mi programa (lo que quiero que felipe haga) funcione con independencia de cómo se encuentre la habitación, necesito ir metiendo por ahí TODOS LOS POTENCIALES ESCENARIOS en los que se me ocurre que la habitación podría estar antes de ejecutar la orden.

## Ese concepto es LA IDEMPOTENCIA:

Es una propiedad de algunas operaciones matemáticas. Cuando una operación se aplica varias veces, y el resultado que obtengo sigue siendo el mismo, decimos que la operación es **idempotente**.

- 5 x 1-> 5 x 1-> 5 x 1-> 5 x 1-> 5
- 5 x 0-> 0 x 0-> 0 x 0-> 0
- 5 ^ 1 -> 5 ^ 1 -> 5 ^ 1 -> 5 ^ 1 -> 5
- +0
- -0
- Valor absoluto

En el mundo del desarrollo de software decimos que una operación es **idempotente** si con independencia del estado inicial del sistema, aplicar la operación varias veces produce el mismo resultado.

Con el script (PROGRAMA) que hemos creado para Felipe (y sus condicionales asquerosos) lo que buscábamos es que con independencia del estado inicial de la habitación, tras ejecutar el programa siempre acabe la habitación en el mismo estado final, con la silla debajo de la ventana.

Eso es asqueroso. Pero estamos muy acostumbrados a ello: INDEFENSIÓN APRENDIDA.

# Alternativa: LENGUAJE DECLARATIVO:

> Felipe: Debajo de la ventana tiene que haber una silla. ES TU RESPONSABILIDAD.      DECLARATIVO

Eso es imperativo? NO

Digo a Felipe NO LO QUE TIENE QUE HACER, Si no el estado final que quiero que se consiga.
Acabo de pasarle la pelota a Felipe: le digo el estado final que quiero, no los pasos para conseguirlo.

Él es el responsable de DETERMINAR EL PLAN DE EJECUCION NECESARIO, en base al estado inicial de la habitación para conseguir lo que le he pedido.

Y por eso EL PARADIGMA DECLARATIVO LO HA PETADO. Para esto necesito a un Felipe MAS LISTO, CON MAS CONOCIMIENTO.
BIEN: CONTRATESE!

El Felipe TONTO era el python, bash, ps1... que por si mismos (out-of-the-box) no saben muchas cosas... les puedo pedir lo que quiera SI SE LO EXPLICO DE ANTEMANO Y PASO A PASO!
El Felipe más LISTO es ANSIBLE, porque sabe muchas cosas por sí mismo (out-of-the-box) y puede determinar el plan de ejecución necesario para conseguir el estado final que le pido.
A este Felipe listo ya hay gente que se lo ha explicado despacito! 
De hecho a Felipe se las han explicado en lenguaje imperativo (PYTHON).

---

Ansible ofrece un lenguaje para crear SCRIPTS (AUTOMATIZACIONES). Ese lenguaje NO ES DECLARATIVO! ES IMPERATIVO 100%.
Igual que la bash o que python. EIN??!?!?!
Y dónde está la gracia?

Realmente Ansible NO SABE HACER LA "O" con un canuto. es torpe a rabiar.
Todo el trabajo que es capaz de hacer le viene explicado en lo que llamamos MODULOS.
Lo que hago es pedirle a ANSIBLE QUE EJECUTE UN TRABAJO mediante un MODULO!

Esos módulos han sido desarrollados por:
- Redhat
- Fabricantes de hardware
- Fabricantes de software
- Comunidad de desarrolladores

Y el 95% de esos módulos ofrecen un lenguaje DECLARATIVO.

ANSIBLE PERMITE CREAR SCRIPTS, usando lenguaje IMPERATIVO.
Pero cada tarea de ese SCRIPT es realizada (ejecutada) usando un MODULO... y la mayor parte (NO TODOS) de esos MODULOS ofrecen un lenguaje DECLARATIVO.

ESTO NOS AYUDA A CREAR SCRIPT IDEMPOTENTES de forma más sencilla que cuando usábamos lenguajes 100% IMPERATIVOS.
ANSIBLE NO ME REGALA LA IDEMPOTENCIA (hay herramientas que sí: TERRAFORM, KUBERNETES...), PERO ME LA FACILITA MUCHO!

Y cuando automatizamos es clave tener idempotencia! Que nuestros scripts sean idempotentes.

> Tengo cientos de servidores.. que quiero gestionar.

Les quiero aplicar una determinada configuración. Tengo garantizado al 100% que todos parten exactamente de la misma configuración? y del mismo estado inicial? NI DE BROMA! 

Por eso, necesito un programa que funcione con independencia del estado inicial en el que se encuentre cada servidor.

> Tengo un programa que instala algo.

Y se quedó a medias la primera vez que lo ejecuté.
Quiero volver a intentarlo... pero el estado inicial ahora, es el mismo estado inicial de la primera vez? NI DE BROMA!

Por eso necesito que el programa sea IDEMPOTENTE: que pueda ejecutarse varias veces sin importar el estado inicial y siempre consiga el resultado esperado.

---

Un programa perfectamente idempotente la segunda vez que lo ejecutase que debería hacer? NADA porque ya estaría hecho de la primera vez. PERO NADA ES NI SIQUIERA DARME UN ERROR.

# Crea una carpeta
> mkdir ventana
  make directory ventana                 IMPERATIVO

# DECLARATIVA: Asegura que existe la carpeta llamada: ventana

---

# SCRIPTS EN EL LENGUAJE DE ANSIBLE

En realidad los scripts en ansible no los llamamos "scripts", los llamamos: "PLAYS".

Y esos plays los escribimos en unos ficheros llamados "PLAYBOOKS" = Libros de plays.

En un fichero de ansible podemos definir muchos scripts.
En un playbook de ansible podemos definir muchos plays.

Esos ficheros que escribimos en ansible usan un lenguaje llamado YAML.

## Estructura de un playbook de ansible

```yaml
# Playbook: Libro con muchos plays
# Cada play es un conjunto de tareas que se ejecutan en uno o varios entornos.
# Cada elemento de la lista es un play. La lista en su conjunto es el playbook.
- name:             Nombre del primer play
  hosts:            # Valor por defecto sobre los entornos en los que querremos ejecutar el play
                    # No le damos tanta importancia
                    # Es un dato que tendemos a pasar en tiempo de ejecución
                    # Valores que solemos/podemos poner aquí:
                    # all           En todos los entornos del inventario con el que trabaje
                    # localhost     Solo en la máquina local
                    # maquina3      Una máquina específica del inventario
                    # grupoA        Un grupo específico del inventario
                    # Y muchas más... cuando nos metamos con INVENTARIOS, recuperaremos este tema!
  vars:             # Constantes! EIN?! Pues si.. constantes! YA hablaremos de ello!
  # Otro tipo de información:
  become: true      # Si quiero que las tareas se ejecuten en el entorno como usuario elevado (con permisos especiales, tipo root) 
                    # Aquí habrá también mucha tela que cortar!
  gather_facts: false   # Indica si Ansible debe recopilar información sobre los entornos de destino antes de ejecutar las tareas
                        # Me puede interesar saber cosas de esos entornos: Sistema operativo, hardware, red, procesos que hay corriendo, paquetería instalada de antemano.
                        # SIEMPRE A FALSE! EIN? Por qué? SIEMPRE A FALSE! Ya os lo contaré más adelante.
  # Y muchos más...

  # Después de ese bloque de datos (que entendemos algo así como es que es una cabecera del script) viene la chicha! Las tareas!
  # Y en ansible hay 4 etiquetas para escribir tareas:
  pre_tasks:
    - name:         Pretarea 1
    - name:         Pretarea2
  tasks:
    - name:         Tarea 1
    - name:         Tarea 2
  post_tasks:
    - name:         Posttarea 1
    - name:         Posttarea 2

  handlers:
    - name:         Handler 1
    - name:         Handler 2
  # La estructura interna de cada bloque es la misma. Cada bloque de esos define una LISTA SECUENCIAL! DE TAREAS QUE DEBEN EJECUTARSE EN ORDEN.
  # Cada tarea tendrá dentro su configuración. En la configuración de cada tarea, tendremos:
  # - name:         Mensaje descriptivo de la tarea: IMPORTANTISIMO.... BUENA NOMENCLATURA PARA LOS NAMES: CRITICO!
  # - Más cosas... en su mayoría que tienen que ver con control de flujo: LENGUAJE IMPERATIVO
  #      - when:        Cuando debe ejecutarse la tarea       ESTE ES EL IF        EL CONDICIONAL! NO ME LOS QUITO A ESTE NIVEL
  #                                                                                 ME LOS QUITO A NIVEL DE CADA TAREA POR DENTRO
  #      - failed_when:  Cuando debe considerarse que la tarea ha fallado
  #      - changed_when: Cuando debe considerarse que la tarea ha realizado un cambio en el entorno
  #      - ignore_errors: Cuando debe ignorarse un error en la tarea
  #     ...
  # - MODULO, con su configuración. Esto es propiamente la tarea que queremos ejecutar.
  #    Un módulo es una especie de PLUGIN que tenemos en ansible que sabe hacer una cosa muy concreta.
  #    Tendremos un módulo para trabajar con ficheros. Tendremos otro para trabajar con usuarios. Tendremos otro para abrir puertos en el firewall de rhel. Tendré otro para abrir puertos en el firewall de otras distribuciones, etc. Tendré otro para formatear un HDD.
  # En ansible hay MILES! Pero.. muchos MILES! de módulos. MUCHOS MILES!
  # Evidentemente en el curso NO VAMOS A APRENDER TODOS LOS MODULOS. De hecho no vamos a aprender ni el 1% de ellos.
  # Vamos a ir usando algunos... al final del curso, habremos usado unos 30-40 módulos... los más habituales al trabajar con entornos linux (en especial RHEL).
  # El objetivo no es aprender todos los módulos. ES TAREA IMPOSIBLE!
  # El objetivo es aprender a ser capaces de trabajar CON CUALQUIER MODULO!
  # En paralelo iremos aprendiendo algunos de ellos... importantes, de los que se usan siempre.
  # Cada módulo requerirá de una configuración específica, que dependerá de la tarea que queramos realizar con ese módulo.
  # Y esos módulos son los que en su mayoría (NO TODOS) hablan lenguaje declarativo.
  # Ejemplo:
   - name:         Asegurar que directorio queda creado como yo quiero en el sistema de archivos 
     file:
       path:       /tmp/mi_directorio
       mode:       '0755'
       state:      directory             # Tengo una propiedad llamada state: ESTA PROPIEDAD INDICA EL ESTADO FINAL DESEADO!
                                         # Que tipo de lenguaje es esto? DECLARATIVO
    # El 95% de los módulos van a tener propiedad state: que indica el estado final deseado.
    # Estos son los modulos que SI HABLAN LENGUAJE DECLARATIVO.
    # Habrá algunos que NO, que hablan imperativo. Habrá que lidiar con ellos.

```

El script: Secuencia de tareas que deben ejecutarse es IMPERATIVO: Va en orden... lleva condicionales, tratamiento de errores...
Lo que hace la tarea por dentro es en su mayoría DECLARATIVO: Indica el estado final deseado, no cómo llegar a él.

Tenemos pre-tareas (pre-tasks), post-tareas (post-tasks), tareas normales (tasks) y handlers. Para qué sirve esto?

Los pre-tasks, las tasks y las post-tasks funcionalmente SON IGUALES! IDENTICAS. Simplemente se ejecutan en orden.
Primero las pre-tasks, luego las tasks normales y finalmente las post-tasks.
Y dentro de esas en el orden definido por el playbook.

```yaml
# ...
 pre_tasks: 
   - name: Tarea 1
     # ..lo que sea que haga
   - name: Tarea 2
 tasks:   
   - name: Tarea 3
   - name: Tarea 4
 post_tasks:
   - name: Tarea 5
   - name: Tarea 6
```

Eso ejecutará las 6 tareas en el orden: 
1. Tarea 1
2. Tarea 2
3. Tarea 3
4. Tarea 4
5. Tarea 5
6. Tarea 6

Y eso sería igual que si mi playbook fuera:

```yaml
pre_tasks: 
  - name: Tarea 1
  - name: Tarea 2
  - name: Tarea 3
  - name: Tarea 4
  - name: Tarea 5
  - name: Tarea 6
# Sin bloque tasks ni post_tasks
```

Este playbook también ejecutaría: 
1. Tarea 1
2. Tarea 2
3. Tarea 3
4. Tarea 4
5. Tarea 5
6. Tarea 6

Igual que el playbook:

```yaml
    # ...
    tasks:
      - name: Tarea 1
      - name: Tarea 2
      - name: Tarea 3
      - name: Tarea 4
      - name: Tarea 5
      - name: Tarea 6
```
Los 3 ejemplos ejecutarían las mismas tareas en el mismo orden: Tarea 1, Tarea 2, Tarea 3, Tarea 4, Tarea 5 y Tarea 6.

¿Entonces para qué esto de tener 3 bloques, en lugar de poner todo en uno?
Hay 2 motivos:
- Organizar mejor conceptualmente el playbook!         IMPORTANTISIMO!
  Esto hará que a futuro sea más fácil entender y mantener el playbook... cuando lleguen cambios.  LLEGARÁN! ESTA ESCRITO!
    - PRE TAREAS        Tareas necesarias para poder acometer las tareas principales de mi playbook
    - TAREAS            Las tareas necesarias para el objetivo principal de mi playbook
    - POST TAREAS       Tareas adicionales una vez el trabajo esté acabado.

> Ejemplo: Quiero un playbook que instale/configure un nginx en un rhel

        PRETAREAS:
        - Instalar en la máquina paquetería requerida por el nginx (curl...)
        - Dar de alta en los repositorios de la máquina, uno que tenga la versión de nginx que quiero instalar
        - Verificar que el servidor tiene recursos necesarios y suficientes (red, cpu...) para poder correr el nginx.

        TAREAS:
        - Instalar el nginx... cómo se hace en un rhel?     "dnf install nginx"
        - Colocar el archivo de configuración de nginx según necesidades
        - Dejar el servicio corriendo
        - Que en el siguiente arranque el servicio de nginx se inicie automáticamente
        - Abrir ciertos puertos

        POSTTAREA:
        - Asegurarme que ha quedado bien instalado
        - Notificar a quien competa el trabajo que hemos hecho o si ha habido un error.


- Pero hay otra cosita a nivel funcional que cambia... MAÑANA LO DESVELAMOS!

## HANDLERS
Hemos hablado que había 4 bloques donde definir tareas: pre_tasks, tasks, post_tasks y handlers.
He hablado de los tres primeros, pero no del último:

```yaml
handlers:
  - name:      Tarea 1
  - name:      Tarea 2
  - name:      Tarea 3
```

Los handlers contienen tareas que NO SE EJECUTAN a no ser que alguien las ACTIVE EN TIEMPO DE EJECUCION!
Hay 2 formas de activarlas. Siempre se activan como consecuencia de la ejecución de otra tarea. Y solo si esa otra tarea ha provocado un cambio en el entorno.

Es decir, tendremos una tarea 1... que puede provocar un cambio en el entorno (o no).
Caso que lo provoque, esa tarea puede solicitar la ejecución de un handler.

Los handlers solo se ejecutan una única vez, aunque hayan sido activados muchas veces.
Y se pueden activar de 2 formas:
- FORMA CUTRE (que evitamos encarecidamente): A través de su nombre.
- FORMA GUAY! (que es la que vamos a usar) : A través de un EVENTO

> Ejemplo:
```yaml
# FORMA CUTRE: Invocarlos por nombre:
# Estamos en nuestro playbook de instalación del nginx
tasks:
    - name: Asegurar que el nginx queda instalado en la versión que quiero
    - name: Asegurar que el nginx queda con la configuración que quiero
      notify: Asegurar que el nginx es reiniciado
    - name: Asegurar que el nginx sirve una páginas por defecto que yo quiero (error de no encontrado...)
      notify: Asegurar que el nginx es reiniciado

handlers:
    - name: Asegurar que el nginx es reiniciado
# Con notify especificamos handlers que deben ejecutarse SOLO SI la tarea provoca un cambio en el entorno.
---
# Forma guay: Usando eventos:
tasks:
    - name: Asegurar que el nginx queda instalado en la versión que quiero
    - name: Asegurar que el nginx queda con la configuración que quiero
      notify: CAMBIO_EN_CONFIGURACION
    - name: Asegurar que el nginx sirve una páginas por defecto que yo quiero (error de no encontrado...)
      notify: CAMBIO_EN_CONFIGURACION

handlers:
    - name: Asegurar que el nginx es reiniciado
      listen: CAMBIO_EN_CONFIGURACION  # Esto es un evento
```

Puede ser que el mismo evento desencadene la ejecución de varios handlers diferentes.
Esto ayuda a aplicar un principio de desarrollo de software, del que mañana hablaré llamado: SoC: Principio de separación de preocupaciones.
--- 

# PLAYBOOK = SCRIPT = PROGRAMA

Que un programa funcione, no es meritorio, se da por descontado.
El objetivo NO ES QUE FUNCIONE BIEN... eso se da por descontado!

ESTO A VECES NOS CUESTA ENTENDERLO . MUCHO!

El objetivo cuando creamos software (y es lo que estamos haciendo) no es que funcione bien... es que sea barato mantenerlo a medio y largo plazo!
ESTA ES LA CLAVE! Igual que un coche!, cuando los ingenieros lo diseñan.

Lo cambio de industria:

> Me voy a comprar un coche.

El objetivo es que el coche que me compre funcione? que me lleve de un sitio a otro? ESO SE DA POR DESCONTADO.. coño, no me voy a comprar un coche que no funcione.
De hecho para recibir el nombre "COCHE" el coche debe funcionar correctamente, eso es un requisito básico y no un mérito.
Si no me transporta pues no es un coche... será una escultura de hierro con ruedas!

Un programa que no funcione SERÁ UN DOCUMENTO DE TEXTO CON PRETENSIONES! pero no un programa... Si es un programa, merecedor de ese nombre, debe funcionar correctamente.
