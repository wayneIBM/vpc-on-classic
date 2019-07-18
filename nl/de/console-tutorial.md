---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: create, configure, permissions, ACL, virtual, server, instance, subnet, block, storage, volume, security, group, images, Windows, Linux, example, monitoring, VPN, load balancer, IKE, IPsec

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

# VPC über die {{site.data.keyword.cloud_notm}}-Konsole erstellen
{: #creating-a-vpc-using-the-ibm-cloud-console}
[comment]: # (Verknüpfter Hilfeabschnitt)

In diesem Dokument erfahren Sie, wie Sie eine Instanz von {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) über die {{site.data.keyword.cloud_notm}}-Konsole erstellen und konfigurieren.

Um eine VPC-Instanz und weitere zugeordnete Ressourcen zu erstellen und zu konfigurieren, führen Sie die Schritte in den nachfolgenden Abschnitten in der folgenden Reihenfolge aus:

1. Erstellen Sie eine VPC und ein Teilnetz, um das Netz zu definieren. Wenn Sie ein Teilnetz erstellen, ordnen Sie ein öffentliches Gateway zu, damit von allen Ressourcen im Teilnetz mit dem öffentlichen Internet kommuniziert werden kann. 
1. Konfigurieren Sie eine Zugriffssteuerungsliste (Access Control List, ACL), um den eingehenden und abgehenden Datenverkehr des Teilnetzes zu begrenzen. Standardmäßig ist der gesamte Datenverkehr zulässig.
1. Erstellen Sie eine Instanz eines virtuellen Servers.
1. Erstellen Sie einen Blockspeicherdatenträger und ordnen Sie ihm eine Instanz zu.
1. Konfigurieren Sie eine Sicherheitsgruppe, um den eingehenden und abgehenden Datenverkehr zu definieren, der für die Instanz zulässig ist.
1. Reservieren Sie eine variable IP-Adresse und ordnen Sie sie zu, damit Ihre Instanz vom Internet aus erreichbar ist.
1. Erstellen Sie eine Lastausgleichsfunktion, um Anforderungen über mehrere Instanzen zu verteilen.
1. Erstellen Sie ein Virtual Private Network (VPN), damit Ihre VPC eine sichere Verbindung mit einem anderen Netz, wie Ihrem lokalen Netz und einer anderen VPC, herstellen kann.

## Vorbereitende Schritte
{: #before}
Stellen Sie sicher, dass Sie über ausreichende Berechtigungen zum Erstellen und Verwalten von Ressourcen in der VPC verfügen. Weitere Informationen hierzu finden Sie im Abschnitt [Benutzerberechtigungen für VPC-Ressourcen verwalten](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Generieren Sie einen SSH-Schlüssel, der verwendet wird, um eine Verbindung zur virtuellen Serverinstanz herzustellen. Generieren Sie z. B. einen SSH-Schlüssel auf Ihrem Linux-Server, indem Sie den Befehl `ssh-keygen -t rsa -C "user_ID"` ausführen. Dieser Befehl generiert zwei Dateien. Der generierte öffentliche Schlüssel befindet sich in der Datei `<your key>.pub`.

Wenn Sie eine Lastausgleichsfunktion erstellen und HTTPS für den Listener verwenden möchten, ist ein SSL-Zertifikat erforderlich. Sie können Zertifikate mit dem [IBM Zertifikatsmanager ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/services/certificate-manager){: new_window} verwalten. Sie müssen auch eine Berechtigung erstellen, damit Ihre Instanz der Lastausgleichsfunktion auf die Zertifikatsmanagerinstanz zugreifen kann, die das SSL-Zertifikat enthält. Sie können eine Berechtigung über [Identitäts- und Zugriffsberechtigungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/iam/#/authorizations){: new_window} erstellen. Wählen Sie für die Quelle **VPC-Infrastruktur** als Quellenservice, **Lastausgleichsfunktion für VPC** als Ressourcentyp und **Alle Ressourceninstanzen** für die Quellenressourceninstanz aus. Wählen Sie **Zertifikatsmanager** als Zielservice aus und ordnen Sie **Autor** für die Servicezugriffsrolle zu. Legen Sie die Zielserviceinstanz auf **Alle Instanzen** oder auf die Zertifikatsmanagerinstanz fest. Weitere Informationen finden Sie im Abschnitt [Lastausgleichsfunktionen in IBM Cloud VPC verwenden](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).

## VPC und Teilnetz erstellen

Führen Sie zum Erstellen einer VPC und eines Teilnetzes die folgenden Schritte aus:

1. Öffnen Sie die [{{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window}.
1. Klicken Sie auf das **Menüsymbol ![Menüsymbol](../../icons/icon_hamburger.svg) > VPC-Infrastruktur > Netz > VPCs ** und klicken Sie auf **Neue virtuelle private Cloud**.
1. Geben Sie einen Namen für die VPC ein, beispielsweise `my-vpc`.
1. Wählen Sie eine Ressourcengruppe für die VPC und alle zugehörigen Ressourcen aus. Mit Ressourcengruppen können Sie Ihre Kontoressourcen für die Zugriffssteuerung und die Rechnungsstellung organisieren. Weitere Informationen finden Sie im Abschnitt [Bewährte Verfahren für die Organisation von Ressourcen in einer Ressourcengruppe](/docs/resources?topic=resources-bp_resourcegroups).
1. _Optional:_ Geben Sie Tags ein, mit denen Sie Ihre Ressourcen organisieren und suchen können. Sie können später weitere Tags hinzufügen. Weitere Informationen hierzu finden Sie im Abschnitt [Mit Tags arbeiten](/docs/resources?topic=resources-tag).
1. Wählen Sie die Standard-ACL für neue Teilnetze in dieser VPC aus oder erstellen Sie sie. In diesem Lernprogramm erstellen Sie eine neue Standard-ACL. Die Regeln für die Zugriffssteuerungsliste (ACL-Regeln) werden später konfiguriert.
1. Wählen Sie aus, ob die Standardsicherheitsgruppe eingehenden SSH- und Ping-Datenverkehr an virtuelle Serverinstanzen in dieser VPC zulässt. Sie konfigurieren dann zu einem späteren Zeitpunkt weitere Regeln für die Standardsicherheitsgruppe.
1. Geben Sie einen Namen für das neue Teilnetz in der VPC ein, beispielsweise `my-subnet`.
1. Wählen Sie einen Standort für das Teilnetz aus. Der Ort besteht aus einer Region und einer Zone.

    Die von Ihnen ausgewählte Region wird als Region der VPC verwendet. Alle zusätzlichen Ressourcen, die Sie in dieser VPC erstellen, werden in der ausgewählten Region erstellt.
    {: tip}

1. Geben Sie einen IP-Bereich für das Teilnetz in der CIDR-Notation ein, z. B. `10.240.0.0/24`. In den meisten Fällen können Sie den IP-Standardbereich verwenden. Wenn Sie einen angepassten IP-Bereich angeben möchten, können Sie den IP-Bereichsberechner verwenden, um ein anderes Adresspräfix auszuwählen oder die Anzahl der Adressen zu ändern.
1. Wählen Sie eine ACL für das Teilnetz aus. Wählen Sie **VPC-Standard verwenden** aus, um die Standard-ACL zu verwenden, die für diese VPC erstellt wurde.
1. Hängen Sie ein öffentliches Gateway an das Teilnetz an, damit alle zugeordneten Ressourcen mit dem öffentlichen Internet kommunizieren können.  

    Sie können das öffentliche Gateway auch nach der Erstellung des Teilnetzes anhängen.
    {: tip}

1. Klicken Sie auf **Virtuelle private Cloud erstellen**.
1. Wenn Sie ein weiteres Teilnetz in dieser VPC erstellen möchten, klicken Sie auf die Registerkarte **Teilnetze** und dann auf  **Neues Teilnetz**. Wenn Sie das Teilnetz definieren, stellen Sie sicher, dass Sie `my_vpc` im Feld **Virtuelle private Cloud** auswählen.

## ACL konfigurieren

Sie können die ACL konfigurieren, um den eingehenden und abgehenden Datenverkehr für das Teilnetz zu begrenzen. Standardmäßig ist der gesamte Datenverkehr zulässig.

Jedes Teilnetz kann nur einer ACL zugeordnet werden. Jede ACL kann jedoch mehreren Teilnetzen zugeordnet werden.

Führen Sie zum Konfigurieren der ACL die folgenden Schritte aus:

1. Klicken Sie im Navigationsfenster auf **Netz > Teilnetze**.
1. Klicken Sie auf das Teilnetz, das Sie erstellt haben.
1. Klicken Sie im Bereich **Teilnetzdetails** auf den Namen der ACL.
1. Klicken Sie auf **Regel hinzufügen**, um die Regeln für eingehende und abgehende Daten zu konfigurieren, die definieren, welcher Datenverkehr für das Teilnetz zulässig ist. Geben Sie für jede Regel die folgenden Informationen an:  
   * Geben Sie die Priorität der Regel an. Regeln mit niedrigeren Zahlen werden als Erstes bewertet und überschreiben Regeln mit höheren Zahlen. Wenn z. B. eine Regel mit Priorität 2 den HTTP-Datenverkehr zulässt und eine Regel mit Priorität 5 den gesamten Datenverkehr verweigert, ist der HTTP-Datenverkehr weiterhin zulässig.  
   * Wählen Sie aus, ob der angegebene Datenverkehr zugelassen oder verweigert werden soll.  
   * Geben Sie einen CIDR-Block an, um den IP-Bereich anzugeben, für den die Regel gilt.
   * Wählen Sie die Protokolle und Ports aus, für die die Regel gilt.
1. Wenn Sie die Erstellung von Regeln abgeschlossen haben, klicken Sie oben auf der Seite auf die Breadcrumb **Alle Zugriffssteuerungslisten**.

### Beispiel-ACL

Sie können beispielsweise Regeln für eingehende Daten konfigurieren, die Folgendes festlegen:

 1. HTTP-Datenverkehr über das Internet zulassen
 1. Allen eingehenden Datenverkehr vom Teilnetz 10.10.20.0/24 zulassen
 1. Allen anderen eingehenden Datenverkehr zurückweisen  

Konfigurieren Sie anschließend abgehende Regeln, die Folgendes festlegen:

1. HTTP-Datenverkehr im Internet zulassen
1. Allen abgehenden Datenverkehr zum Teilnetz 10.10.20.0/24 zulassen
1. Allen anderen abgehenden Datenverkehr zurückweisen  

![Beispielregeln für eingehende und abgehende Daten](images/acl-rules.png)

## Virtuelle Serverinstanz erstellen

Gehen Sie wie folgt vor, um eine virtuelle Serverinstanz im neu erstellten Teilnetz zu erstellen:

1. Klicken Sie im Navigationsfenster auf **Berechnen > Virtuelle Serverinstanz** und klicken Sie auf **Neue Instanz**.
1. Geben Sie einen Namen für die Instanz ein, z. B. `my-instance`.
1. Wählen Sie die VPC aus, die Sie erstellt haben.
1. Wählen Sie im Feld **Position** die Zone aus, in der die Instanz erstellt werden soll.
1. Wählen Sie ein Image (d. h. Betriebssystem und Version) wie Ubuntu Linux 16.04 aus.
1. Um die Instanzgröße festzulegen, wählen Sie eines der gängigen Profile aus oder klicken Sie auf **Alle Profile**, um eine andere Kern- und RAM-Kombination auszuwählen, die für Ihre Workload am besten geeignet ist.
1. Wählen Sie einen vorhandenen SSH-Schlüssel aus oder fügen Sie einen neuen SSH-Schlüssel hinzu, der für den Zugriff auf die virtuelle Serverinstanz verwendet wird. Klicken Sie zum Hinzufügen eines SSH-Schlüssels auf **Neuer Schlüssel** und geben Sie dem Schlüssel einen Namen. Nachdem Sie den zuvor generierten öffentlichen Schlüsselwert eingegeben haben, klicken Sie auf **SSH-Schlüssel hinzufügen**.

Schlüssel können nur am Anfang im Rahmen der Erstellung der virtuellen Serverinstanz (VSI) hinzugefügt werden. Später stehen keine Tools zum Hinzufügen von Schlüsseln zur Verfügung.
{:tip}

1. _Optional:_ Geben Sie Benutzerdaten ein, um allgemeine Konfigurationstasks auszuführen, wenn die Instanz gestartet wird. Sie können z. B. 'cloud-init'-Anweisungen oder Shell-Scripts für Linux-Images angeben. Weitere Informationen finden Sie in [Benutzerdaten](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-user-data).
1. Notieren Sie den Bootdatenträger. Im aktuellen Release werden 100 GB für den Bootdatenträger zugeordnet. *Automatisch löschen* ist für den Datenträger aktiviert; er wird automatisch gelöscht, wenn die Instanz gelöscht wird.
1. Im Bereich **Zugeordneter Blockspeicherdatenträger** können Sie auf **Neuer Blockspeicherdatenträger** klicken, um der Instanz einen Blockspeicherdatenträger zuzuordnen. In diesem Lernprogramm wird ein Blockspeicherdatenträger erstellt und später der Instanz zugeordnet. 
1. Im Bereich **Netzschnittstellen** können Sie die Netzschnittstelle bearbeiten und den Namen ändern. Wenn in der ausgewählten Zone und der VPC mehr als ein Teilnetz vorhanden ist, können Sie ein anderes Teilnetz an die Schnittstelle anschließen. Wenn die Instanz in mehreren Teilnetzen vorhanden sein soll, können Sie weitere Schnittstellen erstellen.

   Sie können auch auswählen, welche Sicherheitsgruppen an die einzelnen Schnittstellen angehängt werden sollen. Standardmäßig ist die VPC-Standardsicherheitsgruppe angehängt. Die Standardsicherheitsgruppe ermöglicht eingehenden SSH- und Ping-Datenverkehr, den gesamten abgehenden Datenverkehr und den gesamten Datenverkehr zwischen den Instanzen in der Gruppe. Aller anderer Datenverkehr ist blockiert. Sie können Regeln konfigurieren, um mehr Datenverkehr zuzulassen. Wenn Sie später die Regeln der Standardsicherheitsgruppe bearbeiten, gelten diese aktualisierten Regeln für alle aktuellen und zukünftigen Instanzen in der Gruppe.

1. Klicken Sie auf **Virtuelle Serverinstanz erstellen**. Der Status der Instanz beginnt mit *Anstehend*, ändert sich in *Gestoppt* und dann in *Eingeschaltet*. Möglicherweise müssen Sie die Seite aktualisieren, um die Statusänderung anzuzeigen.

## Blockspeicherdatenträger erstellen und zuordnen

Sie können einen Blockspeicherdatenträger erstellen und ihn einer virtuellen Serverinstanz zuordnen.

Gehen Sie wie folgt vor, um einen Blockspeicherdatenträger zu erstellen und zuzuordnen:

1. Klicken Sie im Navigationsbereich auf **Speicher > Blockspeicherdatenträger**.
1. Klicken Sie auf der Seite mit den Blockspeicherdatenträgern für VPC auf **Neuer Datenträger** und geben Sie die folgenden Informationen an.
  * **Name**: Geben Sie einen Namen für den Blockspeicherdatenträger ein, zum Beispiel `data-volume-1`.  
  * **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe für den Blockspeicherdatenträger aus. Mit Ressourcengruppen können Sie Ihre Kontoressourcen für die Zugriffssteuerung und die Rechnungsstellung organisieren. Weitere Informationen finden Sie im Abschnitt [Bewährte Verfahren für die Organisation von Ressourcen in einer Ressourcengruppe](/docs/resources?topic=resources-bp_resourcegroups).
  * **Tags**: _Optional:_ Geben Sie Tags ein, mit denen Sie Ihre Ressourcen organisieren und suchen können. Sie können später weitere Tags hinzufügen. Weitere Informationen hierzu finden Sie im Abschnitt [Mit Tags arbeiten](/docs/resources?topic=resources-tag).
  * **Standort**: Wählen Sie einen Standort für den Blockspeicherdatenträger aus. Der Standort besteht aus einer Region und einer Zone, zum Beispiel 'Vereinigte Staaten (Süden) 1'. 
  * **Größe**: Geben Sie für die Größe des Datenträgers einen Wert zwischen 10 GB und 2000 GB an.
  * **IOPS**: Wählen Sie eines der IOPS-Tiers aus oder klicken Sie auf 'Angepasst' (Custom), um einen IOPS-Wert basierend auf der Datenträgergröße einzugeben.
  * **Verschlüsselung**: Bestätigen Sie die vom Provider verwaltete Standardverschlüsselung oder wählen Sie 'Benutzerverwaltet' aus und verwenden Sie Ihren eigenen Verschlüsselungsschlüssel. Für diesen Schritt ist das Bereitstellen einer Key Protect-Serviceinstanz und das Erstellen oder Importieren eines Rootschlüssel erforderlich. Weitere Informationen finden Sie unter [Blockspeicherdatenträger mit benutzerverwalteter Verschlüsselung erstellen](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption#data-vol-encryption-ui).
1. Klicken Sie auf **Datenträger erstellen**.
1. Suchen Sie in der Liste der Blockspeicherdatenträger den Datenträger, den Sie erstellt haben. Wenn als Status 'Verfügbar' angegeben wird, klicken Sie auf '...' und wählen **Zu Instanz zuordnen** aus. 
1. Wählen Sie die Instanz aus, der Sie den Datenträger zuordnen möchten, und klicken Sie auf **Zuordnen**.

## Sicherheitsgruppe für die Instanz konfigurieren

Sie können die Sicherheitsgruppe konfigurieren, um den eingehenden und abgehenden Datenverkehr zu definieren, der für die Instanz zulässig ist. Nachdem Sie beispielsweise ACL-Regeln für das Teilnetz basierend auf den Sicherheitsrichtlinien Ihres Unternehmens konfiguriert haben, können Sie den Datenverkehr für bestimmte Instanzen abhängig von deren Workloads weiter einschränken.

Führen Sie zum Konfigurieren der Sicherheitsgruppe die folgenden Schritte aus:

1. Klicken Sie auf der Seite mit den virtuellen Serverinstanzen auf Ihre Instanz, um die zugehörigen Details anzuzeigen.
1. Klicken Sie im Abschnitt **Netzschnittstellen** auf die Sicherheitsgruppe.
1. Klicken Sie auf **Regel hinzufügen**, um die Regeln für eingehende und abgehende Daten zu konfigurieren, die definieren, welche Art des Datenverkehrs für die Instanz zulässig ist. Geben Sie für jede Regel die folgenden Informationen an:  
   * Geben Sie einen CIDR-Block oder eine IP-Adresse für den zulässigen Datenverkehr an. Alternativ können Sie eine Sicherheitsgruppe in derselben VPC angeben, um Datenverkehr zu oder von allen Instanzen zuzulassen, die der ausgewählten Sicherheitsgruppe zugeordnet sind.   
   * Wählen Sie die Protokolle und Ports aus, für die die Regel gilt.    

   **Tipps:**  
  * Alle Regeln werden ausgewertet, unabhängig von der Reihenfolge, in der sie hinzugefügt werden.
  * Regeln sind statusabhängig, was bedeutet, dass der Verkehr, der als Antwort auf zulässigen Datenverkehr zurückgegeben wird, automatisch zulässig ist. Eine Regel, die den eingehenden TCP-Datenverkehr an Port 80 zulässt, ermöglicht beispielsweise auch das Zurückführen des ausgehenden TCP-Datenverkehrs an Port 80 zum ursprünglichen Host, ohne dass eine zusätzliche Regel erforderlich ist.
1. _Optional:_ Wenn Sie diese Sicherheitsgruppe anderen Instanzen zuordnen möchten, klicken Sie im Navigationsfenster auf ** Zugeordnete Schnittstellen** und wählen Sie zusätzliche Schnittstellen aus.
1. Wenn Sie die Erstellung von Regeln abgeschlossen haben, klicken Sie oben auf der Seite auf die Breadcrumb **Alle Sicherheitsgruppen**.

### Beispielsicherheitsgruppe  

Sie können beispielsweise Regeln für eingehende Daten konfigurieren, die Folgendes festlegen:

 * Gesamten SSH-Datenverkehr zulassen (TCP-Port 22)
 * Gesamten Ping-Datenverkehr zulassen (ICMP-Typ 8)

Konfigurieren Sie dann Regeln für abgehende Daten, die den gesamten TCP-Datenverkehr zulassen.

![Zeigt die Beispielregeln für eingehende und abgehende Daten](images/sg-example-ui.png)

## Variable IP-Adresse reservieren

Reservieren Sie eine variable IP-Adresse und ordnen Sie sie zu, damit Ihre Instanz vom Internet aus erreichbar ist.  

Ihre Instanz muss aktiv sein, bevor Sie eine variable IP-Adresse zuordnen können. Es kann einige Minuten dauern, bis die Instanz betriebsbereit ist.
{: tip}

Gehen Sie wie folgt vor, um eine variable IP-Adresse zu reservieren und zuzuordnen:

1. Klicken Sie auf der Seite mit den virtuellen Serverinstanzen auf Ihre Instanz, um die zugehörigen Details anzuzeigen.
1. Klicken Sie im Abschnitt **Netzschnittstellen** auf **Reservieren** für die Schnittstelle, die Sie einer variablen IP-Adresse zuordnen möchten.

Wenn Sie diese variable IP-Adresse später erneut einer anderen Instanz in derselben Zone zuordnen möchten, suchen Sie die variable IP-Adresse auf der Seite **Netz > Variable IP-Adressen**, klicken Sie auf das Überlaufmenü (**...**) und klicken Sie auf **Zuordnung aufheben**. Klicken Sie anschließend auf **Zuordnen**, um die Instanz und die Netzschnittstelle auszuwählen, die Sie der variablen IP-Adresse zuordnen möchten.
{: tip}

## Verbindung zu Ihrer Instanz herstellen
Setzen Sie unter Verwendung der von Ihnen erstellten variablen IP-Adresse einen Ping-Befehl für die Instanz ab, um sicherzustellen, dass diese betriebsbereit ist:

```
ping <public-ip-address>
```
{:pre}


### Verbindung zu Linux-Images herstellen

Da Sie Ihre Instanz mit einem öffentlichen SSH-Schlüssel erstellt haben, können Sie jetzt direkt mit Ihrem privaten Schlüssel eine Verbindung zu dieser Instanz herstellen:


```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

Weitere Informationen zum Herstellen einer Verbindung zu Ihrer Instanz finden Sie im Abschnitt zum [Herstellen einer Verbindung zu Ihrer Instanz unter Verwendung von Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance).

### Verbindung zu Windows-Images herstellen
Wenn Sie eine Verbindung zu einem Windows-Image herstellen möchten, melden Sie sich mit dem entschlüsselten Kennwort an. Um das entschlüsselte Kennwort abzurufen, kopieren Sie das verschlüsselte Kennwort von der Detailseite der Instanz und führen Sie den folgenden Befehl aus:

```
# Verschlüsseltes Kennwort entschlüsseln
cat ~/examplepwd | base64 --decode > ~/examplepwd64
# Verschlüsseltes Kennwort mit RSA-Private-Key entschlüsseln
openssl pkeyutl -in ~/examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
-pkeyopt rsa_mgf1_md:sha256
```
{:codeblock}


Weitere Informationen zum Herstellen einer Verbindung zu Ihrer Instanz finden Sie im Abschnitt zum [Herstellen einer Verbindung zur Windows-Instanz](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-windows-instance).



## Instanz überwachen

Für ein Aktivitätenprotokoll, das anzeigt, wann die Instanz gestartet, gestoppt oder neu gestartet wurde, klicken Sie im Navigationsfenster auf **Aktivität**.

## Lastausgleichsfunktion erstellen
Sie können eine Lastausgleichsfunktion erstellen, um eingehenden Datenverkehr über mehrere Instanzen zu verteilen.

Gehen Sie wie folgt vor, um eine Lastausgleichsfunktion zu erstellen:
1. Klicken Sie im Navigationsfenster auf **Netz > Lastausgleichsfunktionen**.
1. Klicken Sie auf der Seite mit den Lastausgleichsfunktionen auf **Neue Lastausgleichsfunktion** und geben Sie die folgenden Informationen an.
    * **Name**: Geben Sie einen Namen für die Lastausgleichsfunktion ein, z. B. `my-load-balancer`.
    * **Virtuelle private Cloud**: Wählen Sie Ihre VPC aus.
    * **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe für die Lastausgleichsfunktion aus.
    * **Typ**: Wählen Sie als Typ für die Lastausgleichsfunktion entweder 'Öffentlich' oder 'Privat' aus.
    * **Region**: Gibt die Region an, in der die Lastausgleichsfunktion erstellt wird. Dies ist die Region, die für Ihre VPC ausgewählt wurde.
    * **Teilnetze**: Wählen Sie die Teilnetze aus, in denen die Lastausgleichsfunktion erstellt werden soll. Um die Verfügbarkeit Ihrer Anwendung zu maximieren, wählen Sie Teilnetze in verschiedenen Zonen aus.
1. Klicken Sie auf **Neuer Pool** und geben Sie die folgenden Informationen an, um einen Back-End-Pool zu erstellen. Sie können einen oder mehrere Pools erstellen.
    * **Name**: Geben Sie einen Namen für den Pool ein, z. B. `my-pool`.
    * **Protokoll**: Wählen Sie das Protokoll für Ihre Back-End-Instanzen hinter der Lastausgleichsfunktion aus. Das Protokoll des Pools muss dem Protokoll des zugehörigen Listeners entsprechen. Wenn z. B. ein HTTPS- oder HTTP-Protokoll für den Listener ausgewählt wird, muss das Protokoll des Pools HTTP sein. Ebenso muss das Protokoll des Back-End-Pools TCP sein, wenn es sich bei dem Listenerprotokoll um TCP handelt.
    * **Methode**: Wählen Sie aus, wie die Lastausgleichsfunktion den Datenverkehr über die Back-End-Instanzen verteilen soll:
      * **Round-Robin:** Anforderungen werden abwechselnd an die einzelnen Instanzen weitergeleitet. Alle Instanzen erhalten ungefähr eine gleiche Anzahl von Clientverbindungen.
      * **Gewichtetes Round-Robin:** Für jede Instanz werden Anforderungen im Verhältnis zu ihrer zugeordneten Gewichtung weitergeleitet. Wenn Sie beispielsweise über die Instanzen A, B und C verfügen und ihre Gewichtungen auf 60, 60 und 30 gesetzt sind, erhalten die Instanzen A und B eine gleiche Anzahl von Verbindungen und die Instanz C erhält halb so viele Verbindungen.
      * **Wenigste Verbindungen:** Leiten Sie Anforderungen an die Instanz mit der aktuell geringsten Anzahl an Verbindungen weiter.  
    * **Sitzungsaffinität**: Wählen Sie aus, ob alle Anforderungen während einer Benutzersitzung an dieselbe Instanz gesendet werden.  
    * **Statusprüfungen**: Konfigurieren Sie, wie die Lastausgleichsfunktion den Status der Instanzen überprüft.
      * **Statusprotokoll**: Das Protokoll, das von der Lastausgleichsfunktion zum Senden von Statusprüfnachrichten an die Back-End-Instanzen verwendet wird.
      * **Statuspfad**: Der Statuspfad gilt nur dann, wenn HTTP als Protokoll für die Statusprüfung ausgewählt wird. Der Statuspfad gibt die URL an, die von der Lastausgleichsfunktion verwendet wird, um die Anforderungen der HTTP-Statusüberprüfung an die Back-End-Instanzen zu senden. Standardmäßig werden Statusprüfungen an den Stammverzeichnispfad (`/`) gesendet.
      * **Intervall**: Intervall in Sekunden zwischen zwei aufeinanderfolgenden Versuchen zur Statusprüfung. Standardmäßig werden Statusprüfungen alle fünf Sekunden gesendet.
      * **Zeitlimit**: Die Höchstdauer, die das System auf eine Antwort von einer Anforderung zur Statusprüfung wartet. Standardmäßig wartet die Lastausgleichsfunktion zwei Sekunden auf eine Antwort.
      * **Max. Anzahl Wiederholungen**: Die maximale Anzahl zusätzlicher Versuche zur Statusprüfung, die die Lastausgleichsfunktion vornimmt, bevor eine Back-End-Instanz als nicht in einwandfreiem Zustand deklariert wird. Standardmäßig wird eine Instanz nach zwei fehlgeschlagenen Statusprüfungen als nicht mehr in einwandfreiem Zustand angesehen.

        Obwohl die Lastausgleichsfunktion das Senden von Verbindungen an Instanzen in nicht einwandfreiem Zustand stoppt, überwacht sie weiterhin den Status dieser Instanzen und verwendet diese wieder, wenn die Instanzen nacheinander zwei Statusprüfungsversuche erfolgreich bestanden haben und als in einwandfreiem Zustand befunden wurden.

      Wenn Back-End-Instanzen in nicht einwandfreiem Zustand sind und Sie der Ansicht sind, dass Ihre Anwendung problemlos ausgeführt wird, überprüfen Sie die Werte des Statusprotokolls und des Statuspfads. Überprüfen Sie außerdem alle Sicherheitsgruppen, die an die Instanzen angehängt sind, um sicherzustellen, dass die Regeln den Datenverkehr zwischen der Lastausgleichsfunktion und den Instanzen zulassen.
      {: tip}

1. Klicken Sie auf **Erstellen**.
1. Klicken Sie neben dem Eintrag für den neuen Pool auf **Zuordnen** in der Spalte **Instanzen**, um eine Instanz zum Pool hinzuzufügen. Klicken Sie auf **Hinzufügen**, um dem Pool weitere Instanzen hinzuzufügen. Geben Sie für jede Instanz die folgenden Informationen an:
   * Wählen Sie ein oder mehrere Teilnetze aus, aus denen eine Instanz ausgewählt werden soll.
   * Wählen Sie eine Instanz aus. Wenn eine Instanz über mehrere Schnittstellen verfügt, stellen Sie sicher, dass Sie die richtige IP-Adresse auswählen.
   * Geben Sie den Port an, an dem der Datenverkehr an die Instanz gesendet wird.
   * Wenn Ihr Pool die Methode **Gewichtetes Round-Robin** verwendet, ordnen Sie jeder Instanz eine Gewichtung zu.  

      Die Zuordnung der Gewichtung '0' zu einer Instanz bedeutet, dass keine neuen Verbindungen an diese Instanz weitergeleitet werden, bestehender Datenverkehr jedoch weiterhin fließt, solange die aktuelle Verbindung aktiv ist. Die Verwendung der Gewichtung '0' kann dazu beitragen, eine Instanz ordnungsgemäß zu löschen und sie aus dem Serviceumlauf zu entfernen.
      {: tip}

1. Klicken Sie auf **Neuer Listener** und geben Sie die folgenden Informationen an, um einen Listener zu erstellen. Sie können einen oder mehrere Listener erstellen.
   * **Protokoll**: Das Protokoll, das für den Empfang eingehender Anforderungen verwendet werden soll.
   * **Port**: Der Empfangsport, an dem Anforderungen empfangen werden. Der Portbereich 56500 bis 56520 ist für Verwaltungszwecke reserviert und kann nicht verwendet werden.
   * **Back-End-Pool**: Der Back-End-Pool, an den dieser Listener Datenverkehr weiterleitet.
   * **Max. Anzahl Verbindungen** (optional): Die maximale Anzahl gleichzeitiger Verbindungen, die der Listener zulässt.
   * **SSL-Zertifikat**: Wenn HTTPS das ausgewählte Protokoll für diesen Listener ist, müssen Sie ein SSL-Zertifikat auswählen. Stellen Sie sicher, dass die Lastausgleichsfunktion berechtigt ist, auf das SSL-Zertifikat zuzugreifen. Anweisungen hierzu finden Sie im Abschnitt [Vorbereitende Schritte](#before).
1. Klicken Sie auf **Erstellen**.
1. Nachdem Sie die Erstellung von Pools und Listenern abgeschlossen haben, klicken Sie auf **Lastausgleichsfunktion erstellen**.
1. Wenn Sie die Details einer vorhandenen Lastausgleichsfunktion anzeigen möchten, klicken Sie auf der Seite **Lastausgleichsfunktionen** auf den Namen der Lastausgleichsfunktion.

## VPN erstellen
Sie können ein Virtual Private Network (VPN) erstellen, damit Ihre VPC eine sichere Verbindung mit einem anderen Netz, wie Ihrem lokalen Netz und einer anderen VPC, herstellen kann.

Informationen zum Anzeigen eines Codebeispiels finden Sie im Abschnitt zum [Verwenden des VPN mit der VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
{: tip}

Gehen Sie wie folgt vor, um ein VPN zu erstellen:
1. Klicken Sie im Navigationsfenster auf **Netz > VPNs**.
1. Klicken Sie auf der Seite "VPN" auf **Neues VPN-Gateway** und geben Sie die folgenden Informationen an:
    * **Name**: Geben Sie einen Namen für das VPN-Gateway in Ihrer VPC ein, z. B. `my-vpn-gateway`.
    * **Virtuelle private Cloud**: Wählen Sie Ihre VPC aus.
    * **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe für das VPN aus.
    * **Teilnetz**: Wählen Sie das Teilnetz aus, in dem das VPN-Gateway erstellt werden soll.
1. Definieren Sie im Abschnitt **Neue VPN-Verbindung** eine Verbindung zwischen diesem Gateway und einem Netz außerhalb Ihrer VPC, indem Sie die folgenden Informationen angeben.
    * **Verbindungsname**: Geben Sie einen Namen für die Verbindung ein, z. B. `my-connection`.
    * **Peer-Gateway-Adresse**: Geben Sie die IP-Adresse des VPN-Gateways für das Netz außerhalb Ihrer VPC an.
    * **Vorab verteilter Schlüssel**: Geben Sie den Authentifizierungsschlüssel des VPN-Gateways für das Netz außerhalb Ihrer VPC an.
    * **Lokale Teilnetze**: Geben Sie ein oder mehrere Teilnetze in der VPC ein, die Sie über den VPN-Tunnel verbinden möchten.
    * **Peer-Teilnetze**: Geben Sie ein oder mehrere Teilnetze in dem anderen Netz an, das Sie über den VPN-Tunnel verbinden möchten.
1. Um zu konfigurieren, wie das Cloud-Gateway Nachrichten sendet, um zu überprüfen, ob das Peer-Gateway aktiv ist, geben Sie die folgenden Informationen im Abschnitt **Dead-Peer-Detection** an.
    * **Aktion für Dead-Peer-Detection**: Die Aktion, die ausgeführt werden soll, wenn ein Peer-Gateway nicht mehr antwortet. Wählen Sie beispielsweise **Erneut starten** aus, wenn Sie möchten, dass das Gateway die Verbindung sofort neu aushandeln soll.
    * **Intervall**: Gibt an, wie oft geprüft werden soll, ob das Peer-Gateway aktiv ist. Standardmäßig werden Nachrichten alle 30 Sekunden gesendet.
    * **Zeitlimit**: Gibt an, wie lange auf eine Antwort vom Peer-Gateway gewartet werden soll. Standardmäßig wird ein Peer-Gateway nicht mehr als aktiv betrachtet, wenn eine Antwort nicht innerhalb von 150 Sekunden empfangen wird.
1. Geben Sie die Sicherheitsparameter für IKE und IPsec für die Verbindung an.
    * Wählen Sie **Automatisch** aus, wenn das Cloud-Gateway versuchen soll, die Verbindung automatisch herzustellen.
    * Wählen Sie angepasste Richtlinien aus oder erstellen Sie sie, wenn Sie bestimmte Sicherheitsanforderungen umsetzen müssen oder das VPN-Gateway für das andere Netz die Sicherheitsvorschläge, die durch die automatische Vereinbarung eingebracht werden, nicht unterstützt.

  **Wichtig**: Die Sicherheitsparameter für IKE und IPsec, die Sie für die Verbindung angeben, müssen dieselben Parameter sein, die auf dem Gateway für das Netz außerhalb der VPC festgelegt sind.

## Herzlichen Glückwunsch!

Sie haben erfolgreich eine VPC und ein Teilnetz, eine Zugriffssteuerungsliste (ACL), eine virtuelle Serverinstanz (VSI), einen Blockspeicherdatenträger, eine Sicherheitsgruppe, eine variable IP-Adresse, eine Lastausgleichsfunktion und ein virtuelles privates Netz (VPN) erstellt und konfiguriert. Sie können Ihre VPC weiter entwickeln, indem Sie weitere Instanzen, Teilnetze und andere Ressourcen hinzufügen.
