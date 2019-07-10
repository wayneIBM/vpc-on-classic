---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: create, configure, permissions, ACL, virtual, server, instance, subnet, block, storage, volume, security, group, images, Windows, Linux, example, monitoring, VPN, load balancer, IKE, IPsec

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Création d'un cloud privé virtuel à l'aide de la console {{site.data.keyword.cloud_notm}}
{: #creating-a-vpc-using-the-ibm-cloud-console}
[comment]: # (rubrique d'aide liée)

Ce document vous montre comment créer et configurer un cloud privé virtuel {{site.data.keyword.cloud}} à l'aide de la console {{site.data.keyword.cloud_notm}}.

Pour créer et configurer votre cloud privé virtuel (VPC) et d'autres ressources associées, effectuez les étapes présentées dans les sections suivantes, dans l'ordre indiqué :

1. Créez un cloud privé virtuel et un sous-réseau afin de définir le réseau. Lorsque vous créez votre sous-réseau, connectez une passerelle publique pour autoriser toutes les ressources du sous-réseau à communiquer avec l'Internet public.
1. Configurez une liste de contrôle d'accès pour limiter le trafic entrant et sortant du sous-réseau. Par défaut, l'intégralité du trafic est autorisée.
1. Créez une instance de serveur virtuel.
1. Créez un volume de stockage par blocs et connectez-le à une instance.
1. Configurez un groupe de sécurité pour définir le trafic entrant et sortant qui est autorisé pour l'instance.
1. Réservez et associez une adresse IP flottante pour que votre instance soit accessible sur Internet.
1. Créez un équilibreur de charge pour répartir les demandes dans plusieurs instances.
1. Créez un réseau privé virtuel (VPN) pour que votre cloud privé virtuel puisse se connecter en toute sécurité à un autre réseau privé, comme votre réseau sur site ou un autre cloud privé virtuel.

## Avant de commencer
{: #before}
Assurez-vous de disposer de droits suffisants pour créer et gérer les ressources dans votre cloud privé virtuel. Pour plus d'informations, voir [Gestion des droits utilisateur pour les ressources de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Générez une clé SSH qui sera utilisée pour la connexion à l'instance de serveur virtuel. Par exemple, générez une clé SSH sur votre serveur Linux en exécutant la commande `ssh-keygen -t rsa -C "user_ID"`. Cette commande génère deux fichiers. La clé publique générée se trouve dans le fichier `<your key>.pub`.

Si vous prévoyez de créer un équilibreur de charge et d'utiliser HTTPs pour l'écouteur, un certificat SSL est requis. Vous pouvez gérer les certificats avec [IBM Certificate Manager ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/catalog/services/certificate-manager){: new_window}. Vous devez également créer une autorisation pour permettre à votre instance d'équilibreur de charge d'accéder à l'instance de Certificate Manager contenant le certificat SSL. Vous pouvez créer une autorisation via [Identity and Access Authorizations ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam/#/authorizations){: new_window}. Pour la source, sélectionnez **VPC Infrastructure** comme service source, **Load Balancer for VPC** comme type de ressource et **Toutes les instances de ressource** pour l'instance de ressource source. Sélectionnez **Certificate Manager** comme service cible et affectez **Auteur** comme rôle d'accès au service. Pour l'instance de service cible, définissez **Toutes les instances** ou votre instance Certificate Manager spécifique. Pour plus d'informations, voir [Utilisation des équilibreurs de charge dans IBM Cloud VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).

## Création d'un cloud privé virtuel et d'un sous-réseau

Pour créer un cloud privé virtuel et un sous-réseau :

1. Ouvrez la console [{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}){: new_window}
1. Cliquez sur l'**icône de menu ![Icône de menu](../../icons/icon_hamburger.svg) > Infrastructure VPC > Réseau > VPC** et cliquez sur **Nouveau cloud privé virtuel**.
1. Entrez un nom pour le cloud privé virtuel, par exemple `my-vpc`.
1. Sélectionnez un groupe de ressources pour le cloud privé virtuel et toutes ses ressources associées. Les groupes de ressources vous permettent d'organiser vos ressources de compte pour le contrôle d'accès et la facturation. Pour plus d'informations, voir [Meilleures pratiques pour l'organisation des ressources dans un groupe de ressources](/docs/resources?topic=resources-bp_resourcegroups).
1. _Facultatif :_ entrez des étiquettes pour faciliter l'organisation et la recherche de vos ressources. Vous pourrez ajouter d'autres étiquettes ultérieurement. Pour plus d'informations, voir [Utilisation d'étiquettes](/docs/resources?topic=resources-tag).
1. Sélectionnez ou créez la liste de contrôle d'accès par défaut pour les nouveaux sous-réseaux dans ce cloud privé virtuel. Dans ce tutoriel, nous allons créer une nouvelle liste de contrôle d'accès par défaut. Nous configurerons les règles pour la liste de contrôle d'accès ultérieurement.
1. Indiquez si le groupe de sécurité par défaut autorise le trafic SSH et ping entrant vers les instances de serveur virtuel dans ce cloud privé virtuel. Nous configurerons d'autres règles pour le groupe de sécurité par défaut ultérieurement.
1. Entrez un nom pour le nouveau sous-réseau dans votre cloud privé virtuel, par exemple `my-subnet`.
1. Sélectionnez un emplacement pour le sous-réseau, c'est-à-dire une région et une zone.

    La région que vous sélectionnez est utilisée comme région du cloud privé virtuel. Toutes les ressources supplémentaires que vous créez dans ce cloud privé virtuel seront créées dans la région sélectionnée.
    {: tip}

1. Entrez une plage d'adresses IP pour le sous-réseau dans la notation CIDR, par exemple : `10.240.0.0/24`. Dans la plupart des cas, vous pouvez utiliser la plage d'adresses IP par défaut. Si vous voulez spécifier une plage d'adresses IP personnalisée, vous pouvez utiliser la calculatrice de plage d'adresses IP afin de sélectionner un préfixe d'adresse différent ou de changer le nombre d'adresses.
1. Sélectionnez une liste de contrôle d'accès pour le sous-réseau. Sélectionnez **Utiliser la valeur par défaut de VPC** afin d'utiliser la liste de contrôle d'accès par défaut qui a été créée pour ce cloud privé virtuel.
1. Associez une passerelle publique au sous-réseau pour permettre à toutes les ressources associées de communiquer avec l'Internet public.  

    Vous pouvez également associer la passerelle publique après avoir créé le sous-réseau.
    {: tip}

1. Cliquez sur **Nouveau cloud privé virtuel**.
1. Pour créer un autre sous-réseau dans ce cloud privé virtuel, cliquez sur l'onglet **Sous-réseaux**, puis cliquez sur **Nouveau sous-réseau**. Lorsque vous définissez le sous-réseau, assurez-vous de sélectionner `my_vpc` dans la zone **Cloud privé virtuel**.

## Configuration de la liste de contrôle d'accès

Vous pouvez configurer la liste de contrôle d'accès afin de limiter le trafic entrant et sortant du sous-réseau. Par défaut, l'intégralité du trafic est autorisée.

Chaque sous-réseau peut être associé à une liste de contrôle d'accès et une seule. Toutefois, chaque liste de contrôle d'accès peut être associée à plusieurs sous-réseaux.

Pour configurer la liste de contrôle d'accès :

1. Dans le panneau de navigation, cliquez sur **Réseau > Sous-réseaux**.
1. Cliquez sur le sous-réseau que vous avez créé.
1. Dans la zone **Détails du sous-réseau**, cliquez sur le nom de la liste de contrôle d'accès.
1. Cliquez sur **Ajouter une règle** pour configurer les règles de trafic entrant et sortant qui définissent le trafic autorisé vers ou depuis le sous-réseau. Pour chaque règle, spécifiez les informations suivantes :  
   * Spécifiez la priorité de la règle. Les règles sont évaluées dans l'ordre croissant des valeurs de priorité et une valeur faible prend le pas sur une valeur élevée. Par exemple, si une règle de priorité 2 autorise le trafic HTTP et qu'une autre de priorité 5 interdit tout le trafic, le trafic HTTP reste autorisé.  
   * Indiquez si le trafic spécifié doit être autorisé ou interdit.  
   * Spécifiez un bloc CIDR pour indiquer la plage d'adresses IP à laquelle la règle s'applique.
   * Sélectionnez les protocoles et les ports auxquels la règle s'applique.
1. Une fois que vous avez terminé de créer les règles, cliquez sur l'élément de navigation **Toutes les listes de contrôle d'accès** en haut de la page.

### Exemple de liste de contrôle d'accès

Par exemple, vous pouvez configurer des règles de trafic entrant qui :

 1. Autorisent le trafic HTTP depuis Internet
 1. Autorisent l'intégralité du trafic entrant depuis le sous-réseau 10.10.20.0/24
 1. Interdisent tout autre trafic entrant  

Ensuite, configurez des règles de trafic sortant qui :

1. Autorisent le trafic HTTP vers Internet
1. Autorisent l'intégralité du trafic sortant vers le sous-réseau 10.10.20.0/24
1. Interdisent tout autre trafic sortant  

![Affiche les exemples de règles de trafic entrant et sortant](images/acl-rules.png)

## Création d'une instance de serveur virtuel

Pour créer une instance de serveur virtuel sur le sous-réseau que vous venez de créer :

1. Cliquez sur **Calcul > Instance de serveur virtuel** dans le panneau de navigation, puis cliquez sur **Nouvelle instance**.
1. Entrez un nom pour l'instance, par exemple `my-instance`.
1. Sélectionnez le cloud privé virtuel que vous avez créé.
1. Dans la zone **Emplacement**, sélectionnez la zone dans laquelle créer l'instance.
1. Sélectionnez une image (c'est-à-dire un système d'exploitation et une version), par exemple Ubuntu Linux 16.04.
1. Pour définir la taille de l'instance, sélectionnez l'un des profils courants ou cliquez sur **Tous les profils** pour choisir une autre combinaison de coeurs et de mémoire RAM plus adaptée à votre charge de travail.
1. Sélectionnez une clé SSH existante ou ajoutez une nouvelle clé SSH qui sera utilisée pour accéder à l'instance de serveur virtuel. Pour ajouter une clé SSH, cliquez sur **Nouvelle clé** et attribuez un nom à la clé. Une fois que vous avez entré une valeur pour la clé publique générée précédemment, cliquez sur **Ajouter une clé SSH**.

Les clés ne peuvent être ajoutées qu'initialement dans le cadre de la création de l'instance de serveur virtuel. Il n'existe aucun outil permettant d'ajouter des clés par la suite.
{:tip}

1. _Facultatif :_ entrez des données utilisateur afin d'effectuer des tâches de configuration courantes au démarrage de votre instance. Par exemple, vous pouvez spécifier des directives cloud-init ou des scripts shell pour les images Linux. Pour plus d'informations, voir [Données utilisateur](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-user-data).
1. Prenez note du volume d'amorçage. Dans l'édition en cours, 100 Go lui sont alloués. La fonction *Suppression automatique* est activée pour le volume ; celui-ci est supprimé automatiquement si l'instance est supprimée.
1. Dans la zone **Volume de stockage par blocs connecté**, vous pouvez cliquer sur **Nouveau volume de stockage par blocs** pour connecter un volume de stockage par blocs à votre instance. Dans ce tutoriel, nous allons créer un volume de stockage par blocs et le connecter à l'instance par la suite.
1. Dans la zone **Interfaces réseau**, vous pouvez éditer l'interface réseau et modifier son nom. S'il existe plusieurs sous-réseaux dans la zone et le cloud virtuel privé sélectionnés, vous pouvez associer un sous-réseau différent à l'interface. Si vous voulez que l'instance se trouve sur plusieurs sous-réseaux, vous pouvez créer davantage d'interfaces.

   Vous pouvez également sélectionner les groupes de sécurité à associer à chaque interface. Par défaut, le groupe de sécurité par défaut du cloud privé virtuel est associé. Il autorise le trafic SSH et ping entrant, l'intégralité du trafic sortant, et tout le trafic entre les instances dans le groupe. Le reste du trafic est bloqué ; vous pouvez configurer des règles pour autoriser d'autres types de trafic. Si par la suite, vous éditez les règles du groupe de sécurité par défaut, ces règles mises à jour sont appliquées à toutes les instances en cours et ultérieures dans le groupe.

1. Cliquez sur **Créer une instance de serveur virtuel**. Au départ, le statut de l'instance est *En attente* ; il devient *Arrêté*, puis *Démarré*. Il peut être nécessaire d'actualiser la page pour constater le changement de statut.

## Création et connexion d'un volume de stockage par blocs

Vous pouvez créer un volume de stockage par blocs et le connecter à votre instance de serveur virtuel.

Pour créer et connecter un volume de stockage par blocs :

1. Dans le panneau de navigation, cliquez sur **Stockage > Volumes de stockage par blocs**.
1. Sur la page des volumes de stockage par blocs pour VPC, cliquez sur **Nouveau volume** et spécifiez les informations suivantes.
  * **Nom** : entrez un nom pour le volume de stockage par blocs, tel que `data-volume-1`.  
  * **Groupe de ressources** : sélectionnez un groupe de ressources pour le volume de stockage par blocs. Les groupes de ressources vous permettent d'organiser vos ressources de compte pour le contrôle d'accès et la facturation. Pour plus d'informations, voir [Meilleures pratiques pour l'organisation des ressources dans un groupe de ressources](/docs/resources?topic=resources-bp_resourcegroups).
  * **Etiquettes** : (_facultatif_) entrez des étiquettes pour vous aider à organiser et trouver vos ressources. Vous pourrez ajouter d'autres étiquettes ultérieurement. Pour plus d'informations, voir [Utilisation d'étiquettes](/docs/resources?topic=resources-tag).
  * **Emplacement** : sélectionnez un emplacement pour le volume de stockage par blocs. L'emplacement est constitué d'une région et d'une zone, par exemple US South 1.
  * **Taille** : indiquez la taille du volume entre 10 Go et 2000 Go.
  * **E-S/s** : sélectionnez l'un des niveaux d'E-S/s ou cliquez sur Personnalisé pour entrer une valeur en fonction de la taille du volume.
  * **Chiffrement** : acceptez le chiffrement géré par le fournisseur par défaut ou sélectionnez Géré par le client et utilisez votre propre clé de chiffrement. Cette étape nécessite la mise à disposition d'une instance de service Key Protect et la création ou l'importation d'une clé racine. Pour plus d'informations, voir [Création de volumes de stockage par blocs avec chiffrement géré par le client](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption#data-vol-encryption-ui).
1. Cliquez sur **Créer un volume**.
1. Dans la liste des volumes de stockage par blocs, recherchez le volume que vous avez créé. Lorsque le statut est Disponible, cliquez sur "..." et sélectionnez **Connecter à l'instance**.
1. Sélectionnez l'instance à laquelle vous souhaitez connecter le volume et cliquez sur **Connecter**.

## Configuration du groupe de sécurité pour l'instance

Vous pouvez configurer le groupe de sécurité afin de définir le trafic entrant et sortant qui est autorisé pour l'instance. Par exemple, après avoir configuré des règles de liste de contrôle d'accès pour le sous-réseau en fonction des politiques de sécurité de votre société, vous pouvez restreindre plus précisément le trafic pour des instances spécifiques en fonction de leurs charges de travail.

Pour configurer le groupe de sécurité :

1. Dans la page Instances de serveur virtuel, cliquez sur votre instance pour afficher ses détails.
1. Dans la section **Interfaces réseau**, cliquez sur le groupe de sécurité.
1. Cliquez sur **Ajouter une règle** pour configurer les règles de trafic entrant et sortant qui définissent le trafic autorisé vers et depuis l'instance. Pour chaque règle, spécifiez les informations suivantes :  
   * Spécifiez un bloc CIDR ou une adresse IP pour le trafic autorisé. Vous pouvez aussi spécifier un groupe de sécurité dans le même cloud privé virtuel pour autoriser le trafic vers ou depuis toutes les instances associées au groupe de sécurité sélectionné.   
   * Sélectionnez les protocoles et les ports auxquels la règle s'applique.    

   **Astuces :**  
  * Toutes les règles sont évaluées, quel que soit l'ordre dans lequel elles ont été ajoutées.
  * Les règles possèdent un état, ce qui signifie que le trafic en retour en réponse au trafic autorisé est admis automatiquement. Par exemple, une règle qui autorise le trafic TCP entrant sur le port 80 autorise également le trafic TCP sortant en réponse sur le port 80 vers l'hôte d'origine, sans qu'une règle supplémentaire ne soit nécessaire.
1. <dMD:em>Facultatif :_ si vous voulez associer ce groupe de sécurité à d'autres instances, cliquez sur **Interfaces connectées** dans le panneau de navigation et sélectionnez des interfaces supplémentaires.
1. Une fois que vous avez terminé de créer les règles, cliquez sur l'élément de navigation **Tous les groupes de sécurité** en haut de la page.

### Exemple de groupe de sécurité  

Par exemple, vous pouvez configurer des règles de trafic entrant qui :

 * Autorisent tout le trafic SSH (port TCP 22)
 * Autorisent tout le trafic ping (type ICMP 8)

Ensuite, configurez des règles de trafic sortant qui autorisent tout le trafic TCP.

![Affiche les exemples de règles de trafic entrant et sortant](images/sg-example-ui.png)

## Réservation d'une adresse IP flottante

Réservez et associez une adresse IP flottante pour que votre instance soit accessible sur Internet.  

Votre instance doit être démarrée pour que vous puissiez associer une adresse IP flottante. L'instance peut nécessiter quelques minutes pour être opérationnelle.
{: tip}

Pour réserver et associer une adresse IP flottante :

1. Dans la page Instances de serveur virtuel, cliquez sur votre instance pour afficher ses détails.
1. Dans la section **Interfaces réseau**, cliquez sur **Réserver** pour l'interface à associer à une adresse IP flottante.

Si par la suite, vous voulez réaffecter cette adresse IP flottante à une autre instance dans la même zone, recherchez l'adresse IP flottante dans la page **Réseau > IP flottantes**, cliquez sur son menu déroulant dynamique (**...**), puis cliquez sur **Désassocier**. Ensuite, cliquez sur **Associer** pour sélectionner l'instance et l'interface réseau à associer à l'adresse IP flottante.
{: tip}

## Connexion à votre instance
Avec l'adresse IP flottante que vous avez créée, envoyez une commande PING à votre instance pour vous assurer qu'elle est opérationnelle :

```
ping <public-ip-address>
```
{:pre}


### Connexion aux images Linux

Etant donné que vous avez créé votre instance avec une clé SSH publique, vous pouvez maintenant vous connecter à votre instance directement en utilisant votre clé privée :


```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

Voir [Connecting to your instance using Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) pour plus d'informations sur la connexion à votre instance.

### Connexion aux images Windows
Pour vous connecter à une image Windows, connectez-vous à l'aide de son mot de passe déchiffré. Pour obtenir le mot de passe déchiffré, copiez le mot de passe chiffré figurant dans la page de détails de l'instance et exécutez la commande suivante :

```
# Decode the encrypted password
cat ~/examplepwd | base64 --decode > ~/examplepwd64
# Decrypt the decoded password using the RSA private key
openssl pkeyutl -in ~/examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
-pkeyopt rsa_mgf1_md:sha256
```
{:codeblock}


Voir [Connecting to your Windows instance](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-windows-instance) pour plus d'informations sur la connexion à votre instance.



## Surveillance de votre instance

Pour afficher un journal d'activité qui indique quand l'instance a été démarrée, arrêtée ou redémarrée, cliquez sur **Activité** dans le panneau de navigation.

## Création d'un équilibreur de charge
Vous pouvez créer un équilibreur de charge pour répartir le trafic entrant dans plusieurs instances.

Pour créer un équilibreur de charge :
1. Dans le panneau de navigation, cliquez sur **Réseau > Equilibreurs de charge**.
1. Dans la page Equilibreurs de charge, cliquez sur **Nouvel équilibreur de charge** et spécifiez les informations suivantes :
    * **Nom** : entrez un nom pour l'équilibreur de charge, par exemple `my-load-balancer`.
    * **Cloud privé virtuel** : sélectionnez votre cloud privé virtuel.
    * **Groupe de ressources** : sélectionnez un groupe de ressources pour l'équilibreur de charge.
    * **Type** : sélectionnez le type d'équilibreur de charge, Public ou Privé.
    * **Région** : indique la région dans laquelle l'équilibreur de charge sera créé, c'est-à-dire la région sélectionnée pour votre cloud privé virtuel.
    * **Sous-réseaux** : sélectionnez les sous-réseaux sur lesquels créer votre équilibreur de charge. Pour optimiser la disponibilité de votre application, sélectionnez des sous-réseaux dans des zones différentes.
1. Cliquez sur **Nouveau pool** et spécifiez les informations ci-dessous pour créer un pool de back-end. Vous pouvez créer un ou plusieurs pools.
    * **Nom** : entrez un nom pour le pool, par exemple `my-pool`.
    * **Protocole** : sélectionnez le protocole pour vos instances de back-end qui se trouvent derrière l'équilibreur de charge. Le protocole du pool doit correspondre au protocole de son écouteur associé. Par exemple, si un protocole HTTPS ou HTTP est sélectionné pour l'écouteur, le protocole du pool doit être HTTP. De même, si le protocole d'écouteur est TCP, le protocole du pool de back-end doit être TCP.
    * **Méthode** : sélectionnez la façon dont l'équilibreur de charge doit répartir le trafic dans les instances de back-end :
      * **Permutation circulaire** : réacheminez les demandes vers chaque instance, à tour de rôle. Toutes les instances reçoivent approximativement un nombre égal de connexions client.
      * **Permutation circulaire pondérée** : réacheminez les demandes vers chaque instance proportionnellement au poids qui lui est affecté. Par exemple, si vous disposez des instances A, B et C et que leurs poids respectifs sont 60, 60 et 30, les instances A et B reçoivent un nombre égal de connexions client et l'instance C reçoit la moitié de ce nombre.
      * **Nb min. de connexions** : réacheminez les demandes vers l'instance dont le nombre de connexions est le plus faible.  
    * **Conservation de session** : indiquez si toutes les demandes émises au cours d'une session utilisateur sont envoyées à la même instance.  
    * **Contrôles de santé de serveur** : configurez la façon dont l'équilibreur de charge contrôle la santé des instances.
      * **Protocole de santé** : protocole utilisé par l'équilibreur de charge pour envoyer des messages de diagnostic d'intégrité aux instances de back-end.
      * **Chemin de santé** : le chemin de santé est applicable uniquement si HTTP est sélectionné comme protocole de santé. Le chemin de santé spécifie l'URL utilisée par l'équilibreur de charge pour envoyer les demandes de contrôle de santé aux instances de back-end. Par défaut, les contrôles de santé sont envoyés à la racine (`/`).
      * **Intervalle** : intervalle en secondes entre deux tentatives de contrôle de santé consécutives. Par défaut, les contrôles de santé sont envoyés toutes les cinq secondes.
      * **Délai d'attente** : durée maximale pendant laquelle le système attend une réponse d'une demande de contrôle de santé. Par défaut, l'équilibreur de charge attend une réponse pendant deux secondes.
      * **Nb max. de nouvelles tentatives** : nombre maximal de tentatives de contrôle de santé supplémentaires effectuées par l'équilibreur de charge avant qu'il ne déclare qu'une instance est en mauvaise santé. Par défaut, une instance n'est plus considérée comme étant en bonne santé après deux échecs de contrôle de santé.

        Bien que l'équilibreur de charge arrête d'envoyer des connexions aux instances en mauvaise santé, il continue de surveiller la santé de ces instances et les réutilise dès qu'elles sont à nouveau considérées comme étant en bonne santé, après deux tentatives de contrôle de santé consécutifs satisfaisants.

      Si des instances de back-end sont en mauvaise santé et que vous pensez que votre application s'exécute correctement, vérifiez les valeurs de protocole de santé et de chemin de santé. Vérifiez également tout groupe de sécurité associé aux instances pour vous assurer que les règles autorisent le trafic entre l'équilibreur de charge et les instances.
      {: tip}

1. Cliquez sur **Créer**.
1. A côté de l'entrée du nouveau pool, cliquez sur **Connecter** dans la colonne **Instances** pour ajouter une instance au pool. Cliquez sur **Ajouter** pour ajouter d'autres instances au pool. Spécifiez les informations suivantes pour chaque instance :
   * Sélectionnez un ou plusieurs sous-réseaux depuis lesquels sélectionner une instance.
   * Sélectionnez une instance. Si une instance possède plusieurs interfaces, assurez-vous de sélectionner l'adresse IP appropriée.
   * Spécifiez le port sur lequel le trafic est envoyé à l'instance.
   * Si votre pool utilise la méthode **Permutation circulaire pondérée**, affectez un poids à chaque instance.  

      L'affectation du poids '0' à une instance signifie qu'aucune nouvelle connexion ne sera réacheminée vers cette instance ; toutefois, le trafic existant continue de circuler tant que la connexion en cours reste active. L'utilisation du poids '0' peut permettre d'arrêter une instance correctement et de la retirer de la rotation des services.
      {: tip}

1. Cliquez sur **Nouvel écouteur** et spécifiez les informations ci-dessous pour créer un écouteur. Vous pouvez créer un ou plusieurs écouteurs.
   * **Protocole** : protocole à utiliser pour la réception des demandes entrantes.
   * **Port** : port d'écoute sur lequel les demandes sont reçues. La plage de ports de 56500 à 56520 est réservée pour la gestion et ne peut pas être utilisée.
   * **Pool de back-end** : pool de back-end vers lequel cet écouteur réachemine le trafic.
   * **Nb max. de connexions** (facultatif) : nombre maximal de connexions simultanées autorisées par l'écouteur.
   * **Certificat SSL** : si HTTPS est le protocole sélectionné pour cet écouteur, vous devez sélectionner un certificat SSL. Assurez-vous que l'équilibreur de charge est autorisé à accéder au certificat SSL. Pour des instructions, voir [Avant de commencer](#before).
1. Cliquez sur **Créer**.
1. Une fois que vous avez terminé de créer des pools et des écouteurs, cliquez sur **Créer un équilibreur de charge**.
1. Pour afficher les détails d'un équilibreur de charge existant, cliquez sur le nom de votre équilibreur de charge dans la page **Equilibreurs de charge**.

## Création d'un réseau privé virtuel (VPN)
Vous pouvez créer un réseau privé virtuel (VPN) pour que votre cloud privé virtuel puisse se connecter en toute sécurité à un autre réseau privé, comme un réseau sur site ou un autre cloud privé virtuel.

Pour un exemple de code, voir [Utilisation d'un VPN avec votre VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
{: tip}

Pour créer un réseau privé virtuel :
1. Dans le panneau de navigation, cliquez sur **Réseau > Réseaux VPN**.
1. Dans la page Réseau privé virtuel, cliquez sur **Nouvelle passerelle VPN** et spécifiez les informations suivantes :
    * **Nom** : entrez un nom pour la passerelle VPN dans votre cloud privé virtuel, par exemple `my-vpn-gateway`.
    * **Cloud privé virtuel** : sélectionnez votre cloud privé virtuel.
    * **Groupe de ressources** : sélectionnez un groupe de ressources pour le réseau privé virtuel.
    * **Sous-réseau** : sélectionnez le sous-réseau sur lequel créer la passerelle VPN.
1. Dans la section **Nouvelle connexion VPN**, définissez une connexion entre cette passerelle et un réseau hors de votre cloud privé virtuel en spécifiant les informations suivantes :
    * **Nom de la connexion** : entrez un nom pour la connexion, par exemple `my-connection`.
    * **Adresse de passerelle homologue** : spécifiez l'adresse IP de la passerelle VPN pour le réseau qui se trouve hors de votre cloud privé virtuel.
    * **Clé pré-partagée** : spécifiez la clé d'authentification de la passerelle VPN pour le réseau qui se trouve hors de votre cloud privé virtuel.
    * **Sous-réseaux locaux** : spécifiez un ou plusieurs sous-réseaux dans le cloud privé virtuel à connecter via le tunnel VPN.
    * **Sous-réseaux homologues** : spécifiez un ou plusieurs sous-réseaux sur l'autre réseau à connecter via le tunnel VPN.
1. Pour configurer la façon dont la passerelle cloud envoie des messages afin de vérifier que la passerelle homologue est active, spécifiez les informations suivantes dans la section **Détection d'homologue mort (DPD)** :
    * **Action DPD** : action à effectuer si une passerelle homologue arrête de répondre. Par exemple, sélectionnez **Redémarrer** si vous voulez que la passerelle renégocie immédiatement la connexion.
    * **Intervalle** : fréquence à laquelle le système vérifie si la passerelle homologue est active. Par défaut, des messages sont envoyés toutes les 30 secondes.
    * **Délai d'attente** : durée d'attente d'une réponse de la passerelle homologue. Par défaut, une passerelle homologue n'est plus considérée comme active si aucune réponse n'est reçue dans les 150 secondes.
1. Spécifiez les paramètres de sécurité IKE et IPsec pour la connexion.
    * Sélectionnez **Auto** si vous voulez que la passerelle cloud essaye automatiquement d'établir la connexion.
    * Sélectionnez ou créez des stratégies personnalisées si vous devez appliquer des exigences de sécurité particulières ou si la passerelle VPN pour l'autre réseau ne prend pas en charge les propositions de sécurité tentées par la négociation automatique.

  **Important** : les paramètres de sécurité IKE et IPsec que vous spécifiez pour la connexion doivent être identiques à ceux qui sont définis sur la passerelle pour le réseau qui se trouve hors de votre cloud privé virtuel.

## Félicitations !

Vous venez de créer et de configurer un cloud privé virtuel (VPC) et un sous-réseau, une liste de contrôle d'accès (ACL), une instance de serveur virtuel, un volume de stockage par blocs, un groupe de sécurité, une adresse IP flottante, un équilibreur de charge et un réseau privé virtuel (VPN). Vous pouvez continuer à développer votre cloud privé virtuel en ajoutant des instances et des sous-réseaux supplémentaires, ainsi que d'autres ressources.
