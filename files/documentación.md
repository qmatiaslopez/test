# Obligatorio Febrero 2025 - Taller de servidores Linux 


![LogoORT!](/files/images/logort.png)


|----------------------------------|-----------------------------------| **Sol Varietti - 288481 / Caetano Rodriguez - 241102** |

|----------------------------------|----------------------------------| **Profesor: Enrique Verdes / Fecha de entrega: 25/02/2025** |


----------------


## Introducción

A lo largo de este documento presentaremos el trabajo de fin de curso del taller de Servidores Linux. El objetivo principal de este trabajo es aplicar los conocimientos adquiridos sobre la configuración y automatización de servidores utilizando Ansible. 

Para eso, se empleó la herramienta de VirtualBox para llevar a cabo la creación, gestión y virtualización de las 2 máquinas virtuales. En una de estas máquinas se instaló la versión "Server with GUI" del sistema operativo CentOS Stream 9 y en la otra se utilizó la versión "Minimal" del sistema operativo Ubuntu 24.04.  

El trabajo se basa en varias tareas que abarcan la configuración de inventarios, la ejecución de comandos ad-hoc y el desarrollo de playbook. A modo de aprendizaje se realizó la documentación con formato Markdown; cabe aclarar que, al utilizar Markdown, evitamos tener conflictos con el repositorio GIT.

En este trabajo se utilizó un repositorio de Git debido a la necesidad de facilitar la colaboración entre los integrantes del equipo, ya que todos pueden trabajar en conjunto sin riesgo de sobrescribir o perder información importante.

Por otro lado, actua como un respaldo centralizado, permitiendo que culquier persona pueda replicar el entorno y configurar los servidores de manera automatizada mediante Ansible. Esto garantiza que las implementaciones sean reproducibles, consistentes y fáciles de mantener en el tiempo.


### Estrucutra de comunicación entre maquinas virtuales
A continuación, se describe la estructura utilizada para la conexión entre las máquinas virtuales en este entorno de trabajo.

El escenario cuenta con dos maquinas virtuales que se comunican a través de dos adaptadores de red configurados de la siguiente manera:

- Adaptador 1 (NAT): Proporciona acceso a Internet a ambas máquinas virtuales, permitiendo la descarga de paquetes y actualizaciones necesarias para su configuración.
  
- Adaptador 2 (Solo anfitrión): Establece una red privada entre las máquinas virtuales, facilitando la comunicación directa sin depender de una conexión externa.
Esta configuración permite que las máquinas interactúen entre sí de manera segura, asegurando la conectividad necesaria para la ejecución de Ansible y otras herramientas de administración remota.

![diagrama](/files/images/diagrama.png)



-----




## Tarea 1: Configuración del archivo de inventario 

En esta primera instancia se creará un archivo de inventario en formato INI, que permitirá a Ansible administrar los diferentes servidores agrupándolos en categorías. Se definirán las credenciales y direcciones IP necesarias para acceder a estos servidores.

### Archivo *inventory.ini*

En el siguiente archivo, ubicado dentro de la carpeta Inventories en el home del repositorio obligatorio2025, se definen los servidores administrados por Ansible. Este inventario está estructurado de acuerdo con los requisitos en la letra, creando los grupos Ubuntu y Centos , tomando en cuenta que contienen un host por grupo. Por otro lado, contamos con el grupo Linux, que contiene los subgrupos Ubuntu y Centos, y un grupo webserver, que también cuenta con el servidor Centos.

```python
    [centos]
    centos-srv ansible_host=192.168.56.20

    [ubuntu]
    ubuntu-srv ansible_host=192.168.56.10

    [linux:children]
    centos
    ubuntu

    [webserver]
    centos-srv

    [linux:vars]
    ansible_user=sysadmin
```

Además, el archivo define las variables *ansible_user* y *ansible_host*. Estas son vitales para establecer la conexión remota, dado que cada variable permite especificar el usuario para ejecutar las tareas de Ansible y la ip de cada servidor.

        Esto asegura la correcta comunicación entre el controlador y los nodos administrados???? 

### Prueba de conexión

Se realizo una prueba de conexión con el fin de validar que los servidores son accesibles desde la maquina control (centos-srv). Para eso se utilizó el siguiente comando: 

```bash
    ansible all -i inventory.ini -m ping
```

El cual arrojo el siguiente resultado. 

![pruebadeconexión.t1](/files/images/pruebaconexion.png)



------





## Tarea 2: Ejecutar comandos ad-hoc

Los comandos ad-hoc en Ansible permiten ejecutar comandos, que nos permiten interactuar con los servidores de manera directa y realizar tareas específicas sin necesidad de escribir un playbook completo. Un ejemplo de esto es el comando ejecutado anteriormente para verificar la conexión con los ervidores dentro del inventario.

Dada la consigna de la tarea, se ejecutarán comandos para realizar tareas específicas como verificar el estado del sistema, instalar software y monitorear el uso del almacenamiento. Estas acciones nos permitirán comprobar la conectividad, la correcta instalación de los servicios y la disponibilidad de recursos en los servidores administrados con Ansible.Estos comandos se ejecutarán desde la máquina de control utilizando la línea de comandos de Ansible y permitirán gestionar múltiples servidores simultáneamente sin requerir configuraciones adicionales.

Las acciones solicitadas fueron las siguientes: 


- Verifica el tiempo de actividad (uptime) en todos los servidores:
El comando aplicado ejecuta el módulo `command` para obtener el tiempo de actividad de todos los servidores especificados dentro de *inventory.ini*.


 ```bash
ansible -i inventories/inventory.ini all -m command -a "uptime"
 ```

El resultado arrojó lo siguiente:

![uptimesrv](/files/images/uptimeall.png)




- Instala apache en los servidores web: 
El comando aplicado realiza la instalación del servidor web Apache en los servidores dentro del subgrupo `webserver`. Se utiliza el módulo `dnf` y las opciones `--become` y `--ask-become-pass` para elevar los permisos del usuario y solicitar la contraseña de root.



 ```bash
ansible -i inventories/inventory.ini webserver -m dnf -a "name=httpd state=present" --become --ask-become-pass
 ```



El resultado arrojó lo siguiente:

![websrvdnf](/files/images/httpdserweb.png)

- Recupera el uso de espacio en disco de los servidores Ubuntu:
El comando aplicado ejecuta `df -h` en los servidores `ubuntu`, proporcionando información sobre el uso del disco en un formato legible para humanos.


 ```bash
ansible -i inventories/inventory.ini ubuntu -m command -a "df -hT"
 ```

El resultado arrojó lo siguiente:

![df-hTubutu](/files/images/df-hTubunu.png)

        ELEGIR BIEN CUAL 

![df-hubutu](/files/images/df-hubuntu.png)






------





## Tarea 3: Crear y ejecutar playbook de Ansible





















