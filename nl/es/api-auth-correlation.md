---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Autorizaciones de recursos necesarias para las llamadas de API y CLI
{: #resource-authorizations-required-for-api-and-cli-calls}

En la tabla siguiente se muestran las autorizaciones necesarias para interactuar con los objetos de la infraestructura de {{site.data.keyword.cloud}} Virtual Private Cloud. Esta información resulta particularmente útil para saber cuándo se están realizando llamadas de API. Esto es lo que se necesita saber para utilizar esta tabla:

Los términos _conectado_ o _no conectado_ hacen referencia a si el recurso está asociado a una o varias VPC (ya sea directa o indirectamente a través de las subredes del recurso). Una IP flotante o una ACL de red no conectada no tiene subredes y, por lo tanto, no está asociada a ninguna VPC, por lo que no se aplica la autorización de VPC.

* Las acciones **Ver** y **Listar** corresponden al rol de **Visor**.
* Las acciones **Operar** corresponden al rol **Operador**.
* **Actualizar** y las acciones relacionadas corresponden a los roles de **Editor** o de **Administrador**.
* Los recursos están protegidos por autorización; los objetos de referencia de recursos (ID, CRN, etc.) no lo están.


| Recurso | Operación | Autorización necesaria |
|--------|--------|---------|
| VPC | Crear | Autorización para Ver sobre el grupo de recursos para ese VPC y autorización para Actualizar sobre los recursos de VPC|
| VPC | Actualizar, Suprimir |  Autorización para Actualizar sobre VPC |
| VPC |  Ver, Listar | Autorización para Ver sobre VPC  |
| ACL predeterminada, SG predeterminado de VPC|  Ver, Listar | Autorización para Ver sobre VPC |
| Prefijos de direcciones de VPC |  Crear, Actualizar, Suprimir | Autorización para Actualizar sobre VPC |
| Prefijos de direcciones de VPC |  Ver, Listar | Autorización para Ver sobre VPC  |
|—————|——————|———————|
| IP flotante (no asociada) | Crear, Actualizar, Suprimir | Cualquier usuario de la cuenta y autorización para Ver sobre todos los servicios de gestión de cuentas, si el recurso se crea en el grupo de recursos predeterminado. Pulse [aquí](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources#setting-up-viewer-access) para ver instrucciones sobre cómo configurar el acceso de visor. **Nota: las acciones asociar y eliminar asociación forman parte de la operación de actualización de IP flotante**|
| IP flotante (no asociada) | Ver, Listar | Usuario de la cuenta |
| IP flotante (asociada) | Actualizar | Autorización para Actualizar sobre la VPC de la subred asociada (no puede crear ni suprimir una IP flotante después de que se asocie) |
| IP flotante (asociada) | Ver, Listar | Autorización para Ver sobre la VPC de la subred asociada de la IP flotante | 
|——————|———————|————————|
| ACL de red (no conectada), reglas de ACL | Crear, Actualizar, Suprimir | Cualquier usuario de la cuenta |
| ACL de red (no conectada), reglas de ACL | Ver, Listar | Cualquier usuario de la cuenta |
| ACL de red (conectada), reglas de ACL | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre todas las VPC asociadas a subredes conectadas |
| ACL de red (conectada), reglas de ACL | Ver, Listar | Autorización para Ver sobre al menos 1 de las VPC asociadas a subredes conectadas |
|——————|———————|————————|
| Pasarela pública | Crear, Actualizar, Suprimir |  Autorización para Actualizar sobre la VPC de PGW |
| Pasarela pública | Ver, Listar | Autorización para Ver sobre la VPC de PGW |
|—————————|————————|———————————|
| Geografía | Ver, Listar |  Para regiones y zonas, cualquier usuario de la cuenta |
|———————|————————|—————————|
| Clave SSH | Crear, Actualizar, Suprimir | Autorización para Actualizar para la clave SSH |
| Clave SSH | Ver, Listar | Autorización para Ver sobre la clave SSH |
|————————|—————————|————————|
| Subred | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre la VPC de la subred |
| Subred | Ver, Listar | Autorización para Ver sobre la VPC de la subred |
| ACL o PGW de subred | Conectar, Desconectar | Autorización para Actualizar sobre la VPC de la subred |
| ACL o PGW de subred | Ver, Listar | Autorización para Ver sobre la VPC de la subred |
|——————|—————————|————————|
| Grupo de seguridad | Obtener     | Autorización para Ver sobre el grupo de seguridad.
| Grupo de seguridad | Listar    | Autorización para Ver sobre el grupo de seguridad (o, de lo contrario, el grupo se omite de la respuesta).
| Grupo de seguridad | Crear  | Autorización para Editar sobre el grupo de seguridad que se va a crear (por ejemplo, autorización para Editar sobre todos los grupos de seguridad o autorización para Editar sobre el grupo de recursos donde se va a crear el grupo de seguridad).<br />Autorización para Ver sobre la VPC donde se va a crear el grupo.
| Grupo de seguridad | Actualizar / Suprimir  | Autorización para Editar sobre el grupo de seguridad.
| Regla de grupo de seguridad | Obtener / Listar | Autorización para Ver sobre el grupo de seguridad.
| Regla de grupo de seguridad | Crear / Actualizar / Suprimir | Autorización para Editar sobre el grupo de seguridad.
| Interfaz de red del grupo de seguridad | Obtener     | Autorización para Ver sobre el grupo de seguridad.<br />Autorización para Ver sobre la instancia.
| Interfaz de red del grupo de seguridad | Listar    | Autorización para Ver sobre el grupo de seguridad (o, de lo contrario, se obtiene una respuesta 404).<br />Autorización para Ver sobre la instancia a la que pertenece la interfaz de red (o, de lo contrario, la interfaz se omite en la respuesta).
| Interfaz de red del grupo de seguridad | Conectar / Desconectar | Autorización para Operar sobre el grupo de seguridad.<br />Autorización para Editar sobre la instancia a la que pertenece la interfaz de red.
|—————————|—————————|—————————|
| Imágenes | Ver, Listar  | Cualquier usuario de la cuenta |
|—————————|—————————|—————————|
| Instancias | Crear| Autorización para Actualizar sobre la instancia y el volumen<br />Autorización para Operar sobre la VPC<br />Autorización para Operar sobre el grupo de seguridad si se especifican|
| Instancias | Actualizar, Suprimir | Autorización para Actualizar sobre la instancia |
| Instancias | Ver, Listar  | Autorización para Ver sobre la instancia |
| Acciones de instancia | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre la instancia|
| Acciones de instancia, inicialización, NICs| Ver, Listar  | Autorización para Ver sobre la instancia |
| FIP de instancia | Ver, Listar | Autorización para Ver sobre la instancia y la VPC de la subred asociada |
| FIP de instancia | Asociar | Autorización para Actualizar sobre la instancia<br />Autorización para Operar sobre la VPC de la subred asociada|
| FIP de instancia | Desasociar | Autorización para Actualizar sobre la instancia |
|————————|——————|————————|
| Pasarela de VPN | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre la VPN |
| Pasarela de VPN | Ver, Listar  | Autorización para Ver sobre la VPN |
| Conexiones de pasarela de VPN | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre la VPN |
| Conexiones de pasarela de VPN | Ver, Listar  | Autorización para Ver sobre la VPN |
| ike_policies, ipsec_policies y conexiones de pasarela de VPN | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre la VPN |
| ike_policies, ipsec_policies y conexiones de pasarela de VPN|Ver, Listar  | Autorización para Ver sobre la VPN |
|————————|——————|————————|
| Equilibrador de carga | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre el equilibrador de carga |
| Equilibrador de carga | Ver, Listar  | Autorización para Ver sobre la equilibrador de carga |
| Agrupaciones y escuchas de equilibrador de carga | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre el equilibrador de carga |
| Agrupaciones y escuchas de equilibrador de carga | Ver, Listar  | Autorización para Ver sobre la equilibrador de carga |
|————————|——————|————————|
| Volúmenes | Crear, Actualizar, Suprimir | Autorización para Actualizar sobre el volumen
| Volúmenes | Ver, Listar  | Autorización para Ver sobre el volumen |
| Perfiles de volumen | Ver, Listar  | Cualquier usuario de la cuenta |


