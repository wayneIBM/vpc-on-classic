---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# Glossário do VPC
{: #vpc-glossary}

A terminologia a seguir é normalmente usada e é específica para o {{site.data.keyword.cloud}} Virtual Private Cloud. Veja [Termos do glossário para o IBM Cloud](/docs/overview/glossary?topic=overview-glossary) para obter termos mais amplos.

## Lista de Controle de Acesso
{: #access-control-list}

Uma Lista de Controle de Acesso (ACL) fornece controles de segurança para o tráfego de entrada e de saída para uma [sub-rede](#subnet). Uma ACL é stateless. Cada ACL consiste em regras, com base em um _IP de origem_, _porta de origem_, _IP de destino_, _porta de destino_ e _protocolo_.

No IBM Cloud VPC, cada sub-rede é criada com uma ACL padrão, que permite o tráfego de entrada e de saída, mas os clientes podem criar ACLs customizadas. Apenas uma ACL é anexada a uma sub-rede a qualquer momento, mas uma ACL pode ser anexada a múltiplas sub-redes.

Às vezes, também chamada de _ACL de rede_ ou NACL.

## IP Flutuante
{: #floating-ip}

IP Flutuante é um método para fornecer acesso de entrada e de saída à Internet para recursos do VPC, como instâncias, um balanceador de carga ou um túnel VPN, usando _Endereços IP flutuantes_ designados por meio de um conjunto.

Endereços IP flutuantes são endereços IP públicos que são fornecidos pelo sistema por meio do conjunto preexistente. Não é possível trazer seus próprios endereços IP públicos. Os endereços IP flutuantes são atingíveis por meio da Internet e podem ser associados a uma instância (por exemplo, uma VSI, um balanceador de carga ou um gateway VPN) por meio de uma vNIC.

É possível reservar um endereço IP Flutuante por meio do conjunto de endereços IP Flutuantes disponíveis fornecidos pela IBM e é possível associá-lo ou desassociá-lo de qualquer instância no mesmo Virtual Private Cloud. O IP Flutuante faz uso de [NAT](#nat) 1 para 1 para permitir que um servidor se comunique com a Internet pública e também com uma sub-rede privada dentro de seu ambiente de nuvem.

## Geografia
{: #geography}

Em um VPC, a designação de geografia fornece informações sobre a região e a zona dentro da qual o VPC e seus recursos associados são implementados.

## Imagem
{: #image}

As informações necessárias para ativar uma instância de servidor virtual ou _instância_. Geralmente, é uma imagem de captura instantânea de um sistema operacional comercialmente disponível, usado para inicialização.

## Roteador implícito
{: #implicit-router}

A conectividade de rede inerente entre todas as sub-redes criadas em um VPC.

## Ocorrência
{: #instance}

Um nome abreviado para um servidor virtual, ou VSI, que é executado em um VPC.

## LBaaS
{: #lbaas}

O Load Balancer as a Service (LBaaS) fornece a capacidade de distribuir o tráfego em uma alocação balanceada entre as instâncias em um VPC.

## NAT
{: #nat}

A Conversão de Endereço de Rede (NAT) é um método de endereçamento descrito em [RFC 1631 de Internet ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://tools.ietf.org/html/rfc1631){: new_window} para que um endereço IP possa ser usado para se comunicar com vários outros endereços IP, como aqueles em uma sub-rede privada, essencialmente por meio de uma tabela de consulta. A NAT tem dois tipos principais: NAT 1 para 1 e [NAT Muitos para 1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}. O NAT foi originalmente planejado como uma maneira de ampliar a vida útil de endereços IP IPv4, mas agora ele é comumente usado em ambientes de nuvem como uma maneira de criar comunicação com sub-redes privadas.

## Perfil
{: #profile}

Um Perfil é uma combinação popular de vCPU e RAM que pode ser instanciado rapidamente para iniciar uma instância de servidor virtual (VSI). Três famílias de perfis são suportadas: Balanceado, Cálculo e Memória.


## Gateway público
{: #public-gateway}

Um Gateway Público (PGW) permite o acesso somente de saída para uma sub-rede (com todas as VSIs anexadas à sub-rede) para se conectar à Internet. Observe que as sub-redes são privadas por padrão; no entanto, opcionalmente, é possível criar um PGW e conectar uma sub-rede a ele. Depois que uma sub-rede é conectada ao PGW, todas as VSIs nessa sub-rede podem se conectar à Internet.

O PGW usa o NAT Muitos para 1, o que significa que milhares de VSIs com endereços privados usarão um endereço IP público para conversar com a Internet pública. O PGW não ativa a Internet para iniciar uma conexão com essas instâncias.

## Região
{: #region}

Uma área geográfica dentro da qual um VPC é implementado. Cada região contém múltiplas zonas, que representam domínios de falha independentes. O IBM Cloud VPC abrange múltiplas zonas dentro de sua região designada.

## Recurso
{: #resource}

Um tipo de recurso que faz parte de uma implementação do VPC e pode incorrer em encargos de faturamento, por exemplo, uma VSI, um IP flutuante ou o próprio VPC. Os recursos podem ser combinados em _grupos de recursos_ para tornar a contabilidade mais fácil quando determinados recursos são sempre usados em combinação em um VPC.

## Grupo de Segurança
{: #security-group}

Um grupo de segurança age como um firewall virtual que controla o tráfego de entrada e de saída para um ou mais servidores (VSIs). Um grupo de segurança é uma coleção de regras que especificam se o tráfego deve ser permitido para uma VSI associada. Quando um cliente ativa uma VSI, ele pode associar um ou mais grupos de segurança a essa VSI. Dadas as permissões corretas, os clientes podem modificar as regras do grupo de segurança.

## Compartilhamentos
{: #shares}

Uma maneira de fornecer armazenamento de arquivo em um VPC.

## Sub-rede
{: #subnet}

Uma sub-rede é um intervalo de endereço IP, ligado a uma única Zona, que não pode abranger múltiplas Zonas ou Regiões. Uma sub-rede pode abranger a totalidade de uma zona em um IBM Cloud VPC.

Para os propósitos do IBM Cloud VPC, a característica importante para uma sub-rede é o fato de que as sub-redes podem ser isoladas umas das outras, assim como ser interconectadas da maneira usual. O isolamento de sub-rede pode ser realizado por meio de Listas de Controle de Acesso (ACLs) que agem como firewalls para controlar o fluxo de pacotes de dados entre as sub-redes. Da mesma forma, os grupos de segurança agem como firewalls virtuais para controlar o fluxo de pacotes de dados para e de instâncias de servidor virtual (VSIs) individuais.

É o isolamento de sub-redes que permite criar um espaço privado dentro da nuvem pública.

## Chave SSH
{: #ssh-key}

As credenciais de acesso criptográfico geradas automaticamente que controlam o acesso às instâncias de servidor virtual em um VPC.

## Volumes
{: #volumes}

O armazenamento de dados de alto desempenho montado pelo hypervisor para instâncias de servidor virtual em um VPC.

## VPC
{: #vpc}

Uma rede virtual vinculada a uma conta. Ela fornece controle de baixa granularidade sobre a segmentação de infraestrutura virtual e de tráfego de rede, juntamente com a segurança e a capacidade de escalar dinamicamente.

## VPN
{: #vpn}

Uma Rede Privada Virtual (VPN) permite uma conexão privada entre dois terminais, mesmo quando os dados são transferidos por meio de uma rede pública. Os clientes podem compartilhar dados como se eles estivessem conectados a uma rede privada. Geralmente, uma VPN é usada em combinação com métodos de segurança, como autenticação e criptografia, para fornecer máxima segurança e privacidade de dados.

## Conexão de VPN
{: #vpn-connection}

Uma conexão privada entre dois terminais, que permanece privada e pode ser criptografada mesmo quando os dados são transferidos por meio de uma rede pública.

## VPNaaS
{: #vpnaas}

A VPN como um Serviço (VPNaaS) fornece a configuração de uma VPN com terminais dentro e fora de um VPC.

## Zona
{: #zone}

Um domínio de falha independente. Uma Zona é uma abstração projetada para ajudar com a tolerância a falhas melhorada e latência diminuída. Uma Zona garante as propriedades a seguir:

 * Como cada zona é um domínio de falha independente, é extremamente improvável que duas zonas em uma região falhem simultaneamente.
 * O tráfego entre zonas em uma região terá menos de 2 ms em latência.
