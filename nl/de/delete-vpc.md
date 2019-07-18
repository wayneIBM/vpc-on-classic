---

copyright:
  years: 2019
lastupdated: "2019-05-29"


keywords: vpc, delete, resources

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

# VPC löschen
{: #deleting}

Eine {{site.data.keyword.cloud}} Virtual Private Cloud-Ressource kann nicht gelöscht werden, wenn sie weitere Ressourcen enthält oder wenn sie einer anderen Ressource zugeordnet ist. In diesem Dokument werden die Beziehungen zwischen VPC-Ressourcen beschrieben; es enthält auch Informationen zu den bewährten Verfahren zum Löschen einer VPC.

In der folgenden Tabelle werden die Typen der VPC-Ressourcen und die Beziehungen zusammengefasst, die beim Löschen dieser Ressourcen berücksichtigt werden sollen.

| Ressource | Vor dem Löschen... | Nach dem Löschen... |
| ---------------- | ----------------------------------------- | --------------------------- |
| VPC | Alle Teilnetze und öffentlichen Gateways in der VPC müssen gelöscht werden | Alle Sicherheitsgruppen und Adresspräfixe werden automatisch gelöscht |
| Teilnetz | Alle Instanzen und Netzschnittstellen im Teilnetz müssen gelöscht werden; alle VPN-Gateways und Lastausgleichsfunktionen im Teilnetz müssen gelöscht werden. | Die Zuordnung aller öffentlichen Gateways, die dem Teilnetz bereitgestellt werden, wird aufgehoben; die Zuordnung aller Netz-ACLs, die dem Teilnetz zugeordnet sind, wird aufgehoben |
| Instanz | ---- | Alle Netzschnittstellen werden automatisch gelöscht und der Bootdatenträger wird mit der Instanz gelöscht; das Löschen des sekundären Datenträgers hängt vom Parameter `delete_volume_on_instance_delete` ab; die Zuordnung der variablen IP-Adressen, die einer der Netzschnittstellen zugeordnet sind, wird automatisch aufgehoben |
| Netzschnittstelle | ---- | Alle variablen IP-Adressen, die der Netzschnittstelle zugeordnet sind, werden freigegeben |
| Schlüssel | ---- | Nachdem Sie einen Schlüssel gelöscht haben, kann er nicht mehr für die Bereitstellung einer neuen Instanz oder zum erneuten Laden eines Betriebssystems für eine vorhandene Instanz verwendet werden. Der Schlüssel ist jedoch noch immer für alle Instanzen verfügbar, die Sie mit ihm bereitgestellt haben; somit können Sie ihn weiterhin zum Anmelden verwenden. |
| Image | ---- | Das Image kann nicht zum Bereitstellen einer neuen Instanz verwendet werden, vorhandene Instanzen mit Image sind hiervon jedoch nicht betroffen. |
| Datenträger | Die Zuordnung des Datenträgers zu allen Instanzen muss aufgehoben werden | ---- |
| Netz-ACL | Die Zuordnung einer Netz-ACL muss zu allen Teilnetzen aufgehoben werden; Standardnetz-ACLs für eine VPC können nicht gelöscht werden | ---- |
| Sicherheitsgruppe | Die Zuordnung einer Sicherheitsgruppe muss zu allen Netzschnittstellen aufgehoben werden; die Standardsicherheitsgruppe für eine VPC kann nicht gelöscht werden. | ---- |
| Variable IP-Adresse | Die Zuordnung einer variablen IP-Adresse muss zu allen Netzschnittstelle aufgehoben werden. | ---- |
| Öffentliches Gateway | Die Zuordnung eines öffentlichen Gateways muss zu allen Teilnetzen aufgehoben werden |  ---- |
| Lastausgleichsfunktion | ---- | ---- |
| VPN-Gateway | ---- | Aufgrund der Möglichkeit, IKE- und IPsec-Richtlinien zwischen Gateways gemeinsam zu nutzen, werden diese nicht gelöscht, wenn ein VPN-Gateway gelöscht wird. IKE- und IPsec-Richtlinien müssen manuell entfernt werden. |


## Löschen von VPC-Ressourcen in transientem Status nicht möglich
{: #deleting-status}

VPC-Ressourcen können nicht gelöscht werden, wenn sie sich in einem transienten Status befinden, zum Beispiel `pending`. Alle Ressourcen müssen sich in einem "endgültigen" Status befinden, wie zum Beispiel `available` oder `failed`, damit sie gelöscht werden können. Sie müssen warten, bis der Status der Ressource zu `available` oder `failed` wechselt; danach können Sie versuchen, die Ressource zu löschen. [Wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support), wenn für eine Ressource innerhalb von 30 Minuten nicht der Status `available` oder `failed` angezeigt wird. 

Sobald Sie die Anforderung zum Löschen einer Ressource ausgegeben haben, wechselt die Ressource in den transienten Status `deleting`. Warten Sie, bis die Ressource nicht mehr in der Liste angezeigt wird, bevor Sie versuchen, eine übergeordnete oder zugeordnete Ressource zu löschen. In manchen Situationen kann die Löschoperation fehlschlagen; die Ressource wechselt in diesem Fall innerhalb weniger Minuten in den Status `failed`. Wenden Sie sich an den  Support, wenn die Ressource nicht ausgeblendet wird oder wenn ihr Status innerhalb von 30 Minuten nach dem Ausgeben der Löschanforderung nicht `failed` lautet. Sobald sich die Ressource im Status `failed` befindet, können Sie versuchen, sie wieder zu löschen. [Wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support), wenn Sie eine Ressource im Status `failed` nicht löschen können. 

## Reihenfolge für das Löschen einer VPC
{: #deleting-order}

Eine VPC enthält Teilnetze und öffentliche Gateways. Ein öffentliches Gateway kann mindestens einem Teilnetz in der VPC zugeordnet werden. Im Teilnetz einer VPC sind eine Lastausgleichsfunktion und ein VPN-Gateway vorhanden. Ein Teilnetz kann auch virtuelle Serverinstanzen (VSIs) enthalten, und eine VSI kann über mehrere Netzschnittstellen in verschiedenen Teilnetzen innerhalb der VPC verfügen. Damit ein Teilnetz gelöscht werden kann, müssen vorher alle VPN-Gateways, Lastausgleichsfunktionen und Netzschnittstellen im Teilnetz gelöscht werden. Eine VPC enthält auch Sicherheitsgruppen; diese werden jedoch automatisch gelöscht, wenn die VPC gelöscht wird. Adresspräfixe sind Attribute einer VPC und werden automatisch gelöscht, wenn die VPC gelöscht wird.

Beachten Sie beim Löschen einer VPC die folgende Reihenfolge: 

1.  Löschen Sie alle VPN-Gateways, die in einem Teilnetz der VPC bereitgestellt werden.
2.  Heben Sie die Zuordnung aller Lastausgleichsfunktionen auf, die in einem Teilnetz der VPC bereitgestellt werden.
3.  Löschen Sie alle Instanzen, die über Netzschnittstellen in einem Teilnetz der VPC verfügen. Die Zuordnung aller variablen IP-Adressen, die einer Netzschnittstelle zugeordnet sind, wird automatisch aufgehoben, wenn die Instanz gelöscht wird. 
4.  Löschen Sie alle Teilnetze in der VPC. Die Zuordnung aller öffentlichen Gateways, die einem Teilnetz zugeordnet sind, wird automatisch aufgehoben, wenn das Teilnetz gelöscht wird.
5.  Löschen Sie alle öffentlichen Gateways in der VPC.
6.  Sobald die VPC nicht mehr über Teilnetze und öffentliche Gateways verfügt, kann die VPC gelöscht werden. Alle Adresspräfixe in der VPC werden automatisch gelöscht, wenn die VPC gelöscht wird.

## Beziehungen einer VPC-Ressource
{: #deleting-relationships}

Manche VPC-Ressourcen sind in anderen VPC-Ressourcen (als 'untergeordnete Ressourcen') enthalten, andere VPC-Ressourcen befinden sich jedoch auf der Kontoebene und somit außerhalb einer konkreten VPC. Damit eine übergeordnete Ressource gelöscht werden kann, müssen vorher alle untergeordneten VPC-Ressourcen gelöscht werden. Für VPC-Ressourcen auf Kontoebene müssen alle Links zu vorhandenen Ressourcen gelöscht werden, damit die Kontoressource gelöscht werden kann.

In einigen Fällen wird durch das Löschen einer VPC-Ressource automatisch die Verknüpfung zu den vorhandenen Ressourcen gelöscht. In den folgenden Tabellen wird das erwartete Verhalten beschrieben.

### VPC
{: #deleting-details}

VPCs enthalten Teilnetze und öffentliche Gateways. In einem Teilnetz werden Lastausgleichsfunktionen, VPN-Gateways und Instanzen bereitgestellt. Damit eine VPC gelöscht werden kann, müssen vorher alle Teilnetze und öffentlichen Gateways in der VPC gelöscht werden. Somit müssen zuerst alle öffentlichen Gateways, Lastausgleichsfunktionen, VPN-Gateways und Instanzen in der VPC gelöscht werden. 

Eine VPC enthält auch Adresspräfixe und Sicherheitsgruppen; diese Ressourcen werden jedoch automatisch gelöscht, wenn die VPC gelöscht wird.

Eine VPC muss über eine Standardsicherheitsgruppe und eine Standardnetz-ACL verfügen. Wenn Sie eine VPC ohne Angabe einer Standardsicherheitsgruppe und Standardnetz-ACL erstellen, werden automatisch eine Standardsicherheitsgruppe und eine Standardnetz-ACL für die VPC erstellt. Die Standardsicherheitsgruppe und die Standardnetz-ACL können nicht gelöscht werden. Die Standardnetz-ACL wird automatisch gelöscht, wenn die VPC gelöscht wird. Alle Sicherheitsgruppen in der VPC werden automatisch gelöscht, wenn die VPC gelöscht wird.

| In VPC | Kann enthalten | Kann zugeordnet sein zu | Status? | Wird beim Löschen der VPC automatisch gelöscht | Zuordnung wird beim Löschen automatisch aufgehoben |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Teilnetz | Instanzen, Lastausgleichsfunktionen, VPN-Gateways | Öffentliches Gateway, Netz-ACL | Ja | Nein | Ja |
| Öffentliches Gateway | --- | Teilnetze, variable IP-Adressen| Ja | Nein | Nein für Teilnetze, ja für variable IP-Adressen |
| Sicherheitsgruppen | ---  | Instanzen (NIC), VPC als Standard | Nein | Ja | Nein |

### Teilnetz
{: #deleting-subnet}

Alle Ressourcen in einem Teilnetz müssen gelöscht werden, damit das Teilnetz gelöscht werden kann. In den Teilnetzen sind die Netzschnittstellen, die Lastausgleichsfunktionen und die VPN-Gateways einer Instanz enthalten. Die Netzschnittstelle, die dem Teilnetz zugeordnet ist, muss zusammen mit allen im Teilnetz bereitgestellten Lastausgleichsfunktionen oder VPN-Gateways gelöscht werden, damit das Teilnetz gelöscht werden kann. 

Eine Instanz kann über mehrere Netzschnittstellen verfügen und diese Netzschnittstellen können zu mehreren Teilnetzen der VPC gehören. Damit ein Teilnetz gelöscht werden kann, müssen vorher alle Netzschnittstellen im Teilnetz gelöscht werden. Da die primäre Netzschnittstelle einer Instanz nicht gelöscht oder in ein anderes Teilnetz verschoben werden kann, müssen alle Instanzen mit einer primären Netzschnittstelle gelöscht werden, bevor das Teilnetz gelöscht wird. 


| Im Teilnetz | Kann enthalten | Kann zugeordnet sein zu | Status? | Wird beim Löschen des Teilnetzes automatisch gelöscht | Zuordnung wird beim Löschen automatisch aufgehoben |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Instanz (Netzschnittstelle) | Mehrere Netzschnittstellen | Datenträgerzuordnungen, Sicherheitsgruppen | Ja | Nein | Ja |
| VPN | --- | ---| Ja | Nein | --- |
| Lastausgleichsfunktion | ---  | --- | Ja | Nein | ---  |

### Instanz
{: #deleting-instance}

Für das Löschen einer Instanz müssen keine Voraussetzungen erfüllt sein. Wenn eine Instanz gelöscht wird, werden alle zugehörigen Netzschnittstellen automatisch gelöscht. Der Bootdatenträger der Instanz und alle Datenträgerzuordnungen werden gelöscht. Alle variablen IP-Adressen, die einer ihrer Netzschnittstellen zugeordnet sind, werden automatisch freigegeben. Alle Blockspeicherdatenträger, die der Instanz zugeordnet sind, werden automatisch gelöscht, wenn während der Erstellung des Datenträgers für das Flag `delete_volume_on_instance_delete` der Wert 'true' festgelegt war; andernfalls wird die Zuordnung der Instanz zum Datenträger aufgehoben, aber der Datenträger bleibt bestehen. Wenn eine Sicherheitsgruppe einer Netzschnittstelle der Instanz zugeordnet ist, wird die Zuordnung der Sicherheitsgruppen ebenfalls automatisch aufgehoben, wenn die Netzschnittstellen gelöscht werden.

| In Instanz | Kann enthalten | Kann zugeordnet sein zu | Status? | Wird beim Löschen der Instanz automatisch gelöscht | Zuordnung wird beim Löschen automatisch aufgehoben |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Netzschnittstelle | --- | Teilnetze, variable IP-Adressen, Sicherheitsgruppen | Nein | Ja | Ja |

### Lastausgleichsfunktion
{: #deleting-lb}

Für das Löschen einer Lastausgleichsfunktion müssen keine Voraussetzungen erfüllt sein. Wenn die Lastausgleichsfunktion gelöscht wird, werden automatisch alle Listener, Pools und Poolmitglieder gelöscht, die Bestandteil der Lastausgleichsfunktion sind. 

Das Löschen einer Lastausgleichsfunktion kann bis zu 30 Minuten dauern. Durch die Löschanforderung wechselt der Bereitstellungsstatus der Lastausgleichsfunktion unverzüglich zu `deleting`. Sie ist jedoch erst gelöscht, wenn die Lastausgleichsfunktion in der Listenabfrage nicht mehr angezeigt wird.
{: important}

### VPN
{: #deleting-vpn}

Für das Löschen eines VPN-Gateways müssen keine Voraussetzungen erfüllt sein. Wenn das VPN-Gateway gelöscht wird, werden auch seine zugeordneten Verbindungen automatisch gelöscht. Die IKE-Richtlinien und die IPSec-Richtlinien werden nicht gelöscht, wenn ein VPN-Gateway gelöscht wird.

Das Löschen eines VPN-Gateways kann bis zu 30 Minuten dauern. Durch die Löschanforderung wechselt der Bereitstellungsstatus des VPN-Gateways unverzüglich zu `deleting`. Es ist jedoch erst gelöscht, wenn der VPN-Gateway in der Listenabfrage nicht mehr angezeigt wird.
{: important}

### Variable IP-Adresse
{: #deleting-floating-ip}

Eine variable IP-Adresse ist außerhalb einer VPC auf Kontoebene vorhanden. Wenn die variable IP-Adresse an ein öffentliches Gateway oder eine Instanz gebunden ist, muss die Verknüpfung gelöscht (freigegeben) werden, damit die variable IP-Adresse gelöscht werden kann. 

Wenn Sie eine Ressource löschen, an die die variable IP-Adresse gebunden ist, zum Beispiel beim Löschen einer Netzschnittstelle einer Instanz, wird die variable IP-Adresse automatisch freigegeben.

### Datenträger
{: #deleting-volume}

In einer VPC können zwei Datenträgertypen vorhanden sein: ein Blockspeicherdatenträger zum Booten und ein Blockspeicherdatenträger für Daten. Diese Datenträger werden unterschiedlich gelöscht.

**Bootdatenträger löschen**

Bootdatenträger sind in einer Instanz vorhanden und werden nicht als separate Entitäten dieser Instanz betrachtet. Wenn Sie eine Instanz erstellen, wird auch ein Bootdatenträger erstellt und der Instanz zugeordnet. Der Bootdatenträger wird automatisch gelöscht, wenn Sie die Instanz löschen. Sie können den Bootdatenträger nicht löschen, ohne die Instanz zu löschen.

**Datenträger für Daten löschen**

Blockspeicherdatenträger für Daten können separat von den zugehörigen virtuellen Serverinstanzen bereitgestellt und verwaltet werden. Einen Datenträger für Daten können Sie einer virtuellen Serverinstanz als sekundären Speicher zuordnen. Sie können einen Datenträger für Daten nicht löschen, der einer Instanz zugeordnet ist (das bedeutet, dass der Datenträger 'aktiv' ist). Sie müssen zuerst die Zuordnung des Datenträger zur Instanz aufheben, danach können Sie den Datenträger löschen. 

Ein Datenträger für Daten verfügt über ein Attribut (oder Flag) mit der Bezeichnung `delete_volume_on_instance_delete` in der API und `Auto Delete` in der Befehlszeilenschnittstelle und Benutzerschnittstelle. Falls für dieses Flag `true` (`Enabled` in der Benutzerschnittstelle) festgelegt ist, wenn die Instanz mit dem zugeordneten Datenträger gelöscht wird, wird automatische die Zuordnung des Datenträgers aufgehoben und er wird automatisch gelöscht. Wenn das Flag für den Datenträger den Wert `false` (`Disabled` in der Benutzerschnittstelle) aufweist, wird zwar die Zuordnung zum Datenträger aufgehoben, aber der Datenträger wird nicht gelöscht, wenn die zugeordnete Instanz gelöscht wird. Der Datenträger kann einer anderen Instanz zugeordnet werden. 

Ein Blockspeicherdatenträger kann jeweils nur einem virtuellen Server zugeordnet werden.
{: tip}

### Sicherheitsgruppen
{: #deleting-secgroup}

Eine Sicherheitsgruppe kann nicht gelöscht werden, wenn sie von einer Netzschnittstelle verwendet wird oder die Standardsicherheitsgruppe einer VPC ist. Entfernen Sie vor dem Löschen einer Sicherheitsgruppe alle Netzschnittstellen aus der Sicherheitsgruppe und stellen Sie sicher, dass die Sicherheitsgruppe nicht als Standardsicherheitsgruppe der VPC verwendet wird. 

Wenn eine VPC gelöscht wird, werden automatisch alle Sicherheitsgruppen in dieser VPC gelöscht. 

### Netz-ACLs
{: #deleting-netacl}

Eine Netz-ACL kann nicht gelöscht werden, wenn sie von einem Teilnetz verwendet wird oder die Standardnetz-ACL einer VPC ist. Heben Sie vor dem Löschen einer Netz-ACL die Zuordnung der Netz-ACL zu allen Teilnetzen auf und stellen Sie sicher, dass die Netz-ACL nicht als Standardnetz-ACL der VPC verwendet wird. 

Wenn eine VPC erstellt wird, ist eine Standardnetz-ACL erforderlich. Wenn eine vorhandene Netz-ACL beim Erstellen der VPC nicht als Standard angegeben wird, wird eine neue Netz-ACL erstellt und als Standard festgelegt. Diese Standardnetz-ACL wird automatisch gelöscht, wenn die VPC gelöscht wird, falls die ACL nicht anderweitig verwendet wird.

Im Gegensatz zu Sicherheitsgruppen können Netz-ACLs verschiedenen VPCs zugeordnet werden. Daher werden beim Löschen einer VPC nicht die Netz-ACLs gelöscht.
{: note}

## Nächste Schritte
{: #deleting-nextsteps}

In den folgenden Abschnitten finden Sie weitere Beispiele zum Löschen von VPC-Ressourcen mithilfe der IBM Cloud-Konsole, der Befehlszeilenschnittstelle oder der API.

-   [VPC-Ressourcen mithilfe der Benutzerschnittstelle löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-console)
-   [VPC-Ressourcen mithilfe der Befehlszeilenschnittstelle löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)
-   [VPC-Ressourcen mithilfe der API löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-api)
