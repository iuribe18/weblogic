# Ansible Role Para Weblogic 14 e ICBS
Automatización de la ambientación para ICBS en Weblogic 14 con Java 8

### Prerequisitos maquina que ejecutará el playbook
Tener instalado previamente lo siguiente paquetes:

* Python
* Ansible 2.9
* jmespath

### Prerequisitos maquinas a ser configuradas (hosts)

Los hosts, en el usuario root deben tener la llave ssh publica de la maquina que ejecuta el playbook, si es que se ejecuta desde el rol `commons`. ($ ssh-copy-id root@10.0.0.X)

En caso que no se necesite ejecutar `commons`, entonces el usuario oracle debe tener la llave ssh publica de la maquina que ejecuta el playbook.

### Preparación
Se deben definir los servidores en los cuales se instalará el dominio en el archivo hosts de la siguiente manera

```
[qa_supp]
adminserver ansible_host=10.0.0.4
server1 ansible_host=10.0.0.9
server2 ansible_host=10.0.0.49
serverX ansible_host=10.0.0.x
```

El nombre del grupo no es relevante para el nombre del dominio, pero debe coincidir con el archivo yml que está dentro de group_vars con la variable server que se le entrega al comando ansible-playbook.
group_vars/qa_supp.yml
El archivo yml dentro de group_vars contiene la definición del dominio, en este se pueden añadir varios clusters, cada uno con sus recursos propios de datasources, jmsmodules, colas, saf, etc...

Luego están los archivos yml dentro del directorio host_vars, es importante que estos existan para cada host definido en el inventario con el nombre de archivo correspondiente a cada entrada, en estos archivos se definen el nombre de la machine asociada al host, el nombre del server, el jmsserver entre otras cosas.
Ejemplo host_vars/server1.yml

Notas:

Actualmente los archivos e instaladores de java y weblogic se descargan desde el servidor http://10.0.0.14:7000/downloads/

De no tenerse se debe configurar la ruta local donde estará almacenado el instalador. En el script  tiene una ubicación web predeterminada donde esta el instalador:
http://10.0.0.14:7000/downloads/fmw_14.1.1.0.0_wls_lite_generic.jar
Se debe ajustar esta ruta de instalación por la ruta local donde se ubique el ejecutable

Se da por entendido que los servidores donde se ejecutaran los procesos, no tienen instalaciones anteriores de weblogic y se encuentran limpios para este proceso.

##### Propiedades
Los archivos properties se deben descargar en la ruta roles/icbs-domain-environment/files/
Ver archivo leeme de dicha ruta:
 roles/icbs-domain-environment/files/README.md 
En esta ruta, se requieren los siguientes directorios de propiedades
- antiphishing-images-files 
- icbs-fm-pgp 
- icbs-properties

## Modo de ejecución
ansible-playbook -e server="qa_supp" weblogic-12c.yml -v