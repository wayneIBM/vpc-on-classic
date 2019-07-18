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

# VPC mithilfe der IBM Cloud-Konsole löschen
{: #deleting-using-console}

Damit Sie eine VPC in der IBM Cloud-Konsole löschen können, müssen Sie vorher alle öffentlichen Gateways, Teilnetze und zugeordneten Ressourcen der VPC löschen.

Gehen Sie wie folgt vor, um eine VPC mithilfe der Konsole zu löschen:

1. Suchen Sie alle Teilnetze in der VPC. Klicken Sie im Navigationsfenster auf **VPCs** und wählen Sie Ihre VPC aus. 
2. Wechseln Sie auf der Detailseite der VPC zur Liste **Teilnetze in dieser VPC** und wählen Sie ein Teilnetz aus, dessen Details Sie anzeigen möchten.
3. Löschen Sie alle Ressourcen, die dem Teilnetz zugeordnet sind. Klicken Sie im Navigationsfenster auf **Zugeordnete Ressourcen**. Wählen Sie für jede einzelne zugeordnete Instanz, zugeordnete Lastausgleichsfunktion bzw. zugeordnetes VPN-Gateways die Ressource aus, um die zugehörige Detailseite aufzurufen. Klicken Sie auf der Detailseite für jede Ressource auf das Löschsymbol. 

  Der Status der Ressource wechselt danach zwar sofort zu **Deleting**, es kann jedoch bis zu 30 Minuten dauern, bis die Löschoperation abgeschlossen ist. Das Teilnetz kann erst gelöscht werden, wenn alle zugeordneten Ressourcen nicht mehr in der Konsole angezeigt werden.
  {: tip}

4. Wechseln Sie nach dem Löschen aller zugeordneten Ressourcen zurück zur Detailseite des Teilnetzes und klicken Sie auf das Löschsymbol. 
5. Wechseln Sie nach dem Löschen aller Teilnetze zurück zur Detailseite für die VPC und klicken Sie auf das Löschsymbol. Die VPC und alle öffentlichen Gateways werden gelöscht.
