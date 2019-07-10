---

copyright:
  years: 2019

lastupdated: "2019-06-13"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc-on-classic

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Eventos do Activity Tracker with LogDNA
{: #at-events}

Use o serviço {{site.data.keyword.at_full}} para controlar como seus usuários e aplicativos interagem com o {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) no {{site.data.keyword.cloud}}.
{:shortdesc}

O serviço {{site.data.keyword.at_full}} registra as atividades iniciadas pelo usuário para mudar o estado de um serviço no {{site.data.keyword.cloud}}. Para obter mais informações, consulte [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## Lista de eventos: recursos de rede
{: #events-volumes}

| Recurso  | Ações  | Descrição  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | O VPC foi criado. |
| vpc  | is.vpc.vpc.update   | O VPC foi atualizado.  |
| vpc  | is.vpc.vpc.delete   | O VPC foi excluído.  |
| vpc  | is.vpc.address-prefix.create  | O Prefixo de endereço foi incluído no VPC.  |
| vpc  | is.vpc.address-prefix.update  | O Prefixo de endereço do VPC foi atualizado.   |
| vpc  | is.vpc.address-prefix.delete  | O Prefixo de endereço foi removido do VPC.  |
| vpc  | is.vpc.vpc-route.create   | A rota foi incluída no VPC.   |
| vpc  | is.vpc.vpc-route.update   | A Rota do VPC foi atualizada.  |
| vpc  | is.vpc.vpc-route.delete   | A rota foi removida do VPC.   |
| floating-ip  | is.floating-ip.floating-ip.delete   | O IP flutuante foi criado.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | O IP flutuante foi atualizado.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | O IP flutuante foi excluído.  |
| network-acl  | is.network-acl.network-acl.create   | A ACL de rede foi criada.  |
| network-acl  | is.network-acl.network-acl.update   | A ACL de rede foi atualizada.  |
| network-acl  | is.network-acl.network-acl.delete   | A ACL de rede foi excluída.  |
| network-acl  | is.network-acl.rule.create  | A regra foi incluída na ACL de Rede.  |
| network-acl  | is.network-acl.rule.update  | A regra da ACL de Rede foi atualizada.   |
| network-acl  | is.network-acl.rule.delete  | A regra foi removida da ACL de Rede.  |
| public-gateway | is.public-gateway.public-gateway.create   | O Gateway público foi criado.   |
| public-gateway | is.public-gateway.public-gateway.update   | O Gateway público foi atualizado.   |
| public-gateway | is.public-gateway.public-gateway.delete   | O Gateway público foi excluído.   |
| security-group | is.security-group.security-group.create   | Criar Grupo de segurança   |
| security-group | is.security-group.security-group.delete   | Excluir Grupo de segurança   |
| security-group | is.security-group.security-group.update   | Atualizar Grupo de segurança   |
| security-group | is.security-group.security-group.rule-create  | Criação de regra do Grupo de segurança  |
| security-group | is.security-group.security-group.rule-delete  | Exclusão de regra do Grupo de segurança  |
| security-group | is.security-group.security-group.rule-update  | Atualização de regra do Grupo de segurança  |
| security-group | is.security-group.security-group.interface-attach | Conexão da interface do Grupo de segurança   |
| security-group | is.security-group.security-group.interface-detach | Desconexão da interface do Grupo de segurança   |
| sub-rede   | is.subnet.subnet.create   | A sub-rede foi criada.   |
| sub-rede   | is.subnet.subnet.update   | A sub-rede foi atualizada.   |
| sub-rede   | is.subnet.subnet.delete   | A sub-rede foi excluída.   |
| sub-rede   | is.subnet.network-acl.update  | A ACL de Rede da sub-rede foi substituída.   |
| sub-rede   | is.subnet.public-gateway.operate  | O Gateway público foi anexado à Sub-rede.  |
| sub-rede   | is.subnet.public-gateway.operate  | O Gateway público foi desanexado da Sub-rede.  |

## Lista de eventos: Recursos de cálculo
{: #events-computes}

A tabela a seguir lista as ações relacionadas aos recursos de cálculo e à geração de eventos.

| Recurso  | Ações  | Descrição  |
|:----------------|:-----------------------|:-----------------------|
| instância   | is.instance.instance.create   | A instância foi criada.   |
| instância   | is.instance.instance.delete   | A instância foi excluída.   |
| instância   | is.instance.instance.update   | A instância foi atualizada.   |
| instância   | is.instance.action.create   | A ação da instância foi criada.  |
| instância   | is.instance.action.delete   | A ação da instância pendente foi excluída.  |
| instância   | is.instance.network_interface.floating_ip.create  | O IP flutuante foi associado à interface de rede da instância  |
| instância   | is.instance.network_interface.floating_ip.delete  | O IP flutuante foi desassociado da interface de rede da instância |
| instância   | is.instance.volume_attachement.create   | O anexo do volume da instância foi criado  |
| instância   | is.instance.volume_attachement.delete   | O anexo do volume da instância foi excluído  |
| instância   | is.instance.volume_attachement.update   | O anexo do volume da instância foi atualizado  |
| principais  | is.key.key.create   | A chave foi criada.  |
| principais  | is.key.key.delete   | A chave foi excluída.  |
| principais  | is.key.key.update   | A chave foi atualizada.  |

## Lista de eventos: recursos de armazenamento
{: #events-storage}

A tabela a seguir lista as ações relacionadas aos recursos de armazenamento e à geração de um evento.

| Recurso  | Ações  | Descrição  |
|:----------------|:-----------------------|:-----------------------|
| compactado  | is.volume.volume.create  | O volume foi criado.  |
| compactado  | is.volume.volume.update  | O volume foi atualizado.  |
| compactado  | is.volume.volume.delete  | O volume foi excluído.  |


## Lista de eventos: balanceadores de carga
{: #events-load-balancers}

A tabela a seguir lista as ações relacionadas aos balanceadores de carga e à geração de eventos.

| Recurso  | Ações  | Descrição  |
|:----------------|:-----------------------|:-----------------------|
| Equilibrador de carga |  is.load-balancer.load-balancer.create | O balanceador de carga foi criado |
| Equilibrador de carga |  is.load-balancer.load-balancer.update | O balanceador de carga foi atualizado |
| Equilibrador de carga |  is.load-balancer.load-balancer.delete | O balanceador de carga foi excluído |
| Listener |  is.load-balancer.load-balancer.listener.create | O listener foi criado |
| Listener |  is.load-balancer.load-balancer.listener.update | O listener foi atualizado |
| Listener |  is.load-balancer.load-balancer.listener.delete | O listener foi excluído |
| Conjunto |  is.load-balancer.load-balancer.pool.create | O conjunto foi criado |
| Conjunto |  is.load-balancer.load-balancer.pool.update | O conjunto foi atualizado |
| Conjunto |  is.load-balancer.load-balancer.pool.delete | O conjunto foi excluído |
| Membro |  is.load-balancer.load-balancer.pool.member.create | O membro foi criado |
| Membro |  is.load-balancer.load-balancer.pool.member.update | O membro foi atualizado |
| Membro |  is.load-balancer.load-balancer.pool.member.delete | O membro foi excluído |
| Política |  is.load-balancer.load-balancer.listener.policy.create | A política foi criada |
| Política |  is.load-balancer.load-balancer.listener.policy.update | A política foi atualizada |
| Política |  is.load-balancer.load-balancer.listener.policy.delete | A política foi excluída |
| Regra |  is.load-balancer.load-balancer.listener.policy.rule.create | A regra foi criada |
| Regra |  is.load-balancer.load-balancer.listener.policy.rule.update | A regra foi atualizada |
| Regra |  is.load-balancer.load-balancer.listener.policy.rule.delete | A regra foi excluída |

Os eventos de auditoria do balanceador de carga são registrados para {{site.data.keyword.at_full}} na região `us-south`. A região na qual você provisiona o serviço do balanceador de carga não importa.
{:note}

## Lista de eventos: VPNs
{: #events-vpn}

A tabela a seguir lista as ações relacionadas a VPNs e à geração de eventos.

| Recurso  | Ações  | Descrição  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn.create   | A VPN foi criada   |
| vpn  | is.vpn.vpn.delete   | A VPN foi excluída   |
| vpn  | is.vpn.vpn.update   | A VPN foi atualizada  |
| vpn  | is.vpn.vpn.update   | A conexão de VPN foi criada  |
| vpn  | is.vpn.vpn.update   | A conexão de VPN foi excluída  |
| vpn  | is.vpn.vpn.update   | A conexão de VPN foi atualizada  |


## Locais suportados
{: #at-supported-locations}

O suporte {{site.data.keyword.at_full}} está disponível atualmente para os locais de Dallas e Frankfurt. A tabela a seguir lista o Activity Tracker with LogDNA para cada região do VPC.

| Região do VPC | Local do Activity Tracker with LogDNA |
|--------------|---------------------------------------|
| para sul   | Dallas  |
| jp-tok   | Tóquio  |
| eu-de  | Frankfurt   |

## Onde procurar eventos
{: #at-events-ui}

Quando você provisiona uma instância do {{site.data.keyword.at_full}}, os eventos são encaminhados automaticamente para o Activity Tracker com uma instância de serviço LogDNA em seu [local suportado](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`).
