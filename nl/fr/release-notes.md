---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# Notes sur l'édition
{: #release-notes}

Utilisez ces notes sur l'édition pour en savoir plus sur les derniers changements apportés à {{site.data.keyword.cloud}} Virtual Private Cloud.
{:shortdesc}

## 14 juin 2019
{: #june-14-2019}

**Mises à jour de l'interface utilisateur d'IBM Cloud VPC**

- **Clés SSH et groupes de ressources.**
    * Les groupes de ressources sont présentés dans la listes de clés SSH.
    * Les groupes de ressources peuvent être sélectionnés dans la fenêtre modale de mise à disposition des clés SSH.
- **Profils et bande passante.** Le plafond de bande passante affecté à chaque profil est présenté sur les pages d'affichage **Profils courants** et **Tous les profils**.
- **Groupes de ressources.** La page de mise à disposition des instances de serveur virtuel présente la liste déroulante des groupes de ressources.

**Mises à niveau dans Block Storage for VPC**
- La **longueur du nom de volume** a été augmentée pour passer à 63 caractères alphanumériques.
- Les **codes d'erreur de volume** suivants ont été renommés et les messages mis à jour ou supprimés :
    * `volume_encryption_key_account_id_mismatch` est renommé `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` est renommé `volume_crn_cname_mismatch`
    * `volume_encryption_key_region_not_found` a été supprimé

**Mises à jour du logiciel SDK**

- **Terraform provider v0.17.1** a été publié. Téléchargez le [fichier binaire le plus récent](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1).
- L'**image Docker** a également été mise à jour avec la dernière version de Terraform provider. Vous pouvez extraire l'image Docker la plus récente à l'aide de cette commande :  `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- N'oubliez pas de définir `generation` en tant qu'argument de provider ou d'exporter `IC_GENERATION= 1` pour que votre code fonctionne avec la version actuelle publiée de l'infrastructure VPC on Classic. Par défaut la valeur est définie à 2 (VPC, à paraître, mais en version bêta pour le moment).
- Les arguments de 'provider' (`bluemix_api_key` et `bluemix_timeout`) ont été dépréciés, et vous pouvez recevoir un avertissement lorsque vous exécutez ou appliquez le plan Terraform.

## 7 juin 2019
{: #june-07-2019}

- **IBM Cloud VPC est à présent en version GA (Generally Available).** Tous les [comptes Paiement à la carte](/docs/account?topic=account-accounts) peuvent mettre à disposition des ressources VPC.
Connectez-vous à la [console IBM Cloud](https://{DomainName}/vpc/overview) et commencez dès aujourd'hui !

- **Les règles d'autorisation individuelles sur les instances, les clés, les volumes et les groupes de sécurité peuvent désormais être définies.** Découvrez quels sont les rôles requis pour gérer ces ressources dans [Autorisations de ressource requises pour les appels d'API et d'interface de ligne de commande](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Les utilisateurs avec accès en avant-première ne sont pas affectés par les règles existantes.

- **Le paramètre de vitesse du port d'une interface de réseau virtuel a été supprimé de l'interface utilisateur, de l'interface de ligne de commande et de l'API.**

- **La prise en charge multi-région pour surveiller les métriques est désormais disponible dans l'interface utilisateur.**


## 31 mai 2019
{: #may-31-2019}

**Nouvelle version de l'API disponible.** La version de l'API VPC on Classic a été mise à jour pour passer de `2019-01-01` à `2019-05-31`. Pour obtenir les instructions et les valeurs recommandées, voir [Gestion des versions](https://{DomainName}/apidocs/vpc-on-classic#versioning) dans l'API Virtual Private Cloud on Classic.

## 24 mai 2019
{: #may-24-2019}

- **Les instances d'équilibreur de charge avec le statut 'deleting' (suppression) sont désormais dans la vue de liste de l'interface utilisateur.** Lorsque vous effectuez une demande de suppression d'équilibreur de charge dans l'interface utilisateur,  la vue de liste continue à afficher l'équilibreur de charge tant que le processus de suppression n'est pas terminé.

- **Les fonctions de l'équilibreur de charge de couche 7 sont disponibles dans l'interface utilisateur.** Vous pouvez désormais configurer les fonctions de cet équilibreur dans l'interface utilisateur.

- **Les règles d'affichage et de modification de l'équilibreur de charge sont désormais disponibles dans l'interface utilisateur.** Une nouvelle fonction permettant d'afficher et modifier les règles d'équilibreur de charge est disponible dans l'interface utilisateur.

Pour plus d'informations, voir la [documentation sur l'équilibreur de charge](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10 mai 2019
{: #may-10-2019}


- **Les noms de profil ont changé**. Les noms de profil d'instance ont été modifiés. Les commandes permettant de mettre à disposition les instances doivent être mises à jour pour utiliser les nouveaux noms de profil. En lire plus [sur les profils ici](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **L'estimation de la tarification mensuelle d'une adresse IP flottante est disponible dans l'interface utilisateur**. Lorsque vous réservez une adresse IP flottante, vous pouvez désormais voir une ligne indiquant le coût mensuel estimé de cette adresse.

- **Support disponible pour Hyper Protect**.

- **Le nom du sous-réseau et la zone apparaissent sous forme de liens qui vont vous diriger vers le sous-réseau associé dans l'interface utilisateur**. Les sous-réseaux s'affichent désormais par nom plutôt que par ID de sous-réseau et le nom sert de lien vers la page des détails du sous-réseau associé.

- **L'estimateur récapitulatif des coûts est disponible dans l'interface utilisateur**.

- **L'interrogation des pools d'équilibreur de charge et des écouteurs est répercutée dans l'interface utilisateur**.

    * Sur la page des pools d'équilibreur de charge, vous pouvez désormais voir un compteur indiquant la dernière mise à jour, au-dessous du tableau des ressources.
    * L'intervalle d'interrogation par défaut est de 30 secondes.
    * Quatre (4) états différents peuvent être affichés : `newProvision`, `active`, `failed` et `all other states`.
        * Pour une nouvelle mise à disposition (newProvision) : l'intervalle est de 15 secondes. Les pools et les écouteurs passent à l'état actif (active) directement.
        * Pour l'état actif (active) : la liste des ressources et l'en-tête sont mis à jour toutes les 60 secondes.
        * Pour l'état d'échec (failed) : l'interrogation est arrêtée.
        * Pour tous les autres états (all other states) : l'interrogation se poursuit par intervalles de 30 secondes.
    * Si vous supprimez un écouteur, vous pouvez entraîner le changement du statut de l'en-tête à `Updating (Actions Unavailable)`.
    * Vous pouvez également constater que les informations d'en-tête sont actualisées lorsqu'une mise à jour est effectuée.
    * Ces mêmes changements s'appliquent à la fonction d'interrogation pour les écouteurs.

- **Le nombre de membres d'équilibreur de charge s'affiche sur la page des détails et des membres dans l'interface utilisateur, pendant l'interrogation des membres**.

    * Dans la section Etat de santé, vous apercevez le nombre de membres avec l'extraction des détails de l'instance à côté.
    * Dans la colonne Instances, vous apercevez le nombre de membres avec l'extraction des détails de l'instance à côté.
    * Une fois le chargement effectué, si vous avez des membres non valides, une icône d'avertissement jaune s'affiche.

- **La page des détails du cloud privé virtuel dans l'interface utilisateur affiche tous les sous-réseaux connectés**.
