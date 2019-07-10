---

copyright:
  years: 2019
lastupdated: "2019-05-29"


keywords: vpc, delete, resources

subcollection: vpc-on-classic
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

# Excluindo um VPC
{: #deleting}

Um recurso {{site.data.keyword.cloud}} Virtual Private Cloud não poderá ser excluído se ele contiver outros recursos ou se ele estiver conectado a outro recurso. Este documento descreve os relacionamentos entre os recursos do VPC e fornece informações sobre as melhores práticas para excluir um VPC.

A tabela a seguir resume os tipos de recursos do VPC e os relacionamentos a serem considerados ao excluir esses recursos.

| Recurso | Antes que possa ser excluído... | Depois que ele for excluído... |
| ---------------- | ----------------------------------------- | --------------------------- |
| VPC | Todos os gateways públicos e sub-redes no VPC devem ser excluídos | Todos os grupos de segurança e prefixos de endereço são excluídos automaticamente |
| Sub-rede | Todas as instâncias e quaisquer interfaces de rede na sub-rede devem ser excluídas; todos os gateways de VPN e balanceadores de carga na sub-rede devem ser excluídos | Qualquer gateway público que atenda à sub-rede será desconectado; qualquer ACL de rede associada à sub-rede será desconectada |
| Ocorrência | ---- | Todas as interfaces de rede são excluídas automaticamente; o volume de inicialização é excluído com a instância; a exclusão do volume secundário depende do parâmetro `delete_volume_on_instance_delete`; qualquer IP flutuante conectado a uma das interfaces de rede é separado automaticamente |
| Interface de rede | ---- | Qualquer IP flutuante conectado à interface de rede é liberado |
| Key | ---- | Depois de excluir uma chave, ela não pode mais ser usada para provisionar uma nova instância nem para executar um recarregamento de S.O. em uma instância existente. No entanto, a chave ainda está disponível em todas as instâncias que você provisionou com ela e é possível continuar usando-a para efetuar login. |
| Imagem | ---- | A imagem não pode ser usada para provisionar uma nova instância, mas as instâncias existentes com a imagem não são afetadas. |
| Volume | O volume deve ser separado de todas as instâncias | ---- |
| ACL de rede | A ACL de rede deve ser separada de todas as sub-redes; as ACLs de rede padrão para um VPC não podem ser excluídas  | ---- |
| Grupo de segurança | O grupo de segurança deve ser separado de todas as interfaces de rede; o grupo de segurança padrão para um VPC não pode ser excluído. | ---- |
| IP Flutuante | O IP flutuante deve ser separado de qualquer interface de rede | ---- |
| Gateway público | O gateway público deve ser separado de todas as sub-redes |  ---- |
| Equilibrador de carga | ---- | ---- |
| Gateway VPN | ---- | Devido à capacidade de compartilhar políticas IKE e IPsec entre gateways, elas não serão excluídas quando um Gateway de VPN for excluído. As políticas IKE e IPsec devem ser removidas manualmente. |


## Os recursos do VPC não podem ser excluídos em um estado temporário
{: #deleting-status}

Os recursos do VPC não poderão ser excluídos quando estiverem em um estado temporário, como `pending`. Todos os recursos devem estar em um estado "final", como `available` ou `failed`, antes que eles possam ser excluídos. Deve-se aguardar que o recurso fique `available` ou `failed` antes de tentar excluir o recurso. [Entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se um recurso não mostrar um status `available` ou `failed` dentro de 30 minutos.

Depois de emitir uma solicitação para excluir um recurso, o recurso entrará em um estado temporário de `deleting`. Aguarde até que o recurso desapareça da lista antes de tentar excluir o recurso pai ou anexado. Em algumas situações, a operação de exclusão pode falhar e o recurso entra em um status `failed` dentro de alguns minutos. Entre em contato com o suporte se um recurso não desaparecer nem mostrar um status `failed` dentro de 30 minutos após você ter feito uma solicitação de exclusão. Depois que o recurso estiver no status `failed`, será possível tentar excluí-lo novamente. [Entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se não for possível excluir um recurso no status `failed`.

## Siga esta ordem ao excluir um VPC
{: #deleting-order}

Um VPC contém sub-redes e gateways públicos. Um gateway público pode ser conectado a uma ou mais sub-redes no VPC. Um balanceador de carga e um gateway de VPN existem dentro de uma sub-rede de um VPC. Uma sub-rede também pode conter instâncias do servidor virtual (VSIs), e uma VSI pode ter várias interfaces de rede em sub-redes diferentes dentro do VPC. Antes que uma sub-rede possa ser excluída, quaisquer gateways de VPN, balanceadores de carga e interfaces de rede na sub-rede devem primeiro ser excluídos. Um VPC contém grupos de segurança também, mas eles são excluídos automaticamente quando o VPC é excluído. Os prefixos de endereço são atributos de um VPC e eles são excluídos automaticamente quando o VPC é excluído.

Siga esta ordem ao excluir um VPC:

1.  Exclua todos os gateways de VPN que são provisionados em qualquer sub-rede do VPC.
2.  Desconecte todos os balanceadores de carga que são provisionados em qualquer sub-rede do VPC.
3.  Exclua todas as instâncias que possuem interfaces de rede em qualquer sub-rede do VPC. Qualquer IP flutuante associado a uma interface de rede é automaticamente separado da interface de rede quando a instância é excluída.
4.  Exclua todas as sub-redes no VPC. Qualquer gateway público conectado a uma sub-rede é automaticamente separado da sub-rede quando a sub-rede é excluída.
5.  Exclua todos os gateways públicos no VPC.
6.  Depois que o VPC não tiver nenhuma sub-rede e nenhum gateway público, o VPC poderá ser excluído. Todos os Prefixos de endereço no VPC são excluídos automaticamente quando o VPC é excluído.

## Relacionamentos de recurso de VPC
{: #deleting-relationships}

Alguns recursos do VPC estão contidos em outros recursos do VPC (como "filhos"), mas outros recursos do VPC estão contidos no nível de conta, fora de qualquer VPC específico. Todos os recursos do VPC filhos devem ser excluídos antes que o recurso pai possa ser excluído. Para recursos do VPC de nível de conta, todos os links para recursos existentes devem ser excluídos antes que o recurso da conta possa ser excluído.

Em alguns casos, a exclusão de um recurso de VPC remove o link para os recursos existentes automaticamente. As tabelas a seguir descrevem o comportamento esperado.

### VPC
{: #deleting-details}

Os VPCs contêm sub-redes e gateways públicos. Os balanceadores de carga, os gateways de VPN e as instâncias são provisionados em uma sub-rede. Antes que um VPC possa ser excluído, todas as sub-redes e todos os gateways públicos no VPC devem ser excluídos. Em outras palavras, todos os gateways públicos, balanceadores de carga, gateways de VPN e instâncias no VPC devem primeiro ser excluídos.

Um VPC também contém prefixos de endereços e grupos de segurança, mas esses recursos são excluídos automaticamente quando o VPC é excluído.

Um VPC deve ter um grupo de segurança padrão e uma ACL de rede padrão. Se você criar um VPC sem especificar um grupo de segurança padrão e uma ACL de rede, um grupo de segurança padrão e uma ACL de rede serão criados automaticamente para o VPC. O grupo de segurança padrão e a ACL de rede padrão não podem ser excluídos. A ACL de rede padrão é excluída automaticamente quando o VPC é excluído. Todos os grupos de segurança no VPC são excluídos automaticamente quando o VPC é excluído.

| No VPC | Pode conter | Pode conectar-se a | Tem status? | Excluído automaticamente quando o VPC é excluído | Separado automaticamente quando excluído |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Sub-rede | instâncias, balanceadores de carga, gateways de VPN | gateway público, ACL de rede | Sim| Não | Sim|
| Gateway público| --- | sub-redes, IP flutuante | Sim | Não | Não às sub-redes, Sim ao IP flutuante |
| Grupos de Segurança | ---  | instâncias (NIC), VPC como padrão | Não| Sim | Não |

### Sub-rede
{: #deleting-subnet}

Todos os recursos em uma sub-rede devem ser excluídos antes que a sub-rede possa ser excluída. As sub-redes contêm as interfaces de rede da instância, os balanceadores de carga e os gateways de VPN. A interface de rede associada à sub-rede deve ser excluída antes que a sub-rede possa ser excluída, juntamente com quaisquer balanceadores de carga ou gateway de VPN provisionados na sub-rede.

Uma instância pode ter várias interfaces de rede que podem pertencer a várias sub-redes do VPC. Antes que uma sub-rede possa ser excluída, qualquer interface de rede na sub-rede deve ser excluída primeiro. A interface de rede primária da instância não pode ser excluída nem movida para outra sub-rede, portanto, todas as instâncias com uma interface de rede primária na sub-rede devem ser excluídas antes que a sub-rede possa ser excluída.


| Na Sub-rede | Pode conter | Pode conectar-se a | Tem status? | Excluído automaticamente quando a sub-rede é excluída | Separado automaticamente quando excluído |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Instância (interface de rede) | várias interfaces de rede | anexos de volume, grupos de segurança | Sim| Não  | Sim|
| VPN | --- | ---| Sim | Não  | --- |
| Balanceador de carga | ---  | --- | Sim | Não | ---  |

### Ocorrência
{: #deleting-instance}

Nenhum pré-requisito é necessário para excluir uma instância. Quando a instância é excluída, todas as suas interfaces de rede são excluídas automaticamente. O volume de inicialização da instância e todos os seus anexos de volume são excluídos. Quaisquer IPs flutuantes conectados a qualquer uma de suas interfaces de rede são liberados automaticamente. Qualquer volume de armazenamento de bloco conectado à instância é excluído automaticamente se o volume foi criado com o sinalizador `delete_volume_on_instance_delete` configurado como true; caso contrário, a instância é separada do volume, mas o volume permanece. Se algum grupo de segurança estiver anexado a qualquer uma das interfaces de rede da instância, os grupos de segurança serão separados automaticamente também, quando as interfaces de rede forem excluídas.

| Em Instância | Pode conter | Pode conectar-se a | Tem status? | Excluído automaticamente quando a instância é excluída | Separado automaticamente quando excluído |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Interface de rede | --- | sub-redes, IP flutuante, grupos de segurança | Não | Sim | Sim |

### Balanceador de carga
{: #deleting-lb}

Nenhum pré-requisito é necessário para excluir um balanceador de carga. Quando o balanceador de carga é excluído, todos os membros do listener, conjuntos e membros do conjunto que fazem parte do balanceador de carga são excluídos automaticamente.

A exclusão de um balanceador de carga pode levar até 30 minutos. A solicitação de exclusão muda imediatamente o status de provisionamento do balanceador de carga para `deleting`. No entanto, ele não está realmente excluído até que o balanceador de carga desapareça da consulta de lista.
{: important}

### VPN
{: #deleting-vpn}

Nenhum pré-requisito é necessário para excluir um gateway de VPN. Quando o gateway de VPN é excluído, suas conexões associadas também são excluídas automaticamente. As políticas IKE e as políticas IPSec não são excluídas quando um gateway de VPN é excluído.

A exclusão de um gateway de VPN pode levar até 30 minutos. A solicitação de exclusão muda imediatamente o status de provisionamento do gateway de VPN para `deleting`. No entanto, ele não está realmente excluído até que o gateway de VPN desapareça da consulta de lista.
{: important}

### IP Flutuante
{: #deleting-floating-ip}

Um IP flutuante existe fora de um VPC, no nível da conta. Se o IP flutuante estiver ligado a um gateway público ou instância, o link deverá ser excluído (liberado) antes que o IP flutuante possa ser excluído.

Se você excluir um recurso para o qual o IP flutuante está ligado, por exemplo, se você excluir uma interface de rede da instância, o IP flutuante será liberado automaticamente.

### Volume
{: #deleting-volume}

Dois tipos de volumes podem existir em um VPC: um volume de inicialização de armazenamento de bloco e um volume de dados de armazenamento de bloco. Esses volumes são excluídos de forma diferente.

**Excluindo volumes de inicialização**

Os volumes de inicialização existem em uma instância e não são considerados entidades separadas dessa instância. Quando você cria uma instância, um volume de inicialização também é criado e anexado à instância. O volume de inicialização é excluído automaticamente quando você exclui a instância. Não é possível excluir o volume de inicialização sem excluir a instância.

**Excluindo volumes de dados**

Os volumes de dados de armazenamento de bloco podem ser provisionados e gerenciados separadamente de suas instâncias de servidor virtual associadas. É possível anexar um volume de dados como armazenamento secundário a uma instância do servidor virtual. Não é possível excluir um volume de dados que está anexado a uma instância (ou seja, se o volume estiver "ativo"). Deve-se, primeiro, separar o volume da instância e, em seguida, é possível excluir o volume.

Um volume de dados possui um atributo (ou sinalizador) chamado `delete_volume_on_instance_delete` na API e `Auto Delete` na CLI e na IU. Se esse sinalizador estiver configurado como `true` (`Enabled` na IU), quando a instância com o volume anexado for excluída, o volume será separado e excluído automaticamente. Se o sinalizador do volume for configurado como `false` (`Disabled` na IU), a instância será separada do volume, mas o volume não é excluído quando a instância conectada é excluída. O volume pode ser anexado a outra instância.

Um volume de armazenamento de bloco pode ser conectado a apenas um servidor virtual de cada vez.
{: tip}

### Grupos de segurança
{: #deleting-secgroup}

Um grupo de segurança não poderá ser excluído se ele estiver sendo usado por uma interface de rede ou se for o grupo de segurança padrão de um VPC. Antes de excluir o grupo de segurança, remova todas as interfaces de rede do grupo de segurança e certifique-se de que o grupo de segurança não esteja sendo usado como o grupo de segurança padrão do VPC.

A exclusão de um VPC excluirá todos os grupos de segurança nesse VPC, automaticamente.

### ACLs de rede
{: #deleting-netacl}

Uma ACL de rede não poderá ser excluída se estiver sendo usada por uma sub-rede ou se for a ACL de rede padrão de um VPC. Antes de excluir a ACL de rede, desconecte a ACL de rede de todas as sub-redes e certifique-se de que a ACL de rede não esteja sendo usada como a ACL de rede padrão de um VPC.

Quando qualquer VPC é criado, ele requer uma ACL de rede padrão. Se uma ACL de rede existente não é especificada como o padrão quando o VPC é criado, uma nova ACL de rede é criada e configurada como o padrão. Essa ACL de rede padrão será excluída automaticamente quando o VPC for excluído, se a ACL não estiver em uso em nenhum outro lugar.

Ao contrário dos grupos de segurança, as ACLs de rede podem ser designadas entre VPCs. Portanto, excluir um VPC não exclui as ACLs de rede.
{: note}

## Próximas Etapas
{: #deleting-nextsteps}

Os tópicos a seguir fornecem mais exemplos sobre como excluir recursos do VPC, usando o IBM Cloud Console, a Interface da linha de comandos ou a API.

-   [Excluindo recursos de VPC usando a IU](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-console)
-   [Excluindo recursos de VPC usando a CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)
-   [Excluindo recursos de VPC usando a API](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-api)
