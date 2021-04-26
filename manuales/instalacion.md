![República Dominicana](../assets/presidencia.svg)

------

#### Instalación

El servidor de seguridad es el encargado de registrar los servicios de la entidad productora del servicio y la encargada de consumir los servicios de toda la red como entidad consumidora (o ambas).

La misma como requisitos mínimos necesita de estas características:

 **Ref** | Detalle                                        | Explicación 
 ------ | --------------------------------------- | ----------------------------------------------------------
 1    | Ubuntu 18.04, x86-64, 3GB RAM, 10GB espacio libre de disco | Requisitos mínimos sin los complementos de "monitorización" y "monitorización de operaciones" ya que con los complementos se requiere un mínimo de 15 GB de RAM..
 2    | <usuario>                                         | Cuenta para la interfaz de usuario
 3    | **Puertos entrantes de la red externa** | Puertos para conexiones entrantes desde la red externa al servidor de seguridad
 &nbsp; | TCP 5500                                | Intercambio de mensajes entre servidores de seguridad
 &nbsp; | TCP 5577                                | Consulta de respuestas OCSP entre servidores de seguridad
 4    | **Puertos salientes a red externa**  | Puertos para conexiones salientes desde el servidor de seguridad a la red externa
 &nbsp; | TCP 5500                                | Intercambio de mensajes entre servidores de seguridad
 &nbsp; | TCP 5577                                | Consulta de respuestas OCSP entre servidores de seguridad
 &nbsp; | TCP 4001                                | Comunicación con el servidor central
 &nbsp; | TCP 80                                  | Descarga de la configuración global del servidor central
 &nbsp; | TCP 80,443                              | Servicios de sellado de tiempo y OCSP comunes
 5    | **Puertos entrantes de la red interna** | Puertos para conexiones entrantes desde la red interna al servidor de seguridad
 &nbsp; | TCP 4000                                | Interfaz de usuario y API REST de gestión (red local). **¡No debe ser accesible desde Internet!**
 &nbsp; | TCP 80, 443                             | Puntos de acceso al sistema de información (en la red local).
 6    | **Puertos salientes a la red interna**  | Puertos para conexiones entrantes desde la red interna al servidor de seguridad
 &nbsp; | TCP 80, 443, *otros*                    | Puertos finales del sistema de información del productor
 &nbsp; | TCP 2080                                | Intercambio de mensajes entre el servidor de seguridad y el servicio de monitoreo de datos operativos (por defecto en localhost)



#### Preparando el sistema operativo

Se necesita agregar el usuario para utilizar en la interfaz grafica del servidor de seguridad de X-Road, el mismo se puede hacer con este comando:
````bash
sudo adduser <usuario>;
````
> __Nota__
> no se recomienda utilizar el usuario __xroad__ ya que puede crear conflictos al momento de instalar la plataforma debido a que es reservado del sistema.



Una vez agregado el usuario entonces se proceden a desinstalar algunos paquetes que por defecto vienen en algunos sistemas de Ubuntu como son **apache2** y **nginx**.

````bash
apt remove --purge apache2 nginx;
````
> **Nota**
> La directiva **--purge** intentara eliminar todas las configuraciones y directorios asociados a dichos servicios



Una vez desinstalados los paquetes por defecto procedemos a instalar algunos paquetes necesarios, pero primero vamos a descargar los paquetes del repositorio oficial y para ello ejecutamos el siguiente comando:

````bash
wget -q --show-progress -r -nH  http://cs.optic.gob.do/paquetes/;
````



Una vez se terminen de descargar los paquetes del repositorio procedemos a instalar las dependencias del software en el sistema operativo:

````bash
apt install -y openjdk-8-jre-headless ca-certificates-java ntp unzip expect net-tools \
                   postgresql postgresql-contrib postgresql-client crudini rlwrap \
                   curl debconf rlwrap rsyslog unzip libmhash2 authbind;
````



Después de que la instalación se haya creado correctamente entonces procedemos a cambiar el hostname de la maquina y su zona horaria con los siguientes comandos:

````bash
timedatectl set-timezone America/Santo_Domingo;
hostnamectl set-hostname <dominio a usar en el servidor de seguridad>;
````
> **Nota**
> El dominio que se utilizara es necesario cambiarlo en el sistema ya que es el mismo que utilizara la configuración del sistema al momento de instalar.



#### Conflictos con nginx-light

Hay un error de dependencias entre nginx y nginx-light (configuraciones especificas de proxy_pass) por eso procederemos con la desinstalación al principio (en caso de que existiera), ahora procedemos a instalar nginx-light correctamente y a iniciar su servicio
````bash
apt install nginx-light -y;
````



Una vez se instale, el servicio iniciara automáticamente entonces procedemos a instalar los paquetes base de xroad y para esto tenemos que situarnos dentro del folder de instalación y correr el siguiente comando:

````bash
dpkg -i xroad-base_6.22.0-1.20210323124414git00d600d.ubuntu18.04_amd64.deb \
        xroad-jetty9_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-signer_6.22.0-1.20210323124414git00d600d.ubuntu18.04_amd64.deb \
        xroad-nginx_6.22.0-1.20210323124414git00d600d.ubuntu18.04_amd64.deb \
        xroad-confclient_6.22.0-1.20210323124414git00d600d.ubuntu18.04_amd64.deb;
````



En este punto es necesario inicializar el servicio de postgresql para que xroad-jetty y xroad-signer empiecen a correr las migraciones con el siguiente comando:

````bash
service postgresql restart;
````



Ahora instalamos el proxy y el monitor de consultas con el siguiente comando:

````bash
dpkg -i xroad-proxy_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-monitor_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-opmonitor_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-addon-opmonitoring_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb;
````

> Nota: Se harán algunas preguntas en el instalador como el nombre de usuario, IP, etc estos deben ser completados para continuar con el siguiente paso.



Luego de su instalación corremos el siguiente comando para instalar algunas dependencias necesarias para el correcto funcionamiento:

````bash
dpkg -i xroad-addon-metaservices_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-addon-messagelog_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-addon-proxymonitor_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-addon-wsdlvalidator_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb;
````



Solo queda instalar el modulo de autologin (opcional) y el servidor de seguridad (obligatorio):

````bash
dpkg -i xroad-securityserver_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb \
        xroad-autologin_6.22.0-1.20210323124414git00d600d.ubuntu18.04_all.deb;
````



Cuando culmine la instalación deberíamos de poder ingresar al dominio asignado para acceder a la interfaz grafica:

[https://dominio:4000/](https://dominio:4000/)