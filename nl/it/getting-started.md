---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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

# Esercitazione introduttiva
{: #getting-started}
[comment]: # (argomento della guida collegato)


Per iniziare a utilizzare {{site.data.keyword.cloud}} Virtual Private Cloud:

1. Crea un VPC (Virtual Private Cloud).
2. Crea una o più sottoreti nel VPC (Virtual Private Cloud) in una o più zone.
3. Crea un gateway pubblico (PGW) su una sottorete se vuoi che le risorse nella tua sottorete abbiano accesso a Internet o viceversa.
4. Seleziona i profili delle istanze del server virtuale (VSI) che vuoi eseguire e istanziali.
5. Riserva un indirizzo IP mobile e associalo a un'istanza del server virtuale se vuoi raggiungerla da Internet.
5. Distribuisci il tuo servizio o le tue applicazioni tra le istanze del server virtuale.

## Prerequisiti

 * **Autorizzazioni utente**: assicurati che il tuo utente disponga delle autorizzazioni sufficienti per creare e gestire le risorse nel tuo VPC. Per un elenco di autorizzazioni richieste, vedi [Concessione delle autorizzazioni necessarie per gli utenti VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

 * **Devi avere la tua chiave SSH pronta**: utilizzerai una chiave SSH per la connessione alle tue istanze del server virtuale.

   * Ricerca un file denominato `id_rsa.pub` in una directory `.ssh` all'interno della tua directory home, ad esempio `/Users/<USERNAME>/.ssh/id_rsa.pub`. Il file inizia con `ssh-rsa` e termina con il tuo indirizzo email.

   * Oppure genera una chiave SSH: se non hai una chiave SSH pubblica o se hai dimenticato la password di una chiave esistente, generane una nuova eseguendo il comando `ssh-keygen` e seguendo le istruzioni. Ad esempio, puoi generare una chiave SSH sul tuo server Linux eseguendo il comando `ssh-keygen -t rsa -C "user_ID"`. Tale comando genera due file. La chiave pubblica generata si trova nel file `<your key>.pub`.

## Utilizza l'IU, la CLI o l'API REST

Puoi eseguire il provisioning e gestire tutte le tue risorse VPC tramite l'IU, la CLI o l'API REST.

Se sei un nuovo utente di IBM Cloud Virtual Private Cloud, scegli uno dei link sottostanti che ti guideranno nel processo di creazione del tuo IBM Cloud VPC e delle sue risorse, dall'inizio alla fine. Puoi scegliere se iniziare dall'IU della console, dalla CLI o dalle API REST.

* Per l'accesso tramite l'interfaccia utente, accedi alla [console IBM Cloud ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")]( https://{DomainName}/vpc){: new_window}. Per ulteriori informazioni, vedi [Creazione di un VPC utilizzando la console IBM Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console).
* Per utilizzare l'interfaccia della riga di comando, segui l'esempio [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli).
* Per gli utenti più avanzati, puoi chiamare direttamente le [API REST](https://{DomainName}/apidocs/vpc-on-classic). Segui l'esercitazione sul [codice di esempio](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) per iniziare con le API REST.

## Passi successivi
Se sei pronto ad approfondire, vai direttamente alla documentazione dettagliata su **Rete per VPC**, **VSI per VPC** e **Block Storage per VPC**:

* [**Rete per Virtual Private Cloud**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Server virtuali per Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage per Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
