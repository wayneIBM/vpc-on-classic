---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, api, rias, ui, console

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

# Suppression d'un cloud privé virtuel à l'aide de la console IBM Cloud
{: #deleting-using-console}

Pour pouvoir supprimer un cloud privé virtuel (VPC) dans la console IBM Cloud, vous devez supprimer toutes les passerelles publiques du VPC, les sous-réseaux et les ressources connectées.

Pour supprimer un VPC via la console :

1. Recherchez tous les sous-réseaux figurant dans le VPC.  Cliquez sur **VPC** dans le panneau de navigation et sélectionnez votre cloud privé virtuel. 
2. Sur la page des détails du VPC, accédez à la liste des **sous-réseaux de ce VPC** et sélectionnez un sous-réseau pour en afficher les détails.
3. Supprimez toutes les ressources connectées à ce sous-réseau. Dans le panneau de navigation, cliquez sur **Ressources connectées**. Pour chaque instance, équilibreur de charge ou passerelle VPN connectés, sélectionnez la ressource pour accéder à la page des détails correspondants. Sur la page des détails de chaque ressource, cliquez sur l'icône de suppression. 

  Bien que le statut de la ressource passe immédiatement à **Suppression**, l'opération de suppression peut prendre jusqu'à 30 minutes. Le sous-réseau ne peut pas être supprimé tant qu'il y a encore des ressources connectées affichées dans la console.
  {: tip}

4. Une fois que toutes les ressources connectées sont supprimées, revenez à la page des détails du sous-réseau et cliquez sur l'icône de suppression.
5. Dès que vous avez supprimé tous les sous-réseaux, revenez à la page des détails du VPC et cliquez sur l'icône de suppression. Le VPC et toutes les passerelles publiques éventuelles sont supprimés.
