---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

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

# Criando um VPC e uma região diferente
{: #creating-a-vpc-in-a-different-region}

Uma região é uma localização geográfica específica na qual é possível implementar apps, serviços e outros recursos do {{site.data.keyword.cloud}}. As regiões consistem em uma ou mais zonas, que são data centers físicos que hospedam os recursos de computação, rede e armazenamento, com resfriamento e energia relacionados, para serviços e aplicativos do host. As zonas são isoladas umas das outras, o que garante que não exista um ponto único de falha compartilhado dentro de uma região.

O serviço IBM Cloud VPC é regional. Para permitir a recuperação de desastre (DR), deve-se fornecer a capacidade de recuperação usando o failover para uma região alternativa, estabelecendo um VPC em uma de nossas outras regiões.
{: note}

O Virtual Private Cloud está sendo implementado em todas as regiões do {{site.data.keyword.cloud}} em fases.

|   Localização     | Região | Terminal de API | Status |
| ------- | :------: | :------: |:------: |
| Dallas | para sul | ` us-south.iaas.cloud.ibm.com `| Disponíveis |
| Frankfurt | eu-de | `eu-de.iaas.cloud.ibm.com`| Disponíveis |
| Tóquio | jp-tok | ` jp-tok.iaas.cloud.ibm.com `| Disponíveis |
| Londres | eu-gb | ` eu-gb.iaas.cloud.ibm.com `| Em breve |
| Sydney | au-syd | ` au-syd.iaas.cloud.ibm.com `| Em breve |
| Washington DC | nós-Leste | ` us-east.iaas.cloud.ibm.com `| Em breve |

O terminal da API Regional (VPC) é configurado automaticamente pela CLI do IBM Cloud quando você efetua login em uma região específica.
{: note}

## Efetuar login em uma região específica usando a CLI
{: #log-in-to-a-specific-region-using-the-cli}

Quando você efetua login no IBM Cloud, é possível especificar uma região ou escolhê-la posteriormente. Por exemplo, para efetuar login no terminal de API global na região Dallas (`us-south`) diretamente, execute os comandos a seguir, que variam conforme você tem uma conta federada (SSO) ou uma conta não federada.

Para uma conta federada,

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

Para uma conta não federada,

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

Para escolher a região posteriormente, não especifique o parâmetro `-r<region>` e a CLI solicitará que você escolha uma região.

Saída de exemplo:

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567 2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd 2. jp-tok
3. eu-de 4. eu-gb 5. us-south 6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## Alternar regiões usando a CLI
{: #switch-regions-using-the-cli}

Para obter o status mais recente das regiões do VPC, execute o comando a seguir:

```
ibmcloud são regiões
```
{: pre}

Para alternar para uma região diferente, execute o comando `ibmcloud target -r <region>`. Por exemplo, para alternar para a região de Frankfurt, execute o comando a seguir:

```
ibmcloud target -r eu-de
```
{: pre}

Para verificar seu local atual, execute o comando a seguir:

```
ibmcloud target
```
{: pre}

## Alternar regiões usando a API  
{: #switch-regions-using-the-api}

Para interagir com a API do VPC Regional usando REST, direcione sua solicitação para o terminal de API associado à região na qual você deseja criar recursos. O terminal de API da região é mostrado na tabela anterior. Também é possível localizar os terminais associados às regiões ao executar o comando a seguir:

```
ibmcloud são regiões
```
{: pre}


Por exemplo, para obter a lista de VPCs na região `us-south`, execute o comando a seguir:

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## Obter zonas
{: #get-zones}

Para obter a lista de zonas disponíveis para cada região, execute o comando `ibmcloud is zones<region>`. Por exemplo, para obter a lista de zonas na região `us-south`, execute o comando a seguir:

```
ibmcloud is zones us-south
```
{: pre}
