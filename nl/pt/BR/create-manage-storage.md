---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

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

# Criando e gerenciando armazenamento de bloco no VPC
{: #creating-and-managing-storage-in-vpc}

O {{site.data.keyword.cloud}} Virtual Private Cloud fornece um volume de armazenamento de inicialização e permite volumes de armazenamento de bloco adicionais, que são armazenamentos de dados de alto desempenho montados por hypervisor para as instâncias do servidor virtual (VSIs). A infraestrutura do VPC oferece ajuste de escala rápido do armazenamento em várias regiões e zonas, com desempenho e segurança extras.

## Resumo das características
{: #summary-of-characteristics}

Quando você provisiona uma instância do {{site.data.keyword.vsi_is_full}}, um volume de armazenamento de bloco IOPS de propósito geral (3 IOPS/GB) de 100 GB é criado automaticamente como um volume de inicialização primário, conectado à VSI. Durante o provisionamento, é possível renomear o volume de inicialização e mudar o tamanho do volume.
{:shortdesc}

* É possível criar volumes de armazenamento de bloco sempre que provisionar uma instância do servidor virtual em uma rede do VPC.  
* É possível criar novos volumes, independentemente do ciclo de vida da VSI e posteriormente anexar esses volumes a uma VSI.
* É possível criar volumes de dados secundários, que são anexados à VSI, automaticamente. 
* Os volumes de dados persistem quando separados da VSI. Portanto, é possível anexar um volume a uma nova instância, posteriormente. 
* É possível especificar se os volumes de dados são excluídos automaticamente quando você exclui uma VSI.  
* Os volumes de inicialização são excluídos quando você exclui suas VSIs.
* Por padrão, os volumes de inicialização e de dados são criptografados com a criptografia gerenciada pela IBM. 
* É possível criptografar seus volumes usando suas próprias chaves de criptografia, durante o provisionamento de VSI ou ao criar um volume independente.


## Volumes de armazenamento de bloco
{: #block-storage-volumes}

Informações mais completas sobre como trabalhar com volumes de armazenamento de bloco e VPC estão disponíveis em nossa [Documentação do Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started).

Para obter informações gerais sobre volumes de armazenamento de bloco para o VPC, consulte [Sobre o Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

Para começar a criar volumes independentes do provisionamento de VSI, consulte [Introdução ao {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started).



## Melhores práticas para criar e nomear seus volumes de armazenamento de bloco de VPC:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Determine os requisitos de armazenamento antes de provisionar um volume. Permita a capacidade adequada e o desempenho do IOPS.
* Decida se um perfil IOPS predefinido atende melhor às suas necessidades de capacidade e desempenho ou se a especificação de configurações customizadas é melhor.
* Decida se você deseja criar volumes de dados secundários durante o provisionamento da instância do servidor virtual. Esses volumes são anexados automaticamente à instância.
* Decida se você deseja que o volume anexado a uma VSI seja excluído automaticamente quando você excluir a VSI.
* Ao criptografar volumes com sua própria chave de criptografia, assegure-se de que a chave seja válida, que você tenha autorização para usar a chave e que ela possa ser usada em sua região atual.
* Assegure-se de que você esteja nomeando TODOS os volumes no momento da provisão, incluindo o volume de inicialização.
* Assegure-se de que você esteja nomeando TODOS os anexos de volume no momento de anexar.
* Cada volume deve ter um nome distinto dentro de uma região dentro de uma conta.

Será possível reutilizar o nome de um volume depois que esse volume tiver sido excluído. No entanto, esteja ciente de que a reutilização de um nome de volume pode causar confusão para propósitos de faturamento.
{:note}
