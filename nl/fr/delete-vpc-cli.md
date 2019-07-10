---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

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
{:DomainName: data-hd-keyref="DomainName"}

# Suppression d'un cloud privé virtuel à l'aide de l'interface de ligne de commande d'IBM Cloud
{: #deleting-using-cli}

Cette rubrique présente des exemples de suppression de ressources d'{{site.data.keyword.cloud}} Virtual Private Cloud, pour chaque ressource de cloud privé virtuel (VPC), dans l'ordre suggéré. 

Voici quelques éléments clés à mémoriser concernant la suppression :

* Un VPC ne peut pas être supprimé tant qu'il n'est pas vide. 
* Toutes les ressources qu'il contient doivent être supprimées pour que la ressource parent puisse être supprimée. 
* Si la ressource est en cours de suppression, son statut affiche `deleting` lorsqu'elle est affichée dans les requêtes de liste. 
* La plupart des demandes de suppression sont _asynchrones_, ce qui signifie que le statut de la ressource peut être **deleting** dans les requêtes de liste alors qu'elle n'a pas encore été complètement supprimée. 
* Vous devez passer par une interrogation pour vérifier que la ressource enfant ne figure plus dans la liste avant d'essayer de supprimer la ressource parent. 

Si le statut de la ressource passe de `deleting` à `failed`, vous pouvez essayer de relancer la suppression de la ressource. Si vous ne parvenez pas à supprimer une ressource dont le statut est `failed`, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Etape 1 :  recherchez tous les sous-réseaux figurant dans le VPC que vous souhaitez supprimer.
{: #deleting-find-subnets-cli}

Pour pouvoir supprimer un VPC, chacun de ses sous-réseaux doivent être supprimés. Obtenez l'ID du VPC que vous souhaitez supprimer en exécutant la commande suivante et en examinant la valeur du paramètre `ID` :

```
ibmcloud is vpcs
```
{: pre}

Enregistrez l'ID du VPC dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Pour obtenir la liste de tous les sous-réseaux figurant dans votre compte, exécutez la commande suivante :

```
ibmcloud is subnets
```
{: pre}

Examinez la valeur de `VPC` pour déterminer les sous-réseaux à supprimer. Enregistrez l'ID du sous-réseau dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Si le VPC que vous souhaitez supprimer comporte plusieurs sous-réseaux, répétez ces étapes pour supprimer chaque sous-réseau.
{: important}

## Etape 2 : supprimez chaque sous-réseau du VPC
{: #deleting-subnet-resources-cli}

### Supprimez toutes les passerelles VPN dans le sous-réseau, le cas échéant

Pour répertorier toutes les passerelles VPN de votre compte, exécutez la commande suivante :

```
ibmcloud is vpn-gateways
```
{: pre}

Pour chaque passerelle VPN du sous-réseau que vous voulez supprimer, exécutez la commande suivante, où `$vpnid` correspond à l'ID de la passerelle VPN.

```
ibmcloud is vpn-gateway-delete $vpnid
```

Le statut de la passerelle VPN doit passer à `deleting` (suppression) immédiatement, mais elle apparaît toujours dans le résultat d'une requête de liste. La suppression d'une passerelle VPN peut prendre jusqu'à 30 minutes.
{: note}

Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que la passerelle VPN soit supprimée. Cependant, le sous-réseau ne peut pas être supprimé tant que la passerelle VPN et toutes les autres ressources du sous-réseau continuent à apparaître dans les résultats des requêtes de liste. 

### Supprimez tous les équilibreurs de charge dans le sous-réseau, le cas échéant
{: #deleting-lbs-cli}

Pour répertorier tous les équilibreurs de charge dans votre compte, exécutez la commande suivante :

```
ibmcloud is load-balancers
```
{: pre}

Pour chaque équilibreur de charge du sous-réseau que vous voulez supprimer, exécutez la commande suivante, où `$lbid` correspond à l'ID de l'équilibreur de charge.

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

Le statut de l'équilibreur de charge doit passer à `deleting` (suppression) immédiatement, mais il est toujours affiché dans le résultat d'une requête de liste. La suppression d'un équilibreur de charge peut prendre jusqu'à 30 minutes.
{: note}

Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que l'équilibreur de charge soit supprimé. Cependant, le sous-réseau ne peut pas être supprimé tant que l'équilibreur de charge et toutes les autres ressources du sous-réseau continuent à apparaître dans les résultats des requêtes de liste. 

### Supprimez toutes les interfaces réseau dans le sous-réseau, le cas échéant
{: #deleting-nics-cli}

Une instance peut comporter plusieurs interfaces réseau et ces interfaces réseau peuvent appartenir à plusieurs sous-réseaux du VPC. Pour pouvoir supprimer un sous-réseau, il faut d'abord supprimer toutes les interfaces réseau figurant dans le sous-réseau.  

Le seul moyen de supprimer une interface réseau dans un sous-réseau consiste à supprimer l'instance.
{: note}

Pour répertorier toutes les instances dans votre compte, exécutez la commande suivante :

```
ibmcloud is instances
```
{: pre}

Si vous supprimez toutes les instances dans le VPC, vous pouvez exécuter la commande suivante pour chaque instance du VPC, où `$vsi` correspond à l'ID de l'instance que vous souhaitez supprimer. Lorsque l'instance est supprimée, toutes les interfaces réseau figurant dans d'autres sous-réseaux seront automatiquement supprimées, le cas échéant.

```
ibmcloud is instance-delete $vsi
```
{: pre}

Le statut de l'instance doit passer à `deleting` (suppression) immédiatement, mais elle apparaît toujours dans le résultat d'une requête de liste. La suppression d'une instance peut prendre jusqu'à 30 minutes.
{: note}

Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que l'instance soit supprimée. Cependant, le sous-réseau ne peut pas être supprimé tant que l'instance et toutes les autres ressources du sous-réseau continuent à apparaître dans les résultats des requêtes de liste. 

Pour répertorier toutes les interfaces réseau d'une instance, exécutez la commande suivante : 

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

S'il existe une interface réseau secondaire dans le sous-réseau que vous tentez de supprimer, vous devez supprimer l'instance. Une interface réseau ne peut pas être supprimée de l'instance sans supprimer l'instance.

### Supprimez le sous-réseau
{: #deleting-subnet-cli}

Une fois que toutes les ressources à l'intérieur du sous-réseau ont été supprimées, ce qui signifie qu'elle ne sont pas renvoyées dans le résultat d'une requête de liste, exécutez la commande suivante pour supprimer le sous-réseau :

```
ibmcloud is subnet-delete $subnet
```
{: pre}

Le statut du sous-réseau doit passer à `deleting` (suppression) immédiatement, mais il peut s'écouler quelques minutes pour que la suppression du sous-réseau soit effective et qu'il n'apparaisse plus dans les résultats des requêtes de liste.

## Etape 3 : supprimez toutes les passerelles publiques dans le VPC, le cas échéant
{: #deleting-pgs-cli}

Pour répertorier toutes les passerelles publiques dans votre compte, exécutez la commande suivante :

```
ibmcloud is public-gateways
```
{: pre}

Pour chaque passerelle publique dans le VPC que vous souhaitez supprimer, exécutez la commande suivante pour supprimer la passerelle publique, où `$gateway` correspond à l'ID de la passerelle publique.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Etape 4 : supprimez le VPC
{: #deleting-single-vpc-cli}

Dès que les sous-réseaux, les passerelles VPN, les équilibreurs de charge, les passerelles publiques du VPC ont été supprimés, exécutez la commande suivante pour supprimer le VPC, où `$vpc` correspond à l'ID du VPC que vous supprimez.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

Le statut du VPC doit passer à `deleting` (suppression) immédiatement, mais il peut s'écouler quelques minutes pour que la suppression du VPC soit effective et qu'il n'apparaisse plus dans les résultats des requêtes de liste.
