---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Sobre o Virtual Private Cloud
{: #about}

O {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) é a plataforma IBM Cloud da próxima geração. O VPC permite que você crie seu próprio espaço no IBM Cloud a fim de executar um ambiente isolado dentro da nuvem pública. O VPC dá a você a segurança de uma nuvem particular, com a agilidade e a facilidade de uma nuvem pública.

É possível gerenciar serviços de rede e ativar instâncias, conforme necessário, para suportar os aplicativos essenciais, tolerantes à nuvem e nativos da nuvem. Recursos-chave disponíveis:

* Criar e gerenciar ambientes de aplicativos isolados por meio de uma API
* Definir suas próprias políticas de rede projetadas para segurança e acesso conveniente
* Projetar topologias de rede com BYOIP (Bring Your Own IP)
* Provisionar seus recursos e conectá-los uns aos outros ou isolá-los entre eles
* Abranger múltiplas regiões para recuperação de desastre e resiliência
* Usar zonas de disponibilidade que permitem conexões de baixa latência e de alta velocidade entre regiões, com HA
* Utilize dispositivos de rede de alta velocidade e de armazenamento
* Permitir serviços Always On (plano de controle)
* Fornecer e utilizar serviços principais: IPAM, VPN, firewalls, SSH, DNS e Balanceamento de carga L4
* Configurar outros serviços: Auto-scaling, CDNs, NFV de terceiros

A figura Visão geral do VPC, que aparece a seguir, ilustra os componentes do VPC disponíveis e como eles trabalham juntos para fornecer a você o serviço e a conectividade que talvez sejam necessários. Consulte novamente essa figura à medida que você se aprofunda em tópicos que fornecem mais detalhes sobre cada componente.

![Visão geral do IBM Cloud VPC](images/vpc-experience-simple.svg "Visão geral do IBM Cloud VPC")

As seções a seguir fornecem a você uma visão geral dos recursos disponíveis no IBM Cloud VPC.

## Recursos de rede

O {{site.data.keyword.cloud_notm}} VPC oferece recursos de rede fáceis e abrangentes, incluindo a capacidade de criar vários Virtual Private Clouds em [regiões de várias zonas disponíveis globalmente](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region), sub-redes em zonas diferentes, uma [seleção de intervalo de endereço IP](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), firewalls virtuais ([grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) e [ACLs de rede](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), redes privadas virtuais de site para site ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) e balanceamento de carga ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) com elasticidade.

Um VPC é dividido em sub-redes usando um intervalo de endereços IP privados. No entanto, por padrão, todos os recursos (como VSIs) dentro do mesmo VPC podem se comunicar uns com os outros, independentemente de sua sub-rede. As sub-redes estão contidas em uma única zona e não podem abranger várias zonas, o que ajuda com a segurança, com a redução da latência e com a alta disponibilidade.

Crie vários VPCs e sub-redes facilmente usando os intervalos de prefixo sugeridos e as políticas de segurança de rede pré-configuradas ou projete e defina seus próprios prefixos de endereço e políticas de segurança customizadas.

Os blocos de CIDR 161.26/16 e 166.8/14 são reservados e roteados para cada sub-rede. Leia sobre nossos [Terminais de serviço disponíveis para o IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc). Além disso, o bloco CIDR 10.240/13 é reservado para prefixos de endereço VPC e dividido entre as zonas [de uma maneira predefinida](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes) e não pode ser mudado.
{: important}

### Acesso à Internet

Três opções estão disponíveis para ativar as comunicações de suas instâncias de servidor virtual (VSIs) para a Internet pública:
* Use um gateway público (PGW) para ativar a comunicação com a Internet para todas as instâncias do servidor virtual na sub-rede conectada. Não há encargo para usar um PGW, exceto para a largura da banda usada.
* Use um IP flutuante (FIP) para ativar a comunicação para e de uma única instância do servidor virtual (VSI) na Internet.
* Use um gateway de VPN. Para obter mais informações, consulte a [documentação do VPN for VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)
{: note}

### Acesso clássico

Os usuários de infraestrutura do {{site.data.keyword.cloud_notm}} existentes podem conectar sua infraestrutura clássica, incluindo conectividade de Link Direto, a um VPC por região usando nosso [Acesso Clássico](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

### BYOIP

É possível trazer sua própria variação de endereços IPv4 públicos (BYOIP) para a sua conta do IBM Cloud VPC.

Ao usar o BYOIP, o {{site.data.keyword.cloud_notm}} deve configurar esses endereços IPv4 nos recursos do {{site.data.keyword.cloud_notm}}, que enviarão pacotes para e dos endereços fornecidos. Portanto, como resultado do uso de seu intervalo IPv4 fornecido no {{site.data.keyword.cloud_notm}}, esses endereços IP podem ser expostos para a equipe de suporte da IBM e terceiros.
{: important}

Deve-se usar a API ou a CLI para usar o BYOIP. Esse recurso não está disponível por meio da IU do console do IBM Cloud.
{: note}

### Conectividade global

É possível definir o escopo de seus aplicativos e recursos disponíveis para abranger múltiplas regiões. Usando o [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc), é possível criar conexões privadas com outros projetos e outras partes de suas implementações de nuvem híbrida.

Facilmente escalável e compartilhável, uma rede VPC e recursos podem se estender do seu recurso no local até a sua nuvem.

## Segurança

A segurança é integrada ao seu {{site.data.keyword.cloud_notm}} VPC, com [grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) que atuam como firewalls virtuais para a proteção de nível de instância e com [listas de controle de acesso à rede](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACLs) para proteção de nível de sub-rede.

## Alta disponibilidade

O {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) fornece o isolamento lógico de seus aplicativos de outras redes em todas as regiões, enquanto fornece escalabilidade e segurança. Use [Balanceadores de Carga](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc) para distribuir seu tráfego de rede por meio de um conjunto de destinos para melhorar o desempenho e a HA. Os Load Balancers também monitoram o funcionamento de seus aplicativos e serviços. É possível configurar um balanceador de carga para distribuir o tráfego de aplicativo recebido entre as instâncias em uma única zona ou em múltiplas zonas dentro de uma região.

## Recursos de cálculo

Use o [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) para provisionar recursos de cálculo escaláveis no IBM Cloud. Crie instâncias do servidor virtual (VSIs) rapidamente, usando os [perfis](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles) predefinidos otimizados para suas cargas de trabalho específicas. Ao provisionar as instâncias multihomed, [multi-vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options), escolha no banco de [imagens](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) suportado ou faça upload de uma imagem customizada.

O {{site.data.keyword.cloud_notm}} VPC inclui uma camada de orquestração de rede que elimina o limite do pod de instâncias do servidor virtual, criando capacidade infinita para instâncias de ajuste de escala. A camada de orquestração de rede manipula toda a rede para todas as instâncias de servidor virtual que estão dentro de um IBM Cloud VPC entre regiões e zonas. Com os recursos de rede definidos pelo software que o {{site.data.keyword.cloud_notm}} VPC fornece, você tem mais opções para VPNs, LBaaS, instâncias multi-vNIC e tamanhos de sub-rede maiores.

## Recursos de armazenamento

Ao provisionar uma instância do {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud, um volume de armazenamento de bloco IOPS (3 IOPS/GB) de 100 GB, de propósito geral, é criado automaticamente, como um volume de inicialização primário e conectado à instância. É possível criar volumes de armazenamento de bloco ao provisionar uma instância do servidor virtual em uma rede VPC ou criar novos volumes independentes do ciclo de vida VSI.

Ao provisionar o armazenamento de bloco adicional para seu VPC, é possível selecionar uma [camada IOPS](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers) para seu volume de armazenamento de bloco ou especificar um [perfil de IOPS customizado](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom).

## Projetado para cargas de trabalho nativas de nuvem e híbridas

Um VPC é ideal para cargas de trabalho nativas de nuvem e para vincular sua infraestrutura existente ao {{site.data.keyword.cloud_notm}}. É possível criar a melhor nuvem para cargas de trabalho, como cálculos cognitivos, IA e Aprendizado de máquina.

Aqui estão mais maneiras que o VPC suporta suas cargas de trabalho híbridas, tolerantes à nuvem e nativa de nuvem:

* Balanceamento de carga com auto-scaling horizontal
* Monitoramento e verificações de funcionamento
* Suporte para GPUs
* Provisionamento VSI com grupos de afinidade e antiafinidade

## Saiba mais

* [**Terminologia do VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**Segurança do VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**Regiões do VPC e sub-redes disponíveis**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**Criando e gerenciando recursos de rede no VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**Criando e gerenciando instâncias do servidor virtual no VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**Criando e gerenciando armazenamento de bloco no VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
