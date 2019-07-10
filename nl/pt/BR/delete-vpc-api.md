---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Excluindo um VPC usando as APIs de REST
{: #deleting-using-api}

A exclusão de um {{site.data.keyword.cloud}} Virtual Private Cloud usando as APIs de REST segue as mesmas etapas gerais no processo de [exclusão](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) como exclusão usando a [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli).

A seguir, estão as etapas principais no processo:

1. Localize todas as sub-redes no VPC que você deseja excluir.
2. Exclua cada sub-rede no VPC, o que significa:
    - Exclua todos os gateways de VPN na sub-rede.
    - Exclua todos os Balanceadores de Carga na sub-rede.
    - Exclua todas as interfaces de rede (instâncias) na sub-rede.
    - Exclua a sub-rede.
3. Exclua todos os gateways públicos no VPC.
4. Exclua o VPC.

As seções a seguir fornecem algumas chamadas de API de exemplo, usando `curl`, que você pode executar para excluir um VPC.

## Etapa 1: localizar todas as sub-redes no VPC que você deseja excluir
{: #deleting-find-subnets-api}

Antes que um VPC possa ser excluído, cada uma de suas sub-redes deve ser excluída. Obtenha o ID do VPC que você deseja excluir executando o comando a seguir e observando o valor `id`:

Inclua ` | json_pp ` ou ` | jq ` após o comando curl para obter uma sequência JSON legível.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Salve o ID do VPC em uma variável para que possamos usá-lo posteriormente, por exemplo:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Para obter a lista de todas as sub-redes em sua conta, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Observe o valor `vpc` para determinar as sub-redes que precisam ser excluídas. Salve o ID da sub-rede em uma variável para uso posterior, por exemplo:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Se o VPC que você deseja excluir tiver várias sub-redes, repita as etapas para excluir cada sub-rede.
{: important}

## Etapa 2: excluir cada sub-rede no VPC
{: #deleting-subnet-resources-api}

### Exclua todos os gateways de VPN na sub-rede, se houver

Para listar todos os gateways de VPN em sua conta, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Para cada gateway de VPN na sub-rede que você gostaria de excluir, execute o comando a seguir, em que `$vpnid` é o ID do gateway de VPN.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

O status do gateway de VPN deve ser mudado para `deleting` imediatamente, mas ele ainda será retornado ao executar uma consulta de lista. A exclusão de um gateway de VPN pode levar até 30 minutos. É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto esperam que o gateway de VPN seja excluído, mas a sub-rede não pode ser excluída até que o gateway de VPN e todos os outros recursos na sub-rede não apareçam mais nas consultas de lista.

### Exclua todos os Balanceadores de Carga na sub-rede, se houver
{: #deleting-lbs-api}

Para listar todos os Balanceadores de Carga em sua conta, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Para cada Balanceador de Carga na sub-rede que você gostaria de excluir, execute o comando a seguir, em que `$lbid` é o ID do Balanceador de Carga.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

O status do Balanceador de Carga deve ser mudado para `deleting` imediatamente, mas ele ainda será retornado ao executar uma consulta de lista. A exclusão de um Balanceador de Carga pode levar até 30 minutos. É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto aguardam que o Balanceador de Carga seja excluído, mas a sub-rede não pode ser excluída até que o Balanceador de Carga e todos os outros recursos na sub-rede não apareçam mais nas consultas de lista.

### Exclua todas as interfaces de rede na sub-rede, se houver
{: #deleting-nics-api}

Uma instância pode ter várias interfaces de rede que podem pertencer a várias sub-redes do VPC. Antes que uma sub-rede possa ser excluída, qualquer interface de rede na sub-rede deve ser excluída primeiro. A única maneira de excluir uma interface de rede em uma sub-rede é excluir a instância.

Para listar todas as instâncias em sua conta, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Se você estiver excluindo todas as instâncias no VPC, será possível executar o comando a seguir para cada instância no VPC, em que `$vsi` é o ID da instância que você deseja excluir. Quando a instância for excluída, todas as interfaces de rede em outras sub-redes, se houver, serão excluídas automaticamente.

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

O status da instância deve ser mudado para `deleting` imediatamente, mas ele ainda pode ser exibido nos resultados de uma consulta de lista. A exclusão de uma instância pode levar até 30 minutos. É possível solicitar que outros recursos de sub-rede sejam excluídos em paralelo enquanto aguardam que a instância seja excluída, mas a sub-rede não pode ser excluída até que a instância e todos os outros recursos na sub-rede não apareçam mais nas consultas de lista.

Para listar todas as interfaces de rede de uma instância, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Se existir uma interface de rede secundária na sub-rede que você está tentando excluir, será necessário excluir a instância. Uma interface de rede não pode ser excluída da instância sem excluir a instância.

### Excluir a sub-rede
{: #deleting-subnet-api}

Depois que todos os recursos dentro da sub-rede tiverem sido excluídos e não forem retornados ao executar uma consulta de lista, execute o comando a seguir para excluir a sub-rede:

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

O status da sub-rede deve ser mudado para `deleting` imediatamente, mas pode demorar alguns minutos para que a sub-rede seja excluída, de modo que ela não seja mais retornada nas consultas de lista.

## Etapa 3: excluir todos os gateways públicos no VPC, se houver
{: #deleting-pgs-api}

Para listar todos os gateways públicos em sua conta, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Para cada gateway público no VPC que você deseja excluir, execute o comando a seguir para excluir o gateway público, em que `$gateway` é o ID do gateway público.

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## Etapa 4: excluir o VPC
{: #deleting-single-vpc-api}

Depois que todas as sub-redes, gateways de VPN, balanceadores de carga, gateways públicos no VPC tiverem sido excluídos, execute o comando a seguir para excluir o VPC, em que `$vpc` é o ID do VPC que você está excluindo.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

O status do VPC deve ser mudado para `deleting` imediatamente, mas pode demorar alguns minutos para que o VPC seja excluído, de modo que ele não apareça mais nas consultas de lista.
