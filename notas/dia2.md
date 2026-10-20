
# Automatizar

En nuestro mundo (IT) automatizar = PROGRAMAR.
No es desarrollo de software en sentido amplio (creando cualquier tipo de software)... sino crear scripts que automaticen tareas específicas / procesos.

Dado que vamos a a crear software, es importante aprender y aplicar conceptos de la gente que lleva décadas pegandose con el software.

## Distintos niveles en la automatización:

- Tareas individuales: Automatizar tareas específicas y repetitivas.
- Procesos completos: Automatizar flujos de trabajo que involucran múltiples tareas.
  A su vez, los procesos pueden ser parte de procesos de mayor envergadura, formando así una jerarquía de automatización dentro de la organización.

## Nota sobre automatizar.

Para poder plantearme automatizar un trabajo, NECESITO SABER UN HUEVO de lo que sea que voy a a automatizar.
Estas herramientas NO ME LO PONEN FACIL.
ME LO PONEN MAS COMPLICADO!

Es mucho MUCHO más dificil automatizar un trabajo que hacerlo manualmente.
Entre otras cosas porque cuando lo hago a mano tengo la oportunidad de IR APRENDIENDO SOBRE LA MARCHA.
Cuando automatizo, el conocimiento lo tengo que tener de antemano... No aprendo sobre la marcha!
Creo un programa que hará el trabajo. Y A ESE PROGRAMA LE TENGO QUE DAR HASTA EL MAS MINIMO DETALLE.

## Devops

Devops es una cultura, una filosofía, un movimiento en pro de LA AUTOMATIZACION.
Cuando en una empresa decimos: CHIC@S VAMOS A AUTOMATRIZAR TODO LO QUE PODAMOS => DEVOPS!
Ese pensamiento es devops!

Choca el nombre, cuando lo vemos sin perspectiva, sobre todo desde el lado de sistemas.

La infra per sé aporta CERO VALOR a la organización!
APORTAR VALOR EN TANTO Y CUANTO PERMITE RODAR SOFTWARE ENCIMA!
EL SOFTWARE per sé aporta CERO VALOR A LA ORGANIZACIÓN!
SOLO APORTA VALOR EN TANTO Y CUANDO PERMITE GESTIONAD DATOS QE PORTAN VALOR A NEGOCIO.
LOS DATOS ES LO ÚNICO QUE REALMENTE APORTAN VALOR A LA ORGANIZACIÓN.

Cuando monto una infra (quitando cosas muy genericas... que también, pero no lo asociados directamente a programas **VER 1)
hay una relación INTRINSECA ENTRE SOFTWARE <> INFRAESTRUCTURA.

PLAN
    CODE
       BUILD
          TEST
            RELEASE
                DEPLOY
                    MONITOR

Y Quiero automatizar TODO lo que hay ahí susceptible de ser automatizado.
- El empaquetar el código que hace un desarrollador (compilarlo, linkarlo, ensamblarlo) es una tare que puedo automatizar.
- Una prueba que hago sobre ese sistema (TEST) es otra tarea que puedo automatizar.
- Crear / Provisionar una infraestructua es otra tarea que puedo automatizar.

De hecho, provionar la infra no es realmente una tarea... es un proceso grande en si mismo, que englobará muchas tareas:
- Crear servidores
- Instalares paquetería
- Sistema de monitorización
- Configuración de redes
- Almacenamiento y backups

Las pruebas SON OTRO PROCESO:
- Pruebas unitarias
- Pruebas de integración
- Pruebas de aceptación
- Pruebas de rendimiento
- Pruebas de seguridad
- Pruebas de calidad de código

El compilar y empaquetar un sistema, realmente es otro proceso que engloba varias tareas relacionadas con la construcción del softwar:
- Descargar dependencias de terceros
- Compilar el código fuente
- Linkar bibliotecas
- Ensamblar módulos
- Empaquetar el sistema

Pero luego, puedo unir esos procesos! en otros procesos de más alto nivel:

Compilar el código de un desarrollador, instalarlo sobre una infraestructura provisionada para pruebas y ejecutar pruebas sobre el sistema para producir un informe de pruebas en tiempo real! INTEGRACION CONTINUA: Tener CONTINUAMENTE en el entorno de INTEGRACION la última versión de un producto sometidaa pruebas automatizadas, para producir un informe de pruebas en tiempo real.

O puedo ir un paso más: Y una vez probado el sistema, ponerlo en manos de la gente (OPERACIONES o usuarios finales) en AUTOMATICO: CONTINUOUS DELIVERY

O puedo ir a más y de paso, instalarlo en AUTOMATICO en el entorno de producción: CONTINUOUS DEPLOYMENT

CI/CD son procesos de muy alto nivel (AMPLIA ENVERGADURA) que orquestan múltiples procesos de más bajo nivel.
Que a su vez orquestan tareas más pequeñas dentro de cada proceso específico.

ESTE ESQUEMA NO PODEMOS PERDERLO DE VISTA.
Ansible y los playbooks que hagamos SERÁN SIMPLEMENTE UN ESLAVON MAS EN LA CADENA!
Quizás hoy en día NO ESTAN EN MI EMPRESA INTEGRADOS DENTRO DE PROCESOS DE MAS ALTO NIVEL, PERO LLEGARÁ EL DIA.

Igual que no puedo automatizar el subir y bajar la persina si no hay motor que lo haga funcionar, NO PUEDO AUTOMATIZAR LOS PROCESOS DE ALTO NIVEL DE LA EMPRESA si no tengo las automatizaciones de nivel inferior.

PERO EL OBJETIVO ULTIMO NO ES TENER UN PLAYBOOK, SINO AUTOMATIZAR LOS PROCESOS DE ALTO NIVEL DE LA EMPRESA.


---

NOTA 1:
- Si monto una oficina, tendré que poner ordenadores a la gente, redes de comunicaciones...
  Evidentemente eso ES PARA RODAR SOFTWARE ENCIMA.. pero muy variopinto.
  No asocio tanto la infra <> software.

  Pero para una determinada aplicación/sistema que voy a tener en la organización (APP DE GESTION DE EXPEDIENTES...) creo una infra adhoc que soporte específicamente esa aplicación. (50 servidores, redes, almacenamiento, etc.)


# Ansible

- Primero: No es una plataforma, ni herramienta...es un ECOSISTEMA que permite la automatización de tareas de configuración de infraestructura.
- Por ahora nos centramos en lo que se llama ansible-core (antiguamente ansible engine).

# Ansible-core

- Un lenguaje IMPERATIVO en el que la definición de las tareas (en su mayor parte) es DECLARATIVA.
- Un lenguaje (realmente 3) para definir INVENTARIOS: hosts, grupos de hosts y variables asociadas.

En general, nos pone más fácil conseguir scripts IDEMPOTENTES!

Lo que quiero es que un script que genere con ansible (PLAYBOOKS) sea capaz de ejecutar las mismas tareas de manera consistente y repetible, sin importar el estado inicial del sistema.

Esos playbooks los definíamos en archivos YAML, eso si, con un esquema propio que define ANSIBLE.

```yaml

- name:             My play 1
  hosts:            all
  pre_tasks:
    - name:         Tarea 1
      notify:       Evento1
    - name:         Tarea 2
  tasks:
    - name:         Tarea 3
      notify:       Evento2
    - name:         Tarea 4
      notify:       Evento2
  post_tasks:
    - name:         Tarea 5
    - name:         Tarea 6
      notify:       Evento3
  handlers:
    - name:         Handler1
      listen:       Evento1
    - name:         Handler2
      listen:       Evento2
    - name:         Handler3
      listen:       Evento3
```
Si todas las tareas al ejecutarlo PROVOCASEN CAMBIO, CUAL SERIA EL ORDEN DE EJECUCION FINAL!
- Tarea 1
- Tarea 2
- Handler1
- Tarea 3
- Tarea 4
- Handler2
- Tarea 5
- Tarea 6
- Handler3

Hay trucos para forzar que los handlers e ejecuten en otro momento... Intentamos evitarlos a toda costa. ROMPLEN EL FLUJO NATURAL DEL PLAYBOOK.
Si es estrictamente necesario lo hacemos (RARO ES QUE LO SEA... normalmente si me hace falta usar estos trucos sucios y rastreros es porque tengo un mal diseño del playbook ESTA MAL ORGANIZADO.) Para gloriosas excepciones:
- Usar `meta: flush_handlers` para forzar la ejecución inmediata de los handlers pendientes.

```yaml

- name:             My play 1
  hosts:            all
  pre_tasks:
    - name:         Tarea 1
      notify:       Evento1
    - name:         Tarea 2
  tasks:
    - name:         Tarea 3
      notify:       Evento2
    - meta:         flush_handlers
    - name:         Tarea 4
      notify:       Evento2
  post_tasks:
    - name:         Tarea 5
    - name:         Tarea 6
      notify:       Evento3
  handlers:
    - name:         Handler1
      listen:       Evento1
    - name:         Handler2
      listen:       Evento2
    - name:         Handler3
      listen:       Evento3
```

# Ejecución de playbooks

Los playbooks van ejecutando tareas.
Y cada tarea que se ejecuta acaba en un estado!

## Estados posibles de las tareas de un playbook

- OK:       La tarea se ha ejecutado sin problema y no ha provocado ningún cambio en el sistema.
- CHANGED:  La tarea se ha ejecutado correctamente y ha provocado un cambio en el sistema.
Los módulos son los que dicen si la tarea ha provocado un cambio en el sistema o no.
- FAILED:   La tarea no se ha ejecutado correctamente y ha provocado un error en el sistema.
- SKIPPED:  La tarea ha sido saltada y no se ha ejecutado en el sistema.

# Cuando se ejecuta un playbook?

---

# JINJA

Es un sistema de plantillas de python.
ANSIBLE HACE USO INTENSIVO de jinja... pero cuidado.. no solo para los ficheitos como el de configuración que acabamos de crear...

TODO LO QUE ESCRIBIMOS EN UN PLAYBOOK SON PLANTILLAS JINJA

---

# Dónde se ejecutan los playbooks de ansible

En ansible hablamos de:
- Entornos remotos:       Cualquier entorno sobre el que vamos a ir ejecutando las tareas de nuestro playbook.
- Entorno local:          El equipo desde el que ejecuto el playbook

En el entorno local, es donde necesito tener instalado ANSIBLE para poder ejecutar los playbooks.
En los entornos remotos lo único que necesito es PYTHON instalado para poder ejecutar los módulos de Ansible.

Cada tarea del playbook abre una conexión con el entorno remoto donde se va a ejecutar.
Cada tarea la suya!
- Si trabajo contra un host remoto linux... normalmente abriré una conexión vía SSH.
- Si trabajo contra un host remoto Windows... normalmente abriré una conexión vía WinRM.

Por defecto, las tareas se ejecutan en las máquinas remotas.
Pero ... hay otra palabrita que tenemos en ansible, que podemos poner en las tareas
```yaml
    - name: Tarea
      delegate_to: localhost  #IP/FQDN del entorno en el que quiero que se ejecute la tarea si no es en el entorno remoto por defecto.
      # localhost sería el entorno local desde el que ejecuto el playbook.

```