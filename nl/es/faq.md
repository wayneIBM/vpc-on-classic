---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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
{:faq: data-hd-content-type='faq'}


# Preguntas frecuentes
{: #faqs}

## ¿Puedo conectar mi VPC a mis otras cargas de trabajo de IBM Cloud?  
{: #faq-0}
{:faq}

Sí, puede configurar el acceso a la infraestructura clásica de {{site.data.keyword.cloud}} desde una VPC en cada región. Para obtener más información, consulte [Configuración del acceso a la infraestructura clásica](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## ¿Cuál es el límite en el número de caracteres en un nombre de VPC?
{: #faq-1}
{:faq}

Actualmente, el límite es de 100. Si se supera este límite, es posible que reciba un mensaje de "error interno".

## ¿Puede empezar alguno de los nombres de mis recursos de VPC por un número?
{: #faq-2}
{:faq}

No; aunque el nombre puede contener números, debe comenzar por una letra.

## ¿Existen restricciones en cuanto a los caracteres que puedo utilizar en un nombre?
{: #faq-3}
{:faq}

Sí, la interfaz de usuario no permite que los dobles guiones duplicados, los signos de subrayado y los puntos formen parte del nombre de una VSI.

## ¿Se puede crear una VSI sin una subred, solo con la IP flotante?
{: #faq-9}
{:faq}

No.

## ¿Se puede conectar una VSI a más de una VPC?
{: #faq-10}
{:faq}

No.

## ¿Se puede cambiar el tamaño de una subred después de crearla en una VPC?
{: #faq-11}
{:faq}

No.

## ¿Existen cargos de ancho de banda de salida para el tráfico entre VPC y COS?
{: #faq-17}
{:faq}

No hay cargos de ancho de banda de salida siempre que se utilice el punto final de ADN para establecer una conexión entre VPC y COS, pero es posible que se apliquen cargos de llamada a la API.

 ## ¿Por qué tengo que especificar una subred al configurar mi VPN para VPC y qué debo especificar?
{: #faq-16}
{:faq}

Cuando esté configurando una pasarela VPN, debe especificar una subred, de modo que la pasarela VPN pueda obtener las direcciones IP que necesita para el servicio de la VPN. Al especificar la subred, mantiene el control sobre cómo se maneja el tráfico de VPN y tiene la flexibilidad de especificar una ubicación que esté cerca de los recursos que utilizan la VPN. De este modo, puede reducir la latencia y mejorar el rendimiento.

## ¿Cómo puedo saber dónde se desplegarán las instancias del equilibrador de carga?
{: #faq-18}
{:faq}

Las subredes elegidas determinan dónde se va a desplegar el equilibrador de carga de VPC. El equilibrador de carga de VPC es un recurso regional. La mejor práctica es elegir subredes de diferentes zonas bajo una región determinada para aumentar la redundancia.


## Durante la asignación de IP flotante en una VPC, un cliente debe especificar el vNIC de la VSI, ¿es correcto?
{: #faq-4}
{:faq}

Sí; de hecho, debe ser la interfaz de red primaria de un servidor.

Sin embargo, si añadimos un recurso de "dirección" en la API bajo el área de red, podríamos perfectamente cambiar la asociación de la IP flotante con la dirección en lugar de con la interfaz.

## ¿Puede un vNIC de una instancia de servidor virtual de mi VPC tener una IP flotante y una IP privada?
{: #faq-5}
{:faq}
 
Sí.

## En la interfaz de usuario de la consola de IBM Cloud para VPC, hay una ”conmutación" para conectar o desconectar cada subred de la pasarela pública (PGW - Public Gateway). ¿Quién crea la PGW? ¿La PGW estará preparada cuando un cliente desee conectar la subred a la PGW en la interfaz de usuario?
{: #faq-6}
{:faq}

La PGW se debe crear de forma explícita mediante la API, ya que la API necesita una asociación explícita de una subred con una pasarela pública determinada.

## Supongamos que VSI1 de una VPC solo tiene vNIC1 y que está conectada a Subnet1. Subnet1 está conectada a una pasarela pública (PGW). ¿Puede un cliente asignar una IP flotante a VSI1?
{: #faq-7}
{:faq}

Sí, un servidor puede estar en una subred conectada a una pasarela pública y también tener una IP flotante. La asignación de una IP flotante a una VSI no está relacionada con si hay una pasarela pública conectada a la subred. Una IP flotante asociada a una VSI prevalecerá sobre la pasarela pública conectada a la subred.

## En mi VPC, VSI1 tiene 2 vNICs, llamadas vNIC1 y vNIC2. ¿Se pueden conectar estas dos vNICs a la misma subred?
{: #faq-8}
{:faq}
 
Sí, no hay limitación en el hecho de tener varias interfaces de red en el mismo servidor conectadas a la misma subred. Sin embargo, no se da soporte a varias NIC en una VSI.

## ¿Quién impone el hecho de que solo puede haber 1 pasarela pública por zona para una VPC?
{: #faq-13}
{: faq}

La API de VPC aplica este límite.

## Durante la creación de PGW, ¿tengo que reservar el FIP, o lo reserva el sistema automáticamente? ¿Veré dicha IP flotante cuando consulte todas las IP flotantes?
{: #faq-12}
{: #faq}

La API crea automáticamente una IP flotante junto con la pasarela pública si no se especifica una IP flotante existente. Y, sí, esa IP flotante se mostrará en la lista.


## ¿Cómo afecta VRF a mis otras funciones de red y a mis subredes?
{: #faq-14}
{:faq}

Advertencias sobre las VLAN y VRF:

* No se permite la expansión de VLAN entre cuentas en el entorno VRF.
* El servicio VPN IPSec está limitado.

VRF no impide el acceso VPN de PPTP o SSL, pero su comportamiento cambia. Sin VRF, una conexión VPN es suficiente para ver todos los servidores de la cuenta. Con VRF, solo puede acceder a los recursos del mercado asociados a su VPN. Así, si se conecta al VPN DAL, solo puede conectarse a servidores DAL.
{: note}

## ¿Cuál es la forma correcta de utilizar el submandato `instance-network-interface-floating-ip-add` en VPC? ¿Se crea o se asigna de antemano una dirección IP flotante?
{: #faq-15}
{:faq}

 Primero tiene que asignar una IP flotante y luego debe proporcionar la dirección FIP en el mandato `add` de IP flotante de la interfaz.

 Por ejemplo: `ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`. Especifique la zona.
