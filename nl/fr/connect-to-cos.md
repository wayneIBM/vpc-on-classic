---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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

# Connexion à IBM Cloud Object Storage à partir de VPC
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

Ce document vous montre comment vous connecter à {{site.data.keyword.cloud}} Object Storage à partir de votre cloud privé virtuel (VPC). Les informations de noeud final s'appliquent spécifiquement à {{site.data.keyword.cloud_notm}} Virtual Private Cloud dans l'environnement d'infrastructure classique.
{: shortdesc}


## Qu'est-ce qu'IBM Cloud Object Storage (COS) ?
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) est une plateforme basée sur le Web qui stocke des données non structurées. Elle fournit fiabilité, sécurité, disponibilité et reprise après incident sans réplication. 

Les informations stockées avec {{site.data.keyword.cloud_notm}} Object Storage sont chiffrées et réparties sur plusieurs emplacements géographiques. Elles sont accessibles via une implémentation de l'API S3. Ce service utilise les technologies de stockage réparti fournies par IBM Cloud Object Storage System. 

IBM Cloud Object Storage est disponible avec trois types de configuration pour la résilience : **Inter-régional**, **Régional** et **Centre de données unique**. 
* Le service **Inter-régional** offre une durabilité et une disponibilité plus élevées que l'utilisation d'une région unique, au prix de temps d'attente légèrement plus longs. Ce service est disponible aujourd'hui dans les zones Etats-Unis, Europe et Asie-Pacifique. 
* Le service **Régional** inverse les compromis. Il distribue des objets sur les différentes zones disponibles au sein d'une seule région. Il est disponible dans les régions Etats-Unis, Europe et Asie-Pacifique. Si une région ou une zone indiquée n'est pas disponible, le stockage d'objets continue à fonctionner sans entrave.
* Le service **Centre de données unique** répartit les objets sur plusieurs machines au sein d'un même emplacement physique. Consultez ce document régulièrement pour obtenir les régions disponibles.

### Noeuds finaux directs de COS à utiliser avec VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Pour envoyer une demande d'API REST, vous devez définir un noeud final cible ou une URL, et chaque emplacement de stockage COS doit avoir son propre ensemble d'URL. Les noeuds finaux directs offrent à vos serveurs des connexions haut débit directes aux services {{site.data.keyword.cloud_notm}}, sans occasionner de coûts de bande passante supplémentaires. Avec le noeud final direct de COS, un cloud privé virtuel peut se connecter à un compartiment COS partout dans le monde. 

Il n'y a aucun frais lié au trafic en provenance de vos clouds privés virtuels vers tous les noeuds COS répertoriés sur cette page.
{: note}

* Un client VPC a également accès au compartiment COS en passant par le noeud final public.
* Un client VPC n'a accès à l'[API de configuration de COS](https://{DomainName}/apidocs/cos/cos-configuration) qu'en passant par le noeud final public, et non par le noeud final direct. L'API de configuration de COS peut être utilisée pour configurer certaines fonctions de COS sur les compartiments, ainsi que pour afficher les métadonnées du compartiment.
* Un client VPC n'a pas accès à un compartiment COS lorsque le pare-feu est activé.

## Connexion à IBM Cloud Object Storage (COS) depuis un cloud privé virtuel
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Mettez à disposition COS depuis le [catalogue ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}.
2. Créez un compartiment COS dans l'une des régions répertoriées à la section suivante.
3. Utilisez le noeud final pour communiquer avec votre compartiment COS.

### Noeuds finaux régionaux
{: #regional-endpoints}

Les compartiments créés au niveau d'un noeud final régional diffusent les données sur trois centres de données répartis sur une zone métropolitaine. Chacun de ces centres de données peut faire l'objet de panne ou de destruction, sans pour autant affecter la disponibilité.

| **Région** | **Noeud final** |
|------------|-------------------------------|
| Sud des Etats-Unis | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| Est des Etats-Unis | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| Europe (Royaume-Uni) | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| Europe (Allemagne) | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| Asie-Pacifique (Australie) | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| Asie-Pacifique (Japon) | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### Noeud-finaux inter-régionaux
{: #cross-region-endpoints}

Les compartiments créés au niveau d'un noeud final inter-régional diffusent les données dans trois régions. Chacune de ces régions peut faire l'objet de panne ou de destruction, sans pour autant affecter la disponibilité. Les demandes sont acheminées vers le centre de données le plus proche de la région à l'aide d'un routage BGP (Border Gateway Protocol).

En cas de panne, les demandes sont redirigées sur une région active, automatiquement. Les utilisateurs expérimentés qui souhaitent écrire leur propre logique de reprise après incident peuvent le faire en envoyant leurs demandes au noeud final de la région spécifique et en contournant le routage BGP.

| **Etats-Unis - Inter-régional** | **Noeud final** |
|------------|-------------------------------|
| Etats-Unis - Inter-régional | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| Point d'accès à San José | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **Europe - Inter-régional** | **Noeud final** |
|------------|-------------------------------|
| Europe - Inter-régional | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Amsterdam | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Francfort | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Milan | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **Asie-Pacifique - Inter-régional** | **Noeud final** |
|------------|-------------------------------|
| Asie-Pacifique - Inter-régional | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Tokyo | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Séoul | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Point d'accès à Hong Kong | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### Noeuds finaux de centre de données unique
 {: #single-datacenter-endpoints}

Les centres de données uniques ne sont pas colocalisés avec les services IBM Cloud, tels que IAM ou Key Protect, et ils n'offrent aucune résilience en cas de panne ou de destruction du site.

Si une panne réseau entraîne une séparation, par exemple si le centre de données ne parvient pas à atteindre une région principale d'IBM Cloud pour accéder à IAM, les informations d'authentification et d'autorisation sont lues à partir d'un cache qui peut être caduque. Cette action peut entraîner l'absence d'application de règles IAM, nouvelles ou modifiées, pendant un délai pouvant atteindre 24 heures.
{:important}

| **Région** | **Noeud final** |
|------------|-------------------------------|
| Amsterdam, Pays-Bas | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, Inde | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hong Kong | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Australie | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Mexico, Mexique | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Milan, Italie | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montréal, Canada | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Norvège | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San José, Etats-Unis | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brésil | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Séoul, Corée du Sud | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Canada | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
