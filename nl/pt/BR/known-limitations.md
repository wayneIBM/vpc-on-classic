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

# Limitações Conhecidas
{: #known-limitations}

Este documento contém um resumo de recursos e APIs que não são suportados, recursos e casos de uso ainda não suportados, problemas conhecidos na liberação atual e uma lista de recursos oferecidos apenas como serviços Beta. As limitações conhecidas podem ser mudadas à medida que incluímos recursos no {{site.data.keyword.vpc_full}} (VPC), portanto, sintam-se livres para conferir esse documento de tempos em tempos.

## Resumo dos recursos não suportados
{: #summary-of-features-not-supported}

* O acesso de Link Direto ao VPC é suportado por meio de [**Acesso Clássico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) apenas.
* Embora um subconjunto de Terminais de serviço privado esteja disponível para o VPC, nem todos os terminais estão disponíveis. Consulte [Terminais de serviço disponíveis para o IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) para serviços que estão disponíveis.
* Salvar e restaurar imagens não é suportado.
* Os {{site.data.keyword.cloud_notm}} VPCs são regionais, portanto, um VPC de uma região não pode se conectar a um VPC em outra região, a menos que eles estejam ativados para "acesso clássico" ou conectadas por meio de uma conexão VPN. Consulte [**Acesso Clássico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Recursos e casos de uso não suportados ainda
{: #features-and-use-cases-not-yet-supported}

Esta seção lista mais detalhes de recursos não suportados e casos de uso, categorizados de acordo com seu serviço do {{site.data.keyword.cloud_notm}}.

### Nuvem Virtual Virtual
{: vpc-unsupported-features-and-use-cases}

* O {{site.data.keyword.vpc_short}} não suporta domínios de multicast ou de transmissão.
* O {{site.data.keyword.vpc_short}} não fornece criptografia de ponta a ponta.
* Não é possível fazer peer de um VPC com outros VPCs.
* Os Terminais de serviço de nuvem não são suportados pelo VPC. Consulte [Terminais de serviço disponíveis para o IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) para serviços que estão disponíveis.

### Compute
{: VSI-unsupported-features-and-use-cases}

* As instâncias de servidor virtual (VSIs) ou os servidores bare metal existentes do {{site.data.keyword.cloud_notm}} não podem ser migrados para um VPC.
* Os servidores bare metal não podem ser provisionados em um VPC.

### Rede
{: network-unsupported-features-and-use-cases}

* As rotas customizadas não podem ser incluídas em um VPC. Todas as sub-redes em um VPC podem se comunicar umas com as outras por padrão.
* Uma sub-rede não pode estar em múltiplas zonas.
* Uma sub-rede não pode ser movida de uma zona para outra.
* As sub-redes não podem ser redimensionadas após serem criadas.
* Os recursos do {{site.data.keyword.cloud_notm}} Classic não podem estar na mesma sub-rede em um VPC. A conectividade entre recursos clássicos e um VPC em sua conta é possível usando [**Acesso Clássico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* IPv6 não é suportado.
* Não é possível usar seu próprio IP público como um IP Flutuante. O conjunto de IPs Flutuantes é fornecido pela IBM.
* Um IP Flutuante é ligado a uma zona. Por exemplo, um IP Flutuante reservado de um conjunto na Zona 1 não pode ser designado a uma VSI na Zona 2 no mesmo VPC.
* Os endereços IP privados não podem ser movidos entre VSIs.
* As interfaces de rede não podem ser movidas entre VSIs.
* Não é possível designar o endereço IP específico da VSI. É possível especificar somente o intervalo de IP para a sub-rede. Quando você anexa uma VSI a uma sub-rede, o sistema designa um IP privado dessa sub-rede.
* O {{site.data.keyword.vpc_short}} vem com sua própria Rede como uma Ferramenta de serviço, como LBaaS, ACLs e grupos de segurança. Não é possível usar seus próprios dispositivos de rede (por exemplo, um Gateway Vyatta ou outro balanceador de carga) para controlar qualquer recurso no VPC. Similarmente, não é possível usar seu próprio balanceador de carga ou serviços de grupos de segurança na infraestrutura clássica do {{site.data.keyword.cloud_notm}} para controlar recursos no {{site.data.keyword.vpc_short}}.
* O gateway público não permite que o tráfego seja iniciado da Internet para uma VSI no IBM Cloud VPC. Para esse propósito, um IP Flutuante deve ser usado.
* A ACL é stateless, portanto, o tráfego de retorno deve ser permitido explicitamente nas regras de ACL.

## Problemas conhecidos nesta liberação
{: #known-issues-in-this-release}

O {{site.data.keyword.block_storage_is_short}} poderá não validar com precisão o nome do volume quanto à exclusividade quando todas as condições a seguir forem atendidas:

* A solicitação de provisão é simultânea
* O volume tem o mesmo nome
* O volume está na mesma região

## Serviços Beta disponíveis

O peering com segurança para um dispositivo está disponível como um serviço Beta. A documentação para peering com vários tipos de dispositivos está disponível:

* [Peer Vyatta remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [Peer StrongSwan remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [Peer FortiGate remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [Peer Juniper vSRX remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [Peer Cisco ASAv remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
