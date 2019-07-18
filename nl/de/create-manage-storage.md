---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# Blockspeicher in VPC erstellen und verwalten
{: #creating-and-managing-storage-in-vpc}

Von {{site.data.keyword.cloud}} Virtual Private Cloud wird ein Bootspeicherdatenträger bereitgestellt; weitere Blockspeicherdatenträger, die an einen Hypervisor angehängt sind und leistungsfähige Datenspeicher für virtuelle Serverinstanzen in einer VPC darstellen, sind zulässig. Die VPC-Infrastruktur ermöglicht eine schnelle Skalierung des Speichers für mehrere Regionen und Zonen, was zusätzliche Leistung und Sicherheit bedeutet. 

## Zusammenfassung der Merkmale
{: #summary-of-characteristics}

Wenn Sie eine {{site.data.keyword.vsi_is_full}}-Instanz bereitstellen, wird automatisch ein 100 GB großer Blockspeicherdatenträger für allgemeine IOPS-Workloads (3 IOPS/GB) als primärer Bootdatenträger erstellt und der virtuellen Serverinstanz zugeordnet. Während der Bereitstellung können Sie den Bootdatenträger umbenennen und die Datenträgergröße ändern.{:shortdesc}

* Sie können Blockspeicherdatenträger erstellen, wenn Sie eine Instanz eines virtuellen Servers (VSI) in einem VPC-Netz bereitstellen.   
* Sie können unabhängig vom Lebenszyklus der VSI neue Datenträger erstellen und diese Datenträger später einer VSI zuordnen. 
* Sie können sekundäre Datenträger erstellen, die automatisch der VSI zugeordnet werden.  
* Die Datenträger bleiben bestehen, wenn die Zuordnung zur VSI aufgehoben wird. Daher können Sie einen Datenträger zu einem späteren Zeitpunkt einer neuen Instanz zuordnen.  
* Sie können angeben, ob die Datendatenträger automatisch gelöscht werden, wenn Sie eine VSI löschen.  
* Bootdatenträger werden gelöscht, wenn ihre VSI gelöscht wird.
* Bootdatenträger und Datenträger für Daten werden standardmäßig mit der von IBM verwalteten Verschlüsselung verschlüsselt. 
* Sie können die Datenträger mit eigenen Verschlüsselungsschlüsseln während der VSI-Bereitstellung oder beim Erstellen eines eigenständigen Datenträgers verschlüsseln.


## Blockspeicherdatenträger
{: #block-storage-volumes}

Umfassendere Informationen zur Arbeit mit Blockspeicherdatenträgern und VPC finden Sie in der [Dokumentation zu Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started).

Übersichtsinformationen zu Blockspeicherdatenträgern für VPC finden Sie unter [Informationen zu Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

Eine Einführung in das Erstellen von Datenträgern unabhängig von der VSI-Bereitstellung finden Sie unter [Einführung in {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started).



## Bewährte Verfahren zum Erstellen und Benennen von VPC-Blockspeicherdatenträgern:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Ermitteln  Sie den Speicherbedarf, bevor Sie einen Datenträger bereitstellen. Stellen Sie eine angemessene Kapazität und IOPS-Leistung sicher. 
* Entscheiden Sie, ob ein vordefiniertes IOPS-Profil Ihren Anforderungen an Kapazität und Leistung am besten entspricht oder ob das Angeben angepasster Einstellungen die bessere Vorgehensweise ist.
* Legen Sie fest, ob während der Bereitstellung der virtuellen Serverinstanz sekundäre Datendatenträger erstellt werden sollen. Diese Datenträger werden der Instanz automatisch zugeordnet. 
* Legen Sie fest, ob ein Datenträger, der einer VSI zugeordnet ist, automatisch gelöscht werden soll, wenn Sie die VSI löschen. 
* Wenn Sie Datenträger mit einem eigenen Verschlüsselungsschlüssel verschlüsseln, stellen Sie sicher, dass der Schlüssel gültig ist, dass Sie über die Berechtigung für die Verwendung des Schlüssels verfügen und dass er in der aktuellen Region verwendet werden kann.
* Stellen Sie sicher, dass Sie ALLE Ihre Datenträger während der Bereitstellung benennen, einschließlich den Bootdatenträger.
* Stellen Sie sicher, dass Sie zum Zeitpunkt der Zuordnung ALLE  Datenträgerzuordnungen benennen.
* Jeder Datenträger muss einen eindeutigen Namen innerhalb einer Region innerhalb eines Kontos haben.

Sie können den Namen eines Datenträgers erneut verwenden, nachdem dieser Datenträger gelöscht wurde. Es ist jedoch zu beachten, dass die Wiederverwendung eines Datenträgernamens zu Verwechslungen bei der Abrechnung führen kann.
{:note}
