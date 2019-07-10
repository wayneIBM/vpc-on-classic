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

# Eliminazione di un VPC utilizzando le API REST
{: #deleting-using-api}

L'eliminazione di un {{site.data.keyword.cloud}} Virtual Private Cloud utilizzando le API REST segue gli stessi passi generali
nel processo di [eliminazione](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) di quelli utilizzati per l'eliminazione tramite la [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli).

Di seguito sono riportati i passi principali del processo:

1. Trova tutte le sottoreti nel VPC che vuoi eliminare.
2. Elimina ogni sottorete nel VPC, il che significa:
    - Elimina tutti i gateway VPN nella sottorete.
    - Elimina tutti i programmi di bilanciamento del carico nella sottorete.
    - Elimina tutte le interfacce di rete (istanze) nella sottorete.
    - Elimina la sottorete.
3. Elimina tutti i gateway pubblici nel VPC.
4. Elimina il VPC.

Le seguenti sezioni forniscono alcune chiamate API di esempio, utilizzando `curl`, che puoi eseguire per eliminare un VPC.

## Passo 1: trova tutte le sottoreti nel VPC che vuoi eliminare
{: #deleting-find-subnets-api}

Prima di poter eliminare un VPC, è necessario eliminare ciascuna delle sue sottoreti. Ottieni l'ID del VPC che vuoi eliminare immettendo il seguente comando e controllando il valore `id`:

Aggiungi ` | json_pp ` o ` | jq ` dopo il comando curl per ottenere una stringa JSON leggibile.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Salva l'ID del VPC in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Per ottenere l'elenco di tutte le sottoreti nel tuo account, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Controlla il valore `vpc` per determinare le sottoreti che devono essere eliminate. Salva l'ID della sottorete in una variabile per un utilizzo successivo, ad esempio:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Se il VPC che vuoi eliminare ha più sottoreti, ripeti i passi per eliminare ogni sottorete.
{: important}

## Passo 2: elimina ogni sottorete nel VPC
{: #deleting-subnet-resources-api}

### Elimina tutti i gateway VPN nella sottorete, se presenti

Per elencare tutti i gateway VPN nel tuo account, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Per ogni gateway VPN nella sottorete che vuoi eliminare, immetti il seguente comando, dove `$vpnid` è l'ID del gateway VPN.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Lo stato del gateway VPN dovrebbe cambiare immediatamente in `deleting`, ma verrà comunque restituito quando si esegue una query di elenco. L'eliminazione di un gateway VPN può richiedere fino a 30 minuti. Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione del gateway VPN, ma la sottorete non può essere eliminata finché il gateway VPN e tutte le altre risorse nella sottorete non vengono più visualizzati nelle query di elenco.

### Elimina tutti i programmi di bilanciamento del carico nella sottorete, se presenti
{: #deleting-lbs-api}

Per elencare tutti i programmi di bilanciamento del carico nel tuo account, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Per ogni programma di bilanciamento del carico nella sottorete che vuoi eliminare, immetti il seguente comando, dove `$lbid` è l'ID del programma di bilanciamento del carico.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Lo stato del programma di bilanciamento del carico dovrebbe cambiare immediatamente in `deleting`, ma verrà comunque restituito quando si esegue una query di elenco. L'eliminazione di un programma di bilanciamento del carico può richiedere fino a 30 minuti. Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione del programma di bilanciamento del carico, ma la sottorete non può essere eliminata finché il programma di bilanciamento del carico e tutte le altre risorse nella sottorete non vengono più visualizzati nelle query di elenco.

### Elimina tutte le interfacce di rete nella sottorete, se presenti
{: #deleting-nics-api}

Un'istanza può avere più interfacce di rete e tali interfacce di rete possono appartenere a più sottoreti del VPC. Prima di poter eliminare una sottorete, è necessario eliminare tutte le interfacce di rete presenti nella sottorete. L'unico modo per eliminare un'interfaccia di rete in una sottorete è eliminare l'istanza.

Per elencare tutte le istanze nel tuo account, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Se elimini tutte le istanze nel VPC, puoi immettere il seguente comando per ciascuna istanza nel VPC, dove `$vsi` è l'ID dell'istanza che vuoi eliminare. Quando l'istanza viene eliminata, tutte le interfacce di rete in altre sottoreti, se presenti, verranno eliminate automaticamente.

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Lo stato dell'istanza dovrebbe cambiare immediatamente in `deleting`, ma può ancora essere visualizzata nei risultati di una query di elenco. L'eliminazione di un'istanza può richiedere fino a 30 minuti. Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione dell'istanza, ma la sottorete non può essere eliminata finché l'istanza e tutte le altre risorse nella sottorete non vengono più visualizzate nelle query di elenco.

Per elencare tutte le interfacce di rete di un'istanza, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Se esiste un'interfaccia di rete secondaria nella sottorete che stai tentando di eliminare, devi eliminare l'istanza. Non è possibile eliminare un'interfaccia di rete dall'istanza senza eliminare l'istanza.

### Elimina la sottorete
{: #deleting-subnet-api}

Una volta che tutte le risorse all'interno della sottorete sono state eliminate e non vengono restituite quando si esegue una query di elenco, immetti il seguente comando per eliminare la sottorete:

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Lo stato della sottorete dovrebbe cambiare immediatamente in `deleting`, ma potrebbero essere richiesti alcuni minuti per eliminare la sottorete, in modo che non venga più restituita nelle query di elenco.

## Passo 3: elimina tutti i gateway pubblici nel VPC, se presenti
{: #deleting-pgs-api}

Per elencare tutti i gateway pubblici nel tuo account, immetti il seguente comando:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Per ogni gateway pubblico nel VPC che vuoi eliminare, immetti il seguente comando per eliminare il gateway pubblico, dove `$gateway` è l'ID del gateway pubblico.

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## Passo 4: elimina il VPC
{: #deleting-single-vpc-api}

Una volta che tutte le sottoreti, i gateway VPN, i programmi di bilanciamento del carico e i gateway pubblici nel VPC sono stati eliminati, immetti il seguente comando per eliminare il VPC, dove `$vpc` è l'ID del VPC che stai eliminando.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Lo stato del VPC dovrebbe cambiare immediatamente in `deleting`, ma potrebbero essere richiesti alcuni minuti per eliminare il VPC, in modo che non venga più visualizzato nelle query di elenco.
