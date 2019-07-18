---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

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

# VPC mithilfe der REST-APIs löschen
{: #deleting-using-api}

Beim Löschen einer {{site.data.keyword.cloud}} Virtual Private Cloud mithilfe von REST-APIs werden dieselben allgemeinen Schritte ausgeführt, die auch beim [Löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) mithilfe der [Befehlszeilenschnittstelle](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli) ausgeführt werden.

Der Prozess besteht aus den folgenden Hauptschritten: 

1. Suchen Sie alle Teilnetze in der VPC, die Sie löschen möchten. 
2. Löschen Sie jedes einzelne Teilnetz in der VPC; dies bedeutet:
    - Löschen Sie alle VPN-Gateways im Teilnetz.
    - Löschen Sie alle Lastausgleichsfunktionen im Teilnetz.
    - Löschen Sie alle Netzschnittstellen (Instanzen) im Teilnetz.
    - Löschen Sie das Teilnetz.
3. Löschen Sie alle öffentlichen Gateways in der VPC.
4. Löschen Sie die VPC.

In den folgenden Abschnitten werden einige Beispiele für API-Aufrufe mit `curl` zum Löschen einer VPC bereitgestellt.

## Schritt 1: Alle Teilnetzen in der VPC suchen, die gelöscht werden sollen
{: #deleting-find-subnets-api}

Damit eine VPC gelöscht werden kann, müssen vorher alle zugehörigen Teilnetze gelöscht werden. Rufen Sie die ID der VPC ab, die Sie löschen möchten; führen Sie hierzu den folgenden Befehl aus und suchen Sie den Wert für `id`:

Fügen Sie ` | json_pp ` oder ` | jq ` nach dem curl-Befehl hinzu, um eine lesbare JSON-Zeichenfolge zu erhalten.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Speichern Sie die ID der VPC in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Führen Sie den folgenden Befehl aus, um die Liste aller Teilnetze in Ihrem Konto abzurufen:

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Überprüfen Sie den Wert für `vpc`, um festzustellen, welche Teilnetze gelöscht werden müssen. Speichern Sie die ID des Teilnetzes in einer Variablen zur späteren Verwendung; Beispiel: 

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Falls die VPC, die Sie löschen möchten, über mehrere Teilnetze verfügt, wiederholen Sie die Schritte, um jedes einzelne Teilnetz zu löschen.
{: important}

## Schritt 2: Alle Teilnetze in VPC löschen
{: #deleting-subnet-resources-api}

### Alle VPN-Gateways im Teilnetz löschen (sofern vorhanden)

Führen Sie den folgenden Befehl aus, um alle VPN-Gateways im Konto aufzulisten:

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Führen Sie für jedes VPN-Gateway in dem Teilnetz, das Sie löschen möchten, den folgenden Befehl aus; hierbei steht `$vpnid` für die ID des VPN-Gateways. 

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Der Status des VPN-Gateways muss sofort zu `deleting` wechseln, er wird aber trotzdem zurückgegeben, wenn eine Listenabfrage ausgeführt wird. Das Löschen eines VPN-Gateways kann bis zu 30 Minuten dauern. Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass das VPN-Gateway gelöscht wird; das Teilnetz kann jedoch erst gelöscht werden, wenn das VPN-Gateway und alle anderen Ressourcen im Teilnetz nicht mehr in den Listenabfragen angezeigt werden. 

### Alle Lastausgleichsfunktionen im Teilnetz löschen (sofern vorhanden)
{: #deleting-lbs-api}

Führen Sie den folgenden Befehl aus, um alle Lastausgleichsfunktionen im Konto aufzulisten:

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Führen Sie für jede Lastausgleichsfunktion in dem Teilnetz, das Sie löschen möchten, den folgenden Befehl aus; hierbei steht `$lbid` für die ID der Lastausgleichsfunktion. 

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Der Status der Lastausgleichsfunktion muss sofort zu `deleting` wechseln, sie wird aber trotzdem zurückgegeben, wenn eine Listenabfrage ausgeführt wird. Das Löschen einer Lastausgleichsfunktion kann bis zu 30 Minuten dauern. Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass die Lastausgleichsfunktion gelöscht wird; das Teilnetz kann jedoch erst gelöscht werden, wenn die Lastausgleichsfunktion und alle anderen Ressourcen im Teilnetz nicht mehr in den Listenabfragen angezeigt werden. 

### Alle Netzschnittstellen im Teilnetz löschen (sofern vorhanden)
{: #deleting-nics-api}

Eine Instanz kann über mehrere Netzschnittstellen verfügen und diese Netzschnittstellen können zu mehreren Teilnetzen der VPC gehören. Damit ein Teilnetz gelöscht werden kann, müssen vorher alle Netzschnittstellen im Teilnetz gelöscht werden. Die einzige Möglichkeit, eine Netzschnittstelle in einem Teilnetz zu löschen, besteht darin, die Instanz zu löschen.

Führen Sie den folgenden Befehl aus, um alle Instanzen im Konto aufzulisten:

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Wenn Sie alle Instanzen in der VPC löschen, können Sie den folgenden Befehl für jede Instanz in der VPC ausführen; hierbei steht `$vsi` für die ID der Instanz, die gelöscht werden soll. Wenn die Instanz gelöscht wird, werden alle Netzschnittstellen in anderen Teilnetzen automatisch gelöscht (sofern vorhanden).

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Der Status der Instanz muss sofort zu `deleting` wechseln, sie kann aber trotzdem in den Ergebnissen der Listenabfrage angezeigt werden. Das Löschen einer Instanz kann bis zu 30 Minuten dauern. Sie können anfordern, dass parallel weitere Teilnetzressourcen gelöscht werden, während darauf gewartet wird, dass die Instanz gelöscht wird; das Teilnetz kann jedoch erst gelöscht werden, wenn die Instanz und alle anderen Ressourcen im Teilnetz nicht mehr in den Listenabfragen angezeigt werden. 

Führen Sie den folgenden Befehl aus, um alle Netzschnittstellen einer Instanz aufzulisten:

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Wenn in dem Teilnetz, das Sie löschen möchten, eine sekundäre Netzschnittstelle vorhanden ist, müssen Sie die Instanz löschen. Eine Netzschnittstelle kann nicht in der Instanz gelöscht werden, ohne die Instanz zu löschen.

### Teilnetz löschen
{: #deleting-subnet-api}

Sobald alle Ressourcen innerhalb des Teilnetzes gelöscht wurden und bei der Ausführung einer Listenabfrage nicht zurückgegeben werden, führen Sie den folgenden Befehl aus, um das Teilnetz zu löschen:

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Der Status des Teilnetzes muss sofort zu `deleting` wechseln; es kann jedoch einige Minuten dauern, bis das Teilnetz so weit gelöscht ist, dass es in den Listenabfragen nicht mehr zurückgegeben wird.

## Schritt 3: Alle öffentlichen Gateways in der VPC löschen (sofern vorhanden)
{: #deleting-pgs-api}

Führen Sie den folgenden Befehl aus, um alle öffentlichen Gateways im Konto aufzulisten:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Führen Sie für jedes öffentliche Gateway in der VPC, das Sie löschen möchten, den folgenden Befehl aus, um das öffentliche Gateway zu löschen; hierbei steht `$gateway` für die ID des öffentlichen Gateways. 

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}


## Schritt 4: VPC löschen
{: #deleting-single-vpc-api}

Sobald alle Teilnetze, VPN-Gateways, Lastausgleichsfunktionen und öffentliche Gateways in der VPC gelöscht sind, führen Sie den folgenden Befehl aus, um die VPC zu löschen; hierbei steht `$vpc` für die ID der VPCs, die die Sie löschen.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Der Status der VPC muss sofort zu `deleting` wechseln; es kann jedoch einige Minuten dauern, bis die VPC gelöscht ist, sodass sie in den Listenabfragen nicht mehr angezeigt wird.
