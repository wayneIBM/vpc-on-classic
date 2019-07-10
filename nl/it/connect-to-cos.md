---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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
{:DomainName: data-hd-keyref="DomainName"}

# Connessione a IBM Cloud Object Storage da VPC
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

Questo documento ti mostra come connetterti a {{site.data.keyword.cloud}} Object Storage dal tuo Virtual Private Cloud. Le informazioni sull'endpoint si applicano nello specifico a {{site.data.keyword.cloud_notm}} Virtual Private Cloud nell'ambiente dell'infrastruttura classica.
{: shortdesc}


## Che cos'è IBM Cloud Object Storage (COS)?
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage (COS) è una piattaforma su scala web che archivia i dati non strutturati. Fornisce affidabilità, sicurezza, disponibilità e ripristino di emergenza senza replica.

Le informazioni archiviate con {{site.data.keyword.cloud_notm}} Object Storage vengono crittografate e distribuite in più ubicazioni geografiche. Sono accessibili tramite un'implementazione dell'API S3. Questo servizio utilizza le tecnologie di archiviazione distribuita fornite dal sistema IBM Cloud Object Storage.

IBM Cloud Object Storage è disponibile con tre tipi di configurazioni per la resilienza: **Più regioni**, **Regionale** e **Singolo data center**. 
* Il servizio **Più regioni** offre una maggiore durata e disponibilità rispetto all'utilizzo di una singola regione, a scapito di una latenza leggermente più elevata. Questo servizio è disponibile attualmente nelle aree degli Stati Uniti, dell'Unione Europea e dell'Asia Pacifico. 
* Il servizio **Regionale** inverte i compromessi. Distribuisce gli oggetti in più zone di disponibilità all'interno di una singola regione. È disponibile nelle regioni degli Stati Uniti, dell'Unione Europea e dell'Asia Pacifico. Se una determinata regione o zona non è disponibile, l'archivio oggetti continua a funzionare senza impedimenti.
Il servizio **Singolo data center** distribuisce gli oggetti in più macchine all'interno della stessa ubicazione fisica. Controlla regolarmente questo documento per verificare le regioni disponibili.

### Endpoint diretti COS da utilizzare con VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Per inviare una richiesta API REST, devi impostare un endpoint di destinazione o un URL e ciascuna ubicazione di archiviazione COS deve avere la propria serie di URL. Gli endpoint diretti consentono connessioni dirette ad alta velocità tra i tuoi server e i servizi {{site.data.keyword.cloud_notm}}, senza l'addebito di costi aggiuntivi per la larghezza di banda. Con l'endpoint diretto COS, un VPC può connettersi a un bucket COS situato in qualsiasi parte del mondo. 

Non vi è alcun addebito per il traffico dai tuoi VPC a tutti gli endpoint COS elencati in questa pagina.
{: note}

* Un client VPC ha accesso al bucket COS anche sull'endpoint pubblico.
* Un client VPC ha accesso all'[API di configurazione COS](https://{DomainName}/apidocs/cos/cos-configuration) solo sull'endpoint pubblico, non sull'endpoint diretto. L'API di configurazione COS può essere utilizzata per configurare alcune funzioni COS sui bucket, nonché per visualizzare i metadati del bucket.
* Un client VPC non ha accesso a un bucket COS quando il firewall è abilitato.

## Come connettersi a IBM Cloud Object Storage (COS) da un VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. Esegui il provisioning di COS dal [catalogo ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}.
2. Crea un bucket COS in una delle regioni elencate nella seguente sezione.
3. Utilizza l'endpoint per comunicare con il tuo bucket COS.

### Endpoint regionali
{: #regional-endpoints}

I bucket creati in un endpoint regionale distribuiscono i dati in tre data center, che sono estesi su un'area metropolitana. Ognuno di questi data center può subire un'interruzione, o addirittura una distruzione, senza compromettere la disponibilità.

| **Regione** | **Endpoint** |
|------------|-------------------------------|
| Stati Uniti Sud | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| Stati Uniti Est | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| Unione Europea, Regno Unito | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| Unione Europea, Germania | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| Asia Pacifico, Australia | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| Asia Pacifico, Giappone | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### Endpoint su più regioni
{: #cross-region-endpoints}

I bucket creati in un endpoint su più regioni distribuiscono i dati in tre regioni. Ognuna di queste regioni può subire un'interruzione, o addirittura una distruzione, senza compromettere la disponibilità. Le richieste vengono instradate al data center della regione più vicina utilizzando l'instradamento BGP (Border Gateway Protocol).

In caso di interruzione, le richieste vengono reinstradate automaticamente a una regione attiva. Gli utenti esperti che desiderano scrivere la propria logica di failover possono farlo inviando le richieste all'endpoint della regione specifica e ignorando l'instradamento BGP.

| **Più regioni USA** | **Endpoint** |
|------------|-------------------------------|
| Più regioni USA | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Dallas | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| Punto di accesso San Jose | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **Più regioni UE** | **Endpoint** |
|------------|-------------------------------|
| Più regioni UE | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Amsterdam | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Francoforte | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Milano | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **Più regioni Asia Pacifico** | **Endpoint** |
|------------|-------------------------------|
| Più regioni A.P. | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Tokyo | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Seoul | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| Punto di accesso Hong Kong | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### Endpoint su singolo data center
 {: #single-datacenter-endpoints}

I singoli data center non sono co-ubicati con i servizi IBM Cloud, come IAM o Key Protect, e non offrono resilienza in caso di interruzione o distruzione di un sito.

Se un errore di rete comporta una partizione, che fa sì che il data center non sia in grado di raggiungere una regione IBM Cloud centrale per l'accesso a IAM, le informazioni di autenticazione e autorizzazione vengono lette da una cache che potrebbe diventare obsoleta. Questa azione potrebbe comportare la mancata applicazione di politiche IAM nuove o modificate per un massimo di 24 ore.
{:important}

| **Regione** | **Endpoint** |
|------------|-------------------------------|
| Amsterdam, Paesi Bassi| `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| Chennai, India | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| Hong Kong | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| Melbourne, Australia | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| Città del Messico, Messico | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| Milano, Italia | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| Montréal, Canada | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| Oslo, Norvegia | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| San Jose, USA | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| São Paulo, Brasile | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| Seul, Corea del Sud | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| Toronto, Canada | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
