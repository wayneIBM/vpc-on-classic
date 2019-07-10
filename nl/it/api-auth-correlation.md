---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

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

# Autorizzazioni delle risorse richieste per le chiamate API e CLI
{: #resource-authorizations-required-for-api-and-cli-calls}

La tabella riportata di seguito elenca le autorizzazioni richieste per interagire con gli oggetti dell'infrastruttura {{site.data.keyword.cloud}} Virtual Private Cloud. Queste informazioni sono particolarmente utili quando effettui chiamate API. Ecco quello che devi sapere per usare questa tabella:

I termini _collegato_ o _non collegato_ indicano se la risorsa è associata a uno o più VPC (direttamente o indirettamente tramite le sottoreti della risorsa). Un IP mobile o un ACL di rete non collegato non ha alcuna sottorete e, pertanto, non è associato ad alcun VPC, quindi l'autorizzazione di VPC non è applicabile.

* Le azioni di **Visualizzazione** ed **Elenco** corrispondono ai ruoli di **Visualizzatore**.
* Le azioni di **Operatività** corrispondono al ruolo di **Operatore**.
* **Aggiornamento** e le azioni correlate corrispondono ai ruoli di **Editor** o **Amministratore**.
* Le risorse sono protette da autorizzazione, gli oggetti di riferimento delle risorse non lo sono (ID, CRN e così via).


| Risorsa | Operazione | Autorizzazione richiesta |
|--------|--------|---------|
| VPC | Creazione | Autorizzazione di visualizzazione sul gruppo di risorse per tale VPC e autorizzazione di aggiornamento sulle risorse VPC|
| VPC | Aggiornamento, Eliminazione |  Autorizzazione di aggiornamento su VPC |
| VPC |  Visualizzazione, Elenco | Autorizzazione di visualizzazione su VPC  |
| ACL predefinito, SG predefinito VPC|  Visualizzazione, Elenco | Autorizzazione di visualizzazione su VPC |
| Prefissi indirizzo VPC |  Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento su VPC |
| Prefissi indirizzo VPC |  Visualizzazione, Elenco | Autorizzazione di visualizzazione su VPC  |
|—————|——————|———————|
| IP mobile (non associato) | Creazione, Aggiornamento, Eliminazione | Qualsiasi utente dell'account e autorizzazione di visualizzazione su tutti i servizi di gestione account, se la risorsa è creata nel gruppo di risorse predefinito. Fai clic [qui](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources#setting-up-viewer-access) per istruzioni sulla configurazione dell'accesso come visualizzatore. **Nota: l'associazione e l'annullamento dell'associazione fanno parte dell'operazione di aggiornamento dell'IP mobile**|
| IP mobile (non associato) | Visualizzazione, Elenco | Utente dell'account |
| IP mobile (associato) | Aggiornamento | Autorizzazione di aggiornamento per il VPC della sottorete associata (non puoi creare o eliminare un IP mobile dopo che è stato associato) |
| IP mobile (associato) | Visualizzazione, Elenco | Autorizzazione di visualizzazione per il VPC della sottorete associata dell'IP mobile | 
|——————|———————|————————|
| ACL di rete (non collegato), regole ACL | Creazione, Aggiornamento, Eliminazione | Qualsiasi utente dell'account |
| ACL di rete (non collegato), regole ACL | Visualizzazione, Elenco | Qualsiasi utente dell'account |
| ACL di rete (collegato), regole ACL | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento su tutte le sottoreti collegate associate al VPC |
| ACL di rete (collegato), regole ACL | Visualizzazione, Elenco | Autorizzazione di visualizzazione su almeno una delle sottoreti collegate associate al VPC |
|——————|———————|————————|
| Gateway pubblico | Creazione, Aggiornamento, Eliminazione |  Autorizzazione di aggiornamento sul VPC del PGW |
| Gateway pubblico | Visualizzazione, Elenco | Autorizzazione di visualizzazione sul VPC del PGW |
|—————————|————————|———————————|
| Area geografica | Visualizzazione, Elenco |  Per le regioni e le zone, qualsiasi utente dell'account |
|———————|————————|—————————|
| Chiave SSH | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per la chiave SSH |
| Chiave SSH | Visualizzazione, Elenco | Autorizzazione di visualizzazione per la chiave SSH |
|————————|—————————|————————|
| Sottorete | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per il VPC della sottorete |
| Sottorete | Visualizzazione, Elenco | Autorizzazione di visualizzazione per il VPC della sottorete |
| ACL o PGW di sottorete | Collegamento, Scollegamento | Autorizzazione di aggiornamento per il VPC della sottorete |
| ACL o PGW di sottorete | Visualizzazione, Elenco | Autorizzazione di visualizzazione per il VPC della sottorete |
|——————|—————————|————————|
| Gruppo di sicurezza | Richiamo   | Autorizzazione di visualizzazione sul gruppo di sicurezza.
| Gruppo di sicurezza | Elenco    | Autorizzazione di visualizzazione sul gruppo di sicurezza (altrimenti il gruppo viene omesso dalla risposta.)
| Gruppo di sicurezza | Creazione  | Autorizzazione di modifica sul gruppo di sicurezza che verrà creato (ad esempio, Autorizzazione di modifica su tutti i gruppi di sicurezza oppure Autorizzazione di modifica sul gruppo di risorse in cui verrà creato il gruppo di sicurezza.)<br />Autorizzazione di visualizzazione sul VPC in cui verrà creato il gruppo.
| Gruppo di sicurezza | Aggiornamento / Eliminazione  | Autorizzazione di modifica sul gruppo di sicurezza.
| Regola gruppo di sicurezza | Richiamo / Elenco | Autorizzazione di visualizzazione sul gruppo di sicurezza.
| Regola gruppo di sicurezza | Creazione / Aggiornamento / Eliminazione | Autorizzazione di modifica sul gruppo di sicurezza.
| Interfaccia di rete gruppo di sicurezza | Richiamo   | Autorizzazione di visualizzazione sul gruppo di sicurezza.<br />Autorizzazione di visualizzazione sull'istanza.
| Interfaccia di rete gruppo di sicurezza | Elenco    | Autorizzazione di visualizzazione sul gruppo di sicurezza (altrimenti genera una risposta 404.)<br />Autorizzazione di visualizzazione sull'istanza a cui appartiene l'interfaccia di rete (altrimenti l'interfaccia viene omessa dalla risposta.)
| Interfaccia di rete gruppo di sicurezza | Collegamento / Scollegamento | Autorizzazione di operatività sul gruppo di sicurezza. <br />Autorizzazione di modifica sull'istanza a cui appartiene l'interfaccia di rete.
|—————————|—————————|—————————|
| Immagini | Visualizzazione, Elenco  | Qualsiasi utente dell'account |
|—————————|—————————|—————————|
| Istanze | Creazione| Autorizzazione di aggiornamento per l'istanza e il volume<br />Autorizzazione di operatività per VPC<br />Autorizzazione di operatività per i gruppi di sicurezza, se specificati|
| Istanze | Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per l'istanza |
| Istanze | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per l'istanza |
| Azioni istanza | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per l'istanza |
| Azioni, inizializzazione, NIC istanza| Visualizzazione, Elenco  | Autorizzazione di visualizzazione per l'istanza |
| FIP istanza | Visualizzazione, Elenco | Autorizzazione di visualizzazione per l'istanza e il VPC della sottorete associata |
| FIP istanza | Associazione | Autorizzazione di aggiornamento per l'istanza <br />Autorizzazione di operatività per il VPC della sottorete associata|
| FIP istanza | Annullamento associazione | Autorizzazione di aggiornamento per l'istanza |
|————————|——————|————————|
| Gateway VPN | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per la VPN |
| Gateway VPN | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per la VPN |
| Connessioni gateway VPN | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per la VPN |
| Connessioni gateway VPN | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per la VPN |
| Politiche IKE, politiche IPSec e connessioni gateway VPN | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per la VPN |
| Politiche IKE, politiche IPSec e connessioni gateway VPN|Visualizzazione, Elenco  | Autorizzazione di visualizzazione per la VPN |
|————————|——————|————————|
| Programma di bilanciamento del carico | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per il programma di bilanciamento del carico |
| Programma di bilanciamento del carico | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per il programma di bilanciamento del carico |
| Pool e listener del programma di bilanciamento del carico | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per il programma di bilanciamento del carico |
| Pool e listener del programma di bilanciamento del carico | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per il programma di bilanciamento del carico |
|————————|——————|————————|
| Volumi | Creazione, Aggiornamento, Eliminazione | Autorizzazione di aggiornamento per il volume
| Volumi | Visualizzazione, Elenco  | Autorizzazione di visualizzazione per il volume |
| Profili volume | Visualizzazione, Elenco  | Qualsiasi utente dell'account |


