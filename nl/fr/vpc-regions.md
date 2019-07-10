---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

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

# Création d'un cloud privé virtuel dans une autre région
{: #creating-a-vpc-in-a-different-region}

Une région est un emplacement géographique spécifique dans lequel vous pouvez déployer des applications, des services et d'autres ressources {{site.data.keyword.cloud}}. Les régions sont constituées d'une ou plusieurs zones, lesquelles correspondent à des centres de données physiques qui contiennent les ressources de calcul, les ressources réseau et les ressources de stockage, avec les systèmes de ventilation et d'alimentation, pour héberger les applications et les services. Les zones sont isolées les unes des autres afin de garantir qu'il n'y a pas de partage de point de défaillance unique au sein d'une région. 

Le service IBM Cloud VPC est régional. Pour permettre la reprise après incident, vous devez faire en sorte que la reprise s'effectue par un basculement sur une autre région, en établissant un cloud privé virtuel dans l'une de nos autres régions.
{: note}

Le déploiement de Virtual Private Cloud s'effectue dans toutes les régions {{site.data.keyword.cloud}} en plusieurs phases.

|   Emplacement     | Région | Noeud final d'API | Statut |
| ------- | :------: | :------: |:------: |
| Dallas | us-south | `us-south.iaas.cloud.ibm.com`| Disponible |
| Francfort | eu-de | `eu-de.iaas.cloud.ibm.com`| Disponible |
| Tokyo | jp-tok | `jp-tok.iaas.cloud.ibm.com`| Disponible |
| Londres | eu-gb | `eu-gb.iaas.cloud.ibm.com`| Bientôt disponible |
| Sydney | au-syd | `au-syd.iaas.cloud.ibm.com`| Bientôt disponible |
| Washington DC | us-east | `us-east.iaas.cloud.ibm.com`| Bientôt disponible |

Le noeud final d'API (VPC) régional est défini automatiquement par l'interface de ligne de commande d'IBM Cloud lorsque vous vous connectez à une région spécifique.
{: note}

## Connexion à une région spécifique via l'interface de ligne de commande
{: #log-in-to-a-specific-region-using-the-cli}

Lorsque vous vous connectez à IBM Cloud, vous pouvez spécifier une région ou la choisir ultérieurement. Par exemple, pour vous connecter directement au noeud final d'API global dans la région de Dallas (`us-south`), exécutez les commandes suivantes, qui varient selon que vous disposez d'un compte fédéré (SSO) ou non fédéré : 

Pour un compte fédéré

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

Pour un compte non fédéré

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

Pour choisir la région ultérieurement, n'indiquez pas le paramètre `-r <region>` et l'interface de ligne de commande vous invitera à choisir une région.

Exemple de sortie :

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## Changement de région via l'interface de ligne de commande
{: #switch-regions-using-the-cli}

Pour obtenir le statut le plus récent des régions VPC, exécutez la commande suivante : 

```
ibmcloud is regions
```
{: pre}

Pour changer de région, exécutez la commande `ibmcloud target -r <region>`. Par exemple, pour passer à la région Francfort, exécutez la commande suivante :

```
ibmcloud target -r eu-de
```
{: pre}

Pour vérifier votre emplacement actuel, exécutez la commande suivante :

```
ibmcloud target
```
{: pre}

## Changement de région à l'aide de l'API  
{: #switch-regions-using-the-api}

Pour interagir avec l'API VPC régionale avec REST, dirigez votre demande sur le noeud final d'API associé à la région dans laquelle vous souhaitez créer des ressources. Le noeud final d'API de la région est affiché dans le tableau précédent. Vous pouvez également trouver les noeuds finaux associés aux régions en exécutant la commande suivante :

```
ibmcloud is regions
```
{: pre}


Par exemple, pour obtenir la liste des clouds privés virtuels qui se trouvent dans la région `us-south`, exécutez la commande suivante :

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## Obtention des zones
{: #get-zones}

Pour obtenir la liste des zones disponibles pour chaque région, exécutez la commande `ibmcloud is zones <region>`. Par exemple, pour obtenir la liste des zones qui se trouvent dans la région `us-south`, exécutez la commande suivante :

```
ibmcloud is zones us-south
```
{: pre}
