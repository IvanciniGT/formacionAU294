# AU294 — Red Hat Enterprise Linux Automation with Ansible

## Datos del curso

| Dato       | Valor                                         |
|------------|-----------------------------------------------|
| Fechas     | Del lunes 19 al viernes 23 de octubre de 2026 |
| Horario    | De 9:00 a 14:00                               |
| Duración   | 25 horas: 5 sesiones de 5 horas               |
| Sistema    | Red Hat Enterprise Linux 10                   |
| Ansible    | Ansible Core 2.16 y Ansible development tools |
| Plataforma | Red Hat Ansible Automation Platform 2.5 y 2.6 |

## A quién va dirigido

- Administradores de sistemas Linux.
- Ingenieros DevOps y SRE.
- Ingenieros de automatización de infraestructura.
- Desarrolladores que se encargan de tareas repetitivas: gestión de la
  configuración, despliegue de aplicaciones, preparación de servidores de
  desarrollo, pruebas o producción e integración en CI/CD.

## Requisitos previos

Conocimientos de administración de Red Hat Enterprise Linux equivalentes a la
certificación RHCSA (EX200): manejo de la terminal, usuarios, permisos,
paquetes, servicios y SSH.

## Objetivos

Al terminar el curso, el alumno será capaz de:

- Instalar y configurar las herramientas de desarrollo de Ansible en VS Code, y
  ajustar la configuración de Ansible.
- Crear y mantener inventarios de equipos gestionados y gestionar las
  conexiones con ellos.
- Automatizar tareas de administración con *playbooks*.
- Escribir *playbooks* eficaces y que escalen.
- Proteger datos sensibles con Ansible Vault.
- Reutilizar código con roles y colecciones de contenido.

## Contenidos

### 1. Introducción a Ansible

- Conceptos fundamentales de Ansible y para qué se usa.
- Instalación y configuración de las herramientas de desarrollo en VS Code.
- Configuración de Ansible.

### 2. Desarrollo de contenido de automatización

- Construcción de inventarios.
- Escritura y ejecución de *playbooks* sencillos y complejos.
- Resolución de problemas en los *playbooks* y fallos en los equipos
  gestionados.

### 3. Desarrollo de contenido de automatización: variables

- Variables para simplificar la gestión de los *playbooks*.
- Protección de datos sensibles en variables.
- *Facts* y variables mágicas para consultar la información de los equipos
  gestionados.

### 4. Desarrollo de contenido de automatización: control de tareas

- Ejecutar una tarea una vez por cada elemento de una lista.
- Ejecutar tareas sólo cuando se cumplen determinadas condiciones.

### 5. Desarrollo de contenido de automatización: despliegue de ficheros

- Desplegar ficheros en los equipos gestionados.
- Personalizar y adaptar esos ficheros.

### 6. Desarrollo de contenido de automatización a escala

- Importar o incluir otros *playbooks* y tareas.
- Patrones de equipos avanzados para seleccionar equipos concretos de un
  inventario.

### 7. Reutilización de código con roles y colecciones de contenido

- Roles y colecciones para desarrollar *playbooks* más rápido.
- Reutilización de código de Ansible.

### 8. Automatización de tareas de administración de Linux

- Automatizar con Ansible las tareas habituales de administración de un
  sistema Linux.

## Reparto orientativo por días

El temario oficial está pensado para cuatro días. Aquí tenemos cinco sesiones
de cinco horas, así que el reparto es una guía y se ajusta al ritmo del grupo.

| Día | Fecha        | Contenidos                                                                             |
|-----|--------------|----------------------------------------------------------------------------------------|
| 1   | Lunes 19     | Bienvenida y laboratorio. Capítulo 1 y comienzo del 2: inventarios y primer *playbook* |
| 2   | Martes 20    | Capítulo 2 y capítulo 3: variables, *facts* y Ansible Vault                            |
| 3   | Miércoles 21 | Capítulos 4 y 5: bucles, condicionales y plantillas                                    |
| 4   | Jueves 22    | Capítulos 6 y 7: *playbooks* a escala, roles y colecciones                             |
| 5   | Viernes 23   | Capítulo 8 y un ejercicio final que junta todo lo anterior                             |

## Diferencias con el laboratorio oficial

El contenido es el del curso oficial, pero el laboratorio no es el entorno
*online* de Red Hat:

- Las máquinas gestionadas son **Rocky Linux** (compatible con RHEL) y
  **Ubuntu**. Así se practica de verdad el manejo de distribuciones distintas
  con un mismo *playbook*.
- Los *playbooks* se ejecutan desde **AWX**, que es el proyecto de código abierto
  del que sale el *automation controller* de Ansible Automation Platform.

Los detalles están en el [README](../README.md) del repositorio.
