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

# Creazione di un VPC utilizzando le API REST
{: #creating-a-vpc-using-the-rest-apis}

Questo documento ti mostra come creare le risorse {{site.data.keyword.cloud}} Virtual Private Cloud utilizzando le API REST con il comando `curl`. Per i frammenti di codice che richiamano le API REST utilizzando Go e Python, vedi questo [codice di esempio](https://github.com/IBM-Cloud/vpc-api-samples).

## Prerequisiti

1. Assicurati di avere una chiave SSH pubblica, che verrà utilizzata per la connessione all'istanza del server virtuale.

   Potresti avere già una chiave SSH pubblica. Ricerca un file denominato `id_rsa.pub` in una directory `.ssh` all'interno della tua directory home, ad esempio `/Users/<USERNAME>/.ssh/id_rsa.pub`. Il file inizia con `ssh-rsa` e termina con il tuo indirizzo email.

   Se non hai una chiave SSH pubblica o se hai dimenticato la password di una chiave esistente, generane una nuova eseguendo il comando `ssh-keygen` e seguendo le istruzioni.
2.  Assicurati di avere una chiave API per il tuo account IBM Cloud. Se non hai una chiave API, vedi [Creazione di una chiave API](/docs/iam?topic=iam-userapikey#create_user_key). Dovrai memorizzare questa chiave API in una variabile di ambiente nel Passo 1.

## Passo 1: memorizza la tua chiave API come variabile

Immetti il seguente comando per memorizzare la tua chiave API in una variabile di ambiente:

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## Passo 2: ottieni un token IBM Identity and Access Management (IAM)

Fai riferimento all'argomento [Ottenimento di un token IAM IBM Cloud utilizzando una chiave API](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) per informazioni su come ottenere un token IAM o utilizza i seguenti comandi di esempio.

Immetti il seguente comando per ottenere e analizzare un token IAM utilizzando il programma di utilità `jq`. Puoi modificare il comando per utilizzare un altro strumento di analisi o puoi rimuovere l'ultima parte del comando se preferisci analizzare il token manualmente.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

Per visualizzare il token IAM, esegui `echo $iam_token`. Il risultato dovrebbe essere simile a questo:

```
Bearer <your token>
```

L'intestazione dell'autorizzazione prevede che il token inizi con `Bearer`. Se il risultato che ricevi non include `Bearer`, aggiorna la variabile `iam_token` per includerlo. In questi esempi si presuppone che `Bearer` sia incluso in `iam_token`.

Poiché il token scade, devi ripetere il passo precedente per aggiornare il token IAM ogni ora.
{: important}

## Passo 3: memorizza l'endpoint API e il parametro di versione come variabile

Gli endpoint API variano in base alla regione e seguono la convenzione `https://<region>.iaas.cloud.ibm.com`. Gli endpoint per regione sono:

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

Memorizza l'endpoint API in una variabile in modo da poterla utilizzare nelle richieste API senza dover digitare l'URL completo. Ad esempio, per memorizzare l'endpoint us-south in una variabile, esegui questa comando:

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

Per verificare che questa variabile sia stata salvata, esegui `echo $rias_endpoint` e assicurati che la risposta non sia vuota.

Ogni richiesta API deve includere il parametro `version`, nel formato `YYYY-MM-DD`. Immetti il seguente comando per memorizzare la data della versione in una variabile in modo che possa essere riutilizzata nella tua sessione. Per ulteriori informazioni sull'impostazione del parametro `version`, vedi **Versioning** nella [Guida di riferimento API per VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

Per verificare che questa variabile sia stata salvata, esegui `echo $version` e assicurati che la risposta non sia vuota.

## Passo 4: esegui l'API GET regions

Il seguente comando restituisce le regioni disponibili per VPC, in formato JSON. Dovrebbe essere restituito almeno un oggetto.

È richiesto un parametro di query `version` e `generation` in ogni richiesta API. Per i dettagli, vedi la [Guida di riferimento API per VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Se rilevi dei risultati imprevisti, aggiungi l'indicatore `--verbose` (debug) dopo il comando `curl` per ottenere informazioni di registrazione dettagliate. Vedi anche gli errori più comuni nella nostra [pagina di risoluzione dei problemi](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Passo 5: esegui l'API GET zones

Il seguente comando restituisce tutte le zone disponibili per VPC nella regione `us-south`, in formato JSON. Dovrebbero essere restituiti almeno tre oggetti.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Aggiorna il nome della regione nel parametro, `us-south`, a seconda dell'endpoint della regione che stai utilizzando. Un endpoint della regione conosce
solo le proprie zone, quindi se stati utilizzando l'endpoint a TOKYO, utilizza `jp-tok`.
{: note}

## Passo 6: esegui l'API GET profiles

Il seguente comando restituisce i profili disponibili per le tue istanze, in formato JSON. Dovrebbe essere restituito almeno un oggetto.

Aggiungi ` | json_pp ` dopo il comando curl per ottenere una stringa JSON leggibile
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Se l'output dell'API è limitato dall'impaginazione, imposta un limite superiore a `total_count`, ad esempio:

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Passo 7: esegui l'API GET images

Il seguente comando restituisce le immagini disponibili per le tue istanze, in formato JSON. Dovrebbe essere restituito almeno un oggetto.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Passo 8: esegui l'API GET VPCs

Il seguente comando restituisce gli eventuali VPC creati nel tuo account, in formato JSON.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Passo 9: crea un VPC (virtual private cloud)

Crea un VPC {{site.data.keyword.cloud_notm}} chiamato `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

Per il resto delle chiamate, dovrai conoscere l'ID del VPC {{site.data.keyword.cloud_notm}} appena creato. Memorizza il suo ID in una variabile. Il comando sarà simile a questo: `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## Passo 10: crea una sottorete

Crea una sottorete nel tuo {{site.data.keyword.cloud_notm}} VPC. Il seguente esempio crea un VPC nella zona `us-south-2`.

Il seguente esempio utilizza il [prefisso di indirizzo predefinito](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) per la zona `us-south-2`. Le impostazioni predefinite potrebbero essere state modificate per il tuo account; vedi [Come pianificare i tuoi indirizzi VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

Per ottenere un elenco di prefissi di indirizzo per il tuo VPC, immetti il seguente comando
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

Memorizza l'ID della sottorete in una variabile.

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## Passo 11: controlla lo stato della tua sottorete

Per eseguire il provisioning delle risorse nella tua sottorete, è necessario che lo stato della sottorete sia `available`. Prima di continuare, esegui una query sulla risorsa della sottorete e assicurati che lo stato sia `available`. Se lo stato è `failed`, contatta il [Supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) fornendo i dettagli. Puoi provare a continuare tentando di eseguire il provisioning di un'altra sottorete.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Passo 12: crea un gateway pubblico

Per consentire alle istanze del server virtuale nella sottorete di accedere all'Internet pubblico, aggiungi un gateway pubblico (PGW) alla sottorete.  

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

Memorizza l'ID del gateway pubblico in una variabile.

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

Per collegare e utilizzare il gateway pubblico, è necessario che il suo stato sia `available`. Prima di continuare, esegui una query sulla risorsa del gateway pubblico e assicurati che lo stato sia `available`. Se lo stato è `failed`, contatta il [Supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) fornendo i dettagli. Puoi provare a continuare tentando di eseguire il provisioning di un altro gateway pubblico.

Per controllare lo stato del gateway pubblico, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Passo 13: collega il gateway pubblico alla sottorete

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Passo 14: crea una chiave SSH

Crea una chiave con la tua chiave SSH pubblica. Questa chiave viene utilizzata quando crei l'istanza del server virtuale. La chiave è anche necessaria per accedere all'istanza del server virtuale.

Le chiavi possono essere aggiunte quando crei per la prima volta il VPC nell'IU o nella CLI. Non esistono strumenti per aggiungere le chiavi in un secondo momento.
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

Memorizza l'ID della chiave in una variabile.

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## Passo 15: scegli un profilo e un'immagine per la tua istanza del server virtuale

Esegui le API per elencare tutti i profili e le immagini disponibili per la tua istanza del server virtuale e scegli una combinazione.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Per ottenere ulteriori dettagli sui profili, puoi eseguire una query sul catalogo globale utilizzando il comando della CLI {{site.data.keyword.cloud_notm}} `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`. Ad esempio:

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

Il seguente comando elenca le immagini disponibili.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Memorizza il nome del profilo e l'ID immagine nelle variabili, che utilizzerai per eseguire il provisioning di un'istanza del server virtuale.

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## Passo 16: esegui il provisioning di un'istanza del server virtuale

Esegui il provisioning di un'istanza del server virtuale (VSI) nella sottorete appena creata. Passa la tua chiave SSH pubblica in modo da poter accedere dopo aver eseguito il provisioning dell'istanza.

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

Memorizza l'ID dell'istanza del server virtuale in una variabile.

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## Passo 17: controlla lo stato della tua istanza del server virtuale

Il provisioning di un'istanza del server virtuale può richiedere alcuni minuti. Prima di continuare, esegui una query sullo stato del server e assicurati che sia `running`.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Memorizza l'ID dell'interfaccia di rete primaria dell'istanza del server virtuale (restituita nella chiamata API precedente) in una variabile.  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

Non puoi ottenere l'interfaccia di rete primaria finché non esegui una query sull'istanza specifica.
{: note}

## Passo 18: crea un IP mobile

Per creare un IP mobile per l'istanza del server virtuale, utilizza l'interfaccia di rete primaria dell'istanza come destinazione per il nuovo indirizzo IP mobile.

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


Memorizza l'ID dell'IP mobile in una variabile.

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## Passo 19: aggiungi una regola al gruppo di sicurezza predefinito per consentire il traffico SSH

Configura il gruppo di sicurezza per definire il traffico in entrata e in uscita consentito per l'istanza.

Trova il gruppo di sicurezza per il VPC:

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Memorizza l'ID del gruppo di sicurezza in una variabile.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Adesso, crea una regola per consentire il traffico SSH in entrata:

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

## Passo 20: esegui l'SSH nella tua istanza del server virtuale

Per eseguire l'SSH nel server, utilizza l'`address` dell'IP mobile che hai creato in precedenza. Per ottenere l'`address` dell'IP mobile, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Utilizza l'`address` dell'IP mobile per connetterti all'istanza del server virtuale con SSH:

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

L'accesso SSH al server virtuale potrebbe essere impedito dai gruppi di sicurezza. Se è richiesto l'accesso SSH, devi utilizzare un [gruppo di sicurezza appropriato](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).
{: note}

Vedi [Connessione alla tua istanza utilizzando Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) per ulteriori informazioni su come connetterti alla tua istanza.

## Passo 21: crea e collega un volume di archiviazione blocchi

Crea un volume di archiviazione blocchi con una richiesta simile a questo esempio e specifica un nome, una zona e un profilo del volume.

Per visualizzare un elenco di profili del volume, fornisci questa richiesta:

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

I profili possono essere di utilizzo generico (3 IOPS/GB), di livello 5iops, di livello 10iops e personalizzati. Vedi [Informazioni su Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
per informazioni sulla capacità e sugli intervalli IOPS del volume in base al profilo del volume che hai selezionato.

Non devi fornire un nome di volume nella richiesta poiché il sistema ne crea uno per impostazione predefinita.  Questo esempio specifica un nome di volume, `helloworld-vol`.  Tutti i nomi di volume devono essere univoci.

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

Per il resto delle chiamate, dovrai conoscere l'ID del volume appena creato. Salva l'ID del volume come variabile, ad esempio `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

Lo stato del volume è `pending` quando viene creato per la prima volta. Prima di poter procedere, il volume deve passare allo stato `available`, il che richiede alcuni minuti.
{: note}


Per controllare lo stato del volume, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Crea un collegamento del volume per collegare il nuovo volume all'istanza del server virtuale. Nella richiesta, utilizza la variabile dell'ID istanza che hai creato in precedenza.  Utilizza l'ID volume, il CRN del volume o l'URL per specificare il volume nel parametro di dati.  Questo esempio utilizza la variabile che abbiamo creato per l'ID volume.

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

Dopo aver creato il collegamento del volume, richiama il suo ID.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Salva l'ID del collegamento come variabile.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

Specifica l'ID del collegamento del volume per collegare il nuovo volume a un'istanza del server virtuale.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Passo 22 (facoltativo): elimina le risorse

Facoltativamente, elimina le risorse. Non è possibile eliminare una risorsa se contiene altre risorse. Ad esempio, non è possibile eliminare un VPC (Virtual Private Cloud) se contiene delle sottoreti e non è possibile eliminare una sottorete se contiene istanze del server virtuale. In un'operazione di eliminazione, l'API può essere restituita rapidamente ma l'eliminazione della risorsa potrebbe ancora essere in corso. Dopo aver emesso la richiesta di eliminazione, assicurati che la risorsa sia stata eliminata prima di tentare di eliminare la risorsa principale. Per ulteriori dettagli, vedi [Eliminazione di un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

### Elimina l'IP mobile

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Elimina l'istanza del server virtuale

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Elimina la chiave

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Elimina la sottorete

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Elimina il gateway pubblico

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### Elimina il VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Elimina il volume

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## Congratulazioni!

Hai eseguito correttamente il provisioning di un'istanza VPC (Virtual Private Cloud) utilizzando le API REST. Per provare ulteriori comandi API, esplora il riferimento completo:

* [Guida di riferimento API per VPC](https://{DomainName}/apidocs/vpc-on-classic)
