---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Configuración del acceso a la infraestructura clásica desde VPC
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (tema de ayuda enlazado)

Puede configurar el acceso a la infraestructura clásica de {{site.data.keyword.cloud}}, incluida la conectividad Direct Link, desde una VPC en cada región, para cualquier cuenta. Estas "VPC de acceso clásico" especiales utilizan la misma funcionalidad de direccionamiento (direccionador implícito) que la infraestructura clásica de {{site.data.keyword.cloud}}. Se da por supuesto que los lectores están familiarizados con la red de infraestructura clásica.

Cuando haya configurado una VPC para el acceso clásico, cada host de cálculo (VSI o nativo) que no tenga una interfaz pública de su cuenta clásica puede enviar y recibir paquetes a y desde la VPC de acceso clásico. Sin embargo, recuerde que los cortafuegos, las pasarelas, las ACL de red o los grupos de seguridad pueden filtrar este tráfico. Como práctica recomendada, sugerimos que solo permita el tráfico necesario para que las aplicaciones funcionen correctamente.

En los hosts con una interfaz pública, debe añadir una ruta de vuelta a la VPC habilitada para infraestructura clásica.
{: important}

## Requisitos previos:
{: #vpc-prerequisites}

1. La cuenta clásica debe estar enlazada a su cuenta de IBM Cloud. Consulte [Enlace de cuentas de IBMid](/docs/account?topic=account-unifyingaccounts) para ver instrucciones sobre cómo hacerlo.
1. La cuenta clásica debe estar habilitada para VRF.
    * Si ya tiene Direct Link en su cuenta, estará listo.
    * Si su cuenta no está habilitada para VRF, abra una incidencia para solicitar "Migración de VRF" para su cuenta. Consulte [Cómo puede iniciar la conversión](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion) en la documentación de Direct Link para obtener más información sobre el proceso de conversión.

Los cortafuegos, las pasarelas, las ACL de red o los grupos de seguridad pueden filtrar todo el tráfico de red o parte del mismo entre los recursos de la infraestructura clásica y de VPC.
{: important}

Todas las subredes de una VPC con acceso clásico se compartirán en el VRF de infraestructura clásica, que utiliza direcciones IP del espacio `10.0.0.0/8`. Para evitar conflictos de direcciones IP, **no utilice** direcciones IP del espacio `10.0.0.0/8` al crear subredes en una VPC de acceso clásico.
{: important}

## Creación de una VPC de acceso clásico
{: #create-a-classic-access-vpc}

Puede crear una VPC con la función de acceso clásico utilizando la interfaz de usuario (IU), la interfaz de línea de mandatos (CLI) o la interfaz de programación de aplicaciones (API).

Se debe configurar una VPC para el acceso clásico cuando se cree; no puede actualizar una VPC para añadir o eliminar la función de acceso clásico.
{: note}

### Creación de una VPC de acceso clásico desde la interfaz de usuario
{: #create-a-classic-access-vpc-from-the-user-interface}

Cree una VPC de acceso clásico desde la página **Crear VPC**, pulsando en el recuadro de selección **Habilitar acceso a recurso clásico**, bajo la etiqueta **Acceso clásico**.

![classic-access-ui](/images/classic-access-ui.png)

### Creación de una VPC de acceso clásico mediante la CLI
{: #create-a-classic-access-vpc-using-the-cli}

Utilice el distintivo `--classic-access` cuando cree la VPC. Por ejemplo:

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### Creación de una VPC de acceso clásico mediante la API
{: #create-a-classic-access-vpc-using-the-api}

Pase el parámetro `classic_access` cuando cree la VPC. Por ejemplo:

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Prefijos de dirección predeterminados de una VPC de acceso clásico
{: #classic-access-default-address-prefixes}

Todas las subredes de una VPC con acceso clásico se compartirán en el VRF de infraestructura clásica, que utiliza direcciones IP del espacio `10.0.0.0/8`. Para evitar conflictos de direcciones IP, **no utilice** direcciones IP del espacio `10.0.0.0/8` al crear subredes en una VPC de acceso clásico.
{: important}

Cuando se crea una nueva VPC de acceso clásico, se asignan los prefijos de dirección predeterminados, en función de la región y de la zona, tal como se muestra en la tabla siguiente:

Zona         | Prefijo de dirección
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## Limitaciones
{: #vpc-limitations}

* Solo las redes privadas (o "internas" en la documentación antigua) se conectarán al direccionador implícito privado de la cuenta.
* Únicamente las subredes asignadas con API clásicas se conectan a la parte clásica del direccionador implícito privado. La función de direccionamiento del direccionador implícito no funciona entre subredes en VLAN clásicas.
* Solo se puede habilitar una VPC por región y por cuenta para el acceso clásico.

Según el sistema operativo que tenga instalado en sus VSI clásicas o servidores nativos, el procedimiento de configuración variará. Para obtener más información, consulte [Acerca de los servidores virtuales públicos](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
