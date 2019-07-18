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

# Fehlernachrichten in der IBM Cloud Virtual Private Cloud-API
{: #rias-error-messages}

Wenn Sie eine Fehlernachricht von den {{site.data.keyword.cloud}} Virtual Private Cloud-APIs erhalten, können Sie die Nachrichten-ID verwenden, um weitere Informationen zur Lösung des Problems zu erhalten.
{:shortdesc}

## account_type_invalid
**Nachricht**: Sie müssen über ein nutzungsabhängiges Konto verfügen, damit eine virtuelle private Cloud bereitgestellt wird.

Ihr Konto muss Bestandteil eines nutzungsabhängigen Plans sein, damit VPCs bereitgestellt werden. Weitere Details zum Upgrade Ihres Kontos finden Sie unter [Upgrade für eigenes Konto durchführen](/docs/account?topic=account-accounts#upgrade-lite-account).

## acl_in_use
**Nachricht**: Die Netz-ACL kann nicht gelöscht werden, weil sie an Ressourcen angehängt ist.

Die Netz-ACL kann nicht gelöscht werden, weil sie einem Teilnetz oder einer VPC zugeordnet ist.

Wenn Sie feststellen möchten, ob die Netz-ACL von einem Teilnetz genutzt wird, verwenden Sie die API `GET /v1/subnets?version=2019-05-31&generation=1`; funktional entsprechender CLI-Befehl: `ibmcloud is subnets`. Verwenden Sie zum Aktualisieren einer Netz-ACL, die von einem Teilnetz genutzt wird, die API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` oder die CLI `ibmcloud is subnet-update`.

Wenn Sie feststellen möchten, ob die Netz-ACL von einer VPC genutzt wird, verwenden Sie die API `GET /v1/vpcs?version=2019-05-31&generation=1`. Der funktional entsprechende Befehl in der CLI lautet `ibmcloud is vpcs`. Es ist nicht möglich, eine Standardnetz-ACL zu aktualisieren oder zu löschen, die von einer VPC verwendet wird. Die Standardnetz-ACL wird automatisch gelöscht, wenn die VPC gelöscht wird. 

## address_prefix_conflict
**Nachricht**: Das Adresspräfix mit dieser CIDR wird verwendet.

Die CIDR für das angeforderte Adresspräfix steht in Konflikt mit einem vorhandenen Adresspräfix.

Verwenden Sie zum Anzeigen der Liste mit den Adresspräfixen für eine VPC die API `GET /vpcs/{vpc-id}/address_prefixes` und stellen Sie sicher, dass die angeforderte CIDR-Adresse nicht im Feld `cidr` eines anderen Adresspräfixes verwendet wird. Funktional entsprechender CLI-Befehl: `ibmcloud ist vpc-address-prefix`. 

## address_prefix_in_use
**Nachricht**: Ein Adresspräfix, das aktuell verwendet wird, kann nicht gelöscht werden.

Ein oder mehrere Teilnetze verwenden das Adresspräfix. Wenn Sie feststellen möchten, von welchen Teilnetzen das Adresspräfix genutzt wird, verwenden Sie den API-Befehl `GET /v1/subnets?version=2019-05-31&generation=1` und überprüfen das Feld `ipv4_cidr_block`. Funktional entsprechender CLI-Befehl: `ibmcloud is subnets`; überprüfen Sie die Spalte `Subnet CIDR`. Sie müssen alle Teilnetze löschen, von denen das Adresspräfix verwendet wird, damit das Adresspräfix gelöscht werden kann.

## backend_service_unavailable
**Nachricht**: Der Back-End-Service ist nicht verfügbar.

Von einem Back-End-Cloud-Service, der von einer VPC verwendet wird, konnte nicht geantwortet werden. Von der VPC werden mehrere IBM Cloud-Services verwendet; hierzu gehören zum Beispiel die folgenden: 

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Global Catalog](https://{DomainName}/catalog)
- Resource Controller
- Resource Manager

Sie können den [Status](https://{DomainName}/status) der IBM Cloud-Services überprüfen und es in wenigen Minuten erneut versuchen. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_field
**Nachricht**: Korrekte Instanz-UUID muss angegeben werden.

Einer der in der Anforderung angegebenen Werte ist falsch. Überprüfen Sie den Wert für `target` im zurückgegebenen Fehler auf Hinweise darauf, welcher Parameter falsch war. In manchen Fällen kann die UUID oder der Datenträger nicht in der Anforderung gefunden werden. Geben Sie einen gültigen Wert an und versuchen Sie es erneut.

Weitere Hilfe finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_request
**Nachricht**: Die angegebenen Informationen waren ungültig, fehlerhaft oder es fehlt ein erforderliches Feld.

Die Anfrage entsprach nicht den Erwartungen an eine Anfrage. Verwenden Sie die [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} als Hilfe beim Formatieren der Anforderung. 

Wenn die Anfrage die Anforderungen an die Spezifikation erfüllt und trotzdem noch ein Fehler auftritt, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Nachricht**: In einer Region kann nur eine VPC mit klassischem Zugriff erstellt werden. 

Pro Region kann nur eine VPC mit klassischem Zugriff erstellt werden. Wenn Sie die VPCs mit klassischem Zugriff auflisten möchten, führen Sie `ibmcloud is vpcs --classic-access` aus. Eine vorhandene VPC mit klassischem Zugriff muss gelöscht werden, damit eine andere mit klassischem Zugriff erstellt werden kann.

## classic_access_vpc_account_not_VRF_enabled
**Nachricht**: Das Konto muss VRF-fähig sein, damit eine VPC mit klassischem Zugriff erstellt werden kann.

Das verknüpfte klassische Konto ist nicht VRF-fähig. Für eine VPC mit klassischem Zugriff VPC ist es erforderlich, dass das verknüpfte klassische Konto VRF-fähig ist. Informationen zum Aktivieren des Kontos für VRF finden Sie unter [VRF in IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_found
**Nachricht**: Das Standardadresspräfix wurde nicht gefunden.

Diese Fehlernachricht wird möglicherweise angezeigt, wenn das Standardadressenpräfix nicht gefunden wird.

## duplicate_error
**Nachricht**: Die bereitgestellte Eingabe ist bereits vorhanden.

Die angegebene Ressource ist bereits vorhanden. Versuchen Sie, einen anderen Namen für die Ressource zu verwenden, die Sie erstellen möchten. Wenn Sie zum Beispiel eine neue VPC erstellen, können Sie die vorhandenen VPCs durch Ausführen des Befehls `ibmcloud is vpcs` auflisten. Wählen Sie einen Namen aus, durch den keine Konflikte verursacht werden. 

## floating_ip_in_use
**Nachricht**: Die variable IP-Adresse wird verwendet.

Diese variable IP-Adresse ist einer virtuellen Serverinstanz oder einem öffentlichen Gateway zugeordnet.

Wenn Sie anzeigen möchten, an welchen Positionen die variable IP-Adresse verwendet wird, verwenden Sie die API `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` und überprüfen die Werte für 'target'. Der funktional entsprechende Befehl in der CLI lautet `ibmcloud is floating-ip FLOATINGIP_ID`. 

## gateway_too_many
**Nachricht**: Es kann nur ein öffentliches Gateway pro Zone in einer VPC vorhanden sein.

Es ist nur ein öffentliches Gateway pro Zone in einer VPC zulässig, aber das öffentliche Gateway kann mehreren Teilnetzen in der Zone zugeordnet werden. Um das öffentliche Gateway für eine Zone zu finden, führen Sie die API "GET public_gateways" aus und sehen Sie sich die Werte für "vpc" und "zone" an. Wenn Sie die CLI verwenden, können Sie den Befehl `ibmcloud is public-gateways` ausführen und den Wert für "vpc" und "zone" anzeigen.

Verwenden Sie die ID des öffentlichen Gateways, um sie einem Teilnetz zuzuordnen. Sehen Sie sich hierzu ein Beispiel in den [API-Beispielen](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-) an. Wenn Sie die CLI verwenden, können Sie den Befehl zum Aktualisieren des Teilnetzes verwenden, um es einem öffentlichen Gateway zuzuordnen, z. B. `ibmcloud is subnet-update TEILNETZ-ID --public-gateway-id ID_DES_ÖFFENTLICHEN_GATEWAYS`.

## health_monitor_invalid_delay
**Nachricht**: Die angegebene Verzögerung bei Statusüberwachung <health_monitor_delay> ist ungültig.

Geben Sie einen Wert zwischen 2 und 60 für den Parameter für die Verzögerung bei Statusüberwachung (`health monitor delay`) an. 

## health_monitor_invalid_max_retries
**Message**: Der angegebene Wert für die maximale Anzahl der Wiederholungen für die Statusüberwachung <health_monitor_max_retries> ist ungültig. 

Geben Sie einen Wert zwischen 1 und 10 für den Parameter für die maximale Anzahl der Wiederholungen für die Statusüberwachung (`health max retries`) an.

## health_monitor_invalid_timeout
**Nachricht**: Das angegebene Zeitlimit für die Statusüberwachung <health_monitor_timeout> ist ungültig.

Geben Sie einen Wert zwischen 1 und 59 für das Zeitlimit für die Statusüberwachung (`health timeout`) an. Der ausgewählte Wert muss kleiner als der Wert von `health monitor delay` sein.

## health_monitor_invalid_url_path
**Nachricht**: Der angegebene URL-Pfad zur Statusüberwachung <health_monitor_url> ist ungültig.

Geben Sie eine gültige URL für die Statusüberwachung an. Die URL kann alphanumerische Zeichen, Bindestriche und Punkte enthalten. Leerzeichen und umgekehrte Schrägstriche sind nicht zulässig. Die maximale Länge des URL-Pfads beträgt 255 Zeichen.

## health_monitor_invalid_protocol
**Nachricht**: Das angegebene Protokoll für die Statusüberwachung <health_monitor_protocol> ist ungültig.

Geben Sie einen gültigen Wert für das Protokoll der Statusüberwachung an. Zulässige Werte sind `http` und `tcp`.

## health_monitor_missing_protocol
**Nachricht**: Das Protokoll der Statusüberwachung fehlt. 

Das Feld für das Protokoll der Statusüberwachung ist ein erforderliches Feld. Geben Sie in der Anforderung das Protokoll der Statusüberwachung an. Die zulässigen Werte sind `http` und `tcp`.

## health_monitor_missing_url_path
**Nachricht**: Der URL-Pfad für die Statusüberwachung fehlt. Für eine Statusüberwachung des Typs HTTP ist er erforderlich. 

Für eine Statusüberwachung, von der das HTTP-Protokoll verwendet wird, ist ein URL-Pfad für die Statusüberwachung erforderlich. Geben Sie einen gültigen URL-Pfad an.

## health_monitor_timeout_delay_conflict
**Nachricht**: Der Wert des Zeitlimits für die Statusüberwachung <health_monitor_timeout> darf nicht größer-gleich dem Wert für die Verzögerung <health_monitor_delay> sein.

Der Wert des Parameters `health monitor delay` muss größer als der Wert des Parameters `health monitor timeout` sein. 

## http_request_size_exceeded
**Nachricht**: Die HTTP-Anforderung ist zu groß.

Dieses Problem tritt auf, wenn die von Ihnen in Ihrer Anforderung gesendeten Nutzdaten zu viele Zeichen enthalten. Versuchen Sie es erneut mit einer kleineren Nutzlast. Anstatt zu versuchen, alles in einer einzigen Anforderung zu verarbeiten, versuchen Sie, eine minimale Ressource in einer Anforderung zu erstellen und anschließend den Status in mehreren nachfolgenden Anforderungen inkrementell entsprechend anhängen.

Weitere Hilfe finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
**Nachricht**: Keine

Im IAM-Service ist ein Fehler bei der Authentifizierung oder Autorisierung aufgetreten. Die Ausfallzeit des IAM-Service kann temporär sein. Wiederholen Sie die Anforderung nach einigen Minuten. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
**Nachricht**: Die IKE-Richtlinie kann nicht erstellt werden, weil das Kontingent für Ihr Konto ausgeschöpft ist. 

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen IKE-Richtlinien anzuzeigen, verwenden Sie die API `GET /ike_policies`. Funktional entsprechender CLI-Befehl: `ibmcloud ist ike-policies`. 

## ike_policy_duplicate_name
**Nachricht**: Der Name `<ike_policy_name>` wird bereits von der IKE-Richtlinie `<ike_policy_id>` verwendet.

Geben Sie einen anderen IKE-Richtlinienname an. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
**Nachricht**: Die IKE-Richtlinie `<ike_policy_id>` wird von mindestens einer anderen Verbindung verwendet.

Eine IKE-Richtlinie kann nicht gelöscht werden, wenn sie von einer oder mehreren Verbindungen verwendet wird.

Wenn Sie feststellen möchten, von welchen Verbindungen die IKE-Richtlinie verwendet wird, verwenden Sie die API `GET /ike_policies/<ike_policy_id>/connections`. Funktional entsprechender CLI-Befehl: `ibmcloud is ike-policy-connections IKE_POLICY_ID`.

## ike_policy_invalid_name
**Nachricht**: Der Name `<ike_policy_name>` ist kein gültiger IKE-Richtlinienname.

Ein gültiger IKE-Richtlinienname beginnt mit einem Buchstaben, gefolgt von Buchstaben, Ziffern, Unterstrichen oder Bindestrichen.

## ike_policy_not_created
**Nachricht**: Die IKE-Richtlinie `<ike_policy_id>` wurde nicht erstellt. 

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
**Nachricht**: Die IKE-Richtlinie `<ike_policy_id>` konnte nicht gelöscht werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_found
**Nachricht**: Die IKE-Richtlinie `<ike_policy_id>` wurde nicht gefunden.

Sie haben auf eine IKE-Richtlinie verwiesen, die nicht vorhanden ist. Überprüfen Sie Ihre Anforderung, um sicherzustellen, dass Sie die richtige IKE-Richtlinien-ID angegeben haben. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
**Nachricht**: Die IKE-Richtlinie `<ike_policy_id>` konnte nicht aktualisiert werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
**Nachricht**: Die Instanz kann im aktuellen Status nicht gelöscht werden.

Sie können eine Instanz nicht löschen, wenn sie den Status `deleting`, `pending`, `starting`, `stopping` oder `restarting` aufweist. Die Instanz muss sich im Status `running` oder `stopped` befinden. Wenn sich der Status nicht ändert, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
**Nachricht**: Die Instanz kann nicht erstellt werden, weil der Hostname nicht gültig ist. 

Der Hostname einer Instanz muss die folgenden Voraussetzungen erfüllen:
* Der Hostname darf nur alphanumerische Zeichen und Gedankenstriche enthalten.
* Großbuchstaben sind nicht zulässig.
* Führende und abschließende Gedankenstriche sind nicht zulässig.
* Führende Zahlen sind nicht zulässig.
* Aufeinanderfolgende Gedankenstriche sind nicht zulässig.
* Die maximale Länge beträgt 15 Zeichen unter Windows.
* Die maximale Länge beträgt 40 Zeichen unter anderen Betriebssystemen.

## instance_invalid_port_speed
**Nachricht**: Die Instanz kann nicht erstellt werden, weil die angegebene Geschwindigkeit des Netzschnittstellenports nicht gültig ist. 

Die Geschwindigkeit des Netzschnittstellenports muss `100` oder `1000` MB/s betragen. 

## instance_name_exists
**Nachricht**: Derselbe Instanzname ist bereits vorhanden.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
**Nachricht**: Die Instanz kann nicht erstellt werden, weil die Anzahl der Datenträgerzuordnungen das Kontingent überschreiten würde.

Die VPC-Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window} angegeben.

## instance_too_many_keys
**Nachricht**: Die Windows-Instanz kann nicht erstellt werden, weil die Anforderung mehrere Schlüssel enthält. 

Von Windows-Instanzen wird nur ein Schlüssel unterstützt.

## insufficient_space_for_subnet
**Nachricht**: Unzureichender Platz für Teilnetz in Adresspräfix.

Das Teilnetz kann nicht erstellt werden, weil die Anzahl der angeforderten Adressen nicht zugeordnet werden kann. Versuchen Sie es unter Verwendung einer niedrigeren Anzahl an Adressen oder eines höheren CIDR-Werts erneut. 

## internal_error
**Nachricht**: Es ist ein interner Fehler aufgetreten.

Es ist ein unerwarteter Fehler aufgetreten. Dieses Problem kann temporär sein. Wiederholen Sie die Anforderung in ein paar Minuten. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
**Nachricht**: Keine

Es ist ein unerwarteter Fehler aufgetreten. Dieses Problem kann temporär sein. Wiederholen Sie die Anforderung in ein paar Minuten. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
**Nachricht**: Wenden Sie sich an Ihren Administrator.

Es ist ein unerwarteter Fehler aufgetreten. Dieses Problem kann temporär sein. Wiederholen Sie die Anforderung in ein paar Minuten. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_generation_parameter
**Nachricht**: Für den Parameter zur Generierung von Abfragen muss der Wert 1 festgelegt sein. 

Für Versionen ab dem 31. 5. 2019 muss für den Abfrageparameter `generation` der Wert 1 festgelegt sein, damit API-Anforderungen von Virtual Private Cloud on Classic verarbeitet werden können.

## invalid_id_format
**Nachricht**: Fehlerhaftes ID-Format. Stellen Sie sicher, dass das Format korrekt ist.

Stellen Sie sicher, dass die ID, die Sie angegeben haben, keine fehlerhaften Daten enthält.

Dieser Fehler kann auftreten, wenn beim Herstellen einer Anforderung für die Seitenaufteilung eine fehlerhafte Startabfrage angegeben wird. Beispiel: `GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` enthält zwei Fragezeichen (`?`). Korrigieren Sie die Abfrage und versuchen Sie es erneut.

## invalid_route
**Nachricht**: Die angeforderte Route ist nicht vorhanden.

Die angeforderte Route für die von Ihnen angegebene API-URL ist nicht vorhanden. Stellen Sie sicher, dass die URL korrekt ist, die Sie zum Aufrufen des API-Endpunkts angegeben haben. 

## invalid_request_field
**Nachricht**: Ein in der Anforderung angegebenes Feld ist nicht gültig.

Beispiel: Verwenden Sie zum Aktualisieren einer Netz-ACL, die von einem Teilnetz genutzt wird, die API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’`. Die folgende Anforderung wäre ungültig, weil 'networkacl' kein gültiges Feld ist: `PATCH /v1/subnets/ {2}{0}{1}{9}-05-31 &generation=1 -d '{"}}'`

Informationen zu den gültigen Werten finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. 

## invalid_state
**Nachricht**: Eine Aktion wurde für eine Ressource angefordert, die beim aktuellen Status der Ressource nicht unterstützt wird.

Wenn es sich bei der Ressource um eine Instanz handelt:

1. Für die Instanz kann bereits eine Warmstartoperation ausgeführt werden. Abhängig vom Status der Instanz finden Sie Informationen hierzu unter [Zulässige Aktionen](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance).

2. Während der Status der Instanz `pending` lautet, können Sie die folgenden Aktionen nicht ausführen:
  * Die Instanz löschen
  * Einen Datenträger der Instanz zuordnen
  * Die Zuordnung eines Datenträgers zur Instanz aufheben
  * Die Instanz aktualisieren

Versuchen Sie später erneut, die Aktion auszuführen. Wenn sich der Status der Ressource nicht ändert, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

Wenn es sich bei der Ressource um ein Image handelt:

Während der Status der Instanz `deleting` lautet, können Sie die folgenden Aktionen nicht ausführen:
  * Patch auf Image anwenden
  * Image löschen

Erstellen Sie die Anforderung nicht erneut. Für das Löschen des Image ist ein gewisser Zeitraum erforderlich. Sobald der Löschvorgang abgeschlossen ist, schlagen alle Anfragen für das Image mit dem Fehler `not_found` fehl. 

## invalid_version
**Nachricht**: Der Parameter `version` ist ungültig. Er muss das Format `JJJJ-MM-TT` haben.

Die Version muss das Format _JJJJ-MM-TT_ einhalten. Für einstellige Monats- oder Datumsangaben, wie z. B. den 1. Januar, sollte die Version folgendermaßen aussehen: `2019-01-01`. Der Versionsparameter muss in der URL vorhanden sein. Das im Versionsparameter angegebene Datum muss nach '2019-01-01' und vor dem aktuellen Datum liegen. 

## invalid_version_range
**Nachricht**: Der Wert für `version` kann nicht auf ein zukünftiges Datum oder auf ein Datum vor `2019-01-01` gesetzt werden.

Das im Versionsparameter angegebene Datum muss nach dem 01. 01. 2019 (2019-01-01) und vor dem aktuellen Datum liegen. 

## invalid_zone
**Nachricht**: Überprüfen Sie, ob die Ressourcen, die Sie anfordern, sich in derselben Zone befinden.

Diese Nachricht wird möglicherweise angezeigt, wenn versucht wird, ein öffentliches Gateway in Zone 1 einem Teilnetz in Zone 2 zuzuordnen. 

## ipsec_policies_quota_exceeded
**Nachricht**: Die IPsec-Richtlinie kann nicht erstellt werden, weil das Kontingent für Ihr Konto ausgeschöpft ist. 

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen IPsec-Richtlinien anzuzeigen, verwenden Sie die API `GET /ipsec_policies`. Funktional entsprechender CLI-Befehl: `ibmcloud ist ipsec-policies`. 

## ipsec_policy_duplicate_name
**Nachricht**: Der Name `<ipsec_policy_name>` wird bereits von der IPsec-Richtlinie `<ipsec_policy_id>` verwendet.

Geben Sie einen anderen IPsec-Richtlinienname an. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
**Nachricht**: Die IPsec-Richtlinie `<ipsec_policy_id>` wird von mindestens einer anderen Verbindung verwendet.

Eine IPsec-Richtlinie kann nicht gelöscht werden, wenn sie von einer oder mehreren Verbindungen verwendet wird.

Wenn Sie feststellen möchten, von welchen Verbindungen die IPsec-Richtlinie verwendet wird, verwenden Sie die API `GET /ipsec_policies/<ipsec_policy_id>/connections`. Funktional entsprechender CLI-Befehl: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`.

## ipsec_policy_invalid_name
**Nachricht**: Der Name `<ipsec_policy_name>` ist kein gültiger IPsec-Richtlinienname.

Ein gültiger IPsec-Richtlinienname beginnt mit einem Buchstaben, gefolgt von Buchstaben, Ziffern, Unterstrichen oder Bindestrichen.

## ipsec_policy_not_created
**Nachricht**: Die IPsec-Richtlinie `<ipsec_policy_id>` wurde nicht erstellt. 

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
**Nachricht**: Die IPsec-Richtlinie `<ipsec_policy_id>` konnte nicht gelöscht werden. 

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_found
**Nachricht**: Die IPsec-Richtlinie `<ipsec_policy_id>` wurde nicht gefunden.

Sie haben auf eine IPsec-Richtlinie verwiesen, die nicht vorhanden ist. Überprüfen Sie die Anforderung und geben Sie eine gültige IPsec-Richtlinien-ID an. Wenn Sie sicher sind, dass die Richtlinie vorhanden ist, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
**Nachricht**: Die IPsec-Richtlinie `<ipsec_policy_id>` konnte nicht aktualisiert werden. 

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
**Nachricht**: Der gleiche Schlüsselinhalt ist bereits vorhanden.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
**Nachricht**: Derselbe Schlüsselname ist bereits vorhanden.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
**Nachricht**: Der Schlüssel kann nicht erstellt werden, weil der Name nicht gültig ist. 

Der Schlüsselname muss die folgenden Voraussetzungen erfüllen:
* Er muss mit einem alphabetischen Zeichen (A-Z oder a-z) beginnen
* Er darf nur alphanumerische Zeichen, Gedankenstriche oder Unterstreichungszeichen (-, A-Z, a-z, 0-9 und _) enthalten
* Die Länge darf 65 Zeichen nicht überschreiten

## key_invalid_type
**Nachricht**: Der Schlüssel kann nicht erstellt werden, weil der Typ nicht gültig ist. 

Der einzige unterstützte Schlüsseltyp ist `rsa`.

## key_parse_failure
**Nachricht**: Der Schlüssel kann nicht erstellt werden, weil der Wert des öffentlichen Schlüssels nicht gültig ist. 

Der in der Anforderung angegebene öffentliche Schlüssel ist nicht gültig. Überprüfen Sie den Wert oder generieren Sie den öffentlichen Schlüssel erneut, und versuchen Sie es erneut.

Unter Linux-Betriebssystemen können Sie den folgenden Befehl ausführen, um den gültigen öffentlichen Schlüssel `ssh-keygen -l -v -f <key_file>` zu verwenden.

## listener_certificate_not_found
**Nachricht**: Zertifikatsinstanzen mit dem CRN `<listener_certificate_crn>` wurden nicht gefunden oder Sie haben keine Berechtigung für den Zugriff auf die Zertifikatsinstanz.

Geben Sie einen vorhandenen CRN für die Zertifikatsinstanz an oder bitten Sie Ihren Kontoadministrator, Ihnen Zugriffsberechtigungen für die bereitgestellte Zertifikatsinstanz zu erteilen.

## listener_duplicate_port
**Nachricht**: Port `<listener_port>` wird von einer anderen Listenerinstanz verwendet. Wählen Sie einen anderen Port aus.

Wählen Sie einen anderen Port aus.

## listener_invalid_certificate
**Nachricht**: Der CRN der Zertifikatsinstanz für den HTTPS-Listener ist ungültig.

Geben Sie einen gültigen CRN für die Zertifikatsinstanz an.

## listener_invalid_connection_limit
**Nachricht**: Limit `<listener_connection_limit>` für Listenerverbindung ist ungültig.

Geben Sie ein gültiges Verbindungslimit an. Das Verbindungslimit muss eine ganze Zahl zwischen 1 und 15000 sein.

## listener_invalid_port
**Nachricht**: Listener-Port `<listener_port>` ist ungültig.

Wählen Sie einen anderen Port aus.

## listener_invalid_protocol
**Nachricht**: Das Listenerprotokoll `<listener_protocol>` ist ungültig.

Geben Sie ein gültiges Listenerprotokoll an. Zulässige Werte für das Listenerprotokoll sind `http`, `https` und `tcp`.

## listener_missing_certificate_crn
**Nachricht**: CRN der Zertifikatsinstanz für den `https `-Listener fehlt.

Der CRN der Zertifikatsinstanz ist ein erforderliches Feld.

## listener_missing_port
**Nachricht**: Der Listener-Port fehlt.

Der Listener-Port ist ein erforderliches Feld.

## listener_missing_protocol
**Nachricht**: Das Listenerprotokoll fehlt.

Das Listenerprotokoll ist ein erforderliches Feld. Geben Sie das Listenerprotokoll in Ihrer Anforderung an. Die zulässigen Werte sind `http`, `https` und `tcp`.

## listener_not_found
**Nachricht**: Der Listener mit der ID `<listener_id>` wurde nicht gefunden.

Geben Sie eine vorhandene Listener-ID an.

## listener_over_quota
**Nachricht**: Der Listener kann nicht erstellt werden. Das Kontingent von Listener für die Ressource der Lastausgleichsfunktion hat den Maximalwert erreicht.

Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window} angegeben. 

## listener_pool_protocols_conflict
**Nachricht**: Das Listenerprotokoll (`<listener_protocol>`) und das Poolprotokoll (`<pool_protocol>`) stehen in Konflikt.

Ein Listener mit dem Protokoll `https` oder `http` kann nur einem Pool mit dem Protokoll `http` zugeordnet werden. In ähnlicher Weise kann ein `tcp`-Listener nur einem `tcp`-Pool zugeordnet werden.

## listener_reserved_port_conflict
**Nachricht**: Der Listener-Port `<listener_port>` ist einer der reservierten Ports. Der Portbereich 56500 bis 56520 ist für Verwaltungszwecke reserviert. Wählen Sie einen anderen Port aus.

Wählen Sie einen anderen Port aus.

## load_balancer_delete_conflict
**Nachricht**: Die Lastausgleichsfunktion mit der ID <id_der_lastausgleichsfunktion> kann nicht gelöscht werden, weil sie eine der folgenden Statusarten aufweist:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Löschen Sie die Lastausgleichsfunktion, wenn sie sich im Status ACTIVE befindet.

## load_balancer_duplicate_name
**Nachricht**: Der Name `<load_balancer_name>` wird von einer anderen Instanz der Lastausgleichsfunktion verwendet. Wählen Sie einen anderen Namen aus.

Geben Sie einen anderen Namen für die Instanz der Lastausgleichsfunktion an. Der Name der Lastausgleichsfunktion muss innerhalb der VPC und des Kontos eindeutig sein.

Die Lastausgleichsfunktion für die VPC verursacht keine Konflikte mit der Lastausgleichsfunktion der Cloud, weil die DNS-Domänennamen für diese Services unterschiedlich sind und der Name der Lastausgleichsfunktion für die VPC nicht im DNS-Namen enthalten ist.
{: note}

## load_balancer_insufficient_ips
**Nachricht**: Das Teilnetz mit der ID bzw. den IDs <teilnetz-id> verfügt nicht über ausreichend verfügbare IPv4-Adressen.

Geben Sie in der Anforderung ein gültiges Teilnetz mit verfügbaren IPv4-Adressen an, in dem die Lastausgleichsfunktion erstellt werden soll.

## load_balancer_invalid_description
**Nachricht**: Die Beschreibung `<load_balancer_description>` der Lastausgleichsfunktion ist ungültig.

Eine Beschreibung einer Lastausgleichsfunktion darf 255 Zeichen nicht überschreiten.

## load_balancer_invalid_is_public
**Nachricht**: Der Wert von 'is_public' für `<load_balancer_is_public>` ist ungültig.

Der Wert für den Parameter `is_public` der Lastausgleichsfunktion muss entweder _true_ oder _false_ lauten.

## load_balancer_invalid_name
**Nachricht**: Der Name `<load_balancer_name>` ist ungültig.

Namen dürfen nicht leer sein. Ein gültiger Name für die Lastausgleichsfunktion beginnt mit einem Buchstaben gefolgt von Buchstaben, Ziffern und Unterstrichen. Die Länge des Namens darf 40 Zeichen nicht überschreiten. 

## load_balancer_invalid_subnet
**Nachricht**: Das Teilnetz mit der ID <teilnetz-id> ist nicht gültig. Stellen Sie sicher, dass ein vorhandenes Teilnetz mit einem verfügbaren Status ('available') verwendet wird.

Geben Sie gültige Teilnetze an, die sich im Status `available` befinden, wenn die Anforderung zum Erstellen einer Lastausgleichsfunktion eingegeben wird.

## load_balancer_missing_is_public
**Nachricht**: Das Feld 'is_public' fehlt.

Das Feld `is_public` ist erforderlich. Geben Sie einen Wert für das Feld `is_public` an.

## load_balancer_missing_name
**Nachricht**: Der Name der Lastausgleichsfunktion fehlt.

Das Feld `Name` für den Namen der Lastausgleichsfunktion ist ein erforderliches Feld. Geben Sie einen Namen für die Lastausgleichsfunktion an.

## load_balancer_missing_subnets
**Nachricht**: Die Teilnetze der Lastausgleichsfunktion fehlen.

Das Feld `subnets` ist ein erforderliches Feld. Geben Sie in Ihrer Anforderung zum Erstellen einer Lastausgleichsfunktion die Informationen zu den Teilnetzen an, in denen die Lastausgleichsfunktion erstellt werden soll.

## load_balancer_missing_subnet_id
**Nachricht**: Die Subnet-ID fehlt.

Das Feld **subnet-id** ist ein erforderliches Feld. Geben Sie mit dem Befehl eine Teilnetz-ID an.

## load_balancer_not_found
**Nachricht**: Die Lastausgleichsfunktion mit der ID `<load_balancer_id>` wurde nicht gefunden.

Geben Sie die ID einer vorhandenen Lastausgleichsfunktion an.

## load_balancer_over_quota
**Nachricht**: Die Lastausgleichsfunktion kann nicht erstellt werden. Das Kontingent von Instanzen der Lastausgleichsfunktion unter Ihrem Konto hat den Maximalwert erreicht.

Löschen Sie eine vorhandene Lastausgleichsfunktion oder wenden Sie sich an den Support, um das Kontingent für die Lastausgleichsfunktion für Ihr Konto zu erhöhen.

Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window} angegeben. 

## load_balancer_subnet_not_found
**Nachricht**: Das Teilnetz mit der ID <teilnetz-id> kann nicht gefunden werden. 

Sie müssen in Ihrer Anforderung die IDs vorhandener Teilnetze angeben, in denen die Lastausgleichsfunktion erstellt werden soll.

## load_balancer_unchanged_update
**Nachricht**: Es gibt nichts, mit dem die ID `<load_balancer_id>` der Lastausgleichsfunktion aktualisiert werden kann.

Zum Aktualisieren der Instanz einer Lastausgleichsfunktion mit der von Ihnen angegebenen ID wurden keine Informationen angegeben.

## load_balancer_update_conflict
**Nachricht**: Die Lastausgleichsfunktion mit der ID <id_der_lastausgleichsfunktion> kann nicht aktualisiert werden, weil sie eine der folgenden Statusarten aufweist:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Aktualisieren Sie die Lastausgleichsfunktion, wenn sie sich im Status ACTIVE befindet.

## member_duplicate_address_and_port
**Nachricht**: Ein Mitglied mit der Adresse <mitgliedsadresse> und dem Port <mitlgiedsport> ist bereits im Pool vorhanden. 

Geben Sie in Ihrer Anforderung eine eindeutige Kombination aus Mitgliedsadresse und -port an. 

## member_invalid_address
**Nachricht**: Die angegebene IP-Adresse <mitglieds-ip> des Mitglieds ist ungültig.

Geben Sie in der Anforderung eine gültige IPv4-CIDR-Adresse für das Mitglied an.

## member_invalid_port
**Nachricht**: Der angegebene Mitgliedsport `<member_port>` ist ungültig.

Geben Sie eine gültige Portnummer an. Eine gültige Portnummer liegt zwischen 1 und 65535.

## member_invalid_weight
**Nachricht**: Die angegebene Gewichtung `<member_weight>` für das Mitglied ist ungültig.

Geben Sie eine gültige Gewichtung für das Mitglied an. Der Wert muss eine ganze Zahl zwischen 0 und 100 sein.

## member_ip_not_found
**Nachricht**: Das Mitglied mit IP-Adresse <mitglieds-IP> gehört nicht zu Teilnetzen in der Region und der VPC der Lastausgleichsfunktion.

Geben Sie in der Anforderung die Adresse eines Mitglieds an, das zu einem Teilnetz in derselben Region und VPC gehört, in der sich die Lastausgleichsfunktion befindet.

## member_missing_address
**Nachricht**: Die Mitgliedsadresse fehl.

Die Mitgliedsadresse ist ein erforderliches Feld. Geben Sie einen Wert für die Adresse des Mitglieds an. 

## member_missing_protocol_port
**Nachricht**: Der Mitgliedsport fehlt.

Der Mitgliedsport ist ein erforderliches Feld. Geben Sie einen Wert für den Port des Mitglieds an. 

## member_not_found
**Nachricht**: Das Mitglied mit der ID <mitglieds-id> kann nicht gefunden werden. 

Geben Sie eine vorhandene Mitglieds-ID an. 

## member_over_quote
**Nachricht**: Das Mitglied kann nicht erstellt werden. Das Kontingent von Mitgliedsinstanzen unter dem Pool hat den Maximalwert erreicht.

Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window} angegeben. 

## missing_generation_parameter
**Nachricht**: Der Abfrageparameter für die Generierung ist erforderlich.

Für Versionen ab dem 31. 5. 2019 ist der Abfrageparameter `generation` für API-Anforderungen von Virtual Private Cloud on Classic erforderlich. 

## missing_ims_account_id
**Nachricht**: Keine

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## missing_version
**Nachricht**: Der Parameter `version` ist erforderlich und muss das Format `JJJJ-MM-TT` haben.

Für alle API-Anforderungen ist ein Versionsparameter erforderlich. Die Version muss das Format _JJJJ-MM-TT_ einhalten. Für einstellige Monats- oder Datumsangaben, wie z. B. den 1. Januar, sollte die Version folgendermaßen aussehen: `2019-01-01`. Das im Versionsparameter angegebene Datum muss nach dem 01. 01. 2019 (2019-01-01) und vor dem aktuellen Datum liegen. Informationen zum Angeben des Versionsparameters finden Sie in den [API-Beispielen](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis).

## network_conflict
**Nachricht**: CIDR-Konflikte mit vorhandenem Adresspräfix in VPC.

Diese Nachricht wird möglicherweise angezeigt, wenn Sie eine Netz-CIDR angeben, die einen Konflikt mit einer vorhandenen Netz-CIDR in demselben IP-Bereich verursacht.

Führen Sie zum Suchen nach vorhandenen CIDRs in Adresspräfixen den API-Befehl `GET /v1/vpcs/<vpc-id>/address_prefixes` aus und überprüfen Sie die `cidr`-Werte der Antwort. Überprüfen  Sie den Wert von `cidr`, um sicherzustellen, dass Ihre Angaben für die CIDR nicht in anderen Adresspräfixen verwendet werden.

Wenn Sie die Befehlszeilenschnittstelle verwenden, führen Sie den Befehl `ibmcloud is vpc-addrs <vpc-id>` aus und überprüfen den Wert von `CIDR Block` auf Konflikte.

Führen Sie zum Suchen nach vorhandenen CIDRs in Teilnetzen den API-Befehl `GET /v1/subnets` aus, um alle Teilnetze für die VPC aufzulisten. Führen Sie anschließend den API-Befehl `GET /v1/subnets/<subnet-id>` für jedes Teilnetz in der VPC aus und überprüfen Sie den Wert von `ipv4_cidr_block` in der Antwort. Überprüfen Sie den Wert von `ipv4_cidr_block`, um sicherzustellen, dass Ihre CIDR-Angaben (CIDR - Classless InterDomain Routing) nicht in anderen Adresspräfixen verwendet werden.

Wenn Sie die Befehlszeilenschnittstelle (CLI) verwenden, führen Sie den Befehl `ibmcloud is subnets` aus, um alle Teilnetze für die VPC aufzulisten. Führen Sie anschließend den Befehl `ibmcloud is subnet<subnet-id>` für jedes Teilnetz in der VPC aus und überprüfen Sie, ob Konflikte mit dem Wert von `IPv4 CIDR` in der CLI-Ausgabe auftreten.

## not_authorized
**Nachricht**: Die Anforderung ist nicht autorisiert.

Möglicherweise wird dieser Fehler angezeigt, wenn das IAM-Token fehlt oder abgelaufen ist. Anweisungen zum Generieren eines Tokens finden Sie im Abschnitt [VPC mit den REST-APIs erstellen](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis). Wenn das Token nicht abgelaufen ist, stellen Sie sicher, dass das verwendete Konto zur Nutzung dieser Funktion berechtigt ist und dass Sie über die [korrekten Berechtigungen](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) verfügen.

Wenn Sie über die korrekte Berechtigung verfügen und der Fehler trotzdem weiterhin auftritt, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_found
**Nachricht**: Überprüfen Sie, ob die Ressource, die Sie anfordern, vorhanden ist.

Sie haben auf eine Ressource verwiesen, die nicht vorhanden ist, oder auf eine Ressource, auf die Sie keinen Zugriff haben. Überprüfen Sie Ihre Anforderung, um sicherzustellen, dass Sie die richtigen IDs und Verweise angegeben haben.

Weitere Hilfe finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implemented
**Nachricht**: Keine

Die Methode ist nicht implementiert. Überprüfen Sie die Anforderung, um sicherzustellen, dass Sie eine [gültige API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} aufrufen. [Wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support), wenn Sie erwarten, dass diese API implementiert werden soll. 

## not_in_address_prefix
**Nachricht**: Die angegebene CIDR passt in keine der Adresspräfixe in der bereitgestellten Zone.

Dieser Fehler tritt auf, wenn ein Benutzer versucht, ein Teilnetz mit einer CIDR zu erstellen, das nicht in die Adresspräfixe für die angegebene Zone fällt.

Führen Sie `GET /vpcs/{vpc_id}/address_prefixes` aus, um die Liste der Adresspräfixe für die VPC abzurufen. Wenn Sie die Befehlszeilenschnittstelle verwenden, können Sie `ibmcloud is vpc-address-prefixes` ausführen, um alle Adresspräfixe für die VPC aufzulisten. Überprüfen Sie die Werte für `cidr` und `zone` in der Antwort und stellen Sie sicher, dass der Wert für `cidr` des Teilnetzes eine Untergruppe von `cidr` des Adresspräfixes für die Zone ist, die Sie erstellen möchten.

## over_quota
**Nachricht**: Die Anforderung würde das Kontingent für einen Ressourcentyp überschreiten.

Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window} angegeben. 

## password_not_ready
**Nachricht**: Keine

Ihre Instanz muss aktiv sein, bevor Sie eine variable IP-Adresse zuordnen können. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
**Nachricht**: Der Pool kann nicht gelöscht werden, weil er immer noch einem oder mehreren Listenern zugeordnet ist.

Stellen Sie sicher, dass die Zuordnung zwischen dem Pool und den Listenern aufgehoben ist, und versuchen Sie es erneut. 

## pool_duplicate_name
**Nachricht**: Der Poolname `<pool_name>` wird bereits von einem anderen Pool verwendet. Wählen Sie einen anderen Namen aus.

Wählen Sie einen anderen Poolnamen aus.

## pool_invalid_name
**Nachricht**: Der Name <poolname> ist ungültig.

Geben Sie einen gültigen Poolnamen an. Ein gültiger Name für die Lastausgleichsfunktion beginnt mit einem Buchstaben gefolgt von Buchstaben, Ziffern und Bindestrichen. Die Länge des Namens darf 40 Zeichen nicht überschreiten. 

## pool_invalid_session_persistence
**Nachricht**: Der Persistenzwert für die Poolsitzung ist nicht gültig.

Geben Sie einen gültigen Persistenzwert für die Sitzung an. Der zulässige Wert ist `QUELLEN-IP`.

## pool_missing_algorithm
**Nachricht**: Der Lastausgleichsalgorithmus fehlt.

Der Lastausgleichsalgorithmus ist ein erforderliches Feld. Geben Sie den Lastausgleichsalgorithmus in der Anforderung an. Die zulässigen Werte sind `round_robin`, `weighted_round_robin` und `least_connections`.

## pool_missing_health_monitor
**Nachricht**: Der Diagnosemonitor für den Pool fehlt.

Der Diagnosemonitor für den Pool ist ein erforderliches Feld.

## pool_missing_members
**Nachricht**: Poolmitglieder fehlen.

Geben Sie Poolmitglieder an.

## pool_missing_name
**Nachricht**: Der Poolname fehlt.

Der Poolname ist ein erforderliches Feld.

## pool_missing_protocol
**Nachricht**: Das Poolprotokoll fehlt.

Das Poolprotokoll ist ein erforderliches Feld. Geben Sie das Poolprotokoll in der Anforderung an. Die zulässigen Werte sind `http` und `tcp`.

## pool_not_found
**Nachricht**: Der Pool mit der ID `<pool_id>` wurde nicht gefunden.

Geben Sie eine vorhandene Pool-ID an.

## pool_over_quota
**Nachricht**: Der Pool kann nicht erstellt werden. Das Kontingent von Pools für die Ressource der Lastausgleichsfunktion hat den Maximalwert erreicht.

Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window} angegeben. 

## public_gateway_in_use
**Nachricht**: Ein öffentliches Gateway kann nicht gelöscht werden, wenn es im Gebrauch ist.

Das öffentliche Gateway ist derzeit an ein oder mehrere Teilnetze angeschlossen. Sie müssen das öffentliche Gateway von allen Teilnetzen abhängen, bevor Sie es löschen können.

Wenn Sie feststellen möchten, von welchem Teilnetz das öffentliche Gateway verwendet wird, verwenden Sie die API `GET /v1/subnets?version=2019-05-31&generation=1`. Der funktional entsprechende Befehl in der CLI lautet `ibmcloud is subnets`. Wenn Sie die Zuordnung des öffentlichen Gateways zum Teilnetz aufheben möchten, verwenden Sie den API-Befehl `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` oder den CLI-Befehl `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
**Nachricht**: Zu viele Anforderungen innerhalb einer kurzen Zeit.

Diese Fehlernachricht wird zurückgegeben, wenn zu viele Anforderungen innerhalb eines angegebenen Zeitintervalls empfangen werden. Warten Sie eine Weile und versuchen Sie es erneut.

## security_group_active_transactions
**Nachricht**: Die Schnittstelle kann erst angehängt oder abgehängt werden, wenn die Instanz einen aktiven Status aufweist.

Die Instanz muss aktiv sein, bevor ihre Netzschnittstellen einer Sicherheitsgruppe zugeordnet werden können. Verwenden Sie `GET /v1/instances/{id}?version=2019-05-31&generation=1` oder `ibmcloud is instance`, um den Status der Instanz zu überprüfen. Sobald der Status `running` (aktiv) ist, versuchen Sie es erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
**Nachricht**: Die Schnittstelle ist bereits der Sicherheitsgruppe zugeordnet.

Die Schnittstelle ist bereits der Sicherheitsgruppe zugeordnet. Verwenden Sie `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` oder `ibmcloud is security-group-network-interfaces`, um die zugeordneten Schnittstellen anzuzeigen.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_exists
**Nachricht**: Die Sicherheitsgruppe ist bereits vorhanden.

Die Namen der Sicherheitsgruppen müssen innerhalb einer VPC eindeutig sein. Eine Sicherheitsgruppe mit diesem Namen ist in der Ziel-VPC bereits vorhanden. Verwenden Sie `GET /v1/security_groups?version=2019-05-31&generation=1` oder `ibmcloud is security-groups`, um die aktuellen Sicherheitsgruppen anzuzeigen. 

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic) oder dem [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_attached
**Nachricht**: Die Sicherheitsgruppe kann nicht gelöscht werden, während Schnittstellen zugeordnet sind.


Heben Sie die Zuordnung für alle Schnittstellen auf, bevor Sie die Sicherheitsgruppe löschen. Verwenden Sie `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` oder `ibmcloud is security-group-network-interface-remove`, um die Zuordnung einer Schnittstelle aufzuheben. 

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic) oder dem [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Nachricht**: Das Limit der Schnittstellen pro Sicherheitsgruppe wurde überschritten.

Das Zuordnen einer weiteren Schnittstelle zu der Sicherheitsgruppe würde das Limit der Schnittstellen pro Sicherheitsgruppe überschreiten. Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window} angegeben. Bewerten Sie Ihre Strategie und überlegen Sie, eine weitere Sicherheitsgruppe mit ähnlichen Regeln zu erstellen.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic) oder dem [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_last_security_group_is_default
**Nachricht**: Die Standardsicherheitsgruppe kann nicht entfernt werden, wenn sie die einzige zugeordnete Sicherheitsgruppe ist.

Eine Netzschnittstelle muss mindestens einer Sicherheitsgruppe zugeordnet sein.
Sie wird der Standardsicherheitsgruppe der VPC zugeordnet, wenn keine Sicherheitsgruppe angegeben ist.
Hängen Sie die Schnittstelle an eine andere Sicherheitsgruppe an und versuchen Sie erneut, sie von der Standardsicherheitsgruppe abzuhängen.
Informationen zur Standardsicherheitsgruppe finden Sie im [Dokument zur Verwendung von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
**Nachricht**: Das Limit für die Sicherheitsgruppe wurde überschritten.

Sie haben versucht, eine neue Sicherheitsgruppe zu erstellen, aber Sie bewegen sich derzeit im Rahmen Ihres Kontokontingents. Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window} angegeben. Bewerten Sie Ihre Strategie für die Zuordnung von Instanzen zu Sicherheitsgruppen. Es ist häufig möglich, die Gesamtzahl der Sicherheitsgruppen durch die Zuordnung mehrerer Instanzen zu derselben Sicherheitsgruppe zu reduzieren. Mit dieser Strategie wird die Anzahl der Sicherheitsgruppen reduziert und Sie bleiben im Rahmen des Kontingents für Ihr Konto. In einigen wenigen Fällen, in der Regel für große Organisationen, besteht die Notwendigkeit, das Kontingent zu erweitern. In diesem Fall [wenden Sie sich bitte an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support), um zu erfragen, ob dies möglich ist.

## security_group_network_interface_not_active
**Nachricht**: Die Schnittstelle kann nicht zugeordnet werden, da sie nicht aktiv ist.

Netzschnittstellen müssen aktiv sein, bevor sie mit Sicherheitsgruppen verbunden werden. Verwenden Sie `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` oder `ibmcloud is instance-network-interface`, um den Status einer Schnittstelle anzuzeigen. Sobald der Status 'available' (verfügbar) lautet, versuchen Sie es erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
**Nachricht**: Die Schnittstelle ist nicht zugeordnet.

Die Schnittstelle ist der Sicherheitsgruppe nicht zugeordnet. Verwenden Sie `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` oder `ibmcloud is security-group-network-interfaces`, um die zugeordneten Schnittstellen anzuzeigen.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic) oder dem [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_not_in_vpc
**Nachricht**: Die Schnittstelle und die Sicherheitsgruppe befinden sich in verschiedenen VPCs.

Wenn eine Netzschnittstelle einer Sicherheitsgruppe zugeordnet werden soll, muss sich die Instanz in derselben VPC wie die Sicherheitsgruppe befinden. Verwenden Sie `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` oder `ibmcloud is security-group`, um die Sicherheitsgruppendetails und die VPC anzuzeigen. Verwenden Sie `GET /v1/instances/{id}?version=2019-05-31&generation=1` oder `ibmcloud is instance`, um die Instanzdetails und die VPC anzuzeigen.

## security_group_order_bindings
**Nachricht**: Die Sicherheitsgruppe kann nicht gelöscht werden, solange noch Aufträge für Instanzen vorhanden sind.

Wenn eine Instanz erstellt wurde und Sicherheitsgruppen für die Netzschnittstelle angegeben sind, müssen die Instanz und Netzschnittstellen aktiv sein, bevor die Sicherheitsgruppe gelöscht werden kann. Verwenden Sie `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` oder `ibmcloud is security-group-network-interfaces`, um die zugeordneten Netzschnittstellen und ihren Status anzuzeigen. Sobald der Status der Schnittstellen 'available' (verfügbar) lautet, versuchen Sie es erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Nachricht**: Der maximale Wert für den TCP/UDP-Port kann nicht kleiner sein als der Mindestwert für den Port.

Der maximale Wert für den Port kann nicht kleiner sein als der Mindestwert für den Port. Geben Sie einen maximalen Portwert an, der größer als der minimale Portwert ist.

## security_group_port_range_both_or_neither
**Nachricht**: Der Portbereich darf nicht festgelegt sein oder es müssen sowohl der minimale und maximale Portbereich für 'tcp' und 'udp' festgelegt sein.

Regeln mit dem 'tcp'- oder 'udp'-Protokoll weisen möglicherweise einen nicht festgelegten Portbereich auf (um die Regel auf den gesamten Datenverkehr anzuwenden) oder sie können einen Portbereich angeben. Um den Datenverkehr auf einen einzelnen Port zu beschränken, setzen Sie sowohl den minimalen als auch den maximalen Port auf den Portwert. Der folgende Befehl würde z. B. eine Regel erstellen, um SSH an Port 22 zu ermöglichen: `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

Weitere Informationen zu den Konfigurationen der Sicherheitsgruppenregeln finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic).


## security_group_port_range_invalid_protocol
**Nachricht**: Es wurde ein Portbereich mit einem Protokoll 'icmp' angegeben. Ein Portbereich ist nur für ein Protokoll des Typs 'tcp' oder 'udp' gültig.

Regeln mit einem Protokoll des Typ 'icmp' haben einen Typ und Code. Regeln mit den Protokollen 'tcp' oder 'udp' weisen einen Portbereich auf. Weitere Informationen zu den Konfigurationen der Sicherheitsgruppenregeln finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_remote_group_not_in_vpc
**Nachricht**: Die ferne Gruppe befindet sich nicht in derselben VPC wie diese Sicherheitsgruppe.

Sicherheitsgruppenregeln können sich auf eine ferne Sicherheitsgruppe beziehen, die den Datenaustausch zwischen verbundenen Schnittstellen der beiden Gruppen ermöglicht. Ferne Sicherheitsgruppen müssen sich in derselben VPC befinden. Verwenden Sie `GET /v1/security_groups?2019-01-01` oder `ibmcloud is security-groups`, um die aktuellen Sicherheitsgruppen und deren VPCs anzuzeigen.

Weitere Anweisungen zum Beheben dieses Problems finden Sie im [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Nachricht**: Die Sicherheitsgruppe kann nicht gelöscht werden, während die Regeln für das Remoting angehängt sind.

Die Sicherheitsgruppe enthält eine oder mehrere Regeln, die auf eine ferne Sicherheitsgruppe verweisen. Verwenden Sie `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` oder `ibmcloud is security-group-rules`, um die Regeln anzuzeigen. Im Feld 'remote' werden alle fernen Sicherheitsgruppen angegeben. Versuchen Sie es nach dem Löschen oder Bearbeiten der Regel für das Remoting erneut.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic) oder dem [Dokument zum Verwenden von Sicherheitsgruppen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

## security_group_remoting_rules_per_sg_exceeded
**Nachricht**: Das Limit der Regeln für das Remoting pro Sicherheitsgruppe wurde überschritten.

Das Hinzufügen einer weiteren Regel, die sich auf eine ferne Sicherheitsgruppe bezieht, würde das Limit der Regeln für das Remoting pro Sicherheitsgruppe überschreiten. Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window} angegeben. 

## security_group_rules_per_sg_exceeded
**Nachricht**: Das Limit der Regeln pro Sicherheitsgruppe wurde überschritten.

Das Hinzufügen einer Regel würde das Limit der Regeln pro Sicherheitsgruppe überschreiten. Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window} angegeben. Evaluieren Sie Ihre Strategie und ziehen Sie in Betracht, eine weitere Sicherheitsgruppe zu erstellen.

## security_group_sgs_per_interface_exceeded
**Nachricht**: Das Limit der Sicherheitsgruppen pro Schnittstelle wurde überschritten.

Die Zuordnung dieser Schnittstelle zu einer anderen Sicherheitsgruppe würde das Limit der Sicherheitsgruppen pro Schnittstelle überschreiten. Die Kontingente pro Ressource sind im Abschnitt [Kontingente und Limits für VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window} angegeben. Evaluieren Sie Ihre Strategie und ziehen Sie in Betracht, Regeln zu einer vorhandenen Sicherheitsgruppe hinzuzufügen.


## security_group_type_code_invalid_protocol
**Nachricht**: Es wurde der Typ/Code 'icmp' angegeben, das angeforderte Protokoll war jedoch nicht 'icmp'. Setzen Sie das Protokoll auf 'icmp' oder geben Sie einen Portbereich an.

Nur Regeln mit dem 'icmp'-Protokoll haben einen Typ und einen Code. Regeln mit den Protokollen 'tcp' oder 'udp' weisen einen Portbereich auf. Weitere Informationen zu den Konfigurationen der Sicherheitsgruppenregeln finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_vpc_default
**Nachricht**: Die Standardsicherheitsgruppe für eine VPC kann nicht gelöscht werden.

Die Standardsicherheitsgruppe kann nicht gelöscht werden. Informationen zur Standardsicherheitsgruppe finden Sie im Leitfaden zum [Aktualisieren der Standardsicherheitsgruppe](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
**Nachricht**: Keine

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
**Nachricht**: Die CIDR steht im Konflikt mit dem vorhandenen Teilnetz in VPC.

Führen Sie die API `GET /subnets` aus, um alle Teilnetze in VPC abzurufen. Überprüfen Sie den Wert von `ipv4_cidr_block`, um sicherzustellen, dass Ihre CIDR-Angaben (CIDR - Classless InterDomain Routing) nicht in anderen Teilnetzen verwendet werden.

Wenn Sie die CLI verwenden, können Sie den Befehl `ibmcloud is subnets` ausführen und den Wert für "Teilnetz-CIDR" auf mögliche Konflikte überprüfen.

Weitere Hilfe finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
**Nachricht**: Das Teilnetz kann erst gelöscht werden, wenn es leer ist. Löschen Sie alle Ressourcen im Teilnetz und versuchen Sie es erneut. 

Es gab eine Anforderung zum Löschen des Teilnetzes, dieses enthält jedoch noch Ressourcen. Teilnetze können Instanzen, VPNs, Lastausgleichsfunktionen oder öffentliche Gateways enthalten. Sie müssen Ihre Ressourcen in dem Teilnetz löschen, damit Sie das Teilnetz löschen können.

In einigen Situationen kann dieser Fehler auch auftreten, wenn in der Konsole 0 virtuelle Serverinstanzen und 0 Lastausgleichsfunktionen angezeigt werden. Das Löschen verläuft asynchron und es kann einige Minuten dauern, bis der interne Status geändert wird. Wiederholen Sie die Teilnetzlöschung in ein paar Minuten.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Nachricht**: Das Teilnetz kann nicht gelöscht werden, wenn es an ein öffentliches Gateway angeschlossen ist. Heben Sie die Zuordnung des öffentlichen Gateways auf und versuchen Sie es erneut.

Es gab eine Anforderung zum Löschen eines Teilnetzes, diesem ist jedoch noch ein öffentliches Gateway zugeordnet. Sie müssen das öffentliche Gateway löschen oder die Zuordnung aufheben, bevor Sie das Teilnetz löschen können.

Wenn Sie die CLI verwenden, können Sie den Befehl `ibmcloud is public-gateways` zum Auflisten der öffentlichen Gateways und den Befehl `ibmcloud is subnet-public-gateway-detach` zum Abhängen eines öffentlichen Gateways von einem Teilnetz ausführen.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Nachricht**: Das Teilnetz kann nicht gelöscht werden, während es IP-Adressen enthält. Löschen Sie eine beliebige Serverinstanz, die der IP-Adresse zugeordnet ist, und wiederholen Sie den Vorgang.

Es gab eine Anforderung zum Löschen eines Teilnetzes, dieses enthält jedoch noch IP-Adressen. Sie müssen die Serverinstanz löschen, die der IP-Adresse zugeordnet ist, bevor Sie das Teilnetz löschen können.

Wenn Sie die Befehlszeilenschnittstelle verwenden, können Sie `ibmcloud is instances` ausführen, um die Serverinstanzen aufzulisten. Überprüfen Sie den Wert für 'Address' zur Ermittlung der IP-Adresse und den Wert für 'Status' zur Ermittlung des Status. Führen Sie den Befehl `ibmcloud is instance-delete` aus, um eine Serverinstanz zu löschen, die noch nicht den Status 'deleting' (wird gelöscht) aufweist.

In einigen Situationen kann dieser Fehler auch auftreten, wenn in der Konsole 0 virtuelle Serverinstanzen angezeigt werden. Das Löschen verläuft asynchron und es kann einige Minuten dauern, bis der interne Status geändert wird. Wiederholen Sie die Teilnetzlöschung in ein paar Minuten.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Nachricht**: Das Teilnetz kann nicht gelöscht werden, solange es eine Lastausgleichsfunktion enthält. Löschen Sie die Lastausgleichsfunktion und versuchen Sie es erneut.

Es gab eine Anforderung zum Löschen eines Teilnetzes, dieses enthält jedoch noch eine Lastausgleichsfunktion. Sie müssen die Lastausgleichsfunktion löschen, bevor Sie das Teilnetz löschen können.

Wenn Sie die Befehlszeilenschnittstelle verwenden, können Sie `ibmcloud is load-balancer` ausführen, um die Lastausgleichsfunktionen aufzulisten. Überprüfen Sie den Wert für 'Subnets' zur Ermittlung des Teilnetzes und den Wert für 'Status' zur Ermittlung des Status. Führen Sie den Befehl `ibmcloud is load-balancer-delete` aus, um eine Lastausgleichsfunktion zu löschen, die noch nicht den Status 'deleting' (wird gelöscht) aufweist.

In einigen Situationen kann dieser Fehler auch auftreten, wenn in der Konsole 0 Lastausgleichsfunktionen angezeigt werden. Das Löschen verläuft asynchron und es kann einige Minuten dauern, bis der interne Status geändert wird. Wiederholen Sie die Teilnetzlöschung in ein paar Minuten.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Nachricht**: Das Teilnetz kann nicht gelöscht werden, solange es ein VPN-Gateway enthält. Löschen Sie das VPN-Gateway und versuchen Sie es erneut.

Es gab eine Anforderung zum Löschen eines Teilnetzes, diesem ist jedoch noch ein VPN-Gateway zugeordnet. Sie müssen das VPN-Gateway löschen, bevor Sie das Teilnetz löschen können.

Wenn Sie die Befehlszeilenschnittstelle verwenden, können Sie `ibmcloud is vpn-gateways` ausführen, um die VPN-Gateways aufzulisten. Überprüfen Sie den Wert für 'Subnets' zur Ermittlung des Teilnetzes und den Wert für 'Status' zur Ermittlung des Status. Führen Sie den Befehl `ibmcloud is vpn-gateway-delete` aus, um ein VPN-Gateway zu löschen, das noch nicht den Status 'deleting' (wird gelöscht) aufweist.

In einigen Situationen kann dieser Fehler auch auftreten, wenn in der Konsole 0 VPN-Gateways angezeigt werden. Das Löschen verläuft asynchron und es kann einige Minuten dauern, bis der interne Status geändert wird. Wiederholen Sie die Teilnetzlöschung in ein paar Minuten.

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_unknown_state
**Nachricht**: Das Teilnetz `<subnet_id>` befindet sich für die angeforderte Operation in einem ungültigen Status.

Das Teilnetz muss den Status `available` aufweisen, damit Sie es betreiben können. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
**Nachricht**: Das Ziel verfügt bereits über eine variable IP-Adresse.

Es wurde eine Anforderung zum Zuordnen einer variablen IP-Adresse zur Netzschnittstelle eines Servers erstellt, der Netzschnittstelle war aber bereits eine variable IP-Adresse zugeordnet. 

Wenn Sie feststellen möchten, welche variable IP-Adresse der Netzschnittstelle derzeit zugeordnet ist, führen Sie den API-Befehl `GET /v1/floating_ips?version=2019-05-31&generation=1` aus und überprüfen im Feld `target.id` die ID der Netzschnittstelle.   

Wenn Sie die Befehlszeilenschnittstelle verwenden, führen Sie den Befehl `ibmcloud is floating-ips` aus und überprüfen den Wert für `target`. Beachten Sie hierbei, dass die Netzschnittstellen-ID von der Befehlszeilenschnittstelle beim ersten Gedankenstrich (-) abgeschnitten wird. Beispiel: Für die primäre Netzschnittstelle eines Servers mit der ID `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` wird in der Ausgabe der Befehlszeilenschnittstelle `primary(abdfcb29-.)` angezeigt. 

## token_invalid
**Nachricht**: Das Service-Token war abgelaufen oder ungültig.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
**Nachricht**: Das Service-Token war leer oder nicht in der Anforderung vorhanden.

Erstellen Sie ein Token erneut und wiederholen Sie den Vorgang.

## validation_enum
**Nachricht**: Der angegebene Wert war keine gültige Option.

Eines der angegebenen Felder hat einen Wert, der aus einer Aufzählung möglicher Werte stammen muss.

Für ACL-Regeln für Netze können Sie eine Richtung angeben:

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

Der folgende Beispiel wäre beispielsweise ungültig, da `northbound` keine gültige Option in der Aufzählung `[ inbound, outbound ]` ist.

```json
{
    "direction":"northbound"
}
```

Informationen zu den gültigen Werten finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} oder in der [CLI-Referenz](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}.

## validation_failure
**Nachricht**: Die angegebene JSON stimmt nicht mit der erwarteten Struktur überein.

Um dieses Problem zu beheben, stellen Sie sicher, dass der Inhalt Ihrer Anforderung eine gültige JSON ist und dass Ihre Anforderung dem entspricht, was in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} angegeben ist.

## validation_invalid_cidr
**Nachricht**: Der Wert ist keine gültige CIDR.

Der Wert muss ein gültiger interner CIDR-Block mit dem Host-Bit '0' (Hostteil) sein.

Bestimmte IP-Adressbereiche sind reserviert. Weitere Informationen zu den reservierten IP-Bereichen finden Sie in der Übersicht zur [Verwendung der VPC mit Regionen und Teilnetzen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Nachricht**: Der Wert ist kein gültiges IPv4-CIDR.

Der Wert muss ein gültiger interner IPv4-CIDR-Block mit dem Host-Bit '0' (Hostteil) sein.

Bestimmte IP-Adressbereiche sind reserviert. Weitere Informationen zu den reservierten IP-Bereichen finden Sie in der Übersicht zur [Verwendung der VPC mit Regionen und Teilnetzen](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Nachricht**: Der Wert ist kein gültiges IPv6-CIDR.

Derzeit wird IPv6 nicht unterstützt. Verwenden Sie eine IPv4-Adresse.

## validation_invalid_address
**Nachricht**: Der Wert ist keine gültige Adresse.

Muss eine gültige IP-Adresse sein.

Eine Liste der einzeln reservierten IP-Adressen ist im Dokument [Regionen und Teilnetze](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) angegeben.

## validation_invalid_ipv4_address
**Nachricht**: Der Wert ist keine gültige IPv4-Adresse.

Geben Sie eine gültige IPv4-Adresse an.

## validation_invalid_ipv6_address
**Nachricht**: Der Wert ist keine gültige IPv6-Adresse.

Geben Sie eine gültige IPv6-Adresse an. Derzeit wird IPv6 nicht unterstützt; verwenden Sie eine IPv4-Adresse.

## validation_invalid_field_type
**Nachricht**: Der Werttyp stimmt nicht mit dem Feldtyp überein.

Um dieses Problem zu beheben, stellen Sie sicher, dass der Inhalt Ihrer Anforderung dem entspricht, was in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} zu dem Endpunkt angegeben ist, den Sie aufrufen.

## validation_max_value
**Nachricht**: Der für einen Parameter angegebene Wert überschreitet die zulässige Obergrenze.

Geben Sie einen niedrigeren Wert an, der die in der [Spezifikation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} angegebene Obergrenze nicht überschreitet. 

## validation_min_value
**Nachricht**: Der für einen Parameter angegebene Wert unterschreitet die zulässige Untergrenze.

Dieser Fehler kann auftreten, wenn Sie versuchen, ein Teilnetz mit einem Wert für `total_ipv4_address_count` zu erstellen, der unter 8 liegt.

Geben Sie einen höheren Wert an, der die in der [Spezifikation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} angegebene Untergrenze nicht unterschreitet. 

## validation_not_null
**Nachricht**: Das angegebene Feld muss null sein.

Stellen Sie sicher, dass der Wert null ist. Informationen hierzu finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. 

Es folgt ein ungültiges Beispiel:

```json
{
    "name": null
}
```

## validation_only_one
**Nachricht**: Der Wert muss einem der Unterschemas entsprechen.

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt.

## validation_discriminator_forbidden
**Nachricht**: Das Diskriminatorfeld verbietet diese Unterstruktur.

Wenn Sie z. B. Folgendes angeben:
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

Das Protokoll ist `icmp` und _port_min_ ist ein `tcp`-Feld, sodass Sie einen Fehler erhalten.
1. validation_discriminator_required für fehlende `icmp`-Regeln
2. validation_discriminator_forbidden für `tcp`-Felder mit angegebenem Wert `icmp`

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
**Nachricht**: Keine

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
**Nachricht**: Der Wert ist kein gültiger Name.

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
**Nachricht**: Der Wert ist kein gültiges HREF-Attribut.

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
**Nachricht**: Keine

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
**Nachricht**: Es fehlt ein erforderliches Feld.

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
**Nachricht**: Das angegebene Feld muss eindeutig sein.

Möglicherweise haben Sie einen Namen angegeben, der bereits verwendet wird. Versuchen Sie es mit einem anderen Wert.

Stellen Sie sicher, dass die Anforderung die Vorgaben in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window} erfüllt. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Nachricht**: Die Datenträgerzuordnung kann nicht gelöscht werden.

Diese Operation ist nicht zulässig. Die Datenträgerzuordnung des Bootdatenträgers kann nicht gelöscht werden, weil der Bootdatenträger für die Instanz erforderlich ist. 

## volume_attachment_update_invalid_request
**Nachricht**: Die Datenträgerzuordnung konnte nicht aktualisiert werden, weil die Anforderung ungültig ist.

Die Datenträgerzuordnung konnte aus einem der folgenden Gründe nicht aktualisiert werden:

* Die Datenträgerzuordnung des Bootdatenträgers kann nicht umbenannt werden.
* Für das Feld `delete_volume_on_instance_delete` der Datenträgerzuordnung für den Bootdatenträger kann nicht der Wert `true` festgelegt werden.

## volume_capacity_max
**Nachricht**: Die maximal zulässige Datenträgerkapazität beträgt 2000 GB.

Die angegebene Datenträgerkapazität überschreitet die maximale Datenträgerkapazität. Geben Sie einen Wert zwischen 10 und 2000 GB an. Informationen zu den zulässigen Kapazitätsbereichen für ein angepasstes Profil finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

## volume_capacity_missing
**Nachricht**: In der Anforderung fehlt der erforderliche Wert für die Datenträgerkapazität.

Wenn Sie einen Datenträger erstellen, müssen Sie basierend auf dieser Profildefinition eine Datenträgerkapazität angeben. Weitere Informationen finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_capacity_zero_or_negative
**Nachricht**: Die Datenträgerkapazität muss größer als null sein.

Bei der Erstellung eines Datenträgers muss der in der Anforderung angegebene Kapazitätswert eine positive Zahl zwischen 10 GB und 2000 GB sein. Informationen zu den unterstützten Werten für die Datenträgerkapazität finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_crn_account_id_mismatch
**Nachricht**: Der in der Anforderung angegebene CRN gehört nicht zu dem Konto des aktuellen Benutzers.

Der Cloudressourcenname (CRN) für den Key Protect-Stammschlüssel stimmt nicht mit der IAM-Berechtigungskonto-ID überein. Geben Sie einen anderen Cloudressourcennamen für den Key Protect-Stammschlüssel für das IAM-Konto an. Weitere Informationen hierzu finden Sie in der Dokumentation zu [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial).

## volume_crn_cname_mismatch
**Nachricht**: Der in der Anforderung angegebene CRN kann nicht für die aktuelle Umgebung verwendet werden.

Der in der Anforderung angegebene CRN kann nicht in der aktuellen Umgebung verwendet werden. Geben Sie einen CRN an, der in der Umgebung verwendet werden kann.

## volume_encryption_key_crn_invalid
**Nachricht**: Der in der Anforderung angegebene CRN für den Verschlüsselungsschlüssel des Datenträgers ist nicht gültig.

Der angegebene Key Protect-Cloudressourcenname (CRN) ist nicht gültig. Rufen Sie den CRN des Stammschlüssels im Cloud Identity and Access Management-Service ab. Weitere Informationen finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption).

## volume_encryption_key_not_active
**Nachricht**: Der in der Anforderung angegebene Verschlüsselungsschlüssel des Datenträgers ist nicht aktiv.

Aktivieren Sie den vorhandenen Schlüssel oder geben Sie einen neuen Schlüssel an. Wenn Sie keinen eigenen Schlüssel aktivieren können, wenden Sie sich an Ihren Systemadministrator oder an die [Kundenunterstützung](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
**Nachricht**: Sie sind nicht berechtigt, auf den angegebenen Verschlüsselungsschlüssel des Datenträgers zuzugreifen.

Geben Sie einen anderen Verschlüsselungsschlüssel an, auf den Sie zugreifen können, oder wenden Sie sich an Ihren Systemadministrator, um Zugriff auf den aktuellen Schlüssel zu erhalten.

## volume_encryption_key_not_implemented
Die Funktion für den Verschlüsselungsschlüssel des Datenträgers wird nicht unterstützt.

Mit dem Verschlüsselungsschlüssel konnte kein Datenträger erstellt werden. Von dieser Version werden nur Datenträger mit einer vom Provider verwalteten Verschlüsselung unterstützt. Wenn Sie einen eigenen Verschlüsselungsschlüssel verwenden möchten, verwenden Sie eine Version von Block Storage for VPC, von der eine angepasste und vom Kunden verwaltete Verschlüsselung unterstützt wird. Weitere Informationen erhalten Sie von der [Kundenunterstützung](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_id_invalid
**Nachricht**: Die im Anforderungsparameter angegebene Datenträger-ID ist nicht gültig.

Die angegebene Datenträger-ID weist nicht das korrekte Format auf. Stellen Sie sicher, dass Sie die Datenträger-ID korrekt eingegeben haben, und versuchen Sie es erneut. Wenn Sie nicht sicher sind, ob die Datenträger-ID korrekt ist, geben Sie `ibmcloud is volumes` in der Befehlszeilenschnittstelle oder `GET volumes` in der API an, und durchsuchen die Liste der Datenträger.

## volume_id_missing
**Nachricht**: Die Datenträger-ID fehlt im Anforderungsparameter.

Sie müssen die Datenträger-ID im Anforderungsparameter angeben, wenn ein bestimmter Datenträger abgerufen wird.

## volume_id_not_found
**Nachricht**: Datenträger wurde nicht gefunden. Die Datenträger-ID `<volume_id>` ist nicht vorhanden; hierbei steht `<volume_id>` für die angegebene Datenträger-ID. 

Die angegebene Datenträger-ID ist nicht vorhanden. Geben Sie eine gültige Datenträger-ID an.

## volume_iops_zero_or_negative
**Nachricht**: Der Wert für die Datenträger-IOPS (IOPS = E/A-Operationen pro Sekunde) muss größer als null sein.

Bei der Erstellung eines Datenträgers muss der in der Anforderung angegebene IOPS-Wert eine positive Zahl sein. Informationen zu den unterstützten IOPS-Werten finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_name_duplicate
**Nachricht**: Der Datenträgername ist mehrfach vorhanden. Der Datenträgername `<volume_name>`, der in der Anforderung angegeben wurde, ist bereits vorhanden; hierbei steht `<volume_name>` für den angegebenen Datenträgernamen.

Der in der Anforderung angegebene Datenträgername ist bereits vorhanden. Geben Sie einen Datenträgernamen an, der derzeit nicht verwendet wird.

## volume_not_available
**Nachricht**: Der Datenträger ist nicht verfügbar. Ein Datenträger kann nur im verfügbaren Status geändert werden. Der Status des aktuellen Datenträgers `<volume_id>` lautet `<volume_status>`; hierbei steht `<volume_id>` für die in der Anforderung angegebene Datenträger-ID und `<volume_status>` für den aktuellen Datenträgerstatus.

Ein Datenträger kann nur geändert werden, wenn er sich im Status `available` befindet. Stellen Sie sicher, dass der Datenträger verfügbar ist, bevor Sie ihn ändern.

## volume_not_deletable
**Nachricht**: Der Löschvorgang ist fehlgeschlagen.

Der Datenträger kann nur gelöscht werden, wenn er sich im Status `available` oder `failed` befindet. Stellen Sie sicher, dass sich der Datenträger in einem gültigen Status befindet, in dem er gelöscht werden kann.

## volume_not_found
**Nachricht**: Ein Datenträger mit der angegebenen ID kann nicht gefunden werden.

Stellen Sie sicher, dass die eingegebene Datenträger-ID korrekt ist, und versuchen Sie es erneut. Geben Sie zum Abrufen der Datenträgerliste in der Befehlszeilenschnittstelle `ibmcloud is volumes` oder in der API `GET volumes` ein.

## volume_not_implemented
**Nachricht**: Die angeforderte Operation wird in diesem Release nicht unterstützt.

Weitere Informationen erhalten Sie von der [Kundenunterstützung](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_profile_capacity_iops_invalid
**Nachricht**: Das in der Anforderung angegebene Datenträgerprofil ist für den angegebenen Kapazitätswert und/oder IOPS-Wert nicht gültig.

Der in der Anforderung angegebene Datenträgerkapazitätswert und/oder IOPS-Wert ist für das Datenträgerprofil nicht zulässig. Informationen zu den gültigen Werten für die minimale und maximale Kapazität und den gültigen IOPS-Werten für ein angepasstes Profil finden Sie in der Dokumentation zu [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_profile_iops_invalid
**Nachricht**: Vom in der Anforderung angegebenen Datenträgerprofil werden keine angepassten IOPS (E/A-Operationen pro Sekunde) unterstützt.

Für das Datenträgerprofil sind angepasste IOPS-Werte nicht zulässig. Möglicherweise haben Sie ein Profil für gestaffelte IOPS angegeben, für das das Angeben eines IOPS-Werts nicht erforderlich ist. Wenn Sie einen bestimmten IOPS-Wert angeben möchten, verwenden Sie das angepasste IOPS-Profil.

## volume_profile_name_missing
**Nachricht**: In der Anforderung fehlt der erforderliche Profilname für den Datenträger.

Der Datenträgerprofilname wurde beim Erstellen des Datenträgers im Anforderungshauptteil oder beim Abrufen eines Datenträgers im Anforderungsparameter nicht angegeben. Geben Sie einen Datenträgerprofilnamen in der Anforderung an und versuchen Sie es erneut. Wenn Sie den Datenträgerprofilnamen nicht kennen, geben Sie `ibmcloud is volume-profiles` in der Befehlszeilenschnittstelle an oder verwenden Sie `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` in der API-Anforderung; durchsuchen Sie danach die Liste. 

## volume_profile_not_found
**Nachricht**: Ein Datenträgerprofil mit dem angegebenen Namen wurde nicht gefunden.

Ein Datenträgerprofil mit diesem Namen konnte nicht gefunden werden. Stellen Sie sicher, dass Sie den korrekten Datenträgerprofilnamen angegeben haben. Wenn Sie den Datenträgerprofilnamen nicht kennen, geben Sie `ibmcloud is volume-profiles` in der Befehlszeilenschnittstelle an oder verwenden Sie `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` in der API-Anforderung; durchsuchen Sie danach die Liste. 

## volume_quota_reached
**Nachricht**: Das Datenträgerkontingent wurde erreicht.

Das Kontingent für das aktuelle Konto is ausgeschöpft. Versuchen Sie, einige Datenträger zu löschen, oder wenden Sie sich an die [Kundenunterstützung](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support), um Ihr Kontingent zu erhöhen. Informationen hierzu finden Sie in der Dokumentation unter [Kontingente für die Virtual Private Cloud-Infrastruktur](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
**Nachricht**: Die in der Anforderung angegebene Ressourcengruppen-ID ist nicht gültig.

Die in der Anforderung angegebene Ressourcengruppen-ID ist nicht gültig. Stellen Sie sicher, dass die Ressourcengruppen-ID korrekt ist. Verwenden Sie in der Befehlszeilenschnittstelle den Befehl `ibmcloud is volume VOLUME_ID`. Aus den daraus resultierenden Informationen gehen die Ressourcengruppe und die Ressourcengruppen-ID hervor. Weitere Informationen finden Sie unter [IBM Cloud CLI for VPC Reference](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_resource_group_not_authorized
**Nachricht**: Zur Ausführung der aktuellen Aktion für die angegebene Ressourcengruppe ist keine Berechtigung vorhanden.

Sie sind nicht berechtigt, Datenträger in der angegebenen Ressourcengruppe aufzulisten oder zu erstellen. Verwenden Sie eine andere Ressourcengruppe, für die Sie über eine Berechtigung verfügen, oder fordern Sie vom Administrator die Berechtigungen zum Auflisten und Erstellen für die Ressourcengruppe an.

## volume_resource_group_not_found
**Nachricht**: Eine Ressourcengruppe mit der angegebenen ID konnte nicht gefunden werden.

Die Ressourcengruppen-ID konnte nicht gefunden werden, weil sie möglicherweise falsch eingegeben wurde oder nicht vorhanden ist. Stellen Sie sicher, dass die Ressourcengruppen-ID korrekt ist. Verwenden Sie in der Befehlszeilenschnittstelle den Befehl `ibmcloud is volume VOLUME_ID`. Aus den daraus resultierenden Informationen gehen die Ressourcengruppe und die Ressourcengruppen-ID hervor. Weitere Informationen finden Sie unter [IBM Cloud CLI for VPC Reference](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_start_not_found
**Nachricht**: Ein Datenträger mit der ID, die als Seitenstartparameter angegeben wurde, wurde nicht gefunden.

Die Datenträger-ID, die im Startparameter des Abrufs zum Auflisten der Datenträger angegeben wurde, konnte nicht gefunden werden. Stellen Sie sicher, dass die Datenträger-ID korrekt ist und dass Sie über Zugriff auf den Datenträger verfügen. 

## volume_status_not_available
**Nachricht**: Der Datenträger mit der angegebenen ID ist nicht verfügbar.

Der Datenträger befindet sich nicht im Status 'available'; er kann sich in einem Wartestatus (pending) befinden. Warten Sie, bis der Datenträger wieder verfügbar ist, und versuchen Sie es danach erneut. Wenn der Datenträger gelöscht wird, verwenden Sie einen anderen Datenträger. Geben Sie zum Überprüfen des Status des Datenträgers `ibmcloud is volume VOLUME_ID` in der Befehlszeilenschnittstelle oder `GET $rias_endpoint/v1/volumes/ $volume_id?version=YYYY-MM-DD` in der API-Anforderung an.

## volume_still_attached
**Nachricht**: Der Datenträger ist noch einer Instanz zugeordnet.

Ein Datenträger, der noch einer virtuellen Serverinstanz (Virtual Server Instance, VSI) zugeordnet ist, kann nicht gelöscht werden. Wenn Sie für den Datenträger das automatische Löschen festlegen, warten Sie, bis die VSI gelöscht ist; danach wird die Zuordnung des Datenträgers aufgehoben und der Datenträger wird gelöscht. Wenn Sie das automatische Löschen nicht festgelegt haben, heben Sie die Zuordnung des Datenträger manuell auf.

## volume_template_invalid
**Nachricht**: Es wurde eine ungültige Datenträgervorlage angegeben.

Sie haben eine ungültige Datenträgervorlage für die Erstellung eines Datenträgers angegeben. Wenn Sie überprüfen möchten, ob Sie im Anforderungshauptteil die korrekten Parameter angegeben haben, finden Sie Informationen hierzu unter [API-Referenz für VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## volume_update_invalid_request
**Nachricht**: Die Aktualisierungsanforderung für den Datenträger ist nicht gültig.

Die im Hauptteil der Aktualisierungsanforderung angegebene Eigenschaft ist nicht gültig. Überprüfen Sie die Informationen in [API-Referenz für VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} und korrigieren Sie die Syntax der API-Anforderung.

## vpc_not_empty
**Nachricht**: Die VPC kann nicht gelöscht werden, da sie nicht leer ist.

Alle Ressourcen in der VPC müssen gelöscht werden, damit die VPC gelöscht werden kann. Eine VPC enthält Instanzen, Teilnetze, öffentliche Gateways, Lastausgleichsfunktionen und VPN-Gateways; einige dieser Ressourcen enthalten ebenfalls Ressourcen. Details hierzu finden Sie in der Dokumentation zur [Vorgehensweise zum Löschen einer VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
**Nachricht**: Die Ressourcen befinden sich in verschiedenen VPCs.

Die Ressourcen müssen sich in derselben VPC befinden. Wenn Sie beispielsweise versuchen, ein öffentliches Gateway einem Teilnetz zuzuordnen, muss sich das öffentliche Gateway in derselben VPC wie das Teilnetz befinden.

Um festzustellen, welche VPC das öffentliche Gateway enthält, führen Sie den Befehl `GET /public_gateways/{id}` aus und überprüfen den Wert für 'vpc'. Der funktional entsprechende CLI-Befehl lautet `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
**Nachricht**: Für CIDR-Blöcke des Typs `<cidr_type>` sind die doppelten CIDR-Blöcke `<cidr_blocks>` vorhanden.

Beim Erstellen der Verbindung wurden doppelte CIDR-Blöcke angegeben. Stellen Sie sicher, dass die CIDR-Blöcke eindeutig sind.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt das Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_created
**Nachricht**: Ein CIDR-Block konnte der VPN-Verbindung `<vpn_connection_id>` nicht hinzugefügt werden.

Geben Sie eine gültige CIDR (Classless InterDomain Routing) an, die die in der Spezifikation angegebenen Anforderungen erfüllt.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Nachricht**: Ein CIDR-Block konnte nicht aus der VPN-Verbindung `<vpn_connection_id>` gelöscht werden.

Geben Sie eine gültige CIDR an, die in der Verbindung vorhanden ist. Eine Verbindung muss über mindestens eine lokale und eine Peer-CIDR verfügen.

Um die CIDR-Blöcke der Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen die Felder `local_cidrs` und `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**Nachricht**: Der CIDR-Block wurde in der VPN-Verbindung `<vpn_connection_id>` nicht gefunden.

Geben Sie eine gültige CIDR an, die sich in der Verbindung befindet.

Um die CIDR-Blöcke der Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen Sie die Felder `local_cidrs` und `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**Nachricht**: Ein CIDR-Block in der VPN-Verbindung `<vpn_connection_id>` konnte nicht aktualisiert werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
**Nachricht**: Der CIDR-Block `<cidr_block>` stellt keine gültige Adresse dar.

Der Wert muss ein gültiger interner CIDR-Block ohne festgelegte Hostbits sein.

Um die CIDR-Blöcke der Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen Sie die Felder `local_cidrs` und `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**Nachricht**: Der CIDR-Block `<cidr_block_1>` weist eine Überschneidung mit `<cidr_block_2>` auf. Zwei Peer-CIDR-Blöcke dürfen sich nicht auf derselben VPC überschneiden und zwei lokale CIDR-Blöcke dürfen sich nicht in derselben Verbindung überschneiden.

Geben Sie eine gültige CIDR (Classless InterDomain Routing) an, die die in der Spezifikation angegebenen Anforderungen erfüllt.

Um die CIDR-Blöcke der Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen die Felder `local_cidrs` und `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Nachricht**: Der Name `<vpn_connection_name>` wird bereits von der VPN-Verbindung `<vpn_connection_id>` verwendet.

Geben Sie einen anderen Verbindungsnamen an. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
**Nachricht**: Der Name `<vpn_connection_name>` ist kein gültiger VPN-Verbindungsname.

Ein gültiger Verbindungsname beginnt mit einem Buchstaben, gefolgt von Buchstaben, Ziffern, Unterstreichungszeichen oder Bindestrichen.

## vpn_connection_invalid_psk_format
**Nachricht**: Der vorab bekannte gemeinsame Schlüssel (PSK, Pre-Shared Key) `<psk>` liegt nicht in einem gültigen Format vor.

Ein gültiger vorab bekannter gemeinsamer Schlüssel sollte nur 6 bis 128 Zeichen lang sein, bestehend aus Buchstaben, Ziffern, `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Nachricht**: Beim Erstellen einer VPN-Verbindung ist mindestens ein lokaler CIDR-Block erforderlich.

Geben Sie einen gültigen lokalen CIDR-Wert an, der die Anforderungen der Spezifikation erfüllt.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Nachricht**: Das Kontingent für die lokalen Teilnetze für VPN-Verbindungen für das VPN-Gateway `<vpn_gateway_id>` wurde erreicht.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen lokalen Teilnetze für alle VPN-Verbindungen anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections` und überprüfen Sie für jede Verbindung das Feld `local_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Nachricht**: Das Kontingent für die lokalen Teilnetze für die VPN-Verbindung wurde erreicht.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen lokalen Teilnetze für eine VPN-Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen Sie das Feld `local_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**Nachricht**: Die VPN-Verbindung `<vpn_connection_id>` konnte nicht erstellt werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
**Nachricht**: Die VPN-Verbindung `<vpn_connection_id>` konnte nicht gelöscht werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_found
**Nachricht**: Die VPN-Verbindung `<vpn_connection_id>` wurde nicht gefunden.

Überprüfen Sie, ob die Verbindungs-ID richtig ist. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
**Nachricht**: Die VPN-Verbindung `<vpn_connection_id>` konnte nicht aktualisiert werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
**Nachricht**: Mindestens ein Paar aus lokaler CIDR und Peer-CIDR mit demselben Peer-Gateway wurde dupliziert.

Für dieses Gateway ist eine weitere Verbindung mit übereinstimmender lokaler CIDR und übereinstimmender Peer-CIDR vorhanden. Geben Sie eine gültige Kombination aus lokaler CIDR und Peer-CIDR an.

## vpn_connection_peer_cidrs_required
**Nachricht**: Bei der Erstellung einer VPN-Verbindung ist mindestens ein Peer-CIDR-Block erforderlich.

Geben Sie einen gültigen Peer-CIDR-Wert an, der die Anforderungen der Spezifikation erfüllt.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Nachricht**: Das Kontingent für die Peer-Teilnetze für VPN-Verbindungen für das VPN-Gateway `<vpn_gateway_id>` wurde erreicht.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen Peer-Teilnetze für alle VPN-Verbindungen anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections` und überprüfen Sie für jede Verbindung das Feld `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Nachricht**: Das Kontingent für die Peer-Teilnetze für die VPN-Verbindung wurde erreicht.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen Peer-Teilnetze für eine VPN-Verbindung anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` und überprüfen Sie das Feld `peer_cidrs`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**Nachricht**: Es konnte keine statische Route für den Peer-CIDR-Block `<peer_cidr>` hinzugefügt werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Nachricht**: Es konnte keine statische Route für den Peer-CIDR-Block `<peer_cidr>` entfernt werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
**Nachricht**: Beim Aktualisieren einer Verbindung können CIDR-Blöcke nicht geändert werden. Verwenden Sie die richtige API beim Erstellen oder Löschen von CIDR-Blöcken.

Geben Sie einen gültigen Anforderungswert an, der die in der Spezifikation angegebenen Anforderungen erfüllt.

Weitere Anweisungen zum Beheben dieses Problems finden Sie in der [API-Dokumentation](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connections_quota_exceeded
**Nachricht**: Die VPN-Verbindung kann nicht erstellt werden, weil das Kontingent für das VPN-Gateway `<vpn_gateway_id>` erreicht wurde.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen VPN-Verbindungen für ein VPN-Gateway anzuzeigen, verwenden Sie die API `GET /vpn_gateways/<vpn_gateway_id>/connections`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**Nachricht**: Der Name `<vpn_gateway_name>` wird bereits vom VPN-Gateway `<vpn_gateway_id>` verwendet.

Geben Sie einen anderen Namen für das VPN-Gateway an. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
**Nachricht**: Das VPN-Gateway konnte nicht initialisiert werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
**Nachricht**: Der Name `<vpn_gateway_name>` ist kein gültiger VPN-Gateway-Name.

Ein gültiger VPN-Gateway-Name beginnt mit einem Buchstaben, gefolgt von Buchstaben, Ziffern, Unterstreichungszeichen oder Bindestrichen.

## vpn_gateway_invalid_state
**Nachricht**: Das VPN-Gateway `<vpn_gateway_id>` weist für die angeforderte Operation einen ungültigen Status auf.

Das VPN-Gateway muss den Status `available` (verfügbar) haben, bevor Sie es betreiben können. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
**Nachricht**: Es konnte keine IP-Adresse für das VPN-Gateway erstellt werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
**Nachricht**: Es konnte keine Teilnetz-ID für die angegebene VPN-Gateway-Vorlage gefunden werden.

Das Feld **subnet-id** ist ein erforderliches Feld. Geben Sie mit dem Befehl eine Teilnetz-ID an.

## vpn_gateway_missing_name
**Nachricht**: Es konnte kein Name für die angegebene VPN-Gateway-Vorlage gefunden werden.

Das Feld **name** ist ein erforderliches Feld. Geben Sie mit dem Befehl einen Namen an.

## vpn_gateway_not_created
**Nachricht**: Das VPN-Gateway `<vpn_gateway_id>` konnte nicht erstellt werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
**Nachricht**: Das VPN-Gateway `<vpn_gateway_id>` konnte nicht gelöscht werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_found
**Nachricht**: Das VPN-Gateway `<vpn_gateway_id>` wurde nicht gefunden.

Überprüfen Sie, ob die VPN-Gateway-ID richtig ist. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
**Nachricht**: Das VPN-Gateway `<vpn_gateway_id>` konnte nicht aktualisiert werden.

Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_found
**Nachricht**: Das Teilnetz `<subnet_id>` wurde für das angegebene VPN-Gateway nicht gefunden.

Sie haben auf ein Teilnetz verwiesen, das nicht vorhanden ist. Überprüfen Sie Ihre Anforderung, um sicherzustellen, dass Sie die richtige Teilnetz-ID angegeben haben. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Nachricht**: Das VPN-Gateway konnte aufgrund des Teilnetzstatus nicht erstellt werden.

Geben Sie ein Teilnetz an, das sich im Status `available` befindet. Versuchen Sie es in ein paar Minuten erneut. Bleibt dieses Problem bestehen, [wenden Sie sich an den Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Nachricht**: Das VPN-Gateway kann nicht erstellt werden, weil das Kontingent für Ihr Konto und/oder Ihre Region erreicht wurde.

Die Kontingente pro Ressource sind auf der Seite mit den [Kontingenten](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} angegeben.

Um die aktuellen VPN-Gateways anzuzeigen, verwenden Sie die API `GET /vpn_gateways`. Funktional entsprechender CLI-Befehl: `ibmcloud is vpn-gateways`. 

## zone_inconsistency
**Nachricht**: Die Ressourcen müssen zu derselben Zone gehören.

* Stellen Sie beim Zuordnen eines Datenträgers sicher, dass sich der Datenträger und die Instanz in derselben Zone befinden.
* Stellen Sie beim Zuordnen einer variablen IP-Adresse sicher, dass sich die variable IP-Adresse und das Teilnetz in derselben Zone befinden.
