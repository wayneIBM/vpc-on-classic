---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Creación de una VPC mediante las API REST
{: #creating-a-vpc-using-the-rest-apis}

En este documento se muestra cómo crear recursos de {{site.data.keyword.cloud}} Virtual Private Cloud con las API REST, utilizando `curl`. Para ver fragmentos de código que llamen a las API REST utilizando Go y Python, vea este otro [código de ejemplo](https://github.com/IBM-Cloud/vpc-api-samples).

## Requisitos previos

1. Asegúrese de tener una clave SSH, que se utilizará para conectar con la instancia de servidor virtual.

   Es posible que ya tenga una clave SSH pública. Busque un archivo denominado `id_rsa.pub` en un directorio `.ssh` del directorio de inicio; por ejemplo, `/Users/<USERNAME>/.ssh/id_rsa.pub`. El archivo empieza por `ssh-rsa` y termina con su dirección de correo electrónico.

   Si no tiene una clave SSH pública o si ha olvidado la contraseña de una clave existente, genere una nueva ejecutando el mandato `ssh-keygen` y siguiendo la solicitud siguiente.
2.  Asegúrese de tener una clave de API para su cuenta de IBM Cloud. Si no tiene ninguna clave de API, consulte [Creación de una clave de API](/docs/iam?topic=iam-userapikey#create_user_key). Necesitará almacenar esta clave de API en una variable de entorno en el Paso 1.

## Paso 1: Almacene la clave de API como una variable

Ejecute el mandato siguiente para almacenar la clave de API en una variable de entorno:

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## Paso 2: Obtenga una señal de IBM Identity and Access Management (IAM)

Consulte el tema [Obtención de una señal de IBM Cloud IAM utilizando una clave de API](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) para saber cómo obtener una señal IAM, o bien utilice los mandatos de ejemplo siguientes.

Ejecute el mandato siguiente para obtener y analizar una señal IAM utilizando el programa de utilidad `jq`. Puede modificar el mandato para utilizar otra herramienta de análisis, o bien puede eliminar la última parte del mandato si prefiere analizar la señal usted mismo, manualmente.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

Para ver la señal IAM, ejecute `echo $iam_token`. El resultado se parecerá al siguiente:

```
Bearer <su señal>
```

La cabecera de autorización espera que la señal IAM empiece por `Bearer`. Si el resultado que recibe no incluye `Bearer`, actualice la variable `iam_token` para incluirlo. En estos ejemplos se presupone que `Bearer` está incluido en `iam_token`.

Debe repetir el paso anterior para renovar la señal de IAM cada hora, ya que la señal caduca.
{: important}

## Paso 3: Almacene el punto final de la API y el parámetro de versión como una variable

Los puntos finales de API son por región y siguen la convención `https://<region>.iaas.cloud.ibm.com`. Los puntos finales por región son:

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

Almacene el punto final de la API en una variable para poder utilizarlo en las solicitudes de API sin tener que escribir el URL completo. Por ejemplo, para almacenar el punto final us-south en una variable, ejecute este mandato:

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

Para verificar que esta variable se ha guardado, ejecute `echo $rias_endpoint` y asegúrese de que la respuesta no está vacía.

Todas las solicitudes de API deben incluir el parámetro `version`, con el formato `AAAA-MM-DD`. Ejecute el siguiente mandato para almacenar la fecha de la versión en una variable para poderla reutilizar en la sesión. Para obtener más información sobre cómo establecer el parámetro `version`, consulte **Control de versiones** en la publicación [Consulta de API para VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

Para verificar que esta variable se ha guardado, ejecute `echo $version` y asegúrese de que la respuesta no está vacía.

## Paso 4: Ejecute la API GET regions

El mandato siguiente devuelve las regiones disponibles para VPC, en formato JSON. Se debe devolver al menos un objeto.

Se requiere un parámetro de consulta `version` y `generation` en cada solicitud de API. Para obtener detalles, consulte la publicación [Consulta de API para VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Si se encuentra con resultados inesperados, añada el distintivo `--verbose` (depuración) después del mandato `curl` para obtener información de registro detallada. Consulte también los errores encontrados con frecuencia en nuestra [página de resolución de problemas](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Paso 5: Ejecute la API GET zones

El mandato siguiente devuelve todas las zonas disponibles para VPC en la región `us-south`, en formato JSON. Se debe devolver al menos tres objetos.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Actualice el nombre de la región en el parámetro, `us-south`, según el punto final de región que esté utilizando. Un punto final de región sólo conoce sus propias zonas, por lo que si está utilizando el punto final en TOKIO, utilice `jp-tok`.
{: note}

## Paso 6: Ejecute la API GET profiles

El mandato siguiente devuelve los perfiles disponibles para sus instancias, en formato JSON. Se debe devolver al menos un objeto.

Añada ` | json_pp ` después del mandato curl para obtener una serie JSON legible
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Si la salida de API está limitada por la paginación, establezca un límite superior a `total_count`, por ejemplo:

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Paso 7: Ejecute la API GET images

El mandato siguiente devuelve las imágenes disponibles para sus instancias, en formato JSON. Se debe devolver al menos un objeto.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Paso 8: Ejecute la API GET VPCs

El mandato siguiente devuelve las VPC que se hayan creado bajo su cuenta, en formato JSON.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Paso 9: Cree una nube privada virtual (VPC)

Cree una VPC de {{site.data.keyword.cloud_notm}} denominada `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

Para el resto de llamadas, tendrá que conocer el ID de la VPC de {{site.data.keyword.cloud_notm}} que ha creado. Almacene su ID en una variable. El mandato será algo parecido a esto: `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## Paso 10: Cree una subred

Cree una subred en la VPC de {{site.data.keyword.cloud_notm}}. En el ejemplo siguiente se crea una VPC en la zona `us-south-2`.

En el ejemplo siguiente se utiliza el [prefijo de dirección predeterminado](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) para la zona `us-south-2`. Puede que se hayan cambiado los valores predeterminados para su cuenta, consulte [Cómo planificar las direcciones VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

Para obtener una lista de prefijos de direcciones para su vpc, ejecute el mandato siguiente `curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`.
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Almacene el ID de subred en una variable.

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## Paso 11: Compruebe el estado de la subred

Para suministrar recursos en la subred, la subred debe estar en estado `available`. Antes de continuar, consulte el recurso de subred y asegúrese de que el estado es `available`. Si el estado es `failed`, póngase en contacto con [el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) con los detalles. Puede intentar suministrar otra subred.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Paso 12: Cree una pasarela pública

Para permitir que las instancias del servidor virtual de la subred tengan acceso a Internet público, añada una pasarela pública (PGW) a la subred.  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Almacene el ID de pasarela pública en una variable.

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

A fin de conectar y utilizar la pasarela pública, ésta debe estar en estado `available`. Antes de continuar, consulte el recurso de pasarela pública y asegúrese de que el estado es `available`. Si el estado es `failed`, póngase en contacto con [el equipo de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) con los detalles. Puede intentar continuar probando a suministrar otra pasarela pública.

Para comprobar el estado de la pasarela pública, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Paso 13: Conecte la pasarela pública a la subred

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Paso 14: Cree una clave SSH

Cree una clave con la clave SSH pública. Esta clave se utiliza cuando se crea la instancia de servidor virtual. La clave también es necesaria para iniciar sesión en la instancia de servidor virtual.

Las claves sólo se pueden añadir claves al crear por primera vez la VPC en la IU o la CLI. No existe ninguna herramienta para añadir claves posteriormente.
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

Almacene el ID de clave en una variable.

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## Paso 15: Elija un perfil y una imagen para la instancia de servidor virtual

Ejecute las API para obtener una lista de todos los perfiles y de todas las imágenes disponibles para la instancia de servidor virtual, y elija una combinación.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Para obtener detalles adicionales acerca de los perfiles, puede consultar el catálogo global con el mandato de CLI de {{site.data.keyword.cloud_notm}}: `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`. Por ejemplo:

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

El mandato siguiente muestra la lista de las imágenes disponibles.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Almacene el nombre de perfil y el ID de imagen en variables, que utilizará para suministrar una instancia de servidor virtual.

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## Paso 16: Suministre una instancia de servidor virtual

Suministre una instancia de servidor virtual (VSI) en la subred que ha creado. Pase la clave SSH pública para que pueda iniciar la sesión después de que se suministre la instancia.

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-south-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

Almacene el ID de instancia de servidor virtual en una variable.

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## Paso 17: Compruebe el estado de la instancia de servidor virtual

El suministro de una instancia de servidor virtual puede tardar unos minutos. Antes de continuar, consulte el estado del servidor y asegúrese de que esté `running`.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Almacene el ID de interfaz de red primaria de la instancia de servidor virtual (devuelta en la llamada de API anterior) en una variable.  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

No puede obtener la interfaz de red primaria hasta que consulte la instancia específica.
{: note}

## Paso 18: Cree una IP flotante

Para crear una IP flotante para la instancia de servidor virtual utilice la interfaz de red primaria de la instancia como destino para la nueva dirección IP flotante.

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Almacene el ID de la IP flotante en una variable.

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## Paso 19: Añada una regla al grupo de seguridad predeterminado para permitir tráfico SSH

Configure el grupo de seguridad para definir el tráfico de entrada y de salida que se permite para la instancia.

Busque el grupo de seguridad para la VPC:

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Almacenar el ID del grupo de seguridad en una variable.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Ahora cree una regla para permitir el tráfico SSH de entrada:

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Paso 20: SSH en la instancia de servidor virtual

Para ejecutar SSH sobre el servidor, utilice la dirección (`address`) de la IP flotante que ha creado antes. Para obtener la dirección (`address`) de la IP flotante, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Utilice la dirección (`address`) de la IP flotante para conectarse a la instancia de servidor virtual con SSH:

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

El acceso SSH al servidor virtual puede estar bloqueado por los grupos de seguridad. Si se necesita el acceso SSH, debe utilizar un [grupo de seguridad adecuado](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).
{: note}

Consulte [Conexión con la instancia mediante Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance)
para obtener más información sobre cómo conectarse a su instancia.

## Paso 21: Cree y conecte un volumen de almacenamiento en bloques

Cree un volumen de almacenamiento en bloques con una solicitud similar a este ejemplo y especifique un nombre de volumen, una zona y un perfil.

Para ver una lista de los perfiles de volumen, haga esta solicitud:

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

Los perfiles pueden ser generales (3 IOPS/GB), 5iops-tier, 10iops-tier o personalizados. Consulte [Acerca de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
para obtener información sobre la capacidad de volumen y los rangos de IOPS según el perfil de volumen que haya seleccionado.

No es necesario que proporcione un nombre de volumen en la solicitud, el sistema crea uno de forma predeterminada.  En este ejemplo se especifica un nombre de volumen `helloworld-vol`.  Todos los nombres de volumen deben ser exclusivos.

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Para el resto de llamadas, tendrá que conocer el ID del nuevo volumen que ha creado. Guarde el ID del volumen como una variable, por ejemplo: `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

El estado del volumen es `pendiente` cuando se crea por primera vez. Para poder continuar, el volumen debe pasar al estado `available`, lo que tarda unos minutos.
{: note}


Para comprobar el estado del volumen, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Cree una conexión de volumen para conectar el nuevo volumen a la instancia de servidor virtual. Utilice la variable de ID de instancia que ha creado anteriormente en la solicitud.  Utilice el ID de volumen, el CRN del volumen o el URL para especificar el volumen en el parámetro de datos.  En este ejemplo se utiliza la variable que hemos creado para el ID de volumen.

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

Tras crear la conexión de volumen, obtenga la ID de la conexión de volume.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Guarde el ID de conexión como una variable.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

Especifique el ID de conexión de volumen para adjuntar el nuevo volumen a una instancia de servidor virtual.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Paso 22 (opcional): Suprima los recursos

Opcionalmente, suprima los recursos. No se puede suprimir un recurso si contiene otros recursos. Por ejemplo, una nube privada virtual no se puede suprimir si contiene subredes, y una subred no se puede suprimir si contiene instancias del servidor virtual. En una operación de supresión, la API puede retornar rápidamente, pero es posible que la supresión de recursos siga en curso. Después de emitir la solicitud de supresión, asegúrese de que el recurso se ha suprimido antes de intentar suprimir el recurso padre. Consulte [Supresión de una VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obtener más detalles.

### Suprima la IP flotante

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Suprima la instancia de servidor virtual

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Suprima la clave

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Suprima la subred

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Suprima la pasarela pública

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### Suprima la VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Suprima el volumen

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## ¡Enhorabuena!

Ha suministrado correctamente una instancia de nube privada virtual utilizando las API REST. Para probar más mandatos de API, consulte el siguiente enlace:

* [Consulta de API para VPC](https://{DomainName}/apidocs/vpc-on-classic)
