---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Informazioni su Virtual Private Cloud
{: #about}

{{site.data.keyword.cloud}} VPC (Virtual Private Cloud) è la piattaforma IBM Cloud di nuova generazione. VPC ti consente di creare il tuo proprio spazio in IBM Cloud, per eseguire un ambiente isolato all'interno del cloud pubblico. VPC ti offre la sicurezza di un cloud privato, con l'agilità e la facilità di un cloud pubblico.

Puoi gestire i servizi di rete e avviare le istanze in base alle esigenze per supportare le tue applicazioni di importanza critica, a tolleranza cloud e native del cloud. Funzioni chiave disponibili:

* Crea e gestisci ambienti applicativi isolati tramite un'API
* Definisci le tue politiche di rete progettate per la sicurezza e l'accesso pratico
* Progetta topologie di rete con BYOIP (Bring Your Own IP)
* Esegui il provisioning delle tue risorse e connettile tra loro oppure isolale l'una dall'altra
* Copri più regioni per il ripristino di emergenza e la resilienza
* Utilizza le zone di disponibilità che consentono connessioni ad alta velocità e bassa latenza tra le regioni, con HA
* Utilizza dispositivi di rete e di archiviazione ad alta velocità
* Consenti servizi Always On (piano di controllo)
* Fornisci e utilizza servizi core: IPAM, VPN, firewall, SSH, DNS e bilanciamento del carico L4
* Configura altri servizi: ridimensionamento automatico, CDN, NFV di terze parti

La figura che segue, relativa alla Panoramica di VPC, illustra i componenti VPC disponibili e il modo in cui interagiscono per fornirti il servizio e la connettività di cui potresti aver bisogno. Fai riferimento a questa figura man mano che consulti gli argomenti che forniscono maggiori dettagli su ciascun componente.

![Panoramica di IBM Cloud VPC](images/vpc-experience-simple.svg "Panoramica di IBM Cloud VPC")

Le seguenti sezioni ti forniscono una panoramica delle funzioni disponibili in IBM Cloud VPC.

## Funzionalità di rete

{{site.data.keyword.cloud_notm}} VPC offre funzionalità di rete semplici e complete, compresa la possibilità di creare più Virtual Private Cloud in [regioni multizona disponibili a livello globale](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region), sottoreti in zone diverse, [la selezione dell'intervallo di indirizzi IP](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), firewall virtuali ([gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) e [ACL di rete](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), reti private virtuali ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) site-to-site e bilanciamento del carico ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) con elasticità.

Un VPC è suddiviso in sottoreti, utilizzando un intervallo di indirizzi IP privati. Tuttavia, per impostazione predefinita, tutte le risorse (come le VSI) all'interno dello stesso VPC possono comunicare tra loro, indipendentemente dalla loro sottorete. Le sottoreti sono contenute in una singola zona e non possono estendersi su più zone, il che aiuta a garantire la sicurezza, la riduzione della latenza e l'alta disponibilità.

Crea facilmente più VPC e sottoreti utilizzando gli intervalli di prefissi suggeriti e le politiche di sicurezza di rete preconfigurate o progetta e definisci i tuoi prefissi di indirizzo e le tue politiche di sicurezza personalizzate.

I blocchi CIDR 161.26/16 e 166.8/14 sono entrambi riservati e instradati a ogni sottorete. Consulta [Endpoint di servizio disponibili per IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc). Inoltre, il blocco CIDR 10.240/13 è riservato per i prefissi di indirizzo VPC e suddiviso tra le zone [in un modo predefinito](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes) e non può essere modificato.
{: important}

### Accesso a Internet

Sono disponibili tre opzioni per abilitare le comunicazioni dalle tue istanze del server virtuale (VSI) all'Internet pubblico:
* Utilizzare un gateway pubblico (PGW) per abilitare la comunicazione su Internet per tutte le istanze del server virtuale nella sottorete collegata. Non vi è alcun addebito per l'utilizzo di un PGW, fatta eccezione per la larghezza di banda utilizzata.
* Utilizzare un IP mobile (FIP) per abilitare la comunicazione da e verso una singola istanza del server virtuale (VSI) su Internet.
* Utilizzare un gateway VPN. Per ulteriori informazioni, consulta la nostra [documentazione VPN for VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)
{: note}

### Accesso classico

Gli attuali utenti dell'infrastruttura {{site.data.keyword.cloud_notm}} possono connettere la loro infrastruttura classica, inclusa la connettività Direct Link, a un VPC per ogni regione utilizzando il nostro [Accesso classico](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

### BYOIP

Puoi utilizzare il tuo intervallo di indirizzi IPv4 pubblici (BYOIP) per il tuo account IBM Cloud VPC.

Quando utilizzi BYOIP, {{site.data.keyword.cloud_notm}} deve configurare questi indirizzi IPv4 sulle risorse {{site.data.keyword.cloud_notm}}, che invieranno pacchetti da e verso gli indirizzi che hai fornito. Pertanto, in seguito all'utilizzo del tuo intervallo IPv4 fornito su {{site.data.keyword.cloud_notm}}, questi indirizzi IP possono essere esposti al personale di supporto di IBM e a terze parti.
{: important}

Per utilizzare BYOIP, devi usare l'API o la CLI. Questa funzionalità non è disponibile tramite l'IU della console IBM Cloud.
{: note}

### Connettività globale

Puoi estendere le tue applicazioni e le risorse disponibili per l'espansione in più regioni. Utilizzando la [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc), puoi creare connessioni private ad altri progetti e ad altre parti delle tue distribuzioni cloud ibride.

In modo facilmente scalabile e condivisibile, una rete VPC e le risorse possono estendersi dalla tua struttura in loco al tuo cloud.

## Sicurezza

La sicurezza è integrata nel tuo {{site.data.keyword.cloud_notm}} VPC, con i [gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) che fungono da firewall virtuali per la protezione a livello di istanza e con gli [ACL di rete](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) per la protezione a livello di sottorete.

## Alta disponibilità

{{site.data.keyword.cloud_notm}} VPC (Virtual Private Cloud) offre alle tue applicazioni un isolamento logico da altre reti in tutte le regioni, fornendo al contempo scalabilità e sicurezza. Utilizza i [Programmi di bilanciamento del carico](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc) per distribuire il traffico di rete attraverso una serie di destinazioni per migliorare le prestazioni e l'alta disponibilità (HA). I programmi di bilanciamento del carico monitorano inoltre l'integrità delle tue applicazioni e dei tuoi servizi. Puoi configurare un programma di bilanciamento del carico per distribuire il traffico dell'applicazione in entrata tra le istanze in una singola zona o tra più zone all'interno di una regione.

## Funzionalità di calcolo

Utilizza [IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) per eseguire il provisioning di risorse di calcolo scalabili in IBM Cloud. Crea rapidamente istanze del server virtuale (VSI), utilizzando i [profili](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles) predefiniti ottimizzati per i tuoi specifici carichi di lavoro. Quando esegui il provisioning delle istanze multi-homed [con più vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options), scegli tra le [immagini](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) predefinite supportate o carica un'immagine personalizzata.

{{site.data.keyword.cloud_notm}} VPC aggiunge un livello di orchestrazione di rete che elimina il limite di pod delle istanze del server virtuale, creando una capacità infinita per il ridimensionamento delle istanze. Il livello di orchestrazione di rete gestisce tutta la rete per tutte le istanze del server virtuale che si trovano all'interno di un IBM Cloud VPC tra le regioni e le zone. Con le funzionalità di rete definita dal software fornite da {{site.data.keyword.cloud_notm}} VPC, hai più opzioni per VPN, LBaaS e istanze con più vNIC e maggiori dimensioni di sottorete.

## Funzionalità di archiviazione

Quando esegui il provisioning di un'istanza {{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud, un volume di archiviazione blocchi IOPS (3 IOPS/GB) di utilizzo generico da 100 GB viene creato automaticamente, come volume di boot principale, e collegato all'istanza. Puoi creare i volumi di archiviazione blocchi quando esegui il provisioning di un'istanza del server virtuale in una rete VPC o puoi creare nuovi volumi indipendenti dal ciclo di vita della VSI.

Durante il provisioning di ulteriore archiviazione blocchi per il tuo VPC, puoi selezionare un [livello IOPS](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers) per il tuo volume di archiviazione blocchi o specificare un [profilo IOPS personalizzato](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom).

## Progettato per i carichi di lavoro nativi del cloud e ibridi

Un VPC è ideale per i carichi di lavoro nativi del cloud e per collegare la tua infrastruttura esistente a {{site.data.keyword.cloud_notm}}. Puoi creare il miglior cloud per carichi di lavoro come i calcoli cognitivi, AI e Machine Learning.

Ecco altri modi in cui VPC supporta i tuoi carichi di lavoro ibridi, a tolleranza cloud e nativi del cloud:

* Bilanciamento del carico con ridimensionamento automatico orizzontale
* Monitoraggio e controlli di integrità
* Supporto per le GPU
* Provisioning di VSI con gruppi di affinità e anti-affinità

## Ulteriori informazioni

* [**Terminologia di VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**Sicurezza di VPC**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**Regioni e sottoreti VPC disponibili**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**Creazione e gestione delle risorse di rete in VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**Creazione e gestione delle istanze del server virtuale in VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**Creazione e gestione dell'archiviazione blocchi in VPC**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
