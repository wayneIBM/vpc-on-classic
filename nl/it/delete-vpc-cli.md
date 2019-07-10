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

# Eliminazione di un VPC utilizzando la CLI IBM Cloud
{: #deleting-using-cli}

Questo argomento fornisce esempi su come eliminare le risorse dal tuo {{site.data.keyword.cloud}} Virtual Private Cloud, per ogni risorsa VPC, nell'ordine suggerito. 

Ecco alcuni aspetti chiave da ricordare in merito all'eliminazione:

* Un VPC non può essere eliminato finché non è vuoto. 
* È necessario eliminare correttamente tutte le risorse contenenti prima di poter eliminare la risorsa principale. 
* Se la risorsa è in attesa di eliminazione, mostra uno stato `deleting` quando viene visualizzata nelle query di elenco. 
* La maggior parte delle richieste di eliminazione sono _asincrone_, il che significa che la risorsa potrebbe mostrare uno stato **deleting** nelle query di elenco quando non è stata ancora eliminata completamente. 
* Devi eseguire il polling per assicurarti che la risorsa secondaria non sia più nella vista elenco prima di tentare di eliminare la risorsa principale. 

Se lo stato della risorsa cambia da `deleting` a `failed`, puoi provare a eliminare di nuovo la risorsa. Se non puoi eliminare una risorsa in stato `failed`, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Passo 1: trova tutte le sottoreti all'interno del VPC che vuoi eliminare
{: #deleting-find-subnets-cli}

Prima di poter eliminare un VPC, è necessario eliminare ciascuna delle sue sottoreti. Ottieni l'ID del VPC che vuoi eliminare immettendo il seguente comando e controllando il valore `ID`:

```
ibmcloud is vpcs
```
{: pre}

Salva l'ID del VPC in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Per ottenere l'elenco di tutte le sottoreti nel tuo account, immetti il seguente comando:

```
ibmcloud is subnets
```
{: pre}

Controlla il valore `VPC` per determinare le sottoreti che devono essere eliminate. Salva l'ID della sottorete in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Se il VPC che vuoi eliminare ha più sottoreti, ripeti questi passi per eliminare ogni sottorete.
{: important}

## Passo 2: elimina ogni sottorete nel VPC
{: #deleting-subnet-resources-cli}

### Elimina tutti i gateway VPN nella sottorete, se presenti

Per elencare tutti i gateway VPN nel tuo account, immetti il seguente comando:

```
ibmcloud is vpn-gateways
```
{: pre}

Per ogni gateway VPN nella sottorete che vuoi eliminare, immetti il seguente comando, dove `$vpnid` è l'ID del gateway VPN.

```
ibmcloud is vpn-gateway-delete $vpnid
```

Lo stato del gateway VPN dovrebbe cambiare immediatamente in `deleting`, ma verrà comunque visualizzato nel risultato di una query di elenco. L'eliminazione di un gateway VPN può richiedere fino a 30 minuti.
{: note}

Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione del gateway VPN. Tuttavia, la sottorete non può essere eliminata finché il gateway VPN e tutte le altre risorse nella sottorete non vengono più visualizzati nei risultati della query di elenco.

### Elimina tutti i programmi di bilanciamento del carico nella sottorete, se presenti
{: #deleting-lbs-cli}

Per elencare tutti i programmi di bilanciamento del carico nel tuo account, immetti il seguente comando:

```
ibmcloud is load-balancers
```
{: pre}

Per ogni programma di bilanciamento del carico nella sottorete che vuoi eliminare, immetti il seguente comando, dove `$lbid` è l'ID del programma di bilanciamento del carico.

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

Lo stato del programma di bilanciamento del carico dovrebbe cambiare immediatamente in `deleting`, ma verrà comunque mostrato nel risultato di una query di elenco. L'eliminazione di un programma di bilanciamento del carico può richiedere fino a 30 minuti.
{: note}

Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione del programma di bilanciamento del carico. Tuttavia, la sottorete non può essere eliminata finché il programma di bilanciamento del carico e tutte le altre risorse nella sottorete non vengono più visualizzati nei risultati della query di elenco.

### Elimina tutte le interfacce di rete nella sottorete, se presenti
{: #deleting-nics-cli}

Un'istanza può avere più interfacce di rete e tali interfacce di rete possono appartenere a più sottoreti del VPC. Prima di poter eliminare una sottorete, è necessario eliminare tutte le interfacce di rete nella sottorete.  

L'unico modo per eliminare un'interfaccia di rete in una sottorete è eliminare l'istanza.
{: note}

Per elencare tutte le istanze nel tuo account, immetti il seguente comando:

```
ibmcloud is instances
```
{: pre}

Se elimini tutte le istanze nel VPC, puoi immettere il seguente comando per ciascuna istanza nel VPC, dove `$vsi` è l'ID dell'istanza che vuoi eliminare. Quando l'istanza viene eliminata, tutte le interfacce di rete in altre sottoreti, se presenti, verranno eliminate automaticamente.

```
ibmcloud is instance-delete $vsi
```
{: pre}

Lo stato dell'istanza dovrebbe cambiare immediatamente in `deleting`, ma verrà comunque visualizzata nel risultato di una query di elenco. L'eliminazione di un'istanza può richiedere fino a 30 minuti.
{: note}

Puoi richiedere che altre risorse della sottorete vengano eliminate in parallelo mentre attendi l'eliminazione dell'istanza. Tuttavia, la sottorete non può essere eliminata finché l'istanza e tutte le altre risorse nella sottorete non vengono più visualizzate nei risultati della query di elenco.

Per elencare tutte le interfacce di rete di un'istanza, immetti il seguente comando:

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

Se esiste un'interfaccia di rete secondaria all'interno della sottorete che stai tentando di eliminare, devi eliminare l'istanza. Non è possibile eliminare un'interfaccia di rete dall'istanza senza eliminare l'istanza.

### Elimina la sottorete
{: #deleting-subnet-cli}

Una volta che tutte le risorse all'interno della sottorete sono state eliminate, il che significa che non vengono restituite nel risultato di una query di elenco, immetti il seguente comando per eliminare la sottorete:

```
ibmcloud is subnet-delete $subnet
```
{: pre}

Lo stato della sottorete dovrebbe cambiare immediatamente in `deleting`, ma potrebbero essere richiesti alcuni minuti affinché la sottorete venga effettivamente eliminata e non venga più visualizzata nei risultati della query di elenco.

## Passo 3: elimina tutti i gateway pubblici nel VPC, se presenti
{: #deleting-pgs-cli}

Per elencare tutti i gateway pubblici nel tuo account, immetti il seguente comando:

```
ibmcloud is public-gateways
```
{: pre}

Per ogni gateway pubblico nel VPC che vuoi eliminare, immetti il seguente comando per eliminare il gateway pubblico, dove `$gateway` è l'ID del gateway pubblico.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Passo 4: elimina il VPC
{: #deleting-single-vpc-cli}

Una volta che tutte le sottoreti, i gateway VPN, i programmi di bilanciamento del carico e i gateway pubblici nel VPC sono stati eliminati, immetti il seguente comando per eliminare il VPC, dove `$vpc` è l'ID del VPC che stai eliminando.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

Lo stato del VPC dovrebbe cambiare immediatamente in `deleting`, ma potrebbero essere richiesti alcuni minuti affinché il VPC venga effettivamente eliminato e non venga più visualizzato nei risultati della query di elenco.
