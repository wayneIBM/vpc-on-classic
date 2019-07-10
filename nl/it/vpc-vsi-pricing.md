---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Prezzi per {{site.data.keyword.vsi_is_short}} 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (argomento della guida collegato)


{{site.data.keyword.vsi_is_full}} viene offerto in regioni selezionate con fino a 62 vCPU e 248 GB di RAM per adattarsi a qualsiasi esigenza di carico di lavoro. Ti viene addebitata solo una tariffa oraria, a cui verranno applicati degli sconti se la tua istanza rimane in esecuzione più a lungo. I tempi di utilizzo del server virtuale vengono calcolati al secondo, sia per il tempo di utilizzo che per il tempo di sospensione della tua istanza. Ad esempio, se la tua istanza viene eseguita per 45 minuti e 32 secondi, ti verranno addebitati 45 minuti e 32 secondi.
{:shortdesc}

## Utilizzo prolungato
{: #sustained-usage}

Sebbene le istanze vengano addebitate a una tariffa oraria, maggiore è il tempo di esecuzione della tua istanza e meno costosa sarà la tariffa. Con l'avanzare del mese di fatturazione, ricevi i seguenti sconti orari.

| Tempo trascorso in un mese       | Sconto di fatturazione  | 
| ----------------------------- | ----------------- | 
| 0-20%                         | tariffa al dettaglio regolare |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="Tabella 1. Sconti a più livelli" caption-side="top"}  

Questi livelli scontati ti offrono un risparmio del 10% per mantenere le tue istanze in esecuzione su base mensile anziché su base oraria. Questo sconto si applica solo alle tariffe orarie di base; non si applica a software, archiviazione, rete o altri addebiti.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### Esempio di utilizzo prolungato
{: #sustained-usage-example}

Supponiamo che tu abbia acquistato un'istanza dalla famiglia di server virtuali bilanciati, con 16 CPU e 64 GB di RAM, a un prezzo di base di $0,795 all'ora. Con il passare del mese, la tua tariffa diminuisce come segue:

| Tempo trascorso in un mese       | Sconto di fatturazione  |  Tariffa di esempio     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | tariffa al dettaglio regolare ($0,795) | $116,07    |                
| 21-40%                        | 5%        |   $110,27   |                 
| 41-60%                        | 10%       |    $104,46  |            
| 61-80%                        | 15%        |    $98,66    |                
| 81-100%                       | 20% |       $92,86      |
{: caption="Tabella 2. Esempio di sconti a più livelli" caption-side="top"}  

Con questo modello, se l'istanza è rimasta in esecuzione per l'intero mese, la tua fatturazione totale è di $522,32. Gli sconti comportano un risparmio mensile complessivo del 10%, rispetto alla tariffa oraria.

## Prezzi delle istanze di base
{: #base-instance-prices}

I prezzi delle istanze di base partono da $0,087 all'ora. Quando crei un server virtuale, ti viene richiesto di scegliere una famiglia di server virtuali e di selezionare una configurazione del profilo. Quando effettui la tua selezione, la tariffa oraria associata viene visualizzata nella tabella. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## Sistemi operativi inclusi
{: #included-operating-systems}

I seguenti sistemi operativi sono inclusi gratuitamente:

* CentOS 7.più recente
* Ubuntu 16.04 LTS, 18.04 (minimo)
* Debian 8.più recente, 9.più recente (minimo)

Sono disponibili sistemi operativi premium e altri componenti aggiuntivi. Vedrai i prezzi riflessi nel tuo Riepilogo costi.

## Sospensione della fatturazione
{: #suspend-billing}

Quando spegni un'istanza, non accumuli costi per determinate risorse di calcolo. La fatturazione si arresta automaticamente quando interrompi l'istanza. La funzione di sospensione della fatturazione ti consente di ridurre i costi e di evitare di dover ricreare un'istanza quando ti servono nuovamente le sue risorse.

Nelle situazioni in cui vuoi ridimensionare la tua infrastruttura in base alle esigenze del carico di lavoro, puoi utilizzare la funzione di sospensione della fatturazione come alternativa più rapida alla creazione e all'eliminazione delle istanze.
{:tip}

### Dettagli di fatturazione
{: #billing-details}

È importante comprendere quali costi si arrestano e quali rimangono quando viene spenta la tua istanza del server virtuale.

La fatturazione viene sospesa solo quando spegni la tua istanza del server virtuale tramite la console, la CLI o l'API IBM Cloud. Se spegni l'istanza del server virtuale direttamente attraverso il sistema operativo, la fatturazione non viene sospesa per tale istanza.
{:note}

Esamina la seguente tabella per i dettagli su come la sospensione della fatturazione influisce sui vari addebiti delle risorse.

| Risorsa                      | Fatturazione arrestata   | Fatturazione non arrestata |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Upgrade larghezza di banda            |          X        |                  |
| Licenze sistema operativo     |          X        |                  |
| IP mobili, programmi di bilanciamento del carico o altre offerte di rete collegate |                   |         X        |
| Archiviazione                       |                   |         X        |
{: caption="Tabella 1. Dettagli sulla fatturazione della risorsa" caption-side="top"}   

I tempi di utilizzo vengono calcolati al secondo, sia per il tempo di utilizzo che per il tempo di sospensione della tua istanza del server virtuale. Anche se non avvii mai la funzione di sospensione della fatturazione spegnendo la tua istanza, la fatturazione viene calcolata al secondo del ciclo di vita dell'istanza. 
{:note}

#### Sospensione della fatturazione e sconti sull'utilizzo prolungato
{: #suspend-billing-and-sustained-usage-discounts}

Per gli sconti sull'utilizzo prolungato, un'immagine sospesa riprende da dove era stata interrotta sul livello di sconto. In altre parole, uno sconto sull'utilizzo si applica solo al momento in cui l'immagine è in uso, non al momento in cui l'immagine è stata sospesa.

#### Addebito di utilizzo minimo
{: #minimum-usage-charge}

Le istanze del server virtuale hanno un addebito di utilizzo minimo al mese. Se l'utilizzo è maggiore del 25% nel ciclo di fatturazione, ti viene addebitato l'effettivo utilizzo. Se l'utilizzo è inferiore al 25% del tempo in cui era presente nel ciclo di fatturazione, si applica l'addebito minimo del 25% . 

Ad esempio, supponiamo che tu abbia un'istanza in esecuzione per un intero ciclo di fatturazione (720 ore). Di questo tempo, l'istanza è rimasta sospesa per 577 ore e in esecuzione per 143 ore. L'istanza viene addebitata per 180 ore (il minimo delle ore disponibili in quel periodo di fatturazione).  

Supponiamo invece che tu avessi un'istanza che è stata creata e arrestata all'interno di un ciclo di fatturazione che è durato per un totale di 400 ore. Di questo tempo, l'istanza è stata sospesa per 120 ore ed eseguita per 280 ore. L'istanza verrebbe addebitata per il suo utilizzo di 280 ore.

#### Ricevuta della fatturazione
{: #billing-invoice}

Quando sospendi la fatturazione su un server virtuale, vedrai alcune modifiche nella ricevuta della fatturazione. Gli addebiti pertinenti ora vengono visualizzati come dettagli basati sull'utilizzo. Ad esempio, potresti vedere le seguenti aggiunte che riflettono le ore disponibili, le ore utilizzate e il numero totale di ore addebitate:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### Dettagli risorsa
{: #resource-details}

#### Archiviazione
{: #suspend-billing-storage}

Quando sospendi la fatturazione su un'istanza del server virtuale, l'archiviazione associata persiste, ma non puoi accedere ai dati mentre l'istanza del server virtuale è spenta. Quando riprendi la fatturazione sull'istanza, puoi accedere ai tuoi dati e ricomincia la normale fatturazione.

#### Indirizzi IP
{: #suspend-billing-ip-addresses}

Tutte le configurazioni di rete e gli IP (IP privati dall'intervallo della sottorete) rimangono invariati mentre l'istanza è sospesa.

Puoi visualizzare se il tuo dispositivo è stato arrestato nella pagina dei dettagli dell'istanza. Per vedere quando è cambiato lo stato, fai clic su **Attività** nel riquadro di navigazione. 

#### Limitazioni
{: #suspend-billing-limitations}

I server virtuali sospesi continueranno a essere conteggiati nella quota del dispositivo a livello di account.
