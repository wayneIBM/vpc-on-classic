---

copyright:
  years: 2019
lastupdated: "2019-05-29"


keywords: vpc, delete, resources

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

# Eliminazione di un VPC
{: #deleting}

Non è possibile eliminare una risorsa {{site.data.keyword.cloud}} Virtual Private Cloud se contiene altre risorse o se è collegata a un'altra risorsa. Questo documento descrive le relazioni tra le risorse VPC e fornisce informazioni sulle procedure ottimali per l'eliminazione di un VPC.

La seguente tabella riepiloga i tipi di risorse VPC e le relazioni da considerare quando si eliminano tali risorse.

| Risorsa | Prima dell'eliminazione... | Dopo l'eliminazione... |
| ---------------- | ----------------------------------------- | --------------------------- |
| VPC | È necessario eliminare tutte le sottoreti e i tutti i gateway pubblici nel VPC | Tutti i gruppi di sicurezza e i prefissi di indirizzo vengono eliminati automaticamente |
| Sottorete | È necessario eliminare tutte le istanze e tutte le interfacce di rete nella sottorete; è necessario eliminare tutti i gateway VPN e i programmi di bilanciamento del carico nella sottorete | Qualsiasi gateway pubblico che serve la sottorete verrà scollegato; qualsiasi ACL di rete associato alla sottorete verrà scollegato |
| Istanza | ---- | Tutte le interfacce di rete vengono eliminate automaticamente; il volume di boot viene eliminato con l'istanza; l'eliminazione del volume secondario dipende dal parametro `delete_volume_on_instance_delete`; qualsiasi IP mobile collegato a una delle interfacce di rete viene scollegato automaticamente |
| Interfaccia di rete | ---- | Qualsiasi IP mobile collegato all'interfaccia di rete viene rilasciato |
| Chiave | ---- | Dopo aver eliminato una chiave, non può più essere utilizzata per eseguire il provisioning di una nuova istanza o per eseguire un ricaricamento del sistema operativo su un'istanza esistente. Tuttavia, la chiave è ancora disponibile in tutte le istanze in cui ne hai eseguito il provisioning e puoi continuare a utilizzarla per effettuare l'accesso.  |
| Immagine | ---- | L'immagine non può essere utilizzata per eseguire il provisioning di una nuova istanza, ma le istanze esistenti con l'immagine non vengono influenzate. |
| Volume | Il volume deve essere scollegato da tutte le istanze | ---- |
| ACL di rete | L'ACL di rete deve essere scollegato da tutte le sottoreti; gli ACL di rete predefiniti per un VPC non possono essere eliminati  | ---- |
| Gruppo di sicurezza | Il gruppo di sicurezza deve essere scollegato da tutte le interfacce di rete; il gruppo di sicurezza predefinito per un VPC non può essere eliminato. | ---- |
| IP mobile | L'IP mobile deve essere scollegato da qualsiasi interfaccia di rete | ---- |
| Gateway pubblico | Il gateway pubblico deve essere scollegato da tutte le sottoreti |  ---- |
| Programma di bilanciamento del carico | ---- | ---- |
| Gateway VPN | ---- | Per via della possibilità di condividere le politiche IKE e IPsec tra i gateway, queste non verranno eliminate quando si elimina un gateway VPN. Le politiche IKE e IPsec devono essere rimosse manualmente. |


## Le risorse VPC non possono essere eliminate in uno stato temporaneo
{: #deleting-status}

Le risorse VPC non possono essere eliminate quando si trovano in uno stato temporaneo, come ad esempio `pending`. Tutte le risorse devono essere in uno stato "finale", come `available` o `failed`, prima che possano essere eliminate. Devi attendere che la risorsa raggiunga lo stato `available` o `failed` prima di tentare di eliminarla. [Contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se una risorsa non mostra uno stato `available` o `failed` entro 30 minuti.

Dopo aver emesso una richiesta per eliminare una risorsa, la risorsa entra nello stato temporaneo `deleting`. Attendi che la risorsa scompaia dall'elenco prima di tentare di eliminare la risorsa principale o collegata. In alcune situazioni, l'operazione di eliminazione potrebbe non riuscire e la risorsa entra in uno stato `failed` entro pochi minuti. Contatta il supporto se una risorsa non scompare o non mostra uno stato `failed` entro 30 minuti dopo aver effettuato una richiesta di eliminazione. Una volta che la risorsa è in stato `failed`, puoi tentare di eliminarla di nuovo. [Contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se non sei in grado di eliminare una risorsa in stato `failed`.

## Segui questo ordine quando elimini un VPC
{: #deleting-order}

Un VPC contiene sottoreti e gateway pubblici. Un gateway pubblico può essere collegato a una o più sottoreti nel VPC. All'interno di una sottorete di un VPC sono presenti un programma di bilanciamento del carico e un gateway VPN. Una sottorete può contenere anche istanze del server virtuale (VSI) e una VSI può avere più interfacce di rete in diverse sottoreti all'interno del VPC. Prima di poter eliminare una sottorete, è necessario eliminare tutti i gateway VPN, i programmi di bilanciamento del carico e le interfacce di rete presenti nella sottorete. Un VPC contiene anche dei gruppi di sicurezza, ma questi vengono eliminati automaticamente quando si elimina il VPC. I prefissi di indirizzo sono attributi di un VPC e vengono eliminati automaticamente quando il VPC viene eliminato.

Segui questo ordine quando elimini un VPC:

1.  Elimina tutti i gateway VPN di cui è stato eseguito il provisioning in qualsiasi sottorete del VPC.
2.  Scollega tutti i programmi di bilanciamento del carico di cui è stato eseguito il provisioning in qualsiasi sottorete del VPC.
3.  Elimina tutte le istanze con interfacce di rete in qualsiasi sottorete del VPC. Qualsiasi IP mobile associato a un'interfaccia di rete viene scollegato automaticamente dall'interfaccia di rete quando l'istanza viene eliminata.
4.  Elimina tutte le sottoreti nel VPC. Qualsiasi gateway pubblico collegato a una sottorete viene scollegato automaticamente quando la sottorete viene eliminata.
5.  Elimina tutti i gateway pubblici nel VPC.
6.  Una volta che il VPC non contiene più sottoreti e gateway pubblici, è possibile eliminarlo. Tutti i prefissi di indirizzo nel VPC vengono eliminati automaticamente quando il VPC viene eliminato.

## Relazioni tra le risorse VPC
{: #deleting-relationships}

Alcune risorse VPC sono contenute in altre risorse VPC (come "risorse secondarie"), ma altre risorse VPC sono contenute a livello di account, al di fuori di qualsiasi specifico VPC. È necessario eliminare tutte le risorse VPC secondarie prima di poter eliminare la risorsa principale. Per le risorse VPC a livello di account, è necessario eliminare tutti i collegamenti alle risorse esistenti prima di poter eliminare la risorsa dell'account.

In alcuni casi, l'eliminazione di una risorsa VPC rimuove automaticamente il collegamento alle risorse esistenti. Le seguenti tabelle descrivono il comportamento previsto.

### VPC
{: #deleting-details}

I VPC contengono sottoreti e gateway pubblici. In una sottorete viene eseguito il provisioning di programmi di bilanciamento del carico, gateway VPN e istanze. Prima di poter eliminare un VPC, è necessario eliminare tutte le sottoreti e tutti i gateway pubblici presenti nel VPC. In altre parole, tutti i gateway pubblici, i programmi di bilanciamento del carico, i gateway VPN e le istanze nel VPC devono essere eliminati per primi.

Un VPC contiene anche dei prefissi di indirizzo e gruppi di sicurezza, ma queste risorse vengono eliminate automaticamente quando si elimina il VPC.

Un VPC deve avere un gruppo di sicurezza predefinito e un ACL di rete predefinito. Se crei un VPC senza specificare un gruppo di sicurezza e un ACL di rete predefiniti, queste risorse predefinite verranno create automaticamente per il VPC. Il gruppo di sicurezza predefinito e l'ACL di rete predefinito non possono essere eliminati. L'ACL di rete predefinito viene eliminato automaticamente quando si elimina il VPC. Tutti i gruppi di sicurezza nel VPC vengono eliminati automaticamente quando si elimina il VPC.

| Nel VPC | Può contenere | Può collegarsi a | Ha uno stato? | Eliminato automaticamente quando si elimina il VPC | Si scollega automaticamente quando eliminato |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Sottorete | istanze, programmi di bilanciamento del carico, gateway VPN | gateway pubblico, ACL di rete | Sì| No | Sì|
| Gateway pubblico| --- | sottoreti, IP mobile | Sì | No | No alle sottoreti, sì all'IP mobile |
| Gruppi di sicurezza | ---  | istanze (NIC), VPC come impostazione predefinita | No| Sì | No |

### Sottorete
{: #deleting-subnet}

È necessario eliminare tutte le risorse in una sottorete prima di poter eliminare la sottorete. Le sottoreti contengono le interfacce di rete, i programmi di bilanciamento del carico e i gateway VPN dell'istanza. Prima di poter eliminare la sottorete, è necessario eliminare l'interfaccia di rete a essa associata, insieme a qualsiasi programma di bilanciamento del carico o gateway VPN di cui è stato eseguito il provisioning nella sottorete.

Un'istanza può avere più interfacce di rete e tali interfacce di rete possono appartenere a più sottoreti del VPC. Prima di poter eliminare una sottorete, è necessario eliminare tutte le interfacce di rete nella sottorete. L'interfaccia di rete primaria dell'istanza non può essere eliminata o spostata in un'altra sottorete, quindi prima di eliminare la sottorete è necessario eliminare tutte le istanze con un'interfaccia di rete primaria nella sottorete.


| Nella sottorete | Può contenere | Può collegarsi a | Ha uno stato? | Eliminato automaticamente quando si elimina la sottorete | Scollegato automaticamente quando eliminato |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Istanza (interfaccia di rete) | più interfacce di rete | collegamenti del volume, gruppi di sicurezza | Sì| No  | Sì|
| VPN | --- | ---| Sì | No  | --- |
| Programma di bilanciamento del carico | ---  | --- | Sì | No | ---  |

### Istanza
{: #deleting-instance}

Non sono richiesti prerequisiti per l'eliminazione di un'istanza. Quando l'istanza viene eliminata, tutte le sue interfacce di rete vengono eliminate automaticamente. Il volume di boot dell'istanze e tutti i relativi collegamenti vengono eliminati. Qualsiasi IP mobile collegato a una delle sue interfacce di rete viene rilasciato automaticamente. Qualsiasi volume di archiviazione blocchi collegato all'istanza viene eliminato automaticamente se il volume è stato creato con l'indicatore `delete_volume_on_instance_delete` impostato su true; altrimenti, l'istanza viene scollegata dal volume, ma il volume rimane. Se un gruppo di sicurezza è collegato a una qualsiasi delle interfacce di rete dell'istanza, anche i gruppi di sicurezza vengono scollegati automaticamente, quando le interfacce di rete vengono eliminate.

| Nell'istanza | Può contenere | Può collegarsi a | Ha uno stato? | Eliminato automaticamente quando si elimina l'istanza | Scollegato automaticamente quando eliminato |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Interfaccia di rete | --- | sottoreti, IP mobile, gruppi di sicurezza | No | Sì | Sì |

### Programma di bilanciamento del carico
{: #deleting-lb}

Non sono richiesti prerequisiti per l'eliminazione di un programma di bilanciamento del carico. Quando il programma di bilanciamento del carico viene eliminato, tutti i listener, i pool e i membri del pool che fanno parte del programma di bilanciamento del carico vengono eliminati automaticamente.

L'eliminazione di un programma di bilanciamento del carico può richiedere fino a 30 minuti. La richiesta di eliminazione modifica immediatamente lo stato di provisioning del programma di bilanciamento del carico in `deleting`. Tuttavia, non viene effettivamente eliminato finché il programma di bilanciamento del carico non scompare dalla query di elenco.
{: important}

### VPN
{: #deleting-vpn}

Non sono richiesti prerequisiti per l'eliminazione di un gateway VPN. Quando il gateway VPN viene eliminato, vengono eliminate automaticamente anche le sue connessioni associate. Le politiche IKE e IPSec non vengono eliminate quando si elimina un gateway VPN.

L'eliminazione di un gateway VPN può richiedere fino a 30 minuti. La richiesta di eliminazione modifica immediatamente lo stato di provisioning del gateway VPN in `deleting`. Tuttavia, non viene effettivamente eliminato finché il gateway VPN non scompare dalla query di elenco.
{: important}

### IP mobile
{: #deleting-floating-ip}

Un IP mobile si trova al di fuori di un VPC, a livello di account. Se l'IP mobile è associato a un gateway pubblico o a un'istanza, il collegamento deve essere eliminato (rilasciato) prima che l'IP mobile possa essere eliminato.

Se elimini una risorsa a cui è associato l'IP mobile, ad esempio se elimini l'interfaccia di rete di un'istanza, l'IP mobile viene rilasciato automaticamente.

### Volume
{: #deleting-volume}

In un VPC possono esistere due tipi di volumi: un volume di boot di archiviazione blocchi e un volume di dati di archiviazione blocchi. Questi volumi vengono eliminati in modo diverso.

**Eliminazione dei volumi di boot**

I volumi di boot sono presenti all'interno di un'istanza e non sono considerati entità separate da tale istanza. Quando crei un'istanza, viene creato anche un volume di boot che viene collegato all'istanza. Il volume di boot viene eliminato automaticamente quando elimini l'istanza. Non puoi eliminare il volume di boot senza eliminare l'istanza.

**Eliminazione dei volumi di dati**

I volumi di dati di archiviazione blocchi possono essere sottoposti a provisioning e gestiti separatamente dalle istanze del server virtuale associate. Puoi collegare un volume di dati come archiviazione secondaria a un'istanza del server virtuale. Non puoi eliminare un volume di dati che è collegato a un'istanza (ovvero, se il volume è "attivo"). Devi prima scollegare il volume dall'istanza e quindi eliminare il volume.

Un volume di dati ha un attributo (o indicatore) denominato `delete_volume_on_instance_delete` nell'API e `Auto Delete` nella CLI e nell'IU. Se questo indicatore è impostato su `true` (`Enabled` nell'IU), quando l'istanza con il volume collegato viene eliminata, il volume viene scollegato ed eliminato automaticamente. Se l'indicatore del volume è impostato su `false` (`Disabled` nell'IU), l'istanza viene scollegata dal volume, ma il volume non viene eliminato quando si elimina l'istanza collegata. Il volume può essere collegato a un'altra istanza.

Un volume di archiviazione blocchi può essere collegato a un solo server virtuale alla volta.
{: tip}

### Gruppi di sicurezza
{: #deleting-secgroup}

Un gruppo di sicurezza non può essere eliminato se è utilizzato da un'interfaccia di rete o se è il gruppo di sicurezza predefinito di un VPC. Prima di eliminare il gruppo di sicurezza, rimuovi tutte le interfacce di rete da quel gruppo di sicurezza e assicurati che non sia utilizzato come gruppo di sicurezza predefinito del VPC.

L'eliminazione di un VPC eliminerà automaticamente tutti i gruppi di sicurezza presenti in quel VPC.

### ACL di rete
{: #deleting-netacl}

Un ACL di rete non può essere eliminato se è utilizzato da una sottorete o se è l'ACL di rete predefinito di un VPC. Prima di eliminare l'ACL di rete, scollegalo da tutte le sottoreti e assicurati che non sia utilizzato come ACL di rete predefinito di un VPC.

Quando si crea un VPC, questo richiede un ACL di rete predefinito. Se un ACL di rete esistente non viene specificato come predefinito durante la creazione del VPC, viene creato un nuovo ACL di rete che viene impostato come predefinito. Questo ACL di rete predefinito viene eliminato automaticamente quando si elimina il VPC, se l'ACL non è in uso da altre parti.

A differenza dei gruppi di sicurezza, gli ACL di rete possono essere assegnati tra i VPC. Pertanto, l'eliminazione di un VPC non elimina gli ACL di rete.
{: note}

## Passi successivi
{: #deleting-nextsteps}

I seguenti argomenti forniscono ulteriori esempi su come eliminare le risorse VPC, utilizzando la console, la CLI (command line interface) o l'API IBM Cloud.

-   [Eliminazione di risorse VPC utilizzando l'IU](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-console)
-   [Eliminazione di risorse VPC utilizzando la CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)
-   [Eliminazione di risorse VPC utilizzando l'API](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-api)
