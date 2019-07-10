---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

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

# Autorisations de ressource requises pour les appels API et d'interface de ligne de commande
{: #resource-authorizations-required-for-api-and-cli-calls}

Le tableau ci-dessous répertorie les autorisations requises pour l'interaction avec les objets de l'infrastructure {{site.data.keyword.cloud}} Virtual Private Cloud. Ces informations sont particulièrement utiles pour savoir quand effectuer des appels API. A savoir pour utiliser ce tableau :

Les termes _associé_ et _dissocié_ signifient que la ressource est associée à un ou plusieurs clouds privés virtuels (directement ou indirectement par le biais de ses sous-réseaux), ou dissociée. Une adresse IP flottante ou une liste de contrôle d'accès au réseau dissociée ne possède pas de sous-réseau ; par conséquent, elle n'est associée à aucun cloud privé virtuel et l'autorisation par VPC n'est pas applicable.

* Les actions **Afficher** et **Répertorier** correspondent au rôle **Afficheur**.
* Les actions **Exploiter** correspondent au rôle **Opérateur**.
* L'action **Mettre à jour** et les actions connexes correspondent au rôle **Editeur** ou **Administrateur**.
* Les ressources sont protégées par une autorisation, mais pas les objets de référence de ressource (ID, nom de ressource de cloud, etc.).


| Ressource | Opération | Autorisation requise |
|--------|--------|---------|
| Cloud privé virtuel | Créer | Autorisation d'affichage pour le groupe de ressources pour ce cloud privé virtuel et autorisation de mise à jour pour les ressources de cloud privé virtuel|
| Cloud privé virtuel | Mettre à jour, supprimer |  Autorisation de mise à jour pour le cloud privé virtuel |
| Cloud privé virtuel |  Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel  |
| Liste de contrôle par défaut du cloud privé virtuel, groupe de sécurité par défaut|  Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel |
| Préfixes d'adresse de cloud privé virtuel |  Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le cloud privé virtuel |
| Préfixes d'adresse de cloud privé virtuel |  Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel  |
|—————|——————|———————|
| Adresse IP flottante (dissociée) | Créer, mettre à jour, supprimer | Tout utilisateur de compte et autorisation d'affichage pour tous les services de gestion des comptes, si la ressource est créée dans le groupe de ressources par défaut. Cliquez [ici](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources#setting-up-viewer-access) pour des instructions sur la configuration de l'accès Afficheur. **Remarque : l'association et la dissociation sont effectuées dans le cadre de l'opération de mise à jour de l'adresse IP flottante**|
| Adresse IP flottante (dissociée) | Afficher, répertorier | Utilisateur de compte |
| Adresse IP flottante (associée) | Mettre à jour | Autorisation de mise à jour pour le cloud privé virtuel du sous-réseau associé (vous ne pouvez pas créer ni supprimer une adresse IP flottante une fois que celle-ci a été associée) |
| Adresse IP flottante (associée) | Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel du sous-réseau associé de l'adresse IP flottante | 
|——————|———————|————————|
| Liste de contrôle d'accès au réseau (dissociée), règles de liste de contrôle d'accès | Créer, mettre à jour, supprimer | Tout utilisateur de compte |
| Liste de contrôle d'accès au réseau (dissociée), règles de liste de contrôle d'accès | Afficher, répertorier | Tout utilisateur de compte |
| Liste de contrôle d'accès au réseau (associée), règles de liste de contrôle d'accès | Créer, mettre à jour, supprimer | Autorisation de mise à jour sur tous les réseaux connectés associés au cloud privé virtuel |
| Liste de contrôle d'accès au réseau (associée), règles de liste de contrôle d'accès | Afficher, répertorier | Autorisation d'affichage pour au moins l'un des sous-réseaux associés au cloud privé virtuel |
|——————|———————|————————|
| Passerelle publique | Créer, mettre à jour, supprimer |  Autorisation de mise à jour pour le cloud privé virtuel de la passerelle publique |
| Passerelle publique | Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel de la passerelle publique |
|—————————|————————|———————————|
| Géographie | Afficher, répertorier |  Pour les régions et les zones, tout utilisateur de compte |
|———————|————————|—————————|
| Clé SSH | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour la clé SSH |
| Clé SSH | Afficher, répertorier | Autorisation d'affichage pour la clé SSH |
|————————|—————————|————————|
| Sous-réseau | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le cloud privé virtuel du sous-réseau |
| Sous-réseau | Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel du sous-réseau |
| Liste de contrôle d'accès au sous-réseau ou passerelle publique | Associer, dissocier | Autorisation de mise à jour pour le cloud privé virtuel du sous-réseau |
| Liste de contrôle d'accès au sous-réseau ou passerelle publique | Afficher, répertorier | Autorisation d'affichage pour le cloud privé virtuel du sous-réseau |
|——————|—————————|————————|
| Groupe de sécurité | Obtenir     | Autorisation d'affichage sur le groupe de sécurité.
| Groupe de sécurité | Répertorier    | Autorisation d'affichage sur le groupe de sécurité (sinon le groupe est omis dans la réponse.)
| Groupe de sécurité | Créer  | Autorisation d'édition sur le groupe de sécurité qui sera créé (par exemple, autorisation d'édition sur tous les groupes de sécurité ou sur le groupe de ressources dans lequel le groupe de sécurité sera créé.)<br />Autorisation d'affichage sur le cloud privé virtuel dans lequel le groupe sera créé.
| Groupe de sécurité | Mettre à jour / supprimer  | Autorisation d'édition sur le groupe de sécurité.
| Règle du groupe de sécurité | Obtenir / répertorier | Autorisation d'affichage sur le groupe de sécurité.
| Règle du groupe de sécurité | Créer / mettre à jour / supprimer | Autorisation d'édition sur le groupe de sécurité.
| Interface réseau du groupe de sécurité | Obtenir     | Autorisation d'affichage sur le groupe de sécurité.<br />Autorisation d'affichage sur l'instance.
| Interface réseau du groupe de sécurité | Répertorier    | Autorisation d'affichage sur le groupe de sécurité (sinon une réponse 404 est obtenue.)<br />Autorisation d'affichage sur l'instance à laquelle appartient l'interface réseau (sinon l'interface est omise dans la réponse.)
| Interface réseau du groupe de sécurité | Connecter / déconnecter | Autorisation d'exploitation sur le groupe de sécurité.<br />Autorisation d'édition sur l'instance à laquelle appartient l'interface réseau.
|—————————|—————————|—————————|
| Images | Afficher, répertorier  | Tout utilisateur de compte |
|—————————|—————————|—————————|
| Instances | Créer| Autorisation de mise à jour pour l'instance et le volume<br />Autorisation d'exploitation pour VPC<br />Autorisation d'exploitation pour le groupe de sécurité, le cas échéant|
| Instances | Mettre à jour, supprimer | Autorisation de mise à jour pour l'instance |
| Instances | Afficher, répertorier  | Autorisation d'affichage pour l'instance |
| Actions d'instance | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour l'instance|
| Actions d'instance, Initialisation, Cartes d'interface réseau| Afficher, répertorier  | Autorisation d'affichage pour l'instance |
| Adresses IP flottantes de l'instance | Afficher, répertorier | Autorisation d'affichage pour l'instance et le cloud privé virtuel du sous-réseau associé |
| Adresses IP flottantes de l'instance | Associer | Autorisation de mise à jour pour l'instance<br />Autorisation d'exploitation pour le cloud privé virtuel du sous-réseau associé|
| Adresses IP flottantes de l'instance | Dissocier | Autorisation de mise à jour pour l'instance |
|————————|——————|————————|
| Passerelle VPN | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le réseau privé virtuel |
| Passerelle VPN | Afficher, répertorier  | Autorisation d'affichage pour le réseau privé virtuel |
| Connexions de passerelle VPN | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le réseau privé virtuel |
| Connexions de passerelle VPN | Afficher, répertorier  | Autorisation d'affichage pour le réseau privé virtuel |
| Connexions, stratégies IPSec et stratégies IKE de passerelle VPN | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le réseau privé virtuel |
| Connexions, stratégies IPSec et stratégies IKE de passerelle VPN|Afficher, répertorier  | Autorisation d'affichage pour le réseau privé virtuel |
|————————|——————|————————|
| Equilibreur de charge | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour l'équilibreur de charge |
| Equilibreur de charge | Afficher, répertorier  | Autorisation d'affichage pour l'équilibreur de charge |
| Pools et écouteurs d'équilibreur de charge | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour l'équilibreur de charge |
| Pools et écouteurs d'équilibreur de charge | Afficher, répertorier  | Autorisation d'affichage pour l'équilibreur de charge |
|————————|——————|————————|
| Volumes | Créer, mettre à jour, supprimer | Autorisation de mise à jour pour le volume
| Volumes | Afficher, répertorier  | Autorisation d'affichage pour le volume |
| Profils de volume | Afficher, répertorier  | Tout utilisateur de compte |


