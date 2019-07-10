---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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

# Tutoriel d'initiation
{: #getting-started}
[comment]: # (rubrique d'aide liée)


Pour vous initier à {{site.data.keyword.cloud}} Virtual Private Cloud :

1. Créez un cloud privé virtuel.
2. Créez un ou plusieurs sous-réseaux dans le cloud privé virtuel dans une ou plusieurs zones.
3. Créez une passerelle publique sur un sous-réseau si vous souhaitez que les ressources de votre sous-réseau aient accès à Internet ou inversement.
4. Sélectionnez les profils des instances de serveur virtuel à exécuter, et instanciez-les.
5. Réservez une adresse IP flottante et associez-la à une instance de serveur virtuel si vous voulez y accéder depuis Internet.
5. Déployez votre service ou vos applications dans les instances de serveur virtuel.

## Prérequis

 * **Droits utilisateur** : assurez-vous que votre utilisateur dispose des autorisations suffisantes pour créer et gérer des ressources dans votre cloud privé virtuel. Pour obtenir la liste des droits requis, voir [Gestion des droits utilisateur pour les ressources de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

 * **Préparez votre clé SSH** : vous utilisez une clé SSH pour vous connecter à votre ou vos instances de serveur virtuel.

   * Recherchez un fichier appelé `id_rsa.pub` sous un répertoire `.ssh` de votre répertoire principal, par exemple, `/Users/<USERNAME>/.ssh/id_rsa.pub`. Le fichier commence par `ssh-rsa` et se termine par votre adresse électronique.

   * Ou bien, générez une clé SSH : si vous ne possédez pas de clé SSH publique ou si vous avez oublié le mot de passe d'une clé existante, générez-en une nouvelle en exécutant la commande `ssh-keygen` et en suivant les invites. Par exemple, vous pouvez générer une clé SSH sur votre serveur Linux en exécutant la commande `ssh-keygen -t rsa -C "user_ID"`. Cette commande génère deux fichiers. La clé publique générée se trouve dans le fichier `<your key>.pub`.

## Utilisation de l'interface utilisateur, l'interface de ligne de commande ou l'API REST

Vous pouvez mettre à disposition et gérer toutes vos ressources de cloud privé virtuel via l'interface utilisateur, l'interface de ligne de commande ou l'API REST.

Si vous débutez avec IBM Cloud Virtual Private Cloud, suivez l'un des liens ci-dessous, qui vous guidera tout au long du processus de création de votre cloud privé virtuel IBM Cloud et de ses ressources. Vous pouvez choisir de démarrer à partir de l'interface utilisateur de la console, de l'interface de ligne de commande ou des API REST. 

* Pour un accès via l'interface utilisateur, connectez-vous à la [console IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")]( https://{DomainName}/vpc){: new_window}. Pour plus d'informations, voir [Création d'un cloud privé virtuel à l'aide de la console IBM Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console).
* Pour utiliser l'interface de ligne de commande, suivez l'exemple [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli).
* Si vous êtes un utilisateur expérimenté, vous pouvez appeler les [API REST](https://{DomainName}/apidocs/vpc-on-classic) directement. Suivez le tutoriel d'[exemple de code](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) pour vous initier aux API REST.

## Etapes suivantes
Si vous êtes prêt à aller plus loin, accédez directement à la documentation détaillée sur la **mise en réseau**, les **instances de serveur virtuel** et le **stockage par blocs de cloud privé virtuel** :

* [**Networking for Virtual Private Cloud**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Virtual Servers for Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage for Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
