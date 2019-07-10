---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Gestion des droits utilisateur pour les ressources de cloud privé virtuel
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.cloud}} Virtual Private Cloud utilise un contrôle d'accès basé sur les rôles qui permet aux administrateurs de compte de contrôler l'accès de leurs utilisateurs aux ressources de cloud privé virtuel (VPC). 

Pour plus d'informations sur la façon dont IBM Cloud VPC utilise le contrôle d'accès basé sur les rôles et se sert des règles d'accès et de rôle IAM, voir [Affectation d'un accès basé sur les rôles aux ressources de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources).

Ce document explique comment l'administrateur du compte peut ajouter des utilisateurs supplémentaires au compte et leur accorder les droits appropriés pour gérer les [ressources d'infrastructure de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources). Il présente deux scénarios courants pour un administrateur de cloud privé virtuel :

* **Scénario pour un accès simple :** explique comment affecter une règle d'accès à un utilisateur, pour qu'il puisse créer et utiliser des ressources d'infrastructure VPC (notamment des clouds privés virtuels). 

* **Scénario pour un accès par des équipes :** explique comment configurer des groupes de ressources et des règles d'accès qui permettent à deux équipes distinctes de créer et d'utiliser les ressources de cloud privé virtuel affectées à leur équipe.

L'application des modifications apportées à des règles d'accès IAM pour le cloud privé virtuel peut prendre jusqu'à 10 minutes.
{: note}

## Scénario pour un accès simple
{: #simple-access-scenario}

Ce scénario présente les étapes de base nécessaires pour configurer un utilisateur individuel. Nous allons traiter deux cas de figure : l'invitation d'un nouvel utilisateur sur le compte et la modification des droits d'un utilisateur existant.

### Invitation d'un nouvel utilisateur pour créer ou gérer des ressources de cloud privé virtuel
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

Invitez un utilisateur IBM Cloud sur votre compte et autorisez-le à accéder au service `VPC Infrastructure` pour qu'il puisse afficher, créer et mettre à jour des ressources de cloud privé virtuel. Cette section présente rapidement les étapes IAM. Vous trouverez des informations supplémentaires en suivant les liens présentés dans la section **Liens connexes** à la fin de ce document.

Voici les étapes de base à effectuer dans IAM pour inviter des utilisateurs dans des ressources et des services de cloud privé virtuel :

1. Accédez à l'[interface des utilisateurs IAM ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam#/users){: new_window} dans la console IBM Cloud.
2. Dans la page **Utilisateurs**, cliquez sur **Inviter des utilisateurs**.
3. Dans la page **Inviter des utilisateurs**, dans la section **Utilisateurs**, entrez les adresses électroniques des utilisateurs à inviter dans la zone **Adresse électronique**.
4. Dans la section **Accès**, développez **Services** et effectuez les opérations suivantes :
  * Sélectionnez **Ressource** dans la liste **Affecter l'accès à**.
  * Sélectionnez **VPC Infrastructure** dans la liste **Services**.
  * Sélectionnez le rôle d'accès à la plateforme à affecter aux utilisateurs. Il peut s'agir du rôle **Administrateur**, **Editeur**, **Opérateur** ou **Afficheur**.
  * Cliquez sur **Inviter des utilisateurs**.

### Attribution du droit de gestion des ressources de cloud privé virtuel à un utilisateur existant
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

Ce scénario présente les étapes de base nécessaires pour accorder à un utilisateur existant sur votre compte le droit d'éditer des ressources de cloud privé virtuel.

Au cours des étapes qui suivent, vous aller créer deux règles IAM. Celles-ci sont nécessaires pour que votre utilisateur puisse créer et utiliser les ressources d'infrastructure VPC. Toutes les ressources doivent se trouver dans le groupe de ressources par défaut du compte.

1. Accédez à l'[interface des utilisateurs IAM ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam#/users){: new_window} dans la console IBM Cloud.
2. Sélectionnez l'utilisateur à autoriser.
3. Dans l'onglet **Règles d'accès**, sélectionnez **Affecter un accès**.
4. Sélectionnez **Affecter l'accès au sein d'un groupe de ressources**.
5. Sélectionnez le groupe de ressources par défaut du compte.
6. Assurez-vous que l'option **Affecter l'accès à un groupe de ressources** conserve la valeur **Afficheur**.
7. Pour affecter le service approprié, sélectionnez **VPC Infrastructure**.
8. Assurez-vous que la valeur de **Type de ressource** est **Tous les types de ressource**.
9. Sélectionnez le rôle **Editeur**.
10. Cliquez sur **Affecter**.

A présent, l'utilisateur est autorisé à créer et à utiliser des ressources de cloud privé virtuel dans le groupe de ressources par défaut du compte. La première règle (étapes 1 à 6) autorise l'utilisateur à afficher le groupe de ressources par défaut du compte. La deuxième règle (étapes 7 à 10) a affecté à l'utilisateur le rôle **Editeur** pour les ressources de l'infrastructure VPC, mais l'accès est limité aux ressources figurant dans le groupe de ressources par défaut du compte.

{: tip}

## Affichage des droits de votre utilisateur
{: #viewing-your-user-s-permissions}

Les règles peuvent être affichées dans l'onglet **Règles d'accès** de l'utilisateur.

Vous pouvez utiliser les commandes d'interface de ligne de commande suivantes pour valider les droits d'accès au groupe de ressources affectés à votre utilisateur, par une règle ou par un groupe d'accès :

**Par une règle**
```
ibmcloud iam user-policies <username>
```
**Par un groupe d'accès**
```
ibmcloud iam access-groups -u <username>
```

L'application des modifications apportées à des règles d'accès IAM pour le cloud privé virtuel peut prendre jusqu'à 10 minutes.
{: note}

## Scénario pour un accès par des équipes
{: #team-access-scenario}

Ce scénario explique comment un administrateur de compte peut affecter une autorisation de sorte que différentes équipes aient accès à des ressources de cloud privé virtuel distinctes. L'exemple utilise des _groupes de ressources_ afin de configurer un accès aux ressources distinct pour deux équipes. Dans cet exemple, les ressources ne sont pas partagées par les équipes.

L'exemple présente le processus de création de groupes de ressources, de création de groupes d'accès, et d'affectation des règles appropriées afin de permettre à vos équipes d'accéder à des ressources de cloud privé virtuel distinctes.

Imaginez que vous configurez deux équipes de projet différentes qui utiliseront deux clouds privés virtuels distincts. Vous affectez l'accès aux membres d'équipe de sorte qu'ils aient accès aux ressources de cloud privé virtuel de leur équipe (uniquement).

* Votre première équipe est une équipe de test. Vous avez décidé de lui affecter l'accès aux clouds privés virtuels d'un groupe de ressources nommé `test_vpcs`.
* La deuxième équipe est votre équipe de production. Vous lui affectez l'accès aux clouds privés virtuels d'un groupe de ressources nommé `production_vpcs`.

Cette stratégie peut être utilisée pour affecter des ressources de cloud privé virtuel distinctes à des équipes. Toutefois, toutes les ressources partagent les mêmes quotas de cloud privé virtuel pour le compte. Pour plus d'informations sur les quotas et les limites, voir [Quotas de cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas).
{: tip}

### Etape 1 : créez des groupes de ressources
{: #step-1-create-resource-groups}

Par défaut, les administrateurs de compte disposent de l'accès permettant de créer des groupes de ressources. Les autres utilisateurs doivent d'abord obtenir le rôle **Editeur** pour **Tous les services de gestion des comptes** afin de pouvoir créer des groupes de ressources.

Tout d'abord, vous devez créer des groupes de ressources qui contiendront les ressources de cloud privé virtuel de chacune de vos équipes.

1. Créez un groupe de ressources dont le nom est `test_team`.  
2. Créez un groupe de ressources dont le nom est `production_team`.  

Pour plus d'informations sur la création de groupes de ressources, voir [Gestion des groupes de ressources](/docs/resources?topic=resources-rgs).

### Etape 2 : créez des groupes d'accès
{: #step-2-create-access-groups}

L'accès aux ressources peut être affecté à des utilisateurs individuels ou à des groupes d'utilisateurs. Les groupes d'utilisateurs disposant des mêmes droits d'accès sont appelés _groupes d'accès_. Dans ce scénario, vous allez créer un groupe d'accès pour représenter chaque regroupement de membres d'équipe nécessitant un type spécifique d'accès au cloud privé virtuel, c'est-à-dire au total quatre groupes d'accès uniques.

**Tâche :** créez quatre groupes d'accès avec les noms suivants : `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs` et `production_team_view_vpcs`.

Pour de l'aide relative à la création de groupes d'accès, voir [Créer des groupes d'accès](/docs/iam?topic=iam-groups#create_ag).

### Etape 3 : ajoutez des règles IAM aux groupes d'accès que vous venez de créer
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

Ajoutez les règles d'accès au cloud privé virtuel nécessaires pour que le groupe d'accès `test_team` puisse gérer (créer, mettre à jour et supprimer) des ressources de cloud privé virtuel.  

1. Accédez à l'[interface de groupe IAM ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam#/groups){: new_window} dans la console IBM Cloud.
2. Sélectionnez le groupe d'accès de votre choix (commencez par `test_team_manage_vpcs`).
3. Dans l'onglet **Règles d'accès**, cliquez sur **Affecter un accès**.
4. Sélectionnez **Affecter l'accès au sein d'un groupe de ressources**.
5. Sélectionnez le groupe de ressources de votre choix (commencez par `test_team`).
6. Assurez-vous que l'option **Affecter l'accès à un groupe de ressources** conserve la valeur **Afficheur**.
7. Sélectionnez le service **VPC Infrastructure**.
8. Assurez-vous que l'option **Type de ressource** conserve la valeur **Tous les types de ressource**.
9. Sélectionnez le rôle **Editeur**.
10. Sélectionnez **Affecter**.

Ces étapes affectent également l'accès **Afficheur** au groupe de ressources `test_team`. L'accès Afficheur est requis pour mettre à jour, créer et supprimer des ressources au sein du groupe de ressources `test_team`.

{: tip}

Répétez les étapes précédentes pour les trois autres groupes d'accès. Vous effectuez les tâches suivantes pour mettre en corrélation les groupes d'accès avec les groupes de ressources :

* Affectez au groupe d'accès `test_team_view_vpcs` le rôle **Afficheur** pour les ressources de l'infrastructure VPC dans le groupe de ressources `test_team`. 
* Affectez au groupe d'accès `production_team_manage_vpcs` le rôle **Editeur** pour les ressources de l'infrastructure VPC dans le groupe de ressources `production_team`. 
* Affectez au groupe d'accès `production_team_view_vpcs` le rôle **Afficheur** pour les ressources de l'infrastructure VPC dans le groupe de ressources `production_team`. 

Les utilisateurs disposant de l'accès **Editeur** aux ressources de cloud privé virtuel peuvent également les afficher. Il n'est pas nécessaire d'ajouter des membres à la fois au groupe d'accès **Editeur** ET au groupe d'accès **Afficheur**.

#### Configuration de l'accès Afficheur
{: #setting-up-viewer-access)

Les ressources de type adresse IP flottante (`Floating IP`) sont créées dans le groupe de ressources par défaut du compte. Par conséquent, les utilisateurs qui doivent gérer des `adresses IP flottantes` ont besoin de l'accès **Afficheur** au groupe de ressources par défaut du compte.
{: tip}

Voici comment créer cet accès **Afficheur** :

1. Accédez à l'[interface de groupe IAM ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam#/groups){: new_window} dans la console IBM Cloud.
2. Sélectionnez le groupe d'accès de votre choix (commencez par `test_team_manage_vpcs`).
3. Dans l'onglet **Règles d'accès**, cliquez sur **Affecter un accès**.
4. Sélectionnez **Affecter l'accès aux services de gestion des comptes**.
7. Sélectionnez le service **Tous les services de gestion des comptes**.
9. Sélectionnez le rôle **Afficheur**.
10. Sélectionnez **Affecter**.

Répétez les étapes précédentes pour affecter au groupe d'accès `production_team_manage_vpcs` le rôle **Afficheur** pour **Tous les services de gestion des comptes**.

### Etape 4 : ajoutez des utilisateurs aux groupes d'accès
{: #step-4-add-users-to-the-access-groups}

A présent, vous pouvez affecter des membres d'équipe (utilisateurs) aux groupes d'accès appropriés. Suivez les étapes ci-dessous pour ajouter chaque membre de l'équipe de test au groupe d'accès permettant la gestion du cloud privé virtuel de l'équipe de test :

1. Accédez à l'[interface de groupe IAM ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/iam#/groups){: new_window} dans la console IBM Cloud.
2. Sélectionnez le groupe d'accès de votre choix (commencez par `test_team_manage_vpcs`).
3. Dans l'onglet **Utilisateurs**, cliquez sur **Ajouter des utilisateurs**.
4. Sélectionnez chaque utilisateur à ajouter au groupe d'accès.
5. Cliquez sur **Ajouter au groupe**.

Répétez les étapes précédentes pour :
* Affecter les utilisateurs de votre choix au groupe d'accès `test_team_view_vpcs`.
* Affecter les utilisateurs de votre choix au groupe d'accès `production_team_manage_vpcs`.
* Affecter les utilisateurs de votre choix au groupe d'accès `production_team_view_vpcs`.

Il n'est pas nécessaire d'affecter aux utilisateurs l'accès **Afficheur** pour les ressources de cloud privé virtuel s'ils disposent de l'accès permettant la gestion.
{: tip}

## Etapes suivantes
{: #permissions-next-steps}

Les deux exemples d'équipe sont désormais configurés pour l'utilisation de clouds privés virtuels. A ce stade, les membres des groupes d'accès `test_team_manage_vpcs` et `production_team_manage_vpcs` peuvent créer des clouds privés virtuels dans les groupes de ressources qui leur sont affectés.


## Liens connexes
{: #permissions-related-links}

* [Gestion de l'identité et de l'accès](/docs/iam?topic=iam-getstarted)
* [Gestion des utilisateurs et de l'accès](/docs/iam?topic=iam-iamuserinv)
* [Présentation d'IAM](/docs/iam?topic=iam-iamoverview)
