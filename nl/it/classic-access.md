---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Configurazione dell'accesso alla tua infrastruttura classica da VPC
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (argomento della guida collegato)

Puoi configurare l'accesso alla tua infrastruttura classica di {{site.data.keyword.cloud}}, inclusa la connettività Direct Link, da un VPC in ogni regione, per qualsiasi account. Questi speciali "VPC con accesso classico" utilizzano la stessa funzionalità di instradamento (router implicito) della tua infrastruttura classica {{site.data.keyword.cloud}}. Si presuppone che i lettori abbiano familiarità con la rete dell'infrastruttura classica.

Quando configuri un VPC per l'accesso classico, ogni host di calcolo (VSI o Bare Metal) senza un'interfaccia pubblica nel tuo account classico può inviare e ricevere pacchetti mediante il VPC con accesso classico. Tuttavia, ricorda che firewall, gateway, ACL di rete o gruppi di sicurezza possono filtrare questo traffico. Come procedura ottimale, ti consigliamo di consentire solo il traffico necessario per il corretto funzionamento delle tue applicazioni.

Negli host con un'interfaccia pubblica, devi aggiungere un instradamento al tuo VPC abilitato per l'accesso classico.
{: important}

## Prerequisiti:
{: #vpc-prerequisites}

1. Il tuo account classico deve essere collegato al tuo account IBM Cloud. Per istruzioni su come effettuare questa operazione, vedi [Collegamento degli account ID IBM](/docs/account?topic=account-unifyingaccounts).
1. Il tuo account classico deve essere abilitato per VRF.
    * Se hai già Direct Link sul tuo account, sei pronto.
    * Se il tuo account non è abilitato per VRF, apri un ticket per richiedere la "Migrazione VRF" per il tuo account. Consulta l'argomento su [come poter iniziare la conversione](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion) nella nostra documentazione di Direct Link per ulteriori informazioni sul processo di conversione.

I firewall, gateway, ACL di rete o gruppi di sicurezza possono filtrare tutto o parte del traffico di rete tra le risorse classiche e VPC.
{: important}

Tutte le sottoreti in un VPC con accesso classico verranno condivise nel VRF dell'infrastruttura classica, che utilizza gli indirizzi IP nello spazio `10.0.0.0/8`. Per evitare conflitti di indirizzo IP, **non utilizzare** gli indirizzi IP sullo spazio `10.0.0.0/8` durante la creazione di sottoreti in un VPC con accesso classico.
{: important}

## Crea un VPC con accesso classico
{: #create-a-classic-access-vpc}

Puoi creare un VPC con funzionalità di accesso classico utilizzando l'IU (interfaccia utente), la CLI (interfaccia della riga di comando) o l'API (Application Programming Interface).

Un VPC deve essere configurato per l'accesso classico quando viene creato: non puoi aggiornare un VPC per aggiungere o rimuovere la funzionalità di accesso classico.
{: note}

### Crea un VPC con accesso classico dall'interfaccia utente
{: #create-a-classic-access-vpc-from-the-user-interface}

Crea un VPC con accesso classico dalla pagina **Create VPC**, facendo clic sulla casella di spunta intitolata **Enable access to classic resource**, sotto l'etichetta **Classic access**.

![iu-accesso-classico](/images/classic-access-ui.png)

### Crea un VPC con accesso classico utilizzando la CLI
{: #create-a-classic-access-vpc-using-the-cli}

Utilizza l'indicatore `--classic-access` quando crei il VPC. Ad esempio:

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### Crea un VPC con accesso classico utilizzando l'API
{: #create-a-classic-access-vpc-using-the-api}

Passa il parametro `classic_access` quando crei il VPC. Ad esempio:

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Prefissi di indirizzo predefiniti del VPC con accesso classico
{: #classic-access-default-address-prefixes}

Tutte le sottoreti in un VPC con accesso classico verranno condivise nel VRF dell'infrastruttura classica, che utilizza gli indirizzi IP nello spazio `10.0.0.0/8`. Per evitare conflitti di indirizzo IP, **non utilizzare** gli indirizzi IP sullo spazio `10.0.0.0/8` durante la creazione di sottoreti in un VPC con accesso classico.
{: important}

Quando viene creato un nuovo VPC con accesso classico, vengono assegnati i prefissi di indirizzo predefiniti, in base alla regione e alla zona, come mostrato nella seguente tabella:

Zona         | Prefisso indirizzo
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## Limitazioni
{: #vpc-limitations}

* Solo le tue reti private (o di "backend" nella documentazione precedente) saranno connesse al router implicito privato del tuo account.
* Solo le sottoreti assegnate con le API classiche sono connesse al lato classico del tuo router implicito privato. La funzione di instradamento del router implicito non funziona tra le sottoreti sulle VLAN classiche.
* Solo un VPC per regione, per ogni account, può essere abilitato per l'accesso classico.

A seconda del sistema operativo che hai installato sulle VSI o sui server bare metal classici, la procedura di configurazione può variare. Per ulteriori informazioni, vedi [Informazioni sui server virtuali pubblici](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
