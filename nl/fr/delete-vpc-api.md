---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

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

# Suppression d'un cloud privé virtuel à l'aide des API REST
{: #deleting-using-api}

Le processus de [suppression](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) d'un cloud privé virtuel (VPC) {{site.data.keyword.cloud}} à l'aide des API REST suit les mêmes étapes générales que la suppression à l'aide de l'[interface de ligne de commande](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli).

Voici les étapes principales du processus :

1. Recherchez tous les sous-réseaux dans le VPC que vous souhaitez supprimer.
2. Supprimez chaque sous-réseau dans le VPC, ce qui signifie :
    - Supprimer toutes les passerelles VPN dans le sous-réseau.
    - Supprimer tous les équilibreurs de charge dans le sous-réseau.
    - Supprimer toutes les interfaces réseau (instances) dans le sous-réseau.
    - Supprimer le sous-réseau.
3. Supprimez toutes les passerelles publiques dans le VPC.
4. Supprimez le VPC.

Les sections suivantes présentent quelques exemples d'appels d'API à l'aide de la commande `curl`, que vous pouvez exécuter pour supprimer un VPC.

## Etape 1 :  recherchez tous les sous-réseaux dans le VPC que vous souhaitez supprimer.
{: #deleting-find-subnets-api}

Pour pouvoir supprimer un VPC, chacun de ses sous-réseaux doivent être supprimés. Obtenez l'ID du VPC que vous souhaitez supprimer en exécutant la commande suivante et en examinant la valeur du paramètre `id` :

Ajoutez ` | json_pp ` ou ` | jq ` après la commande curl pour obtenir une chaîne JSON lisible.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Enregistrez l'ID du VPC dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Pour obtenir la liste de tous les sous-réseaux figurant dans votre compte, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Examinez la valeur de `vpc` pour déterminer les sous-réseaux à supprimer. Enregistrez l'ID du sous-réseau dans une variable en vue d'une utilisation ultérieure, par exemple :

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Si le VPC que vous souhaitez supprimer comporte plusieurs sous-réseaux, répétez les étapes pour supprimer chaque sous-réseau.
{: important}

## Etape 2 : supprimez chaque sous-réseau du VPC
{: #deleting-subnet-resources-api}

### Supprimez toutes les passerelles VPN dans le sous-réseau, le cas échéant

Pour répertorier toutes les passerelles VPN de votre compte, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Pour chaque passerelle VPN du sous-réseau que vous voulez supprimer, exécutez la commande suivante, où `$vpnid` correspond à l'ID de la passerelle VPN.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Le statut de la passerelle VPN doit passer à `deleting` (suppression) immédiatement, mais elle est toujours renvoyée lorsque vous effectuez une requête de liste. La suppression d'une passerelle VPN peut prendre jusqu'à 30 minutes. Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que la passerelle VPN soit supprimée, mais le sous-réseau ne peut pas être supprimé tant que la passerelle VPN et toutes les autres ressources du sous-réseau continuent à apparaître dans les requêtes de liste.

### Supprimez tous les équilibreurs de charge dans le sous-réseau, le cas échéant
{: #deleting-lbs-api}

Pour répertorier tous les équilibreurs de charge dans votre compte, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Pour chaque équilibreur de charge du sous-réseau que vous voulez supprimer, exécutez la commande suivante, où `$lbid` correspond à l'ID de l'équilibreur de charge.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Le statut de l'équilibreur de charge doit passer à `deleting` (suppression) immédiatement, mais il est toujours renvoyé lorsque vous effectuez une requête de liste. La suppression d'un équilibreur de charge peut prendre jusqu'à 30 minutes. Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que l'équilibreur de charge soit supprimé, mais le sous-réseau ne peut pas être supprimé tant que l'équilibreur de charge et toutes les autres ressources du sous-réseau continuent à apparaître dans les requêtes de liste.

### Supprimez toutes les interfaces réseau dans le sous-réseau, le cas échéant
{: #deleting-nics-api}

Une instance peut comporter plusieurs interfaces réseau et ces interfaces réseau peuvent appartenir à plusieurs sous-réseaux du VPC. Pour pouvoir supprimer un sous-réseau, il faut d'abord supprimer toutes les interfaces réseau figurant dans le sous-réseau. Le seul moyen de supprimer une interface réseau dans un sous-réseau consiste à supprimer l'instance.

Pour répertorier toutes les instances dans votre compte, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Si vous supprimez toutes les instances dans le VPC, vous pouvez exécuter la commande suivante pour chaque instance du VPC, où `$vsi` correspond à l'ID de l'instance que vous souhaitez supprimer. Lorsque l'instance est supprimée, toutes les interfaces réseau figurant dans d'autres sous-réseaux seront automatiquement supprimées, le cas échéant.

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Le statut de l'instance doit passer à `deleting` (suppression) immédiatement, mais elle peut encore apparaître dans les résultats d'une requête de liste. La suppression d'une instance peut prendre jusqu'à 30 minutes. Vous pouvez demander la suppression en parallèle d'autres ressources de sous-réseau en attendant que l'instance soit supprimée, mais le sous-réseau ne peut pas être supprimé tant que l'instance et toutes les autres ressources du sous-réseau continuent à apparaître dans les requêtes de liste.

Pour répertorier toutes les interfaces réseau d'une instance, exécutez la commande suivante : 

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

S'il existe une interface réseau secondaire dans le sous-réseau que vous tentez de supprimer, vous devez supprimer l'instance. Une interface réseau ne peut pas être supprimée de l'instance sans supprimer l'instance.

### Supprimez le sous-réseau
{: #deleting-subnet-api}

Une fois que toutes les ressources à l'intérieur du sous-réseau ont été supprimées et ne sont pas renvoyées lorsque vous effectuez une requête de liste, exécutez la commande suivante pour supprimer le sous-réseau :

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Le statut du sous-réseau doit passer à `deleting` (suppression) immédiatement, mais la suppression du sous-réseau peut prendre quelques minutes avant qu'il disparaisse complètement des requêtes de liste.

## Etape 3 : supprimez toutes les passerelles publiques dans le VPC, le cas échéant
{: #deleting-pgs-api}

Pour répertorier toutes les passerelles publiques dans votre compte, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Pour chaque passerelle publique dans le VPC que vous souhaitez supprimer, exécutez la commande suivante pour supprimer la passerelle publique, où `$gateway` correspond à l'ID de la passerelle publique.

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## Etape 4 : supprimez le VPC
{: #deleting-single-vpc-api}

Dès que les sous-réseaux, les passerelles VPN, les équilibreurs de charge, les passerelles publiques du VPC ont été supprimés, exécutez la commande suivante pour supprimer le VPC, où `$vpc` correspond à l'ID du VPC que vous supprimez.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Le statut du VPC doit passer à `deleting` (suppression) immédiatement, mais la suppression du VPC peut prendre quelques minutes avant qu'il disparaisse complètement des requêtes de liste.
