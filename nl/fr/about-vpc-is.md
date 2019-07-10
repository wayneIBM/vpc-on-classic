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

# A propos de Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) est la plateforme IBM Cloud de nouvelle génération. VPC vous permet de créer votre propre espace dans IBM Cloud, afin d'exécuter un environnement isolé au sein du cloud public. VPC vous offre la sécurité d'un cloud privé avec l'agilité et la souplesse d'un cloud public.

Vous pouvez gérer des services réseau et lancer des serveurs virtuels selon les besoins pour la prise en charge de vos applications critiques, compatibles avec le cloud et natives pour le cloud. Principales fonctions disponibles :

* Création et gestion d'environnements d'application isolés via une API
* Définition de vos propres stratégies réseau conçues pour assurer la sécurité et la facilité d'accès
* Conception de topologies de réseau avec l'ajout de votre propre plage d'adresses IP (BYOIP, Bring Your Own IP)
* Mise à disposition de vos ressources en les connectant entre elles ou en les isolant
* Couverture de plusieurs régions pour la reprise après incident et la résilience
* Utilisation de zones de disponibilité permettant des connexions haut débit, à faible temps d'attente entre les régions, tout en assurant la haute disponibilité
* Utilisation d'unités de stockage et de mise en réseau à haute vitesse
* Autorisation systématique pour les services (plan de contrôle)
* Mise à disposition et utilisation de services de base : gestion des adresses IP, réseau privé virtuel, pare-feux, SSH, DNS et équilibrage de charge L4
* Configuration d'autres services : mise à l'échelle automatique, réseaux de distribution de contenu, virtualisation des fonctions réseau de tiers

La figure suivante "Présentation de VPC" illustre les composants VPC disponibles et montre comment ils interagissent pour vous offrir le service et la connectivité dont vous pouvez avoir besoin. N'hésitez pas à revenir à cette figure lorsque vous vous plongez dans des rubriques qui fournissent des détails supplémentaires sur chaque composant.

![Présentation d'IBM Cloud VPC](images/vpc-experience-simple.svg "Présentation d'IBM Cloud VPC")

Les sections suivantes vous donnent une vue d'ensemble des fonctions disponibles dans IBM Cloud VPC.

## Fonctions de mise en réseau

{{site.data.keyword.cloud_notm}} VPC propose des fonctions de mise en réseau faciles et complètes, notamment la possibilité de créer plusieurs clouds privés virtuels dans des [régions à zones multiples, disponibles dans le monde entier](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region), ainsi que des sous-réseaux dans différentes zones, la possibilité de [sélectionner une plage d'adresses IP](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), de mettre en place des pare-feux virtuels ([groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) et [listes de contrôle d'accès au réseau](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), des réseaux privés virtuels ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) de site à site et un équilibrage de charge ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) avec élasticité.

Un VPC est divisé en sous-réseaux à l'aide d'une plage d'adresses IP privées. Cependant, par défaut, toutes les ressources (comme les instances de serveur virtuel) dans un même cloud privé virtuel peuvent communiquer les unes avec les autres, quel que soit le sous-réseau associé. Les sous-réseaux se trouvent dans une seule zone et ne peuvent pas couvrir plusieurs zones, ce qui contribue à la sécurité, tout en réduisant les temps d'attente et en assurant la haute disponibilité. 

Créez facilement plusieurs VPC et sous-réseaux en utilisant les plages de préfixes suggérées et les règles de sécurité réseau préconfigurées, ou concevez et définissez vos propres préfixes d'adresse et règles de sécurité personnalisées.

Les blocs CIDR 161.26/16 et 166.8/14 sont réservés et routés dans chaque sous-réseau. Lisez les informations concernant les [noeuds finaux de service disponibles pour IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc). De plus, le bloc CIDR 10.240/13 est réservé aux préfixes d'adresse VPC et réparti entre les différentes zones [d'une manière prédéfinie](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes). Il n'est pas modifiable.
{: important}

### Accès Internet

Il existe trois options disponibles pour activer les communications de vos instances de serveur virtuel (VSI) avec l'Internet public :
* Utiliser une passerelle publique afin de permettre la communication Internet pour toutes les instances de serveur virtuel sur le sous-réseau associé. L'utilisation d'une passerelle publique n'est pas facturée, à l'inverse de l'utilisation de la bande passante.
* Utiliser une adresse IP flottante (FIP) afin de permettre la communication Internet en provenance et à destination d'une instance de serveur virtuel. 
* Utiliser une passerelle VPN. Pour plus d'informations, voir notre [documentation VPN pour VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)
{: note}

### Accès classique

Les utilisateurs de l'infrastructure {{site.data.keyword.cloud_notm}} existante peuvent connecter leur infrastructure classique, notamment la connectivité Direct Link, à un VPC par région en utilisant notre [accès classique](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

### Ajout de votre propre plage d'adresses IP (BYOIP)

Vous pouvez ajouter votre propre plage d'adresses IPv4 publiques à votre compte IBM Cloud VPC.

Lorsque vous utilisez BYOIP (Bring Your Own IP), {{site.data.keyword.cloud_notm}} doit configurer ces adresses IPv4 sur des ressources {{site.data.keyword.cloud_notm}}, ce qui entraînera l'envoi de paquets à destination et en provenance des adresses que vous avez fournies. Par conséquent, l'utilisation de la plage d'adresses IPv4 que vous avez indiquée sur {{site.data.keyword.cloud_notm}}, peut révéler ces adresses IP à l'équipe de support IBM et à des tiers.
{: important}

Vous devez utiliser l'API ou l'interface de ligne de commande pour pouvoir ajouter votre propre plage d'adresses IP. Cette fonction n'est pas disponible depuis l'interface utilisateur de la console IBM Cloud.
{: note}

### Connectivité globale

Vous pouvez définir la portée de vos applications et des ressources disponibles de sorte qu'elles s'étendent sur plusieurs régions. Avec un réseau privé virtuel ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)), vous pouvez créer des connexions privées vers d'autres projets et d'autres parties de vos déploiements de cloud hybride.

Comme il est facile de les partager et de les adapter, un réseau et des ressources VPC peuvent s'étendre de votre installation sur site à votre cloud.

## Sécurité

La sécurité est intégrée dans {{site.data.keyword.cloud_notm}} VPC, avec des [groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) qui font office de pare-feux virtuels pour assurer la protection au niveau de l'instance, et des [listes de contrôle d'accès au réseau](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACL) pour assurer la protection au niveau du sous-réseau.

## Haute disponibilité

{{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) vous permet d'isoler logiquement vos applications des autres réseaux dans toutes les régions, tout en assurant leur évolutivité et leur sécurité. Utilisez des [équilibreurs de charge](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc) pour répartir votre trafic réseau sur un ensemble de cibles afin d'améliorer les performances et la haute disponibilité. Les équilibreurs de charge surveillent également la santé de vos applications et de vos services. Vous pouvez configurer un équilibreur de charge pour répartir le trafic d'application entrant dans les instances d'une zone unique ou dans plusieurs zones d'une région.

## Capacité de traitement

Utilisez [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) pour mettre à disposition des ressources de traitement dans IBM Cloud. Créez des instances de serveur virtuel rapidement, à l'aide de [profils](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles) prédéfinis, optimisés pour vos charges de travail spécifiques. Lorsque vous mettez à disposition des instances [à plusieurs cartes d'interface réseau virtuel (vNIC)](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) et plusieurs emplacements, effectuez votre sélection dans notre stock d'[images](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) ou téléchargez une image personnalisée.

{{site.data.keyword.cloud_notm}} VPC ajoute une couche d'orchestration réseau qui élimine les limites de pods des instances de réseau virtuel, créant ainsi une capacité illimitée en termes de mise à l'échelle des instances. La couche d'orchestration réseau traite toute la mise en réseau de toutes les instances de serveur virtuel dans un cloud privé virtuel IBM Cloud VPC dans les différentes régions et zones. Avec les fonctions de mise en réseau définies par logiciel fournies par {{site.data.keyword.cloud_notm}} VPC, vous bénéficiez d'options supplémentaires pour les VPN, les équilibreurs de charge (LBaaS), les instances à plusieurs interfaces réseau virtuelle (vNIC) et de tailles de sous-réseaux plus importantes.

## Capacité de stockage

Lorsque vous mettez à disposition une instance d'{{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud, un volume de stockage par blocs de 100 Go, avec des E-S/s de type general-purpose (3 E-S/s par Go) est créé automatiquement en tant que volume d'amorçage principal et connecté à l'instance. Vous pouvez créer des volumes de stockage par blocs lorsque vous mettez à disposition une instance de serveur virtuel dans un réseau VPC ou lorsque vous créez de nouveaux volumes indépendamment du cycle de vie de l'instance de serveur virtuel.

Lorsque vous mettez à disposition du stockage supplémentaire pour votre VPC, vous pouvez sélectionner un [niveau d'E-S/s](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers) pour votre volume de stockage par blocs, ou spécifier un [profil d'E-S/s personnalisé](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom).

## Conception pour les charges de travail hybrides et natives pour le cloud

Un cloud privé virtuel est idéal pour les charges de travail natives pour le cloud et pour la liaison de votre infrastructure existante dans {{site.data.keyword.cloud_notm}}. Vous pouvez créer le meilleur cloud pour des charges de travail, de type calculs cognitifs, calculs d'intelligence artificielle et calculs d'apprentissage automatique. 

Voici d'autres possibilités prises en charge par VPC pour vos charges de travail hybrides, compatibles avec le cloud et natives pour le cloud :

* Equilibrage de charge avec mise à l'échelle horizontale
* Surveillance et contrôle de santé
* Prise en charge des processeurs graphiques (GPU)
* Mise à disposition d'instance de serveur virtuel, avec des groupes d'affinité et d'anti-affinité

## En savoir plus

* [**Terminologie relative à VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**Sécurité VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**Régions et sous-régions VPC disponibles**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**Création et gestion des ressources réseau dans VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**Création et gestion des instances de serveur virtuel dans VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**Création et gestion de stockage par blocs dans VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
