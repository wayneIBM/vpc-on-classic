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

# Configuration de l'accès à votre infrastructure classique depuis VPC
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (rubrique d'aide liée)

Vous pouvez configurer l'accès à votre infrastructure classique d'{{site.data.keyword.cloud}}, notamment la connectivité Direct Link, depuis un cloud privé virtuel dans chaque région, pour n'importe quel compte. Ces "clouds privés virtuels à accès classique" spéciaux utilisent la même fonction de routage (routeur implicite) que l'infrastructure classique d'{{site.data.keyword.cloud}}. Les lecteurs sont censés connaître les fonctions réseau de l'infrastructure classique.

Une fois que vous avez configuré un cloud privé virtuel pour l'accès classique, chaque hôte de calcul (interface de serveur virtuel ou bare metal) sans interface publique sur votre compte classique peut envoyer et recevoir des paquets vers et depuis le cloud privé virtuel à accès classique. Toutefois, n'oubliez pas que les pare-feux, les passerelles, les listes de contrôle d'accès au réseau ou les groupes de sécurité peuvent filtrer ce trafic. Il est recommandé de n'autoriser que le trafic nécessaire au bon fonctionnement de vos applications.

Sur les hôtes avec une interface publique, vous devez ajouter une route renvoyant à votre cloud privé virtuel à accès classique.
{: important}

## Prérequis :
{: #vpc-prerequisites}

1. Votre compte classique doit être lié à votre compte IBM Cloud. Voir [Liaison des comptes IBMid](/docs/account?topic=account-unifyingaccounts) pour des instructions sur cette opération.
1. Votre compte classique doit être activé pour le service VRF (Routage et transfert virtuel).
    * Si Direct Link est déjà disponible sur votre compte, vous êtes prêt.
    * Si votre compte n'est pas compatible avec le service VRF, ouvrez un ticket afin de demander la "migration VRF" pour votre compte. Voir [Comment lancer la conversion](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion) dans notre documentation Direct Link pour en savoir plus sur le processus de conversion.

Des pare-feux, des passerelles, des listes de contrôle d'accès au réseau ou des groupes de sécurité peuvent exclure une partie ou l'intégralité du trafic réseau entre les ressources classiques et les ressources VPC.
{: important}

Tous les sous-réseaux dans un cloud privé virtuel à accès classique seront partagés dans la fonction VRF de l'infrastructure classique qui utilise des adresses IP dans l'espace `10.0.0.0/8`. Pour éviter les conflits d'adresse IP, **n'utilisez pas** d'adresses IP sur l'espace `10.0.0.0/8` lors de la création de sous-réseaux dans un cloud privé virtuel à accès classique.
{: important}

## Création d'un cloud privé virtuel à accès classique
{: #create-a-classic-access-vpc}

Vous pouvez créer un cloud privé virtuel avec une fonction d'accès classique depuis l'interface utilisateur, l'interface de ligne de commande ou l'interface de programmation (API).

Un cloud privé virtuel doit être configuré pour l'accès classique lorsqu'il est créé : vous ne pourrez pas le mettre à jour ultérieurement pour ajouter ou retirer la fonction d'accès classique.
{: note}

### Création d'un cloud privé virtuel à accès classique depuis l'interface utilisateur
{: #create-a-classic-access-vpc-from-the-user-interface}

Créez un cloud privé virtuel à accès classique depuis la page **Créer un VPC** en cliquant sur la case à cocher **Activer l'accès à une ressource classique**, sous le libellé **Accès classique**.

![classic-access-ui](/images/classic-access-ui.png)

### Création d'un cloud privé virtuel à accès classique via l'interface de ligne de commande
{: #create-a-classic-access-vpc-using-the-cli}

Utilisez l'indicateur `--classic-access` lorsque vous créez le cloud privé virtuel. Exemple :

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### Création d'un cloud privé virtuel à accès classique à l'aide de l'API
{: #create-a-classic-access-vpc-using-the-api}

Transmettez le paramètre `classic_access` lorsque vous créez le cloud privé virtuel. Exemple :

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Préfixes d'adresse par défaut d'un cloud privé virtuel à accès classique
{: #classic-access-default-address-prefixes}

Tous les sous-réseaux dans un cloud privé virtuel à accès classique seront partagés dans la fonction VRF de l'infrastructure classique qui utilise des adresses IP dans l'espace `10.0.0.0/8`. Pour éviter les conflits d'adresse IP, **n'utilisez pas** d'adresses IP sur l'espace `10.0.0.0/8` lors de la création de sous-réseaux dans un cloud privé virtuel à accès classique.
{: important}

Lorsqu'un nouveau cloud privé virtuel à accès classique est créé, des préfixes d'adresse par défaut sont affectés, en fonction de la région et de la zone, comme indiqué dans le tableau suivant :

Zone         | Préfixe d'adresse
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


## Limitations
{: #vpc-limitations}

* Seuls vos réseaux privés (ou "en arrière-plan" dans l'ancienne documentation) seront connectés au routeur implicite privé de votre compte. 
* Seuls les sous-réseaux alloués avec des API classiques sont connectés au côté classique de votre routeur implicite privé. La fonction de routage du routeur implicite ne fonctionne pas entre les sous-réseaux sur les réseaux locaux virtuels (VLAN) classiques.
* Un cloud privé virtuel et un seul par région et par compte peut être activé pour l'accès classique.

Votre procédure de configuration pourra varier en fonction du système d'exploitation que vous avez installé sur vos instances de serveur virtuel classiques ou vos serveurs bare metal. Pour plus d'informations, voir [A propos des serveurs virtuels publics](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
