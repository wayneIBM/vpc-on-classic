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

# Terminais em serviço disponíveis para o IBM Cloud VPC
{: #service-endpoints-available-for-ibm-cloud-vpc}

Quando você estiver pronto para executar cargas de trabalho, será possível acessar dois tipos de terminais: Infraestrutura como um Serviço (IaaS) e Terminais de serviço de nuvem (CSE). Os terminais IaaS são pré-provisionados e prontos para uso; no entanto, os CSEs devem ser provisionados separadamente antes de poderem ser usados.

Terminais para esses serviços usam _endereços roteáveis_, ou seja, endereços fora dos especificados no RFC 1918 e pode parecer que esses endereços estão se comunicando por meio da Internet pública, mas o tráfego para e desses terminais não deixa a nuvem. Portanto, esse tráfego evita os encargos de largura de banda associados ao tráfego que sai da nuvem e vai para a Internet pública.

## Terminais de Infraestrutura como Serviço (IaaS)
{: #infrastructure-as-a-service-iaas-endpoints}

Os serviços de infraestrutura estão disponíveis usando determinados nomes de DNS por meio do domínio `adn.networklayer.com` e eles resolvem para endereços `161.26.0.0/16`.

Os serviços que podem ser incluídos incluem:

* Resolvedores de DNS
* Espelhos Ubuntu APT (Advanced Packaging Tool)
* Cloud Object Storage
* NTP

## Terminais do resolvedor DNS (Sistema de Nomes de Domínio)
{: #dns-domain-name-system-resolver-endpoints}

Os resolvedores de DNS usam o endereço IP, em vez de nomes. Para os terminais de serviço de nuvem compartilhados, os endereços do servidor DNS `161.26.0.10` e `161.26.0.11` devem ser usados.

## Espelhos Ubuntu APT
{: #ubuntu-apt-mirrors}

Os espelhos APT para atualizar imagens do Ubuntu estão disponíveis em `mirrors.adn.networklayer.com`, que deve ser resolvido para `161.26.0.6`. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

Por exemplo, para acessar um terminal privado para um serviço de armazenamento de objetos na rede privada do IBM Cloud, substitua a parte "domain" da URL por `cloud-object-storage.appdomain.cloud`. Para acessar um depósito COS criado em outra região, use o terminal para o depósito, não o terminal da região na qual a VSI foi criada.

## Servidores Network Time Protocol (NTP)
{: #network-time-protocol-ntp-servers}

Um servidor NTP está disponível em `time.adn.networklayer.com`, que deve ser resolvido para `161.26.0.6`.

## Terminais de serviço de nuvem
{: #cloud-service-endpoints}

Os terminais de serviço de nuvem são serviços fornecidos por outros usuários de nuvem. Eles estarão disponíveis em breve por meio de nomes DNS no domínio `cloud.ibm.com`. Eles resolvem para os endereços `166.8.0.0/14`.
