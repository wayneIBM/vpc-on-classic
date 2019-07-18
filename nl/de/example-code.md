---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

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

# VPC mithilfe der REST-APIs verwenden
{: #creating-a-vpc-using-the-rest-apis}

In diesem Dokument wird beschrieben, wie {{site.data.keyword.cloud}} Virtual Private Cloud-Ressourcen mithilfe der REST-APIs unter Verwendung eines `curl`-Befehls erstellt werden. Informationen zu Code-Snippets, von denen REST-APIs unter Verwendung von Go und Python aufgerufen werden, finden Sie im folgenden [Beispielcode](https://github.com/IBM-Cloud/vpc-api-samples).

## Vorbedingungen

1. Stellen Sie sicher, dass Sie über einen öffentlichen SSH-Schlüssel verfügen, der verwendet wird, um eine Verbindung zur virtuellen Serverinstanz herzustellen. 

   Möglicherweise haben Sie bereits einen öffentlichen SSH-Schlüssel. Suchen Sie nach einer Datei mit dem Namen `id_rsa.pub` in einem `.ssh`-Verzeichnis unter Ihrem Ausgangsverzeichnis, z. B. `/Users/<USERNAME>/.ssh/id_rsa.pub`. Die Datei beginnt mit `ssh-rsa` und endet mit Ihrer E-Mail-Adresse.

   Wenn Sie keinen öffentlichen SSH-Schlüssel oder das Kennwort eines vorhandenen Schlüssels vergessen haben, generieren Sie einen neuen, indem Sie den Befehl `ssh-keygen` ausführen und den Eingabeaufforderungen folgen.
2.  Stellen Sie sicher, dass Sie über einen API-Schlüssel für Ihr IBM Cloud-Konto verfügen. Wenn Sie über keinen API-Schlüssel verfügen, überprüfen Sie den Abschnitt [API-Schlüssel erstellen](/docs/iam?topic=iam-userapikey#create_user_key). Sie müssen diesen API-Schlüssel in Schritt 1 in einer Umgebungsvariablen speichern.

## Schritt 1: API-Schlüssel als Variable speichern

Führen Sie den folgenden Befehl aus, um Ihren API-Schlüssel in einer Umgebungsvariablen zu speichern:

```bash
apikey="<api-schlüssel>"
```
{: pre}

## Schritt 2: IBM IAM-Token (IAM - Identity and Access Management) abrufen

Informationen zum Abrufen eines IAM-Tokens finden Sie unter [IBM Cloud-IAM-Token mithilfe eines API-Schlüssels abrufen](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey); alternativ können Sie die folgenden Beispielbefehle verwenden.

Führen Sie den folgenden Befehl aus, um eine IAM-Token mit dem Dienstprogramm `jq` abzurufen und zu parsen. Sie können den Befehl so ändern, dass ein anderes Parsing-Tool verwendet wird; alternativ können Sie den letzten Teil des Befehls entfernen, wenn Sie das Token selbst manuell parsen möchten.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

Führen Sie zum Anzeigen des IAM-Tokens `echo $iam_token` aus. Das Ergebnis sollte wie folgt aussehen:

```
Bearer <token>
```

Vom Autorisierungsheader wird erwartet, dass das IAM-Token mit `Bearer` beginnt. Wenn das Ergebnis, das Sie empfangen, nicht `Bearer` enthält, aktualisieren Sie die Variable `iam_token` so, dass sie diese Angabe enthält. Diese Beispiele setzen voraus, dass `Bearer` in `iam_token` enthalten ist.

Sie müssen den vorherigen Schritt wiederholen, um das IAM-Token stündlich zu aktualisieren, da das Token entsprechend abläuft.
{: important}

## Schritt 3: API-Endpunkt und Versionsparameter als Variable speichern

Die API-Endpunkte sind pro Region angegeben und folgen der Konvention `https://<region>.iaas.cloud.ibm.com`. Für die Regionen stehen folgende Endpunkte zur Verfügung:

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

Speichern Sie den API-Endpunkt in einer Variablen, sodass Sie ihn in API-Anforderungen verwenden können, ohne die vollständige URL eingeben zu müssen. Führen Sie zum Beispiel den folgenden Befehl aus, um den Endpunkt 'us-south' in einer Variablen zu speichern:

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

Um zu überprüfen, ob diese Variable gespeichert wurde, führen Sie `echo $rias_endpoint` aus, und stellen Sie sicher, dass die Antwort nicht leer ist.

Jede API-Anforderung muss den Parameter `version` im Format `JJJJ-MM-TT` enthalten. Führen Sie den folgenden Befehl aus, um das Versionsdatum in einer Variablen zu speichern, sodass es in einer zukünftigen Sitzung erneut verwendet werden kann. Weitere Informationen zum Festlegen des Parameters `version` finden Sie in **Versionssteuerung** unter [API-Referenz für VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

Um zu überprüfen, ob diese Variable gespeichert wurde, führen Sie `echo $version` aus, und stellen Sie sicher, dass die Antwort nicht leer ist.

## Schritt 4: API zum Abrufen von Regionen ausführen

Der folgende Befehl gibt die für VPC verfügbaren Regionen im JSON-Format zurück. Es sollte mindestens ein Objekt zurückgegeben werden.

In jeder API-Anforderung sind die Abfrageparameter `version` und `generation` erforderlich. Ausführliche Informationen hierzu finden Sie in [API-Referenz für VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Wenn unerwartete Ergebnisse auftreten, fügen Sie nach dem `curl`-Befehl das Flag `--verbose` (Debug) hinzu, um detaillierte Protokollierungsinformationen zu erhalten. Informationen hierzu finden Sie auch im Abschnitt zu den häufig auftretenden Fehlern auf der [Seite zur Fehlerbehebung](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Schritt 5: API zum Abrufen von Zonen ausführen

Nach Absetzen des folgenden Befehls werden alle für die VPC in der Region `us-south` verfügbaren Zonen im JSON-Format zurückgegeben. Es sollten mindestens drei Objekte zurückgegeben werden.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Aktualisieren Sie den oben im Parameter angegebenen Regionsnamen `us-south` abhängig vom jeweils verwendeten Regionsendpunkt. Von einem Regionsendpunkt werden nur eigene Zonen erkannt; verwenden Sie deswegen bei Verwendung des Endpunkts in Tokio `jp-tok`.
{: note}

## Schritt 6: API zum Abrufen von Profilen ausführen

Der folgende Befehl gibt die für Ihre Instanzen verfügbaren Profile im JSON-Format zurück. Es sollte mindestens ein Objekt zurückgegeben werden.

Fügen Sie ` | json_pp ` nach dem 'curl'-Befehl hinzu, um eine lesbare JSON-Zeichenfolge zu erhalten.
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Wenn die API-Ausgabe durch die Seitenaufteilung begrenzt ist, legen Sie den Grenzwert höher als den Wert für `total_count` fest, z. B.:

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Schritt 7: API zum Abrufen von Images ausführen

Der folgende Befehl gibt die für Ihre Instanzen verfügbaren Images im JSON-Format zurück. Es sollte mindestens ein Objekt zurückgegeben werden.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Schritt 8: API zum Abrufen von VPCs ausführen

Vom folgenden Befehl werden die VPCs zurückgegeben, die unter Ihrem Konto im JSON-Format erstellt wurden.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Schritt 9: VPC erstellen

Erstellen Sie eine {{site.data.keyword.cloud_notm}} VPC-Instanz namens `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

Für den Rest der Anrufe müssen Sie die ID der neu erstellten {{site.data.keyword.cloud_notm}} VPC-Instanz kennen. Speichern Sie die ID in einer Variablen. Der Befehl ähnelt der folgenden Zeichenfolge: `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<vpc-id>"
```
{: pre}

## Schritt 10: Teilnetz erstellen

Erstellen Sie ein Teilnetz in der {{site.data.keyword.cloud_notm}} VPC-Instanz. Im folgenden Beispiel wird eine VPC in der Zone `us-south-2` erstellt.

Im folgenden Beispiel wird das [Standardadresspräfix](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) für die Zone `us-south-2` verwendet. Die Standardeinstellungen für Ihr Konto können sich geändert haben; Informationen hierzu finden Sie unter [Vorgehensweise zum Planen der VPC-Adressen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

Führen Sie den folgenden Befehl aus, um eine Liste der Adresspräfixe für die VPC abzurufen:
`curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`..
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Speichern Sie die Teilnetz-ID in einer Variablen.

```bash
subnet="<teilnetz-id>"
```
{: pre}

## Schritt 11: Status des Teilnetzes überprüfen

Damit Ressourcen im Teilnetz bereitgestellt werden können, muss das Teilnetz den Status `available` (verfügbar) aufweisen. Fragen Sie die Teilnetzressource ab und stellen Sie sicher, dass der Status `available` lautet, bevor Sie fortfahren. Falls der Status `failed` lautet, wenden Sie sich mit den entsprechenden Details an den [Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support). Sie können versuchen fortzufahren, indem Sie versuchen, ein weiteres Teilnetz bereitzustellen.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Schritt 12: Öffentliches Gateway erstellen

Um Instanzen virtueller Server im Teilnetz Zugriff auf das öffentliche Internet zu ermöglichen, fügen Sie dem Teilnetz ein öffentliches Gateway (PGW) hinzu.  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Speichern Sie die ID des öffentlichen Gateways in einer Variablen.

```bash
gateway="<gateway-id>"
```
{: pre}

Damit das öffentliche Gateway zugeordnet und verwendet werden kann, muss sich das öffentliche Gateway im Status `available` befinden. Fragen Sie die Ressource des öffentlichen Gateways ab und stellen Sie sicher, dass der Status `available` lautet, bevor Sie fortfahren. Falls der Status `failed` lautet, wenden Sie sich mit den entsprechenden Details an den [Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support). Sie können versuchen fortzufahren, indem Sie versuchen, ein weiteres öffentliches Gateway bereitzustellen.

Führen Sie den folgenden Befehl aus, um den Status des öffentlichen Gateways zu überprüfen:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Schritt 13: Öffentliches Gateway zu Teilnetz zuordnen

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Schritt 14: SSH-Schlüssel erstellen

Erstellen Sie einen Schlüssel mit dem öffentlichen SSH-Schlüssel. Dieser Schlüssel wird beim Erstellen der virtuellen Serverinstanz verwendet. Der Schlüssel ist auch für die Anmeldung an einer virtuellen Instanz des Servers erforderlich.

Schlüssel können hinzugefügt werden, wenn Sie vorher die VPC in der Benutzerschnittstelle oder in Befehlszeilenschnittstelle erstellt haben. Später stehen keine Tools zum Hinzufügen von Schlüsseln zur Verfügung.
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

Speichern Sie die Schlüssel-ID in einer Variablen.

```bash
key="<schlüssel-id>"
```
{: pre}

## Schritt 15: Profil und Image für virtuelle Serverinstanz auswählen

Führen Sie die APIs aus, um alle Profile und Images aufzulisten, die für Ihre virtuelle Serverinstanz verfügbar sind, und wählen Sie eine Kombination aus.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Wenn Sie weitere Details zu den Profilen abrufen möchten, können Sie den globalen Katalog mit Hilfe des {{site.data.keyword.cloud_notm}}-CLI-Befehls `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]` abfragen. Zum Beispiel:

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

Mit dem folgenden Befehl werden die verfügbaren Images aufgelistet.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Speichern Sie den Profilnamen und die Image-ID in Variablen, die Sie für die Bereitstellung einer Instanz des virtuellen Servers verwenden.

```bash
profile_name="<ausgewählter_profilname>"
image_id="<ausgewählte_image-id>"
```
{: pre}

## Schritt 16: Virtuelle Serverinstanz bereitstellen

Stellen Sie eine virtuelle Serverinstanz (Virtual Server Instance, VSI) im neu erstellten Teilnetz bereit. Geben Sie Ihren öffentlichen SSH-Schlüssel an, damit Sie sich anmelden können, nachdem die Instanz bereitgestellt wurde.

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-south-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

Speichern Sie die ID der virtuellen Serverinstanz in einer Variablen.

```bash
server="<instanz-id>"
```
{: pre}

## Schritt 17: Status der virtuellen Serverinstanz überprüfen

Die Bereitstellung einer virtuellen Serverinstanz kann einige Minuten dauern. Bevor Sie fortfahren, fragen Sie den Status des Servers ab und stellen sicher, dass der Server aktiv (`running`) ist.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Speichern Sie die ID der primären Netzschnittstelle der virtuellen Serverinstanz (die im vorherigen API-Aufruf zurückgegeben wurde) in einer Variablen.  

```bash
network_interface="<netzschnittstellen-id>"
```
{: pre}

Sie können die primäre Netzschnittstelle erst abrufen, wenn Sie die spezielle Instanz abfragen.
{: note}

## Schritt 18: Variable IP-Adresse erstellen

Verwenden Sie zum Erstellen einer variablen IP-Adresse für die virtuelle Serverinstanz die primäre Netzschnittstelle der Instanz als Ziel für die neue variable IP-Adresse.

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Speichern Sie die ID der variablen IP-Adresse in einer Variablen.

```bash
floating_ip="<id_der_variablen_ip-adresse>"
```
{: pre}

## Schritt 19: Regel zur Standardsicherheitsgruppe hinzufügen, um SSH-Datenverkehr zu ermöglichen

Konfigurieren Sie die Sicherheitsgruppe, um den eingehenden und abgehenden Datenverkehr zu definieren, der für die Instanz zulässig ist.

Suchen Sie die Sicherheitsgruppe für die VPC:

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Speichern Sie die ID der Sicherheitsgruppe in einer Variablen.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Erstellen Sie jetzt eine Regel, die den eingehenden SSH-Datenverkehr ermöglicht:

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Schritt 20: Verbindung mit SSH zur virtuellen Serverinstanz herstellen

Um eine Verbindung mit SSH zum Server herzustellen, verwenden Sie den Wert für `address` der von Ihnen vorher erstellen variablen IP-Adresse. Führen Sie den folgenden Befehl aus, um die Adresse (`address`) der variablen IP-Adresse abzurufen:

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Verwenden Sie den Wert von `address` der variablen IP-Adresse, um eine Verbindung zur virtuellen Serverinstanz mit SSH herzustellen:

```
ssh -i <private_schlüsseldatei> root@<variable_ip-adresse>
```
{: pre}

Der SSH-Zugriff auf den virtuellen Server kann durch Sicherheitsgruppen verhindert werden. Wenn SSH-Zugriff erforderlich ist, müssen Sie eine [entsprechende Sicherheitsgruppe](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) verwenden.
{: note}

Weitere Informationen zum Herstellen einer Verbindung zu Ihrer Instanz finden Sie im Abschnitt zum [Herstellen einer Verbindung zu Ihrer Instanz unter Verwendung von Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance).

## Schritt 21: Blockspeicherdatenträger erstellen und zuordnen

Erstellen Sie einen Blockspeicherdatenträger mit einer Anforderung, die diesem Beispiel ähnelt; geben Sie dabei einen Datenträgernamen, eine Zone und ein Profil an.

Geben Sie die folgende Anforderung an, um eine Liste der Datenträgerprofile anzuzeigen:

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

Zur Verfügung stehen Universalprofile (3 IOPS/GB), Profile mit 5 IOPS-Schichten (5iops-tier), Profile mit 10 IOPS-Schichten (10iops-tier) und angepasste Profile. Informationen zur Datenträgerkapazität und den IOPS-Bereichen abhängig vom jeweils ausgewählten Datenträgerprofil finden Sie unter [Informationen zu Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance).

Sie müssen in der Anforderung keinen Datenträgernamen angeben; der Datenträgername wird standardmäßig vom System erstellt. Im folgenden Beispiel wird der Datenträgername `helloworld-vol` verwendet. Alle Datenträgernamen müssen eindeutig sein.

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Für den Rest der Aufrufe müssen Sie die ID des neu erstellten Datenträgers kennen. Speichern Sie die ID des Datenträgers als Variable, zum Beispiel `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<datenträger-id>"
```
{: pre}

Wenn ein Datenträger erstellt wird, lautet sein Status zunächst `pending`. Damit Sie fortfahren können, muss der Datenträger in den Status `available` wechseln; dies kann einige Minuten dauern.
{: note}


Führen Sie den folgenden Befehl aus, um den Status des Datenträgers zu überprüfen:

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Erstellen Sie eine Datenträgerzuordnung, um den neuen Datenträger einer virtuellen Serverinstanz zuzuordnen. Verwenden Sie die Variable mit der Instanz-ID, die Sie zuvor in der Anforderung erstellt haben. Verwenden Sie die Datenträger-ID, den CRN des Datenträgers oder die URL, um den Datenträger im Datenparameter anzugeben. Im folgenden Beispiel wird die Variable verwendet, die für die Datenträger-ID erstellt wurde.

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

Rufen Sie nach der Erstellung der Datenträgerzuordnung die ID für die Datenträgerzuordnung ab.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Speichern Sie die ID für die Zuordnung als Variable.

```bash
attachment="<datenzuordnungs-id>"
```
{: pre}

Geben Sie die Datenträgerzuordnungs-ID an, um den neuen Datenträger einer virtuellen Serverinstanz zuzuordnen. 

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Schritt 22 (Optional): Ressourcen löschen

Löschen Sie optional die Ressourcen. Eine Ressource kann nicht gelöscht werden, wenn sie andere Ressourcen enthält. Beispiel: Eine VPC kann nicht gelöscht werden, wenn sie Teilnetze enthält, und ein Teilnetz kann nicht gelöscht werden, wenn es virtuelle Serverinstanzen enthält. Bei einem Löschvorgang kann es vorkommen, dass der Vorgang von der API schnell als abgeschlossen zurückgegeben wird, die Löschung jedoch noch ausgeführt wird. Stellen Sie nach dem Absetzen der Löschanforderung sicher, dass die Ressource gelöscht wurde, bevor Sie versuchen, die übergeordnete Ressource zu löschen. Weitere Informationen hierzu finden Sie im Abschnitt [VPC löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

### Variable IP-Adresse löschen

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Virtuelle Serverinstanz löschen

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Schlüssel löschen

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Teilnetz löschen

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Öffentliches Gateway löschen

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### VPC löschen

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Datenträger löschen

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## Herzlichen Glückwunsch!

Sie haben erfolgreich eine VPC-Instanz mithilfe der REST-APIs bereitgestellt. Wenn Sie weitere API-Befehle testen möchten, sollten Sie sich die vollständige Referenz ansehen:

* [API-Referenz für VPC](https://{DomainName}/apidocs/vpc-on-classic)
