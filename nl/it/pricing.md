---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# Prezzi per VPC
{: #pricing-for-vpc}

La tabella riepiloga i prezzi per il trasferimento dati su Internet con {{site.data.keyword.cloud}} Virtual Private Cloud. Ricorda che non vi è alcun costo per il traffico tra VPC e altri servizi IBM Cloud classici. Inoltre, per i servizi PayGo, i livelli di servizio sono associati al tuo account, non a un'istanza VPC specifica. Gli importi sono indicati in dollari statunitensi.

Si applicano dei prezzi separati per le [istanze del server virtuale](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) e l'[archiviazione blocchi](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) utilizzate nel tuo IBM Cloud VPC.

## Franchigie per il trasferimento dati su Internet
{: #free-allowances-for-internet-data-transfer}

| Trasferimento dati |  Costo per tutti i clienti di IBM Cloud VPC |
|---------------|------------------|
| All'interno della zona | Gratuito |
| Tra zone nella stessa regione | Gratuito |
| Utilizzo del gateway pubblico | Gratuito (addebitato solo per l'IP mobile utilizzato da PGW) |

## Prezzi per l'IP mobile
{: #floating-ip-pricing}

Un IP mobile viene addebitato alla tariffa di $1 (USA) al mese, a partire dalla data di prenotazione. La tariffa viene addebitata anche se l'IP mobile non è associato a una VSI o non è in uso. Il costo di $1 per la tariffa mensile viene addebitato anche se l'IP mobile è riservato solo per pochi giorni.

## Prezzi PayGo di base per il trasferimento dati su Internet
{: #basic-paygo-pricing-for-internet-transfer}

| Trasferimento dati | Quantità di dati | Prezzi PayGo |
|-----------|-----------|------------------|
| In uscita su Internet |  Da 0 a 5 GB | Gratuito |
|  | Da 6 a 10.000 GB | $0,087 per GB |
|  | Da 10.001 a 50.000 GB | $0,083 per GB |
|  | Da 50.001 a 150.000 GB | $0,07 per GB |
|  | 150.001 GB e oltre | $0,05 per GB |


Quando crei un nuovo VPC, potrebbe essere necessario attendere fino a un'ora per visualizzare i costi di fatturazione iniziali nell'API o nell'IU della console.
{: tip}

Se disponi di un gateway pubblico o di un IP mobile, potresti comunque vedere alcuni addebiti in uscita minimi, anche se non hai inviato alcun traffico in uscita durante tale periodo. Questi addebiti sono per il traffico ARP, che è necessario per gestire il tuo account.
{: important}

## Prezzi per LBaaS for VPC
{: #lb-for-vpc-pricing}

I prezzi per Load Balancers for VPC si basano sulle seguenti metriche, calcolate mensilmente:
* Ore di utilizzo del servizio
* Dati elaborati


| Regione | Utilizzo del servizio LB (all'ora) | Dati LB elaborati per GigaByte (GB) |
|------------|--------------------------|-------------------------|
| Dallas | $0,025 | $0,008 |
| Francoforte | $0,027 | $0,009 |
| Tokyo | $0,028 | $0,009 |

I prezzi variano in base alle regioni multizona.
{: note}

Il seguente grafico mostra un esempio per un cliente che utilizza 4500 GB al mese per il bilanciamento del carico pubblico, in dollari USA, con sede nella regione di Dallas.

| Elemento | Utilizzo | Tariffa | Costo |
|---------|--------|---------|---------|          
| Utilizzo del servizio LB | 720 ore| $0,025/ora | $18 al mese |
| Dati elaborati| 4500 GB  |  $0,008/GB | $36 al mese|

**Tabella 1: esempio di costo mensile. Il costo totale per questo scenario è di $54 (USD) al mese.**


## Prezzi per VPN for VPC
{: #vpn-for-vpc-pricing}

| Regione | Connessione (peer) all'ora | Istanza (gateway) all'ora |
|------------|--------------------------|-------------------------|
| Dallas | $0,04 | $0,045 |
| Francoforte | $0,044 | $0,0495 |
| Tokyo | $0,0452 | $0,0585 |

Il trasferimento di dati su Internet a seguito dell'utilizzo di VPNaaS viene addebitato come trasferimento dati regolare, in uscita su Internet, fatturato a livello di VPC.
{: note}
