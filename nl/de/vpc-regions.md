---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

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

# VPC in einer anderen Region erstellen
{: #creating-a-vpc-in-a-different-region}

Eine Region ist ein bestimmter Standort, an dem Sie Apps, Services und andere {{site.data.keyword.cloud}}-Ressourcen bereitstellen können. Regionen bestehen aus einer oder mehreren Zonen, bei denen es sich um physische Rechenzentren handelt, in denen die für Services und Apps verwendeten Ressourcen für Datenverarbeitung, Vernetzung und Speicher mit der zugehörigen Zone und Stromversorgung bereitgestellt werden. Zonen werden voneinander isoliert, wodurch kein gemeinsamer Single Point of Failure innerhalb einer Region gewährleistet ist.

Der IBM Cloud Virtual Private Cloud-Service ist regional. Damit eine Wiederherstellung nach einem Katastrophenfall (Disaster Recovery) durchgeführt werden kann, müssen Sie die Möglichkeit zur Wiederherstellung mit einem Failover in einer alternativen Region bereitstellen; hierzu richten Sie eine VPC in einer anderen Regionen ein.
{: note}

Virtual Private Cloud wird in Stufen in allen {{site.data.keyword.cloud}}-Regionen bereitgestellt.

|   Standort     | Region | API-Endpunkt | Status |
| ------- | :------: | :------: |:------: |
| Dallas | us-south | `us-south.iaas.cloud.ibm.com`| Verfügbar |
| Frankfurt | eu-de | `eu-de.iaas.cloud.ibm.com`| Verfügbar |
| Tokio | jp-tok | `jp-tok.iaas.cloud.ibm.com`| Verfügbar |
| London | eu-gb | `eu-gb.iaas.cloud.ibm.com`| Demnächst verfügbar |
| Sydney | au-syd | `au-syd.iaas.cloud.ibm.com`| Demnächst verfügbar |
| Washington DC | us-east | `us-east.iaas.cloud.ibm.com`| Demnächst verfügbar |

Der Endpunkt der regionalen API (VPC) wird automatisch von der IBM Cloud-CLI festgelegt, wenn Sie sich an einer bestimmten Region anmelden.
{: note}

## Bei einer bestimmten Region über die Befehlszeilenschnittstelle anmelden
{: #log-in-to-a-specific-region-using-the-cli}

Wenn Sie sich an IBM Cloud anmelden, können Sie eine Region angeben oder sie später auswählen. Wenn Sie sich zum Beispiel in der Region Dallas (`us-south`) direkt am globalen API-Endpunkt anmelden möchten, führen Sie die folgenden Befehle aus; welchen Befehl Sie ausführen müssen, hängt davon ab, ob Sie über ein föderiertes Konto (SSO) oder ein nicht föderiertes Konto verfügen.

Führen Sie für ein föderiertes Konto den folgenden Befehl aus:

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

Führen Sie für ein nicht föderiertes Konto den folgenden Befehl aus:

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

Um eine Region zu einem späteren Zeitpunkt auszuwählen, geben Sie den Parameter `-r <region>` nicht an. Die CLI fordert Sie dann zur Auswahl einer Region auf.

Beispielausgabe:

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## Über die Befehlszeilenschnittstelle zwischen Regionen wechseln
{: #switch-regions-using-the-cli}

Führen Sie den folgenden Befehl aus, um den aktuellen Status der VPC-Regionen abzurufen:

```
ibmcloud is regions
```
{: pre}

Um zu einer anderen Region zu wechseln, führen Sie den Befehl `ibmcloud target -r <region>` aus. Um beispielsweise zur Region "Frankfurt" zu wechseln, führen Sie den folgenden Befehl aus:

```
ibmcloud target -r eu-de
```
{: pre}

Führen Sie den folgenden Befehl aus, den aktuellen Standort zu überprüfen:

```
ibmcloud target
```
{: pre}

## Über die API zwischen Regionen wechseln  
{: #switch-regions-using-the-api}

Wenn Sie mit der regionalen VPC-API über REST interagieren möchten, leiten Sie die Anforderung an den API-Endpunkt weiter, der der Region zugeordnet ist, in der Sie die Ressourcen erstellen möchten. Der API-Endpunkt der Region wird in der vorherigen Tabelle angezeigt. Sie können auch die Endpunkte suchen, die den Regionen zugeordnet sind, wenn Sie den folgenden Befehl ausführen:

```
ibmcloud is regions
```
{: pre}


Wenn Sie beispielsweise die Liste der VPCs in der Region `us-south` abrufen möchten, führen Sie den folgenden Befehl aus:

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## Zonen abrufen
{: #get-zones}

Um die Liste der Zonen abzurufen, die für jede Region verfügbar sind, führen Sie den Befehl `ibmcloud is zones <region>` aus. Wenn Sie beispielsweise die Liste der Zonen in der Region `us-south` abrufen möchten, führen Sie den folgenden Befehl aus:

```
ibmcloud is zones us-south
```
{: pre}
