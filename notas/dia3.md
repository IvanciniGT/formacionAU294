# Sobre variables

En tiempo de ejecución ANSIBLE TIENE VALORES para las variables.
Esos valores son diferentes para cada servidor donde se ejecuta el playbook.

EN CADA SERVIDOR SE USA UN CONJUNTO DE VALORES INDEPENDIENTE.
Por ejmplo, si estamos generand plantillas (como las jinja) en los servidores, cada ejecución de cada servidor generará una plantilla con los valores que estén definidos para esa ejecución/servidor.

Ahora bien: COMO SE RELLENAN LOS VALORES DE LAS VARIABLES PARA CADA SERVIDOR?

Hay varias opciones a la hora de suministrar esos valores:
- OPCION 1: Tener un fichero de configuración con los valores de las variables. Y se lo proporcionamos a Ansible para que los use durante la ejecución del playbook. EN ESTE CASO, ANSIBLE TOMARIA LOS VALORES DE ESE FICHERO y los INYECTA en CADA SERVIDOR. Todos los servidores tendrían los mismos valores para esas variables.
> $ ansible-playbook -i inventario play-book.yaml -e "@fichero_de_variables.yml"

- OPCION 2: Suministrar valores sueltos vía linea de comandos
> $ ansible-playbook -i inventario play-book.yaml -e "variable1=valor1 variable2=valor2"
Pasa lo mismo... Todos los servidores recibirán los mismos valores para esas variables.

- OPCION 3: Definir variables específicas para cada host en el inventario.
> servidor1 -> variable1=valor1 variable2=valor2
> servidor2 -> variable1=valor3 variable2=valor4

En este caso cada servidor tendrá sus propios valores para las variables, definidos específicamente en el inventario. Esto permite que cada servidor tenga configuraciones independientes sin necesidad de pasar los valores manualmente en cada ejecución del playbook.

Además puedes hacer todo eso variable a variable... es decir:
PAra algunas variables puedo tener un fichero, que aplico a todos los servidores, y para otras variables discretas puedo definir valores específicos para cada servidor en el inventario. Esto me permite combinar ambos enfoques según las necesidades de mi configuración.


## En programación tradicional separamos entre:

- Constantes: Datos que no cambian durante la ejecución del programa. Son valores que se establecen en tiermpo de desarrollo y permanecen fijos.
- Argumentos: Datos que no cambian durante la ejecución del programa, pero que se proporcionan al programa desde el exterior en cada ejecución. Ejecuciones diferentes pueden recibir distintos valores para esos argumentos.
- Variables:  Datos que pueden cambiar durante la ejecución del programa. Son valores que se establecen en tiempo de ejecución y pueden variar según las condiciones o la lógica del programa.

Ansible le llama a todo "variables", independientemente de si son constantes, argumentos o variables en el sentido tradicional de la programación. La diferencia radica en cómo y dónde se definen esos valores y cómo se aplican a cada servidor durante la ejecución del playbook.

# TAGS: ETIQUETAS

Como buena práctica, TODA tarea debe tener etiquetas.
Las etiquetas nos permiten luego, en tiempo de ejecución del playbook, filtrar las tareas que queremos o que NO QUEREMOS ejecutar.

> $ ansible-playbook -i inventario play-book.yaml --tags "etiqueta1,etiqueta2" # Solo quiero las tareas que tengan estas etiquetas
> $ ansible-playbook -i inventario play-book.yaml --skip-tags "etiqueta1,etiqueta2" # Quiero ejecutar todas las tareas excepto las que tengan estas etiquetas

Cuando creo un playbook, ANTES DE EMPEZAR, debería cerrar una colección de etiquetas que voy a utilizar en todas las tareas.
Esa colección la documentar, para que cualquier persona que quiera ejecutar el playbook sepa qué etiquetas existen y cómo utilizarlas correctamente.