---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, control

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

# Affectation d'un accès basé sur les rôles aux ressources de cloud privé virtuel
{: #assigning-role-based-access-to-vpc-resources}

Les administrateurs de compte peuvent utiliser des _règles_ d'autorisation, qui contrôlent l'accès aux _ressources_ en fonction du _rôle_ de l'utilisateur. Les règles peuvent également être appliquées pour :

(1) l'autorisation d'un groupe d'utilisateurs, appelé _groupe d'accès_,

(2) les caractéristiques affectées d'une _ressource_ unique, ou

(3) une collection de ressources, appelée _groupe de ressources_.

Les autorisations pour les ressources et les autorisations pour les utilisateurs peuvent être affectées indépendamment les unes des autres.

Pour plus d'informations sur la création d'utilisateurs, de groupes d'accès utilisateur, de groupes de ressources et de règles, voir [A propos des ressources](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources).

## Contrôle d'accès basé sur IAM
{: #iam-based-access-control}

En général, les pratiques de gestion des ressources et des autorisations d'{{site.data.keyword.cloud}} Virtual Private Cloud sont coordonnées avec les services IAM (IBM Cloud Identity and Access Management). Pour plus d'informations sur IAM, les groupes de ressources et les groupes d'accès en général, voir les documents IBM Cloud suivants :

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [Groupes de ressources](/docs/overview?topic=overview-whatis-rgs)
* [Groupes d'accès](/docs/overview?topic=overview-cloudaccess)

Certaines fonctions du service IBM Cloud IAM ont été personnalisées en vue de leur utilisation dans IBM Cloud VPC. Le document [A propos des ressources](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources) présente des informations supplémentaires sur les règles d'autorisation IAM telles qu'appliquées dans un cloud privé virtuel.

En résumé, vous pouvez affecter des autorisations basées sur IAM en fonction :

* d'utilisateurs individuels
* de groupes d'accès (groupes d'utilisateurs)
* de types de ressource spécifiques
* de groupes de ressources

## Affectation de droits utilisateur
{: #assigning-user-permissions}

Pour les utilisateurs, vous contrôlez l'accès en affectant des rôles IAM définis par le système :

* Administrateur
* Editeur
* Opérateur
* Afficheur

Le tableau ci-dessous présente la correspondance entre chaque rôle utilisateur et le type de contrôle qu'il autorise pour les clouds privés virtuels :

| Nom du rôle | Type d'accès autorisé |
|-----------|-------------------------|
| Afficheur | Afficher le cloud privé virtuel, répertorier les clouds privés virtuels  |
| Editeur | Afficher le cloud privé virtuel, répertorier les clouds privés virtuels, supprimer des clouds privés virtuels, mettre à jour des clouds privés virtuels |
| Opérateur  | Afficher le cloud privé virtuel, répertorier les clouds privés virtuels |
| Administrateur |Afficher le cloud privé virtuel, répertorier les clouds privés virtuels, supprimer des clouds privés virtuels, mettre à jour des clouds privés virtuels, affecter des règles à d'autres utilisateurs |


## Etapes suivantes
{: #permissions-next-steps}

Pour des instructions pas à pas d'octroi de droits basés sur les rôles à des utilisateurs pour certaines tâches, voir la rubrique [Gestion des droits utilisateur pour les ressources de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).
