---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-12"

keywords: endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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

# Puntos finales de servicio disponibles para IBM Cloud VPC
{: #service-endpoints-available-for-ibm-cloud-vpc}

Cuando esté listo para ejecutar cargas de trabajo, puede acceder a dos tipos de puntos finales: Puntos finales de IaaS (Infrastructure as a Service) y puntos finales de servicio de nubes (CSE - Cloud Service Endpoints). Los puntos finales de IaaS vienen subministrados previamente y están listos para utilizar; pero, los CSE se deben suministrar aparte para poder utilizarlos.

Los puntos finales de estos servicios utilizan _direcciones direccionables_; es decir, direcciones distintas a las especificadas en RFC 1918, y estas direcciones puede parecer que se comunican a través de la Internet pública, pero el tráfico de y hacia estos puntos finales no sale de la nube. Por lo tanto, este tráfico evita los cargos por ancho de banda asociados con el tráfico que sale de la nube y va a la Internet pública.

## Puntos finales de Infrastructure as a Service (IaaS)
{: #infrastructure-as-a-service-iaas-endpoints}

Hay disponibles servicios de infraestructura utilizando ciertos nombres DNS del dominio `adn.networklayer.com`, y resuelven en direcciones `161.26.0.0/16`.

Los servicios a los que puede acceder incluyen:

* Resoluciones de DNS
* Duplicados de Ubuntu APT (Advanced Packaging Tool)
* Cloud Object Storage
* NTP

## Puntos finales de resolutores DNS (Domain Name System)
{: #dns-domain-name-system-resolver-endpoints}

Los resolutores de DNS utilizan direcciones IP, en lugar de nombres. Para puntos finales de servicio de nube compartidos, se deben utilizar las direcciones de servidor DNS `161.26.0.10` y `161.26.0.11`.

## Duplicados de Ubuntu APT
{: #ubuntu-apt-mirrors}

Hay disponibles duplicados de APT para actualizar imágenes de Ubuntu en `mirrors.adn.networklayer.com`, que deberían resolverse en `161.26.0.6`. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

Por ejemplo, para acceder a un punto final privado correspondiente a un servicio de almacenamiento de objetos en la red privada de IBM Cloud, sustituya la parte de "domain" del URL por `cloud-object-storage.appdomain.cloud`. Para acceder a un grupo de COS creado en otra región, utilice el punto final del grupo, no el punto final de la región donde se ha creado la VSI.

## Servidores NTP (Network Time Protocol)
{: #network-time-protocol-ntp-servers}

Hay un servidor NTP disponible en `time.adn.networklayer.com`, que debería resolverse en `161.26.0.6`.

## Puntos finales de servicio de nube
{: #cloud-service-endpoints}

Los puntos finales de servicio de nube son servicios proporcionados por otros usuarios de la nube. Pronto estarán disponibles a través de nombres DNS en el dominio `cloud.ibm.com`. Se resuelven en direcciones `166.8.0.0/14`.
