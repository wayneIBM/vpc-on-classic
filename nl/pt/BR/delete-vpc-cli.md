---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Excluindo um VPC usando a CLI do IBM Cloud
{: #deleting-using-cli}

Este tópico fornece exemplos de como excluir recursos de seu {{site.data.keyword.cloud}} Virtual Private Cloud, para cada recurso do VPC, na ordem sugerida. 

Aqui estão alguns fatos importantes para se lembrar sobre a exclusão:

* Um VPC não pode ser excluído até que esteja vazio. 
* Todos os recursos contidos devem ser excluídos com êxito antes que o recurso pai possa ser excluído. 
* Se o recurso estiver com uma exclusão pendente, ele mostrará um status `deleting` quando visualizado em consultas de lista. 
* A maioria das solicitações de exclusão são _assíncronas_, o que significa que o recurso pode mostrar um status **deleting** em consultas de lista quando ele ainda não tiver sido excluído completamente. 
* Deve-se pesquisar para se certificar de que o recurso filho não esteja mais na visualização de lista antes de tentar excluir o recurso pai. 

Se o status do recurso for mudado de `deleting` para `failed`, será possível tentar excluir o recurso novamente. Se não for possível excluir um recurso no status `failed`, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Etapa 1: localizar todas as sub-redes dentro do VPC que você deseja excluir
{: #deleting-find-subnets-cli}

Antes que um VPC possa ser excluído, cada uma de suas sub-redes deve ser excluída. Obtenha o ID do VPC que você deseja excluir executando o comando a seguir e observando o valor `ID`:

```
ibmcloud is vpcs
```
{: pre}

Salve o ID do VPC em uma variável para que possamos usá-lo posteriormente, por exemplo:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Para obter a lista de todas as sub-redes em sua conta, execute o comando a seguir:

```
ibmcloud is subnets
```
{: pre}

Observe o valor `VPC` para determinar as sub-redes que precisam ser excluídas. Salve o ID da sub-rede em uma variável para que possamos usá-lo posteriormente, por exemplo:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Se o VPC que você deseja excluir tiver várias sub-redes, repita estas etapas para excluir cada sub-rede.
{: important}

## Etapa 2: excluir cada sub-rede no VPC
{: #deleting-subnet-resources-cli}

### Exclua todos os gateways de VPN na sub-rede, se houver

Para listar todos os gateways de VPN em sua conta, execute o comando a seguir:

```
ibmcloud is vpn-gateways
```
{: pre}

Para cada gateway de VPN na sub-rede que você gostaria de excluir, execute o comando a seguir, em que `$vpnid` é o ID do gateway de VPN.

```
ibmcloud is vpn-gateway-delete $vpnid
```

O status do gateway de VPN deve ser mudado para `deleting` imediatamente, mas ele ainda aparecerá em um resultado da consulta de lista. A exclusão de um gateway de VPN pode levar até 30 minutos.
{: note}

É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto aguardam que o gateway de VPN seja excluído. No entanto, a sub-rede não pode ser excluída até que o gateway de VPN e todos os outros recursos na sub-rede não apareçam mais nos resultados da consulta de lista.

### Exclua todos os Balanceadores de Carga na sub-rede, se houver
{: #deleting-lbs-cli}

Para listar todos os Balanceadores de Carga em sua conta, execute o comando a seguir:

```
ibmcloud is load-balancers
```
{: pre}

Para cada Balanceador de Carga na sub-rede que você gostaria de excluir, execute o comando a seguir, em que `$lbid` é o ID do Balanceador de Carga.

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

O status do Balanceador de Carga deve ser mudado para `deleting` imediatamente, mas ele ainda será mostrado em um resultado de consulta de lista. A exclusão de um Balanceador de Carga pode levar até 30 minutos.
{: note}

É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto aguardam que o Balanceador de Carga seja excluído. No entanto, a sub-rede não pode ser excluída até que o Balanceador de Carga e todos os outros recursos na sub-rede não apareçam mais nos resultados da consulta da lista.

### Exclua todas as interfaces de rede na sub-rede, se houver
{: #deleting-nics-cli}

Uma instância pode ter várias interfaces de rede que podem pertencer a várias sub-redes do VPC. Antes que uma sub-rede possa ser excluída, qualquer interface de rede na sub-rede deve ser excluída primeiro. 

A única maneira de excluir uma interface de rede em uma sub-rede é excluir a instância.
{: note}

Para listar todas as instâncias em sua conta, execute o comando a seguir:

```
ibmcloud is instances
```
{: pre}

Se você estiver excluindo todas as instâncias no VPC, será possível executar o comando a seguir para cada instância no VPC, em que `$vsi` é o ID da instância que você deseja excluir. Quando a instância for excluída, todas as interfaces de rede em outras sub-redes, se houver, serão excluídas automaticamente.

```
ibmcloud is instance-delete $vsi
```
{: pre}

O status da instância deve ser mudado para `deleting` imediatamente, mas ele ainda aparecerá em um resultado da consulta de lista. A exclusão de uma instância pode levar até 30 minutos.
{: note}

É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto aguardam que a instância seja excluída. No entanto, a sub-rede não pode ser excluída até que a instância e todos os outros recursos na sub-rede não apareçam mais nos resultados da consulta da lista.

Para listar todas as interfaces de rede de uma instância, execute o comando a seguir:

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

Se existir uma interface de rede secundária na sub-rede que você está tentando excluir, a instância deverá ser excluída. Uma interface de rede não pode ser excluída da instância sem excluir a instância.

### Excluir a sub-rede
{: #deleting-subnet-cli}

Depois que todos os recursos dentro da sub-rede tiverem sido excluídos, o que significa que eles não são retornados em um resultado de consulta de lista, execute o comando a seguir para excluir a sub-rede:

```
ibmcloud is subnet-delete $subnet
```
{: pre}

O status da sub-rede deve ser mudado para `deleting` imediatamente, mas pode levar alguns minutos até que a sub-rede seja realmente excluída e que não apareça mais nos resultados da consulta da lista.

## Etapa 3: excluir todos os gateways públicos no VPC, se houver
{: #deleting-pgs-cli}

Para listar todos os gateways públicos em sua conta, execute o comando a seguir:

```
ibmcloud is public-gateways
```
{: pre}

Para cada gateway público no VPC que você deseja excluir, execute o comando a seguir para excluir o gateway público, em que `$gateway` é o ID do gateway público.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Etapa 4: excluir o VPC
{: #deleting-single-vpc-cli}

Depois que todas as sub-redes, gateways de VPN, balanceadores de carga, gateways públicos no VPC tiverem sido excluídos, execute o comando a seguir para excluir o VPC, em que `$vpc` é o ID do VPC que você está excluindo.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

O status do VPC deve ser mudado para `deleting` imediatamente, mas pode demorar alguns minutos até que o VPC seja realmente excluído e não apareça mais nos resultados da consulta da lista.
