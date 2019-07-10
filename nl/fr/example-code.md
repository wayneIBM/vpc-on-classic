---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

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

# Création d'un cloud privé virtuel à l'aide des API REST
{: #creating-a-vpc-using-the-rest-apis}

Ce document vous montre comment créer des ressources {{site.data.keyword.cloud}} Virtual Private Cloud avec des API REST utilisant `curl`. Pour obtenir des fragments de code appelant les API REST en utilisant Go et Python, voir cet [exemple de code](https://github.com/IBM-Cloud/vpc-api-samples).

## Prérequis

1. Vérifiez que vous disposez d'une clé SSH publique, qui sera utilisée pour la connexion à l'instance de serveur virtuel.

   Vous avez peut-être déjà une clé SSH publique. Recherchez un fichier appelé `id_rsa.pub` sous un répertoire `.ssh` de votre répertoire principal, par exemple, `/Users/<USERNAME>/.ssh/id_rsa.pub`. Le fichier commence par `ssh-rsa` et se termine par votre adresse électronique.

   Si vous ne possédez pas de clé SSH publique ou si vous avez oublié le mot de passe d'une clé existante, générez-en une nouvelle en exécutant la commande `ssh-keygen` et en suivant les invites.
2.  Vérifiez que vous disposez d'une clé d'API pour votre compte IBM Cloud. Si vous n'avez pas de clé d'API, voir [Création d'une clé d'API](/docs/iam?topic=iam-userapikey#create_user_key). Il vous faudra stocker cette clé d'API dans une variable d'environnement à l'étape 1.

## Etape 1 : stockez votre clé d'API sous forme de variable

Exécutez la commande suivante pour stocker votre clé d'API dans une variable d'environnement :

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## Etape 2 : obtenez un jeton IBM IAM (Identity and Access Management)

Consultez la rubrique [Obtention d'un jeton IBM Cloud IAM à l'aide d'une clé d'API](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) pour savoir comment obtenir un jeton IAM ou utilisez les exemples de commande suivants.

Exécutez la commande suivante pour obtenir et analyser un jeton IAM à l'aide de l'utilitaire `jq`. Vous pouvez modifier la commande pour utiliser un autre outil d'analyse ou supprimer la dernière partie de la commande si vous préférez analyser vous-même le jeton manuellement.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

Pour afficher le jeton IAM, exécutez la commande `echo $iam_token`. Le résultat doit être similaire au code suivant :

```
Bearer <your token>
```

L'en-tête d'autorisation s'attend à ce que le jeton IAM commence par `Bearer`. Si le résultat que vous obtenez n'inclut pas `Bearer`, mettez à jour la variable `iam_token` pour l'inclure. Ces exemples supposent que `Bearer` est inclus dans le jeton IAM (`iam_token`).

Vous devez répéter l'étape précédente pour actualiser votre jeton IAM toutes les heures, car celui-ci expire.
{: important}

## Etape 3 : stockez le noeud final d'API et le paramètre de version sous forme de variable

Les noeuds finaux d'API s'appliquent par région et suivent la convention `https://<region>.iaas.cloud.ibm.com`. Les noeuds finaux par région sont les suivants :

* Sud des Etats-Unis (US-SOUTH) : `https://us-south.iaas.cloud.ibm.com`
* Europe (Allemagne) (EU-DE) : `https://eu-de.iaas.cloud.ibm.com`
* Tokyo (Japon) (JP-TOK) : `https://jp-tok.iaas.cloud.ibm.com`

Stockez le noeud final d'API dans une variable pour pouvoir l'utiliser dans des demandes d'API sans avoir à saisir l'URL complète. Par exemple, pour stocker le noeud final us-south dans une variable, exécutez cette commande :

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

Pour vérifier que cette variable a été enregistrée, exécutez `echo $rias_endpoint` et assurez-vous que la réponse n'est pas vide.

Toutes les demandes d'API doivent inclure le paramètre `version`, au format `YYYY-MM-DD`. Exécutez la commande suivante pour stocker la date de version dans une variable pour pouvoir la réutiliser dans votre session. Pour plus d'informations sur la définition du paramètre `version`, voir **Gestion des versions** dans le document [Référence d'API pour VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

Pour vérifier que cette variable a été enregistrée, exécutez `echo $version` et assurez-vous que la réponse n'est pas vide. 

## Etape 4 : exécutez l'API GET regions

La commande ci-dessous renvoie les régions disponibles pour le cloud privé virtuel au format JSON. Un objet au moins doit être renvoyé. 

Un paramètre de requête `version` et `generation` est requis dans chaque demande d'API. Pour plus de détails, voir le document [Référence d'API pour VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Si vous rencontrez des résultats inattendus, ajoutez l'indicateur `--verbose` (debug) après la commande `curl` pour obtenir des informations de journalisation détaillées. Consultez également les erreurs les plus courantes sur notre [page relative au traitement des incidents](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Etape 5 : exécutez l'API GET zones

La commande suivante renvoie toutes les zones disponibles pour VPC dans la région `us-south`, au format JSON. Trois objets au moins doivent être renvoyés. 

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Mettez à jour le nom de la région dans le paramètre, `us-south` ci-dessus, en fonction du noeud final de la région que vous utilisez. Un noeud final de région ne connaît
que ses propres zones, par conséquent, si vous utilisez le noeud final à TOKYO, utilisez `jp-tok`.
{: note}

## Etape 6 : exécutez l'API GET profiles

La commande ci-dessous renvoie les profils disponibles pour vos instances au format JSON. Un objet au moins doit être renvoyé. 

Ajoutez ` | json_pp ` après la commande curl pour obtenir une chaîne JSON lisible.
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Si la sortie de l'API est limitée à cause de la pagination, définissez une limite supérieure à `total_count`, par exemple :

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etape 7 : exécutez l'API GET images

La commande ci-dessous renvoie les images disponibles pour vos instances au format JSON. Un objet au moins doit être renvoyé. 

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etape 8 : exécutez l'API GET VPCs

La commande ci-dessous renvoie les clouds privés virtuels (VPC) qui ont été créés sur votre compte, au format JSON. 

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etape 9 : créez un cloud privé virtuel 

Créez un cloud privé virtuel (VPC) {{site.data.keyword.cloud_notm}} appelé `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

Pour les autres appels, vous devez connaître l'ID du VPC {{site.data.keyword.cloud_notm}} que vous venez de créer. Stockez cet ID dans une variable. La commande sera semblable à ceci : `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## Etape 10 : créez un sous-réseau 

Créez un sous-réseau dans votre cloud privé virtuel (VPC) {{site.data.keyword.cloud_notm}}. L'exemple suivant crée un VPC dans la zone `us-south-2`.

L'exemple suivant utilise le [préfixe d'adresse par défaut](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) pour la zone `us-south-2`. Les valeurs par défaut peuvent avoir changé pour votre compte, voir [comment planifier vos adresses VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

Pour obtenir la liste des préfixes d'adresse correspondant à votre VPC, exécutez la commande suivante :
`curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`.
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Stockez l'ID du sous-réseau dans une variable.

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## Etape 11 : vérifiez le statut de votre sous-réseau

Pour mettre à disposition des ressources sur votre sous-réseau, celui-ci doit avoir le statut `available` (disponible). Avant de continuer, interrogez la ressource de sous-réseau et assurez-vous que son statut est `available` avant de continuer. Si le statut est `failed` (échec), communiquez les détails au [support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support). Vous pouvez essayer de mettre à disposition un autre sous-réseau.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Etape 12 : créez une passerelle publique 

Pour que les instances de serveur virtuel sur le sous-réseau aient accès à l'Internet public, ajoutez une passerelle publique au sous-réseau.  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Stockez l'ID de la passerelle publique dans une variable.

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

Pour connecter et utiliser la passerelle publique, la passerelle doit avoir le statut `available` (disponible). Avant de continuer, interrogez la ressource de passerelle publique et assurez-vous que son statut est `available` avant de continuer. Si le statut est `failed` (échec), communiquez les détails au [support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support). Vous pouvez essayer de mettre à disposition une autre passerelle publique.

Pour vérifier le statut de la passerelle publique, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etape 13 : connectez la passerelle publique au sous-réseau

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Etape 14 : créez une clé SSH

Créez une clé avec votre clé SSH publique. Cette clé est utilisée lorsque vous créez l'instance de serveur virtuel. Elle est également nécessaire pour se connecter à cette instance.

Des clés peuvent être ajoutées la première fois que vous créez le cloud privé virtuel dans l'interface utilisateur ou l'interface de ligne de commande. Il n'existe aucun outil permettant d'ajouter des clés par la suite.
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

Stockez l'ID de la clé dans une variable.

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## Etape 15 : sélectionnez un profil et une image pour votre instance de serveur virtuel

Exécutez les API pour répertorier tous les profils et toutes les images disponibles pour votre instance de serveur virtuel, et choisissez une combinaison.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Pour obtenir des détails supplémentaires sur les profils, vous pouvez interroger le catalogue global à l'aide de la commande d'interface de ligne de commande d'{{site.data.keyword.cloud_notm}} `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`. Exemple :

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

La commande suivante répertorie les images disponibles :

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Stockez le nom du profil et l'ID de l'image dans des variables, que vous utiliserez par la suite pour mettre à disposition une instance de serveur virtuel.

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## Etape 16 : mettez à disposition une instance de serveur virtuel

Mettez à disposition une instance de serveur virtuel (VSI) dans le sous-réseau que vous venez de créer. Transmettez votre clé SSH publique pour pouvoir vous connecter une fois l'instance mise à disposition.

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-south-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

Stockez l'ID de l'instance de serveur virtuel dans une variable.

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## Etape 17 : vérifiez le statut de votre instance de serveur virtuel

La mise à disposition d'une instance de serveur virtuel peut prendre quelques minutes. Avant de continuer, interrogez le statut du serveur et assurez-vous qu'il s'agit de `running` (en fonctionnement).

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Stockez l'ID de l'interface réseau principale de l'instance de serveur virtuel (renvoyé dans l'appel d'API précédent) dans une variable.  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

Vous ne pouvez pas obtenir l'interface réseau principale si vous n'interrogez pas l'instance spécifique.
{: note}

## Etape 18 : créez une adresse IP flottante

Pour créer une adresse IP flottante pour l'instance de serveur virtuel, utilisez l'interface réseau principale de l'instance comme cible pour la nouvelle adresse IP flottante. 

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Stockez l'ID de l'adresse IP flottante dans une variable.

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## Etape 19 : ajoutez une règle au groupe de sécurité par défaut pour autoriser le trafic SSH

Configurez le groupe de sécurité pour définir le trafic entrant et sortant qui est autorisé pour l'instance.

Recherchez le groupe de sécurité pour le cloud privé virtuel :

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Stockez l'ID du groupe de sécurité dans une variable.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Créez à présent une règle pour autoriser le trafic SSH entrant :

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Etape 20 : établissez une liaison SSH à votre instance de serveur virtuel

Pour établir une liaison SSH au serveur, utilisez l'élément `address` de l'adresse IP flottante que vous avez créée précédemment. Pour obtenir la valeur `address` de l'adresse IP flottante, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Utilisez l'élément `address` de l'adresse IP flottante pour établir la connexion à l'instance de serveur virtuel avec SSH :

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

L'accès SSH au serveur virtuel peut être empêché par des groupes de sécurité. Si un accès SSH est requis, vous devez utiliser un [groupe de sécurité approprié](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).
{: note}

Voir [Connecting to your instance using Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) pour plus d'informations sur la connexion à votre instance.

## Etape 21 : créez et connectez un volume de stockage par blocs

Créez un volume de stockage par blocs avec une demande semblable à cet exemple et indiquez un nom de volume, une zone et un profil.

Pour voir la liste des profils de volume, effectuez cette demande :

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

Les profils peuvent être general-purpose (3 ES/s/Go), 5iops-tier, 10iops-tier et custom. Voir [A propos de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
pour obtenir des informations sur la capacité du volume et les plages d'E-S/s en fonction du profil que vous sélectionnez.

Il n'est pas nécessaire d'indiquer un nom de volume dans la demande car le système en crée un par défaut.  Cet exemple indique le nom de volume, `helloworld-vol`.  Tous les noms de volume doivent être uniques.

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Pour les autres appels, vous devez connaître l'ID du volume qui vient d'être créé. Enregistrez l'ID du volume sous forme de variable, par exemple : `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

Le statut du volume est `pending` lors de sa création. Pour que vous puissiez continuer, le volume doit passer au statut `available`, ce qui prend quelques minutes.
{: note}


Pour vérifier le statut du volume, exécutez la commande suivante :

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Créez une connexion de volume pour connecter le nouveau volume à l'instance de serveur virtuel. Utilisez la variable d'ID de l'instance que vous avez créée auparavant dans la demande.  Utilisez l'ID et le CRN du volume ou l'URL pour indiquer le volume dans les paramètres de données.  Cet exemple utilise la variable que nous avons créée pour l'ID du volume.

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

Après avoir créé la connexion de volume, obtenez l'ID de connexion de volume.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Enregistrez l'ID de connexion sous forme de variable.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

Indiquez l'ID de la connexion de volume pour connecter le nouveau volume à une instance de serveur virtuel.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Etape 22 (facultative) : supprimez les ressources

Supprimez éventuellement les ressources. Une ressource ne peut pas être supprimée si elle contient d'autres ressources. Par exemple, un cloud privé virtuel ne peut pas être supprimé s'il contient des sous-réseaux, et un sous-réseau ne peut pas être supprimé s'il contient des instances de serveur virtuel. Lors d'une opération de suppression, l'API peut indiquer que la commande a été exécutée, alors que la suppression des ressources peut encore être en cours. Après avoir émis la demande de suppression, vérifiez que la ressource a bien été supprimée avant d'essayer de supprimer la ressource parent. Voir [Suppression d'un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) pour plus de détails.

### Supprimez l'adresse IP flottante

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Supprimez l'instance de serveur virtuel

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Supprimez la clé

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Supprimez le sous-réseau

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Supprimez la passerelle publique

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### Supprimez le VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Supprimez le volume

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## Félicitations !

Vous avez mis à disposition une instance de cloud privé virtuel à l'aide des API REST. Pour essayez d'autres commandes d'API, découvrez le document de référence complet :

* [Référence d'API pour VPC](https://{DomainName}/apidocs/vpc-on-classic)
