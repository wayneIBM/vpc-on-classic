---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# Glossario VPC
{: #vpc-glossary}

La seguente terminologia è comunemente utilizzata ed è specifica per {{site.data.keyword.cloud}} Virtual Private Cloud. Vedi [Termini del glossario per IBM Cloud](/docs/overview/glossary?topic=overview-glossary) per ulteriori termini più generali.

## ACL (Access Control List)
{: #access-control-list}

Un ACL (Access Control List) fornisce controlli di sicurezza per il traffico in entrata e in uscita per una [sottorete](#subnet). Un ACL è senza stato. Ogni ACL è costituito da regole, basate su un _IP di origine_, una _porta di origine_, un _IP di destinazione_, una _porta di destinazione_ e un _protocollo_.

In IBM Cloud VPC, ogni sottorete viene creata con un ACL predefinito, che consente il traffico in entrata e in uscita, ma i clienti possono creare ACL personalizzati. È sempre collegato solo un ACL a una sottorete, ma un ACL può essere collegato a più sottoreti.

A volte viene chiamato anche _ACL di rete_ o NACL.

## IP mobile
{: #floating-ip}

L'IP mobile è un metodo per fornire l'accesso in entrata e in uscita a Internet per le risorse VPC quali istanze, un programma di bilanciamento del carico o un tunnel VPN, utilizzando gli _Indirizzi IP mobili_ assegnati da un pool.

Gli indirizzi IP mobili sono indirizzi IP pubblici che vengono forniti dal sistema dal pool preesistente. Non puoi portare i tuoi indirizzi IP pubblici. Gli indirizzi IP mobili sono raggiungibili da Internet e possono essere associati a un'istanza (ad esempio, una VSI, un programma di bilanciamento del carico o un gateway VPN) tramite una vNIC.

Puoi riservare un indirizzo IP mobile dal pool di indirizzi IP mobili disponibili forniti da IBM e puoi associarlo o disassociarlo da qualsiasi istanza presente nello stesso Virtual Private Cloud. L'IP mobile utilizza il [NAT](#nat) 1 a 1 per consentire a un server di comunicare con l'Internet pubblico e anche con una sottorete privata all'interno del tuo ambiente cloud.

## Area geografica
{: #geography}

In un VPC, la designazione dell'area geografica fornisce informazioni sulla regione e sulla zona in cui vengono distribuiti il VPC e le sue risorse associate.

## Immagine
{: #image}

Le informazioni richieste per avviare un'istanza del server virtuale o _istanza_. In genere, è un'immagine istantanea di un sistema operativo disponibile sul mercato, utilizzata per l'avvio.

## Router implicito
{: #implicit-router}

La connettività di rete intrinseca tra tutte le sottoreti create all'interno di un VPC.

## Istanza
{: #instance}

Un nome breve per un server virtuale, o VSI, che viene eseguito all'interno di un VPC.

## LBaaS
{: #lbaas}

LBaaS (Load Balancer as a Service) offre la possibilità di distribuire il traffico in un'allocazione bilanciata tra le istanze in un VPC.

## NAT
{: #nat}

NAT (Network Address Translation) è un metodo di indirizzamento descritto in [Internet RFC 1631 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://tools.ietf.org/html/rfc1631){: new_window} che fa sì che un indirizzo IP possa essere utilizzato per comunicare con molti altri indirizzi IP, come quelli su una sottorete privata, essenzialmente tramite una tabella di ricerca. NAT ha due tipi principali: NAT 1 a 1 e [NAT molti a uno ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}. Originariamente il NAT era inteso come un modo per estendere la vita utile degli indirizzi IP IPv4, ma ora è comunemente usato negli ambienti cloud come un modo per creare comunicazioni con le sottoreti private.

## Profilo
{: #profile}

Un profilo è una combinazione popolare di vCPU e RAM che può essere istanziata rapidamente per avviare un'istanza del server virtuale (VSI). Sono supportate tre famiglie di profili: Bilanciato, Calcolo e Memoria.


## Gateway pubblico
{: #public-gateway}

Un gateway pubblico (PGW) consente l'accesso solo in uscita per la connessione di una sottorete (con tutte le VSI collegate alla sottorete) a Internet. Tieni presente che le sottoreti sono private per impostazione predefinita; tuttavia, puoi facoltativamente creare un PGW e collegargli una sottorete. Dopo aver collegato una sottorete al PGW, tutte le VSI in tale sottorete possono connettersi a Internet.

PGW utilizza il NAT molti a 1, il che significa che migliaia di VSI con indirizzi privati utilizzeranno un indirizzo IP pubblico per comunicare con l'Internet pubblico. Il PGW non consente a Internet di avviare una connessione con tali istanze.

## Regione
{: #region}

Un'area geografica all'interno della quale viene distribuito un VPC. Ogni regione contiene più zone, che rappresentano dei domini di errore indipendenti. IBM Cloud VPC si estende su più zone all'interno della regione assegnata.

## Risorsa
{: #resource}

Un tipo di asset che fa parte di una distribuzione VPC e che può comportare addebiti di fatturazione, ad esempio una VSI, un IP mobile o il VPC stesso. Le risorse possono essere combinate in _gruppi di risorse_ per semplificare la contabilità quando determinate risorse vengono utilizzate sempre in combinazione all'interno di un VPC.

## Gruppo di sicurezza
{: #security-group}

Un gruppo di sicurezza funge da firewall virtuale che controlla il traffico in entrata e in uscita per uno o più server (VSI). Un gruppo di sicurezza è una raccolta di regole che specificano se consentire il traffico per una VSI associata. Quando un cliente avvia una VSI, può associare uno o più gruppi di sicurezza a tale VSI. Date le autorizzazioni corrette, i clienti possono modificare le regole del gruppo di sicurezza.

## Condivisioni
{: #shares}

Un modo per fornire l'archiviazione file in un VPC.

## Sottorete
{: #subnet}

Una sottorete è un intervallo di indirizzi IP, associato a una singola zona, che non può estendersi su più zone o regioni. Una sottorete può estendersi su un'intera zona in un IBM Cloud VPC.

Ai fini di IBM Cloud VPC, l'importante caratteristica di una sottorete è il fatto che le sottoreti possono essere isolate l'una dall'altra, oltre ad essere interconnesse nel solito modo. L'isolamento delle sottoreti può essere realizzato tramite gli ACL (Access Control List) di rete che fungono da firewall per controllare il flusso dei pacchetti di dati tra le sottoreti. Analogamente, i gruppi di sicurezza fungono da firewall virtuali per controllare il flusso dei pacchetti di dati da e verso singole istanze del server virtuale (VSI).

È l'isolamento delle sottoreti che ti consente di creare uno spazio privato all'interno del cloud pubblico.

## Chiave SSH
{: #ssh-key}

Credenziali di accesso crittografate generate automaticamente che controllano l'accesso alle istanze del server virtuale in un VPC.

## Volumi
{: #volumes}

Archiviazione dati ad alte prestazioni montata su hypervisor per le istanze del server virtuale in un VPC.

## VPC
{: #vpc}

Una rete virtuale correlata a un account. Fornisce un controllo dettagliato sull'infrastruttura virtuale e sulla segmentazione del traffico di rete, oltre alla sicurezza e alla capacità di ridimensionare in modo dinamico.

## VPN
{: #vpn}

Una VPN (Virtual Private Network) consente una connessione privata tra due endpoint, anche quando i dati vengono trasferiti su una rete pubblica. I clienti possono condividere i dati come se fossero connessi a una rete privata. Di solito, una VPN viene utilizzata in combinazione con metodi di sicurezza come l'autenticazione e la crittografia per garantire la massima sicurezza e privacy dei dati.

## Connessione VPN
{: #vpn-connection}

Una connessione privata tra due endpoint, che rimane privata e può essere crittografata anche quando i dati vengono trasferiti su una rete pubblica.

## VPNaaS
{: #vpnaas}

VPNaaS (VPN as a Service) fornisce la configurazione di una VPN con endpoint all'interno e all'esterno di un VPC.

## Zona
{: #zone}

Un dominio di errore indipendente. Una zona è un'astrazione progettata per migliorare la tolleranza di errore e ridurre la latenza. Una zona garantisce le seguenti proprietà:

 * Poiché ogni zona è un dominio di errore indipendente, è estremamente improbabile che in due zone all'interno di una regione si verifichi contemporaneamente un errore.
 * Il traffico tra le zone di una regione avrà una latenza inferiore a 2 ms.
