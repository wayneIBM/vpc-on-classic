---

copyright:

  years: 2018, 2019
lastupdated: "2019-06-11"

keywords: error, message, API, limitations, rias, support

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# Mensajes de error de la API de IBM Cloud Virtual Private Cloud
{: #rias-error-messages}

Cuando reciba un mensaje de error de las API de nube privada virtual de {{site.data.keyword.cloud}}, puede utilizar el ID del mensaje para encontrar más información sobre cómo resolver el problema.
{:shortdesc}

## account_type_invalid
**Mensaje**: Debe tener una cuenta de Pago según uso para suministrar una nube privada virtual.

Su cuenta debe tener un plan de Pago según uso para suministrar VPC. Puede encontrar más detalles sobre la actualización de su cuenta aquí: [Actualizar la cuenta](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**Mensaje**: La ACL de red no se puede suprimir porque está conectada a recursos.

La ACL de red no se puede suprimir porque está conectada a una subred o una VPC.

Para ver si una subred está utilizando la ACL de red, utilice la API `GET /v1/subnets?version=2019-05-31&generation=1`.  Mandato de CLI equivalente: `ibmcloud is subnets`.  Para actualizar la ACL de red utilizada por una subred, utilice la API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` o la CLI `ibmcloud is subnet-update`.

Para ver si una VPC está utilizando la ACL de red, utilice la API `GET /v1/vpcs?version=2019-05-31&generation=1`.  Mandato de CLI equivalente: `ibmcloud is vpcs`. No es posible actualizar o suprimir la ACL de red predeterminada utilizada por una VPC. La ACL de red predeterminada se suprime automáticamente cuando se suprime la VPC.

## address_prefix_conflict
**Mensaje**: El prefijo de dirección con este CIDR se está utilizando.

El CIDR correspondiente al prefijo de dirección solicitado está en conflicto con un prefijo de dirección existente.

Para ver la lista de prefijos de dirección de una VPC, utilice la API `GET /vpcs/{vpc-id}/address_prefixes` y compruebe que la dirección CIDR no se está utilizando en el campo `cidr` de otro prefijo de dirección.
Mandato de CLI equivalente: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**Mensaje**: No se puede suprimir un prefijo de dirección en uso.

Una o varias subredes están utilizando el prefijo de dirección.  Para determinar las subredes que están utilizando el prefijo de dirección, utilice el mandato de la API `GET /v1/subnets?version=2019-05-31&generation=1` y compruebe el campo `ipv4_cidr_block`.  Mandato de CLI equivalente:  `ibmcloud is subnets` y compruebe la columna `Subnet CIDR`. Debe suprimir todas las subredes que utilizan el prefijo de dirección para poder suprimir el prefijo de dirección.

## backend_service_unavailable
**Mensaje**: El servicio de fondo no está disponible.

Un servicio en la nube de fondo que utiliza la VPC no ha respondido. Una VPC utiliza varios servicios de IBM Cloud, como por ejemplo:

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Catálogo global](https://{DomainName}/catalog)
- Controlador de recursos
- Gestor de recursos

Puede comprobar el [estado](https://{DomainName}/status) de los servicios de IBM Cloud e intentarlo de nuevo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_field
**Mensaje**: Se debe proporcionar el UUID de instancia correcto

Uno de los valores que se proporcionan en la solicitud es incorrecto. Compruebe el valor de `target` (destino) en el error devuelto para ver las pistas en las que el parámetro era incorrecto. En algunos casos, no se puede encontrar el UUID ni el volumen de la solicitud. Proporcione un valor válido y vuelva a intentarlo.

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obtener ayuda adicional. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_request
**Mensaje**: La información proporcionada no es válida, está mal formada o falta un campo necesario.

La solicitud no era lo que esperábamos. Utilice la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para ayudarle a formatear la solicitud.

Si está siguiendo la especificación pero sigue obteniendo error, [póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Mensaje**: Sólo se puede crear una VPC con acceso clásico por región.

Sólo se puede crear una VPC con acceso clásico por región. Para listar la VPC con el acceso clásico, ejecute `ibmcloud is vpcs --classic-access`. Se debe suprimir la VPC con acceso clásico existente para poder crear otra con acceso clásico.

## classic_access_vpc_account_not_VRF_enabled
**Mensaje**: La cuenta debe estar habilitada para VRF para crear una VPC de acceso clásico.

La cuenta clásica enlazada no está habilitada para VRF. Una VPC de acceso clásico requiere que la cuenta clásica enlazada esté habilitada para VRF. Para habilitar la cuenta para VRF, consulte [VRF en IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_found
**Mensaje**: No se ha encontrado el prefijo de dirección predeterminado.

Es posible que vea este mensaje de error cuando no se encuentre el prefijo de dirección predeterminado.

## duplicate_error
**Mensaje**: La entrada proporcionada ya existe.

El recurso especificado ya existe. Pruebe a utilizar otro nombre para el recurso que desea crear. Por ejemplo, al crear una nueva VPC, puede listar las VPC existentes ejecutando `ibmcloud is vpcs`.  Elija un nombre que no entre en conflicto.

## floating_ip_in_use
**Mensaje**: La IP flotante se está utilizando.

Esta IP flotante está asociada a una instancia de servidor virtual o pasarela pública.

Para ver dónde se utiliza la IP flotante, utilice la API `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` y consulte los valores de "target" (destino). Mandato de CLI equivalente: `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_many
**Mensaje**: Solo se puede tener una pasarela pública por zona en una VPC.

Solo se permite una pasarela pública por zona en una VPC, pero la pasarela pública se puede conectar a varias subredes en la zona. Para encontrar la pasarela pública para una zona, ejecute la API GET public_gateways y examine los valores "vpc" y "zone". Si utiliza la CLI, ejecute el mandato `ibmcloud is public-gateways` y examine el valor de "VPC" y "Zone".

Utilice el ID de la pasarela pública para conectarla a una subred; encontrará un ejemplo en nuestros [ejemplos de API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-). Si utiliza la CLI, utilice el mandato de actualización de subred para conectarla a una pasarela pública, como en este ejemplo: `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
**Mensaje**: El retardo de supervisor de estado especificado <health_monitor_delay> no es válido

Especifique un valor del 2 al 60 para el parámetro `health monitor delay`.

## health_monitor_invalid_max_retries
**Mensaje**: El valor de número máximo de reintentos de supervisor de estado especificado <health_monitor_max_retries> no es válido

Especifique un valor del 1 al 10 para `health max retries`.

## health_monitor_invalid_timeout
**Mensaje**:  El valor de tiempo de espera de supervisor de estado especificado  <health_monitor_timeout> no es válido.

Especifique un valor del 1 al 59 para `health timeout`. El valor seleccionado debe ser inferior al valor de `health monitor delay`.

## health_monitor_invalid_url_path
**Mensaje**: La vía de acceso de URL de estado especificada <health_monitor_url> no es válida.

Especifique un URL válido del supervisor de estado. El URL puede contener caracteres alfanuméricos, guiones y puntos. No se permiten espacios ni barras inclinadas invertidas. La longitud máxima de la vía de acceso del URL es de 255 caracteres.

## health_monitor_invalid_protocol
**Mensaje**: El protocolo del supervisor de estado especificado <health_monitor_protocol> no es válido.

Especifique un valor válido para el protocolo del supervisor de estado. Los valores aceptados son `http` y `tcp`.

## health_monitor_missing_protocol
**Mensaje**: Falta el protocolo del supervisor de estado

El protocolo del supervisor de estado es un campo obligatorio. En la solicitud, especifique el protocolo del monitor de estado. Los valores válidos son `http` y `tcp`.

## health_monitor_missing_url_path
**Mensaje**: Falta la vía de acceso del URL del supervisor de estado. Es obligatoria para un supervisor de estado de tipo HTTP.

Para un monitor de estado que utilice el protocolo HTTP, el campo de vía de acceso del URL del supervisor de estado es obligatorio. Especifique una vía de acceso de URL válida.

## health_monitor_timeout_delay_conflict
**Mensaje**:  El tiempo de espera del supervisor de estado <health_monitor_timeout> no puede ser mayor ni igual que el retardo <health_monitor_delay>.

El valor del parámetro `health monitor delay` debe ser mayor que el valor de `health monitor timeout`.

## http_request_size_exceeded
**Mensaje**: La solicitud HTTP es demasiado larga.

Este problema se produce cuando la carga útil que ha enviado en la solicitud tiene demasiados caracteres. Inténtelo de nuevo con una carga útil más corta. Por ejemplo, en lugar de intentar hacer todo en una sola solicitud, intente crear un recurso mínimo en una solicitud y luego añada el estado al mismo de forma incremental en varias solicitudes posteriores.

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obtener ayuda adicional. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
**Mensaje**: Ninguno

Se ha producido un error en el servicio IAM, verificando la autenticación o la autorización. La interrupción del servicio IAM puede ser temporal. Vuelva a intentar la solicitud transcurridos unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
**Mensaje**: No se puede crear la política de IKE porque su cuenta ha alcanzado su cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las políticas de IKE actuales, utilice la API `GET /ike_policies`.
Mandato de CLI equivalente: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**Mensaje**: El nombre `<ike_policy_name>` ya está siendo utilizado por la política de IKE `<ike_policy_id>`.

Especifique un nombre de política de IKE distinto. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
**Mensaje**: La política de IKE `<ike_policy_id>` ya está siendo utilizada por una o varias conexiones.

Una política de IKE no se puede suprimir si hay una o varias conexiones que la están utilizando.

Para ver las conexiones que utilizan la política de IKE, utilice la API `GET /ike_policies/<ike_policy_id>/connections`.
Mandato de CLI equivalente: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**Mensaje**: El nombre `<ike_policy_name>` no es un nombre de política de IKE válido.

Un nombre de política de IKE válido comienza por una letra, seguida de letras, dígitos, signos de subrayado o guiones.

## ike_policy_not_created
**Mensaje**: No se ha podido crear la política de IKE `<ike_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
**Mensaje**: No se ha podido suprimir la política de IKE `<ike_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_found
**Mensaje**: No se ha podido encontrar la política de IKE `<ike_policy_id>`.

Ha hecho referencia a una política de IKE que no existe. Revise su solicitud para asegurarse de que ha especificado el ID de política de IKE adecuado. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
**Mensaje**: No se ha podido actualizar la política de IKE `<ike_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
**Mensaje**: No se puede suprimir la instancia en el estado actual.

No se puede suprimir una instancia si está en estado `deleting`, `pending`, `starting`, `stopping` o `restarting`. La instancia debe estar en estado `running` o `stopped`. Si el estado no cambia, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
**Mensaje**: No se puede crear la instancia porque el nombre de host no es válido.

El nombre de host de la instancia debe cumplir los requisitos siguientes:
* El nombre de host sólo puede contener caracteres alfanuméricos y guiones
* Los caracteres en mayúsculas no están permitidos
* No se permiten guiones iniciales ni finales
* No se permiten números iniciales
* No se permiten guiones consecutivos
* La longitud máxima es de 15 caracteres para Windows
* La longitud máxima es de 40 caracteres para otros sistemas operativos

## instance_invalid_port_speed
**Mensaje**: No se puede crear la instancia porque la velocidad de puerto de interfaz de red especificada no es válido.

La velocidad de puerto de interfaz de red debe ser `100` o `1000` Mbps.

## instance_name_exists
**Mensaje**: Ya existe este mismo nombre de instancia.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
**Mensaje**: No se puede crear la instancia porque el número de conexiones de volumen superaría la cuota.

Las cuotas por recurso de una VPC se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}.

## instance_too_many_keys
**Mensaje**: No se puede crear la instancia de Windows porque la solicitud contiene varias claves.

Las instancias de Windows sólo admiten una clave.

## insufficient_space_for_subnet
**Mensaje**: No hay suficiente espacio para la subred en el prefijo de dirección.

No se puede crear la subred porque no se puede asignar el número de direcciones solicitadas. Vuelva a intentarlo utilizando un número de direcciones más pequeño o un CIDR más grande.

## internal_error
**Mensaje**: Se ha producido un error interno.

Se ha producido un error inesperado. Este problema puede ser temporal. Vuelva a intentar la solicitud transcurridos unos minutos. Si este error continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
**Mensaje**: Ninguno

Se ha producido un error inesperado. Este problema puede ser temporal. Vuelva a intentar la solicitud transcurridos unos minutos. Si este error continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
**Mensaje**: Póngase en contacto con el administrador.

Se ha producido un error inesperado. Este problema puede ser temporal. Vuelva a intentar la solicitud transcurridos unos minutos. Si este error continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_generation_parameter
**Mensaje**: El parámetro de consulta de generación debe establecerse en 1.

Para las versiones a partir del 31/5/2019, el parámetro de consulta `generation` se debe establecer en 1 para permitir solicitudes de API de VPC on Classic.

## invalid_id_format
**Mensaje**: Formato de ID incorrecto. Asegúrese de que el formato sea correcto.

Asegúrese de que el ID que ha proporcionado no contiene ningún dato mal formado.

Puede que obtenga este mensaje de error si realiza una consulta de inicio mal formada al hacer una solicitud de paginación. Por ejemplo,
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` contiene dos signos de interrogación (`?`). Corrija la consulta y vuelva a intentarlo.

## invalid_route
**Mensaje**: La ruta solicitada no existe.

La ruta solicitada en el URL de API que ha proporcionado no existe. Verifique que el URL que ha especificado para llamar al punto final de API sea correcto.

## invalid_request_field
**Mensaje**: Un campo especificado en la solicitud no es válido.

Por ejemplo, para actualizar la ACL de red utilizada por una subred, utilice la API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’`.
La siguiente solicitud no sería válida porque “networkacl” no es un campo válido, `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para conocer los valores aceptados.

## invalid_state
**Mensaje**: Se ha solicitado una acción en un recurso que no está soportada en el estado actual del recurso.

Si el recurso es una instancia:

1. Es posible que ya haya una operación de rearranque en curso para la instancia. Consulte [acciones permitidas](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance), en función del estado de la instancia.

2. Mientras el estado de la instancia sea `pending`, no puede realizar las acciones siguientes:
  * suprimir la instancia
  * conectar un volumen a la instancia
  * desconectar un volumen de la instancia
  * actualizar la instancia

Vuelva a intentar la acción más tarde. Si el estado del recurso no cambia, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

Si el recurso es una imagen:

Mientras el estado de la instancia sea `deleting`, no puede realizar las acciones siguientes:
  * aplicar un parche a la imagen
  * suprimir la imagen

No vuelva a hacer la solicitud. La supresión de la imagen se completará en su momento. Una vez que se haya completado la supresión, todas las solicitudes sobre esa imagen fallarán con el error `not_found`.

## invalid_version
**Mensaje**: El parámetro `version` no es válido, debe tener el formato `AAAA-MM-DD`.

La versión debe ajustarse al formato _AAAA-MM-DD_. En el caso de meses o fechas de un solo dígito, como el 1 de enero, la versión debe tener este formato: `2019-01-01`. El parámetro de versión debe aparecer en el URL. La fecha especificada en el parámetro de versión debe ser posterior al 1 de enero de 2019, pero anterior a la fecha actual.

## invalid_version_range
**Mensaje**: El valor de `version` no se puede establecer en una fecha futura ni anterior a `2019-01-01`.

La fecha especificada en el parámetro de versión debe ser posterior al 1 de enero de 2019, pero anterior a la fecha actual.

## invalid_zone
**Mensaje**: Compruebe si los recursos que está solicitando se encuentran en la misma zona.

Puede que vea este mensaje al intentar conectar una pasarela pública en la zona-1 a una subred en la zona-2.

## ipsec_policies_quota_exceeded
**Mensaje**: No se puede crear la política de IPsec porque su cuenta ha alcanzado su cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las políticas de IPsec actuales, utilice la API `GET /ipsec_policies`.
Mandato de CLI equivalente: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**Mensaje**: El nombre `<ipsec_policy_name>` ya está siendo utilizado por la política de IPsec `<ipsec_policy_id>`.

Especifique un nombre de política de IPsec distinto. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
**Mensaje**: La política de IPsec `<ipsec_policy_id>` ya está siendo utilizada por una o varias conexiones.

Una política de IPsec no se puede suprimir si hay una o varias conexiones que la están utilizando.

Para ver las conexiones que utilizan la política de IPsec, utilice la API `GET /ipsec_policies/<ipsec_policy_id>/connections`.
Mandato de CLI equivalente: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**Mensaje**: El nombre `<ipsec_policy_name>` no es un nombre de política de IPsec válido.

Un nombre de política de IPsec válido comienza por una letra, seguida de letras, dígitos, signos de subrayado o guiones.

## ipsec_policy_not_created
**Mensaje**: No se ha podido crear la política de IPsec `<ipsec_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
**Mensaje**: No se ha podido suprimir la política de IPsec `<ipsec_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_found
**Mensaje**: No se ha podido encontrar la política de IPsec `<ipsec_policy_id>`.

Ha hecho referencia a una política de IPsec que no existe. Revise su solicitud y especifique un ID de política de IPsec válido. Si está seguro de que la política existe, [póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
**Mensaje**: No se ha podido actualizar la política de IPsec `<ipsec_policy_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
**Mensaje**: Este contenido de clave ya existe.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
**Mensaje**: Ya existe este mismo nombre de clave.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
**Mensaje**: No se puede crear la clave porque el nombre no es válido.

El nombre de clave debe cumplir los requisitos siguientes:
* debe empezar por un carácter alfabético, [ A-Za-z]
* debe contener sólo caracteres alfanuméricos, guiones o guiones de subrayado, [-A-Za-z0-9_]
* la longitud no puede superar los 65 caracteres

## key_invalid_type
**Mensaje**: No se puede crear la clave porque el tipo no es válido.

El único tipo de clave soportado es `rsa`.

## key_parse_failure
**Mensaje**: No se puede crear la clave porque el valor de clave pública no es válido.

La clave pública proporcionada en la solicitud no es válida. Compruebe el valor o vuelva a generar la clave pública e inténtelo de nuevo.

En los sistemas operativos Linux, puede ejecutar el mandato siguiente para validar la clave pública `ssh-keygen -l -v -f <key_file>`.

## listener_certificate_not_found
**Mensaje**: No se puede encontrar la instancia de certificado con el CRN `<listener_certificate_crn>` o no se dispone de permiso para acceder a la instancia de certificado.

Especifique un CRN de instancia de certificado existente o solicite al administrador de la cuenta que le otorgue permiso de acceso a la instancia de certificado proporcionada.

## listener_duplicate_port
**Mensaje**: El puerto `<listener_port>` está siendo utilizado por otra instancia de escucha. Elija otro puerto.

Elija otro puerto.

## listener_invalid_certificate
**Mensaje**: El CRN de la instancia de certificado para el escucha HTTPS no es válido.

Proporcione un CRN de instancia de certificado válido.

## listener_invalid_connection_limit
**Mensaje**: El límite de conexiones de escucha `<listener_connection_limit>` no es válido.

Especifique un límite de conexiones válido. El límite de conexiones debe ser un entero de 1 al 15000.

## listener_invalid_port
**Mensaje**: El puerto de escucha `<listener_port>` no es válido.

Elija otro puerto.

## listener_invalid_protocol
**Mensaje**: El protocolo de escucha `<listener_protocol>` no es válido.

Especifique un protocolo de escucha válido. Los valores válidos para el protocolo de escucha son `http`, `https` y `tcp`.

## listener_missing_certificate_crn
**Mensaje**: Falta el CRN de la instancia de certificado para el escucha `https`.

El CRN de la instancia de certificado es un campo obligatorio.

## listener_missing_port
**Mensaje**: Falta el puerto de escucha.

El puerto de escucha es un campo obligatorio.

## listener_missing_protocol
**Mensaje**: Falta el protocolo de escucha.

El protocolo de escucha es un campo obligatorio. Especifique el protocolo de escucha en la solicitud. Los valores válidos son
`http`, `https` y `tcp`.

## listener_not_found
**Mensaje**: No se puede encontrar el escucha con el ID `<listener_id>`.

Especifique un ID de escucha existente.

## listener_over_quota
**Mensaje**: No se puede crear el escucha. La cuota de escuchas para el recurso del equilibrador de carga ha alcanzado el límite máximo.

Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## listener_pool_protocols_conflict
**Mensaje**: El protocolo de escucha (`<listener_protocol>`) y el protocolo de agrupación (`<pool_protocol>`) entran en conflicto.

Un escucha con el protocolo `https` o `http` solo se puede asociar con una agrupación con el protocolo `http`. Paralelamente, un escucha `tcp` solo se puede asociar a una agrupación `tcp`.

## listener_reserved_port_conflict
**Mensaje**: El puerto de escucha `<listener_port>` es uno de los puertos reservados. El rango de puertos 56500 a 56520 está reservado para fines de gestión y no se puede utilizar. Elija otro puerto.

Elija otro puerto.

## load_balancer_delete_conflict
**Mensaje**: El equilibrador de carga con el ID <load_balancer_id> o se puede suprimir porque su estado es uno de siguientes:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Suprima el equilibrador de carga cuando esté en estado ACTIVE.

## load_balancer_duplicate_name
**Mensaje**: El nombre `<load_balancer_name>` está siendo utilizado por otra instancia de equilibrador de carga. Elija otro nombre.

Especifique otro nombre para la instancia del equilibrador de carga. El nombre del equilibrador de carga debe ser exclusivo dentro de la VPC y de la cuenta.

El equilibrador de carga de VPC no entra en conflicto con el equilibrador de carga de nube, porque los nombres de dominio DNS son diferentes para estos servicios, y el nombre del equilibrador de carga de VPC no está incluido en el nombre DNS.
{: note}

## load_balancer_insufficient_ips
**Mensaje**: La subred con el o los ID <subnet_id> no tiene suficientes direcciones IPv4 disponibles.

En la solicitud, proporcione una subred válida con direcciones IPv4 disponibles, en las que desee crear el equilibrador de carga.

## load_balancer_invalid_description
**Mensaje**: La descripción del equilibrador de carga `<load_balancer_description>` no es válida.

Una descripción de equilibrador de carga no puede superar los 255 caracteres.

## load_balancer_invalid_is_public
**Mensaje**: El valor de is_public `<load_balancer_is_public>` no es válido.

El valor del parámetro de equilibrador de carga `is_public` debe ser _true_ o _false_.

## load_balancer_invalid_name
**Mensaje**: El nombre `<load_balancer_name>` no es válido.

Los nombres pueden no estar vacíos. Un nombre de equilibrador de carga válido empieza por una letra, seguida de letras, dígitos o caracteres de subrayado. La longitud del nombre no puede superar los 40 caracteres.

## load_balancer_invalid_subnet
**Mensaje**: La subred con el ID <subnet_id> no es válida. Asegúrese de utilizar una subred existente con el estado 'available'.

Proporcione subredes válidas que estén en estado `available` al entrar la solicitud para crear un equilibrador de carga.

## load_balancer_missing_is_public
**Mensaje**: Falta el campo 'is_public'.

El campo `is_public` es obligatorio. Especifique un valor para el campo `is_public`.

## load_balancer_missing_name
**Mensaje**: Falta el nombre del equilibrador de carga.

El campo `name` del equilibrador de carga es obligatorio. Especifique un nombre para el equilibrador de carga.

## load_balancer_missing_subnets
**Mensaje**: Faltan las subredes del equilibrador de carga.

El campo `subnets` es obligatorio. En la solicitud para crear un equilibrador de carga, proporcione información acerca de las subredes en las que desea crear el equilibrador de carga.

## load_balancer_missing_subnet_id
**Mensaje**: Falta el ID de subred.

El campo **Subnet ID** es obligatorio. Proporcione un ID de subred con el mandato.

## load_balancer_not_found
**Mensaje**: No se puede encontrar el equilibrador de carga con el ID `<load_balancer_id>`.

Especifique el ID de un equilibrador de carga existente.

## load_balancer_over_quota
**Mensaje**: No se puede crear el equilibrador de carga. La cuota de instancias del equilibrador de carga bajo su cuenta ha alcanzado el límite máximo.

Suprima un equilibrador de carga existente o póngase en contacto con el servicio de soporte para aumentar la cuota de equilibradores de carga de su cuenta.

Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## load_balancer_subnet_not_found
**Mensaje**: No se encuentra la subred con el ID <subnet_id>.

En la solicitud, debe proporcionar los ID de subredes existentes en las que desee crear el equilibrador de carga.

## load_balancer_unchanged_update
**Mensaje**: No hay nada que actualizar en el equilibrador de carga con el ID `<load_balancer_id>`.

No se ha proporcionado ninguna información para actualizar una instancia de equilibrador de carga con el ID que ha especificado.

## load_balancer_update_conflict
**Mensaje**: El equilibrador de carga con el ID <load_balancer_id> o se puede actualizar porque su estado es uno de siguientes:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Actualice el equilibrador de carga cuando esté en estado ACTIVE.

## member_duplicate_address_and_port
**Mensaje**: Ya existe el miembro con la dirección <member_address> y el puerto <member_port> en la agrupación.

Proporcione una combinación de dirección y puerto de miembro exclusiva en la solicitud.

## member_invalid_address
**Mensaje**: La dirección IP de miembro especificada <member_ip> no es válida.

En la solicitud, proporcione una dirección CIDR IPv4 válida para el miembro.

## member_invalid_port
**Mensaje**: El puerto de miembro especificado `<member_port>` no es válido.

Indique un número de puerto válido. Los números de puerto válidos son los comprendidos entre 1 y 65535.

## member_invalid_weight
**Mensaje**: El peso de miembro especificado `<member_weight>` no es válido.

Indique un peso de miembro válido. El valor debe ser un entero del 0 al 100.

## member_ip_not_found
**Mensaje**: El miembro con la dirección IP <member_IP> no pertenece a ninguna subred de la región y VPC del equilibrador de carga.

En la solicitud, proporcione la dirección de un miembro que pertenezca a una subred de la misma región y VPC que el equilibrador de carga.

## member_missing_address
**Mensaje**: Falta la dirección del miembro.

La dirección del miembro es un campo obligatorio. Proporcione un valor para la dirección del miembro.

## member_missing_protocol_port
**Mensaje**: Falta el puerto del miembro.

El puerto del miembro es un campo obligatorio. Proporcione un valor para el puerto del miembro.

## member_not_found
**Mensaje**:  No se encuentra el miembro con el ID <member_id>.

Proporcione un ID de miembro existente.

## member_over_quota
**Mensaje**: No se puede crear el miembro. La cuota de instancias del miembro bajo la agrupación ha alcanzado el límite máximo.

Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## missing_generation_parameter
**Mensaje**: El parámetro de consulta 'generation' es obligatorio.

Para las versiones a partir del 31/5/2019, el parámetro de consulta `generation` es obligatorio para solicitudes de API de VPC on Classic.

## missing_ims_account_id
**Mensaje**: Ninguno

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## missing_version
**Mensaje**: El parámetro `version` es obligatorio y debe tener el formato `AAAA-MM-DD`.

Se requiere un parámetro de versión para todas las solicitudes de API. La versión debe ajustarse al formato _AAAA-MM-DD_. En el caso de meses o fechas de un solo dígito, como el 1 de enero, la versión debe tener este formato: `2019-01-01`. La fecha especificada en el parámetro de versión debe ser posterior al 1 de enero de 2019, pero anterior a la fecha actual. Consulte estos [ejemplos de API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) para saber cómo proporcionar el parámetro de versión.

## network_conflict
**Mensaje**: el CIDR entra en conflicto con un prefijo de direcciones existente en VPC.

Es posible que vea este mensaje si ha proporcionado un CIDR de red que está en conflicto con un CIDR de red existente en el mismo espacio de IP.

Para encontrar los CIDR existentes en los prefijos de dirección de la VPC, ejecute el mandato de API `GET /v1/vpcs/<vpc-id>/address_prefixes` y mire los valores de `cidr` de la respuesta. Compruebe el valor de `cidr` para asegurarse de que el CIDR que ha proporcionado no está siendo utilizado por otros prefijos de direcciones.

Si está utilizando la CLI, ejecute el mandato `ibmcloud is vpc-addrs <vpc-id>` y compruebe el valor de `CIDR Block` para ver los conflictos.

Para encontrar los CIDR existentes en las subredes, ejecute el mandato de API `GET /v1/subnets` para listar todas las subredes de la VPC. A continuación, ejecute el mandato de API `GET /v1/subnets/<subnet-id>` para cada subred de la VPC y compruebe el valor de `ipv4_cidr_block` en la respuesta. Compruebe el valor de `ipv4_cidr_block` para asegurarse de que el CIDR que ha proporcionado no está siendo utilizado por otros prefijos de direcciones.

Si está utilizando la CLI, ejecute el mandato `ibmcloud is subnets` para listar todas las subredes de la VPC. A continuación, ejecute el mandato `ibmcloud is subnet <subnet-id>` para cada subred de la VPC, y compruebe si hay conflictos con el valor de `IPv4 CIDR` en la salida de la CLI.

## not_authorized
**Mensaje**: La solicitud no está autorizada.

Es posible que vea este error si falta la señal IAM o ha caducado. Para ver instrucciones sobre cómo generar una señal, consulte el apartado [Creación de una VPC mediante las API REST](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis). Si la señal no ha caducado, asegúrese de que la cuenta que está utilizando está autorizada para utilizar esta función y de que tiene los [permisos correctos](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Si tiene la autorización correcta, pero sigue obteniendo este error, [póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_found
**Mensaje**: Compruebe si el recurso que está solicitando existe.

Ha hecho referencia a un recurso que no existe o a un recurso al que no tiene acceso. Revise su solicitud para asegurarse de que ha especificado las referencias y los ID adecuados.

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obtener ayuda adicional. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implemented
**Mensaje**: Ninguno

No se ha implementado el método. Compruebe la solicitud para asegurarse de que está llamando a una [API válida](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. [Póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si espera que esta API esté implementada.

## not_in_address_prefix
**Mensaje**: El CIDR proporcionado no encaja en ninguno de los prefijos de direcciones de la zona proporcionada.

Este error se produce si un usuario intenta crear una subred con un CIDR que no se encuentra en ningún prefijo de direcciones para la zona determinada.

Ejecute `GET /vpcs/{vpc_id}/address_prefixes` para obtener la lista de prefijos de direcciones para la VPC. Si utiliza la CLI, puede ejecutar `ibmcloud is vpc-address-prefixes` para listar todos los prefijos de direcciones para la VPC. Mire los valores de `cidr` y de `zone` de la respuesta y asegúrese de que el `cidr` de la subred es un subconjunto del `cidr` del prefijo de direcciones correspondiente a la zona en la que lo está intentando crear.

## over_quota
**Mensaje**: La solicitud superará la cuota de un tipo de recurso.

Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## password_not_ready
**Mensaje**: Ninguno

La instancia debe estar en ejecución para poder recuperar la contraseña. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
**Mensaje**: La agrupación no se puede suprimir porque todavía está asociada con uno o más escuchas.

Asegúrese de que la agrupación esté desasociada de los escuchas y vuelva a intentarlo.

## pool_duplicate_name
**Mensaje**: El nombre de agrupación `<pool_name>` ya está siendo utilizado por otra agrupación. Elija otro nombre.

Elija un nombre de agrupación diferente.

## pool_invalid_name
**Mensaje**: El nombre <pool_name> no es válido.

Proporcione un nombre de agrupación válido.  Un nombre de equilibrador de carga válido comienza por una letra, seguida de letras, dígitos, guiones. La longitud del nombre no puede superar los 40 caracteres.

## pool_invalid_session_persistence
**Mensaje**: El valor de persistencia de sesión de agrupación no es válido.

Proporcione un valor de persistencia de sesión válido. Un valor aceptado es `SOURCE_IP`.

## pool_missing_algorithm
**Mensaje**: Falta el algoritmo de equilibrio de carga.

El algoritmo de equilibrio de carga es un campo obligatorio. Indique el algoritmo de equilibrio de carga en la solicitud. Los valores válidos son `round_robin`, `weighted_round_robin` y `least_connections`.

## pool_missing_health_monitor
**Mensaje**: Falta el supervisor de estado de la agrupación.

El supervisor de estado de la agrupación es un campo obligatorio.

## pool_missing_members
**Mensaje**: Faltan miembros de la agrupación.

Proporcione miembros de agrupación.

## pool_missing_name
**Mensaje**: Falta el nombre de la agrupación.

El nombre de la agrupación es un campo obligatorio.

## pool_missing_protocol
**Mensaje**: Falta el protocolo de la agrupación.

El protocolo de la agrupación es un campo obligatorio. Especifique el protocolo de la agrupación en la solicitud. Los valores válidos son `http` y `tcp`.

## pool_not_found
**Mensaje**: No se puede encontrar la agrupación con el ID `<pool_id>`.

Especifique un ID de agrupación existente.

## pool_over_quota
**Mensaje**: No se puede crear la agrupación. La cuota de agrupaciones para el recurso del equilibrador de carga ha alcanzado el límite máximo.

Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## public_gateway_in_use
**Mensaje**: No se puede suprimir una pasarela pública cuando se está utilizando.

La pasarela pública está conectada actualmente a una o varias subredes. Debe desconectar la pasarela pública de todas las subredes para poder suprimirla.

Para ver qué subred está utilizando la pasarela pública, utilice la API `GET /v1/subnets?version=2019-05-31&generation=1`.  Mandato de CLI equivalente: `ibmcloud is subnets`.  Para desconectar la pasarela pública de la subred, utilice el mandato de API `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` o el mandato de CLI `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
**Mensaje**: Demasiadas solicitudes en un breve periodo de tiempo.

Este mensaje de error se devuelve si se reciben demasiadas solicitudes dentro de un intervalo de tiempo especificado. Espere un momento e inténtelo de nuevo.

## security_group_active_transactions
**Mensaje**: La interfaz no se puede conectar o desconectar hasta que la instancia esté en estado Activo.

La instancia debe estar activa para que sus interfaces de red se puedan conectar a un grupo de seguridad. Utilice `GET /v1/instances/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance` para comprobar el estado de la instancia. Cuando el estado sea `running`, inténtelo de nuevo. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
**Mensaje**: La interfaz ya está conectada al grupo de seguridad.

La interfaz ya está conectada al grupo de seguridad. Utilice `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` para ver las interfaces conectadas.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_exists
**Mensaje**: El grupo de seguridad ya existe.

Los nombres de grupo de seguridad deben ser exclusivos dentro de una VPC. Ya existe un grupo de seguridad con ese nombre en la VPC de destino. Utilice `GET /v1/security_groups?version=2019-05-31&generation=1` o `ibmcloud is security-groups` para ver los grupos de seguridad actuales.

Para ver instrucciones sobre cómo solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic) o el [documento sobre cómo utilizar grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_attached
**Mensaje**: No se puede suprimir el grupo de seguridad mientras haya interfaces conectadas.


Desconecte todas las interfaces antes de suprimir el grupo de seguridad. Utilice `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interface-remove` para desconectar una interfaz.

Para ver instrucciones sobre cómo solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic) o el [documento sobre cómo utilizar grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Mensaje**: Se ha superado el límite de interfaces por grupo de seguridad.

Si se conecta otra interfaz al grupo de seguridad, se superará el límite de interfaces por grupo de seguridad. Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evalúe su estrategia y considere la posibilidad de crear otro grupo de seguridad con reglas similares.

Para ver instrucciones sobre cómo solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic) o el [documento sobre cómo utilizar grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_last_security_group_is_default
**Mensaje**: El grupo de seguridad predeterminado no se puede eliminar si es el único grupo de seguridad conectado.

Una interfaz red debe estar conectada al menos a un grupo de seguridad.
Se conectará al grupo de seguridad predeterminado de la VPC si no se especifica ninguno.
Conecte la interfaz a otro grupo de seguridad y luego intente de nuevo desconectarla del grupo de seguridad predeterminado.
Para obtener información sobre el grupo de seguridad predeterminado, consulte el [documento sobre utilización de grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
**Mensaje**: Se ha superado el límite de grupos de seguridad.

Ha intentado crear un nuevo grupo de seguridad, pero actualmente ha alcanzado la cuota de su cuenta. Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evalúe su estrategia para asignar instancias a grupos de seguridad. Generalmente es posible reducir el número global de grupos de seguridad asignando varias instancias al mismo grupo de seguridad. Esta estrategia reducirá el número de grupos de seguridad y le situará por debajo de la cuota de su cuenta. En casos excepcionales, en general en grandes organizaciones, es necesario ampliar la cuota. En este caso, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para consultar si esto es posible.

## security_group_network_interface_not_active
**Mensaje**: No se puede conectar la interfaz porque no está activa.

Las interfaces de red deben estar activas para que se puedan conectar a grupos de seguridad. Utilice `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance-network-interface` para ver el estado de una interfaz. Cuando el estado sea 'disponible', inténtelo de nuevo. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
**Mensaje**: La interfaz no está conectada.

La interfaz no está conectada al grupo de seguridad. Utilice `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` para ver las interfaces conectadas.

Para ver instrucciones sobre cómo solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic) o el [documento sobre cómo utilizar grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_not_in_vpc
**Mensaje**: La interfaz y el grupo de seguridad están en distintas VPC.

Para conectar una interfaz de red a un grupo de seguridad, la instancia debe estar en la misma VPC que el grupo de seguridad. Utilice `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` o `ibmcloud is security-group` para ver detalles de los grupos de seguridad y la VPC. Utilice `GET /v1/instances/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance` para ver detalles de la instancia y la VPC.

## security_group_order_bindings
**Mensaje**: No se puede suprimir el grupo de seguridad mientras haya pedidos de instancias pendientes.

Si una instancia se ha grado con grupos de seguridad especificados en las interfaces de red, la instancia y las interfaces de red debe estar activas para que se pueda suprimir el grupo de seguridad. Utilice `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` para ver las interfaces de red conectadas y su estado. Cuando el estado de las interfaces sea 'disponible', inténtelo de nuevo. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Mensaje**: El valor máximo de puertos TCP/UDP no puede ser menor que el valor mínimo de puertos.

El valor máximo de puertos no puede ser menor que el valor mínimo de puertos. Especifique un valor máximo de puertos que sea mayor que el valor mínimo de puerto.

## security_group_port_range_both_or_neither
**Mensaje**: El rango de puertos debe estar sin establecer, o bien deben estar establecidos tanto el valor mínimo como el valor máximo de puertos para 'tcp' y 'udp'.

Las reglas con un protocolo 'tcp' o 'udp' pueden tener un rango de puertos sin establecer (para aplicar la regla a todo el tráfico) o bien deben especificar un rango de puertos. Para restringir el tráfico a un solo puerto, establezca el mismo valor para el mínimo y el máximo de puertos. Por ejemplo, el mandato siguiente crearía una regla para permitir SSH en el puerto 22: `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

Para obtener más información sobre configuraciones de reglas de grupos de seguridad, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic).


## security_group_port_range_invalid_protocol
**Mensaje**: Se ha especificado un rango de puertos con el protocolo 'icmp'. El rango de puertos solo es válido para los protocolos 'tcp' o 'udp'.

Las reglas con el protocolo 'icmp' tienen un tipo y un código. Las reglas con los protocolos 'tcp' o 'udp' tienen un rango de puertos. Para obtener más información sobre configuraciones de reglas de grupos de seguridad, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_remote_group_not_in_vpc
**Mensaje**: El grupo remoto no está en la misma VPC que este grupo de seguridad.

Las reglas de grupo de seguridad pueden hacer referencia a un grupo de seguridad remoto, lo que permite el tráfico entre las interfaces conectadas de los dos grupos. Los grupos de seguridad remotos deben estar en la misma VPC. Utilice `GET /v1/security_groups?2019-01-01` o `ibmcloud is security-groups` para ver los grupos de seguridad actuales y sus VPC.

Para obtener más instrucciones para solucionar este problema, consulte el [documento sobre utilización de grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Mensaje**: No se puede suprimir el grupo de seguridad mientras haya reglas de referencia remota conectadas.

El grupo de seguridad contiene una o varias reglas que hacen referencia a un grupo de seguridad remoto. Utilice `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` o `ibmcloud is security-group-rules` para ver las reglas. El campo 'remote' especifica los grupos de seguridad remotos. Vuélvalo a intentar después de suprimir o editar la regla de referencia remota.

Para ver instrucciones sobre cómo solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic) o el [documento sobre cómo utilizar grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

## security_group_remoting_rules_per_sg_exceeded
**Mensaje**: Se ha superado el límite de reglas de referencia remota por grupo de seguridad.

Si se añade otra regla que haga referencia a un grupo de seguridad remoto, se superará el límite de reglas de referencia remota por grupo de seguridad. Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.

## security_group_rules_per_sg_exceeded
**Mensaje**: Se ha superado el límite de reglas por grupo de seguridad.

Si se añade una regla se superará el límite de reglas por grupo de seguridad. Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evalúe su estrategia y considere la posibilidad de crear otro grupo de seguridad.

## security_group_sgs_per_interface_exceeded
**Mensaje**: Se ha superado el límite de grupos de seguridad por interfaz.

Si se conecta esta interfaz a otro grupo de seguridad, se superará el límite de grupos de seguridad por interfaz. Las cuotas por recurso se especifican en [Cuotas y límites para VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evalúe su estrategia y tenga en cuenta la posibilidad de añadir reglas a un grupo de seguridad existente.


## security_group_type_code_invalid_protocol
**Mensaje**: Se ha especificado un tipo/código 'icmp', pero el protocolo solicitado no era 'icmp'. Establezca el protocolo en 'icmp' o especifique un rango de puertos.

Solo las reglas con el protocolo 'icmp' tienen un tipo y un código. Las reglas con los protocolos 'tcp' o 'udp' tienen un rango de puertos. Para obtener más información sobre configuraciones de reglas de grupos de seguridad, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_vpc_default
**Mensaje**: No se puede suprimir el grupo de seguridad predeterminado para una VPC.

El grupo de seguridad predeterminado no se puede suprimir. Para obtener información sobre el grupo de seguridad predeterminado, consulte la guía sobre [Actualización del grupo de seguridad predeterminado](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
**Mensaje**: Ninguno

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
**Mensaje**: CIDR está en conflicto con una subred existente en VPC.

Ejecute la API `GET /subnets` para recuperar todas las subredes de la VPC. Compruebe el valor de `ipv4_cidr_block` para asegurarse de que el CIDR que ha proporcionado no está siendo utilizado por otras subredes.

Si utiliza la CLI, ejecute `ibmcloud is subnets` y observe el valor de "Subnet CIDR" para resolver los conflictos.

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obtener ayuda adicional. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
**Mensaje**: No se puede suprimir la subred hasta que esté vacía. Suprima los recursos de la subred y vuelva a intentarlo.

Se ha solicitado suprimir una subred, pero la subred todavía contiene recursos. Las subredes pueden contener instancias, VPN, equilibradores de carga o pasarelas públicas. Debe suprimir los recursos de la subred para poder suprimir la subred.

En algunas situaciones, este error se puede producir incluso cuando la consola muestra 0 VSI y 0 equilibradores de carga. La supresión es asíncrona y el estado interno puede tardar unos minutos en cambiar. Vuelva a intentar suprimir la subred en unos minutos.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Mensaje**: No se puede suprimir la subred mientras está conectada a una pasarela pública. Desconecte la pasarela pública y vuelva a intentarlo.

Se ha solicitado suprimir una subred, pero la subred todavía tiene una pasarela pública conectada. Debe suprimir o desconectar la pasarela pública para poder suprimir la subred.

Si utiliza la CLI, ejecute `ibmcloud is public-gateways` para obtener una lista de las pasarelas públicas e `ibmcloud is subred-public-public-gateway-detach` para desconectar una pasarela pública de una subred.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Mensaje**: No se puede suprimir la subred mientras contiene direcciones IP. Suprima cualquier instancia de servidor asociada a la dirección IP y vuelva a intentarlo.

Se ha solicitado suprimir una subred, pero la subred todavía contiene direcciones IP. Debe suprimir la instancia de servidor asociada a la dirección IP para poder suprimir la subred.

Si utiliza la CLI, puede ejecutar `ibmcloud is instances` para listar las instancias de servidor. Examine el valor de "Address" para determinar su dirección IP y el de "Status" para determinar su estado. Ejecute `ibmcloud is instance-delete` para suprimir una instancia de servidor que aún no tenga el estado "deleting".

En algunas situaciones, este error se puede producir incluso cuando la consola muestra 0 VSI. La supresión es asíncrona y el estado interno puede tardar unos minutos en cambiar. Vuelva a intentar suprimir la subred en unos minutos.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Mensaje**: No se puede suprimir la subred mientras contiene un equilibrador de carga. Suprima el equilibrador de carga y vuelva a intentarlo.

Se ha solicitado suprimir una subred, pero la subred todavía contiene un equilibrador de carga. Debe suprimir el equilibrador de carga para poder suprimir la subred.

Si utiliza la CLI, puede ejecutar `ibmcloud is load-balancers` para listar los equilibradores de carga. Examine el valor de "Subnets" para determinar su subred y el de "Status" para determinar su estado. Ejecute `ibmcloud is load-balancer-delete` para suprimir un equilibrador de carga que aún no tenga el estado "deleting".

En algunas situaciones, este error se puede producir incluso cuando la consola muestra 0 equilibradores de carga. La supresión es asíncrona y el estado interno puede tardar unos minutos en cambiar. Vuelva a intentar suprimir la subred en unos minutos.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Mensaje**: No se puede suprimir la subred mientras contiene una pasarela VPN. Suprima la pasarela VPN y vuelva a intentarlo.

Se ha solicitado suprimir una subred, pero la subred todavía tiene una pasarela VPN conectada. Debe suprimir la pasarela VPN para poder suprimir la subred.

Si utiliza la CLI, puede ejecutar `ibmcloud is vpn-gateways` para listar las pasarelas de VPN. Examine el valor de "Subnets" para determinar su subred y el de "Status" para determinar su estado. Ejecute `ibmcloud is vpn-gateway-delete` para suprimir una pasarela VPN que aún no tenga el estado "deleting".

En algunas situaciones, este error se puede producir incluso cuando la consola muestra 0 pasarelas de VPN. La supresión es asíncrona y el estado interno puede tardar unos minutos en cambiar. Vuelva a intentar suprimir la subred en unos minutos.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_unknown_state
**Mensaje**: La subred `<subnet_id>` está en un estado no válido para la operación solicitada.

La subred debe estar en estado `available` para que se pueda trabajar con la misma. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
**Mensaje**: El destino ya tiene una IP flotante conectada.

Se ha solicitado adjuntar una dirección IP flotante a la interfaz de red de un servidor, pero la interfaz de red ya tiene una IP flotante conectada.

Para encontrar la dirección IP flotante conectada actualmente a una interfaz de red, ejecute el mandato de API `GET /v1/floating_ips?version=2019-05-31 &generation=1` y busque el ID de interfaz de red en el campo `target.id`.  

Si está utilizando la CLI, ejecute el mandato `ibmcloud is floating-ips` y mire el valor de `Target`. Tenga en cuenta que la CLI trunca el ID de interfaz de red en el primer carácter de guión (“-“). Por ejemplo, la interfaz de red primaria de un servidor con el ID `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` aparece como `primary(abdfcb29-.)` en la salida de la CLI.

## token_invalid
**Mensaje**: La señal de servicio ha caducado o no es válida.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
**Mensaje**: La señal de servicio está vacía o no existe en la solicitud.

Vuelva a crear una señal e inténtelo de nuevo.

## validation_enum
**Mensaje**: El valor proporcionado no es una opción válida.

Uno de los campos suministrados tiene un valor que debe proceder de una enumeración de valores posibles.

Para las reglas de ACL de red, puede especificar una dirección:

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

Por ejemplo, el siguiente valor no sería válido porque `northbound` no es una opción válida en la enumeración `[ inbound, outbound ]`.

```json
{
    "direction":"northbound"
}
```

Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} o la [Consulta de CLI](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window} para saber los valores aceptados.

## validation_failure
**Mensaje**: El archivo JSON proporcionado no coincide con la estructura esperada.

Para solucionar este problema, asegúrese de que el contenido de la solicitud es un archivo JSON válido y de que la solicitud se ajusta a la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_invalid_cidr
**Mensaje**: El valor no es un CIDR válido.

El valor debe ser un bloque CIDR interno válido con una parte de host 0.

Algunos rangos de direcciones IP están reservados. Encontrará más información acerca de los rangos de IP reservados en nuestra visión general sobre [Utilización de su VPC con regiones y subredes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Mensaje**: El valor no es un CIDR IPv4 válido.

Debe ser un bloque CIDR interno de IPv4 con una parte de host 0.

Algunos rangos de direcciones IP están reservados. Encontrará más información acerca de los rangos de IP reservados en nuestra visión general sobre [Utilización de su VPC con regiones y subredes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Mensaje**: El valor no es un CIDR IPv6 válido.

Actualmente, IPv6 no recibe soporte. Utilice una dirección IPv4.

## validation_invalid_address
**Mensaje**: El valor no es una dirección válida.

Debe ser una dirección IP válida.

Encontrará una lista de direcciones IP reservadas individualmente en el documento sobre [Regiones y subredes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv4_address
**Mensaje**: El valor no es una dirección IPv4 válida.

Proporcione una dirección IPv4 válida.

## validation_invalid_ipv6_address
**Mensaje**: El valor no es una dirección IPv6 válida.

Proporcione una dirección IPv6 válida. Actualmente, IPv6 no recibe soporte; utilice una dirección IPv4.

## validation_invalid_field_type
**Mensaje**: El tipo de valor no coincide con el tipo de campo.

Para solucionar este problema, asegúrese de que el contenido de la solicitud cumple con las especificaciones de la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para el punto final al que está llamando.

## validation_max_value
**Mensaje**: Un valor proporcionado en un parámetro es mayor que el permitido.

Proporcione un valor menor que se ajuste al máximo mostrado en la [especificación](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_min_value
**Mensaje**: Un valor proporcionado en un parámetro es menor que el permitido.

Puede obtener este error si intenta crear una subred con `total_ipv4_address_count` menor que 8.

Proporcione un valor mayor que se ajuste al mínimo mostrado en la [especificación](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_not_null
**Mensaje**: El campo suministrado debe ser nulo.

Asegúrese de que el valor sea nulo. Consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

A continuación se muestra un ejemplo no válido:

```json
{
    "name": null
}
```

## validation_only_one
**Mensaje**: El valor proporcionado debe coincidir con uno de los subesquemas.

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_discriminator_forbidden
**Mensaje**: El campo de discriminador prohíbe esta subestructura.

Por ejemplo, si especifica
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

El protocolo es `icmp`, y _port_min_ es un campo `tcp`, de modo que recibirá un error.
1. validation_discriminator_required para reglas `icmp` que faltan
2. validation_discriminator_forbidden para campos `tcp` con `icmp` especificado

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
**Mensaje**: Ninguno

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
**Mensaje**: El valor no es un nombre válido.

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
**Mensaje**: El valor no es un HREF válido.

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
**Mensaje**: Ninguno

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
**Mensaje**: Falta un campo obligatorio.

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
**Mensaje**: El campo suministrado debe ser exclusivo.

Es posible que haya especificado un nombre que ya se está utilizando. Inténtelo con otro valor.

Asegúrese de que la solicitud es conforme con la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Mensaje**: No se puede suprimir la conexión de volumen.

Esta operación no está permitida. No se puede suprimir la conexión de volumen del volumen de arranque porque la instancia necesita el volumen de arranque.

## volume_attachment_update_invalid_request
**Mensaje**: No se ha podido actualizar la conexión de volumen porque la solicitud no es válida.

No se ha podido actualizar la conexión de volumen por una de estas razones:

* No se puede renombrar la conexión de volumen del volumen de arranque.
* No se puede establecer a `true` el campo `delete_volume_on_instance_delete` de la conexión de volumen del volumen de arranque.

## volume_capacity_max
**Mensaje**: La capacidad máxima de volumen permitida es de 2000 GB.

La capacidad de volumen que ha especificado sobrepasa la capacidad de volumen máxima. Especifique un valor del 10 al 2000 GB. Consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para saber los rangos de capacidad aceptados para un perfil personalizado.

## volume_capacity_missing
**Mensaje**: Falta el valor de capacidad de volumen obligatorio en la solicitud.

Cuando crea un volumen, debe especificar una capacidad de volumen basada en esta definición de perfil. Para obtener más información, consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_capacity_zero_or_negative
**Mensaje**: La capacidad de volumen debe ser mayor que cero.

Cuando se crea un volumen, el valor de capacidad especificado en la solicitud debe ser un número positivo de 10 GB a 2000 GB. Consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para saber los valores de capacidad de volumen soportados.

## volume_crn_account_id_mismatch
**Mensaje**: El CRN especificado en la solicitud no pertenece a la cuenta de usuario actual.

El CRN de clave raíz de Key Protect no coincide con el ID de cuenta de su autorización de IAM. Especifique un CRN de clave raíz de Key Protect distinto para su cuenta de IAM. Consulte la documentación de [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) para obtener más información.

## volume_crn_cname_mismatch
**Mensaje**: El CRN especificado en la solicitud no se puede utilizar para el entorno actual.

El CRN que ha especificado en la solicitud no se puede utilizar en el entorno actual. Proporcione un CRN aplicable a su entorno.

## volume_encryption_key_crn_invalid
**Mensaje**: El CRN de clave de cifrado de volumen especificado en la solicitud no es válido.

El CRN de Key Protect que ha proporcionado no es válido. Obtenga la CRN de la clave raíz en el servicio IAM (Identity and Access Management) de la nube. Para obtener más información, consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption).

## volume_encryption_key_not_active
**Mensaje**: La clave de cifrado de volumen especificada en la solicitud no está activa.

Active la clave existente o proporcione una nueva clave. Si no puede activar su clave, póngase en contacto con el administrador del sistema o con el [servicio de soporte al cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
**Mensaje**: No tiene autorización para acceder a la clave de cifrado de volumen proporcionada.

Proporcione otra clave de cifrado para la que tenga acceso, o póngase en contacto con el administrador del sistema para obtener acceso a la clave actual.

## volume_encryption_key_not_implemented
La característica de clave de cifrado de volumen no está soportada.

No se ha podido crear un volumen utilizando la clave de cifrado. Esta versión sólo admite volúmenes con cifrado gestionado por el proveedor. Para utilizar su propia clave de cifrado, utilice una versión de Block Storage for VPC que dé soporte al cifrado gestionado por el cliente.
Póngase en contacto con el [servicio de soporte al cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para obtener más información.

## volume_id_invalid
**Mensaje**: El ID de volumen especificado en el parámetro de solicitud no es válido.

El ID de volumen que ha especificado no tiene el formato correcto. Verifique que ha especificado correctamente el ID de volumen e inténtelo de nuevo. Si no está seguro del ID de volumen, especifique `ibmcloud is volumes` en la CLI o `GET volumes` en la API y busque en la lista de volúmenes.

## volume_id_missing
**Mensaje**: Falta el ID de volumen en el parámetro de solicitud.

Debe proporcionar el ID de volumen en el parámetro de solicitud al obtener un volumen específico.

## volume_id_not_found
**Mensaje**: No se ha encontrado el volumen. El ID de volumen `<volume_id>` no existe, donde `<volume_id>` es el ID de volumen proporcionado.

El ID de volumen especificado no existe. Proporcione un ID de volumen válido.

## volume_iops_zero_or_negative
**Mensaje**: El valor IOPS del volumen debe ser mayor que cero.

Cuando se crea un volumen, el valor de IOPS especificado en la solicitud debe ser un número positivo. Consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para saber los valores de IOPS soportados.

## volume_name_duplicate
**Mensaje**: El nombre de volumen está duplicado. El nombre de volumen `<volume_name>` proporcionado en la solicitud ya existe, donde `<volume_name>` es el nombre de volumen proporcionado.

El nombre de volumen especificado en la solicitud ya existe. Proporcione un nombre de volumen que no esté actualmente en uso.

## volume_not_available
**Mensaje**: El volumen no está disponible. El volumen sólo se puede modificar cuando está en estado 'available'. El estado actual del volumen `<volume_id>` es `<volume_status>`, donde `<volume_id>` es el ID de volumen ID proporcionado en la solicitud y `<volume_status>` es el estado de volumen actual.

Un volumen sólo se puede modificar cuando está en estado `available`. Asegúrese de que el volumen está disponible antes de modificarlo.

## volume_not_deletable
**Mensaje**: La supresión ha fallado.

El volumen sólo se puede suprimir si está en estado `available` o `failed`. Asegúrese de que el volumen está en un estado válido para suprimir.

## volume_not_found
**Mensaje**: No se ha encontrado un volumen con el ID especificado.

Verifique que el ID de volumen que ha especificado es correcto e inténtelo de nuevo. Para obtener una lista de volúmenes, especifique `ibmcloud is volumes` en la CLI o `GET volumes` en la API y busque en la lista de volúmenes.

## volume_not_implemented
**Mensaje**: La operación solicitada no está soportada en este release.

Póngase en contacto con el [servicio de soporte al cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para obtener más información.

## volume_profile_capacity_iops_invalid
**Mensaje**: El perfil de volumen especificado en la solicitud no es válido para la capacidad y/o el valor de IOPS proporcionado.

La capacidad de volumen y/o el valor de IOPS que ha especificado en la solicitud no se permite en el perfil de volumen. Consulte la documentación de [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para saber la capacidad mínima y máxima válida y los valores de IOPS para un perfil personalizado.

## volume_profile_iops_invalid
**Mensaje**: El perfil de volumen especificado en la solicitud no puede aceptar IOPS personalizado.

El perfil de volumen no acepta un valor de IOPS personalizado. Es posible que haya especificado un perfil IOPS en capas, que no requiere que especifique un valor IOPS. Si desea proporcionar un valor de IOPS específico, utilice el perfil de IOPS personalizado.

## volume_profile_name_missing
**Mensaje**: Falta el nombre de perfil de volumen obligatorio en la solicitud.

Falta el nombre de perfil de volumen en el cuerpo de la solicitud al crear un volumen o en el parámetro de solicitud al obtener un volumen. Proporcione un nombre de perfil de volumen en la solicitud e inténtelo de nuevo. Si no sabe el nombre de perfil de volumen, especifique `ibmcloud is volume-profiles` en la CLI o utilice `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` en la solicitud de API y busque en la lista.

## volume_profile_not_found
**Mensaje**: No se encuentra un perfil de volumen con el nombre especificado.

No se ha podido encontrar un perfil de volumen con ese nombre. Verifique que ha proporcionado el nombre de perfil de volumen correcto. Si no sabe el nombre de perfil de volumen, especifique `ibmcloud is volume-profiles` en la CLI o utilice `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` en la solicitud de API y busque en la lista.

## volume_quota_reached
**Mensaje**: Se ha alcanzado la cuota del volumen.

Ha agotado la cuota para la cuenta actual. Intente suprimir algunos volúmenes o póngase en contacto con el [servicio de soporte al cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para aumentar la cuota. Consulte la documentación para obtener información acerca de [cuotas para la infraestructura de Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
**Mensaje**: El ID de grupo de recursos especificado en la solicitud no es válido.

El ID de grupo de recursos que ha especificado en la solicitud no es válido. Compruebe el ID de grupo de recursos correcto. Desde la CLI, utilice el mandato `ibmcloud is volume VOLUME_ID`. La información resultante incluirá el grupo de recursos y el ID del grupo de recursos. Consulte la publicación [Consulta de la CLI de IBM Cloud para VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage) para obtener más información.

## volume_resource_group_not_authorized
**Mensaje**: La acción actual no está autorizada en el grupo de recursos especificado.

No tiene autorización para listar o crear volúmenes en el grupo de recursos especificado. Utilice un grupo de recursos distinto para el que tenga permiso o solicite permiso al administrador para obtener una lista y crear privilegios en el grupo de recursos.

## volume_resource_group_not_found
**Mensaje**: No se ha podido encontrar un grupo de recursos con el ID especificado.

No se ha podido encontrar el ID de grupo de recursos porque es posible que lo haya especificado incorrectamente o que no exista. Compruebe el ID de grupo de recursos correcto. Desde la CLI, utilice el mandato `ibmcloud is volume VOLUME_ID`. La información resultante incluirá el grupo de recursos y el ID del grupo de recursos. Consulte la publicación [Consulta de la CLI de IBM Cloud para VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage) para obtener más información.

## volume_start_not_found
**Mensaje**: No se ha encontrado un volumen con el ID especificado como parámetro de inicio.

No se ha podido encontrar el ID de volumen que ha especificado en el parámetro de inicio de la llamada de volumen de lista. Verifique que el ID de volumen es correcto y que tiene acceso al volumen.

## volume_status_not_available
**Mensaje**: El volumen con el ID especificado no está disponible.

El volumen no está en estado 'Available', puede que esté en estado 'Pending'. Espere a que el volumen esté disponible e inténtelo de nuevo. Si el volumen se está suprimiendo, utilice otro volumen. Para comprobar el estado del volumen, especifique `ibmcloud is volume VOLUME_ID` en la CLI o utilice `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` en la solicitud de API.

## volume_still_attached
**Mensaje**: El volumen todavía está conectado a una instancia.

No se puede suprimir un volumen que todavía está conectado a una VSI. Si ha establecido la supresión automática para el volumen, espere a que se suprima la VSI y el volumen se desconectará y suprimirá. Si no ha establecido la supresión automática, desconecte el volumen manualmente.

## volume_template_invalid
**Mensaje**: Se ha proporcionado una plantilla de volumen no válida.

Ha proporcionado una plantilla de volumen no válida para la creación de un volumen. Consulte la publicación [Referencia de API de VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para verificar que está proporcionando los parámetros correctos en el cuerpo de la solicitud.

## volume_update_invalid_request
**Mensaje**: La solicitud de actualización del volumen no es válida.

La propiedad de cuerpo de la solicitud de actualización que ha especificado no es válida. Consulte la publicación [Referencia de API de VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obtener información y corrija la sintaxis de la solicitud de API.

## vpc_not_empty
**Mensaje**: La VPC no se puede suprimir porque no está vacía.

Se deben suprimir todos los recursos de la VPC para poder suprimir la VPC. Una VPC contiene instancias, subredes, pasarelas públicas, equilibradores de carga y pasarelas de VPN, y algunos de estos recursos contienen recursos a su vez. Consulte la documentación [Cómo suprimir una VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obtener información detallada.

Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
**Mensaje**: Los recursos se encuentran en diferentes VPC.

Los recursos deben estar en la misma VPC. Por ejemplo, si está intentando conectar una pasarela pública a una subred, la pasarela pública debe estar en la misma VPC que la subred.

Para determinar la VPC que contiene la pasarela pública, ejecute el mandato `GET /public_gateways/{id}` y mire el valor "vpc". El mandato de CLI equivalente es `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
**Mensaje**: Los bloques CIDR de `<cidr_type>` tienen bloques CIDR duplicados `<cidr_blocks>`.

Se han proporcionado bloques CIDR duplicados al crear la conexión. Asegúrese de que los bloques CIDR son exclusivos.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_created
**Mensaje**: No se ha podido añadir un bloque CIDR a la conexión de VPN `<vpn_connection_id>`.

Proporcione un CIDR válido que cumpla los requisitos que proporciona la especificación.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Mensaje**: No se ha podido suprimir un bloque CIDR de la conexión de VPN `<vpn_connection_id>`.

Proporcione un CIDR válido que exista en la conexión. Una conexión debe tener al menos un CIDR local y homólogo.

Para ver los bloques CIDR de la conexión, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe los campos `local_cidrs` y `peer_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**Mensaje**: No se ha encontrado el bloque CIDR en la conexión de VPN `<vpn_connection_id>`.

Proporcione un CIDR válido que esté en la conexión.

Para ver los bloques CIDR de la conexión, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe los campos `local_cidrs` y `peer_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**Mensaje**: No se ha podido actualizar un bloque CIDR de la conexión de VPN `<vpn_connection_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
**Mensaje**: El bloque CIDR `<cidr_block>` no representa una dirección válida.

El valor debe ser un CIDR interno válido sin bits de host establecidos.

Para ver los bloques CIDR de la conexión, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe los campos `local_cidrs` y `peer_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**Mensaje**: El bloque CIDR `<cidr_block_1>` se solapa con `<cidr_block_2>`. No se pueden solapar dos bloques CIDR iguales en la misma VPC y dos bloques CIDR locales no se pueden solapar en la misma conexión.

Proporcione un CIDR válido que cumpla los requisitos que proporciona la especificación.

Para ver los bloques CIDR de la conexión, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe los campos `local_cidrs` y `peer_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Mensaje**: El nombre `<vpn_connection_name>` ya se utiliza en la conexión de VPN `<vpn_connection_id>`.

Especifique otro nombre de conexión. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
**Mensaje**: El nombre `<vpn_connection_name>` no es un nombre de conexión de VPN válido.

Un nombre de conexión válido comienza por una letra, seguida de letras, dígitos, signos de subrayado o guiones.

## vpn_connection_invalid_psk_format
**Mensaje**: La clave precompartida (PSK) `<psk>` no tiene un formato válido.

Una PSK válida solo puede contener de 6 a 128 caracteres que sean letras, dígitos, `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Mensaje**: Se necesita al menos un bloque CIDR local para crear una conexión VPN.

Proporcione un CIDR local válido que cumpla los requisitos que proporciona la especificación.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Mensaje**: Las subredes locales a través de las conexiones de VPN para la pasarela VPN `<vpn_gateway_id>` han alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las subredes locales actuales a través de las conexiones de VPN, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections` y compruebe el campo `local_cidrs` para cada conexión.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Mensaje**: Las subredes locales para la conexión de VPN han alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}.

Para ver las subredes locales actuales de una conexión de VPN, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe el campo `local_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**Mensaje**: No se ha podido crear la conexión de VPN `<vpn_connection_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
**Mensaje**: No se ha podido suprimir la conexión de VPN `<vpn_connection_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_found
**Mensaje**: No se ha podido encontrar la conexión de VPN `<vpn_connection_id>`.

Compruebe si el ID de conexión es correcto. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
**Mensaje**: No se ha podido actualizar la conexión de VPN `<vpn_connection_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
**Mensaje**: Al menos un par de CIDR local y CIDR homólogo con la misma pasarela homóloga está duplicado.

Existe otra conexión que tiene un CIDR local y un CIDR homólogo coincidente con éste que existe en esta pasarela.  Proporcione un conjunto válido de CIDR local / homólogo.

## vpn_connection_peer_cidrs_required
**Mensaje**: Se necesita al menos un bloque CIDR homólogo para crear una conexión VPN.

Proporcione un CIDR homólogo válido que cumpla los requisitos que proporciona la especificación.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Mensaje**: Las subredes homólogas a través de las conexiones de VPN para la pasarela VPN `<vpn_gateway_id>` han alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las subredes homólogas actuales a través de las conexiones de VPN, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections` y compruebe el campo `peer_cidrs` para cada conexión.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Mensaje**: Las subredes homólogas para la conexión de VPN han alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las subredes homólogas actuales de una conexión de VPN, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` y compruebe el campo `peer_cidrs`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**Mensaje**: No se ha podido añadir una ruta estática para el bloque CIDR homólogo `<peer_cidr>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Mensaje**: No se ha podido eliminar una ruta estática para el bloque CIDR homólogo `<peer_cidr>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
**Mensaje**: Los bloques CIDR no se pueden modificar cuando se actualiza una conexión. Utilice la API correcta al crear o suprimir bloques CIDR.

Proporcione un valor de solicitud válido que cumpla los requisitos que proporciona la especificación.

Para obtener más instrucciones para solucionar este problema, consulte la [documentación de la API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connections_quota_exceeded
**Mensaje**: Las subredes homólogas a través de las conexiones de VPN para la pasarela de VPN `<vpn_gateway_id>` han alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las conexiones de VPN actuales correspondientes a una pasarela de VPN, utilice la API `GET /vpn_gateways/<vpn_gateway_id>/connections`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**Mensaje**: El nombre `<vpn_gateway_name>` ya se utiliza en la pasarela de VPN `<vpn_gateway_id>`.

Especifique otro nombre de pasarela VPN. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
**Mensaje**: No se ha podido inicializar la pasarela VPN.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
**Mensaje**: El nombre `<vpn_gateway_name>` no es un nombre de pasarela de VPN válido.

Un nombre de pasarela VPN válido comienza por una letra, seguida de letras, dígitos, signos de subrayado o guiones.

## vpn_gateway_invalid_state
**Mensaje**: La pasarela de VPN `<vpn_gateway_id>` está en un estado no válido para la operación solicitada.

La pasarela VPN debe estar en estado `available` para que se pueda trabajar con la misma. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
**Mensaje**: No se ha podido crear una dirección IP para la pasarela VPN.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
**Mensaje**: No se ha podido encontrar un identificador de subred para la plantilla de pasarela de VPN indicada.

El campo **Subnet ID** es obligatorio. Proporcione un ID de subred con el mandato.

## vpn_gateway_missing_name
**Mensaje**: No se ha podido encontrar un nombre para la plantilla de pasarela de VPN indicada.

El campo **nombre** es un campo obligatorio. Proporcione un nombre con el mandato.

## vpn_gateway_not_created
**Mensaje**: No se ha podido crear la pasarela de VPN `<vpn_gateway_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
**Mensaje**: No se ha podido suprimir la pasarela de VPN `<vpn_gateway_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_found
**Mensaje**: No se ha podido encontrar la pasarela de VPN `<vpn_gateway_id>`.

Compruebe si el ID de pasarela VPN es correcto. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
**Mensaje**: No se ha podido actualizar la pasarela de VPN `<vpn_gateway_id>`.

Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_found
**Mensaje**: No se ha podido encontrar la subred `<subnet_id>` de la pasarela de VPN indicada.

Ha hecho referencia a una subred que no existe. Revise su solicitud para asegurarse de que ha especificado el ID de subred adecuado. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Mensaje**: No se ha podido crear la pasarela VPN debido al estado de la subred.

Especifique una subred que esté en estado `available`. Vuelva a intentarlo en unos minutos. Si el problema continúa, [póngase en contacto con el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Mensaje**: No se puede crear la pasarela de VPN porque su cuenta y/o la región ha alcanzado la cuota.

Las cuotas por recurso se especifican en la página [Cuotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para ver las pasarelas de VPN actuales, utilice la API `GET /vpn_gateways`.
Mandato de CLI equivalente: `ibmcloud is vpn-gateways`

## zone_inconsistency
**Mensaje**: Los recursos deben pertenecer a la misma zona.

* Al conectar un volumen, asegúrese de que el volumen y la instancia se encuentran en la misma zona.
* Al asociar una IP flotante, asegúrese de que la IP flotante y la subred se encuentran en la misma zona.
