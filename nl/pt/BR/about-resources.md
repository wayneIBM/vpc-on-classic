---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# Sobre recursos do VPC Infrastructure
{: #about-vpc-infrastructure-resources}

O termo _recursos_ refere-se às partes do componente de uma implementação do {{site.data.keyword.cloud}} Virtual Private Cloud. Cada VPC pode conter recursos dos tipos a seguir, que podem ser agrupados, conforme necessário:

* ** vpc **
* ** instância **
* ** principais **
* ** imagem **
* ** sub-rede **
* ** volume **
* **Listas de controle de acesso (ACLs) à rede**
* ** grupo de segurança **
* **IP flutuante**
* ** vnic **
* ** gateway **
* ** loadbalancer **
* ** vpn **

Alguns recursos de Infraestrutura do VPC herdam suas políticas de autorização, que controlam seu uso por meio do VPC pai, mas outras são gerenciadas separadamente.

Atualmente, os tipos de recursos `instance`, `key`, `security-group`, `volume`, `loadbalancer` e `vpn` mantêm suas próprias políticas de autorização. Mais detalhes sobre as autorizações de recurso sobre esses recursos são fornecidos posteriormente neste documento.
{: note}

## Políticas de recurso
{: #resource-policies}

Geralmente, o acesso a recursos em um VPC é controlado por _políticas_. Se você deseja customizar o acesso a recursos para seu VPC, é possível criar políticas customizadas. Uma política de recurso pode ser direcionar:

* todos os recursos
* todos os recursos de um tipo
* todos os recursos em um grupo
* um recurso individual

## Recursos e grupos de recursos
{: #resources-and-resource-groups}

Um _grupo de recursos_ é uma coleção de recursos, por exemplo, um VPC inteiro ou uma única sub-rede e sua ACL, que são associados para o propósito de estabelecer a autorização e o uso. Você pode considerar um grupo de recursos como uma coleção de infraestrutura que pode ser usada por um projeto, um departamento ou uma equipe.

É possível usar a CLI de procura para exibir tags anexadas a um recurso. Com os filtros apropriados, é possível exibir apenas as instâncias com as quais você se importa, como `ibmcloud resource search name:` ou `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

As grandes empresas podem desejar dividir um VPC em grupos de recursos. As empresas menores podem não precisar dividir seu VPC em grupos de recursos, porque toda a equipe deles estaria usando o VPC inteiro. Se você está familiarizado com o _OpenStack_, um grupo de recursos é semelhante em conceito a um _Projeto_ no _OpenStack Keystone_.

A designação de um recurso a um grupo de recursos pode ser feita SOMENTE ao criar seu VPC. Os recursos não podem mudar os grupos de recursos após eles serem criados.
{: .note}

Os grupos de recursos são projetados principalmente para grandes organizações. Para a maioria das empresas e equipes, a quebra de recurso está em um limite de VPC. Essa regra básica geral pode mudar quando é possível designar VSIs (instâncias) individuais a um grupo de recursos e usuários individuais a um recurso VSI (instância).

Quando você estiver configurando seu IBM Cloud VPC, se desejar usar grupos de recursos, será bom ter um plano apropriado antecipadamente de como deseja designar os recursos e os usuários em sua organização a cada grupo de recursos.
{: tip}

## Specifics para VPC
{: #specifics-for-vpc}

Alguns recursos de Infraestrutura do VPC herdam suas políticas de autorização, que controlam seu uso por meio da conta ou do VPC pai, enquanto outros têm políticas individuais.

### Autorização de recurso do VPC por tipo de recurso
{: #vpc-resource-authorization-by-resource-type}

A tabela resume como os recursos do VPC são autorizados por padrão.

| Tipo de recurso | Onde obtém sua autorização padrão |
|-----------|------------|
| VPC | Verificação de autorização quando criada |
| Balanceador de carga | Verificação de autorização quando criada |
| VPN | Verificação de autorização quando criada |
| Sub-rede | VPC pai |
| Gateway público | VPC pai |
| Ocorrência | Verificação de autorização quando criada |
| Grupo de Segurança | Verificação de autorização quando criada |
| Key | Verificação de autorização quando criada |
| Imagem | Conta |
| IP Flutuante | Conta |
| ACL de rede | Conta|
| Volume | Verificação de autorização quando criada |

Os IPs flutuantes e as ACLs de rede podem ser criados com definição de escopo no nível da conta, se não designados. No entanto, assim que um IP flutuante é designado a uma instância ou uma ACL é designada a uma sub-rede, esses recursos se tornam sujeitos ao escopo de autorização do VPC.
{: note}

### Cobertura de VPC de funções e ações autorizadas em recursos
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

Em um cenário com 2 VPCs, aqui estão alguns exemplos do que acontece quando qualquer usuário com determinadas funções tenta executar determinadas ações:

* Usuário _administrador_, com permissão de **Administrador** em todos os recursos
  * Cria  ` vpc1 `: ok

* Usuário _editor_, com permissão de **Editor** em todos os recursos
  * Cria  ` vpc2 `: ok
  * Atualizações  ` vpc2 `: ok
  * Exclui  ` vpc2 `: ok

* Usuário _operador_, com permissão de **Operador** em todos os recursos
  * Cria  ` vpc `: negado
  * Atualizações  ` vpc1 `: negada
  * Exclui  ` vpc1 `: negado

* Usuário _visualizador_, com permissão de **Visualizador** em todos os recursos
  * Lista VPCs: vê [vpc1]
  * Reads  ` vpc1 `: ok

* O usuário  _ noRole _, sem políticas
  * Lista VPCs: vê [ ]
  * Reads  ` vpc1 `: negado

### Tabela de resumo de cobertura VPC
{: #vpc-coverage-summary-table}

Para qualquer política que conceda acesso por função (Administrador, Editor, Operador, Visualizador), as ações a seguir são fornecidas (X = negado, o = OK)

 função:      | nenhum | Visualizador | Operador | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Criar      | X    | X      | X     | o      | o
Lista        | X    | o      | X     | o      | o
Ler        | X    | o      | X     | o      | o
Atualizar      | X    | X      | X     | o      | o
Excluir      | X    | X      | X     | o      | o


## Autorização de recurso de VPN para VPC
{: #resource-authorizations-of-vpn-for-vpc}

A autorização de recurso de **VPN para VPC** é configurada separadamente de outros tipos de autorização de recurso do VPC, mas de maneira semelhante.

As políticas no VPN for VPC controlam o acesso aos recursos de VPN, especialmente aos Gateways de VPN. Para acesso aos recursos filhos de um Gateway de VPN, como a conexões de VPN e a CIDRs locais ou de mesmo nível, deve-se ter acesso de **Leitura** (ou maior) ao Gateway de VPN pai. As políticas de recursos podem ser usadas apenas com conexões de VPN que também possuem acesso de **Leitura** ou superior para o Gateway de VPN pai. Uma política de recurso na VPN pode ter como destino:

* todos os recursos de VPN (Gateways de VPN, conexões de VPN, políticas IKE e IPsec)
* todos os recursos de VPN em um grupo de recursos
* um VPN Gateway individual

O uso de VPN é faturado separadamente também.
{: note}

### VPN para cobertura de VPC de funções e ações autorizadas em recursos
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

As mesmas funções de usuário são suportadas, como são suportadas na autorização de recurso do VPC, mas com diferentes ações ativadas para cada função.

Aqui estão alguns exemplos do que acontece quando os usuários com determinadas funções tentam executar determinadas ações relacionadas à VPN:

* Usuário _administrador_, com permissão de **Administrador** em todos os recursos
  * Cria, atualiza, exclui, lê, lista Gateways VPN: ok
  * Cria, atualiza, exclui, lê, lista Conexões VPN: ok
  * Cria, atualiza, exclui, lê, lista CIDRs peer e local de conexão VPN: ok
  * Cria, atualiza, exclui, lê, lista Políticas IKE: ok
  * Cria, atualiza, exclui, lê, lista Políticas IPSec: ok

* Usuário _editor_, com permissão de **Editor** em todos os recursos
  * O mesmo que aqueles do Administrador

* Usuário _operador_, com permissão de **Operador** em todos os recursos
  * Cria, atualiza, exclui Gateways VPN: negado
  * Cria, atualiza, exclui Conexões VPN: negada
  * Cria, atualiza, exclui CIDRs peer e local de conexão VPN: ok
  * Cria, atualiza, exclui Políticas IKE: negada
  * Cria, atualiza, exclui Políticas IPSec: negada
  * Lê, lista Gateways VPN: ok - vê dados reais
  * Lê, lista Conexões VPN: ok - vê dados reais
  * Lê, lista CIDRs peer e local de conexão VPN: ok - vê dados reais
  * Lê, lista Políticas IKE: ok - vê dados reais
  * Lê, lista Políticas IPSec: ok - vê dados reais

* Usuário _visualizador_, com permissão de **Visualizador** em todos os recursos
  * O mesmo que os do Operador

* O usuário  _ noRole _, sem políticas
  * Leituras, Listas de Gateways de VPN: negado - a lista mostra dados vazios
  * Leituras, Listas de conexões de VPN: negadas
  * Leituras, Listas de peer de conexão de VPN e CIDRs locais: negados
  * Leituras, Listas de políticas IKE: negadas - a lista mostra dados vazios
  * Leituras, Listas de políticas IPSec: negadas - a lista mostra dados vazios

### VPN para a tabela de resumo de cobertura VPC
{: #vpn-for-vpc-coverage-summary-table}

O mapeamento de função de ação na VPN para VPC pode ser visualizado pela tabela a seguir (X = negado, o = OK):

função:       | nenhum | Visualizador | Operador | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Criar      | X    | X      | X     | o      | o
Lista        | X    | o      | o     | o      | o
Ler        | X    | o      | o     | o      | o
Atualizar      | X    | X      | X     | o      | o
Excluir      | X    | X      | X     | o      | o

## Autorização de recurso para o Load Balancer for VPC
{: #resource-authorization-for-load-balancer-for-vpc}

A autorização de recurso para o uso do **Load Balancer para VPC** é configurada separadamente de outra autorização de recurso em seu VPC, mas de maneira semelhante.

O uso de Load Balancer é faturado separadamente também.
{: note}

### Cobertura do Load Balancer de funções e ações autorizadas em recursos
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

Aqui estão alguns exemplos do que acontece quando os usuários com determinadas funções tentam executar determinadas ações relacionadas ao VPC Load Balancer:

* Usuário _administrador_, com permissão de **Administrador** em todos os recursos
  * Cria, atualiza, exclui, lê, lista Load Balancers: ok
  * Cria, atualiza, exclui, lê, lista Listeners: ok
  * Cria, atualiza, exclui, lê, lista Conjuntos: ok
  * Cria, atualiza, exclui, lê, lista Membros: ok
  * Cria, atualiza, exclui, lê, lista Estatísticas do Load Balancer: ok

* Usuário _editor_, com permissão de **Editor** em todos os recursos
  * O mesmo que aqueles do Administrador

* Usuário _operador_, com permissão de **Operador** em todos os recursos
  * Cria, atualiza, exclui, lê, lista Load Balancers: negado
  * Cria, atualiza, exclui, lê, lista Listeners: negado
  * Cria, atualiza, exclui, lê, lista Conjuntos: negado
  * Cria, atualiza, exclui, lê, lista Membros: negado
  * Cria, atualiza, exclui, lê, lista Estatísticas do Load Balancer: negado

* Usuário _visualizador_, com permissão de **Visualizador** em todos os recursos
  * Lê, lista Load Balancers: ok
  * Lê, lista Listeners: ok
  * Lê, lista Conjuntos: ok
  * Lê, lista Membros: ok
  * Lê Estatísticas do Balanceador de Carga: ok

* O usuário  _ noRole _, sem políticas
  * Lê, lista Load Balancers: negado - a lista mostra dados vazios
  * Lê, lista Listeners: negado
  * Lê, lists Pools: negado
  * Lê, lista Membros: negado
  * Lê Estatísticas do Balanceador de Carga: negado

### Tabela de resumo de cobertura do Load Balancer
{: #load-balancer-coverage-summary-table}

O mapeamento de função de ação no Load Balancer para VPC pode ser visualizado pela tabela a seguir (X = negado, o = OK):

função:       | nenhum | Visualizador | Operador | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Criar      | X    | X      | X     | o      | o
Lista        | X    | o      | X     | o      | o
Ler        | X    | o      | X     | o      | o
Atualizar      | X    | X      | X     | o      | o
Excluir      | X    | X      | X     | o      | o


## Autorização de recurso para o Virtual Server for VPC
{: #planning-virtual-servers-for-vpc-permissions}

Configure a autorização de recurso para o **Virtual Server for VPC** separadamente de outras autorizações de recurso em seu VPC. 

Os servidores virtuais para uso do VPC são faturados separadamente.
{: note}

* Como um administrador, é possível definir funções e realizar quaisquer ações disponíveis no {{site.data.keyword.vsi_is_short}}.
* Como um editor, é possível modificar o estado e criar ou excluir sub-recursos.
* Como um operador, é possível executar ações que não mudem o estado dos recursos.
* Como um visualizador, é possível executar ações que não mudem o estado dos recursos.

### Tabela de resumo de cobertura do servidor virtual
{: #vsi-coverage-summary-table}

O mapeamento de função de ação no Virtual Server for VPC pode ser visualizado pela tabela a seguir (X = negado, o = OK):

função:       | nenhum | Visualizador | Operador | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Criar      | X    | X      | X     | o      | o
Lista        | X    | o      | o     | o      | o
Ler        | X    | o      | o     | o      | o
Atualizar      | X    | X      | X     | o      | o
Excluir      | X    | X      | X     | o      | o

Ao criar uma instância, você também deverá ter acesso de Operador aos recursos do VPC e do Grupo de segurança, se esses recursos forem especificados. Os recursos de Sub-rede e de IP Flutuante herdam permissões do VPC associado.  
{: tip} 
