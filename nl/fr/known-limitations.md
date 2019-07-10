---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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
{:DomainName: data-hd-keyref="DomainName"}

# Limitations connues
{: #known-limitations}

Ce document comprend un récapitulatif des fonctions et des API non prises en charge, des fonctions et des cas d'utilisation qui ne sont pas encore pris en charge, des problèmes connus dans l'édition actuelle, ainsi qu'une liste de fonctions proposées uniquement sous forme de services bêta. Les limitations connues sont susceptibles de changer au fur et à mesure que nous ajoutons des fonctions à {{site.data.keyword.vpc_full}} (VPC). Par conséquent, n'hésitez pas à consulter ce document régulièrement. 

## Récapitulatif des fonctions non prises en charge
{: #summary-of-features-not-supported}

* L'accès Direct Link à VPC est pris en charge uniquement via l'[**accès classique**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* Bien qu'un sous-ensemble de noeuds finaux de service privés soit disponible dans VPC, les noeuds finaux ne sont pas tous disponibles. Voir [Noeuds finaux de service disponibles pour IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) afin d'obtenir les services disponibles.
* La sauvegarde et la restauration des images ne sont pas prises en charge.
* Les clouds privés virtuels d'{{site.data.keyword.cloud_notm}} sont régionaux, par conséquent un cloud privé virtuel d'une région ne peut pas se connecter à un cloud privé virtuel d'une autre région à moins d'avoir la fonction "accès classique" activée ou d'être connectés au moyen d'une connexion VPN. Voir [**Accès classique**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Fonctions et cas d'utilisation pas encore pris en charge
{: #features-and-use-cases-not-yet-supported}

Cette section présente plus de détails sur les fonctions et les cas d'utilisation non pris en charge, classés en fonction de leur service {{site.data.keyword.cloud_notm}}.

### Cloud privé virtuel
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} ne prend pas en charge les domaines de diffusion et multidiffusion.
* {{site.data.keyword.vpc_short}} ne fournit pas de chiffrement de bout en bout.
* Un cloud privé virtuel ne peut pas être apparié avec d'autres clouds privés virtuels.
* Les noeuds de service cloud ne sont pas pris en charge par VPC. Voir [Noeuds finaux de service disponibles pour IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) afin d'obtenir les services disponibles.

### Calcul
{: VSI-unsupported-features-and-use-cases}

* Les instances de serveur virtuel (VSI) {{site.data.keyword.cloud_notm}} existantes ou les serveurs bare metal ne peuvent pas être migrés sur un cloud privé virtuel.
* Les serveurs bare metal ne peuvent pas être mis à disposition dans un cloud privé virtuel.

### Réseau
{: network-unsupported-features-and-use-cases}

* Des routes personnalisées ne peuvent pas être ajoutées à un cloud privé virtuel (VPC). Tous les sous-réseaux d'un VPC peuvent communiquer les uns avec les autres par défaut.
* Un sous-réseau ne peut pas se trouver dans plusieurs zones.
* Un sous-réseau ne peut pas être déplacé d'une zone à une autre.
* Les sous-réseaux ne peuvent pas être redimensionnés après leur création.
* Les ressources {{site.data.keyword.cloud_notm}} Classic ne peuvent pas figurer dans le même sous-réseau au sein d'un cloud privé virtuel. La connectivité entre des ressources Classic et un cloud privé virtuel sur votre compte est possible via l'[**accès classique**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* Le protocole IPv6 n'est pas pris en charge.
* Vous ne pouvez pas utiliser votre propre adresse IP publique comme adresse IP flottante. Le pool d'adresses IP flottantes est fourni par IBM.
* Une adresse IP flottante est liée à une zone. Par exemple, une adresse IP flottante réservée depuis un pool dans la zone 1 ne peut pas être affectée à une instance de serveur virtuel dans la zone 2 dans un même cloud privé virtuel.
* Les adresses IP privées ne peuvent pas être déplacées d'une instance de serveur virtuel à une autre.
* Les interfaces réseau ne peuvent pas être déplacées d'une instance de serveur virtuel à une autre.
* Vous ne pouvez pas désigner l'adresse IP spécifique de l'instance de serveur virtuel. Vous pouvez uniquement spécifier la plage d'adresses IP pour le sous-réseau. Une fois que vous avez associé une instance de serveur virtuel à un sous-réseau, le système affecte une adresse IP privée depuis ce sous-réseau.
* {{site.data.keyword.vpc_short}} est livré avec ses propres outils NaaS (réseau en tant que service), tels qu'un équilibreur de charge en tant que service (LBaaS), des listes de contrôle d'accès (ACL) et des groupes de sécurité. Vous ne pouvez pas utiliser vos propres dispositifs réseau (comme une passerelle Vyatta ou un autre équilibreur de charge) pour contrôler une ressource dans le VPC. De même, vous ne pouvez pas utiliser votre propre service d'équilibreur de charge ou de groupes de sécurité dans l'infrastructure {{site.data.keyword.cloud_notm}} Classic pour contrôler des ressources dans {{site.data.keyword.vpc_short}}. 
* La passerelle publique n'autorise pas le trafic initié depuis Internet vers une instance de serveur virtuel dans IBM Cloud VPC. A ces fins, une adresse IP flottante doit être utilisée.
* Une liste de contrôle d'accès est sans état ; par conséquent, le trafic en retour doit être autorisé explicitement dans les règles de liste de contrôle d'accès.

## Problèmes connus dans cette édition
{: #known-issues-in-this-release}

{{site.data.keyword.block_storage_is_short}} risque de ne pas valider de manière précise l'unicité du nom du volume lorsque toutes les conditions suivantes sont réunies :

* La demande de mise à disposition est simultanée
* Le volume a un nom identique
* Le volume se trouve dans la même région

## Services disponibles en version bêta

L'appairage sécurisé d'un dispositif est disponible sous forme de service bêta. La documentation sur l'appairage de plusieurs types de dispositifs est disponible :

* [Homologue Vyatta distant](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [Homologue StrongSwan distant](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [Homologue FortiGate distant](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [Homologue Juniper vSRX distant](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [Homologue Cisco ASAv distant](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
