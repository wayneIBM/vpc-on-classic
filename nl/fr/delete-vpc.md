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

# Suppression d'un cloud privé virtuel
{: #deleting}

Une ressource {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) ne peut pas être supprimée si elle contient d'autres ressources ou si elle est associée à une autre ressource. Le présent document décrit les relations entre les ressources VPC et fournit des informations sur les pratiques recommandées pour supprimer un cloud privé virtuel.

Le tableau suivant est un récapitulatif des types de ressources VPC et des relations à prendre en compte lorsque vous supprimez ces ressources.

| Ressource | Avant suppression... | Après suppression... |
| ---------------- | ----------------------------------------- | --------------------------- |
| Cloud privé virtuel | Tous les sous-réseaux et les passerelles publiques figurant dans le VPC doivent être supprimés | Tous les groupes de sécurité et les préfixes d'adresse sont supprimés automatiquement |
| Sous-réseau | Toutes les instances et les interfaces réseau figurant dans le sous-réseau doivent être supprimées ainsi que toutes les passerelles VPN et les équilibreurs de charge figurant dans le sous-réseau | Toute passerelle publique utilisée avec le sous-réseau sera déconnectée ainsi que toute liste de contrôle d'accès au réseau associée au sous-réseau |
| Instance | ---- | Toutes les interfaces réseau sont supprimées automatiquement ; le volume d'amorçage est supprimé avec l'instance ; la suppression du volume secondaire dépend du paramètre `delete_volume_on_instance_delete` ; toute adresse IP flottante associée aux interfaces réseau est automatiquement dissociée |
| Interface réseau | ---- | Toute adresse IP flottante associée à l'interface réseau est libérée |
| Clé | ---- | Une fois supprimée, une clé ne peut plus être utilisée pour la mise à disposition d'une nouvelle instance ou pour effectuer le rechargement d'un système d'exploitation sur une instance existante. Cependant, la clé reste disponible sur les instances que vous avez mises à disposition en l'utilisant et vous pouvez continuer à l'utiliser pour vous connecter. |
| Image | ---- | L'image ne peut pas être utilisée pour la mise à disposition d'une nouvelle instance, mais les instances existantes avec cette image ne sont pas affectées. |
| Volume | Le volume doit être déconnecté de toutes les instances | ---- |
| Liste de contrôle d'accès au réseau | La liste de contrôle d'accès au réseau doit être déconnectée de tous les sous-réseaux ; les listes de contrôle d'accès au réseau par défaut d'un VPC ne peuvent pas être supprimées  | ---- |
| Groupe de sécurité | Le groupe de sécurité doit être dissocié de toutes les interfaces réseau ; le groupe de sécurité par défaut d'un VPC ne peut pas être supprimé. | ---- |
| Adresse IP flottante | L'adresse IP flottante doit être dissociée de toute interface réseau | ---- |
| Passerelle publique | La passerelle publique doit être déconnectée de tous les sous-réseaux |  ---- |
| Equilibreur de charge | ---- | ---- |
| Passerelle VPN | ---- | En raison de la possibilité de partager les stratégies IKE & IPsec entre les passerelles, celles-ci ne sont pas supprimées lorsqu'une passerelle VPN est supprimée. Les stratégies IKE & IPsec doivent être retirées manuellement. |


## Les ressources VPC dans un état transitoire ne peuvent pas être supprimées
{: #deleting-status}

Les ressources VPC ne peuvent pas être supprimées lorsqu'elles sont dans un état transitoire, par exemple `pending` (en attente). Toutes les ressources doivent être dans un état "final", par exemple `available` (disponible) ou `failed` (échec), pour pouvoir être supprimées. Vous devez attendre que la ressource passe à l'état `available` ou `failed` avant d'essayer de la supprimer. [Contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si le statut d'une ressource n'est pas `available` ou `failed` au bout de 30 minutes.

Une fois que vous émettez une demande de suppression d'une ressource, la ressource passe à l'état transitoire `deleting` (suppression). Attendez que la ressource disparaisse de la liste avant d'essayer de supprimer le parent ou la ressource associée. Dans certains cas, l'opération de suppression peut échouer et la ressource peut passer à l'état `failed` en quelques minutes. Contactez le support si une ressource ne disparaît pas ou n'affiche pas le statut `failed` 30 minutes après avoir émis votre demande de suppression. Dès que la ressource passe à l'état `failed`, vous pouvez relancer sa suppression. [Contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si vous ne parvenez pas à supprimer une ressource dont le statut est `failed`.

## La suppression d'un cloud privé virtuel (VPC) s'effectue dans un certain ordre
{: #deleting-order}

Un VPC contient des sous-réseaux et des passerelles publiques. Une passerelle publique peut être connectée à un ou plusieurs sous-réseaux dans le VPC. Il existe un équilibreur de charge et une passerelle VPN dans un sous-réseau de VPC. Un sous-réseau peut également contenir des instances de serveur virtuel (VSI) et une instance VSI peut comprendre plusieurs interfaces réseau dans différents sous-réseaux au sein du VPC. Avant de supprimer un sous-réseau, il faut d'abord supprimer l'ensemble des passerelles VPN, des équilibreurs de charge et des interfaces réseau qui se trouvent dans le sous-réseau. Un VPC contient également des groupes de sécurité, mais ces groupes sont automatiquement supprimés lors de la suppression du VPC. Les préfixes d'adresse sont des attributs d'un VPC. Ils sont automatiquement supprimés lors de la suppression du VPC.

Supprimez un VPC en suivant cet ordre :

1.  Supprimez toutes les passerelles VPN mises à disposition dans tout sous-réseau du VPC.
2.  Déconnectez tous les équilibreurs de charge mis à disposition dans tout sous-réseau du VPC.
3.  Supprimez toutes les instances comportant des interfaces réseau dans tout sous-réseau du VPC. Toute adresse IP flottante associée à une interface réseau est automatiquement dissociée de cette interface lors de la suppression de l'instance.
4.  Supprimez tous les sous-réseaux dans le VPC. Toute passerelle publique connectée à un sous-réseau en est automatiquement déconnectée lors de la suppression du sous-réseau.
5.  Supprimez toutes les passerelles publiques dans le VPC.
6.  Une fois que le VPC n'a plus de sous-réseau et de passerelle publique, il peut être supprimé. Tous les préfixes d'adresse du VPC sont automatiquement supprimés lors de la suppression du VPC.

## Relations entre les ressources VPC
{: #deleting-relationships}

Certaines ressources VPC sont incluses dans d'autres ressources VPC (comme "enfants"), mais d'autres ressources VPC sont incluses au niveau du compte, en dehors de tout VPC spécifique. Tous les VPC enfants doivent être supprimés pour que la ressource parent puisse être supprimée. Pour les ressources VPC au niveau du compte, tous les liens vers les ressources existantes doivent être supprimés afin de pouvoir supprimer la ressource au niveau du compte.

Dans certains cas, la suppression d'une ressource VPC supprime automatiquement le lien vers des ressources existantes. Les tableaux suivants présentent le comportement attendu.

### Cloud privé virtuel (VPC)
{: #deleting-details}

Les VPC contiennent des sous-réseaux et des passerelles publiques. Des équilibreurs de charge, des passerelles VPN et des instances sont mises à disposition dans un sous-réseau. Avant de supprimer un VPC, tous les sous-réseaux et toutes les passerelles publiques figurant dans un VPC doivent être supprimés. En d'autres termes, il faut d'abord supprimer l'ensemble des passerelles publiques, des équilibreurs de charge et des instances dans le VPC.

Un VPC contient également des préfixes d'adresses et des groupes de sécurité, mais ces ressources sont supprimées automatiquement lors de la suppression du VPC.

Un VPC doit disposer d'un groupe de sécurité par défaut et d'une liste de contrôle d'accès au réseau par défaut. Si vous créez un VPC sans spécifier un groupe de sécurité et une liste de contrôle d'accès au réseau par défaut, ces éléments sont automatiquement créés pour le VPC. Vous ne pouvez pas les supprimer. La liste de contrôle d'accès au réseau par défaut est supprimée automatiquement lors de la suppression du VPC. Tous les groupes de sécurité du VPC sont automatiquement supprimés lors de la suppression du VPC.

| Dans le VPC | Peut contenir | Peut être associé à | A un statut ? | Suppression automatique lorsque le VPC est supprimé | Déconnexion automatique lors de la suppression |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Sous-réseau | instances, équilibreurs de charge, passerelles VPN | passerelle publique, liste de contrôle d'accès au réseau | Oui| Non | Oui|
| Passerelle publique| --- | sous-réseaux, adresse IP flottante | Oui | Non | Non pour les sous-réseaux, Oui pour l'adresse IP flottante |
| Groupes de sécurité | ---  | instances (NIC), VPC en tant que valeur par défaut | Non | Oui | Non |

### Sous-réseau
{: #deleting-subnet}

Toutes les ressources d'un sous-réseau doivent être supprimées pour pouvoir supprimer le sous-réseau. Les sous-réseaux contiennent les interfaces réseau, les équilibreurs de charge et les passerelles VPN de l'instance. L'interface réseau connectée au sous-réseau doit être supprimée pour pouvoir supprimer le sous-réseau, ainsi que les équilibreurs de charge ou la passerelle VPN mis à disposition dans le sous-réseau.

Une instance peut comporter plusieurs interfaces réseau et ces interfaces réseau peuvent appartenir à plusieurs sous-réseaux du VPC. Pour pouvoir supprimer un sous-réseau, il faut d'abord supprimer toutes les interfaces réseau figurant dans le sous-réseau. Il est impossible de supprimer l'interface réseau principale d'une instance ou de la transférer vers un autre réseau, par conséquent, toutes les instances avec une interface réseau principale dans le sous-réseau doivent être supprimées pour pouvoir supprimer le sous-réseau.


| Dans le sous-réseau | Peut contenir | Peut être associé à | A un statut ? | Suppression automatique lorsque le sous-réseau est supprimé | Déconnexion automatique lors de la suppression |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Instance (interface réseau) | plusieurs interfaces réseau | connexions de volume, groupes de sécurité | Oui| Non  | Oui|
| Réseau privé virtuel | --- | ---| Oui | Non  | --- |
| Equilibreur de charge | ---  | --- | Oui | Non | ---  |

### Instance
{: #deleting-instance}

Il n'y aucun prérequis nécessaire pour supprimer une instance. Lorsque l'instance est supprimée, toutes ses interfaces réseau sont supprimées automatiquement. Le volume d'amorçage de l'instance et toutes les connexions de volume associées sont supprimés. Toutes les adresses IP connectées à l'une de ses interfaces réseau sont libérées automatiquement. Tout volume de stockage par blocs connecté à l'instance est automatiquement supprimé, si le volume a été créé avec l'indicateur `delete_volume_on_instance_delete` défini avec la valeur true ; sinon, l'instance est déconnectée du volume mais le volume est conservé. Si un groupe de sécurité est associé à l'une des interfaces réseau de l'instance, il est également dissocié automatiquement lors de la suppression des interfaces réseau.

| Dans l'instance | Peut contenir | Peut être associé à | A un statut ? | Suppression automatique lorsque l'instance est supprimée | Déconnexion automatique lors de la suppression |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Interface réseau | --- | sous-réseaux, adresse IP flottante, groupes de sécurité | Non | Oui | Oui |

### Equilibreur de charge
{: #deleting-lb}

Il n'y aucun prérequis nécessaire pour supprimer un équilibreur de charge. Lorsque l'équilibreur de charge est supprimé, tous les écouteurs, les pools et les membres de pool faisant partie de l'équilibreur de charge sont automatiquement supprimés.

La suppression d'un équilibreur de charge peut prendre jusqu'à 30 minutes. La demande de suppression fait immédiatement passer le statut de mise à disposition de l'équilibreur de charge à `deleting`. Cependant, l'équilibreur de charge n'est réellement supprimé que lorsqu'il disparaît de la requête de liste.
{: important}

### Réseau privé virtuel (VPN)
{: #deleting-vpn}

Il n'y aucun prérequis nécessaire pour supprimer une passerelle VPN. Lorsque la passerelle VPN est supprimée, toutes les connexions associées sont également supprimées automatiquement. Les stratégies IKE & IPSec ne sont pas supprimées lors de la suppression d'une passerelle VPN. 

La suppression d'une passerelle VPN peut prendre jusqu'à 30 minutes. La demande de suppression fait immédiatement passer le statut de mise à disposition de la passerelle VPN à `deleting`. Cependant, la passerelle n'est réellement supprimée que lorsqu'elle disparaît de la requête de liste.
{: important}

### IP flottante
{: #deleting-floating-ip}

Il existe une adresse IP flottante en dehors d'un VPC, au niveau du compte. Si cette adresse IP est liée à une passerelle publique ou à une instance, le lien doit être supprimé (libéré) pour que l'adresse IP flottante puisse être supprimée.

Si vous supprimez une ressource à laquelle l'adresse IP flottante est liée, par exemple, si vous supprimez une interface réseau d'une instance, l'adresse IP est libérée automatiquement.

### Volume
{: #deleting-volume}

Il peut exister deux types de volumes dans un VPC : un volume d'amorçage de stockage par blocs et un volume de données de stockage par blocs. Ces volumes sont supprimés de manière différente.

**Suppression des volumes d'amorçage**

Les volumes d'amorçage se trouvent au sein d'une instance et ne sont pas considérés comme des entités distinctes de cette instance. Lorsque vous créez une instance, un volume d'amorçage est créé et connecté à cette instance. Ce volume est supprimé automatiquement lors de la suppression de l'instance. Vous ne pouvez pas supprimer le volume d'amorçage sans supprimer l'instance.

**Suppression des volumes de données**

Les volumes de données de stockage par blocs peuvent être mis à disposition et gérés séparément des instances de serveur virtuel associées. Vous pouvez connecter un volume de données en tant que stockage secondaire dans une instance de serveur virtuel. Vous ne pouvez pas supprimer un volume de données connecté à une instance (c'est-à-dire si le volume est "actif"). Vous devez d'abord déconnecter le volume de l'instance, pour pouvoir supprimer ensuite le volume.

Un volume de données a un attribut (ou indicateur) appelé `delete_volume_on_instance_delete` dans l'API et `Auto Delete` (Suppression automatique) dans l'interface de ligne de commande et l'interface utilisateur. Si cet indicateur est défini avec la valeur `true` (`Enabled` (Activé) dans l'interface utilisateur), lorsque l'instance avec le volume connecté est supprimée, le volume est déconnecté et supprimé automatiquement. Si l'indicateur du volume est défini avec la valeur `false` (`Disabled` (Désactivé) dans l'interface utilisateur), l'instance est déconnectée du volume mais le volume n'est pas supprimé lorsque l'instance connectée est supprimée. Il peut être connecté à une autre instance.

Un volume de stockage par blocs peut être connecté à un seul serveur virtuel à la fois.
{: tip}

### Groupes de sécurité
{: #deleting-secgroup}

Un groupe de sécurité ne peut pas être supprimé s'il est utilisé par une interface réseau ou s'il s'agit du groupe de sécurité par défaut d'un VPC. Avant de supprimer le groupe de sécurité, supprimez toutes les interfaces réseau de ce groupe de sécurité et assurez-vous que le groupe de sécurité n'est pas utilisé comme groupe de sécurité par défaut du VPC.

La suppression d'un VPC supprime tous les groupes de sécurité figurant dans ce VPC, automatiquement.

### Listes de contrôle d'accès au réseau
{: #deleting-netacl}

Une liste de contrôle d'accès au réseau ne peut pas être supprimée si elle est utilisée par un sous-réseau ou s'il s'agit de la liste de contrôle d'accès par défaut d'un VPC. Avant de supprimer la liste de contrôle d'accès au réseau, déconnectez-la de tous les sous-réseaux et assurez-vous qu'elle n'est pas utilisée comme liste de contrôle d'accès au réseau par défaut d'un VPC.

Lorsqu'un VPC est créé, il nécessite une liste de contrôle d'accès au réseau par défaut. Si aucune liste de ce type n'est indiquée comme valeur par défaut lors de la création du VPC, une nouvelle liste de contrôle d'accès au réseau est créée et définie comme valeur par défaut. Cette liste par défaut est supprimée automatiquement lors de la suppression du VPC, si elle n'est pas utilisée ailleurs.

Contrairement aux groupes de sécurité, les listes de contrôle d'accès au réseau par défaut peuvent être attribuées sur plusieurs VPC. Par conséquent, supprimer un VPC n'entraîne pas la suppression des listes de contrôle d'accès au réseau.
{: note}

## Etapes suivantes
{: #deleting-nextsteps}

Les rubriques suivantes fournissent d'autres exemples de suppression de ressources VPC à l'aide de la console, de l'interface de ligne de commande ou de l'API IBM Cloud.

-   [Suppression de ressources VPC à l'aide de l'interface utilisateur](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-console)
-   [Suppression de ressources VPC à l'aide de l'interface de ligne de commande](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)
-   [Suppression de ressources VPC à l'aide de l'API](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-api)
