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

# Cotas
{: #quotas}

Este documento abrange cotas e limites para seu {{site.data.keyword.cloud}} Virtual Private Cloud e os recursos disponíveis dentro dele.

## Cotas VPC
{: #vpc-quotas}

As contas têm as cotas a seguir:

|   Recurso     | Número Máximo |
| ------- | :------: |
| Nuvens Virtuais Privadas | 5 por região|
| VPCs com Acesso Clássico | 1 por região |
| Gateways Públicos (PGWs) | 1 por VPC por zona |
| Prefixos de endereço | 5 por VPC por zona |

## Cotas de sub-
{: #subnet-quotas}

|   Recurso     | Número Máximo |
| ------- | :------: |
| Subnets | 15 por Nuvem Virtual Virtual |


## Cotas VSI
{: #vsi-quotas}

|   Recurso     | Número Máximo |
| ------- | :------: |
| Instâncias de Servidor Virtual (VSIs) | 100 por conta, por padrão |
| vNICs por VSI | 5 por VSI |
| Endereços IP flutuantes | 1 por VSI |
| Chaves SSH | 100 por conta |


## Cotas de grupos de
{: #security-groups-quotas}

Algumas cotas baseadas em conta (limites) existem para grupos de segurança e suas regras associadas.

|Recurso|Cota|
|--------|-----|
|Grupos de segurança|5 por interface de rede|
|Grupos de segurança|500 por conta|
|Regras|50 por grupo de segurança|
|Regras remotas |5 por grupo de segurança|
|Interfaces de Rede|100 por grupo de segurança|

## cotas da ACL
{: #acl-quotas}

|Recurso|Cota|
|--------|-----|
|ACLs| 30 por região |
|Regras de Ingresso|20 por ACL |
|Regras de Egresso |20 por ACL |

## cotas VPN
{: #vpn-quotas}

Aqui estão as limitações de recurso VPN atual por conta:

|Recurso|Cota|
|--------|-----|
| Gateways VPN| 20 por conta |
| Gateways VPN | 3 por zona |
| Conexões VPN | 10 por gateway VPN |
| Políticas IKE | 20 por conta |
| Políticas IPSec | 20 por conta |
| Sub-redes de peer em qualquer gateway VPN único | 50 em todas as conexões VPN|
| Sub-redes de Peer  | 15 em qualquer conexão VPN única|
| Sub-redes locais em qualquer gateway VPN único | 50 em todas as conexões VPN|
| Sub-redes locais |  15 em qualquer conexão VPN única |

## Cotas do balanceador de carga
{: #load-balancer-quotas}

Aqui estão as cotas de recurso do balanceador de carga atual:

|Recurso|Cota|
|--------|-----|
| Load Balancers | 20 por conta |
| Listeners | 10 por Balanceador de Carga |
| Conjuntos | 10 por Balanceador de Carga |
| Membros | 50 por Conjunto |

## Cotas de volume secundário
{: #secondary-volume-quotas}

| Recurso | Cota |
|--------|----- |
| Volumes secundários por instância, ao criar uma instância |  4 volumes secundários podem ser solicitados |
| Volumes secundários por instância, para instâncias existentes com menos de 4 núcleos | 4 volumes secundários |
| Volumes secundários por instância, para instâncias existentes com 4 núcleos ou mais | Até 12 volumes secundários |


