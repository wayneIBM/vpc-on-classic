---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Acerca de Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) es la siguiente generación de la plataforma IBM Cloud. VPC le permite crear su propio espacio en IBM Cloud, para ejecutar un entorno aislado dentro de la nube pública. VPC le proporciona la seguridad de una nube privada, con la agilidad y la facilidad de una nube pública.

Puede gestionar servicios de red e iniciar las instancias que necesite para dar soporte a sus aplicaciones críticas, tolerantes a nube y nativas de nube. Características clave disponibles:

* Crear y gestionar entornos de aplicaciones aisladas a través de una API
* Definir sus propias políticas de red diseñadas para una mayor seguridad y un cómodo acceso
* Diseñar topologías de red con BYOIP (traer su propia IP)
* Suministrar sus recursos y conectarlos entre ellos, o bien aislarlos unos de otros
* Dar cobertura a varias regiones para la recuperación en caso de error y resiliencia
* Utilizar zonas de disponibilidad que permiten conexiones de alta velocidad y baja latencia entre regiones, con alta disponibilidad
* Utilizar redes de alta velocidad y dispositivos de almacenamiento
* Permitir servicios siempre activos (plano de control)
* Proporcionar y utilizar los servicios principales: IPAM, VPN, cortafuegos, SSH, DNS y equilibrio de carga L4
* Configurar otros servicios: escalado automático, CDN, NFV de terceros

La figura de Visión general de VPC, a continuación, ilustra los componentes de VPC disponibles y cómo trabajan conjuntamente para proporcionarle el servicio y la conectividad que puede necesitar. Vuelva a consultar esta imagen a medida que se sumerja en los diversos temas que le ofrecen más detalles sobre cada componente.

![Visión general de IBM Cloud VPC](images/vpc-experience-simple.svg "Visión general de IBM Cloud VPC")

En las secciones siguientes se proporciona una visión general de las características disponibles en IBM Cloud VPC.

## Funciones de redes

{{site.data.keyword.cloud_notm}} VPC ofrece unas funciones de red fáciles y completas, que incluyen la capacidad de crear varias nubes privadas virtuales (VPC) en [regiones multizona disponibles globalmente](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region), subredes en distintas zonas, [selección de rangos de direcciones IP](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), cortafuegos virtuales, ([grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) y [ACL de red](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), redes privadas virtuales ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) de sitio a sitio, y equilibrio de carga ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) con elasticidad.

Una VPC se divide en subredes, utilizando un rango de direcciones IP privadas. Sin embargo, de forma predeterminada, todos los recursos (como las VSI) dentro de la misma VPC pueden comunicarse entre sí, independientemente de su subred. Las subredes están contenidas dentro de una única zona y no pueden abarcar varias zonas, lo que ayuda en la seguridad, en reducir la latencia, y en la alta disponibilidad.

Puede crear varias VPC y subredes fácilmente utilizando los rangos de prefijos sugeridos y las políticas de seguridad de red preconfiguradas, o bien diseñar y definir sus propios prefijos de direcciones y sus propias políticas de seguridad personalizadas.

Los bloques CIDR 161.26/16 y 166.8/14 están los dos reservados y direccionados a cada una de las subredes. Consulte [Puntos finales de servicio disponibles para IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc). Además, el bloque CIDR 10.240/13 está reservado para los prefijos de direcciones de VPC y se divide entre las zonas [de una forma predefinida](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes) y no se puede cambiar.
{: important}

### Acceso a Internet

Hay tres opciones disponibles para habilitar las comunicaciones entre las instancias de servidor virtual (VSI) y la internet pública:
* Utilizar una pasarela pública (PGW) para habilitar la comunicación a internet para todas las instancias de servidor virtual de la subred conectada. No se cobran cargos por utilizar una PGW, excepto por el ancho de banda utilizado.
* Utilizar una IP flotante (FIP) para habilitar la comunicación desde y hacia una sola instancia de servidor virtual (VSI) a internet.
* Utilizar una pasarela VPN. Para obtener más información, consulte la [documentación de VPN para VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)
{: note}

### Acceso clásico

Los usuarios de la infraestructura de {{site.data.keyword.cloud_notm}} pueden conectar su infraestructura clásica, incluyendo la conectividad Direct Link, a una VPC por utilizando el [Acceso clásico](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

### BYOIP

Puede utilizar su propio rango de direcciones IPv4 públicas (BYOIP) en su cuenta de VPC de IBM Cloud.

Si utiliza BYOIP, {{site.data.keyword.cloud_notm}} debe configurar estas direcciones IPv4 en los recursos de {{site.data.keyword.cloud_notm}}, que enviarán paquetes a las direcciones que ha proporcionado y los recibirán de las mismas. Por lo tanto, como resultado de utilizar el rango IPv4 proporcionado en {{site.data.keyword.cloud_notm}}, las direcciones IP pueden estar expuestas al personal de soporte de IBM y a terceros.
{: important}

Debe utilizar la API o la CLI para poder utilizar BYOIP. Esta función no está disponible a través de la interfaz de usuario de la consola de IBM Cloud.
{: note}

### Conectividad global

Puede especificar el ámbito de las aplicaciones y los recursos disponibles de modo que abarquen varias regiones. Utilizando [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc), puede crear conexiones privadas con otros proyectos y con otras partes de sus despliegues híbridos de nube.

Facilidad para escalar y compartir; una red y los recursos de VPC pueden abarcar desde una instalación local hasta la nube.

## Seguridad

La seguridad está integrada en {{site.data.keyword.cloud_notm}} VPC, con [grupos de seguridad](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) que actúan como cortafuegos virtuales para la protección a nivel de instancia y con [listas de control de acceso de red](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACL) para la protección a nivel de subred.

## Alta disponibilidad

{{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) proporciona a sus aplicaciones aislamiento lógico de otras redes en todas las regiones, al tiempo que proporciona escalabilidad y seguridad. Utilice [Equilibradores de carga](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc) para distribuir el tráfico de red a través de un conjunto de destinos para mejorar el rendimiento y la alta disponibilidad. Los equilibradores de carga también supervisan el estado de las aplicaciones y de los servicios. Puede configurar un equilibrador de carga para distribuir el tráfico de aplicaciones de entrada entre instancias en una sola zona o en varias zonas dentro de una región.

## Capacidades de cálculo

Utilice [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) para suministrar recursos de cálculo escalables en IBM Cloud. Cree instancias de servidor virtual (VSI) rápidamente utilizando [perfiles](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles) predefinidos optimizados para sus cargas de trabajo específicas. Al suministrar las instancias de varios alojamientos y [varias vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options), elija una de las [imágenes](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) en stock soportadas o cargue una imagen personalizada.

{{site.data.keyword.cloud_notm}} VPC añade una capa de orquestación de red que elimina el límite de pod de las instancias de servidor virtual, creando una capacidad infinita para escalar instancias. La capa de orquestación de red maneja todas las redes de todas las instancias de servidor virtual que hay dentro de una VPC de IBM Cloud entre regiones y zonas. Con las prestaciones de red definidas por el software que ofrece {{site.data.keyword.cloud_notm}} VPC, tiene más opciones para las VPN, para LBaaS, para las instancias de varias vNIC y para tamaños de subred más grandes.

## Capacidades de almacenamiento

Cuando se suministra una instancia de {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud, se crea automáticamente un volumen de almacenamiento en bloques IOPS (3 IOPS/GB) de finalidad general de 100 GB, como volumen de arranque primario, y se conecta a la instancia. Puede crear volúmenes de almacenamiento en bloques al suministrar una instancia de servidor virtual en una red de VPC, o crear nuevos volúmenes independientes del ciclo de vida de la VSI.

Al suministrar almacenamiento en bloques adicional para la VPC, puede seleccionar un [nivel IOPS](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers) para el almacenamiento en bloques o especificar un [perfil de IOPS personalizado](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom).

## Diseñado para cargas de trabajo nativas de nube e híbridas

Una VPC es ideal para las cargas de trabajo nativas de la nube y para enlazar la infraestructura existente a {{site.data.keyword.cloud_notm}}. Puede crear la mejor nube para todas las cargas de trabajo como, por ejemplo, los cálculos cognitivos, de Inteligencia Artificial y de aprendizaje automático.

Hay más formas en que VPC da soporte a las cargas de trabajo híbridas, tolerantes a nube y nativas de nube:

* Equilibrio de carga con escalado automático horizontal
* Supervisión y comprobación de estado
* Soporte para las GPU
* Suministro rápido de VSI con afinidad y antiafinidad

## Más información

* [**Terminología de VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**Seguridad de VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**Regiones y subredes disponibles de VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**Creación y gestión de recursos de red en VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**Creación y gestión de instancias de servidor virtual en VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**Creación y gestión del almacenamiento en bloques en VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
