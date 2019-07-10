---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}

# Note sulla release
{: #release-notes}

Utilizza queste note sulla release per conoscere le ultime modifiche apportate a {{site.data.keyword.cloud}} Virtual Private Cloud.
{:shortdesc}

## 14 giugno 2019
{: #june-14-2019}

**Aggiornamenti all'IU di IBM Cloud VPC**

- **Chiavi SSH e gruppi di risorse.**
    * I gruppi di risorse sono mostrati nella vista dell'elenco di chiavi SSH.
    * I gruppi di risorse sono selezionabili nel modale di provisioning delle chiavi SSH.
- **Profili e larghezza di banda.** Il limite di larghezza di banda assegnato a ciascun profilo viene mostrato nelle pagine di visualizzazione **Profili popolari** e **Tutti i profili**.
- **Gruppi di risorse.** La pagina di provisioning VSI mostra il menu a discesa del gruppo di risorse.

**Aggiornamenti a Block Storage for VPC**
- La **lunghezza del nome volume** è stata aumentata a 63 caratteri alfanumerici.
- I seguenti **codici di errore volume** sono stati ridenominati e i seguenti messaggi sono stati aggiornati o eliminati:
    * `volume_encryption_key_account_id_mismatch` è stato ridenominato con `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` è stato ridenominato con `volume_crn_cname_mismatch`
    * `volume_encryption_key_region_not_found` è stato rimosso

**Aggiornamenti all'SDK**

- È stato rilasciato il **provider Terraform v0.17.1**. Scarica il file [binario più recente](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1).
- L'**immagine Docker** è stata inoltre aggiornata con l'ultimo provider Terraform. Puoi eseguire il pull dell'immagine Docker più recente utilizzando questo comando:  `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Ricordati di impostare `generation` come argomento del provider o di esportare `IC_GENERATION= 1` in modo che il tuo codice funzioni con la versione attualmente rilasciata del VPC sull'infrastruttura classica. Per impostazione predefinita, il valore è impostato su 2 (VPC, in arrivo, ora in Beta).
- Gli argomenti del provider (`bluemix_api_key` e `bluemix_timeout`) sono stati dichiarati obsoleti, pertanto potrebbe essere generata un'avvertenza quando esegui o applichi il piano Terraform.

## 7 giugno 2019
{: #june-07-2019}

- **IBM Cloud VPC è ora generalmente disponibile.** Tutti gli [account Pagamento a consumo](/docs/account?topic=account-accounts) possono eseguire il provisioning di risorse VPC.
Accedi alla [console IBM Cloud](https://{DomainName}/vpc/overview) e inizia a lavorare oggi stesso!

- **È ora possibile impostare politiche di autorizzazione individuali su istanze, chiavi, volumi e gruppi di sicurezza.** Scopri quali ruoli sono necessari per gestire queste risorse in [Autorizzazioni delle risorse richieste per le chiamate API e CLI](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Gli utenti con accesso anticipato non sono interessati dalle politiche esistenti.

- **Il parametro di velocità della porta per un'interfaccia di rete virtuale è stato rimosso dall'interfaccia utente, dalla CLI e dall'API.**

- **Il supporto multi-regione per le metriche di monitoraggio è ora disponibile nell'interfaccia utente.**


## 31 maggio 2019
{: #may-31-2019}

**Nuova versione API disponibile.** La versione dell'API VPC on Classic è stata aggiornata da `2019-01-01` a `2019-05-31`. Per linee guida e procedure ottimali, vedi [Versioning](https://{DomainName}/apidocs/vpc-on-classic#versioning) nell'API Virtual Private Cloud on Classic.

## 24 maggio 2019
{: #may-24-2019}

- **Le istanze del programma di bilanciamento del carico in stato di eliminazione vengono ora mostrate nella vista di elenco nell'interfaccia utente.** Quando richiedi di eliminare un programma di bilanciamento del carico nell'IU, la vista di elenco continua a mostrare il programma di bilanciamento del carico finché il processo di eliminazione non viene completato.

- **Le funzioni del programma di bilanciamento del carico di livello 7 sono disponibili nell'interfaccia utente.** Puoi ora configurare le funzioni del programma di bilanciamento del carico di livello 7 nell'interfaccia utente.

- **Le politiche di visualizzazione e modifica del programma di bilanciamento del carico sono ora disponibili nell'interfaccia utente.** Nell'interfaccia utente è disponibile una nuova funzione per visualizzare e modificare le politiche del programma di bilanciamento del carico.

Per ulteriori informazioni, consulta la [documentazione del programma di bilanciamento del carico](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10 maggio 2019
{: #may-10-2019}


- **I nomi profilo sono stati modificati**. I nomi dei profili di istanza sono stati modificati. I comandi per eseguire il provisioning delle istanze devono essere aggiornati per utilizzare i nuovi nomi profilo. Leggi ulteriori informazioni [sui profili qui](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **Il prezzo mensile stimato per l'IP mobile è disponibile nell'interfaccia utente**. Mentre prenoti un indirizzo IP mobile, puoi ora vedere una riga che mostra il prezzo mensile stimato per tale IP mobile.

- **È disponibile il supporto per Hyper Protect**.

- **Il nome e la zona della sottorete vengono visualizzati come collegamenti che ti condurranno alla sottorete associata nell'interfaccia utente**. Le sottoreti vengono ora elencate per nome anziché per ID sottorete e il nome funge da collegamento alla pagina dei dettagli della sottorete associata.

- **Lo stimatore del riepilogo dei costi è disponibile nell'interfaccia utente**.

- **Il polling per i pool e i listener del programma di bilanciamento del carico si riflette nell'interfaccia utente**.

    * Nella pagina dei pool del programma di bilanciamento del carico, puoi ora vedere un contatore che mostra l'ultimo aggiornamento, sotto la tabella delle risorse.
    * L'intervallo di polling predefinito è di 30 secondi.
    * Possono essere visualizzati quattro (4) stati diversi: `newProvision`, `active`, `failed` e `all other states`.
        * Per un nuovo provisioning: l'intervallo è di 15 secondi. I pool e i listener passano subito allo stato attivo.
        * Per lo stato attivo: l'elenco delle risorse e l'intestazione vengono aggiornati ogni 60 secondi.
        * Per lo stato non riuscito: il polling viene interrotto.
        * Per tutti gli altri stati: il polling continua a intervalli di 30 secondi.
    * Se elimini un listener, puoi attivare lo stato dell'intestazione per visualizzare `Updating (Actions Unavailable)`.
    * Nota inoltre che le informazioni dell'intestazione vengono aggiornate quando si verifica un aggiornamento.
    * Queste stesse modifiche si applicano al polling per i listener.

- **Il conteggio dei membri del programma di bilanciamento del carico viene mostrato nella pagina dei membri e dei dettagli nell'interfaccia utente, mentre avviene il polling dei membri**.

    * Nella sezione dello stato di integrità, vedi il conteggio dei membri mentre il "Recupero dei dettagli dell'istanza" è accanto ad esso.
    * Nella colonna delle istanze nella tabella delle risorse, vedi il conteggio dei membri mentre il "Recupero dei dettagli dell'istanza" è accanto ad esso.
    * Una volta caricati, se hai dei membri non validi, vedrai l'icona gialla di cautela.

- **La pagina dei dettagli del VPC nell'interfaccia utente mostra tutte le sottoreti collegate**.
