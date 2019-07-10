---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

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

# Gestione delle autorizzazioni utente per le risorse VPC
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.cloud}} VPC (Virtual Private Cloud) utilizza il controllo degli accessi basato sui ruoli che consente agli amministratori di account di controllare l'accesso dei loro utenti alle risorse VPC.

Per ulteriori informazioni su come IBM Cloud VPC utilizza il controllo degli accessi basato sui ruoli e su come VPC utilizza le politiche di ruolo e di accesso IAM, vedi [Assegnazione dell'accesso basato sui ruoli alle risorse VPC](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources).

Questo documento ti mostra in che modo l'amministratore dell'account può aggiungere altri utenti all'account e fornire loro le autorizzazioni corrette per gestire le [risorse dell'infrastruttura VPC](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources). Descrive due scenari comuni per un amministratore VPC:

* **Scenario di accesso semplice:** mostra come assegnare una politica di accesso a un utente, in modo che l'utente possa creare e utilizzare le risorse dell'infrastruttura VPC (inclusi i Virtual Private Cloud).

* **Scenario di accesso del team:** mostra come configurare i gruppi di risorse e le politiche di accesso che consentono a due team separati di creare e utilizzare le risorse VPC assegnate al proprio team.

Per rendere effettive le modifiche alle politiche di accesso IAM per VPC potrebbero essere richiesti fino a 10 minuti.
{: note}

## Scenario di accesso semplice
{: #simple-access-scenario}

Questo scenario analizza i passi di base necessari per configurare un singolo utente. Analizzeremo due casi: l'invito di un nuovo utente all'account e la modifica delle autorizzazioni di un utente esistente.

### Invito di un nuovo utente a creare o gestire le risorse VPC
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

Invita un utente IBM Cloud al tuo account e concedigli l'accesso all'`Infrastruttura VPC` in modo che possa avere accesso per visualizzare, creare e aggiornare le risorse VPC. Questa sezione offre una rapida panoramica delle procedure IAM. Ulteriori informazioni sono disponibili attraverso i link nella sezione **Link correlati** nella parte finale di questo documento.

Di seguito sono riportati i passi di base in IAM necessari per invitare gli utenti ai servizi e alle risorse VPC:

1. Vai all'[IU Utenti IAM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam#/users){: new_window} nella console IBM Cloud.
2. Nella pagina **Utenti**, fai clic su **Invita utenti**.
3. Nella sezione **Utenti** della pagina **Invita utenti**, immetti l'indirizzo email degli utenti che vuoi invitare nel campo **Indirizzo email**.
4. Nella sezione **Accesso**, espandi **Servizi** e completa quindi le seguenti attività:
  * Seleziona **Risorsa** dall'elenco **Assegna accesso a**.
  * Seleziona **Infrastruttura VPC** dall'elenco **Servizi**.
  * Seleziona il ruolo di accesso alla piattaforma che vuoi assegnare agli utenti. Può essere **Amministratore**, **Editor**, **Operatore** o **Visualizzatore**.
  * Fai clic su **Invita utenti**.

### Concessione dell'autorizzazione per gestire le risorse VPC a un utente esistente
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

Questo scenario analizza i passi di base necessari per concedere a un utente esistente nel tuo account l'autorizzazione per modificare le risorse VPC.

Nei passi che seguono, creerai due politiche IAM. Sono necessarie entrambe le politiche prima che l'utente possa creare e utilizzare le risorse dell'infrastruttura VPC. Tutte le risorse devono risiedere all'interno del gruppo di risorse predefinito dell'account.

1. Vai all'[IU Utenti IAM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam#/users){: new_window} nella console IBM Cloud.
2. Seleziona l'utente per il quale stai abilitando l'autorizzazione.
3. Nella scheda **Politiche di accesso**, seleziona **Assegna accesso**.
4. Seleziona **Assegna l'accesso in un gruppo di risorse**.
5. Seleziona il gruppo di risorse predefinito dell'account.
6. Assicurati che l'opzione **Assegna accesso a un gruppo di risorse** rimanga impostata su **Visualizzatore**.
7. Per assegnare il servizio corretto, seleziona **Infrastruttura VPC**.
8. Assicurati che il valore **Tipo di risorsa** sia impostato su **Tutti i tipi di risorsa**.
9. Seleziona il ruolo **Editor**.
10. Fai clic su **Assegna**.

L'utente è ora autorizzato a creare e utilizzare le risorse VPC nel gruppo di risorse predefinito dell'account. La prima politica (passi 1-6) consente all'utente di visualizzare il gruppo di risorse predefinito dell'account. La seconda politica (passi 7-10) ha assegnato all'utente il ruolo di **Editor** per le risorse dell'infrastruttura VPC, ma l'accesso è limitato alle risorse presenti nel gruppo di risorse predefinito dell'account.

{: tip}

## Visualizzazione delle autorizzazioni dell'utente
{: #viewing-your-user-s-permissions}

È possibile visualizzare le politiche nella scheda **Politiche di accesso** dell'utente.

Puoi utilizzare i seguenti comandi della CLI per convalidare le autorizzazioni del gruppo di risorse assegnate al tuo utente, per politica o per gruppo di accesso:

**Per politica**
```
ibmcloud iam user-policies <username>
```
**Per gruppo di accesso**
```
ibmcloud iam access-groups -u <username>
```

Per rendere effettive le modifiche alle politiche di accesso IAM per VPC potrebbero essere richiesti fino a 10 minuti.
{: note}

## Scenario di accesso del team
{: #team-access-scenario}

Questo scenario illustra come un amministratore di account può assegnare l'autorizzazione in modo che diversi team abbiano accesso a risorse VPC separate. L'esempio utilizza i _gruppi di risorse_ per configurare l'accesso alle risorse separate per due team. Ai fini di questo esempio, le risorse non sono condivise tra i team.

L'esempio ti guida nel processo di creazione dei gruppi di risorse, creazione di gruppi di accesso e assegnazione delle politiche appropriate per fornire ai tuoi team l'accesso a risorse VPC separate.

Immagina di impostare due diversi team di progetto per utilizzare due Virtual Private Cloud separati. Assegnerai l'accesso ai membri del team in modo che ogni team abbia accesso alle sole risorse VPC del proprio team.

* Il tuo primo team è un team di test. Hai deciso di assegnare a questo team l'accesso ai VPC in un gruppo di risorse denominato `test_vpcs`.
* Il secondo team è il tuo team di produzione. A loro verrà assegnato l'accesso ai VPC in un gruppo di risorse denominato `production_vpcs`.

Questa strategia può essere utilizzata per assegnare risorse VPC separate a qualsiasi numero di team. Tuttavia, tutte le risorse condividono le stesse quote di VPC per l'account. Per ulteriori informazioni sulle quote e sui limiti, vedi [Quote di VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas).
{: tip}

### Passo 1: crea i gruppi di risorse
{: #step-1-create-resource-groups}

Per impostazione predefinita, gli amministratori di account dispongono dell'accesso per creare nuovi gruppi di risorse. Agli altri utenti deve prima essere assegnato il ruolo di **Editor** per **Tutti i servizi di gestione account**, che consente loro di creare gruppi di risorse.

La tua prima attività è quella di creare gruppi di risorse che conterranno le risorse VPC di ciascuno dei tuoi team.

1. Crea un gruppo di risorse denominato `test_team`.  
2. Crea un gruppo di risorse denominato `production_team`.  

Per ulteriori informazioni su come creare i gruppi di risorse, vedi [Gestione dei gruppi di risorse](/docs/resources?topic=resources-rgs).

### Passo 2: crea i gruppi di accesso
{: #step-2-create-access-groups}

L'accesso alle risorse può essere assegnato a singoli utenti o a gruppi di utenti. Gruppi di utenti con le stesse autorizzazioni di accesso sono chiamati _gruppi di accesso_. In questo scenario, creerai un gruppo di accesso per rappresentare ogni raggruppamento di membri del team che richiedono un tipo specifico di accesso VPC, per un totale di 4 gruppi di accesso univoci.

**Attività:** crea quattro gruppi di accesso con i seguenti nomi: `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs`, `production_team_view_vpcs`.

Per informazioni su come creare i gruppi di accesso, vedi [Creazione dei gruppi di accesso](/docs/iam?topic=iam-groups#create_ag).

### Passo 3: aggiungi le politiche IAM ai gruppi di accesso appena creati
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

Aggiungi le politiche di accesso VPC necessarie in modo che il gruppo di accesso `test_team` possa gestire (creare, aggiornare ed eliminare) le risorse VPC.  

1. Vai all'[IU Gruppo IAM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam#/groups){: new_window} nella console IBM Cloud.
2. Seleziona il gruppo di accesso desiderato (inizia con: `test_team_manage_vpcs`).
3. Nella scheda **Politiche di accesso**, fai clic su **Assegna accesso**.
4. Seleziona **Assegna l'accesso in un gruppo di risorse**.
5. Seleziona il gruppo di risorse desiderato (inizia con: `test_team`)
6. Assicurati che l'opzione **Assegna accesso a un gruppo di risorse** resti impostata su **Visualizzatore**.
7. Seleziona il servizio **Infrastruttura VPC**.
8. Assicurati che l'opzione **Tipo di risorsa** rimanga impostata su **Tutti i tipi di risorsa**.
9. Seleziona il ruolo **Editor**.
10. Seleziona **Assegna**.

Questi passi assegnano anche l'accesso come **Visualizzatore** al gruppo di risorse `test_team`. L'accesso come visualizzatore è necessario per aggiornare, creare ed eliminare le risorse all'interno del gruppo di risorse `test_team`.

{: tip}

Ripeti i passi precedenti per i restanti tre gruppi di accesso. Completerai queste attività per abbinare i gruppi di accesso ai gruppi di risorse:

* Assegna al gruppo di accesso `test_team_view_vpcs` il ruolo di **Visualizzatore** per le risorse dell'infrastruttura VPC all'interno del gruppo di risorse `test_team`.
* Assegna al gruppo di accesso `production_team_manage_vpcs` il ruolo di **Editor** per le risorse dell'infrastruttura VPC all'interno del gruppo di risorse `production_team`.
* Assegna al gruppo di accesso `production_team_view_vpcs` il ruolo di **Visualizzatore** per le risorse dell'infrastruttura VPC all'interno del gruppo di risorse `production_team`.

Gli utenti con l'accesso **Editor** alle risorse VPC possono anche visualizzarle. Non è necessario aggiungere i membri ai gruppi di accesso **Editor** E **Visualizzatore**.

#### Configurazione dell'accesso Visualizzatore
{: #setting-up-viewer-access)

Le risorse `Floating IP` dell'infrastruttura VPC vengono create nel gruppo di risorse predefinito dell'account. Pertanto, gli utenti che devono gestire i `Floating IP` necessitano dell'accesso come **Visualizzatore** al gruppo di risorse predefinito dell'account.
{: tip}

Di seguito è riportata la procedura per creare l'accesso **Visualizzatore**:

1. Vai all'[IU Gruppo IAM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam#/groups){: new_window} nella console IBM Cloud.
2. Seleziona il gruppo di accesso desiderato (inizia con `test_team_manage_vpcs`).
3. Nella scheda **Politiche di accesso**, fai clic su **Assegna accesso**.
4. Seleziona **Assegna l'accesso ai servizi di gestione dell'account**.
7. Seleziona il servizio **Tutti i servizi di gestione account**.
9. Seleziona il ruolo **Visualizzatore**.
10. Seleziona **Assegna**.

Ripeti i passi precedenti per assegnare al gruppo di accesso `production_team_manage_vpcs` il ruolo **Visualizzatore** per **Tutti i servizi di gestione account**.

### Passo 4: aggiungi utenti ai gruppi di accesso
{: #step-4-add-users-to-the-access-groups}

Ora puoi assegnare i membri del team (utenti) ai gruppi di accesso appropriati. Attieniti alla seguente procedura per aggiungere ciascun membro del team di test al gruppo di accesso che consente la gestione VPC del team di test:

1. Vai all'[IU Gruppo IAM ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam#/groups){: new_window} nella console IBM Cloud.
2. Seleziona il gruppo di accesso desiderato (inizia con: `test_team_manage_vpcs`).
3. Nella scheda **Utenti**, fai clic su **Aggiungi utenti**.
4. Seleziona ciascun utente che desideri aggiungere al gruppo di accesso.
5. Fai clic su **Aggiungi al gruppo**.

Ripeti i passi precedenti per completare queste attività:
* Assegnare gli utenti desiderati al gruppo di accesso `test_team_view_vpcs`.
* Assegnare gli utenti desiderati al gruppo di accesso `production_team_manage_vpcs`.
* Assegnare gli utenti desiderati al gruppo di accesso `production_team_view_vpcs`.

Non è necessario assegnare agli utenti l'accesso **Visualizzatore** alle risorse VPC se dispongono dell'accesso di gestione.
{: tip}

## Passi successivi
{: #permissions-next-steps}

I due team di esempio sono ora configurati per utilizzare i VPC. A questo punto, i membri dei gruppi di accesso `test_team_manage_vpcs` e `production_team_manage_vpcs` possono creare i VPC nei loro gruppi di risorse assegnati.


## Link correlati
{: #permissions-related-links}

* [Gestione di identità e accesso](/docs/iam?topic=iam-getstarted)
* [Gestione di utenti e accesso](/docs/iam?topic=iam-iamuserinv)
* [Cos'è IAM](/docs/iam?topic=iam-iamoverview)
