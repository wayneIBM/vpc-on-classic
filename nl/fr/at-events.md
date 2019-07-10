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


# Evénements Activity Tracker avec LogDNA
{: #at-events}

Utilisez le service {{site.data.keyword.at_full}} pour voir comment vos utilisateurs et vos applications interagissent avec {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) dans {{site.data.keyword.cloud}}.
{:shortdesc}

Le service {{site.data.keyword.at_full}} enregistre les activités initiées par l'utilisateur pour modifier l'état d'un service dans {{site.data.keyword.cloud}}. Pour plus d'informations, voir [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

## Liste des événements : ressources réseau
{: #events-volumes}

| Ressource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | Le cloud privé virtuel (VPC) a été créé.  |
| vpc  | is.vpc.vpc.update   | Le cloud privé virtuel (VPC) a été mis à jour.  |
| vpc  | is.vpc.vpc.delete   | Le cloud privé virtuel (VPC) a été supprimé.  |
| vpc  | is.vpc.address-prefix.create  | Le préfixe d'adresse a été ajouté dans VPC.  |
| vpc  | is.vpc.address-prefix.update  | Le préfixe d'adresse VPC a été mis à jour.   |
| vpc  | is.vpc.address-prefix.delete  | Le préfixe d'adresse a été supprimé de VPC.  |
| vpc  | is.vpc.vpc-route.create   | La route a été ajoutée dans VPC.   |
| vpc  | is.vpc.vpc-route.update   | La route VPC a été mise à jour.  |
| vpc  | is.vpc.vpc-route.delete   | La route a été supprimée de VPC.   |
| floating-ip  | is.floating-ip.floating-ip.delete   | L'adresse IP flottante a été créée.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | L'adresse IP flottante a été mise à jour.  |
| floating-ip  | is.floating-ip.floating-ip.delete   | L'adresse IP flottante a été supprimée.  |
| network-acl  | is.network-acl.network-acl.create   | La liste de contrôle d'accès au réseau a été créée.  |
| network-acl  | is.network-acl.network-acl.update   | La liste de contrôle d'accès au réseau a été mise à jour.  |
| network-acl  | is.network-acl.network-acl.delete   | La liste de contrôle d'accès au réseau a été supprimée.  |
| network-acl  | is.network-acl.rule.create  | La règle a été ajoutée à la liste de contrôle d'accès au réseau.  |
| network-acl  | is.network-acl.rule.update  | La règle de la liste de contrôle d'accès au réseau a été mise à jour.   |
| network-acl  | is.network-acl.rule.delete  | La règle a été supprimée de la liste de contrôle d'accès au réseau.  |
| public-gateway | is.public-gateway.public-gateway.create   | La passerelle publique a été créée.   |
| public-gateway | is.public-gateway.public-gateway.update   | La passerelle publique a été mise à jour.   |
| public-gateway | is.public-gateway.public-gateway.delete   | La passerelle publique a été supprimée.   |
| security-group | is.security-group.security-group.create   | Création du groupe de sécurité   |
| security-group | is.security-group.security-group.delete   | Suppression du groupe de sécurité   |
| security-group | is.security-group.security-group.update   | Mise à jour du groupe de sécurité   |
| security-group | is.security-group.security-group.rule-create  | Création de la règle du groupe de sécurité  |
| security-group | is.security-group.security-group.rule-delete  | Suppression de la règle du groupe de sécurité  |
| security-group | is.security-group.security-group.rule-update  | Mise à jour de la règle du groupe de sécurité  |
| security-group | is.security-group.security-group.interface-attach | Connexion de l'interface du groupe de sécurité   |
| security-group | is.security-group.security-group.interface-detach | Déconnexion de l'interface du groupe de sécurité   |
| subnet   | is.subnet.subnet.create   | Le sous-réseau a été créé.   |
| subnet   | is.subnet.subnet.update   | Le sous-réseau a été mis à jour.   |
| subnet   | is.subnet.subnet.delete   | Le sous-réseau a été supprimé.   |
| subnet   | is.subnet.network-acl.update  | La liste de contrôle d'accès au réseau a été remplacée.   |
| subnet   | is.subnet.public-gateway.operate  | La passerelle publique a été connectée au sous-réseau.  |
| subnet   | is.subnet.public-gateway.operate  | La passerelle publique a été déconnectée du sous-réseau.  |

## Liste des événements : ressources de calcul
{: #events-computes}

Le tableau suivant présente les actions liées aux ressources de calcul et à la génération d'événements.

| Ressource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| instance   | is.instance.instance.create   | L'instance a été créée.   |
| instance   | is.instance.instance.delete   | L'instance a été supprimée.   |
| instance   | is.instance.instance.update   | L'instance a été mise à jour.   |
| instance   | is.instance.action.create   | L'action d'instance a été créée.  |
| instance   | is.instance.action.delete   | L'action d'instance en attente a été supprimée.  |
| instance   | is.instance.network_interface.floating_ip.create  | L'adresse IP flottante a été associée à l'interface réseau de l'instance  |
| instance   | is.instance.network_interface.floating_ip.delete  | L'adresse IP flottante a été dissociée de l'interface réseau de l'instance |
| instance   | is.instance.volume_attachement.create   | Le volume de l'instance a été créé  |
| instance   | is.instance.volume_attachement.delete   | La connexion du volume de l'instance a été supprimée  |
| instance   | is.instance.volume_attachement.update   | La connexion du volume de l'instance a été mise à jour  |
| key  | is.key.key.create   | La clé a été créée.  |
| key  | is.key.key.delete   | La clé a été supprimée.  |
| key  | is.key.key.update   | La clé a été mise à jour.  |

## Liste des événements : ressources de stockage
{: #events-storage}

Le tableau suivant présente les actions liées aux ressources de stockage et à la génération d'événements.

| Ressource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  | Le volume a été créé.  |
| volume  | is.volume.volume.update  | Le volume a été mis à jour.  |
| volume  | is.volume.volume.delete  | Le volume a été supprimé.  |


## Liste des événements : équilibreurs de charge
{: #events-load-balancers}

Le tableau suivant présente les actions liées aux équilibreurs de charge et à la génération d'événements.

| Ressource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| Load balancer |  is.load-balancer.load-balancer.create | L'équilibreur de charge a été créé |
| Load balancer |  is.load-balancer.load-balancer.update | L'équilibreur de charge a été mis à jour |
| Load balancer |  is.load-balancer.load-balancer.delete | L'équilibreur de charge a été supprimé |
| Listener |  is.load-balancer.load-balancer.listener.create | L'écouteur a été créé |
| Listener |  is.load-balancer.load-balancer.listener.update | L'écouteur a été mis à jour |
| Listener |  is.load-balancer.load-balancer.listener.delete | L'écouteur a été supprimé |
| Pool |  is.load-balancer.load-balancer.pool.create | Le pool a été créé |
| Pool |  is.load-balancer.load-balancer.pool.update | Le pool a été mis à jour |
| Pool |  is.load-balancer.load-balancer.pool.delete | Le pool a été supprimé |
| Member |  is.load-balancer.load-balancer.pool.member.create | Le membre a été créé |
| Member |  is.load-balancer.load-balancer.pool.member.update | Le membre a été mis à jour |
| Member |  is.load-balancer.load-balancer.pool.member.delete | Le membre a été supprimé |
| Policy |  is.load-balancer.load-balancer.listener.policy.create | La stratégie a été créée |
| Policy |  is.load-balancer.load-balancer.listener.policy.update | La stratégie a été mise à jour |
| Policy |  is.load-balancer.load-balancer.listener.policy.delete | La stratégie a été supprimée |
| Rule |  is.load-balancer.load-balancer.listener.policy.rule.create | La règle a été créée |
| Rule |  is.load-balancer.load-balancer.listener.policy.rule.update | La règle a été mise à jour |
| Rule |  is.load-balancer.load-balancer.listener.policy.rule.delete | La règle a été supprimée |

Les événements d'audit de l'équilibreur de charge sont enregistrés dans {{site.data.keyword.at_full}} dans la région du Sud des Etats-Unis (`us-south`). La région dans laquelle vous mettez à disposition le service d'équilibreur de charge n'a pas d'importance.
{:note}

## Liste des événements : VPN
{: #events-vpn}

Le tableau suivant présente les actions liées aux réseaux privés virtuels (VPN) et à la génération d'événements.

| Ressource  | Action  | Description  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn.create   | Le VPN a été créé   |
| vpn  | is.vpn.vpn.delete   | Le VPN a été supprimé   |
| vpn  | is.vpn.vpn.update   | Le VPN a été mis à jour   |
| vpn  | is.vpn.vpn.update   | La connexion VPN a été créée  |
| vpn  | is.vpn.vpn.update   | La connexion VPN a été supprimée  |
| vpn  | is.vpn.vpn.update   | La connexion VPN a été mise à jour  |


## Emplacements pris en charge
{: #at-supported-locations}

Actuellement la prise en charge d'{{site.data.keyword.at_full}} est disponible aux emplacements Dallas et Francfort. Le tableau suivant présente l'emplacement d'Activity Tracker avec LogDNA pour chaque région VPC.

| Région VPC | Emplacement d'Activity Tracker avec LogDNA |
|--------------|---------------------------------------|
| us-south   | Dallas  |
| jp-tok   | Tokyo  |
| eu-de  | Francfort   |

## Où rechercher des événements
{: #at-events-ui}

Lorsque vous mettez à disposition une instance {{site.data.keyword.at_full}}, les événements sont transférés automatiquement à l'instance de service Activity Tracker avec un agent LogDNA à l'[emplacement pris en charge](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`) associé.
