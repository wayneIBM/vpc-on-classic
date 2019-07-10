---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: create, configure, permissions, ACL, virtual, server, instance, subnet, block, storage, volume, security, group, images, Windows, Linux, example, monitoring, VPN, load balancer, IKE, IPsec

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

# Creazione di un VPC utilizzando la console {{site.data.keyword.cloud_notm}}
{: #creating-a-vpc-using-the-ibm-cloud-console}
[comment]: # (argomento della guida collegato)

Questo documento ti mostra come creare e configurare un {{site.data.keyword.cloud}} Virtual Private Cloud utilizzando la console {{site.data.keyword.cloud_notm}}.

Per creare e configurare il tuo VPC (virtual private cloud) e altre risorse collegate, completa le procedure nelle sezioni che seguono, nel seguente ordine:

1. Crea un VPC e una sottorete per definire la rete. Quando crei la tua sottorete, collega un gateway pubblico per consentire a tutte le risorse presenti nella sottorete di comunicare con l'Internet pubblico.
1. Configura un ACL (access control list) per limitare il traffico in entrata e in uscita della sottorete. Per impostazione predefinita, è consentito tutto il traffico.
1. Crea un'istanza del server virtuale.
1. Crea un volume di archiviazione blocchi e collegalo a un'istanza.
1. Configura un gruppo di sicurezza per definire il traffico in entrata e in uscita consentito per l'istanza.
1. Riserva e associa un indirizzo IP mobile per consentire alla tua istanza di essere raggiungibile da Internet.
1. Crea un programma di bilanciamento del carico per distribuire le richieste su più istanze.
1. Crea una VPN (virtual private network) in modo che il tuo VPC possa connettersi in modo sicuro a un'altra rete privata, come la rete in loco o un altro VPC.

## Prima di cominciare
{: #before}
Assicurati di disporre delle autorizzazioni necessarie per creare e gestire le risorse nel tuo VPC. Per ulteriori informazioni, vedi [Gestione delle autorizzazioni utente per le risorse VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Genera una chiave SSH, che verrà utilizzata per la connessione all'istanza del server virtuale. Ad esempio, genera una chiave SSH sul tuo server Linux eseguendo il comando `ssh-keygen -t rsa -C "user_ID"`. Tale comando genera due file. La chiave pubblica generata si trova nel file `<your key>.pub`.

Se intendi creare un programma di bilanciamento del carico e utilizzare HTTPs per il listener, è richiesto un certificato SSL. Puoi gestire i certificati con [IBM Certificate Manager ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/certificate-manager){: new_window}. Devi inoltre creare un'autorizzazione per consentire alla tua istanza del programma di bilanciamento del carico di accedere all'istanza di Certificate Manager che contiene il certificato SSL. Puoi creare un'autorizzazione tramite [Identity and Access Authorizations ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/iam/#/authorizations){: new_window}. Per l'origine, seleziona **VPC Infrastructure** come servizio di origine, **Load Balancer for VPC** come tipo di risorsa e **All resource instances** per l'istanza della risorsa di origine. Seleziona **Certificate Manager** come servizio di destinazione e assegna **Writer** per il ruolo di accesso al servizio. Imposta l'istanza del servizio di destinazione su **All instances** o sulla tua specifica istanza di Certificate Manager. Per ulteriori informazioni, vedi [Utilizzo dei programmi di bilanciamento del carico in IBM Cloud VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).

## Creazione di un VPC e di una sottorete

Per creare un VPC e una sottorete:

1. Apri la [console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}
1. Fai clic sull'**icona di Menu ![Icona Menu](../../icons/icon_hamburger.svg) > VPC Infrastructure > Network > VPCs** e fai clic su **New virtual private cloud**.
1. Immetti un nome per il VPC, ad esempio `my-vpc`.
1. Seleziona un gruppo di risorse per il VPC e tutte le sue risorse collegate. I gruppi di risorse ti consentono di organizzare le risorse del tuo account per scopi di controllo degli accessi e fatturazione. Per ulteriori informazioni, vedi [Procedure ottimali per organizzare le risorse in un gruppo di risorse](/docs/resources?topic=resources-bp_resourcegroups).
1. _Facoltativo:_ immetti delle tag per aiutarti a organizzare e trovare le tue risorse. Puoi aggiungere più tag in seguito. Per ulteriori informazioni, vedi [Gestione delle tag](/docs/resources?topic=resources-tag).
1. Seleziona o crea l'ACL predefinito per le nuove sottoreti in questo VPC. In questa esercitazione, creiamo un nuovo ACL predefinito. Successivamente, configureremo le regole per l'ACL.
1. Seleziona se il gruppo di sicurezza predefinito consente il traffico SSH e ping in entrata verso le istanze del server virtuale in questo VPC. Configureremo più regole per il gruppo di sicurezza predefinito in seguito.
1. Immetti un nome per la nuova sottorete nel tuo VPC, ad esempio `my-subnet`.
1. Seleziona un'ubicazione per la sottorete. L'ubicazione è composta da una regione e una zona.

    La regione che selezioni viene utilizzata come regione del VPC. Tutte le risorse aggiuntive che crei in questo VPC verranno create nella regione selezionata.
    {: tip}

1. Immetti un intervallo IP per la sottorete nella notazione CIDR, ad esempio: `10.240.0.0/24`. Nella maggior parte dei casi, puoi utilizzare l'intervallo IP predefinito. Se vuoi specificare un intervallo IP personalizzato, puoi utilizzare il calcolatore di intervalli IP per selezionare un prefisso di indirizzo diverso o modificare il numero di indirizzi.
1. Seleziona un ACL per la sottorete. Seleziona **Use VPC default** per utilizzare l'ACL predefinito creato per questo VPC.
1. Collega un gateway pubblico alla sottorete per consentire a tutte le risorse collegate di comunicare con l'Internet pubblico.  

    Puoi anche collegare il gateway pubblico dopo aver creato la sottorete.
    {: tip}

1. Fai clic su **Create virtual private cloud**.
1. Per creare un'altra sottorete in questo VPC, fai clic sulla scheda **Subnets** e quindi su **New subnet**. Quando definisci la sottorete, assicurati di selezionare `my_vpc` nel campo **Virtual private cloud**.

## Configurazione dell'ACL

Puoi configurare l'ACL per limitare il traffico in entrata e in uscita alla sottorete. Per impostazione predefinita, è consentito tutto il traffico.

Ogni sottorete può essere collegata a un solo ACL. Tuttavia, ogni ACL può essere collegato a più sottoreti.

Per configurare l'ACL:

1. Nel riquadro di navigazione, fai clic su **Network > Subnets**.
1. Fai clic sulla sottorete che hai creato.
1. Nell'area **Subnet details**, fai clic su l nome dell'ACL.
1. Fai clic su **Add rule** per configurare le regole in entrata e in uscita che definiscono il traffico consentito all'interno e all'esterno della sottorete. Per ogni regola, specifica le seguenti informazioni:  
   * Specifica la priorità della regola. Le regole con numeri inferiori vengono valutate per prime e sostituiscono le regole con numeri più alti. Ad esempio, se una regola con priorità 2 consente il traffico HTTP e una regola con priorità 5 nega tutto il traffico, il traffico HTTP è comunque consentito.  
   * Seleziona se consentire o negare il traffico specificato.  
   * Specifica un blocco CIDR per indicare l'intervallo IP per il quale si applica la regola.
   * Seleziona i protocolli e le porte a cui si applica la regola.
1. Quando hai finito di creare le regole, fai clic sul breadcrumb **All access control lists** nella parte superiore della pagina.

### ACL di esempio

Ad esempio, puoi configurare le regole in entrata che permettono di:

 1. Consentire il traffico HTTP da Internet
 1. Consentire tutto il traffico in entrata dalla sottorete 10.10.20.0/24
 1. Negare tutto il resto del traffico in entrata  

Quindi, configura le regole in uscita che permettono di:

1. Consentire il traffico HTTP verso Internet
1. Consentire tutto il traffico in uscita alla sottorete 10.10.20.0/24
1. Negare tutto il resto del traffico in uscita  

![Mostra le regole in entrata e in uscita di esempio](images/acl-rules.png)

## Creazione di un'istanza del server virtuale

Per creare un'istanza del server virtuale nella sottorete appena creata:

1. Fai clic su **Compute > Virtual server instance** nel riquadro di navigazione e fai clic su **New instance**.
1. Immetti un nome per l'istanza, ad esempio `my-instance`.
1. Seleziona il VPC che hai creato.
1. Nel campo **Location**, seleziona la zona in cui creare l'istanza.
1. Seleziona un'immagine (cioè, sistema operativo e versione), ad esempio Ubuntu Linux 16.04.
1. Per impostare la dimensione dell'istanza, seleziona uno dei profili più diffusi o fai clic su **All profiles** per scegliere una diversa combinazione di core e RAM più appropriata per il tuo carico di lavoro.
1. Seleziona una chiave SSH esistente o aggiungi una nuova chiave SSH che verrà utilizzata per accedere all'istanza del server virtuale. Per aggiungere una chiave SSH, fai clic su **New key** e specifica un nome per la chiave. Dopo aver immesso il valore della chiave pubblica precedentemente generata, fai clic su **Add SSH key**.

Le chiavi possono essere aggiunte solo inizialmente come parte della creazione della VSI. Non esistono strumenti per aggiungere le chiavi in un secondo momento.
{:tip}

1. _Facoltativo:_ immetti i dati utente per eseguire attività di configurazione comuni all'avvio dell'istanza. Ad esempio, puoi specificare le direttive cloud-init o gli script shell per le immagini Linux. Per ulteriori informazioni, vedi [Dati utente](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-user-data).
1. Nota il volume di boot. Nella release corrente, vengono destinati 100 GB al volume di boot. L'opzione *Auto Delete* è abilitata per il volume; questo verrà eliminato automaticamente se si elimina l'istanza.
1. Nell'area **Attached block storage volume**, puoi fare clic su **New block storage volume** per collegare un volume di archiviazione blocchi alla tua istanza. In questa esercitazione, creeremo un volume di archiviazione blocchi e successivamente lo collegheremo all'istanza.
1. Nell'area **Network interfaces**, puoi modificare l'interfaccia di rete e cambiarne il nome. Se hai più sottoreti nella zona e nel VPC selezionati, puoi collegare una sottorete diversa all'interfaccia. Se vuoi che l'istanza esista in più sottoreti, puoi creare più interfacce.

   Puoi anche selezionare i gruppi di sicurezza da collegare a ciascuna interfaccia. Per impostazione predefinita, è collegato il gruppo di sicurezza predefinito del VPC. Il gruppo di sicurezza predefinito consente il traffico SSH e ping in entrata, tutto il traffico in uscita e tutto il traffico tra le istanze all'interno del gruppo. Tutto il resto del traffico è bloccato; puoi configurare le regole per consentire più traffico. Se in seguito modifichi le regole del gruppo di sicurezza predefinito, tali regole aggiornate verranno applicate a tutte le istanze correnti e future del gruppo.

1. Fai clic su **Create virtual server instance**. Lo stato dell'istanza è inizialmente *Pending*, poi diventa *Stopped* e infine *Powered On*. Potresti dover aggiornare la pagina per vedere la modifica dello stato.

## Creazione e collegamento di un volume di archiviazione blocchi

Puoi creare un volume di archiviazione blocchi e collegarlo alla tua istanza del server virtuale.

Per creare e collegare un volume di archiviazione blocchi:

1. Nel riquadro di navigazione, fai clic su **Storage > Block storage volumes**.
1. Nella pagina Block storage volumes for VPC, fai clic su **New volume** e specifica le seguenti informazioni.
  * **Name**: immetti un nome per il volume di archiviazione blocchi, ad esempio `data-volume-1`.  
  * **Resource group**: seleziona un gruppo di risorse per il volume di archiviazione blocchi. I gruppi di risorse ti consentono di organizzare le risorse del tuo account per scopi di controllo degli accessi e fatturazione. Per ulteriori informazioni, vedi [Procedure ottimali per organizzare le risorse in un gruppo di risorse](/docs/resources?topic=resources-bp_resourcegroups).
  * **Tags**: _facoltativo:_ immetti delle tag per aiutarti a organizzare e trovare le tue risorse. Puoi aggiungere più tag in seguito. Per ulteriori informazioni, vedi [Gestione delle tag](/docs/resources?topic=resources-tag).
  * **Location**: seleziona un'ubicazione per il volume di archiviazione blocchi. L'ubicazione è composta da una regione e una zona, ad esempio Stati Uniti Sud 1.
  * **Size**: specifica la dimensione del volume tra 10 GB e 2000 GB.
  * **IOPs**: seleziona uno dei livelli IOPS o fai clic su Custom per immettere un valore IOPS in base alla dimensione del volume.
  * **Encryption**: accetta la crittografia gestita dal provider predefinita o seleziona Customer managed e utilizza la tua propria chiave di crittografia. Questo passo richiede il provisioning di un'istanza del servizio Key Protect e la creazione o l'importazione di una chiave root. Per ulteriori informazioni, vedi [Creazione di volumi di archiviazione blocchi con crittografia gestita dal cliente](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption#data-vol-encryption-ui).
1. Fai clic su **Create volume**.
1. Nell'elenco dei volumi di archiviazione blocchi, trova il volume che hai creato. Quando lo stato è Available, fai clic su "..." e seleziona **Attach to instance**.
1. Seleziona l'istanza a cui vuoi collegare il volume e fai clic su **Attach**.

## Configurazione del gruppo di sicurezza per l'istanza

Puoi configurare il gruppo di sicurezza per definire il traffico in entrata e in uscita consentito per l'istanza. Ad esempio, dopo aver configurato le regole ACL per la sottorete in base alle politiche di sicurezza della tua azienda, puoi limitare ulteriormente il traffico per istanze specifiche a seconda dei loro carichi di lavoro.

Per configurare il gruppo di sicurezza:

1. Nella pagina Virtual server instances, fai clic sulla tua istanza per visualizzarne i dettagli.
1. Nella sezione **Network interfaces**, fai clic sul gruppo di sicurezza.
1. Fai clic su **Add rule** per configurare le regole in entrata e in uscita che definiscono il tipo di traffico consentito da e verso l'istanza. Per ogni regola, specifica le seguenti informazioni:  
   * Specifica un blocco CIDR o un indirizzo IP per il traffico consentito. In alternativa, puoi specificare un gruppo di sicurezza nello stesso VPC per consentire il traffico da o verso tutte le istanze collegate al gruppo di sicurezza selezionato.   
   * Seleziona i protocolli e le porte a cui si applica la regola.    

   **Suggerimenti:**  
  * Vengono valutate tutte le regole, indipendentemente dall'ordine in cui sono state aggiunte.
  * Le regole sono con stato, il che significa che il traffico di ritorno in risposta al traffico consentito viene automaticamente autorizzato. Ad esempio, una regola che consente il traffico TCP in entrata sulla porta 80 consente anche di replicare il traffico TCP in uscita sulla porta 80 all'host di origine, senza la necessità di una regola aggiuntiva.
1. _Facoltativo:_ se vuoi collegare questo gruppo di sicurezza ad altre istanze, fai clic su **Attached interfaces** nel riquadro di navigazione e seleziona le interfacce aggiuntive.
1. Quando hai finito di creare le regole, fai clic sul breadcrumb **All security groups** nella parte superiore della pagina.

### Gruppo di sicurezza di esempio  

Ad esempio, puoi configurare le regole in entrata che permettono di:

 * Consentire tutto il traffico SSH (porta TCP 22)
 * Consentire tutto il traffico ping (tipo di ICMP 8)

Quindi, configura le regole in uscita che consentono tutto il traffico TCP.

![Mostra le regole in entrata e in uscita di esempio](images/sg-example-ui.png)

## Riserva di un indirizzo IP mobile

Riserva e associa un indirizzo IP mobile per consentire alla tua istanza di essere raggiungibile da Internet.  

È necessario che la tua istanza sia in esecuzione prima di poter associare un indirizzo IP mobile. Potrebbero essere necessari alcuni minuti prima che l'istanza sia operativa.
{: tip}

Per riservare e associare un indirizzo IP mobile:

1. Nella pagina Virtual server instances, fai clic sulla tua istanza per visualizzarne i dettagli.
1. Nella sezione **Network interfaces**, fai clic su **Reserve** per l'interfaccia che vuoi associare a un indirizzo IP mobile.

Se in seguito vuoi riassegnare questo indirizzo IP mobile a un'altra istanza nella stessa zona, individua l'indirizzo IP mobile nella pagina **Network > Floating IPs**, fai clic sul suo menu di overflow (**...**) e seleziona **Unassociate**. Quindi, fai clic su **Associate** per selezionare l'istanza e l'interfaccia di rete che vuoi associare all'indirizzo IP mobile.
{: tip}

## Connessione alla tua istanza
Utilizzando l'indirizzo IP mobile che hai creato, esegui il ping della tua istanza per assicurarti che sia operativa:

```
ping <public-ip-address>
```
{:pre}


### Connessione alle immagini Linux

Poiché hai creato la tua istanza con una chiave SSH pubblica, puoi ora connetterti ad essa direttamente utilizzando la tua chiave privata:


```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

Vedi [Connessione alla tua istanza utilizzando Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) per ulteriori informazioni su come connetterti alla tua istanza.

### Connessione alle immagini Windows
Per connetterti a un'immagine Windows, accedi utilizzando la sua password decrittografata. Per ottenere la password decrittografata, copia la password crittografata dalla pagina dei dettagli dell'istanza e immetti il seguente comando:

```
# Decode the encrypted password
cat ~/examplepwd | base64 --decode > ~/examplepwd64
# Decrypt the decoded password using the RSA private key
openssl pkeyutl -in ~/examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
-pkeyopt rsa_mgf1_md:sha256
```
{:codeblock}


Vedi [Connessione alla tua istanza Windows](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-windows-instance) per ulteriori informazioni su come connetterti alla tua istanza.



## Monitoraggio della tua istanza

Per un log delle attività che mostra quando l'istanza è stata avviata, arrestata o riavviata, fai clic su **Activity** nel riquadro di navigazione.

## Creazione di un programma di bilanciamento del carico
Puoi creare un programma di bilanciamento del carico per distribuire il traffico in entrata su più istanze.

Per creare un programma di bilanciamento del carico:
1. Nel riquadro di navigazione, fai clic su **Network > Load balancers**.
1. Nella pagina Load balancers, fai clic su **New load balancer** e specifica le seguenti informazioni.
    * **Name**: immetti un nome per il programma di bilanciamento del carico, ad esempio `my-load-balancer`.
    * **Virtual private cloud**: seleziona il tuo VPC.
    * **Resource group**: seleziona un gruppo di risorse per il programma di bilanciamento del carico.
    * **Type**: seleziona il tipo di programma di bilanciamento del carico, pubblico o privato.
    * **Region**: indica la regione in cui verrà creato il programma di bilanciamento del carico, ovvero la regione selezionata per il tuo VPC.
    * **Subnets**: seleziona le sottoreti in cui creare il tuo programma di bilanciamento del carico. Per massimizzare la disponibilità della tua applicazione, seleziona sottoreti in zone diverse.
1. Fai clic su **New pool** e specifica le seguenti informazioni per creare un pool di back-end. Puoi creare uno o più pool.
    * **Name**: immetti un nome per il pool, ad esempio `my-pool`.
    * **Protocol**: seleziona il protocollo per le tue istanze di back-end dietro il programma di bilanciamento del carico. Il protocollo del pool deve corrispondere al protocollo del listener associato. Ad esempio, se per il listener è selezionato un protocollo HTTPS o HTTP, il protocollo del pool deve essere HTTP. Allo stesso modo, se il protocollo del listener è TCP, il protocollo del pool di back-end deve essere TCP.
    * **Method**: seleziona come vuoi che il programma di bilanciamento del carico distribuisca il traffico tra le istanze di back-end:
      * **Round robin:** inoltra a turno le richieste a ciascuna istanza. Tutte le istanze ricevono approssimativamente un numero uguale di connessioni client.
      * **Weighted Round robin:** inoltra le richieste a ciascuna istanza in proporzione al peso assegnato. Ad esempio, se hai le istanze A, B e C e il loro peso è impostato su 60, 60 e 30, le istanze A e B ricevono un numero uguale di connessioni e l'istanza C riceve la metà delle connessioni.
      * **Least connections:** inoltra le richieste all'istanza con il minor numero di connessioni al momento attuale.  
    * **Session stickiness**: seleziona se tutte le richieste durante la sessione di un utente vengono inviate alla stessa istanza.  
    * **Health checks**: configura il modo in cui il programma di bilanciamento del carico controlla l'integrità delle istanze.
      * **Health protocol**: il protocollo utilizzato dal programma di bilanciamento del carico per inviare i messaggi di controllo di integrità alle istanze di back-end.
      * **Health path**: il percorso di integrità (Health path) è applicabile solo se HTTP è selezionato come protocollo del controllo di integrità (Health protocol). Il percorso di integrità specifica l'URL utilizzato dal programma di bilanciamento del carico per inviare le richieste di controllo di integrità HTTP alle istanze di back-end. Per impostazione predefinita, i controlli di integrità vengono inviati al percorso root (`/`).
      * **Interval**: intervallo in secondi tra due tentativi di controllo di integrità consecutivi. Per impostazione predefinita, i controlli di integrità vengono inviati ogni cinque secondi.
      * **Timeout**: tempo massimo di attesa del sistema per una risposta da una richiesta di controllo di integrità. Per impostazione predefinita, il programma di bilanciamento del carico attende due secondi per una risposta.
      * **Max retries**: numero massimo di tentativi di controllo di integrità aggiuntivi eseguiti dal programma di bilanciamento del carico prima di dichiarare non integra un'istanza di back-end. Per impostazione predefinita, un'istanza non è più considerata integra dopo due controlli di integrità non riusciti.

        Sebbene il programma di bilanciamento del carico interrompa l'invio di connessioni a istanze non integre, il programma di bilanciamento del carico continua a monitorare l'integrità di queste istanze e ne ripristina l'utilizzo se vengono rilevate nuovamente integre superando correttamente due tentativi di controllo di integrità consecutivi.

      Se le istanze di back-end non sono integre e ritieni che la tua applicazione sia correttamente in esecuzione, ricontrolla i valori del protocollo di integrità e del percorso di integrità. Controlla anche eventuali gruppi di sicurezza collegati alle istanze per garantire che le regole consentano il traffico tra il programma di bilanciamento del carico e le istanze.
      {: tip}

1. Fai clic su **Create**.
1. Accanto alla voce per il nuovo pool, fai clic su **Attach** nella colonna **Instances** per aggiungere un'istanza al pool. Fai clic su **Add** per aggiungere altre istanze al pool. Specifica le seguenti informazioni per ogni istanza:
   * Seleziona una o più sottoreti da cui selezionare un'istanza.
   * Seleziona un'istanza. Se un'istanza ha più interfacce, assicurati di selezionare l'indirizzo IP corretto.
   * Specifica la porta su cui viene inviato il traffico all'istanza.
   * Se il tuo pool utilizza il metodo **Weighted round robin**, assegna un peso per ogni istanza.  

      Assegnare un peso '0' a un'istanza significa che non verranno inoltrate nuove connessioni a tale istanza, ma qualsiasi traffico esistente continuerà a fluire finché la connessione corrente è attiva. L'uso di un peso pari a '0' può aiutare a disattivare normalmente un'istanza e a rimuoverla dalla rotazione del servizio.
      {: tip}

1. Fai clic su **New listener** e specifica le seguenti informazioni per creare un listener. Puoi creare uno o più listener.
   * **Protocol**: il protocollo da utilizzare per ricevere le richieste in ingresso.
   * **Port**: la porta di attesa su cui vengono ricevute le richieste. L'intervallo di porte compreso tra 56500 e 56520 è riservato per scopi di gestione e non può essere utilizzato.
   * **Back-end pool**: il pool di back-end a cui questo listener inoltra il traffico.
   * **Max connections** (facoltativo): numero massimo di connessioni simultanee consentite dal listener.
   * **SSL certificate**: se HTTPS è il protocollo selezionato per questo listener, devi selezionare un certificato SSL. Assicurati che il programma di bilanciamento del carico sia autorizzato ad accedere al certificato SSL. Per istruzioni, vedi [Prima di cominciare](#before).
1. Fai clic su **Create**.
1. Dopo aver completato la creazione di pool e listener, fai clic su **Create load balancer**.
1. Per visualizzare i dettagli di un programma di bilanciamento del carico esistente, fai clic sul nome del programma di bilanciamento del carico nella pagina **Load balancers**.

## Creazione di una VPN
Puoi creare una VPN (virtual private network) in modo che il tuo VPC possa connettersi in modo sicuro a un'altra rete privata, come una rete in loco o un altro VPC.

Per visualizzare un esempio di codice, vedi [Utilizzo della VPN con il tuo VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
{: tip}

Per creare una VPN:
1. Nel riquadro di navigazione, fai clic su **Network > VPNs**.
1. Nella pagina VPN, fai clic su **New VPN gateway** e specifica le seguenti informazioni:
    * **Name**: immetti un nome per il gateway VPN nel tuo VPC (virtual private cloud), ad esempio `my-vpn-gateway`.
    * **Virtual private cloud**: seleziona il tuo VPC.
    * **Resource group**: seleziona un gruppo di risorse per il VPN.
    * **Subnet**: seleziona la sottorete in cui creare il gateway VPN.
1. Nella sezione **New VPN connection**, definisci una connessione tra questo gateway e una rete esterna al tuo VPC specificando le seguenti informazioni.
    * **Connection name**: immetti un nome per la connessione, ad esempio `my-connection`.
    * **Peer gateway address**: specifica l'indirizzo IP del gateway VPN per la rete esterna al tuo VPC.
    * **Preshared key**: specifica la chiave di autenticazione del gateway VPN per la rete esterna al tuo VPC.
    * **Local subnets**: specifica una o più sottoreti nel VPC che vuoi connettere tramite il tunnel VPN.
    * **Peer subnets**: specifica una o più sottoreti nell'altra rete che vuoi connettere tramite il tunnel VPN.
1. Per configurare il modo in cui il gateway del cloud invia i messaggi per verificare che il gateway del peer sia attivo, specifica le seguenti informazioni nella sezione **Dead peer detection**.
    * **Dead peer detection action**: l'azione da intraprendere se un gateway del peer smette di rispondere. Ad esempio, seleziona **Restart** se vuoi che il gateway rinegozi immediatamente la connessione.
    * **Interval**: la frequenza con cui controllare che il gateway del peer sia attivo. Per impostazione predefinita, i messaggi vengono inviati ogni 30 secondi.
    * **Timeout**: il tempo di attesa per una risposta dal gateway del peer. Per impostazione predefinita, un gateway del peer non è più considerato attivo se non viene ricevuta una risposta entro 150 secondi.
1. Specifica i parametri di sicurezza IKE e IPsec per la connessione.
    * Seleziona **Auto** se vuoi che il gateway del cloud provi a stabilire automaticamente la connessione.
    * Seleziona o crea politiche personalizzate se hai bisogno di applicare requisiti di sicurezza particolari o se il gateway VPN per l'altra rete non supporta le proposte di sicurezza che vengono provate tramite la negoziazione automatica.

  **Importante**: i parametri di sicurezza IKE e IPsec che specifichi per la connessione devono essere gli stessi parametri impostati sul gateway per la rete esterna al tuo VPC.

## Congratulazioni!

Hai creato e configurato correttamente un VPC e una sottorete, un ACL, un'istanza del server virtuale, un volume di archiviazione blocchi, un gruppo di sicurezza, un indirizzo IP mobile, un programma di bilanciamento del carico e una VPN. Puoi continuare a sviluppare il tuo VPC aggiungendo ulteriori istanze, sottoreti e altre risorse.
