---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

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
{:DomainName: data-hd-keyref="DomainName"}

# VPC mithilfe der IBM Cloud-Befehlszeilenschnittstelle löschen
{: #deleting-using-cli}

In diesem Abschnitt werden Beispiele zum Löschen von Ressourcen aus {{site.data.keyword.cloud}} Virtual Private Cloud für jede VPC-Ressource in der vorgeschlagenen Reihenfolge bereitgestellt.  

Nachfolgend werden die wichtigsten Fakten aufgeführt, die beim Löschen zu berücksichtigen sind: 

* Eine VPC kann erst gelöscht werden, wenn sie leer ist. 
* Damit eine übergeordnete Ressource gelöscht werden kann, müssen vorher alle enthaltenen Ressourcen gelöscht werden.  
* Wenn das Löschen einer Ressource anstehend ist, lautet ihr Status `deleting`, wenn sie in den Listenabfragen angezeigt wird.  
* Die meisten Löschanforderungen sind _asynchron_; dies bedeutet, dass für die Ressource der Status **deleting** in Listenabfragen anzeigt werden kann, wenn sie noch nicht vollständig gelöscht wurde. 
* Sie müssen eine Abfrage durchführen, um sicherzustellen, dass die untergeordnete Ressource nicht mehr in der Listenansicht vorhanden ist, bevor Sie versuchen, die übergeordnete Ressource zu löschen. 

Wenn der Status der Ressource von `deleting` zu `failed` wechselt, können Sie versuchen, die Ressource erneut zu löschen. Wenn Sie eine Ressource im Status `failed` nicht löschen können, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Schritt 1: Nach allen Teilnetzen in der VPC suchen, die gelöscht werden sollen
{: #deleting-find-subnets-cli}

Damit eine VPC gelöscht werden kann, müssen vorher alle zugehörigen Teilnetze gelöscht werden. Rufen Sie die ID der VPC ab, die Sie löschen möchten; führen Sie hierzu den folgenden Befehl aus und suchen Sie den Wert für `ID`:

```
ibmcloud is vpcs
```
{: pre}

Speichern Sie die ID der VPC in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Führen Sie den folgenden Befehl aus, um die Liste aller Teilnetze in Ihrem Konto abzurufen:

```
ibmcloud is subnets
```
{: pre}

Überprüfen Sie den Wert für `VPC`, um festzustellen, welche Teilnetze gelöscht werden müssen. Speichern Sie die ID des Teilnetzes in einer Variablen so, dass sie später wieder verwendet werden kann. Beispiel:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Falls die VPC, die Sie löschen möchten, über mehrere Teilnetze verfügt, wiederholen Sie diese Schritte, um jedes einzelne Teilnetz zu löschen.
{: important}

## Schritt 2: Jedes Teilnetz in der VPC löschen
{: #deleting-subnet-resources-cli}

### Alle VPN-Gateways im Teilnetz löschen (sofern vorhanden)

Führen Sie den folgenden Befehl aus, um alle VPN-Gateways im Konto aufzulisten:

```
ibmcloud is vpn-gateways
```
{: pre}

Führen Sie für jedes VPN-Gateway in dem Teilnetz, das Sie löschen möchten, den folgenden Befehl aus; hierbei steht `$vpnid` für die ID des VPN-Gateways.

```
ibmcloud is vpn-gateway-delete $vpnid
```

Der Status des VPN-Gateways muss sofort zu `deleting` wechseln; er wird aber angezeigt, wenn eine Listenabfrage ausgeführt wird. Das Löschen eines VPN-Gateways kann bis zu 30 Minuten dauern.
{: note}

Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass das VPN-Gateway gelöscht wird. Das Teilnetz kann jedoch erst gelöscht werden, wenn das VPN-Gateway und alle anderen Ressourcen im Teilnetz nicht mehr in den Ergebnissen der Listenabfrage angezeigt werden. 

### Alle Lastausgleichsfunktionen im Teilnetz löschen (sofern vorhanden)
{: #deleting-lbs-cli}

Führen Sie den folgenden Befehl aus, um alle Lastausgleichsfunktionen im Konto aufzulisten:

```
ibmcloud is load-balancers
```
{: pre}

Führen Sie für jede Lastausgleichsfunktion in dem Teilnetz, das Sie löschen möchten, den folgenden Befehl aus; hierbei steht `$lbid` für die ID der Lastausgleichsfunktion. 

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

Der Status der Lastausgleichsfunktion muss sofort zu `deleting` wechseln, sie wird aber trotzdem im Ergebnis einer Listenabfrage angezeigt. Das Löschen einer Lastausgleichsfunktion kann bis zu 30 Minuten dauern.
{: note}

Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass die Lastausgleichsfunktion gelöscht wird. Das Teilnetz kann jedoch erst gelöscht werden, wenn die Lastausgleichsfunktion und alle anderen Ressourcen im Teilnetz nicht mehr in den Ergebnissen der Listenabfrage angezeigt werden. 

### Alle Netzschnittstellen im Teilnetz löschen (sofern vorhanden)
{: #deleting-nics-cli}

Eine Instanz kann über mehrere Netzschnittstellen verfügen und diese Netzschnittstellen können zu mehreren Teilnetzen der VPC gehören. Damit ein Teilnetz gelöscht werden kann, müssen vorher alle Netzschnittstellen im Teilnetz gelöscht werden.  

Die einzige Möglichkeit, eine Netzschnittstelle in einem Teilnetz zu löschen, besteht darin, die Instanz zu löschen.
{: note}

Führen Sie den folgenden Befehl aus, um alle Instanzen im Konto aufzulisten:

```
ibmcloud is instances
```
{: pre}

Wenn Sie alle Instanzen in der VPC löschen, können Sie den folgenden Befehl für jede Instanz in der VPC ausführen; hierbei steht `$vsi` für die ID der Instanz, die gelöscht werden soll. Wenn die Instanz gelöscht wird, werden alle Netzschnittstellen in anderen Teilnetzen automatisch gelöscht (sofern vorhanden).

```
ibmcloud is instance-delete $vsi
```
{: pre}

Der Status der Instanz muss sofort zu `deleting` wechseln; er wird aber noch im Ergebnis einer Listenabfrage angezeigt. Das Löschen einer Instanz kann bis zu 30 Minuten dauern.
{: note}

Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass die Instanz gelöscht wird. Das Teilnetz kann jedoch erst gelöscht werden, wenn die Instanz und alle anderen Ressourcen im Teilnetz nicht mehr in den Ergebnissen der Listenabfrage angezeigt werden. 

Führen Sie den folgenden Befehl aus, um alle Netzschnittstellen einer Instanz aufzulisten:

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

Wenn in dem Teilnetz, das Sie löschen möchten, eine sekundäre Netzschnittstelle vorhanden ist, müssen Sie die Instanz löschen. Eine Netzschnittstelle kann nicht in der Instanz gelöscht werden, ohne die Instanz zu löschen.

### Teilnetz löschen
{: #deleting-subnet-cli}

Sobald alle Ressourcen innerhalb des Teilnetzes gelöscht wurden und somit nicht im Ergebnis einer Listenabfrage zurückgegeben werden, führen Sie den folgenden Befehl aus, um das Teilnetz zu löschen:

```
ibmcloud is subnet-delete $subnet
```
{: pre}

Der Status des Teilnetzes muss sofort zu `deleting` wechseln; es kann jedoch einige Minuten dauern, bis das Teilnetz tatsächlich gelöscht ist und nicht mehr in den Ergebnissen der Listenabfrage angezeigt wird. 

## Schritt 3: Alle öffentlichen Gateways in der VPC löschen (sofern vorhanden)
{: #deleting-pgs-cli}

Führen Sie den folgenden Befehl aus, um alle öffentlichen Gateways im Konto aufzulisten:

```
ibmcloud is public-gateways
```
{: pre}

Führen Sie für jedes öffentliche Gateway in der VPC, das Sie löschen möchten, den folgenden Befehl aus, um das öffentliche Gateway zu löschen; hierbei steht `$gateway` für die ID des öffentlichen Gateways. 

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Schritt 4: VPC löschen
{: #deleting-single-vpc-cli}

Sobald alle Teilnetze, VPN-Gateways, Lastausgleichsfunktionen und öffentliche Gateways in der VPC gelöscht sind, führen Sie den folgenden Befehl aus, um die VPC zu löschen; hierbei steht `$vpc` für die ID der VPCs, die die Sie löschen.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

Der Status der VPC muss sofort zu `deleting` wechseln; es kann jedoch einige Minuten dauern, bis die VPC tatsächlich gelöscht ist und nicht mehr in den Ergebnissen der Listenabfrage angezeigt wird. 
