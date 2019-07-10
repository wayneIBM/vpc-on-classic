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


# Eventi di Activity Tracker with LogDNA
{: #at-events}

Utilizza il servizio {{site.data.keyword.at_full}} per tracciare il modo in cui utenti e applicazioni interagiscono con {{site.data.keyword.cloud}} VPC (Virtual Private Cloud) in {{site.data.keyword.cloud}}.
{:shortdesc}

Il servizio {{site.data.keyword.at_full}} registra le attività avviate dall'utente che modificano lo stato di un servizio in {{site.data.keyword.cloud}}. Per ulteriori informazioni, vedi [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## Elenco di eventi: risorse di rete
{: #events-volumes}

| Risorsa  | Azione  | Descrizione  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | VPC è stato creato.  |
| vpc  | is.vpc.vpc.update   | VPC è stato aggiornato.  |
| vpc  | is.vpc.vpc.delete   | VPC è stato eliminato.  |
| vpc  | is.vpc.address-prefix.create  | Il prefisso di indirizzo è stato aggiunto a VPC.  |
| vpc  | is.vpc.address-prefix.update  | Il prefisso di indirizzo VPC è stato aggiornato.   |
| vpc  | is.vpc.address-prefix.delete  | Il prefisso di indirizzo è stato rimosso da VPC.  |
| vpc  | is.vpc.vpc-route.create   | L'instradamento è stato aggiunto a VPC.   |
| vpc  | is.vpc.vpc-route.update   | L'instradamento VPC è stato aggiornato.  |
| vpc  | is.vpc.vpc-route.delete   | L'instradamento è stato rimosso da VPC.   |
| ip-mobile  | is.floating-ip.floating-ip.delete   | L'IP mobile è stato creato.  |
| ip-mobile  | is.floating-ip.floating-ip.delete   |L'IP mobile è stato aggiornato.  |
| ip-mobile  | is.floating-ip.floating-ip.delete   |L'IP mobile è stato eliminato.  |
| acl-rete  | is.network-acl.network-acl.create   |L'ACL di rete è stato creato.  |
| acl-rete  | is.network-acl.network-acl.update   |L'ACL di rete è stato aggiornato.  |
| acl-rete  | is.network-acl.network-acl.delete   |L'ACL di rete è stato eliminato.  |
| acl-rete  | is.network-acl.rule.create  | La regola è stata aggiunta all'ACL di rete.  |
| acl-rete  | is.network-acl.rule.update  | La regola dell'ACL di rete è stata aggiornata.   |
| acl-rete  | is.network-acl.rule.delete  | La regola è stata rimossa dall'ACL di rete.  |
| gateway-pubblico | is.public-gateway.public-gateway.create   | Il gateway pubblico è stato creato.   |
| gateway-pubblico | is.public-gateway.public-gateway.update   | Il gateway pubblico è stato aggiornato.   |
| gateway-pubblico | is.public-gateway.public-gateway.delete   | Il gateway pubblico è stato eliminato.   |
| gruppo-sicurezza | is.security-group.security-group.create   | Creazione del gruppo di sicurezza   |
| gruppo-sicurezza | is.security-group.security-group.delete   | Eliminazione del gruppo di sicurezza   |
| gruppo-sicurezza | is.security-group.security-group.update   | Aggiornamento del gruppo di sicurezza   |
| gruppo-sicurezza | is.security-group.security-group.rule-create  | Creazione della regola del gruppo di sicurezza   |
| gruppo-sicurezza | is.security-group.security-group.rule-delete  | Eliminazione della regola del gruppo di sicurezza  |
| gruppo-sicurezza | is.security-group.security-group.rule-update  | Aggiornamento della regola del gruppo di sicurezza  |
| gruppo-sicurezza | is.security-group.security-group.interface-attach | Collegamento dell'interfaccia del gruppo di sicurezza   |
| gruppo-sicurezza | is.security-group.security-group.interface-detach | Scollegamento dell'interfaccia del gruppo di sicurezza   |
| sottorete   | is.subnet.subnet.create   | La sottorete è stata creata.   |
| sottorete   | is.subnet.subnet.update   | La sottorete è stata aggiornata.   |
| sottorete   | is.subnet.subnet.delete   | La sottorete è stata eliminata.   |
| sottorete   | is.subnet.network-acl.update  | L'ACL di rete della sottorete è stato sostituito.   |
| sottorete   | is.subnet.public-gateway.operate  | Il gateway pubblico è stato collegato alla sottorete.  |
| sottorete   | is.subnet.public-gateway.operate  | Il gateway pubblico è stato scollegato dalla sottorete.  |

## Elenco di eventi: risorse di calcolo
{: #events-computes}

La seguente tabella elenca le azioni relative alle risorse di calcolo e alla generazione di eventi.

| Risorsa  | Azione  | Descrizione  |
|:----------------|:-----------------------|:-----------------------|
| istanza   | is.instance.instance.create   | L'istanza è stata creata.   |
| istanza   | is.instance.instance.delete   | L'istanza è stata eliminata.   |
| istanza   | is.instance.instance.update   | L'istanza è stata aggiornata.   |
| istanza   | is.instance.action.create   | L'azione dell'istanza è stata creata.  |
| istanza   | is.instance.action.delete   | L'azione dell'istanza in sospeso è stata eliminata.  |
| istanza   | is.instance.network_interface.floating_ip.create  | L'IP mobile è stato associato all'interfaccia di rete dell'istanza  |
| istanza   | is.instance.network_interface.floating_ip.delete  | L'IP mobile è stato dissociato dall'interfaccia di rete dell'istanza |
| istanza   | is.instance.volume_attachement.create   | Il collegamento del volume dell'istanza è stato creato  |
| istanza   | is.instance.volume_attachement.delete   | Il collegamento del volume dell'istanza è stato eliminato  |
| istanza   | is.instance.volume_attachement.update   | Il collegamento del volume dell'istanza è stato aggiornato  |
| chiave  | is.key.key.create   | La chiave è stata creata.  |
| chiave  | is.key.key.delete   | La chiave è stata eliminata.  |
| chiave  | is.key.key.update   | La chiave è stata aggiornata.  |

## Elenco di eventi: risorse di archiviazione
{: #events-storage}

La seguente tabella elenca le azioni relative alle risorse di archiviazione e alla generazione di eventi.

| Risorsa  | Azione  | Descrizione  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  |  Il volume è stato creato.  |
| volume  | is.volume.volume.update  |  Il volume è stato aggiornato.  |
| volume  | is.volume.volume.delete  |  Il volume è stato eliminato.  |


## Elenco di eventi: programmi di bilanciamento del carico
{: #events-load-balancers}

La seguente tabella elenca le azioni relative ai programmi di bilanciamento del carico e alla generazione di eventi.

| Risorsa  | Azione  | Descrizione  |
|:----------------|:-----------------------|:-----------------------|
| Programma di bilanciamento del carico |  is.load-balancer.load-balancer.create | Il programma di bilanciamento del carico è stato creato |
| Programma di bilanciamento del carico |  is.load-balancer.load-balancer.update | Il programma di bilanciamento del carico è stato aggiornato |
| Programma di bilanciamento del carico |  is.load-balancer.load-balancer.delete | Il programma di bilanciamento del carico è stato eliminato |
| Listener |  is.load-balancer.load-balancer.listener.create | Il listener è stato creato |
| Listener |  is.load-balancer.load-balancer.listener.update | Il listener è stato aggiornato |
| Listener |  is.load-balancer.load-balancer.listener.delete | Il listener è stato eliminato |
| Pool |  is.load-balancer.load-balancer.pool.create | Il pool è stato creato |
| Pool |  is.load-balancer.load-balancer.pool.update | Il pool è stato aggiornato |
| Pool |  is.load-balancer.load-balancer.pool.delete | Il pool è stato eliminato |
| Membro |  is.load-balancer.load-balancer.pool.member.create | Il membro è stato creato |
| Membro |  is.load-balancer.load-balancer.pool.member.update | Il membro è stato aggiornato |
| Membro |  is.load-balancer.load-balancer.pool.member.delete | Il membro è stato eliminato |
| Politica |  is.load-balancer.load-balancer.listener.policy.create | La politica è stata creata |
| Politica |  is.load-balancer.load-balancer.listener.policy.update | La politica è stata aggiornata |
| Politica |  is.load-balancer.load-balancer.listener.policy.delete | La politica è stata eliminata |
| Regola |  is.load-balancer.load-balancer.listener.policy.rule.create | La regola è stata creata |
| Regola |  is.load-balancer.load-balancer.listener.policy.rule.update | La regola è stata aggiornata |
| Regola |  is.load-balancer.load-balancer.listener.policy.rule.delete | La regola è stata eliminata |

Gli eventi di controllo del programma di bilanciamento del carico vengono registrati in {{site.data.keyword.at_full}} nella regione `us-south`. Non importa in quale regione esegui il provisioning del servizio del programma di bilanciamento del carico.
{:note}

## Elenco di eventi: VPN
{: #events-vpn}

La seguente tabella elenca le azioni relative alle VPN e alla generazione di eventi.

| Risorsa  | Azione  | Descrizione  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn.create   | La VPN è stata creata  |
| vpn  | is.vpn.vpn.delete   | La VPN è stata eliminata |
| vpn  | is.vpn.vpn.update   | La VPN è stata aggiornata  |
| vpn  | is.vpn.vpn.update   | La connessione VPN è stata creata  |
| vpn  | is.vpn.vpn.update   | La connessione VPN è stata eliminata |
| vpn  | is.vpn.vpn.update   | La connessione VPN è stata aggiornata |


## Ubicazioni supportate
{: #at-supported-locations}

Il supporto di {{site.data.keyword.at_full}} è attualmente disponibile per le ubicazioni di Dallas e Francoforte. La seguente tabella elenca le ubicazioni di Activity Tracker with LogDNA per ogni regione VPC.

| Regione VPC | Ubicazione di Activity Tracker with LogDNA |
|--------------|---------------------------------------|
| us-south   | Dallas  |
| jp-tok   | Tokyo  |
| eu-de  | Francoforte   |

## Dove cercare gli eventi
{: #at-events-ui}

Quando esegui il provisioning di un'istanza {{site.data.keyword.at_full}}, gli eventi vengono inoltrati automaticamente all'istanza del servizio Activity Tracker with LogDNA nella sua [ubicazione supportata](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`).
