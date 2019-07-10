---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

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

# Creazione e gestione dell'archiviazione blocchi in VPC
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud fornisce un volume di archiviazione di boot e consente ulteriori volumi di archiviazione blocchi, che sono archiviazioni di dati ad alte prestazioni montate su hypervisor per le tue istanze del server virtuale (VSI). L'infrastruttura VPC fornisce un rapido ridimensionamento dell'archiviazione in più regioni e zone, con prestazioni e sicurezza massime.

## Riepilogo delle caratteristiche
{: #summary-of-characteristics}

Quando esegui il provisioning di un'istanza {{site.data.keyword.vsi_is_full}}, un volume di archiviazione blocchi IOPS (3 IOPS/GB) di utilizzo generico da 100 GB viene creato automaticamente come volume di boot principale e viene collegato alla VSI.
Durante il provisioning, puoi ridenominare il volume di boot e modificare la dimensione del volume.
{:shortdesc}

* Puoi creare i volumi di archiviazione blocchi ogni volta che esegui il provisioning di un'istanza del server virtuale in una rete VPC.  
* Puoi creare nuovi volumi, indipendenti dal ciclo di vita della VSI, e in seguito collegare questi volumi a una VSI.
* Puoi creare volumi di dati secondari, che vengono collegati automaticamente alla VSI. 
* I volumi di dati persistono quando vengono scollegati dalla VSI. Pertanto, puoi collegare un volume a una nuova istanza in un secondo momento. 
* Puoi specificare se i volumi di dati vengono eliminati automaticamente quando elimini una VSI.  
* I volumi di boot vengono eliminati quando elimini la loro VSI.
* Per impostazione predefinita, i volumi di boot e di dati sono crittografati con la crittografia gestita da IBM. 
* Puoi crittografare i volumi utilizzando le tue chiavi di crittografia, durante il provisioning della VSI o durante la creazione di un volume autonomo.


## Volumi di archiviazione blocchi
{: #block-storage-volumes}

Informazioni più complete sull'utilizzo dei volumi di archiviazione blocchi e del VPC sono disponibili nella nostra [documentazione Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started).

Per informazioni generali sui volumi di archiviazione blocchi per il VPC, vedi [Informazioni su Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

Per iniziare a creare volumi indipendenti dal provisioning della VSI, vedi [Introduzione a {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started).



## Procedure ottimali per la creazione e la denominazione dei volumi di archiviazione blocchi del VPC:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Stabilisci i tuoi requisiti di archiviazione prima di eseguire il provisioning di un volume. Consenti capacità e prestazioni IOPS adeguate.
* Decidi se un profilo IOPS predefinito soddisfa meglio le tue esigenze di capacità e prestazioni o se è preferibile specificare delle impostazioni personalizzate.
* Decidi se vuoi creare volumi di dati secondari durante il provisioning dell'istanza del server virtuale. Questi volumi vengono collegati automaticamente all'istanza.
* Decidi se vuoi che il volume collegato a una VSI venga eliminato automaticamente quando elimini la VSI.
* Quando crittografi i volumi con la tua propria chiave di crittografia, assicurati che la chiave sia valida, che tu disponga dell'autorizzazione per utilizzare la chiave e che possa essere utilizzata nella tua regione corrente.
* Assicurati di dare un nome a TUTTI i tuoi volumi al momento del provisioning, incluso il volume di boot.
* Assicurati di dare un nome a TUTTI i collegamenti del volume al momento del collegamento.
* Ogni volume deve avere un nome distinto all'interno di una regione all'interno di un account.

Puoi riutilizzare il nome di un volume dopo che il volume è stato eliminato. Tuttavia, tieni presente che riutilizzare un nome di volume potrebbe causare confusione a fini di fatturazione.
{:note}
