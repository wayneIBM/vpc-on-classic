---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

subcollection: vpc-on-classic

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Conexión con IBM Cloud Object Storage desde VPC
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

Este documento le muestra cómo conectarse a {{site.data.keyword.cloud}} Object Storage desde Virtual Private Cloud. La información de punto final se aplica específicamente a {{site.data.keyword.cloud_notm}} Virtual Private Cloud en el entorno de infraestructura clásica.
{: shortdesc}


## ¿Qué es IBM Cloud Object Storage (COS)?
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) es una plataforma de escala web que almacena datos no estructurados. Proporciona fiabilidad, seguridad, disponibilidad y recuperación tras desastre sin réplica.

La información almacenada en {{site.data.keyword.cloud_notm}} Object Storage está cifrada y distribuida entre varias ubicaciones geográficas. Se puede acceder a esta información a través de una implementación de la API S3. Este servicio utiliza las tecnologías de almacenamiento distribuido que proporciona el sistema IBM Cloud Object Storage.

IBM Cloud Object Storage está disponible con tres tipos de configuraciones para la resiliencia: **Varias regiones**, **Regional** y **Un solo centro de datos**. 
* El servicio **Varias regiones** proporciona una mayor durabilidad y disponibilidad que el uso de una sola región, a costa de una latencia ligeramente superior. Este servicio está disponible actualmente en las áreas EE.UU., U.E. y A.P.  
* El servicio **Regional** hace lo inverso. Distribuye los objetos en varias zonas de disponibilidad dentro de una sola región. Está disponible en las regiones de EE.UU., U.E. y A.P . Si una región o una zona no está disponible, el almacén de objetos continúa funcionando sin impedimento. 
El servicio **Un solo centro de datos** distribuye objetos entre varias máquinas dentro de la misma ubicación física. Compruebe este documento periódicamente para saber las regiones disponibles.

### Puntos finales directos de COS para utilizar con VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Para enviar una solicitud de API REST, debe establecer un punto final o un URL de destino cada ubicación de almacenamiento COS debe tener su propio conjunto de URL. Los puntos finales directos proporcionan a los servidores conexiones directas de alta velocidad a los servicios de {{site.data.keyword.cloud_notm}}, que no suponen costes adicionales de ancho de banda. Con el punto final directo de COS, una VPC puede conectarse a un grupo de COS ubicado en cualquier parte del mundo. 

No hay ningún cargo para el tráfico desde los VPC a todos los puntos finales COS listados en esta página.
{: note}

* Un cliente de VPC también tiene acceso al grupo de COS por el punto final público.
* Un cliente de VPC sólo tiene acceso a la [API de configuración de COS](https://{DomainName}/apidocs/cos/cos-configuration) por el punto final público y no por el punto final directo. La API de configuración de COS se puede utilizar para configurar algunas características de COS en los grupos, así como para ver los metadatos de los grupos.
* Un cliente de VPC no tiene acceso a un grupo de COS cuando el cortafuegos está habilitado.

## Cómo conectarse a IBM Cloud Object Storage (COS) desde una VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Suministre COS desde el [catálogo ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}.
2. Cree un grupo de COS en una de las regiones que se muestran en la siguiente sección.
3. Utilice el punto final para comunicarse con el grupo de COS.

### Puntos finales regionales
{: #regional-endpoints}

Los grupos creados en un punto final regional distribuyen datos a través de tres centros de datos, que se distribuyen a través de una área metropolitana. Cualquiera de estos centros de datos puede sufrir una interrupción, o incluso destruirse, sin que afecte a la disponibilidad.

| **Región** | **Punto final** |
|------------|-------------------------------|
| EE.UU. sur | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| EE.UU. este | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| U.E. Reino Unido | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| U.E. Alemania | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| A.P. Australia | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| A.P. Japón | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### Puntos finales entre regiones
{: #cross-region-endpoints}

Los grupos creados en un punto final entre regiones distribuyen datos a través de tres regiones. Cualquiera de estas regiones puede sufrir una interrupción o incluso una destrucción sin afectar a la disponibilidad. Las solicitudes se direccionan al centro de datos de la región más cercana utilizando el direccionamiento de Border Gateway Protocol (BGP).

En el caso de una interrupción, las solicitudes se vuelven a direccionar a una región activa, automáticamente. Los usuarios avanzados que desean escribir su propia lógica de migración tras error pueden hacerlo, enviando solicitudes al punto final específico de la región e ignorando el direccionamiento BGP.

| **Varias regiones - EE.UU.** | **Punto final** |
|------------|-------------------------------|
| Varias regiones - EE.UU. | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de San José | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **Varias regiones - U.E.** | **Punto final** |
|------------|-------------------------------|
| Varias regiones - U.E. | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Amsterdam | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Frankfurt | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Milán | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **Varias regiones - Asia Pacífico** | **Punto final** |
|------------|-------------------------------|
| Varias regiones - Asia Pacífico | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Tokio | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Seúl | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Punto de acceso de Hong Kong | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### Puntos finales de un solo centro de datos
 {: #single-datacenter-endpoints}

Los centros de datos únicos no están colocalizados con los servicios de IBM Cloud, como IAM o Key Protect, y no ofrecen resiliencia en caso de interrupción o destrucción del sitio.

Si un error de red da como resultado una partición, de forma que el centro de datos no puede acceder a un núcleo de la región de IBM Cloud para acceder a IAM, la información de autenticación y autorización se lee de una memoria caché que puede quedar obsoleta. Esta acción puede hacer que no se apliquen las políticas IAM nuevas o alteradas durante un período de hasta 24 horas.
{:important}

| **Región** | **Punto final** |
|------------|-------------------------------|
| Amsterdam, Países Bajos | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, India | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hong Kong | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Australia | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Ciudad de México, México | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Milán, Italia | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montreal, Canadá | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Noruega | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San José, EE.UU. | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brasil | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Seúl, Corea del Sur | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Canadá | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
