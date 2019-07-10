---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

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

# Configurando o acesso ao seu Classic Infrastructure por meio do VPC
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (tópico da ajuda vinculado)

É possível configurar o acesso ao seu {{site.data.keyword.cloud}} Classic Infrastructure, incluindo conectividade de Direct Link, por meio de um VPC em cada região, para qualquer conta. Esses "VPCs de Acesso Clássico" especiais usam o mesmo recurso de roteamento (roteador implícito) que a infraestrutura clássica do {{site.data.keyword.cloud}}. Espera-se que os leitores estejam familiarizados com a rede de infraestrutura clássica.

Quando você configura um VPC para acesso clássico, cada host de cálculo (VSI ou Bare Metal) sem uma interface pública em sua conta clássica pode enviar e receber pacotes para e do VPC de acesso clássico. No entanto, lembre-se de que firewalls, gateways, ACLs de rede ou grupos de segurança podem filtrar esse tráfego. Como uma melhor prática, recomendamos que você permita somente o tráfego que é necessário para que seus aplicativos funcionem adequadamente.

Em hosts com uma interface pública, deve-se incluir uma rota de volta para seu VPC ativado para clássico.
{: important}

## Pré-requisitos:
{: #vpc-prerequisites}

1. Sua conta clássica deve ser vinculada à sua conta do IBM Cloud. Veja [Vinculando contas do IBMid](/docs/account?topic=account-unifyingaccounts) para obter instruções sobre como fazer isso.
1. Sua conta clássica deve ser ativada para VRF.
    * Se já tiver o Direct Link em sua conta, você estará pronto.
    * Se a sua conta não estiver ativada para VRF, abra um chamado para solicitar "Migração VRF" para sua conta. Veja [Como é possível iniciar a conversão](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion) na documentação do Direct Link para saber mais sobre o processo de conversão.

Firewalls, gateways, ACLs de rede ou grupos de segurança podem filtrar alguns ou todo o tráfego de rede entre os recursos Classic e VPC.
{: important}

Todas as sub-redes em um VPC com acesso clássico serão compartilhadas no VRF de infraestrutura clássica, que usa endereços IP no espaço `10.0.0.0/8`. Para evitar conflitos de endereço IP, **não use** os endereços IP no espaço `10.0.0.0/8` ao criar sub-redes em um VPC de Acesso Clássico.
{: important}

## Criar um VPC de Acesso Clássico
{: #create-a-classic-access-vpc}

É possível criar um VPC com recurso de Acesso Clássico usando a Interface com o Usuário (IU), a Interface da Linha de Comandos (CLI) ou a Interface de Programação de Aplicativos (API).

Um VPC deve ser configurado para o Acesso Clássico quando ele é criado: não é possível atualizar um VPC para incluir ou remover o recurso de Acesso Clássico.
{: note}

### Criar um VPC de Acesso Clássico por meio da interface com o usuário
{: #create-a-classic-access-vpc-from-the-user-interface}

Crie um VPC de Acesso Clássico na página **Criar VPC**, clicando na caixa de seleção intitulada **Ativar acesso ao recurso clássico**, sob o rótulo **Acesso clássico**.

![classic-access-ui](/images/classic-access-ui.png)

### Criar um VPC de Acesso Clássico usando a CLI
{: #create-a-classic-access-vpc-using-the-cli}

Use a sinalização `--classic-access` ao criar o VPC. Por exemplo,

```
ibmcloud é vpc-create my-access-vpc -- classic-access
```
{: pre}


### Criar um VPC de Acesso Clássico usando a API
{: #create-a-classic-access-vpc-using-the-api}

Passe o parâmetro `classic_access` ao criar o VPC. Por exemplo,

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Prefixos de endereço padrão do VPC de Acesso Clássico
{: #classic-access-default-address-prefixes}

Todas as sub-redes em um VPC com acesso clássico serão compartilhadas no VRF de infraestrutura clássica, que usa endereços IP no espaço `10.0.0.0/8`. Para evitar conflitos de endereço IP, **não use** os endereços IP no espaço `10.0.0.0/8` ao criar sub-redes em um VPC de Acesso Clássico.
{: important}

Quando um novo VPC de Acesso Clássico é criado, os prefixos de endereço padrão são designados, com base na região e na zona, conforme mostrado na tabela a seguir:

Zona         | Prefixo do endereço
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## Limitações
{: #vpc-limitations}

* Apenas as redes privadas (ou "de volta" na documentação antiga) serão conectadas ao roteador implícito privado da sua conta.
* Somente sub-redes alocadas com APIs clássicas são conectadas ao lado clássico de seu roteador implícito privado. A função de roteamento do roteador implícito não funciona entre sub-redes em VLANs clássicas.
* Somente um VPC por região, por conta, pode ser ativado para o Acesso Clássico.

Dependendo do S.O. que você instalou em suas VSIs clássicas ou servidores bare metal, seu procedimento de configuração variará. Para obter mais informações, consulte [Sobre servidores virtuais públicos](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
