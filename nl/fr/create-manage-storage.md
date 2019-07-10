---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# Création et gestion de stockage par blocs dans VPC
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud fournit un volume d'amorçage et autorise d'autres volumes de stockage par blocs, qui constituent un stockage de données haute performance monté par l'hyperviseur pour vos instances de serveur virtuel (VSI). L'infrastructure VPC offre une mise à l'échelle rapide du stockage sur plusieurs régions et zones, avec des performances et une sécurité accrues.

## Récapitulatif des caractéristiques
{: #summary-of-characteristics}

Lorsque vous mettez à disposition une instance d'{{site.data.keyword.vsi_is_full}}, un volume de stockage par blocs de 100 Go, avec des E-S/s de type general-purpose (3 E-S/s par Go) est créé automatiquement en tant que volume d'amorçage principal, connecté à l'instance de serveur virtuel.
Lors de la mise à disposition, vous pouvez renommer le volume d'amorçage et modifier la taille du volume.
{:shortdesc}

* Vous pouvez créer des volumes de stockage par blocs dès que vous mettez à disposition une instance de serveur virtuel dans un réseau VPC.  
* Vous pouvez créer des volumes, indépendamment du cycle de vie de l'instance de serveur virtuel et connecter par la suite ces volumes à une instance de serveur virtuel.
* Vous pouvez créer des volumes de données secondaires qui sont connectés automatiquement à l'instance de serveur virtuel. 
* Les volumes de données sont conservés lorsqu'ils sont déconnectés de l'instance de serveur virtuel. Par conséquent, vous pouvez connecter un volume à une nouvelle instance ultérieurement. 
* Vous pouvez spécifier si les volumes de données doivent être automatiquement supprimés lorsque vous supprimez une instance de serveur virtuel.  
* Les volumes d'amorçage sont supprimés lorsque vous supprimez l'instance de serveur virtuel associée.
* Par défaut, les volumes d'amorçage et les volumes de données sont chiffrés avec un chiffrement géré par IBM. 
* Vous pouvez chiffrer vos volumes à l'aide de vos propres clés de chiffrement, lors de la mise à disposition d'une instance de serveur virtuel ou lors de la création d'un volume autonome.


## Volumes de stockage par blocs
{: #block-storage-volumes}

Des informations plus complètes sur l'utilisation des volumes de stockage par blocs et de VPC sont disponibles dans notre [documentation Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started).

Pour obtenir des informations générales sur les volumes de stockage par blocs pour VPC, voir [A propos de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

Pour commencer à créer des volumes indépendamment de la mise à disposition d'instance de serveur virtuel, voir [Getting Started with {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started).



## Meilleures pratiques pour la création et la désignation de vos volumes de stockage par blocs VPC :
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Déterminez vos exigences en matière de stockage avant de mettre à disposition un volume. Veillez à fournir la capacité et les niveaux de performance d'E-S/s appropriés.
* Déterminez si un profil d'E-S/s convient le mieux à vos besoins en termes de capacité et de performances ou s'il est préférable de spécifier des paramètres personnalisés.
* Déterminez si vous souhaitez créer des volumes de données secondaires lors de la mise à disposition de l'instance de serveur virtuel. Ces volumes sont automatiquement connectés à l'instance.
* Déterminez si vous souhaitez que le volume connecté à l'instance de serveur virtuel soit automatiquement supprimé lorsque vous supprimez l'instance.
* Lorsque vous chiffrez les volumes avec vos propres clés de chiffrement, assurez-vous que la clé est valide, que vous avez l'autorisation d'utiliser la clé et qu'elle peut être utilisée dans votre région actuelle.
* Assurez-vous de nommer TOUS vos volumes au moment de la mise à disposition, y compris le volume d'amorçage.
* Vérifiez que vous nommez TOUTES vos connexions de volume au moment de la connexion.
* Chaque volume doit avoir un nom différent dans une région sur un compte.

Vous pouvez réutiliser le nom d'un volume après la suppression de ce volume. Toutefois, sachez que la réutilisation du nom d'un volume peut prêter à confusion au moment de la facturation.
{:note}
