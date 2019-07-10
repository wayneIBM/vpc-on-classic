---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Prezzi per Block Storage for VPC
{: #block-storage-pricing}

I prezzi per {{site.data.keyword.block_storage_is_short}} si basano sulla capacità o sull'IOPS di cui è stato eseguito il provisioning, a seconda del profilo di archiviazione che hai scelto.

| Livello IOPS  | Tariffa mensile pubblicata^1 |
|------------|--------------|
|  3 IOPS/GB |  0,13 USD/GB |
|  5 IOPS/GB |  0,25 USD/GB |
| 10 IOPS/GB |  0,58 USD/GB |
| IOPS personalizzato| 0,10 USD/GB + 0,07 USD/IOPS |

^1 Prezzo pubblicato al mese, [calcolato su base oraria](#how-are-charges-calculated).

## Come vengono calcolati gli addebiti?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} viene calcolato ogni ora, in base al numero totale di ore in cui il volume di archiviazione blocchi è presente nell'account, finché il volume non viene eliminato o fino alla fine di un ciclo di fatturazione, a seconda dell'evento che si verifica per primo. La fatturazione oraria viene calcolata in modo diverso per un livello IOPS predefinito rispetto all'IOPS personalizzato. Vedi i seguenti esempi.

### Calcolo della fatturazione per un volume con il livello IOPS di utilizzo generico
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

Esegui il provisioning di un volume da 1000 GB con il livello 3 IOPS/GB di utilizzo generico, quindi utilizzi il volume per 72 ore prima di eliminarlo. Il prezzo totale per il volume viene fatturato all'ora, nel seguente modo:

((1000 GB x 0,13 USD/GB)/730 ore) x 72 ore = $12,82

### Calcolo della fatturazione per un volume con IOPS personalizzato
{: #billing-calculation-for-a-volume-with-custom-IOPS}

Esegui il provisioning di un volume da 1000 GB con 2500 IOPS, quindi utilizzi il volume per 72 ore prima di eliminarlo. Il prezzo totale per il volume viene fatturato all'ora, nel seguente modo:

(((1000 x 0,10 USD/GB) + (2500 x 0,07 USD)) / 730 ore) x 72 ore = $27,12
