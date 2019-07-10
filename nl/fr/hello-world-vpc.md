---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-29"

keywords: create, VPC, CLI, resources, plugin, SSH, key, hello, world, provision, instance, subnet

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Création d'un cloud privé virtuel à l'aide de l'interface de ligne de commande d'IBM Cloud
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (rubrique d'aide liée)

Ce document vous montre comment créer des ressources {{site.data.keyword.cloud}} Virtual Private Cloud via l'interface de ligne de commande d'IBM Cloud. 

## Prérequis :
{: #cli-pre-requisites}

1. Installez l'[interface de ligne de commande d'IBM Cloud](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview).

2. Installez ou mettez à jour le plug-in `vpc-infrastructure` dans l'interface de ligne de commande d'IBM Cloud.

  Pour procéder à l'installation :

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  Pour procéder à la mise à jour :

  ```
  ibmcloud plugin update
  ```
  {: pre}

  Pour afficher les plug-in installés et les versions :

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. Générez une clé SSH publique pour mettre à disposition des instances de serveur virtuel.

Vous avez peut-être déjà une clé SSH publique. Recherchez un fichier appelé ``id_rsa.pub`` sous un répertoire ``.ssh`` de votre répertoire principal, par exemple, ``/Users/<USERNAME>/.ssh/id_rsa.pub``. Le fichier commence par ``ssh-rsa`` et se termine par votre adresse électronique.

Si vous ne possédez pas de clé SSH publique ou si vous avez oublié le mot de passe d'une clé existante, générez-en une nouvelle en exécutant la commande ``ssh-keygen`` et en suivant les invites.


## Etape 1 : connectez-vous à IBM Cloud
{: #step-1-log-in-to-ibm-cloud}

Si vous disposez d'un compte fédéré :
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

sinon

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

Pendant le processus de connexion, vous serez invité à choisir une région. Le VPC n'est pas forcément disponible dans toutes les régions. Consultez la rubrique [Création de VPC dans différentes régions](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region) pour voir les régions disponibles à ce jour.
{: important}

## Etape 2 : définissez une cible de génération

Utilisez la commande suivante pour définir une cible de génération.

```
ibmcloud is target --gen 1
```
{: pre}


## Etape 3 : créez un cloud privé virtuel (VPC) et enregistrez l'ID de ce VPC 
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Utilisez la commande ci-dessous pour créer un cloud privé virtuel nommé _helloworld-vpc_.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

La sortie se présente comme suit : 

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

Enregistrez l'ID dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## Etape 4 : créez un sous-réseau et enregistrez l'ID de ce sous-réseau 
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Choisissez la zone `us-south-2` correspondant à l'emplacement du sous-réseau et nommez le sous-réseau _helloworld-subnet_.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

La sortie se présente comme suit : 

```
Creating Subnet helloworld-subnet in resource group Default under account <Account Name> as user <User>...

ID               50ba0da9-279a-4791-b7cb-cd3d7b2bc14d   
Name             helloworld-subnet   
IPv*             ipv4   
IPv4 CIDR        10.240.64.0/29  
IPv6 CIDR        -   
Addr available   3   
Addr Total       8   
Gen              -   
ACL              allow-all-network-acl-ba9e785a-3e10-418a-811c-56cfe5669676(e9c2790b-cee2-465a-8539-d8cd90d33621)   
Gateway          -   
Created          now   
Status           pending   
Zone             us-south-2   
VPC              helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)   
```
{:screen}

Enregistrez l'ID dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

Notez que le statut du sous-réseau est `pending` (en attente) lorsqu'il est créé. Pour que vous puissiez continuer, le sous-réseau doit passer au statut `available` (disponible), ce qui prend quelques secondes. Pour vérifier le statut du sous-réseau, exécutez la commande suivante :

```
ibmcloud is subnet $subnet
```
{: pre}

## Etape 5 : connectez une passerelle publique 
{: #step-5-attach-a-public-gateway}

Connectez une passerelle publique au sous-réseau pour permettre à toutes les ressources associées de communiquer avec l'Internet public.

Pour créer une passerelle publique, exécutez la commande suivante :

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Enregistrez l'ID dans une variable pour pouvoir l'utiliser ultérieurement, par exemple :

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

Pour connecter la passerelle publique à votre sous-réseau, exécutez la commande suivante :
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


Il n'y a qu'une seule passerelle publique autorisée par zone dans un cloud privé virtuel, mais elle doit être connectée à plusieurs sous-réseaux dans la zone. Pour trouver la passerelle publique d'une zone, exécutez la commande 'ibmcloud is public-gateways` et recherchez les valeurs particulières de VPC et Zone.
{: tip}

La sortie se présente comme suit : 

```
Updating Subnet f174ca4d-cb44-4e4d-a0f3-afce151f4198 under account Account as user ...

ID               f174ca4d-cb44-4e4d-a0f3-afce151f4198   
Name             helloworld-subnet   
IPv*             ipv4   
IPv4 CIDR        10.240.64.0/29   
IPv6 CIDR        -   
Addr available   3   
Addr Total       8   
ACL              allow-all-network-acl-6dcd8188-f1e2-48ca-b872-d04cf54479c1(d589e5dc-3b95-4984-9191-174eed7d1f98)   
Gateway          my-gateway(446c0c63-f0b1-4043-b30d-644f55fde391)   
Created          41 minutes ago   
Status           available   
Zone             us-south-2   
VPC              helloworld-vpc(6dcd8188-f1e2-48ca-b872-d04cf54479c1)
```
{: screen}

## Etape 6 : créez une clé SSH dans IBM Public Cloud 
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

Vous allez utiliser la clé pour mettre à disposition une instance de serveur virtuel. Vous pouvez utiliser la même clé pour mettre à disposition plusieurs instances de serveur virtuel.

Les clés ne peuvent être ajoutées qu'initialement dans le cadre de la création d'une instance de serveur virtuel. Il n'existe aucun outil permettant d'ajouter des clés par la suite.
{:tip}

Pour consulter les clés disponibles dans votre compte IBM Cloud, exécutez la commande suivante :

```
ibmcloud is keys
```
{: pre}

Pour créer une clé, exécutez la commande ci-dessous. Indiquez le chemin d'accès à votre fichier `id_rsa.pub`.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

La sortie se présente comme suit : 

```
Creating key helloworld-key in resource group Default under account <Account Name> as user <User>...

ID               859b4e97-7540-4337-9c64-384792b85653   
Name             helloworld-key   
Type             rsa   
Length           2048   
FingerPrint      SHA256:hkcAOGB5z7QXqZLHd0kGqhj735qrfMjZLH9PxS/42vA   
Key              ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9i2joQ8eiFVdJ7pOlC6h5+IoBL6wFFygkk9Na3gV8Bi52dv44YOAjSJ2oduguHEtLp5r4eh4+5jiEBkFYkHNUhE0MxlcVZABTYWePXx4QnlmGr99xyOfi6DAHhSRQiSBhmhjcGjbADavDuIgoyKpVXbU9O1If3P0miNpaouaZmr+d68OHt4yPvNnztlluV3JBISJgqJ7pzg6wFF0SrjqtEYKBd8oxwogHu+rmRgT7IF09oSiKpKZRF0VfeLFz+REh9RuKa4Jh63aa2PRIgDKq6HO7MEdfOtGzCzoqqlFKgpl+VgGyT0b5BjQEnWv13cwx02bv5QCwma/GeAOpW0CD user@email.com   

Created          now   
```
{:screen}

Enregistrez l'ID dans une variable afin de pouvoir l'utiliser ultérieurement, par exemple :

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## Etape 7 : sélectionnez un profil et une image pour l'instance de serveur virtuel 
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

Pour répertorier toutes les instances de profil disponibles, exécutez la commande suivante :

```
ibmcloud is instance-profiles
```
{: pre}

Pour répertorier toutes les images disponibles, exécutez la commande suivante :

```
ibmcloud is images
```
{: pre}

Par exemple, sélectionnez le profil d'instance `bc1-2x8` et l'image `ubuntu-16.04-amd64`. Pour obtenir l'ID d'image de `ubuntu-16.04-amd64`, exécutez la commande suivante :

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Etape 8 : mettez à disposition une instance de serveur virtuel 
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Les clés ne peuvent être ajoutées qu'initialement dans le cadre de la création de l'instance de serveur virtuel. Il n'existe aucun outil permettant d'ajouter des clés par la suite.
{:tip}

La sortie se présente comme suit : 

```
Creating instance helloworld-vsi in resource group Default under account <Account Name> as user <User>...

ID                4562c5c0-9cf7-4406-bc90-ab4baea91057   
Name              helloworld-vsi   
Gen                  
Profile           bc1-2x8   
CPU Arch          amd64   
CPU Cores         2   
CPU Frequency     2000   
Memory            8   
Primary Intf      primary(2e850924-b5d7-4386-a778-03556d5850c1)   
Primary Address   10.240.64.4  
Image             ubuntu-16.04-amd64(7eb4e35b-4257-56f8-d7da-326d85452591)   
Status            pending   
Created           5 seconds ago   
VPC               helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)   
Zone              us-south-2   
```
{:screen}

Enregistrez l'ID de l'instance dans une variable afin de pouvoir l'utiliser ultérieurement :

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Enregistrez également l'ID de l'interface réseau principale `Primary Intf` dans une variable afin de pouvoir l'utiliser ultérieurement : 

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

Notez que le statut de l'instance est `pending` (en attente) lorsqu'elle est créée. Pour que vous puissiez continuer, l'instance doit passer au statut `running` (en fonctionnement), ce qui prend quelques minutes. Pour vérifier le statut de l'instance, exécutez la commande suivante :

```
ibmcloud is instance $vsi
```
{: pre}

## Etape 9 : créez une adresse IP flottante 
{: #step-9-create-a-floating-ip-address}

Vous avez besoin d'une adresse IP flottante pour pouvoir vous connecter à l'instance de serveur virtuel depuis Internet.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

La sortie se présente comme suit : 

```
Creating floating ip helloworld-fip in resource group Default under account <Account Name> as user <User>...

ID               b9d1cc1f-67db-40e3-81de-9228465170a5   
Address          169.61.181.53   
Name             helloworld-fip   
Target           primary(2e850924-.)   
Target Type      intf   
Target IP        10.0.1.5   
Created          now   
Status           available   
Zone             us-south-2   
```
{:screen}

Enregistrez l'ID de l'adresse IP flottante et l'adresse (`Address`) dans une variable afin de pouvoir les utiliser ultérieurement :

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Etape 10 : ajoutez une règle au groupe de sécurité par défaut pour SSH 
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Recherchez le groupe de sécurité pour le cloud privé virtuel :

```
ibmcloud is vpc-sg $vpc
```
{: pre}

La sortie se présente comme suit : 
```
Getting default security group of vpc ba9e785a-3e10-418a-811c-56cfe5669676 under account <Account Name> as user <User>...

ID               2d364f0a-a870-42c3-a554-000000981149
Name             errand-drastic-imperial-retail-unlocked-jester
Created          1 week ago
VPC              helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)
Resource Group   -
Tags             -

Rules
ID                                     Direction   IPv*   Protocol                  Remote
b597cff2-38e8-4e6e-999d-000002031345   inbound     ipv4   all                       errand-drastic-imperial-retail-unlocked-jester(2d364f0a-.)
b597cff2-38e8-4e6e-999d-000002031527   outbound    ipv4   all                       -

```
{:screen}

Enregistrez l'ID dans une variable afin de pouvoir l'utiliser ultérieurement :

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

A présent, créez la règle pour autoriser SSH :

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

La sortie se présente comme suit : 

```
Creating rule for security group 2d364f0a-a870-42c3-a554-000000981149 under account <Account Name> as user <User>...

ID          b597cff2-38e8-4e6e-999d-000001949921
Direction   inbound
IPv*        ipv4
Protocol    tcp
Min Port    22
Max Port    22
Remote      -
```
{:screen}

## Etape 11 : créez un volume de données de stockage par blocs 
{: #step-11-create-a-block-storage-data-volume}

Exécutez cette commande pour créer un volume de données de stockage par blocs. Spécifiez un nom pour votre volume, votre profil de volume et la zone dans laquelle vous créez le volume. Pour connecter un volume de données de stockage par blocs à une instance, l'instance et le volume doivent être créés dans la même zone.

Pour afficher une liste de profils de volume, exécutez :

```
ibmcloud is volume-profiles
```
{: pre}

Les profils peuvent être general-purpose (3 ES/s/Go), 5iops-tier, 10iops-tier et custom.
Voir [A propos de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
pour obtenir des informations sur la capacité du volume et les plages d'E-S/s en fonction du profil que vous sélectionnez.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

La sortie se présente comme suit :

```
Creating volume helloworld-vol in resource group Default under account <Account Name> as user <User>...

ID                                      933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    helloworld-vol
Capacity                                100
IOPS                                    1000
Auto delete                             false
Encryption                              provider managed
Profile                                 custom
Status                                  pending
Created                                 2019-02-15 10:09:28
Zone                                    us-south-2
Volume Attachment Instance Reference    none
```
{:screen}

Enregistrez l'ID du volume dans une variable afin de pouvoir l'utiliser ultérieurement :

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Notez que le statut du volume est `pending` lors de sa création. Pour que vous puissiez continuer, le volume doit passer au statut `available`, ce qui prend quelques minutes. 

Pour vérifier le statut du volume, exécutez la commande suivante :

```
ibmcloud is volume $vol
```
{: pre}

## Etape 12 : connectez un volume de données de stockage par blocs à une instance 
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Utilisez la commande suivante pour connecter le volume à l'instance de serveur virtuel, à l'aide des variables que nous avons créées :

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Etape 13 : connectez-vous à votre instance de serveur virtuel à l'aide de votre clé SSH privée 
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

Par exemple, vous pouvez utiliser une commande de la forme suivante :

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

Vous recevez une réponse similaire à l'exemple qui suit. Lorsque vous êtes invité à poursuivre la connexion, entrez `yes`.

```
The authenticity of host '169.61.181.53 (169.61.181.53)' can't be established.
ECDSA key fingerprint is SHA256:9MczXIwJq892DYwu0sZpQZOXORdvNXeP1aFioZpQDsM.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '169.61.181.53' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.4.0-133-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@helloworld-vsi:~#
```
{:screen}

## Etape 14 : Hello, World!
{: #step-14-hello-world}

Exécutez la commande suivante dans la fenêtre du terminal :

```
echo `hostname` says "Hello, World!"
```
{:pre}

La sortie se présente comme suit :

```
helloworld-vsi says Hello, World!
```
{:screen}

## Etape 15 (facultative) : commencez à utiliser votre volume de données de stockage par blocs
{: #step-15-optional-start-using-your-block-storage-data-volume}

Pour utiliser votre volume de stockage par blocs comme un système de fichiers, vous devez partitionner le volume, le formater et le monter sous forme de système de fichiers.

Sous Linux, exécutez la commande suivante pour répertorier tous les volumes de stockage par blocs à partir de votre instance :

```
lsblk
```
{:pre}

La sortie se présente comme suit : 

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

Le volume `xvdc` est votre nouveau volume de données de stockage par blocs.

Exécutez la commande suivante pour partitionner le volume. Pour commencer, utilisez la commande `n` pour la nouvelle partition.

```
fdisk /dev/xvdc
```
{:pre}

Formatez le volume.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Intitulez le volume "hellovol".

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Créez le répertoire et montez le volume en tant que système de fichiers.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

Pour voir votre nouveau système de fichiers, exécutez la commande suivante :

```
df -k
```
{:pre}

La sortie se présente comme suit : 

```
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4075344       0   4075344   0% /dev
tmpfs             816936    8844    808092   2% /run
/dev/xvda2     101330012 1261048 100052580   2% /
tmpfs            4084664       0   4084664   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4084664       0   4084664   0% /sys/fs/cgroup
/dev/xvda1        245679   64360    168212  28% /boot
tmpfs             817040       0    817040   0% /run/user/0
/dev/xvdc      103081248   61176  97777192   1% /hellovol
```
{:screen}

Pour accéder au répertoire dans votre nouveau système de fichiers et créer un fichier, procédez comme suit :

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Etape 15 (facultative) : supprimez les ressources
{: #step-15-delete-vpc-cli}

Supprimez éventuellement les ressources. Une ressource ne peut pas être supprimée si elle contient d'autres ressources. Par exemple, un cloud privé virtuel ne peut pas être supprimé s'il contient des sous-réseaux, et un sous-réseau ne peut pas être supprimé s'il contient des instances de serveur virtuel. Lors d'une opération de suppression, l'API peut indiquer que la commande a été exécutée, alors que la suppression des ressources peut encore être en cours. Après avoir émis la demande de suppression, vérifiez que la ressource a bien été supprimée avant d'essayer de supprimer la ressource parent. Voir [Suppression d'un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) pour plus de détails.

### Libérez l'adresse IP flottante

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Supprimez l'instance de serveur virtuel

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Supprimez la clé

```
ibmcloud is key-delete $key
```
{: pre}

### Supprimez le sous-réseau

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Supprimez la passerelle publique

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### Supprimez le VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Supprimez le volume (s'il n'est pas défini avec la fonction de suppression automatique)

```
ibmcloud is volume-delete $vol
```
{: pre}

## Félicitations !
{: #cli-congratulations}

Vous avez mis à disposition et connecté votre instance de cloud privé virtuel via l'interface de ligne de commande d'IBM Cloud. Pour essayer d'autres commandes d'interface de ligne de commande, consultez la référence complète :

* [Référence d'interface de ligne de commande pour VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
