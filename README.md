![República Dominicana](assets/presidencia.svg)

------

####  ¿Que es X-Road?

**X-Road** es un [software de código abierto](https://es.wikipedia.org/wiki/Software_de_código_abierto) que permite a instituciones y organizaciones intercambiar información a través de [Internet](https://es.wikipedia.org/wiki/Internet). X-Road constituye una capa de integración distribuida entre [sistemas de información](https://es.wikipedia.org/wiki/Sistema_de_información), que proporciona un modo estandarizado y seguro de desplegar y utilizar servicios. Este sistema garantiza la [confidencialidad](https://es.wikipedia.org/wiki/Confidencialidad), la [integridad](https://es.wikipedia.org/wiki/Integridad_del_mensaje) y la [interoperabilidad](https://es.wikipedia.org/wiki/Interoperabilidad) entre las partes que intercambian los datos.



#### PRINCIPIOS

- Seguridad, privacidad y protección de la información
- Independencia Tecnológica
- Conectividad entre islas de información
- Registro de transacciones para control y auditoria
- Participación y colaboración de todos los miembros de la red



#### CARACTERISTICAS

- Sin intermediarios, los datos viajan desde la entidad responsable de los datos hacia la entidad autorizada de para recibirlos.
- Facilidad de integración, los servicios que participan en la red pueden estar desarrollados en cualquier lenguaje de programación.
- La comunicación se implementa como llamadas de servicio mediante la tecnología REST (o SOAP, el que prefieras).
- Control local, la entidad responsable de los datos (proveedor de servicios) es la única que puede autorizar o revocar el acceso a los datos.
- Registro de auditoría, todos los mensajes enviados en la red son registrados que esta sellada electrónicamente.
- Sin roles predeterminados, una vez que una entidad se ha unido a la red, está habilitado para actuar como consumidor o proveedor de servicios.



#### COMPONENTES

- ##### **Servidor Central (CS)**
  - Ofrece el ancla de configuración necesaria para agregar los miembros a la red.
  - Mantiene un listado de todas las instituciones miembros de la red y sus servicios.\


- ##### **Servidor de Seguridad (SS)**
  - Controla el acceso a servicios internos
  - Provee acceso a servicios externos
  - Verifica las firmas y sellos electrónicos de cada mensaje
  - Crea túneles VPN-TLS y entrega los mensajes a su destino final
  - Cifra los mensajes salientes y descifra los mensajes entrantes
  - Crea registros sellados electrónicamente por cada transacción 
  - Sella documentos electrónicamente usando la hora nacional ([**TSA**](https://en.wikipedia.org/wiki/Trusted_timestamping#Trusted_(digital)_timestamping))


- ##### **Autoridad Certificadora (CA)**
  - Emite los certificados a las instituciones para que puedan identificarse y firmar los mensajes
  - Verifica la vigencia de los certificados ([**OCSP**](https://es.wikipedia.org/wiki/Online_Certificate_Status_Protocol))
  



#### ¿Deseas iniciar con X-Road?

1. [Terminos y Abreviaciones](manuales/terminos-y-abreviaciones.md)
2. [Instalación del servidor de seguridad.](manuales/instalacion.md)
	1. [Errores comunes durante la instalación](manuales/errores-comunes-instalacion.md)
3. [Configuración inicial del servidor de seguridad.](manuales/configuraciones.md)
	1. [Errores comunes durante la configuración inicial](manuales/errores-comunes-configuracion-inicial.md)
4. [Administración del servidor de seguridad](manuales/administracion-servidor-seguridad.md)





