---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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

# Limitaciones conocidas
{: #known-limitations}

Este documento contiene un resumen de las características y las API que no están soportadas, las características y casos de uso que aún no están soportados, problemas conocidos del release actual y una lista de características ofrecidas sólo como servicios Beta. Las limitaciones conocidas pueden cambiar a medida que se añaden nuevas capacidades a {{site.data.keyword.vpc_full}} (VPC), por lo que le recomendamos que consulte este documento de vez en cuando.

## Resumen de características a las que no se da soporte
{: #summary-of-features-not-supported}

* El acceso mediante Direct Link a la VPC solo está soportado a través del [**acceso clásico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* Aunque hay un subconjunto de puntos finales de servicios privados disponibles para VPC; no están disponibles todos los puntos finales. Consulte [Puntos finales de servicio disponibles para IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) para saber los servicios que hay disponibles.
* No se permite guardar y restaurar imágenes.
* Las VPC de {{site.data.keyword.cloud_notm}} VPC son regionales, por lo tanto, una VPC de una región no se puede conectar a una VPC de otra región a menos que estén habilitadas para el "acceso clásico" o que estén conectados mediante una conexión de VPN. Consulte [**Acceso clásico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Características y casos de uso a los que aún no se da soporte
{: #features-and-use-cases-not-yet-supported}

En la sección se ofrece más información detallada de características no soportadas y casos de uso, categorizados según su servicio de {{site.data.keyword.cloud_notm}}.

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} no da soporte a dominios de multidifusión ni de difusión.
* {{site.data.keyword.vpc_short}} no ofrece cifrado de extremo a extremo.
* Una VPC no se puede utilizar como homóloga de otras VPC.
* Los puntos finales de servicio de nube no están soportados en VPC. Consulte [Puntos finales de servicio disponibles para IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) para saber los servicios que hay disponibles.

### Cálculo
{: VSI-unsupported-features-and-use-cases}

* Las instancias de servidor virtual (VSI) de {{site.data.keyword.cloud_notm}} y los servidores nativos existentes no se pueden migrar a una VPC.
* No se pueden suministrar servidores nativos en un VPC.

### Red
{: network-unsupported-features-and-use-cases}

* No se pueden añadir rutas personalizadas a una VPC. De forma predeterminada, todas las subredes de una VPC pueden comunicarse entre ellas.
* Una subred no puede estar en varias zonas.
* Una subred no se puede trasladar de una zona a otra.
* No se puede cambiar el tamaño de las subredes después de que se creen.
* Los recursos de {{site.data.keyword.cloud_notm}} no pueden estar en la misma subred en una VPC. Se puede establecer la conectividad entre los recursos clásicos y una VPC en la cuenta utilizando el [**Acceso clásico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* No se da soporte a IPv6.
* No puede utilizar su propia IP pública como IP flotante. IBM proporciona la agrupación de IP flotante.
* Una IP flotante está enlazada a una zona. Por ejemplo, una IP flotante reservada de una agrupación de la Zona 1 no se puede asignar a una VSI de la Zona 2 en la misma VPC.
* No se pueden mover direcciones IP privadas entre VSI.
* No se pueden mover interfaces de red entre VSI.
* No se puede designar la dirección IP específica de la VSI. Solo puede especificar el rango de IP para la subred. Cuando conecte una VSI a una subred, el sistema asigna una IP privada desde dicha subred.
* {{site.data.keyword.vpc_short}} se suministra con sus propias herramientas de red como servicio (Network as a Service), como por ejemplo LBaaS, ACL y grupos de seguridad. No puede utilizar sus propios dispositivos de red (por ejemplo, una pasarela Vyatta u otro equilibrador de carga) para controlar ningún recurso de la VPC. Paralelamente, no puede utilizar su propio equilibrador de carga ni sus servicios de grupos de seguridad en la infraestructura clásica de {{site.data.keyword.cloud_notm}} para controlar los recursos de la {{site.data.keyword.vpc_short}}.
* La pasarela pública no permite que el tráfico se inicie desde Internet a una VSI en la misma IBM Cloud VPC. Para tal fin se debe utilizar una IP flotante.
* ACL no tiene estado, de modo que el tráfico de retorno se debe permitir de forma explícita en las reglas de ACL.

## Problemas conocidos de este release
{: #known-issues-in-this-release}

{{site.data.keyword.block_storage_is_short}} podría no validar con precisión la exclusividad del nombre de volumen cuando se cumplen todas las condiciones siguientes:

* La solicitud de suministro es simultánea
* El volumen tiene el mismo nombre
* El volumen se encuentra en la misma región

## Servicios Beta disponibles

La función para intercambiar de forma segura con un dispositivo está disponible como servicio Beta. Dispone de documentación para el intercambio con diversos tipos de dispositivos:

* [Homólogo remoto Vyatta](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [Homólogo remoto StrongSwan](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [Homólogo remoto FortiGate](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [Homólogo remoto Juniper vSRX](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [Homólogo remoto Cisco ASAv](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
