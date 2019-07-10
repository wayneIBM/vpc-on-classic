---

copyright:

  years: 2018, 2019
lastupdated: "2019-06-11"

keywords: error, message, API, limitations, rias, support

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# Messaggi di errore dell'API IBM Cloud Virtual Private Cloud
{: #rias-error-messages}

Quando ricevi un messaggio di errore dalle API {{site.data.keyword.cloud}} Virtual Private Cloud, puoi utilizzare l'ID messaggio per trovare ulteriori informazioni su come risolvere il problema.
{:shortdesc}

## account_type_invalid
**Messaggio**: devi avere un account Pagamento a consumo per eseguire il provisioning di un Virtual Private Cloud.

Il tuo account deve essere su un piano Pagamento a consumo per eseguire il provisioning dei VPC. Puoi trovare ulteriori dettagli sull'upgrade del tuo account qui: [Esegui un upgrade del tuo account](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**Messaggio**: non è possibile eliminare l'ACL di rete perché è collegato alle risorse.

Non è possibile eliminare l'ACL di rete perché è collegato a una sottorete o a un VPC.

Per vedere se una sottorete sta utilizzando l'ACL di rete, utilizza l'API `GET /v1/subnets?version=2019-05-31&generation=1`.  Comando della CLI equivalente: `ibmcloud is subnets`.  Per aggiornare l'ACL di rete utilizzato da una sottorete, utilizza l'API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` o la CLI `ibmcloud is subnet-update`.

Per vedere se un VPC sta utilizzando l'ACL di rete, utilizza l'API `GET /v1/vpcs?version=2019-05-31&generation=1`.  Comando della CLI equivalente: `ibmcloud is vpcs`. Non è possibile aggiornare o eliminare l'ACL di rete predefinito utilizzato da un VPC. L'ACL di rete predefinito viene eliminato automaticamente quando elimini il VPC.

## address_prefix_conflict
**Messaggio**: il prefisso dell'indirizzo con questo CIDR è in uso.

Il CIDR per il prefisso dell'indirizzo richiesto è in conflitto con un prefisso di indirizzo esistente.

Per visualizzare l'elenco di prefissi di indirizzo per un VPC, utilizza l'API `GET /vpcs/{vpc-id}/address_prefixes` e controlla che l'indirizzo CIDR richiesto non sia utilizzato dal campo `cidr` di un altro prefisso di indirizzo.
Comando della CLI equivalente: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**Messaggio**: impossibile eliminare un prefisso di indirizzo in uso.

Una o più sottoreti stanno utilizzando il prefisso dell'indirizzo.  Per determinare quali sottoreti stanno utilizzando il prefisso di indirizzo, utilizza il comando API `GET /v1/subnets?version=2019-05-31&generation=1` e controlla il campo `ipv4_cidr_block`.  Comando della CLI equivalente:  `ibmcloud is subnets` e controlla la colonna `Subnet CIDR`. Devi eliminare tutte le sottoreti che utilizzano il prefisso di indirizzo prima di poter eliminare il prefisso di indirizzo.

## backend_service_unavailable
**Messaggio**: il servizio di back-end non è disponibile.

Un servizio cloud di back-end utilizzato da VPC non ha risposto. VPC utilizza più servizi IBM Cloud, ad esempio:

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Catalogo globale](https://{DomainName}/catalog)
- Controller delle risorse
- Gestore delle risorse

Puoi controllare lo [stato](https://{DomainName}/status) dei servizi IBM Cloud e riprovare dopo qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_field
**Messaggio**: è necessario fornire l'UUID istanza corretto

Uno dei valori forniti nella richiesta non è corretto. Controlla il valore `target` nell'errore restituito per indizi sul parametro non corretto. In alcuni casi, non è possibile trovare l'UUID o il volume nella richiesta. Fornisci un valore valido e riprova.

Fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per ulteriore assistenza. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_request
**Messaggio**: le informazioni fornite non erano valide, non erano corrette o mancava un campo obbligatorio.

La richiesta non era nel formato previsto. Utilizza la [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per aiutarti a formattare la richiesta.

Se stai seguendo la specifica ma ottieni comunque l'errore, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Messaggio**: è possibile creare un solo VPC con accesso classico per ogni regione.

È possibile creare un solo VPC con accesso classico per ogni regione. Per elencare i VPC con accesso classico, esegui `ibmcloud is vpcs --classic-access`. È necessario eliminare il VPC con accesso classico esistente prima di poterne creare un altro. 

## classic_access_vpc_account_not_VRF_enabled
**Messaggio**: l'account deve essere abilitato per VRF per creare un VPC con accesso classico.

L'account classico collegato non è abilitato per VRF. Un VPC con accesso classico richiede che l'account classico collegato sia abilitato per VRF. Per abilitare il tuo account per VRF, fai riferimento a [VRF on IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_found
**Messaggio**: prefisso dell'indirizzo predefinito non trovato.

Potresti visualizzare questo messaggio di errore quando il prefisso dell'indirizzo predefinito non viene trovato.

## duplicate_error
**Messaggio**: l'input fornito esiste già.

La risorsa specificata esiste già. Prova a utilizzare un nome diverso per la risorsa che vuoi creare. Ad esempio, durante la creazione di un nuovo VPC, puoi elencare i VPC esistenti eseguendo `ibmcloud is vpcs`.  Scegli un nome che non sia in conflitto.

## floating_ip_in_use
**Messaggio**: l'IP mobile è in uso.

Questo IP mobile è già associato a un'istanza del server virtuale o a un gateway pubblico.

Per vedere dove viene utilizzato l'IP mobile, utilizza l'API `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` e osserva i valori "target". Comando della CLI equivalente: `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_many
**Messaggio**: è possibile avere solo un gateway pubblico per zona in un VPC.

È consentito un solo gateway pubblico per zona in un VPC, ma l'unico gateway pubblico può essere collegato a più sottoreti nella zona. Per trovare il gateway pubblico per una zona, esegui l'API GET public_gateways e osserva i valori "vpc" e "zone". Se utilizzi la CLI, puoi eseguire il comando `ibmcloud is public-gateways` e osservare i valori "VPC" e "Zone".

Utilizza l'ID del gateway pubblico per collegarlo a una sottorete; vedi un esempio nei nostri [esempi API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-). Se utilizzi la CLI, puoi utilizzare il comando di aggiornamento della rete per collegarlo a un gateway pubblico, ad esempio `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
**Messaggio**: il ritardo del monitoraggio integrità specificato <health_monitor_delay> non è valido

Fornisci un valore compreso tra 2 e 60 per il parametro `health monitor delay`.

## health_monitor_invalid_max_retries
**Messaggio**: il numero massimo di tentativi del monitoraggio integrità specificato <health_monitor_max_retries> non è valido

Fornisci un valore compreso tra 1 e 10 per `health max retries`.

## health_monitor_invalid_timeout
**Messaggio**: il timeout del monitoraggio integrità specificato <health_monitor_timeout> non è valido.

Fornisci un valore compreso tra 1 e 59 per `health timeout`. Il valore selezionato deve essere inferiore al valore di `health monitor delay`.

## health_monitor_invalid_url_path
**Messaggio**: il percorso URL integrità specificato <health_monitor_url> non è valido.

Fornisci un URL valido per il monitoraggio integrità. L'URL può contenere caratteri alfanumerici, trattini e punti. Non sono consentiti spazi o barre rovesciate. La lunghezza massima del percorso URL è di 255 caratteri.

## health_monitor_invalid_protocol
**Messaggio**: il protocollo del monitoraggio integrità specificato <health_monitor_protocol> non è valido.

Fornisci un valore valido per il protocollo del monitoraggio integrità. I valori accettabili sono `http` e `tcp`.

## health_monitor_missing_protocol
**Messaggio**: manca il protocollo del monitoraggio integrità

Il protocollo del monitoraggio integrità è un campo obbligatorio. Nella tua richiesta, specifica il protocollo del monitoraggio integrità. I valori accettabili sono `http` e `tcp`.

## health_monitor_missing_url_path
**Messaggio**: manca il percorso URL del monitoraggio integrità. È richiesto per un monitoraggio integrità di tipo HTTP.

Per un monitoraggio integrità che utilizza il protocollo HTTP, il percorso URL del monitoraggio integrità è un campo obbligatorio. Fornisci un percorso URL valido.

## health_monitor_timeout_delay_conflict
**Messaggio**: il timeout del monitoraggio integrità  <health_monitor_timeout> non deve essere maggiore o uguale al ritardo <health_monitor_delay>.

Il valore del parametro `health monitor delay` deve essere maggiore al valore di `health monitor timeout`.

## http_request_size_exceeded
**Messaggio**: la richiesta HTTP è troppo grande.

Questo problema si verifica quando il payload che hai inviato nella tua richiesta ha troppi caratteri. Prova di nuovo con un payload più piccolo. Ad esempio, anziché cercare di fare tutto in una singola richiesta, prova a creare una risorsa minima in una richiesta e quindi ad aggiungerne lo stato in modo incrementale in diverse richieste successive.

Fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per ulteriore assistenza. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
**Messaggio**: nessuno

Si è verificato un errore nel servizio IAM, verificando l'autenticazione o l'autorizzazione. L'interruzione del servizio IAM potrebbe essere temporanea. Riprova la richiesta dopo qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
**Messaggio**: non è possibile creare la politica IKE perché il tuo account ha raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le politiche IKE correnti, utilizza l'API `GET /ike_policies`.
Comando della CLI equivalente: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**Messaggio**: il nome `<ike_policy_name>` è già utilizzato dalla politica IKE `<ike_policy_id>`.

Fornisci un nome di politica IKE differente. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
**Messaggio**: la politica IKE `<ike_policy_id>` è utilizzata da una o più connessioni.

Una politica IKE non può essere eliminata se è utilizzata da una o più connessioni.

Per vedere quali connessioni stanno utilizzando la politica IKE, utilizza l'API `GET /ike_policies/<ike_policy_id>/connections`.
Comando della CLI equivalente: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**Messaggio**: il nome `<ike_policy_name>` non è un nome di politica IKE valido.

Un nome di politica IKE valido inizia con una lettera, seguita da lettere, cifre, caratteri di sottolineatura o trattini.

## ike_policy_not_created
**Messaggio**: non è stato possibile creare la politica IKE `<ike_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
**Messaggio**: non è stato possibile eliminare la politica IKE `<ike_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_found
**Messaggio**: non è stato possibile trovare la politica IKE `<ike_policy_id>`.

Hai fatto riferimento a una politica IKE che non esiste. Riesamina la tua richiesta per assicurarti di aver specificato l'ID della politica IKE corretto. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
**Messaggio**: non è stato possibile aggiornare la politica IKE `<ike_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
**Messaggio**: non è possibile eliminare l'istanza nello stato corrente.

Non puoi eliminare un'istanza quando il suo stato è `deleting`, `pending`, `starting`, `stopping` o `restarting`. Lo stato dell'istanza deve essere `running` o `stopped`. Se lo stato non cambia, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
**Messaggio**: non è possibile creare l'istanza perché il nome host non è valido.

Il nome host dell'istanza deve soddisfare i seguenti requisiti:
* Il nome host deve contenere solo caratteri alfanumerici e trattini
* Non sono consentiti caratteri maiuscoli
* Non sono consentiti trattini iniziali e finali
* Non sono consentiti numeri iniziali
* Non sono consentiti trattini consecutivi
* La lunghezza massima è di 15 caratteri per Windows
* La lunghezza massima è di 40 caratteri per altri sistemi operativi

## instance_invalid_port_speed
**Messaggio**: non è possibile creare l'istanza perché la velocità della porta dell'interfaccia di rete specificata non è valida.

La velocità della porta dell'interfaccia di rete deve essere `100` o `1000` Mbps.

## instance_name_exists
**Messaggio**: esiste già lo stesso nome di istanza.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
**Messaggio**: non è possibile creare l'istanza perché il numero di collegamenti del volume potrebbe superare la quota.

Le quote VPC per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}.

## instance_too_many_keys
**Messaggio**: non è possibile creare l'istanza di Windows perché la richiesta contiene più chiavi.

Le istanze di Windows supportano solo una chiave.

## insufficient_space_for_subnet
**Messaggio**: spazio insufficiente per la sottorete nel prefisso di indirizzo.

La sottorete non può essere creata perché non è possibile assegnare il numero di indirizzi richiesti. Riprova utilizzando un numero indirizzi più piccolo o un CIDR più grande.

## internal_error
**Messaggio**: si è verificato un errore interno.

Si è verificato un errore imprevisto. Questo problema può essere temporaneo. Prova di nuovo la richiesta tra qualche minuto. Se questo errore persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
**Messaggio**: nessuno

Si è verificato un errore imprevisto. Questo problema può essere temporaneo. Prova di nuovo la richiesta tra qualche minuto. Se questo errore persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
**Messaggio**: contatta il tuo amministratore.

Si è verificato un errore imprevisto. Questo problema può essere temporaneo. Prova di nuovo la richiesta tra qualche minuto. Se questo errore persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_generation_parameter
**Messaggio**: il parametro della query di generazione deve essere impostato su 1.

Per le versioni a partire dal 31/5/2019, il parametro della query `generation` deve essere impostato su 1 per consentire le richieste all'API VPC on Classic.

## invalid_id_format
**Messaggio**: formato ID errato. Assicurati che il formato sia corretto.

Assicurati che l'ID che hai fornito non contenga dati non corretti.

Potresti ottenere questo messaggio di errore se fornisci una query di avvio con formato errato quando effettui una richiesta di impaginazione. Ad esempio,
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` contiene due `?`. Correggi la query e riprova.

## invalid_route
**Messaggio**: l'instradamento richiesto non esiste.

L'instradamento richiesto sull'URL API fornito non esiste. Verifica che l'URL che hai specificato per chiamare l'endpoint API sia corretto.

## invalid_request_field
**Messaggio**: un campo fornito nella richiesta non è valido.

Ad esempio, per aggiornare l'ACL di rete utilizzato da una sottorete, utilizza l'API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’`.
La seguente richiesta non è valida perché “networkacl” non è un campo valido, `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

Per i valori accettabili, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## invalid_state
**Messaggio**: è stata richiesta un'azione su una risorsa che non è supportata nello stato corrente della risorsa.

Se la risorsa è un'istanza:

1. Un'operazione di riavvio potrebbe essere già in corso per l'istanza. Fai riferimento alle [azioni consentite](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance), a seconda dello stato dell'istanza.

2. Mentre lo stato dell'istanza è `pending` non puoi effettuare le seguenti azioni:
  * eliminare l'istanza
  * collegare un volume all'istanza
  * scollegare un volume dall'istanza
  * aggiornare l'istanza

Prova a eseguire l'azione in un secondo momento. Se lo stato della risorsa non cambia, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

Se la risorsa è un'immagine:

Mentre lo stato dell'istanza è `deleting` non puoi effettuare le seguenti azioni:
  * applicare patch all'immagine
  * eliminare l'immagine

Non effettuare di nuovo la richiesta. L'eliminazione dell'immagine verrà completata con i suoi tempi. Una volta che l'eliminazione è stata completata, tutte le richieste su quell'immagine avranno esito negativo con l'errore `not_found`.

## invalid_version
**Messaggio**: il parametro `version` non è valido, deve essere nel formato `YYYY-MM-DD`.

La versione deve essere conforme al formato _YYYY-MM-DD_. Per i mesi o le date a una sola cifra, ad esempio il 1° gennaio, la versione deve essere simile a `2019-01-01`. Il parametro della versione deve essere presente nell'URL. La data indicata nel parametro della versione deve essere successiva al 01-01-2019 ma anteriore alla data corrente.

## invalid_version_range
**Messaggio**: il valore `version` non può essere impostato in una data futura né prima di `2019-01-01`.

La data indicata nel parametro della versione deve essere successiva al 01-01-2019 ma anteriore alla data corrente.

## invalid_zone
**Messaggio**: controlla se le risorse che stai richiedendo si trovano nella stessa zona.

Potresti visualizzare questo messaggio quando tenti di collegare un gateway pubblico nella zona-1 a una sottorete nella zona-2.

## ipsec_policies_quota_exceeded
**Messaggio**: non è possibile creare la politica IPsec perché il tuo account ha raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le politiche IPsec correnti, utilizza l'API `GET /ipsec_policies`.
Comando della CLI equivalente: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**Messaggio**: il nome `<ipsec_policy_name>` è già utilizzato dalla politica IPsec `<ipsec_policy_id>`.

Fornisci un nome di politica IPsec differente. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
**Messaggio**: la politica IPsec `<ipsec_policy_id>` è utilizzata da una o più connessioni.

Una politica IPsec non può essere eliminata se è utilizzata da una o più connessioni.

Per vedere quali connessioni stanno utilizzando la politica IPsec, utilizza l'API `GET /ipsec_policies/<ipsec_policy_id>/connections`.
Comando della CLI equivalente: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**Messaggio**: il nome `<ipsec_policy_name>` non è un nome di politica IPsec valido.

Un nome di politica IPsec valido inizia con una lettera, seguita da lettere, cifre, caratteri di sottolineatura o trattini.

## ipsec_policy_not_created
**Messaggio**: non è stato possibile creare la politica IPsec `<ipsec_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
**Messaggio**: non è stato possibile eliminare la politica IPsec `<ipsec_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_found
**Messaggio**: non è stato possibile trovare la politica IPsec `<ipsec_policy_id>`.

Hai fatto riferimento a una politica IPsec che non esiste. Riesamina la tua richiesta e specifica un IP politica IPsec valido. Se sei sicuro che la politica esiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
**Messaggio**: non è stato possibile aggiornare la politica IPsec `<ipsec_policy_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
**Messaggio**: esiste già lo stesso contenuto chiave.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
**Messaggio**: esiste già lo stesso nome di chiave.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
**Messaggio**: non è possibile creare la chiave perché il nome non è valido.

Il nome della chiave deve soddisfare i seguenti requisiti:
* deve iniziare con un carattere alfabetico [A-Za-z]
* deve contenere solo caratteri alfanumerici, trattini o caratteri di sottolineatura [-A-Za-z0-9_]
* la lunghezza non deve superare i 65 caratteri

## key_invalid_type
**Messaggio**: non è possibile creare la chiave perché il tipo non è valido.

L'unico tipo di chiave supportato è `rsa`.

## key_parse_failure
**Messaggio**: non è possibile creare la chiave perché il valore della chiave pubblica non è valido.

La chiave pubblica fornita nella richiesta non è valida. Controlla il valore o rigenera la chiave pubblica e riprova.

Nei sistemi operativi Linux, puoi immettere il seguente comando per validare la chiave pubblica `ssh-keygen -l -v -f <key_file>`.

## listener_certificate_not_found
**Messaggio**: impossibile trovare l'istanza del certificato con CRN `<listener_certificate_crn>` o nessuna autorizzazione per accedere all'istanza del certificato.

Fornisci un CRN di istanza del certificato esistente o chiedi al tuo amministratore dell'account di concederti le autorizzazioni di accesso all'istanza del certificato fornita.

## listener_duplicate_port
**Messaggio**: la porta `<listener_port>` è utilizzata da un'altra istanza del listener. Scegli un'altra porta.

Scegli una porta diversa.

## listener_invalid_certificate
**Messaggio**: il CRN dell'istanza del certificato per il listener HTTPS non è valido.

Fornisci un CRN valido per l'istanza del certificato.

## listener_invalid_connection_limit
**Messaggio**: il limite di connessione con il listener `<listener_connection_limit>` non è valido.

Fornisci un limite di connessione valido. Il limite di connessione deve essere un numero intero compreso tra 1 e 15000.

## listener_invalid_port
**Messaggio**: la porta del listener `<listener_port>` non è valida.

Scegli una porta diversa.

## listener_invalid_protocol
**Messaggio**: il protocollo del listener `<listener_protocol>` non è valido.

Fornisci un protocollo del listener valido. I valori accettabili per il protocollo del listener sono `http`, `https` e `tcp`.

## listener_missing_certificate_crn
**Messaggio**: manca il CRN dell'istanza del certificato per il listener `https`.

Il CRN dell'istanza del certificato è un campo obbligatorio.

## listener_missing_port
**Messaggio**: manca la porta del listener.

La porta del listener è un campo obbligatorio.

## listener_missing_protocol
**Messaggio**: manca il protocollo del listener.

Il protocollo del listener è un campo obbligatorio. Fornisci un protocollo del listener nella tua richiesta. I valori accettabili sono `http`, `https` e `tcp`.

## listener_not_found
**Messaggio**: impossibile trovare il listener con ID `<listener_id>`.

Fornisci un ID listener esistente.

## listener_over_quota
**Messaggio**: impossibile creare il listener. La quota di listener per la risorsa del programma di bilanciamento del carico ha raggiunto il limite massimo.

Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## listener_pool_protocols_conflict
**Messaggio**: il protocollo del listener (`<listener_protocol>`) e il protocollo del pool (`<pool_protocol>`) sono in conflitto.

Un listener con il protocollo `https` o `http` può essere associato solo a un pool con il protocollo `http`. Allo stesso modo, un listener `tcp` può essere associato solo a un pool `tcp`.

## listener_reserved_port_conflict
**Messaggio**: la porta del listener `<listener_port>` è una delle porte riservate. L'intervallo di porte compreso tra 56500 e 56520 è riservato per scopi di gestione. Scegli una porta diversa.

Scegli un'altra porta.

## load_balancer_delete_conflict
**Messaggio**: non è possibile eliminare il programma di bilanciamento del carico con ID <load_balancer_id> perché il suo stato è uno dei seguenti:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Elimina il programma di bilanciamento del carico quando il suo stato è ACTIVE.

## load_balancer_duplicate_name
**Messaggio**: il nome `<load_balancer_name>` è utilizzato da un'altra istanza del programma di bilanciamento del carico. Scegli un altro nome.

Fornisci un nome diverso per l'istanza del programma di bilanciamento del carico. Il nome del programma di bilanciamento del carico deve essere univoco all'interno del VPC e dell'account.

I tuoi servizi Load Balancer for VPC e Cloud Load Balancer non saranno in conflitto, perché i nomi di dominio DNS sono diversi per questi servizi e il nome di Load Balancer for VPC non è incluso nel nome DNS.
{: note}

## load_balancer_insufficient_ips
**Messaggio**: la sottorete con ID <subnet_id> non ha sufficienti indirizzi IPv4 disponibili.

Nella tua richiesta, fornisci una sottorete valida con gli indirizzi IPv4 disponibili in cui vuoi creare il programma di bilanciamento del carico.

## load_balancer_invalid_description
**Messaggio**: la descrizione del programma di bilanciamento del carico `<load_balancer_description>` non è valida.

La descrizione di un programma di bilanciamento del carico non può superare i 255 caratteri.

## load_balancer_invalid_is_public
**Messaggio**: il valore di is_public `<load_balancer_is_public>` non è valido.

Il valore del parametro del programma di bilanciamento del carico `is_public` deve essere _true_ o _false_.

## load_balancer_invalid_name
**Messaggio**: il nome `<load_balancer_name>` non è valido.

I nomi non possono essere vuoti. Un nome di programma di bilanciamento del carico valido inizia con una lettera, seguita da lettere, cifre o caratteri di sottolineatura. La lunghezza del nome non può superare i 40 caratteri.

## load_balancer_invalid_subnet
**Messaggio**: la sottorete con ID <subnet_id> non è valida. Assicurati di utilizzare una sottorete esistente con lo stato 'available'.

Fornisci sottoreti valide con lo stato `available` quando immetti la tua richiesta di creare un programma di bilanciamento del carico.

## load_balancer_missing_is_public
**Messaggio**: manca il campo 'is_public'.

Il campo `is_public` è obbligatorio. Fornisci un valore per il campo `is_public`.

## load_balancer_missing_name
**Messaggio**: manca il nome del programma di bilanciamento del carico.

Il campo `name` del programma di bilanciamento del carico è obbligatorio. Fornisci un nome per il programma di bilanciamento del carico.

## load_balancer_missing_subnets
**Messaggio**: mancano le sottoreti del programma di bilanciamento del carico.

Il campo `subnets` è obbligatorio. Nella tua richiesta di creazione di un programma di bilanciamento del carico, fornisci le informazioni sulle sottoreti in cui vuoi creare il programma di bilanciamento del carico.

## load_balancer_missing_subnet_id
**Messaggio**: manca l'ID della sottorete.

L'**ID sottorete** è un campo obbligatorio. Fornisci un ID sottorete con il tuo comando.

## load_balancer_not_found
**Messaggio**: impossibile trovare il programma di bilanciamento del carico con ID `<load_balancer_id>`.

Fornisci l'ID di un programma di bilanciamento del carico esistente.

## load_balancer_over_quota
**Messaggio**: impossibile creare il programma di bilanciamento del carico. La quota di istanze del programma di bilanciamento del carico nel tuo account ha raggiunto il limite massimo.

Elimina un programma di bilanciamento del carico esistente o contatta il supporto per aumentare la quota del programma di bilanciamento del carico sul tuo account.

Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## load_balancer_subnet_not_found
**Messaggio**: impossibile trovare la sottorete con ID <subnet_id>.

Nella tua richiesta, devi fornire gli ID delle sottoreti esistenti in cui vuoi creare il programma di bilanciamento del carico.

## load_balancer_unchanged_update
**Messaggio**: non c'è nulla per aggiornare il programma di bilanciamento del carico con ID `<load_balancer_id>`.

Non è stata fornita alcuna informazione per l'aggiornamento di un'istanza del programma di bilanciamento del carico con l'ID specificato.

## load_balancer_update_conflict
**Messaggio**: non è possibile aggiornare il programma di bilanciamento del carico con ID <load_balancer_id> perché il suo stato è uno dei seguenti:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Aggiorna il programma di bilanciamento del carico quando il suo stato è ACTIVE.

## member_duplicate_address_and_port
**Messaggio**: il membro con indirizzo <member_address> e porta <member_port> esiste già nel pool.

Fornisci una combinazione di indirizzo e porta del membro univoca nella tua richiesta.

## member_invalid_address
**Messaggio**: l'indirizzo IP del membro specificato <member_ip> non è valido.

Nella tua richiesta, fornisci un indirizzo CIDR IPv4 valido per il membro.

## member_invalid_port
**Messaggio**: la porta del membro specificata `<member_port>` non è valida.

Fornisci un numero di porta valido. Un numero di porta valido è compreso tra 1 e 65535.

## member_invalid_weight
**Messaggio**: il peso del membro specificato `<member_weight>` non è valido.

Fornisci un peso del membro valido. Il valore deve essere un numero intero compreso tra 0 e 100.

## member_ip_not_found
**Messaggio**: il membro con indirizzo IP <member_IP> non appartiene ad alcuna sottorete nella regione e nel VPC del programma di bilanciamento del carico.

Nella tua richiesta, fornisci l'indirizzo di un membro che appartiene a una sottorete nella stessa regione e nello stesso VPC del programma di bilanciamento del carico.

## member_missing_address
**Messaggio**: manca l'indirizzo del membro.

L'indirizzo del membro è un campo obbligatorio. Fornisci un valore per l'indirizzo del membro.

## member_missing_protocol_port
**Messaggio**: manca la porta del membro.

La porta del membro è un campo obbligatorio. Fornisci un valore per la porta del membro.

## member_not_found
**Messaggio**: impossibile trovare il membro con ID <member_id>.

Fornisci l'ID di un membro esistente.

## member_over_quota
**Messaggio**: impossibile creare il membro. La quota di istanze del membro nel pool ha raggiunto il limite massimo.

Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## missing_generation_parameter
**Messaggio**: il parametro della query di generazione è obbligatorio.

Per le versioni a partire dal 31/5/2019, il parametro della query `generation` è obbligatorio per le richieste all'API VPC on Classic.

## missing_ims_account_id
**Messaggio**: nessuno

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## missing_version
**Messaggio**: il parametro `version` è obbligatorio e deve essere nel formato `YYYY-MM-DD`.

Un parametro di versione è obbligatorio per tutte le richieste API. La versione deve essere conforme al formato _YYYY-MM-DD_. Per i mesi o le date a una sola cifra, ad esempio il 1° gennaio, la versione deve essere simile a `2019-01-01`. La data indicata nel parametro della versione deve essere successiva al 01-01-2019 ma anteriore alla data corrente. Controlla questi [esempi API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) su come fornire il parametro di versione.

## network_conflict
**Messaggio**: il CIDR è in conflitto con il prefisso di indirizzo esistente in VPC.

Potresti visualizzare questo messaggio se hai fornito un CIDR di rete che è in conflitto con un CIDR di rete esistente nello stesso spazio IP.

Per trovare i CIDR esistenti nei prefissi di indirizzo per il VPC, esegui il comando API `GET /v1/vpcs/<vpc-id>/address_prefixes` e osserva i valori `cidr` della risposta. Controlla il valore di `cidr` per assicurarti che il CIDR che hai fornito non sia utilizzato da altri prefissi di indirizzo.

Se utilizzi la CLI, esegui il comando `ibmcloud is vpc-addrs <vpc-id>` e controlla se ci sono dei conflitti con il valore di `CIDR Block`.

Per trovare i CIDR esistenti nelle sottoreti, esegui il comando API `GET /v1/subnets` per elencare tutte le sottoreti per il VPC. Quindi, esegui il comando API `GET /v1/subnets/<subnet-id>` per ogni sottorete nel VPC e controlla il valore di `ipv4_cidr_block` nella risposta. Controlla il valore di `ipv4_cidr_block` per assicurarti che il CIDR che hai fornito non sia utilizzato da altri prefissi di indirizzo.

Se utilizzi la CLI, esegui il comando `ibmcloud is subnets` per elencare tutte le sottoreti per il VPC. Quindi, esegui il comando `ibmcloud is subnet <subnet-id>` per ogni sottorete nel VPC e controlla se ci sono dei conflitti con il valore di `IPv4 CIDR` nell'output della CLI.

## not_authorized
**Messaggio**: la richiesta non è autorizzata.

Potresti visualizzare questo errore se il tuo token IAM manca o è scaduto. Per istruzioni su come generare un token, fai riferimento a [Creazione di un VPC utilizzando le API REST](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis). Se il token non è scaduto, assicurati che l'account che stai utilizzando sia autorizzato a utilizzare questa funzione e di disporre delle [autorizzazioni corrette](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Se disponi dell'autorizzazione corretta ma ricevi ancora questo errore, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_found
**Messaggio**: controlla se la risorsa che stai richiedendo esiste.

Hai fatto riferimento a una risorsa che non esiste o a cui non hai accesso. Riesamina la tua richiesta per assicurarti di aver specificato gli ID e i riferimenti corretti.

Fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per ulteriore assistenza. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implemented
**Messaggio**: nessuno

Il metodo non è implementato. Controlla la tua richiesta per assicurarti di chiamare un'[API valida](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. [Contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se prevedi che questa API venga implementata.

## not_in_address_prefix
**Messaggio**: il CIDR fornito non è adatto ai prefissi dell'indirizzo nella zona fornita.

Questo errore si verifica se un utente tenta di creare una sottorete con un CIDR che non rientra in alcun prefisso dell'indirizzo per la zona specificata.

Esegui `GET /vpcs/{vpc_id}/address_prefixes` per ottenere l'elenco di prefissi di indirizzo per il VPC. Se utilizzi la CLI, puoi eseguire `ibmcloud is vpc-address-prefixes` per elencare tutti i prefissi di indirizzo per il tuo VPC. Osserva i valori `cidr` e `zone` della risposta e assicurati che il `cidr` della sottorete sia un sottoinsieme del `cidr` del prefisso di indirizzo per la zona che stai tentando di creare.

## over_quota
**Messaggio**: la richiesta potrebbe superare la quota per un tipo di risorsa.

Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## password_not_ready
**Messaggio**: nessuno

È necessario che la tua istanza sia in esecuzione prima di poter recuperare la password. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
**Messaggio**: non è possibile eliminare il pool perché è ancora associato a uno o più listener.

Assicurati che il pool sia dissociato da qualsiasi listener e riprova.

## pool_duplicate_name
**Messaggio**: il nome pool `<pool_name>` è già utilizzato da un altro pool. Scegli un altro nome.

Scegli un nome pool differente.

## pool_invalid_name
**Messaggio**: il nome <pool_name> non è valido.

Fornisci un nome pool valido. Un nome di programma di bilanciamento del carico valido inizia con una lettera, seguita da lettere, cifre o trattini. La lunghezza del nome non può superare i 40 caratteri.

## pool_invalid_session_persistence
**Messaggio**: il valore di persistenza della sessione del pool non è valido.

Fornisci un valore di persistenza della sessione valido. Il valore accettabile è `SOURCE_IP`.

## pool_missing_algorithm
**Messaggio**: manca l'algoritmo di bilanciamento del carico.

L'algoritmo di bilanciamento del carico è un campo obbligatorio. Fornisci l'algoritmo di bilanciamento del carico nella tua richiesta. I valori accettabili sono `round_robin`, `weighted_round_robin` e `least_connections`.

## pool_missing_health_monitor
**Messaggio**: manca il monitoraggio integrità del pool.

Il monitoraggio integrità del pool è un campo obbligatorio.

## pool_missing_members
**Messaggio**: mancano i membri del pool.

Fornisci i membri del pool.

## pool_missing_name
**Messaggio**: manca il nome pool.

Il nome pool è un campo obbligatorio.

## pool_missing_protocol
**Messaggio**: manca il protocollo del pool.

Il protocollo del pool è un campo obbligatorio. Fornisci il protocollo del pool nella tua richiesta. I valori accettabili sono `http` e `tcp`.

## pool_not_found
**Messaggio**: impossibile trovare il pool con ID `<pool_id>`.

Fornisci l'ID di un pool esistente.

## pool_over_quota
**Messaggio**: impossibile creare il pool. La quota di pool per la risorsa del programma di bilanciamento del carico ha raggiunto il limite massimo.

Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## public_gateway_in_use
**Messaggio**: impossibile eliminare un gateway pubblico quando è in uso.

Il gateway pubblico è attualmente collegato a una o più sottoreti. Devi scollegare il gateway pubblico da tutte le sottoreti prima di poterlo eliminare.

Per vedere quale sottorete sta usando il gateway pubblico, utilizza l'API `GET /v1/subnets?version=2019-05-31&generation=1`.  Comando della CLI equivalente: `ibmcloud is subnets`.  Per scollegare il gateway pubblico dalla sottorete, utilizza il comando API `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` o il comando della CLI `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
**Messaggio**: troppe richieste in breve tempo.

Questo messaggio di errore viene restituito se vengono ricevute troppe richieste entro un intervallo di tempo specificato. Attendi qualche istante e riprova.

## security_group_active_transactions
**Messaggio**: non è possibile collegare o scollegare l'interfaccia finché l'istanza non sarà in stato Attivo.

L'istanza deve essere attiva prima che le sue interfacce di rete possano essere collegate a un gruppo di sicurezza. Utilizza `GET /v1/instances/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance` per controllare lo stato dell'istanza. Una volta che lo stato è `running`, prova di nuovo. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
**Messaggio**: l'interfaccia è già collegata al gruppo di sicurezza.

L'interfaccia è già collegata al gruppo di sicurezza. Utilizza `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` per visualizzare le interfacce collegate.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_exists
**Messaggio**: il gruppo di sicurezza esiste già.

I nomi dei gruppi di sicurezza devono essere univoci all'interno di un VPC. Un gruppo di sicurezza con tale nome esiste già nel VPC di destinazione. Utilizza `GET /v1/security_groups?version=2019-05-31&generation=1` o `ibmcloud is security-groups` per visualizzare i gruppi di sicurezza correnti.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic) o al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_attached
**Messaggio**: impossibile eliminare il gruppo di sicurezza mentre le interfacce sono collegate.


Scollega tutte le interfacce prima di eliminare il gruppo di sicurezza. Utilizza `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interface-remove` per scollegare un'interfaccia.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic) o al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Messaggio**: è stato superato il limite di interfacce per gruppo di sicurezza.

Collegando un'altra interfaccia al gruppo di sicurezza si supererebbe il limite di interfacce per gruppo di sicurezza. Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.Valuta la tua strategia e considera la possibilità di creare un altro gruppo di sicurezza con regole simili.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic) o al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_last_security_group_is_default
**Messaggio**: il gruppo di sicurezza predefinito non può essere rimosso se è l'unico gruppo di sicurezza collegato.

Un'interfaccia di rete deve essere collegata ad almeno un gruppo di sicurezza.
Se non ne viene specificato uno, verrà collegata al gruppo di sicurezza predefinito del VPC.
Collega l'interfaccia a un gruppo di sicurezza diverso, quindi riprova a scollegarla dal gruppo di sicurezza predefinito.
Per informazioni sul gruppo di sicurezza predefinito, fai riferimento al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
**Messaggio**: limite del gruppo di sicurezza superato.

Hai tentato di creare un nuovo gruppo di sicurezza, ma al momento sei al limite della tua quota di account. Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.Valuta la tua strategia per l'assegnazione di istanze ai gruppi di sicurezza. Spesso è possibile ridurre il numero complessivo dei gruppi di sicurezza assegnando più istanze allo stesso gruppo di sicurezza. Questa strategia ridurrà il numero di gruppi di sicurezza e ti lascerà sotto la tua quota di account. In rari casi, generalmente per le grandi organizzazioni, è necessario espandere la quota. In questo caso, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) per chiedere se è possibile.

## security_group_network_interface_not_active
**Messaggio**: non è possibile collegare l'interfaccia perché non è attiva.

Le interfacce di rete devono essere attive prima di collegarle ai gruppi di sicurezza. Utilizza `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance-network-interface` per visualizzare lo stato di un'interfaccia. Una volta che lo stato è 'available', prova di nuovo. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
**Messaggio**: l'interfaccia non è collegata.

L'interfaccia non è collegata al gruppo di sicurezza. Utilizza `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` per visualizzare le interfacce collegate.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic) o al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_not_in_vpc
**Messaggio**: l'interfaccia e il gruppo di sicurezza sono in diversi VPC.

Per collegare un'interfaccia di rete a un gruppo di sicurezza, l'istanza deve trovarsi nello stesso VPC del gruppo di sicurezza. Utilizza `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` o `ibmcloud is security-group` per visualizzare i dettagli dei gruppi di sicurezza e il VPC. Utilizza `GET /v1/instances/{id}?version=2019-05-31&generation=1` o `ibmcloud is instance` per visualizzare i dettagli dell'istanza e il VPC.

## security_group_order_bindings
**Messaggio**: impossibile eliminare il gruppo di sicurezza mentre ha ordini di istanza in sospeso.

Se è stata creata un'istanza con i gruppi di sicurezza specificati sulle interfacce di rete, l'istanza e le interfacce di rete devono essere attive prima che il gruppo di sicurezza possa essere eliminato. Utilizza `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` o `ibmcloud is security-group-network-interfaces` per visualizzare le interfacce di rete collegate e il loro stato. Una volta che lo stato delle interfacce è 'available', prova di nuovo. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Messaggio**: il numero di porta massimo TCP/UDP non può essere inferiore al numero di porta minimo.

Il valore di porta massimo non può essere inferiore al valore di porta minimo. Specifica un valore di porta massimo maggiore del valore di porta minimo.

## security_group_port_range_both_or_neither
**Messaggio**: l'impostazione dell'intervallo di porte deve essere annullata oppure è necessario impostare un numero di porta minimo e massimo per 'tcp' e 'udp'.

Le regole con un protocollo 'tcp' o 'udp' possono avere un intervallo di porte non impostato (per applicare la regola a tutto il traffico) oppure possono specificare un intervallo di porte. Per limitare il traffico a una singola porta, imposta sia il numero di porta minimo che massimo sul valore della porta. Ad esempio, il seguente comando crea una regola per consentire SSH sulla porta 22: `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

Per ulteriori informazioni sulle configurazioni delle regole del gruppo di sicurezza, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic).


## security_group_port_range_invalid_protocol
**Messaggio**: è stato specificato un intervallo di porte con il protocollo 'icmp'. Un intervallo di porte è valido solo per un protocollo di tipo 'tcp' o 'udp'.

Le regole con un protocollo 'icmp' hanno un tipo e un codice. Le regole con i protocolli 'tcp' o 'udp' hanno un intervallo di porte. Per ulteriori informazioni sulle configurazioni delle regole del gruppo di sicurezza, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_remote_group_not_in_vpc
**Messaggio**: il gruppo remoto non si trova nello stesso VPC di questo gruppo di sicurezza.

Le regole del gruppo di sicurezza possono fare riferimento a un gruppo di sicurezza remoto, consentendo il traffico tra le interfacce collegate dei due gruppi. I gruppi di sicurezza remoti devono trovarsi nello stesso VPC. Utilizza `GET /v1/security_groups?2019-01-01` o `ibmcloud is security-groups` per visualizzare i gruppi di sicurezza correnti e i loro VPC.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Messaggio**: impossibile eliminare il gruppo di sicurezza mentre le regole remote sono collegate.

Il gruppo di sicurezza contiene una o più regole che fanno riferimento a un gruppo di sicurezza remoto. Utilizza `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` o `ibmcloud is security-group-rules` per visualizzare le regole. Il campo 'remote' specificherà qualsiasi gruppo di sicurezza remoto. Prova di nuovo dopo aver eliminato o modificato la regola remota.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic) o al [documento sull'utilizzo dei gruppi di sicurezza](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

## security_group_remoting_rules_per_sg_exceeded
**Messaggio**: è stato superato il limite di regole remote per gruppo di sicurezza.

Aggiungendo un'altra regola che fa riferimento a un gruppo di sicurezza remoto si supererebbe il limite di regole remote per gruppo di sicurezza. Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.

## security_group_rules_per_sg_exceeded
**Messaggio**: è stato superato il limite di regole per gruppo di sicurezza.

Aggiungendo una regola si supererebbe il limite di regole per gruppo di sicurezza. Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Valuta la tua strategia e considera la possibilità di creare un altro gruppo di sicurezza.

## security_group_sgs_per_interface_exceeded
**Messaggio**: è stato superato il limite di gruppi di sicurezza per interfaccia.

Collegando questa interfaccia a un altro gruppo di sicurezza si supererebbe il limite di gruppi di sicurezza per interfaccia. Le quote per ciascuna risorsa sono specificate in [Quote e limiti per VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Valuta la tua strategia e considera la possibilità di aggiungere regole a un gruppo di sicurezza esistente.


## security_group_type_code_invalid_protocol
**Messaggio**: è stato fornito un tipo/codice 'icmp', ma il protocollo richiesto non era 'icmp'. Imposta il protocollo su 'icmp' o specifica un intervallo di porte.

Solo le regole con un protocollo 'icmp' hanno un tipo e un codice. Le regole con i protocolli 'tcp' o 'udp' hanno un intervallo di porte. Per ulteriori informazioni sulle configurazioni delle regole del gruppo di sicurezza, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_vpc_default
**Messaggio**: impossibile eliminare il gruppo di sicurezza predefinito per un VPC.

Non è possibile eliminare il gruppo di sicurezza predefinito. Per informazioni sul gruppo di sicurezza predefinito, fai riferimento alla guida sull'[aggiornamento del gruppo di sicurezza predefinito](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
**Messaggio**: nessuno

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
**Messaggio**: CIDR in conflitto con la sottorete esistente in VPC.

Esegui l'API `GET /subnets` per richiamare tutte le sottoreti in VPC. Controlla il valore di `ipv4_cidr_block` per assicurarti che il CIDR che hai fornito non sia utilizzato da altre sottoreti.

Se utilizzi la CLI, puoi eseguire `ibmcloud is subnets` e osservare il valore "Subnet CIDR" per eventuali conflitti.

Fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per ulteriore assistenza. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
**Messaggio**: non è possibile eliminare la sottorete finché non sarà vuota. Elimina tutte le risorse nella sottorete e riprova.

È stata effettuata una richiesta per eliminare una sottorete, ma la sottorete contiene ancora delle risorse. Le sottoreti possono contenere istanze, VPN, programmi di bilanciamento del carico o gateway pubblici. Devi eliminare le tue risorse presenti nella sottorete prima di poter eliminare la sottorete.

In alcune situazioni, questo errore può verificarsi anche quando la console mostra 0 VSI e 0 programmi di bilanciamento del carico. L'eliminazione è asincrona e la modifica dello stato interno potrebbe richiedere alcuni minuti. Prova di nuovo l'eliminazione della tua sottorete tra qualche minuto.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Messaggio**: impossibile eliminare la sottorete mentre è collegata a un gateway pubblico. Scollega il gateway pubblico e riprova.

È stata effettuata una richiesta per eliminare una sottorete, ma alla sottorete è ancora collegato un gateway pubblico. Devi eliminare o scollegare il gateway pubblico prima di poter eliminare la sottorete.

Se utilizzi la CLI, puoi eseguire `ibmcloud is public-gateways` per elencare i gateway pubblici e `ibmcloud is subnet-public-gateway-detach` per scollegare un gateway pubblico da una sottorete.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Messaggio**: impossibile eliminare la sottorete mentre contiene indirizzi IP. Elimina qualsiasi istanza del server associata all'indirizzo IP e riprova.

È stata effettuata una richiesta per eliminare una sottorete, ma la sottorete contiene ancora indirizzi IP. Devi eliminare l'istanza del server associata all'indirizzo IP prima di poter eliminare la sottorete.

Se utilizzi la CLI, puoi eseguire `ibmcloud is instances` per elencare le istanze del server. Osserva il valore "Address" per determinare il suo indirizzo IP e il valore "Status" per determinare il suo stato. Esegui `ibmcloud is instance-delete` per eliminare un'istanza del server che non abbia già uno stato "deleting".

In alcune situazioni, questo errore può verificarsi anche quando la console mostra 0 VSI. L'eliminazione è asincrona e la modifica dello stato interno potrebbe richiedere alcuni minuti. Prova di nuovo l'eliminazione della tua sottorete tra qualche minuto.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Messaggio**: impossibile eliminare la sottorete mentre contiene un programma di bilanciamento del carico. Elimina il programma di bilanciamento del carico e riprova.

È stata effettuata una richiesta per eliminare una sottorete, ma la sottorete contiene ancora un programma di bilanciamento del carico. Devi eliminare il programma di bilanciamento del carico prima di poter eliminare la sottorete.

Se utilizzi la CLI, puoi eseguire `ibmcloud is load-balancers` per elencare i programmi di bilanciamento del carico. Osserva il valore "Subnets" per determinare la sua sottorete e il valore "Status" per determinare il suo stato. Esegui `ibmcloud is load-balancer-delete` per eliminare un programma di bilanciamento del carico che non abbia già uno stato "deleting".

In alcune situazioni, questo errore può verificarsi anche quando la console mostra 0 programmi di bilanciamento del carico. L'eliminazione è asincrona e la modifica dello stato interno potrebbe richiedere alcuni minuti. Prova di nuovo l'eliminazione della tua sottorete tra qualche minuto.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Messaggio**: impossibile eliminare la sottorete mentre contiene un gateway VPN. Elimina il gateway VPN e riprova.

È stata effettuata una richiesta per eliminare una sottorete, ma alla sottorete è ancora collegato un gateway VPN. Devi eliminare il gateway VPN prima di poter eliminare la sottorete.

Se utilizzi la CLI, puoi eseguire `ibmcloud is vpn-gateways` per elencare i gateway VPN. Osserva il valore "Subnets" per determinare la sua sottorete e il valore "Status" per determinare il suo stato. Esegui `ibmcloud is vpn-gateway-delete` per eliminare un gateway VPN che non abbia già uno stato "deleting".

In alcune situazioni, questo errore può verificarsi anche quando la console mostra 0 gateway VPN. L'eliminazione è asincrona e la modifica dello stato interno potrebbe richiedere alcuni minuti. Prova di nuovo l'eliminazione della tua sottorete tra qualche minuto.

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_unknown_state
**Messaggio**: la sottorete `<subnet_id>` si trova in uno stato non valido per l'operazione richiesta.

La sottorete deve trovarsi nello stato `available` prima di poterla utilizzare. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
**Messaggio**: la destinazione ha già un IP mobile collegato.

È stata effettuata una richiesta per collegare un indirizzo IP mobile all'interfaccia di rete di un server, ma l'interfaccia di rete ha già un IP mobile collegato.

Per trovare l'indirizzo IP mobile attualmente collegato a un'interfaccia di rete, esegui il comando API `GET /v1/floating_ips?version=2019-05-31&generation=1` e cerca l'ID dell'interfaccia di rete nel campo `target.id`.  

Se utilizzi la CLI, esegui il comando `ibmcloud is floating-ips` e cerca il valore `Target`. Tieni presente che la CLI tronca l'ID dell'interfaccia di rete sul primo trattino (“-“). Ad esempio, l'interfaccia di rete primaria di un server con ID `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` viene visualizzata come `primary(abdfcb29-.)` nell'output della CLI.

## token_invalid
**Messaggio**: il token di servizio è scaduto o non è valido.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
**Messaggio**: il token di servizio era vuoto o non esisteva nella richiesta.

Ricrea un token e prova di nuovo.

## validation_enum
**Messaggio**: il valore fornito non era un'opzione valida.

Uno dei campi forniti ha un valore che deve provenire da un'enumerazione di valori possibili.

Per le regole dell'ACL di rete, puoi specificare una direzione:

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

Ad esempio, il seguente valore non è valido poiché `northbound` non è un'opzione valida nell'enumerazione `[ inbound, outbound ]`.

```json
{
    "direction":"northbound"
}
```

Per i valori accettabili, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} o al [Riferimento della CLI](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}.

## validation_failure
**Messaggio**: il JSON fornito non corrispondeva alla struttura prevista.

Per risolvere questo problema, assicurati che il contenuto della tua richiesta sia un JSON valido e che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_invalid_cidr
**Messaggio**: il valore non è un CIDR valido.

Il valore deve essere un blocco CIDR interno valido con una parte host 0.

Alcuni intervalli di indirizzi IP sono riservati. Ulteriori informazioni sugli intervalli IP riservati sono disponibili nella nostra panoramica di [Utilizzo del VPC con regioni e sottoreti](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Messaggio**: il valore non è un CIDR IPv4 valido.

Deve essere un blocco CIDR interno IPv4 con una parte host 0.

Alcuni intervalli di indirizzi IP sono riservati. Ulteriori informazioni sugli intervalli IP riservati sono disponibili nella nostra panoramica di [Utilizzo del VPC con regioni e sottoreti](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Messaggio**: il valore non è un CIDR IPv6 valido.

Attualmente, IPv6 non è supportato. Utilizza un indirizzo IPv4.

## validation_invalid_address
**Messaggio**: il valore non è un indirizzo valido.

Deve essere un indirizzo IP valido.

Un elenco di indirizzi IP riservati singolarmente è fornito nel documento [Regioni e sottoreti](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv4_address
**Messaggio**: il valore non è un indirizzo IPv4 valido.

Fornisci un indirizzo IPv4 valido.

## validation_invalid_ipv6_address
**Messaggio**: il valore non è un indirizzo IPv6 valido.

Fornisci un indirizzo IPv6 valido. Attualmente, IPv6 non è supportato; utilizza un indirizzo IPv4.

## validation_invalid_field_type
**Messaggio**: il tipo di valore non corrisponde al tipo di campo.

Per risolvere questo problema, assicurati che il contenuto della tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per l'endpoint che stai chiamando.

## validation_max_value
**Messaggio**: un valore fornito per un parametro è maggiore di quello consentito.

Fornisci un valore inferiore che rispetti il valore massimo come indicato dalla [specifica](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_min_value
**Messaggio**: un valore fornito per un parametro è minore di quello consentito.

Potresti ottenere questo errore se tenti di creare una sottorete con un valore `total_ipv4_address_count` inferiore a 8.

Fornisci un valore maggiore che rispetti il valore minimo come indicato dalla [specifica](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_not_null
**Messaggio**: il campo fornito deve essere null.

Assicurati che il valore sia null. Fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

Di seguito è riportato un esempio non valido:

```json
{
    "name": null
}
```

## validation_only_one
**Messaggio**: il valore deve corrispondere a uno dei sottoschemi.

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_discriminator_forbidden
**Messaggio**: il campo discriminator vieta questa sottostruttura.

Ad esempio, se specifichi:
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

Il protocollo è `icmp` e _port_min_ è un campo `tcp`, quindi riceverai un errore.
1. validation_discriminator_required per le regole `icmp` mancanti
2. validation_discriminator_forbidden per i campi `tcp` con `icmp` specificato

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
**Messaggio**: nessuno

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
**Messaggio**: il valore non è un nome valido.

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
**Messaggio**: il valore non è un HREF valido.

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
**Messaggio**: nessuno

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
**Messaggio**: manca un campo obbligatorio.

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
**Messaggio**: il campo fornito deve essere univoco.

Potresti aver specificato un nome che è già in uso. Prova un valore diverso.

Assicurati che la tua richiesta sia conforme alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Messaggio**: non è possibile eliminare il collegamento del volume.

Questa operazione non è consentita. Il collegamento del volume di boot non può essere eliminato perché il volume di boot è richiesto dall'istanza.

## volume_attachment_update_invalid_request
**Messaggio**: non è stato possibile aggiornare il collegamento del volume perché la richiesta non è valida.

Non è stato possibile aggiornare il collegamento del volume per uno di questi motivi:

* Il collegamento del volume di boot non può essere ridenominato
* Il campo `delete_volume_on_instance_delete` del collegamento del volume di boot non può essere impostato su `true`

## volume_capacity_max
**Messaggio**: la capacità massima consentita per il volume è di 2000 GB.

La capacità del volume che hai specificato supera la capacità massima. Specifica un valore compreso tra 10 e 2000 GB. Vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) per gli intervalli di capacità accettati per un profilo personalizzato.

## volume_capacity_missing
**Messaggio**: nella richiesta manca il valore di capacità del volume obbligatorio.

Quando crei un volume, devi specificare la sua capacità in base alla definizione di questo profilo. Per ulteriori informazioni, vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_capacity_zero_or_negative
**Messaggio**: la capacità del volume deve essere maggiore di zero.

Quando crei un volume, il valore di capacità specificato nella richiesta deve essere un numero positivo compreso tra 10 GB e 2000 GB. Vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) per i valori di capacità del volume supportati.

## volume_crn_account_id_mismatch
**Messaggio**: il CRN specificato nella richiesta non appartiene all'account dell'utente corrente.

Il CRN della chiave root di Key Protect non corrisponde al tuo ID account di autorizzazione IAM. Specifica un CRN della chiave root di Key Protect diverso per il tuo account IAM. Per ulteriori informazioni, vedi la documentazione di [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial).

## volume_crn_cname_mismatch
**Messaggio**: il CRN specificato nella richiesta non può essere utilizzato per l'ambiente corrente.

Il CRN che hai specificato nella richiesta non può essere utilizzato nell'ambiente corrente. Fornisci un CRN applicabile al tuo ambiente.

## volume_encryption_key_crn_invalid
**Messaggio**: il CRN della chiave di crittografia del volume specificato nella richiesta non è valido.

Il CRN di Key Protect fornito non è valido. Ottieni il CRN della chiave root nel
servizio Cloud Identity and Access Management. Per ulteriori informazioni, vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption).

## volume_encryption_key_not_active
**Messaggio**: la chiave di crittografia del volume specificata nella richiesta non è attiva.

Attiva la chiave di esitazione o fornisci una nuova chiave. Se non riesci ad attivare la tua propria chiave, contatta l'amministratore di sistema o il [supporto clienti](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
**Messaggio**: non sei autorizzato ad accedere alla chiave di crittografia del volume fornita.

Fornisci un'altra chiave di crittografia per la quale hai accesso o contatta l'amministratore di sistema per ottenere l'accesso alla chiave corrente.

## volume_encryption_key_not_implemented
La funzione chiave di crittografia del volume non è supportata.

Non è stato possibile creare un volume utilizzando la tua chiave di crittografia. Questa versione supporta solo i volumi con crittografia gestita dal provider. Per utilizzare la tua propria chiave di crittografia, utilizza una versione di Block Storage for VPC che supporti la crittografia gestita dal cliente.
Per ulteriori informazioni, contatta il [supporto clienti](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_id_invalid
**Messaggio**: l'ID volume specificato nel parametro di richiesta non è valido.

L'ID volume che hai specificato non è nel formato corretto. Verifica di aver immesso correttamente l'ID volume e riprova. Se non sei sicuro dell'ID volume, specifica `ibmcloud is volumes` nella CLI o `GET volumes` nell'API e cerca nell'elenco dei volumi.

## volume_id_missing
**Messaggio**: ID volume mancante nel parametro di richiesta.

Devi fornire un ID volume nel parametro di richiesta quando richiami uno specifico volume.

## volume_id_not_found
**Messaggio**: volume non trovato. L'ID volume `<volume_id>` non esiste, dove `<volume_id>` è l'ID volume fornito.

L'ID volume specificato non esiste. Fornisci un ID volume valido.

## volume_iops_zero_or_negative
**Messaggio**: l'IOPS del volume deve essere maggiore di zero.

Quando crei un volume, il valore IOPS specificato nella richiesta deve essere un numero positivo. Vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) per i valori IOPS supportati.

## volume_name_duplicate
**Messaggio**: il nome volume è un duplicato. Il nome volume `<volume_name>` fornito nella richiesta esiste già, dove `<volume_name>` è il nome volume fornito.

Il nome volume specificato nella richiesta esiste già. Fornisci un nome volume che non sia attualmente in uso.

## volume_not_available
**Messaggio**: il volume non è disponibile. Il volume può essere modificato solo quando è in stato disponibile. Lo stato del volume corrente `<volume_id>` è `<volume_status>`, dove `<volume_id>` è l'ID volume fornito nella richiesta e `<volume_status>` è lo stato del volume corrente.

Un volume può essere modificato solo quando si trova in uno stato `available`. Assicurati che il volume sia disponibile prima di modificarlo.

## volume_not_deletable
**Messaggio**: eliminazione non riuscita.

Il volume può essere eliminato solo se si trova in uno stato `available` o `failed`. Assicurati che il volume sia in uno stato valido per l'eliminazione.

## volume_not_found
**Messaggio**: un volume con l'ID specificato non è stato trovato.

Verifica che l'ID volume immesso sia corretto e riprova. Per un elenco di volumi, specifica `ibmcloud is volumes` nella CLI o `GET volumes` nell'API.

## volume_not_implemented
**Messaggio**: l'operazione richiesta non è supportata in questa release.

Per ulteriori informazioni, contatta il [supporto clienti](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_profile_capacity_iops_invalid
**Messaggio**: il profilo del volume specificato nella richiesta non è valido per la capacità e/o l'IOPS forniti.

Il valore di capacità e/o IOPS del volume che hai specificato nella richiesta non è consentito dal profilo del volume. Vedi la documentazione di [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) per i valori di capacità e IOPS minimi e massimi per un profilo personalizzato.

## volume_profile_iops_invalid
**Messaggio**: il profilo del volume specificato nella richiesta non può accettare l'IOPS personalizzato.

Il tuo profilo del volume non accetta un valore IOPS personalizzato. Potresti aver specificato un profilo IOPS a livelli, che non richiede di specificare un valore IOPS. Se vuoi fornire un valore IOPS specifico, utilizza il profilo IOPS personalizzato.

## volume_profile_name_missing
**Messaggio**: nella richiesta manca il nome profilo del volume obbligatorio.

Il nome profilo del volume non è presente nel corpo della richiesta durante la creazione di un volume o nel parametro di richiesta durante il richiamo di un volume. Fornisci un nome profilo del volume nella tua richiesta e riprova. Se non conosci il nome profilo del volume, specifica `ibmcloud is volume-profiles` nella CLI o utilizza `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` nella richiesta API e cerca nell'elenco.

## volume_profile_not_found
**Messaggio**: un profilo del volume con il nome specificato non è stato trovato.

Non è stato possibile trovare un profilo del volume con quel nome. Verifica di aver fornito il nome profilo del volume corretto. Se non conosci il nome profilo del volume, specifica `ibmcloud is volume-profiles` nella CLI o utilizza `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` nella richiesta API e cerca nell'elenco.

## volume_quota_reached
**Messaggio**: la quota di volume è stata raggiunta.

Hai esaurito la quota per l'account corrente. Prova a eliminare alcuni volumi o contatta il [supporto clienti](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) per aumentare la tua quota. Vedi la documentazione per informazioni sulle [quote per l'infrastruttura Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
**Messaggio**: l'ID gruppo di risorse specificato nella richiesta non è valido.

L'ID gruppo di risorse che hai specificato nella richiesta non è valido. Verifica l'ID gruppo di risorse corretto. Dalla CLI, utilizza il comando `ibmcloud is volume VOLUME_ID`. Le informazioni risultanti includeranno il gruppo di risorse e l'ID del gruppo di risorse. Per ulteriori informazioni, vedi il [Riferimento della CLI IBM Cloud per VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_resource_group_not_authorized
**Messaggio**: l'azione corrente non è autorizzata sul gruppo di risorse specificato.

Non sei autorizzato a elencare o creare volumi nel gruppo di risorse specificato. Utilizza un gruppo di risorse diverso per il quale disponi dell'autorizzazione o richiedi l'autorizzazione al tuo amministratore per ottenere i privilegi di elenco e di creazione sul gruppo di risorse.

## volume_resource_group_not_found
**Messaggio**: non è stato possibile trovare un gruppo di risorse con l'ID specificato.

Non è stato possibile trovare l'ID gruppo di risorse perché potresti averlo immesso in modo errato o non esiste. Verifica l'ID gruppo di risorse corretto. Dalla CLI, utilizza il comando `ibmcloud is volume VOLUME_ID`. Le informazioni risultanti includeranno il gruppo di risorse e l'ID del gruppo di risorse. Per ulteriori informazioni, vedi il [Riferimento della CLI IBM Cloud per VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_start_not_found
**Messaggio**: un volume con l'ID specificato come parametro di inizio pagina non è stato trovato.

Non è stato possibile trovare l'ID volume che hai specificato nel parametro di inizio della chiamata di elenco volumi. Verifica che l'ID volume sia corretto e se disponi dell'accesso al volume. 

## volume_status_not_available
**Messaggio**: il volume con l'ID specificato non è disponibile.

Il volume non è in uno stato disponibile; potrebbe essere in uno stato in sospeso. Attendi che il volume diventi disponibile e riprova. Se il volume è in fase di eliminazione, utilizza un volume diverso. Per controllare lo stato del volume, specifica `ibmcloud is volume VOLUME_ID` nella CLI o utilizza `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` nella richiesta API.

## volume_still_attached
**Messaggio**: il volume è ancora collegato a un'istanza.

Non puoi eliminare un volume che è ancora collegato a una VSI. Se imposti l'eliminazione automatica per il volume, attendi che la VSI venga eliminata e il volume verrà scollegato e quindi eliminato. Se non hai impostato l'eliminazione automatica, scollega il volume manualmente.

## volume_template_invalid
**Messaggio**: è stato fornito un template di volume non valido.

Hai fornito un template di volume non valido per la creazione di un volume. Vedi il [Riferimento API VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per verificare che tu stia fornendo i parametri corretti nel corpo della richiesta.

## volume_update_invalid_request
**Messaggio**: la richiesta di aggiornamento del volume non è valida.

La proprietà che hai specificato nel corpo della richiesta di aggiornamento non è valida. Vedi il [Riferimento API VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} per informazioni e per la sintassi corretta delle richieste API.

## vpc_not_empty
**Messaggio**: non è possibile eliminare il VPC perché non è vuoto.

È necessario eliminare tutte le risorse nel VPC prima di poter eliminare il VPC. Un VPC contiene istanze, sottoreti, gateway pubblici, programmi di bilanciamento del carico e gateway VPN e alcune di queste risorse contengono anche delle risorse. Per i dettagli, fai riferimento alla documentazione su [come eliminare un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
**Messaggio**: le risorse si trovano in VPC diversi.

Le risorse devono trovarsi nello stesso VPC. Ad esempio, se stai tentando di collegare un gateway pubblico a una sottorete, il gateway pubblico deve trovarsi nello stesso VPC della sottorete.

Per determinare quale VPC contiene il gateway pubblico, esegui il comando `GET /public_gateways/{id}` e osserva il valore "vpc". Il comando della CLI equivalente è `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
**Messaggio**: i blocchi CIDR `<cidr_type>` hanno blocchi CIDR duplicati`<cidr_blocks>`.

Sono stati forniti dei blocchi CIDR duplicati durante la creazione della connessione. Assicurati che i blocchi CIDR siano univoci.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_created
**Messaggio**: non è stato possibile aggiungere un blocco CIDR alla connessione VPN `<vpn_connection_id>`.

Fornisci un CIDR valido che soddisfi i requisiti indicati dalla specifica.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Messaggio**: non è stato possibile eliminare un blocco CIDR dalla connessione VPN `<vpn_connection_id>`.

Fornisci un CIDR valido che esista sulla connessione. Una connessione deve avere almeno un CIDR locale e peer.

Per visualizzare i blocchi CIDR della connessione, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla i campi `local_cidrs` e `peer_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**Messaggio**: il blocco CIDR non è stato trovato nella connessione VPN `<vpn_connection_id>`.

Fornisci un CIDR valido che sia presente nella connessione.

Per visualizzare i blocchi CIDR della connessione, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla i campi `local_cidrs` e `peer_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**Messaggio**: non è stato possibile aggiornare un blocco CIDR nella connessione VPN `<vpn_connection_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
**Messaggio**: il blocco CIDR `<cidr_block>` non rappresenta un indirizzo valido.

Il valore deve essere un CIDR interno valido senza bit host impostati.

Per visualizzare i blocchi CIDR della connessione, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla i campi `local_cidrs` e `peer_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**Messaggio**: il blocco CIDR `<cidr_block_1>` si sovrappone a `<cidr_block_2>`. Due blocchi CIDR peer non possono sovrapporsi sullo stesso VPC e due blocchi CIDR locali non possono sovrapporsi sulla stessa connessione.

Fornisci un CIDR valido che soddisfi i requisiti indicati dalla specifica.

Per visualizzare i blocchi CIDR della connessione, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla i campi `local_cidrs` e `peer_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Messaggio**: il nome `<vpn_connection_name>` è già utilizzato dalla connessione VPN `<vpn_connection_id>`.

Fornisci un nome di connessione diverso. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
**Messaggio**: il nome `<vpn_connection_name>` non è un nome di connessione VPN valido.

Un nome di connessione valido inizia con una lettera, seguita da lettere, cifre, caratteri di sottolineatura o trattini.

## vpn_connection_invalid_psk_format
**Messaggio**: la PSK (pre-shared key) `<psk>` non è nel formato valido.

Una PSK valida deve contenere solo da 6 a 128 caratteri che includono lettere, cifre, `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Messaggio**: è necessario almeno un blocco CIDR locale quando si crea una connessione VPN.

Fornisci un CIDR locale valido che soddisfi i requisiti indicati dalla specifica.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Messaggio**: le sottoreti locali nelle connessioni VPN per il gateway VPN `<vpn_gateway_id>` hanno raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le sottoreti locali correnti nelle connessioni VPN, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections` e controlla il campo `local_cidrs` per ciascuna connessione.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Messaggio**: le sottoreti locali per la connessione VPN hanno raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}.

Per visualizzare le sottoreti locali correnti per una connessione VPN, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla il campo `local_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**Messaggio**: non è stato possibile creare la connessione VPN `<vpn_connection_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
**Messaggio**: non è stato possibile eliminare la connessione VPN `<vpn_connection_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_found
**Messaggio**: non è stato possibile trovare la connessione VPN `<vpn_connection_id>`.

Controlla se l'ID di connessione è corretto. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
**Messaggio**: non è stato possibile aggiornare la connessione VPN `<vpn_connection_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
**Messaggio**: almeno una coppia di CIDR locale e CIDR peer con lo stesso gateway peer è stata duplicata.

Su questo gateway esiste un'altra connessione che ha un CIDR locale e un CIDR peer corrispondenti a questa. Fornisci una serie valida di CIDR locale/peer.

## vpn_connection_peer_cidrs_required
**Messaggio**: è necessario almeno un blocco CIDR peer quando si crea una connessione VPN.

Fornisci un CIDR peer valido che soddisfi i requisiti indicati dalla specifica.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Messaggio**: le sottoreti peer nelle connessioni VPN per il gateway VPN `<vpn_gateway_id>` hanno raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le sottoreti peer correnti nelle connessioni VPN, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections` e controlla il campo `peer_cidrs` per ciascuna connessione.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Messaggio**: le sottoreti peer per la connessione VPN hanno raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le sottoreti peer correnti per una connessione VPN, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e controlla il campo `peer_cidrs`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**Messaggio**: impossibile aggiungere un instradamento statico per il blocco CIDR peer `<peer_cidr>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Messaggio**: impossibile rimuovere un instradamento statico per il blocco CIDR peer `<peer_cidr>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
**Messaggio**: non è possibile modificare i blocchi CIDR durante l'aggiornamento di una connessione. Utilizza l'API corretta durante la creazione o l'eliminazione dei blocchi CIDR.

Fornisci un valore di richiesta valido che soddisfi i requisiti indicati dalla specifica.

Per ulteriori istruzioni per risolvere questo problema, fai riferimento alla [documentazione API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connections_quota_exceeded
**Messaggio**: non è possibile creare la connessione VPN perché il gateway VPN `<vpn_gateway_id>` ha raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare le connessioni VPN correnti per un gateway VPN, utilizza l'API `GET /vpn_gateways/<vpn_gateway_id>/connections`.
Comando della CLI equivalente: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**Messaggio**: il nome `<vpn_gateway_name>` è già utilizzato dal gateway VPN `<vpn_gateway_id>`.

Fornisci un nome di gateway VPN diverso. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
**Messaggio**: non è stato possibile inizializzare il gateway VPN.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
**Messaggio**: il nome `<vpn_gateway_name>` non è un nome di gateway VPN valido.

Un nome di gateway VPN valido inizia con una lettera, seguita da lettere, cifre, caratteri di sottolineatura o trattini.

## vpn_gateway_invalid_state
**Messaggio**: il gateway VPN `<vpn_gateway_id>` si trova in uno stato non valido per l'operazione richiesta.

Il gateway VPN deve trovarsi nello stato `available` prima di poterlo utilizzare. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
**Messaggio**: impossibile creare un indirizzo IP per il gateway VPN.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
**Messaggio**: non è stato possibile trovare un identificativo di sottorete per il template di gateway VPN fornito.

L'**ID sottorete** è un campo obbligatorio. Fornisci un ID sottorete con il tuo comando.

## vpn_gateway_missing_name
**Messaggio**: non è stato possibile trovare un nome per il template di gateway VPN fornito.

Il **nome** è un campo obbligatorio. Fornisci un nome con il tuo comando.

## vpn_gateway_not_created
**Messaggio**: non è stato possibile creare il gateway VPN `<vpn_gateway_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
**Messaggio**: non è stato possibile eliminare il gateway VPN `<vpn_gateway_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_found
**Messaggio**: il gateway VPN `<vpn_gateway_id>` non è stato trovato.

Controlla se l'ID del gateway VPN è corretto. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
**Messaggio**: non è stato possibile aggiornare il gateway VPN `<vpn_gateway_id>`.

Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_found
**Messaggio**: non è stato possibile trovare la sottorete `<subnet_id>` per il gateway VPN fornito.

Hai fatto riferimento a una sottorete che non esiste. Riesamina la tua richiesta per assicurarti di aver specificato l'ID di sottorete corretto. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Messaggio**: non è stato possibile creare il gateway VPN a causa dello stato della sottorete.

Fornisci una sottorete che sia nello stato `available`. Riprova tra qualche minuto. Se il problema persiste, [contatta il supporto](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Messaggio**: non è possibile creare il gateway VPN perché il tuo account e/o la regione ha raggiunto la quota.

Le quote per ciascuna risorsa sono specificate nella pagina [Quote](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Per visualizzare i gateway VPN correnti, utilizza l'API `GET /vpn_gateways`.
Comando della CLI equivalente: `ibmcloud is vpn-gateways`

## zone_inconsistency
**Messaggio**: le risorse devono appartenere alla stessa zona.

* Quando colleghi un volume, assicurati che il volume e l'istanza si trovino nella stessa zona.
* Quando associ un IP mobile, assicurati che l'IP mobile e la sottorete si trovino nella stessa zona.
