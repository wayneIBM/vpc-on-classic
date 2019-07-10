---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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
{:DomainName: data-hd-keyref="DomainName"}

# Limitazioni note
{: #known-limitations}

Questo documento contiene un riepilogo di funzioni e API non supportate, funzioni e casi di utilizzo non ancora supportati, problemi noti nella release corrente e un elenco di funzioni offerte solo come servizi Beta. Le limitazioni note potrebbero cambiare man mano che aggiungiamo funzionalità a {{site.data.keyword.vpc_full}} (VPC), per cui non esitare a controllare di tanto in tanto questo documento.

## Riepilogo delle funzioni non supportate
{: #summary-of-features-not-supported}

* L'accesso Direct Link a VPC è supportato solo tramite l'[**Accesso classico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* Sebbene per VPC sia disponibile un sottoinsieme di endpoint del servizio privato, non sono disponibili tutti gli endpoint. Per i servizi disponibili, vedi [Endpoint di servizio disponibili per IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc).
* Il salvataggio e ripristino di immagini non è supportato.
* I VPC di {{site.data.keyword.cloud_notm}} sono regionali, pertanto un VPC di una regione non può connettersi a un VPC in un'altra regione a meno che non siano abilitati per l'"accesso classico" o connessi tramite una connessione VPN. Vedi [**Accesso classico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Funzioni e casi di utilizzo non ancora supportati
{: #features-and-use-cases-not-yet-supported}

Questa sezione elenca ulteriori dettagli su funzioni e casi di utilizzo non supportati, classificati in base al loro servizio {{site.data.keyword.cloud_notm}}.

### VPC (Virtual Private Cloud)
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} non supporta domini multicast o broadcast.
* {{site.data.keyword.vpc_short}} non fornisce la crittografia end-to-end.
* Un VPC non può essere associato ad altri VPC.
* Gli endpoint del servizio cloud non sono supportati da VPC. Per i servizi disponibili, vedi [Endpoint di servizio disponibili per IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc).

### Calcolo
{: VSI-unsupported-features-and-use-cases}

* Le istanze del server virtuale (VSI) o i server bare metal esistenti di {{site.data.keyword.cloud_notm}} non possono essere migrati in un VPC.
* Non è possibile eseguire il provisioning di server bare metal in un VPC.

### Rete
{: network-unsupported-features-and-use-cases}

* Non è possibile aggiungere instradamenti personalizzati a un VPC. Tutte le sottoreti in un VPC possono comunicare tra loro per impostazione predefinita.
* Una sottorete non può trovarsi in più zone.
* Una sottorete non può essere spostata da una zona all'altra.
* Le sottoreti non possono essere ridimensionate dopo la loro creazione.
* Le risorse classiche di {{site.data.keyword.cloud_notm}} non possono essere nella stessa sottorete in un VPC. La connettività tra le risorse classiche e un VPC nel tuo account è possibile utilizzando l'[**Accesso classico**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* IPv6 non è supportato.
* Non puoi utilizzare il tuo IP pubblico come IP mobile. Il pool di IP mobili è fornito da IBM.
* Un IP mobile è associato a una zona. Ad esempio, un IP mobile riservato da un pool nella Zona 1 non può essere assegnato a una VSI nella Zona 2 nello stesso VPC.
* Gli indirizzi IP privati non possono essere spostati tra le VSI.
* Le interfacce di rete non possono essere spostate tra le VSI.
* Non puoi designare l'indirizzo IP specifico della VSI. Puoi specificare solo l'intervallo IP per la sottorete. Dopo aver collegato una VSI a una sottorete, il sistema assegna un IP privato da quella sottorete.
* {{site.data.keyword.vpc_short}} viene fornito con i propri strumenti Network as a Service quali LBaaS, ACL e gruppi di sicurezza. Non puoi utilizzare i tuoi dispositivi di rete (ad esempio, un gateway Vyatta o altri programmi di bilanciamento del carico) per controllare le risorse nel VPC. Analogamente, non puoi utilizzare i tuoi propri servizi di programmi di bilanciamento del carico o di gruppi di sicurezza nell'infrastruttura classica {{site.data.keyword.cloud_notm}} per controllare le risorse in {{site.data.keyword.vpc_short}}.
* Il gateway pubblico non consente l'avvio del traffico da Internet a una VSI in IBM Cloud VPC. A tale scopo, deve essere utilizzato un IP mobile.
* ACL è senza stato, quindi il traffico di ritorno deve essere consentito esplicitamente nelle regole ACL.

## Problemi noti in questa release
{: #known-issues-in-this-release}

{{site.data.keyword.block_storage_is_short}} potrebbe non convalidare accuratamente l'univocità del nome del volume quando vengono soddisfatte tutte le seguenti condizioni:

* La richiesta di provisioning è simultanea
* Il volume ha lo stesso nome
* Il volume si trova nella stessa regione

## Servizi Beta disponibili

Il peering sicuro a un dispositivo è disponibile come servizio Beta. È disponibile la documentazione per il peering con diversi tipi di dispositivi:

* [Peer Vyatta remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [Peer StrongSwan remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [Peer FortiGate remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [Peer Juniper vSRX remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [Peer Cisco ASAv remoto](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
