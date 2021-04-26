![República Dominicana](../assets/presidencia.svg)

------

### Términos y abreviaciones

> Estos son los términos y abreviaciones comúnmente utilizados dentro de toda la red de X-Road, para un claro entendimiento del escenario es imprescindible entender estos conceptos básicos.



#### **CA - Autoridad Certificadora**

La autoridad certificadora (CA) emite certificados para los servidores de seguridad (certificados de autenticación) y las organizaciones miembros  (certificados de firma). Los certificados de autenticación se utilizan para proteger la conexión entre dos servidores de seguridad. Los certificados de firma se utilizan para firmar digitalmente los mensajes enviados por los miembros de la red y solo se pueden utilizar los certificados emitidos por autoridades de certificación de confianza que están definidas en el servidor central.

Los servidores de seguridad verifican la validez de los certificados de firma y autenticación a través del Protocolo de estado de certificados en línea (OCSP). Cada servidor de seguridad es responsable de consultar la información de validez de sus certificados y luego compartir la información con otros servidores de seguridad como parte del proceso de intercambio de mensajes. Solo los servidores de seguridad con certificados de autenticación y firma válidos pueden intercambiar mensajes con otros miembros.



#### **TSA - Autoridad de Sellado de Tiempo**

Todos los mensajes enviados a través de la red tienen una marca de tiempo y el servidor de seguridad los registra. El propósito del sellado de tiempo es certificar la existencia de elementos de datos en un momento determinado. La TSA proporciona un servicio de sellado de tiempo que los servidores de seguridad utilizan para marcar el tiempo de todas las solicitudes/respuestas entrantes/salientes. Solo se pueden utilizar las TSA de confianza definidas en el servidor de configuraciones central.

La autoridad de sellado de tiempo debe implementar el protocolo de sellado de tiempo compatible con la red ya que el sistema utiliza un sellado de tiempo por lotes, lo que reduce la carga del servicio de sellado de tiempo. La carga no depende de la cantidad de mensajes intercambiados por los servidores de seguridad. En cambio, depende de la cantidad de servidores de seguridad en el sistema.



#### **CS - Servidor de Configuraciones Central**

Los servicios centrales constan de servidor central y proxy de configuración. El servidor central contiene el registro de los miembros de la red y sus servidores de seguridad. Además, el servidor central contiene la política de seguridad de la instancia de toda la red que incluye una lista de autoridades de certificación confiables (CA), una lista de autoridades confiables de sellado de tiempo (TSA) y parámetros de configuración. Tanto el registro de miembros como la política de seguridad están disponibles para los servidores de seguridad a través del protocolo HTTP. Este conjunto distribuido de datos forma la configuración global que utilizan los servidores de seguridad para mediar en los mensajes enviados a través de X-Rola redad.



#### **SS - Servidores de Seguridad**

Los servidores de seguridad son el punto de entrada a toda la red y es necesario tanto para producir como para consumir servicios a través de X-Road. Los servidores de seguridad encapsulan los aspectos de seguridad de la infraestructura de toda la red

- *administrar claves para la firma y autenticación*
- *enviar mensajes a través de un canal seguro*
- *crear el valor de prueba para los mensajes con firmas digitales*
- *sellado de tiempo y registro*

Para el sistema de información de un consumidor de servicios y un proveedor de servicios, Security Server ofrece un protocolo de mensajes basado en REST y otro basado en SOAP. El protocolo es el mismo para el cliente y el proveedor de servicios, lo que hace que Security Server sea transparente para las aplicaciones.



#### CSR - Solicitud de firma de Certificado

Es un mensaje enviado por un solicitante a una autoridad de registro de la infraestructura de clave pública para solicitar un certificado de identidad digital. Por lo general, contiene la clave pública para la cual se debe emitir el certificado, información de identificación (como un nombre de dominio) y protección de la integridad (por ejemplo, una firma digital).

Se genera en el servidor de seguridad para una determinada autoridad de certificación aprobada para firmar una clave pública e información asociada.



#### OCSP - Protocolo de estado de certificado en línea

Es un método para determinar el estado de vigencia de un certificado digital X. 509 usando otros medios que no sean el uso de CRL (Listas de Revocación de Certificados).



#### Subsistemas

Representa una parte del sistema de información de un miembro de la red Los miembros de X-Road deben declarar partes de su sistema de información como subsistemas para usar o proporcionar los servicios de X-Road.

- Los derechos de acceso de los subsistemas de los miembros de X-Road son independientes: los derechos de acceso otorgados a un subsistema no afectan los derechos de acceso de los otros subsistemas de los miembros.
- Los servicios proporcionados por un subsistema son independientes de los servicios proporcionados por los otros subsistemas de los miembros.
- Para firmar los mensajes enviados por un subsistema al utilizar o prestar los servicios de X-Road, se utiliza el certificado de firma del miembro que gestiona el subsistema. Un miembro de X-Road puede asociar varios subsistemas diferentes con un servidor de seguridad, y un subsistema puede asociarse con varios servidores de seguridad.