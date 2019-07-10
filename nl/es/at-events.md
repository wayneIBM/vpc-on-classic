---

copyright:
  years: 2019

lastupdated: "2019-06-13"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc-on-classic

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Sucesos de Activity Tracker with LogDNA
{: #at-events}

Utilice el servicio {{site.data.keyword.at_full}} para realizar seguimiento de cómo los usuarios y las aplicaciones interaccionan con {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) en {{site.data.keyword.cloud}}.
{:shortdesc}

El servicio {{site.data.keyword.at_full}} registra las actividades iniciadas por el usuario para cambiar el estado de un servicio dentro de {{site.data.keyword.cloud}}. Para obtener más información, consulte [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## Lista de sucesos: recursos de red
{: #events-volumes}

| Recurso  | Acción  | Descripción  |
|:----------------|:-----------------------|:-----------------------|
| VPC  | is.vpc.vpc.create   | La VPC se ha creado.  |
| VPC  | is.vpc.vpc.update   | La VPC se ha actualizado.  |
| VPC  | is.vpc.vpc.delete   | La VPC se ha suprimido.  |
| VPC  | is.vpc.address-prefix.create  | Se ha añadido el prefijo de dirección a la VPC.  |
| VPC  | is.vpc.address-prefix.update  | El prefijo de dirección de la VPC se ha actualizado.   |
| VPC  | is.vpc.address-prefix.delete  | El prefijo de dirección se ha eliminado de la VPC.  |
| VPC  | is.vpc.vpc-route.create   | Se ha añadido una ruta a la VPC.   |
| VPC  | is.vpc.vpc-route.update   | La ruta de la VPC se ha actualizado.  |
| VPC  | is.vpc.vpc-route.delete   | Se ha eliminado la ruta de la VPC.   |
| IP flotante  | is.floating-ip.floating-ip.delete   | Se ha creado la IP flotante.  |
| IP flotante  | is.floating-ip.floating-ip.delete   | Se ha actualizado la IP flotante.  |
| IP flotante  | is.floating-ip.floating-ip.delete   | Se ha suprimido la IP flotante.  |
| ACL de red  | is.network-acl.network-acl.create   | Se ha creado la ACL de red.  |
| ACL de red  | is.network-acl.network-acl.update   | Se ha actualizado la ACL de red.  |
| ACL de red  | is.network-acl.network-acl.delete   | Se ha suprimido la ACL de red.  |
| ACL de red  | is.network-acl.rule.create  | Se ha añadido la regla a la ACL de red.  |
| ACL de red  | is.network-acl.rule.update  | Se ha actualizado la regla de la ACL de red.   |
| ACL de red  | is.network-acl.rule.delete  | Se ha eliminado la regla de la ACL de red.  |
| pasarela pública | is.public-gateway.public-gateway.create   | Se ha creado la pasarela pública.   |
| pasarela pública | is.public-gateway.public-gateway.update   | Se ha actualizado la pasarela pública.   |
| pasarela pública | is.public-gateway.public-gateway.delete   | Se ha suprimido la pasarela pública.   |
| grupo de seguridad | is.security-group.security-group.create   | Crear grupo de seguridad   |
| grupo de seguridad | is.security-group.security-group.delete   | Suprimir grupo de seguridad   |
| grupo de seguridad | is.security-group.security-group.update   | Actualizar grupo de seguridad   |
| grupo de seguridad | is.security-group.security-group.rule-create  | Crear regla de grupo de seguridad  |
| grupo de seguridad | is.security-group.security-group.rule-delete  | Suprimir regla de grupo de seguridad  |
| grupo de seguridad | is.security-group.security-group.rule-update  | Actualizar regla de grupo de seguridad  |
| grupo de seguridad | is.security-group.security-group.interface-attach | Conectar interfaz a grupo de seguridad   |
| grupo de seguridad | is.security-group.security-group.interface-detach | Desconectar interfaz de grupo de seguridad   |
| subred   | is.subnet.subnet.create   | Se ha creado la subred.   |
| subred   | is.subnet.subnet.update   | Se ha actualizado la subred.   |
| subred   | is.subnet.subnet.delete   | Se ha suprimido la subred.   |
| subred   | is.subnet.network-acl.update  | Se ha sustituido la ACL de red de la subred.   |
| subred   | is.subnet.public-gateway.operate  | La Pasarela pública se ha conectado a la subred.  |
| subred   | is.subnet.public-gateway.operate  | La Pasarela pública se ha desconectado de la subred.  |

## Lista de sucesos: recursos de cálculo
{: #events-computes}

En la tabla siguiente se listan las acciones relacionadas con los recursos de cálculo y la generación de sucesos.

| Recurso  | Acción  | Descripción  |
|:----------------|:-----------------------|:-----------------------|
| instancia   | is.instance.instance.create   | Se ha creado la instancia.   |
| instancia   | is.instance.instance.delete   | Se ha suprimido la instancia.   |
| instancia   | is.instance.instance.update   | Se ha actualizado la instancia.   |
| instancia   | is.instance.action.create   | Se ha creado la acción de instancia.  |
| instancia   | is.instance.action.delete   | Se ha suprimido la acción de instancia pendiente.  |
| instancia   | is.instance.network_interface.floating_ip.create  | La IP flotante se ha asociado a la interfaz de red de la instancia  |
| instancia   | is.instance.network_interface.floating_ip.delete  | La IP flotante se ha desasociado de la interfaz de red de la instancia |
| instancia   | is.instance.volume_attachement.create   | Se ha creado la conexión de volumen de la instancia  |
| instancia   | is.instance.volume_attachement.delete   | Se ha suprimido la conexión de volumen de la instancia  |
| instancia   | is.instance.volume_attachement.update   | Se ha actualizado la conexión de volumen de la instancia  |
| clave  | is.key.key.create   | Se ha creado la clave.  |
| clave  | is.key.key.delete   | Se ha suprimido la clave.  |
| clave  | is.key.key.update   | Se ha actualizado la clave.  |

## Lista de sucesos: recursos de almacenamiento
{: #events-storage}

En la tabla siguiente se listan las acciones relacionadas con los recursos de almacenamiento y la generación de sucesos.

| Recurso  | Acción  | Descripción  |
|:----------------|:-----------------------|:-----------------------|
| volumen  | is.volume.volume.create  |  Se ha creado el volumen.  |
| volumen  | is.volume.volume.update  | Se ha actualizado el volumen.  |
| volumen  | is.volume.volume.delete  | Se ha suprimido el volumen.  |


## Lista de sucesos: equilibradores de carga
{: #events-load-balancers}

En la tabla siguiente se listan las acciones relacionadas con los equilibradores de carga y la generación de sucesos.

| Recurso  | Acción  | Descripción  |
|:----------------|:-----------------------|:-----------------------|
| Equilibrador de carga |  is.load-balancer.load-balancer.create | Se ha creado el equilibrador de carga |
| Equilibrador de carga |  is.load-balancer.load-balancer.update | Se ha actualizado el equilibrador de carga |
| Equilibrador de carga |  is.load-balancer.load-balancer.delete | Se ha suprimido el equilibrador de carga |
| Escucha |  is.load-balancer.load-balancer.listener.create | Se ha creado el escucha |
| Escucha |  is.load-balancer.load-balancer.listener.update | Se ha actualizado el escucha |
| Escucha |  is.load-balancer.load-balancer.listener.delete | Se ha suprimido el escucha |
| Agrupación |  is.load-balancer.load-balancer.pool.create | Se ha creado la agrupación |
| Agrupación |  is.load-balancer.load-balancer.pool.update | Se ha actualizado la agrupación |
| Agrupación |  is.load-balancer.load-balancer.pool.delete | Se ha suprimido la agrupación |
| Miembro |  is.load-balancer.load-balancer.pool.member.create | Se ha creado el miembro |
| Miembro |  is.load-balancer.load-balancer.pool.member.update | Se ha actualizado el miembro |
| Miembro |  is.load-balancer.load-balancer.pool.member.delete | Se ha suprimido el miembro |
| Política |  is.load-balancer.load-balancer.listener.policy.create | Se ha creado la política |
| Política |  is.load-balancer.load-balancer.listener.policy.update | Se ha actualizado la política |
| Política |  is.load-balancer.load-balancer.listener.policy.delete | Se ha suprimido la política |
| Regla |  is.load-balancer.load-balancer.listener.policy.rule.create | Se ha creado la regla |
| Regla |  is.load-balancer.load-balancer.listener.policy.rule.update | Se ha actualizado la regla |
| Regla |  is.load-balancer.load-balancer.listener.policy.rule.delete | Se ha suprimido la regla |

Los sucesos de auditoría del equilibrador de carga se registran en la región `us-south` de {{site.data.keyword.at_full}}. La región donde se suministra el servicio de equilibrador de carga no importa.
{:note}

## Lista de sucesos: VPN
{: #events-vpn}

En la tabla siguiente se listan las acciones relacionadas con las VPN y la generación de sucesos.

| Recurso  | Acción  | Descripción  |
|:----------------|:-----------------------|:-----------------------|
| VPN  | is.vpn.vpn.create   | Se ha creado la VPN   |
| VPN  | is.vpn.vpn.delete   | Se ha suprimido la VPN   |
| VPN  | is.vpn.vpn.update   | Se ha actualizado la VPN   |
| VPN  | is.vpn.vpn.update   | Se ha creado la conexión VPN  |
| VPN  | is.vpn.vpn.update   | Se ha suprimido la conexión VPN  |
| VPN  | is.vpn.vpn.update   | Se ha actualizado la conexión VPN  |


## Ubicaciones con soporte
{: #at-supported-locations}

Actualmente las ubicaciones que disponen de soporte para {{site.data.keyword.at_full}} son Dallas y Frankfurt. En la tabla siguiente se muestra la ubicación de Activity Tracker with LogDNA para cada región de VPC.

| Región de VPC | Ubicación de Activity Tracker with LogDNA |
|--------------|---------------------------------------|
| us-south   | Dallas  |
| jp-tok   | Tokio  |
| eu-de  | Frankfurt   |

## Dónde buscar sucesos
{: #at-events-ui}

Al suministrar una instancia de {{site.data.keyword.at_full}}, los sucesos se reenvían automáticamente a una instancia de servicio de Activity Tracker with LogDNA en su [ubicación soportada](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`).
