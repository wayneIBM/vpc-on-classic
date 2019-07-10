---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-07"
keywords: quota, resource, classic, access, gateway, address, prefix, VSI, vNIC, floating, SSH, key, security, group, rule, remote, peer, ACL, region, ingress, egress, VPN, policies, load balancer, listener, pool, per

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

# Cuotas
{: #quotas}

En este documento se describen las cuotas y los límites de su {{site.data.keyword.cloud}} Virtual Private Cloud y los recursos disponibles en la misma.

## Cuotas de VPC
{: #vpc-quotas}

Las cuentas tienen las cuotas siguientes:

|   Recurso     | Número máximo |
| ------- | :------: |
| Nubes privadas virtuales | 5 por región|
| VPC con acceso clásico | 1 por región |
| Pasarelas públicas (PGW) | 1 por VPC por zona |
| Prefijos de dirección | 5 por VPC por zona |

## Cuotas de subred
{: #subnet-quotas}

|   Recurso     | Número máximo |
| ------- | :------: |
| Subredes | 15 por nube privada virtual |


## Cuotas de VSI
{: #vsi-quotas}

|   Recurso     | Número máximo |
| ------- | :------: |
| Instancias de servidor virtual (VSI) | 100 por cuenta de forma predeterminada |
| vNICs por VSI | 5 por VSI |
| Direcciones IP flotantes | 1 por VSI |
| Claves SSH | 100 por cuenta |


## Cuotas de grupos de seguridad
{: #security-groups-quotas}

Existen cuotas basadas en cuenta (límites) para los grupos de seguridad y las reglas asociadas.

|Recurso|Cuota|
|--------|-----|
|Grupos de seguridad|5 por interfaz de red|
|Grupos de seguridad|500 por cuenta|
|Reglas|50 por grupo de seguridad|
|Reglas remotas |5 por grupo de seguridad|
|Interfaces de red|100 por grupo de seguridad|

## Cuotas de ACL
{: #acl-quotas}

|Recurso|Cuota|
|--------|-----|
|ACL| 30 por región |
|Reglas de entrada|20 por ACL |
|Reglas de salida |20 por ACL |

## Cuotas de VPN
{: #vpn-quotas}

Estas son las limitaciones actuales de recursos de VPN por cuenta:

|Recurso|Cuota|
|--------|-----|
| Pasarelas VPN| 20 por cuenta |
| Pasarelas VPN | 3 por zona |
| Conexiones de VPN | 10 por pasarela VPN |
| Políticas de IKE | 20 por cuenta |
| Políticas de IPSec | 20 por cuenta |
| Subredes iguales en una sola pasarela VPN | 50 entre todas las conexiones VPN|
| Subredes iguales  | 15 en una sola conexión VPN|
| Subredes locales en una sola pasarela VPN | 50 entre todas las conexiones VPN|
| Subredes locales |  15 en una sola conexión VPN |

## Cuotas de equilibrador de carga
{: #load-balancer-quotas}

Estas son las cuotas actuales de recursos del equilibrador de carga:

|Recurso|Cuota|
|--------|-----|
| Equilibradores de carga | 20 por cuenta |
| Escuchas | 10 por equilibrador de carga |
| Agrupaciones | 10 por equilibrador de carga |
| Miembros | 50 por agrupación |

## Cuotas de volumen secundario
{: #secondary-volume-quotas}

| Recurso | Cuota |
|--------|----- |
| Volúmenes secundarios por instancia, al crear una instancia |  se pueden solicitar 4 volúmenes secundarios |
| Volúmenes secundarios por instancia, para instancias existentes con menos de 4 núcleos | 4 volúmenes secundarios |
| Volúmenes secundarios por instancia, para instancias existentes con 4 núcleos o más | hasta 12 volúmenes secundarios |


