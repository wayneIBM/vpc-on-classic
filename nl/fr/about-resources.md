---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# A propos des ressources d'infrastructure VPC
{: #about-vpc-infrastructure-resources}

Le terme _ressources_ désigne les composants d'un déploiement de cloud privé virtuel {{site.data.keyword.cloud}}. Chaque cloud privé virtuel (VPC) peut contenir des ressources des types suivants, qui peuvent être regroupées en fonction des besoins :

* **cloud privé virtuel (vpc)**
* **instance**
* **clé**
* **image**
* **sous-réseau**
* **volume **
* **listes de contrôle d'accès au réseau (acl)**
* **groupe de sécurité**
* **adresse IP flottante**
* **carte d'interface réseau virtuel (vnic)**
* **passerelle**
* **équilibreur de charge**
* **réseau privé virtuel (vpn)**

Certaines ressources d'infrastructure VPC héritent de leurs règles d'autorisation, qui régissent leur utilisation, à partir du VPC parent, tandis que d'autres sont gérées séparément. 

Actuellement, les type de ressources `instance`, `key`, `security-group`, `volume`, `loadbalancer` et `vpn` conservent leurs propres règles d'autorisation. Vous trouverez davantage de détails sur les autorisations de ressource concernant ces ressources ultérieurement dans ce document.
{: note}

## Règles de ressource
{: #resource-policies}

En général, l'accès aux ressources dans un cloud privé virtuel est contrôlé par des _règles_. Si vous voulez personnaliser l'accès aux ressources pour votre cloud privé virtuel, vous pouvez créer des règles personnalisées. Une règle de ressource peut cibler :

* toutes les ressources
* toutes les ressources d'un même type
* toutes les ressources dans un groupe
* une ressource individuelle

## Ressources et groupes de ressources
{: #resources-and-resource-groups}

Un _groupe de ressources_ est une collection de ressources, par exemple un cloud privé virtuel entier, ou un sous-réseau unique et sa liste de contrôle d'accès, qui sont associées en vue de l'établissement d'une autorisation et d'une utilisation. Considérez un groupe de ressources comme une collection d'infrastructure pouvant être utilisée par un projet, un service ou une équipe.

Vous pouvez utiliser la commande search dans l'interface de ligne de commande pour afficher les étiquettes associées à une ressource. Avec les filtres appropriés, vous pouvez afficher uniquement les instances qui vous intéressent, par exemple `ibmcloud resource search name:` ou `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Les entreprises de grande taille peuvent décider de diviser un cloud privé virtuel en groupes de ressources. Les entreprises de petite taille n'ont généralement pas besoin de diviser leur cloud privé virtuel en groupes de ressources car habituellement, tous les membres de leur équipe utilisent le cloud privé virtuel entier. Si vous connaissez _OpenStack_, un groupe de ressources est similaire au concept de _projet_ dans _OpenStack Keystone_.

Une ressource peut être affectée à un groupe de ressources UNIQUEMENT lorsque vous créez votre cloud privé virtuel. L'affectation d'une ressource à un groupe de ressources ne peut pas être changée une fois créée.
{: .note}

Les groupes de ressources ont été conçus essentiellement pour les organisations de grande taille. Pour la plupart des entreprises et des équipes, la répartition des ressources se trouve dans les limites du cloud privé virtuel. Cette règle empirique générale peut changer lorsqu'il est possible d'affecter des instances de serveur virtuel (VSI) à un groupe de ressources et des utilisateurs individuels à une ressource d'instance de serveur virtuel.

Lorsque vous configurez votre cloud privé virtuel IBM Cloud, si vous voulez utiliser des groupes de ressources, il est judicieux d'élaborer à l'avance un plan d'affectation des ressources et des utilisateurs de votre organisation à chaque groupe de ressources.
{: tip}

## Spécificités de VPC
{: #specifics-for-vpc}

Certaines ressources d'infrastructure VPC héritent de leurs règles d'autorisation, qui régissent leur utilisation, à partir du compte ou du VPC parent, alors que d'autres disposent de règles individuelles. 

### Autorisation de ressource VPC par type de ressource
{: #vpc-resource-authorization-by-resource-type}

Le tableau récapitule la façon dont les ressources de cloud privé virtuel sont autorisées par défaut.

| Type de ressource | Provenance de son autorisation par défaut |
|-----------|------------|
| Cloud privé virtuel | Vérification d'autorisation lors de sa création |
| Equilibreur de charge | Vérification d'autorisation lors de sa création |
| Réseau privé virtuel | Vérification d'autorisation lors de sa création |
| Sous-réseau | Cloud privé virtuel parent |
| Passerelle publique | Cloud privé virtuel parent |
| Instance | Vérification d'autorisation lors de sa création |
| Groupe de sécurité | Vérification d'autorisation lors de sa création |
| Clé | Vérification d'autorisation lors de sa création |
| Image | Compte |
| Adresse IP flottante | Compte |
| Liste de contrôle d'accès au réseau | Compte|
| Volume | Vérification d'autorisation lors de sa création |

L'adresse IP flottante et les listes de contrôle d'accès au réseau peuvent être créées au niveau du compte, si aucune n'est affectée. Toutefois, dès qu'une adresse IP flottante est affectée à une instance ou qu'une liste de contrôle d'accès est affectée à un sous-réseau, ces ressources sont soumises à la portée de l'autorisation du cloud privé virtuel.
{: note}

### Couverture VPC des rôles et des actions autorisées sur les ressources
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

Voici quelques exemples de ce qui se passe lorsqu'un utilisateur disposant de certains rôles tente d'effectuer certaines actions, dans le cas où il existe deux clouds privés virtuels :

* Utilisateur _admin_, avec les droits d'accès **Administrateur** sur toutes les ressources
  * Création de `vpc1` : OK

* Utilisateur _editor_, avec les droits d'accès **Editeur** sur toutes les ressources
  * Création de `vpc2` : OK
  * Mise à jour de `vpc2` : OK
  * Suppression de `vpc2` : OK

* Utilisateur _operator_, avec les droits d'accès **Opérateur** sur toutes les ressources
  * Création de `vpc` : refusé
  * Mise à jour de `vpc1` : refusé
  * Suppression de `vpc1` : refusé

* Utilisateur _viewer_, avec les droits d'accès **Afficheur** sur toutes les ressources
  * Affichage de la liste des clouds privés virtuels : affichage de [vpc1]
  * Lecture de `vpc1` : OK

* Utilisateur _noRole_, sans règle
  * Affichage de la liste des clouds privés virtuels : affichage de []
  * Lecture de `vpc1` : refusé

### Tableau récapitulatif de la couverture VPC
{: #vpc-coverage-summary-table}

Pour toute règle accordant l'accès par rôle (Administrateur, Editeur, Opérateur, Afficheur), les actions suivantes sont indiquées (X=refusé, o=OK) :

 rôle :      | aucun | Afficheur | Opérateur | Editeur | Administrateur
-----------:|------|--------|-------|--------|-------
Création      | X    | X      | X     | o      | o
Affichage        | X    | o      | X     | o      | o
Lecture        | X    | o      | X     | o      | o
Mise à jour      | X    | X      | X     | o      | o
Suppression      | X    | X      | X     | o      | o


## Autorisation de ressource de VPN for VPC
{: #resource-authorizations-of-vpn-for-vpc}

L'autorisation de ressource de **VPN for VPC** est configurée séparément des autres types d'autorisation de ressource dans votre cloud privé virtuel, mais de la même manière.

Les règles dans VPN for VPC contrôlent l'accès aux ressources de réseau privé virtuel, notamment les passerelles VPN. Pour accéder aux ressources enfants d'une passerelle VPN, comme des connexions VPN et des routages CIDR locaux ou homologues, vous devez disposer de l'accès en **lecture** (ou supérieur) à la passerelle VPN parent. Les règles de ressource ne peuvent être utilisées qu'avec des connexions VPN disposant également de l'accès en **lecture** ou supérieur pour la passerelle VPN parent. Une règle de ressource dans un réseau privé virtuel peut cibler :

* toutes les ressources VPN (passerelles VPN, connexions VPN et stratégies IKE et IPSec)
* toutes les ressources VPN dans un groupe de ressources
* une passerelle VPN individuelle

L'utilisation du réseau privé virtuel est facturée séparément.
{: note}

### Couverture VPN for VPC des rôles et des actions autorisées sur les ressources
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

Les rôles utilisateur qui sont pris en charge sont les mêmes que ceux pris en charge dans l'autorisation de ressource VPC, mais des actions différentes sont activées pour chaque rôle.

Voici quelques exemples de ce qui se passe lorsque des utilisateurs affectés à certains rôles tentent d'effectuer certaines actions liées au réseau privé virtuel :

* Utilisateur _admin_, avec les droits d'accès **Administrateur** sur toutes les ressources
  * Création, mise à jour, suppression, lecture, affichage de la liste des passerelles VPN : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des connexions VPN : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des blocs CIDR homologues et locaux des connexions VPN : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des stratégies IKE : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des stratégies IPSec : OK

* Utilisateur _editor_, avec les droits d'accès **Editeur** sur toutes les ressources
  * Similaire à Administrateur

* Utilisateur _operator_, avec les droits d'accès **Opérateur** sur toutes les ressources
  * Création, mise à jour, suppression des passerelles VPN : refusé
  * Création, mise à jour, suppression des connexions VPN : refusé
  * Création, mise à jour, suppression des blocs CIDR homologues et locaux des connexions VPN : refusé
  * Création, mise à jour, suppression des stratégies IKE : refusé
  * Création, mise à jour, suppression des stratégies IPSec : refusé
  * Lecture, affichage de la liste des passerelles VPN : OK - affichage des données réelles
  * Lecture, affichage de la liste des connexions VPN : OK - affichage des données réelles
  * Lecture, affichage de la liste des blocs CIDR homologues et locaux des connexions VPN : OK - affichage des données réelles
  * Lecture, affichage de la liste des stratégies IKE : OK - affichage des données réelles
  * Lecture, affichage de la liste des stratégies IPSec : OK - affichage des données réelles

* Utilisateur _viewer_, avec les droits d'accès **Afficheur** sur toutes les ressources
  * Similaire à Opérateur

* Utilisateur _noRole_, sans règle
  * Lecture, affichage de la liste des passerelles VPN : refusé - la liste contient des données vides
  * Lecture, affichage de la liste des connexions VPN : refusé
  * Lecture, affichage de la liste des blocs CIDR homologues et locaux des connexions VPN : refusé
  * Lecture, affichage de la liste des stratégies IKE : refusé - la liste contient des données vides
  * Lecture, affichage de la liste des stratégies IPSec : refusé - la liste contient des données vides

### Tableau récapitulatif de la couverture VPN pour VPC
{: #vpn-for-vpc-coverage-summary-table}

Le mappage des rôles et des actions dans VPN pour VPC est présenté dans le tableau suivant (X=refusé, o=OK) :

rôle :       | aucun | Afficheur | Opérateur | Editeur | Administrateur
-----------:|------|--------|-------|--------|-------
Création      | X    | X      | X     | o      | o
Affichage        | X    | o      | o     | o      | o
Lecture        | X    | o      | o     | o      | o
Mise à jour      | X    | X      | X     | o      | o
Suppression      | X    | X      | X     | o      | o

## Autorisation de ressource pour Load Balancer for VPC
{: #resource-authorization-for-load-balancer-for-vpc}

L'utilisation de l'autorisation de ressource pour **Load Balancer for VPC** est configurée séparément des autres autorisations de ressource dans votre cloud virtuel privé, mais d'une manière comparable.

L'utilisation de Load Balancer est facturée séparément.
{: note}

### Couverture de Load Balancer des rôles et des actions autorisées sur les ressources
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

Voici quelques exemples de ce qui se passe lorsque des utilisateurs affectés à certains rôles tentent d'effectuer certaines actions liées à l'équilibreur de charge du cloud privé virtuel :

* Utilisateur _admin_, avec les droits d'accès **Administrateur** sur toutes les ressources
  * Création, mise à jour, suppression, lecture, affichage de la liste des équilibreurs de charge : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des écouteurs : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des pools : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des membres : OK
  * Création, mise à jour, suppression, lecture, affichage de la liste des statistiques d'équilibreur de charge : OK

* Utilisateur _editor_, avec les droits d'accès **Editeur** sur toutes les ressources
  * Similaire à Administrateur

* Utilisateur _operator_, avec les droits d'accès **Opérateur** sur toutes les ressources
  * Création, mise à jour, suppression, lecture, affichage de la liste des équilibreurs de charge : refusé
  * Création, mise à jour, suppression, lecture, affichage de la liste des écouteurs : refusé
  * Création, mise à jour, suppression, lecture, affichage de la liste des pools : refusé
  * Création, mise à jour, suppression, lecture, affichage de la liste des membres : refusé
  * Création, mise à jour, suppression, lecture, affichage de la liste des statistiques d'équilibreur de charge : refusé

* Utilisateur _viewer_, avec les droits d'accès **Afficheur** sur toutes les ressources
  * Lecture, affichage de la liste des équilibreurs de charge : OK
  * Lecture, affichage de la liste des écouteurs : OK
  * Lecture, affichage de la liste des pools : OK
  * Lecture, affichage de la liste des membres : OK
  * Lecture des statistiques d'équilibreur de charge : OK

* Utilisateur _noRole_, sans règle
  * Lecture, affichage de la liste des équilibreurs de charge : refusé - la liste contient des données vides
  * Lecture, affichage de la liste des écouteurs : refusé
  * Lecture, affichage de la liste des pools : refusé
  * Lecture, affichage de la liste des membres : refusé
  * Lecture des statistiques d'équilibreur de charge : refusé

### Tableau récapitulatif de la couverture de l'équilibreur de charge
{: #load-balancer-coverage-summary-table}

Le mappage des rôles et des actions dans Equilibreur de charge pour VPC est présenté dans le tableau suivant (X=refusé, o=OK) :

rôle :       | aucun | Afficheur | Opérateur | Editeur | Administrateur
-----------:|------|--------|-------|--------|-------
Création      | X    | X      | X     | o      | o
Affichage        | X    | o      | X     | o      | o
Lecture        | X    | o      | X     | o      | o
Mise à jour      | X    | X      | X     | o      | o
Suppression      | X    | X      | X     | o      | o


## Autorisation de ressource pour Virtual Server for VPC
{: #planning-virtual-servers-for-vpc-permissions}

Définissez l'autorisation de ressource pour **Virtual Server for VPC** séparément des autres autorisations de ressource dans votre cloud privé virtuel. 

L'utilisation de Virtual Server for VPC est facturée séparément.
{: note}

* En tant qu'administrateur, vous pouvez définir des rôles et effectuer les actions disponibles sur {{site.data.keyword.vsi_is_short}}.
* En tant qu'éditeur, vous pouvez modifier l'état et créer ou supprimer des sous-ressources.
* En tant qu'opérateur, vous pouvez effectuer des actions qui ne modifient pas l'état des ressources. 
* En tant qu'afficheur, vous pouvez effectuer des actions qui ne modifient pas l'état des ressources. 

### Tableau récapitulatif de la couverture pour Virtual Server
{: #vsi-coverage-summary-table}

Le mappage des rôles et des actions dans Virtual Server for VPC est présenté dans le tableau suivant (X=refusé, o=OK) :

rôle :       | aucun | Afficheur | Opérateur | Editeur | Administrateur
-----------:|------|--------|-------|--------|-------
Création      | X    | X      | X     | o      | o
Affichage        | X    | o      | o     | o      | o
Lecture        | X    | o      | o     | o      | o
Mise à jour      | X    | X      | X     | o      | o
Suppression      | X    | X      | X     | o      | o

Lorsque vous créez une instance, vous devez également disposer de l'accès Opérateur pour les ressources VPC et Groupes de sécurité, si ces ressources sont spécifiées. Les ressources Sous-réseau et Adresse IP héritent des droits d'accès du VPC associé.  
{: tip} 
