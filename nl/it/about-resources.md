---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# Informazioni sulle risorse dell'infrastruttura VPC
{: #about-vpc-infrastructure-resources}

Il termine _risorse_ si riferisce alle parti componenti di una distribuzione di {{site.data.keyword.cloud}} VPC (Virtual Private Cloud). Ogni VPC può contenere i seguenti tipi di risorse, che possono essere raggruppate secondo necessità:

* **vpc**
* **istanza**
* **chiave**
* **immagine**
* **sottorete**
* **volume**
* **acl (access control list) di rete**
* **gruppo di sicurezza**
* **IP mobile**
* **vnic**
* **gateway**
* **programma di bilanciamento del carico**
* **vpn**

Alcune risorse dell'infrastruttura VPC ereditano le loro politiche di autorizzazione, che ne regolano l'utilizzo, dal VPC principale, ma altre vengono gestite separatamente.

Attualmente, le risorse di tipo `instance`, `key`, `security-group`, `volume`, `loadbalancer` e `vpn` mantengono le proprie politiche di autorizzazione. Ulteriori dettagli sulle autorizzazioni per queste risorse sono forniti più avanti in questo documento.
{: note}

## Politiche delle risorse
{: #resource-policies}

In generale, l'accesso alle risorse in un VPC è controllato dalle _politiche_. Se vuoi personalizzare l'accesso alle risorse per il tuo VPC, puoi creare delle politiche personalizzate. Una politica di risorsa può essere destinata a:

* tutte le risorse
* tutte le risorse di un tipo
* tutte le risorse in un gruppo
* una singola risorsa

## Risorse e gruppi di risorse
{: #resources-and-resource-groups}

Un _gruppo di risorse_ è una raccolta di risorse, ad esempio un intero VPC o una singola sottorete e il relativo ACL, associati allo scopo di stabilire l'autorizzazione e l'utilizzo. Puoi considerare un gruppo di risorse come una raccolta di infrastrutture che potrebbero essere utilizzate da un progetto, un reparto o un team.

Puoi utilizzare la CLI di ricerca per visualizzare le tag collegate a una risorsa. Con i filtri appropriati, puoi visualizzare solo le istanze che ti interessano, ad esempio `ibmcloud resource search name:` o `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Le grandi imprese potrebbero voler dividere un VPC in gruppi di risorse. Le aziende più piccole potrebbero non aver bisogno di dividere il VPC in gruppi di risorse, perché tutto il loro team utilizzerebbe l'intero VPC. Se hai familiarità con _OpenStack_, un gruppo di risorse è concettualmente simile a un _Progetto_ in _OpenStack Keystone_.

L'assegnazione di una risorsa a un gruppo di risorse può essere eseguita SOLO durante la creazione del tuo VPC. Le risorse non possono cambiare i gruppi di risorse dopo la loro creazione.
{: .note}

I gruppi di risorse sono progettati principalmente per le grandi organizzazioni. Per la maggior parte delle aziende e dei team, il breakout di risorse è nel limite del VPC. Questa regola generale può cambiare quando è possibile assegnare singole VSI (istanze) a un gruppo di risorse e singoli utenti a una risorsa VSI (istanza).

Quando configuri il tuo IBM Cloud VPC, se vuoi utilizzare i gruppi di risorse, è opportuno disporre di un piano preventivo per l'assegnazione delle risorse e degli utenti della tua organizzazione a ciascun gruppo di risorse.
{: tip}

## Specifiche per VPC
{: #specifics-for-vpc}

Alcune risorse dell'infrastruttura VPC ereditano le loro politiche di autorizzazione, che ne regolano l'utilizzo, dall'account o dal VPC principale, mentre altre hanno politiche individuali.

### Autorizzazione delle risorse VPC per tipo di risorsa
{: #vpc-resource-authorization-by-resource-type}

La tabella riepiloga in che modo le risorse VPC sono autorizzate per impostazione predefinita.

| Tipo di risorsa | Da dove ottiene la sua autorizzazione predefinita |
|-----------|------------|
| VPC | Controllo dell'autorizzazione quando creato |
| Programma di bilanciamento del carico | Controllo dell'autorizzazione quando creato |
| VPN | Controllo dell'autorizzazione quando creato |
| Sottorete | VPC principale |
| Gateway pubblico | VPC principale |
| Istanza | Controllo dell'autorizzazione quando creato |
| Gruppo di sicurezza | Controllo dell'autorizzazione quando creato |
| Chiave | Controllo dell'autorizzazione quando creato |
| Immagine | Account |
| IP mobile | Account |
| ACL di rete | Account|
| Volume | Controllo dell'autorizzazione quando creato |

Se non assegnati, è possibile creare l'IP mobile e gli ACL di rete con ambito a livello di account. Tuttavia, non appena un IP mobile viene assegnato a un'istanza o un ACL viene assegnato a una sottorete, queste risorse diventano soggette all'ambito di autorizzazione del VPC.
{: note}

### Copertura VPC dei ruoli e azioni autorizzate sulle risorse
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

Per uno scenario con 2 VPC, di seguito sono riportati alcuni esempi di ciò che accade quando un utente con determinati ruoli tenta di eseguire determinate azioni:

* Utente _admin_, con autorizzazione **Amministratore** su tutte le risorse
  * Crea `vpc1`: ok

* Utente _editor_, con autorizzazione **Editor** su tutte le risorse
  * Crea `vpc2`: ok
  * Aggiorna `vpc2`: ok
  * Elimina `vpc2`: ok

* Utente _operator_, con autorizzazione **Operatore** su tutte le risorse
  * Crea `vpc`: negato
  * Aggiorna `vpc1`: negato
  * Elimina `vpc1`: negato

* Utente _viewer_, con autorizzazione **Visualizzatore** su tutte le risorse
  * Elenca i VPC: vede [vpc1]
  * Legge `vpc1`: ok

* Utente _noRole_, senza politiche
  * Elenca i VPC: vede []
  * Legge `vpc1`: negato

### Tabella di riepilogo della copertura VPC
{: #vpc-coverage-summary-table}

Per qualsiasi politica che concede l'accesso in base al ruolo (Amministratore, Editor, Operatore, Visualizzatore), vengono fornite le seguenti azioni (X=negato, o=OK)

 ruolo:      | nessuno | Visualizzatore | Operatore | Editor | Amministratore
-----------:|------|--------|-------|--------|-------
Creazione      | X    | X      | X     | o      | o
Elenco        | X    | o      | X     | o      | o
Lettura        | X    | o      | X     | o      | o
Aggiornamento      | X    | X      | X     | o      | o
Eliminazione      | X    | X      | X     | o      | o


## Autorizzazione delle risorse di VPN for VPC
{: #resource-authorizations-of-vpn-for-vpc}

L'autorizzazione delle risorse di **VPN for VPC** viene configurata separatamente dagli altri tipi di autorizzazione delle risorse VPC, ma in modo simile.

Le politiche in VPN for VPC controllano l'accesso alle risorse VPN, in particolare ai gateway VPN. Per accedere alle risorse secondarie di un gateway VPN, quali connessioni VPN e CIDR locali o peer, devi disporre dell'accesso in **Lettura** (o superiore) al gateway VPN principale. Le politiche di risorsa possono essere utilizzate solo con le connessioni VPN che dispongono anche di accesso in **Lettura** o superiore per il gateway VPN principale. Una politica di risorsa in VPN può essere destinata a:

* tutte le risorse VPN (gateway VPN, connessioni VPN, politiche IKE e IPsec)
* tutte le risorse VPN in un gruppo di risorse
* un singolo gateway VPN

L'utilizzo della VPN viene anche fatturato separatamente.
{: note}

### Copertura di VPN for VPC dei ruoli e azioni autorizzate sulle risorse
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

Sono supportati gli stessi ruoli utente di quelli supportati nell'autorizzazione delle risorse VPC, ma con diverse azioni abilitate per ciascun ruolo.

Ecco alcuni esempi di ciò che accade quando gli utenti con determinati ruoli tentano di eseguire determinate azioni relative alla VPN:

* Utente _admin_, con autorizzazione **Amministratore** su tutte le risorse
  * Crea, aggiorna, elimina, legge, elenca i gateway VPN: ok
  * Crea, aggiorna, elimina, legge, elenca le connessioni VPN: ok
  * Crea, aggiorna, elimina, legge, elenca i CIDR peer e locali della connessione VPN: ok
  * Crea, aggiorna, elimina, legge, elenca le politiche IKE: ok
  * Crea, aggiorna, elimina, legge, elenca le politiche IPSec: ok

* Utente _editor_, con autorizzazione **Editor** su tutte le risorse
  * Uguali a quelle dell'Amministratore

* Utente _operator_, con autorizzazione **Operatore** su tutte le risorse
  * Crea, aggiorna, elimina i gateway VPN: negato
  * Crea, aggiorna, elimina le connessioni VPN: negato
  * Crea, aggiorna, elimina i CIDR peer e locali della connessione VPN: negato
  * Crea, aggiorna, elimina le politiche IKE: negato
  * Crea, aggiorna, elimina le politiche IPSec: negato
  * Legge, elenca i gateway VPN: ok - vede i dati reali
  * Legge, elenca le connessioni VPN: ok - vede i dati reali
  * Legge, elenca i CIDR peer e locali della connessione VPN: ok - vede i dati reali
  * Legge, elenca le politiche IKE: ok - vede i dati reali
  * Legge, elenca le politiche IPSec: ok - vede i dati reali

* Utente _viewer_, con autorizzazione **Visualizzatore** su tutte le risorse
  * Uguali a quelle dell'Operatore

* Utente _noRole_, senza politiche
  * Legge, elenca i gateway VPN: negato - l'elenco mostra i dati vuoti
  * Legge, elenca le connessioni VPN: negato
  * Legge, elenca i CIDR peer e locali della connessione VPN: negato
  * Legge, elenca le politiche IKE: negato - l'elenco mostra i dati vuoti
  * Legge, elenca le politiche IPSec: negato - l'elenco mostra i dati vuoti

### Tabella di riepilogo della copertura di VPN for VPC
{: #vpn-for-vpc-coverage-summary-table}

L'associazione tra azione e ruolo in VPN for VPC può essere visualizzata nella seguente tabella (X=negato, o=OK):

ruolo:       | nessuno | Visualizzatore | Operatore | Editor | Amministratore
-----------:|------|--------|-------|--------|-------
Creazione      | X    | X      | X     | o      | o
Elenco        | X    | o      | o     | o      | o
Lettura        | X    | o      | o     | o      | o
Aggiornamento      | X    | X      | X     | o      | o
Eliminazione      | X    | X      | X     | o      | o

## Autorizzazione delle risorse per Load Balancer for VPC
{: #resource-authorization-for-load-balancer-for-vpc}

L'autorizzazione delle risorse per l'utilizzo di **Load Balancer for VPC** viene configurata separatamente dalle altre autorizzazioni di risorse nel tuo VPC, ma in modo simile.

L'utilizzo del programma di bilanciamento del carico viene anche fatturato separatamente.
{: note}

### Copertura del programma di bilanciamento del carico dei ruoli e azioni autorizzate sulle risorse
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

Ecco alcuni esempi di ciò che accade quando gli utenti con determinati ruoli tentano di eseguire determinate azioni relative al programma di bilanciamento del carico di VPC:

* Utente _admin_, con autorizzazione **Amministratore** su tutte le risorse
  * Crea, aggiorna, elimina, legge, elenca i programmi di bilanciamento del carico: ok
  * Crea, aggiorna, elimina, legge, elenca i listener: ok
  * Crea, aggiorna, elimina, legge, elenca i pool: ok
  * Crea, aggiorna, elimina, legge, elenca i membri: ok
  * Crea, aggiorna, elimina, legge, elenca le statistiche del programma di bilanciamento del carico: ok

* Utente _editor_, con autorizzazione **Editor** su tutte le risorse
  * Uguali a quelle dell'Amministratore

* Utente _operator_, con autorizzazione **Operatore** su tutte le risorse
  * Crea, aggiorna, elimina, legge, elenca i programmi di bilanciamento del carico: negato
  * Crea, aggiorna, elimina, legge, elenca i listener: negato
  * Crea, aggiorna, elimina, legge, elenca i pool: negato
  * Crea, aggiorna, elimina, legge, elenca i membri: negato
  * Crea, aggiorna, elimina, legge, elenca le statistiche del programma di bilanciamento del carico: negato

* Utente _viewer_, con autorizzazione **Visualizzatore** su tutte le risorse
  * Legge, elenca i programmi di bilanciamento del carico: ok
  * Legge, elenca i listener: ok
  * Legge, elenca i pool: ok
  * Legge, elenca i membri: ok
  * Legge le statistiche del programma di bilanciamento del carico: ok

* Utente _noRole_, senza politiche
  * Legge, elenca i programmi di bilanciamento del carico: negato - vede i dati vuoti
  * Legge, elenca i listener: negato
  * Legge, elenca i pool: negato
  * Legge, elenca i membri: negato
  * Legge le statistiche del programma di bilanciamento del carico: negato

### Tabella di riepilogo della copertura del programma di bilanciamento del carico
{: #load-balancer-coverage-summary-table}

L'associazione tra azione e ruolo in Load Balancer for VPC può essere visualizzata nella seguente tabella (X=negato, o=OK):

ruolo:       | nessuno | Visualizzatore | Operatore | Editor | Amministratore
-----------:|------|--------|-------|--------|-------
Creazione      | X    | X      | X     | o      | o
Elenco        | X    | o      | X     | o      | o
Lettura        | X    | o      | X     | o      | o
Aggiornamento      | X    | X      | X     | o      | o
Eliminazione      | X    | X      | X     | o      | o


## Autorizzazione delle risorse per Virtual Server for VPC
{: #planning-virtual-servers-for-vpc-permissions}

Imposta l'autorizzazione della risorsa per **Virtual Server for VPC** separatamente dalle altre autorizzazioni di risorsa nel tuo VPC. 

L'utilizzo dei Virtual Server for VPC viene fatturato separatamente.
{: note}

* Come amministratore, puoi definire i ruoli ed eseguire qualsiasi azione disponibile su {{site.data.keyword.vsi_is_short}}.
* Come editor, puoi modificare lo stato e creare o eliminare risorse secondarie.
* Come operatore, puoi eseguire azioni che non modificano lo stato delle risorse.
* Come visualizzatore, puoi eseguire azioni che non modificano lo stato delle risorse.

### Tabella di riepilogo della copertura del server virtuale
{: #vsi-coverage-summary-table}

L'associazione tra azione e ruolo in Virtual Server for VPC può essere visualizzata nella seguente tabella (X=negato, o=OK):

ruolo:       | nessuno | Visualizzatore | Operatore | Editor | Amministratore
-----------:|------|--------|-------|--------|-------
Creazione      | X    | X      | X     | o      | o
Elenco        | X    | o      | o     | o      | o
Lettura        | X    | o      | o     | o      | o
Aggiornamento      | X    | X      | X     | o      | o
Eliminazione      | X    | X      | X     | o      | o

Quando crei un'istanza, devi disporre anche dell'accesso come Operatore per le risorse VPC e Gruppo di sicurezza, se tali risorse sono specificate. Le risorse Sottorete e IP mobile ereditano le autorizzazioni dal VPC associato.  
{: tip} 
