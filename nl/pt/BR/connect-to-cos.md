---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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

# Conectando-se ao IBM Cloud Object Storage por meio do VPC
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

Este documento mostra como se conectar ao {{site.data.keyword.cloud}} Object Storage por meio do seu Virtual Private Cloud. As informações do terminal se aplicam especificamente ao {{site.data.keyword.cloud_notm}} Virtual Private Cloud no ambiente da infraestrutura clássica.
{: shortdesc}


## O que é o IBM Cloud Object Storage (COS)?
{: #what-is-ibm-cloud-object-storage-cos}

O {{site.data.keyword.cloud}} Object Storage (COS) é uma plataforma de escala da web que armazena dados não estruturados. Ele fornece confiabilidade, segurança, disponibilidade e recuperação de desastre sem replicação.

As informações armazenadas com o {{site.data.keyword.cloud_notm}} Object Storage são criptografadas e dispersadas em vários locais geográficos. Ele é acessível por meio de uma implementação da API do S3. Esse serviço faz uso das tecnologias de armazenamento distribuído fornecidas pelo IBM Cloud Object Storage System.

O IBM Cloud Object Storage está disponível com três tipos de configurações para resiliência: **Região cruzada**, **Regional** e **Data center único**. 
* O serviço **Região cruzada** fornece durabilidade e disponibilidade mais altas do que o uso de uma única região, no custo de latência um pouco mais alta. Esse serviço está disponível hoje nas seguintes áreas: EUA, UE e Ásia-Pacífico. 
* O serviço **Regional** reverte as trocas. Ele distribui objetos entre várias zonas de disponibilidade em uma única região. Ele está disponível nas seguintes regiões: EUA, UE e Ásia-Pacífico. Se uma determinada região ou zona estiver indisponível, o armazenamento de objeto continuará a funcionar sem impedimento. O serviço **Data center único** distribui objetos entre várias máquinas dentro do mesmo local físico. Verifique esse documento regularmente para obter as regiões disponíveis.

### Terminais diretos do COS para uso com o VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Para enviar uma solicitação da API de REST, deve-se configurar um terminal de destino ou uma URL, e cada local de armazenamento COS deve ter seu próprio conjunto de URLs. Os terminais diretos fornecem aos seus servidores conexões diretas de alta velocidade com os serviços do {{site.data.keyword.cloud_notm}}, que incorrem em nenhum custo de largura de banda incluído. Com o terminal direto do COS, um VPC pode se conectar a um depósito COS localizado em qualquer lugar do mundo. 

Não há encargos para o tráfego de seus VPCs para todos os terminais COS listados nesta página.
{: note}

* Um cliente VPC também tem acesso ao depósito COS sobre o terminal público.
* Um cliente VPC tem acesso apenas à [API de configuração do COS](https://{DomainName}/apidocs/cos/cos-configuration) sobre o terminal público, não o terminal direto. A API de configuração do COS pode ser usada para configurar alguns recursos COS em depósitos, bem como para visualizar metadados do depósito.
* Um cliente VPC não tem acesso a um depósito COS quando o firewall está ativado.

## Como se conectar ao IBM Cloud Object Storage (COS) por meio de um VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Provisione o COS por meio do [catálogo ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}.
2. Crie um depósito COS em uma das regiões listadas na seção a seguir.
3. Use o terminal para se comunicar com o depósito COS.

### Terminais regionais
{: #regional-endpoints}

Os depósitos criados em um terminal regional distribuem dados em três data centers, que são espalhados por uma área metropolitana. Qualquer um desses data centers pode sofrer uma indisponibilidade ou mesmo a destruição, sem afetar a disponibilidade.

| **Região** | **Terminal** |
|------------|-------------------------------|
| Sul dos EUA | ` s3.direct.us-south.cloud-object-storage.appdomain.cloud `|
| Leste dos EUA | ` s3.direct.us-east.cloud-object-storage.appdomain.cloud `|
| U.E. Reino Unido | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| U.E. Alemanha | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| A.P. Austrália | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| A.P. Japão | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### Terminais de região cruzada
{: #cross-region-endpoints}

Os depósitos criados em um terminal de região cruzada distribuem dados em três regiões. Qualquer uma dessas regiões pode sofrer uma indisponibilidade ou mesmo destruição sem afetar a disponibilidade. As solicitações são roteadas para o data center da região mais próxima usando o roteamento do Protocolo de Roteamento de Borda (BGP).

No caso de uma indisponibilidade, as solicitações são roteadas novamente para uma região ativa, automaticamente. Os usuários avançados que desejam gravar sua própria lógica de failover podem fazer isso, enviando solicitações ao terminal da região específica e efetuando bypass do roteamento de BGP.

| **Região cruzada dos EUA** | **Terminal** |
|------------|-------------------------------|
| Região cruzada dos EUA | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Ponto de Acesso de Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de San Jose | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **Região cruzada da UE** | **Terminal** |
|------------|-------------------------------|
| Região cruzada da UE | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Amsterdã | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Frankfurt | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Milão | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **Região Cruzada da Ásia-Pacífico** | **Terminal** |
|------------|-------------------------------|
| Região cruzada da Ásia-Pacífico | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Tóquio | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Seul | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Ponto de acesso de Hong Kong | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### Terminais do data center único
 {: #single-datacenter-endpoints}

Os data centers únicos não são colocalizados com os serviços do IBM Cloud, como o IAM ou o Key Protect, e eles não oferecem resiliência em caso de indisponibilidade ou destruição de um site.

Se uma falha de rede resultar em uma partição, de forma que o data center não consiga acessar uma região do IBM Cloud principal para acesso ao IAM, informações de autenticação e autorização serão lidas por meio de um cache que pode se tornar antigo. Essa ação pode resultar em uma falta de execução de políticas do IAM novas ou alteradas por até 24 horas.
{:important}

| **Região** | **Terminal** |
|------------|-------------------------------|
| Amsterdã, Holanda | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, Índia | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hong Kong | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Austrália | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Cidade do México, México | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Milão, Itália | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montreal, Canadá | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Noruega | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San Jose, EUA | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brasil | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Seul, Coreia do Sul | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Canadá | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
