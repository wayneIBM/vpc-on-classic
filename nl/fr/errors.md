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

# Messages d'erreur des API d'IBM Cloud Virtual Private Cloud
{: #rias-error-messages}

Lorsque vous recevez un message d'erreur des API {{site.data.keyword.cloud}} Virtual Private Cloud, vous pouvez vous servir de l'ID de message pour savoir comment résoudre le problème.
{:shortdesc}

## account_type_invalid
**Message** : You must have a Pay-As-You-Go account to provision a Virtual Private Cloud.

Votre compte doit figurer dans un plan de type Paiement à la carte pour mettre à disposition des clouds privés virtuels (VPC). Vous pouvez obtenir davantage de détails sur la mise à niveau de votre compte ici : [Mise à niveau de votre compte](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**Message** : The network ACL cannot be deleted because it is attached to resources.

La liste de contrôle d'accès au réseau ne peut pas être supprimée car elle est associée à un sous-réseau ou à un cloud privé virtuel. 

Pour voir si un sous-réseau utilise la liste de contrôle d'accès au réseau, utilisez l'API `GET /v1/subnets?version=2019-05-31&generation=1`. Commande de l'interface de ligne de commande équivalente : `ibmcloud is subnets`.  Pour mettre à jour la liste de contrôle d'accès au réseau utilisée par un sous-réseau, utilisez l'API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` ou la commande de l'interface de ligne de commande `ibmcloud is subnet-update`.

Pour voir si un cloud privé virtuel utilise une liste de contrôle d'accès au réseau, utilisez l'API `GET /v1/vpcs?version=2019-05-31&generation=1`.  Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpcs`. Il est impossible de mettre à jour ou de supprimer la liste de contrôle d'accès au réseau par défaut utilisée par un VPC. La liste par défaut est automatiquement supprimée à la suppression du VPC.

## address_prefix_conflict
**Message** : The address prefix with this CIDR is in use.

Le bloc CIDR pour le préfixe d'adresse demandé est en conflit avec un préfixe d'adresse existant.

Pour afficher la liste des préfixes d'adresse d'un VPC, utilisez l'API `GET /vpcs/{vpc-id}/address_prefixes` et vérifiez que l'adresse CIDR demandée n'est pas utilisée par la zone `cidr` d'un autre préfixe d'adresse.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**Message** : Cannot delete an address prefix in use.

Un ou plusieurs sous-réseaux utilisent le préfixe d'adresse.  Pour identifier les sous-réseaux qui utilisent le préfixe d'adresse, utilisez la commande d'API `GET /v1/subnets?version=2019-05-31&generation=1` et vérifiez la zone `ipv4_cidr_block`.  Commande de l'interface de ligne de commande équivalente :  `ibmcloud is subnets` avec vérification de la colonne `Subnet CIDR`. Vous devez supprimer tous les sous-réseaux utilisant le préfixe d'adresse pour pouvoir supprimer le préfixe.

## backend_service_unavailable
**Message** : The backend service is unavailable.

Un service cloud de back end utilisé par le VPC ne répond pas. Le VPC utilise plusieurs services IBM Cloud, tels que :

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Catalogue global](https://{DomainName}/catalog)
- Resource Controller
- Resource Manager

Vous pouvez vérifier le [statut](https://{DomainName}/status) des services IBM Cloud et réessayer dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_field
**Message** : Correct instance UUID should be provided

L'une des valeurs indiquées dans la demande est incorrecte. Vérifiez la valeur de `target` dans l'erreur renvoyée pour obtenir des indices permettant d'identifier le paramètre incorrect. Dans certains cas, l'identificateur unique universel (UUID) ou le volume dans la demande est introuvable. Fournissez une valeur valide et réessayez.

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir de l'aide supplémentaire. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## bad_request
**Message** : The information given was invalid, malformed, or missing a required field.

La demande ne correspond pas à ce que nous attendions. Utilisez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour vous aider à formuler la demande.

Si vous suivez la spécification et que vous obtenez toujours cette erreur, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Message** : Only one VPC with Classic Access can be created per region.

Un seul cloud privé virtuel (VPC) avec accès classique peut être créé par région. Pour répertorier le VPC avec accès classique, exécutez la commande `ibmcloud is vpcs --classic-access`. Le VPC avec accès classique existant doit être supprimé pour qu'un autre VPC de ce type puisse être créé.

## classic_access_vpc_account_not_VRF_enabled
**Message** : Account must be VRF-enabled to create a classic access VPC.

Le compte classique lié n'est pas activé pour VRF. Un VPC avec accès classique nécessite le compte classique lié pour être activé pour VRF. Pour activer votre compte pour VRF, reportez-vous à [VRF sur IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_found
**Message** : Default address prefix not found.

Ce message d'erreur s'affiche si le préfixe d'adresse par défaut est introuvable.

## duplicate_error
**Message** : The input provided already exists.

La ressource indiquée existe déjà. Essayez d'utiliser un autre nom pour la ressource que vous souhaitez créer. Par exemple, lorsque vous créez un cloud privé virtuel (VPC), vous pouvez répertorier les VPC existants en exécutant la commande `ibmcloud is vpcs`.  Choisissez un nom qui ne soit pas conflictuel.

## floating_ip_in_use
**Message** : The floating IP is in use.

Cette adresse IP flottante est associée à une instance de serveur virtuel ou à une passerelle publique.

Pour voir où est utilisée l'adresse IP flottante, utilisez l'API `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` et examinez les valeurs "target". Commande de l'interface de ligne de commande équivalente : `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_many
**Message** : Can only have one public gateway per zone in a VPC.

Une passerelle publique et une seule est autorisée par zone dans un cloud privé virtuel, mais elle doit être connectée à plusieurs sous-réseaux dans la zone. Afin de rechercher la passerelle publique pour une zone, exécutez l'API GET public_gateways et consultez les valeurs de "vpc" et "zone". Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter la commande `ibmcloud is public-gateways` et consulter les valeurs de "VPC" et "Zone".

Utilisez l'ID de la passerelle publique pour attacher la passerelle publique à un sous-réseau ; vous trouverez un exemple dans les [exemples d'API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-). Si vous utilisez l'interface de ligne de commande, vous pouvez utiliser la commande de mise à jour du sous-réseau pour attacher le sous-réseau à une passerelle publique, par exemple `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
**Message** : The specified health monitor delay <health_monitor_delay> is invalid

Fournissez une valeur comprise entre 2 et 60 pour le paramètre `health monitor delay`.

## health_monitor_invalid_max_retries
**Message** :  The specified health monitor max retries <health_monitor_max_retries> is invalid

Fournissez une valeur comprise entre 1 et 10 pour `health max retries`.

## health_monitor_invalid_timeout
**Message** :  The specified health monitor timeout <health_monitor_timeout> is invalid.

Fournissez une valeur comprise entre 1 et 59 pour `health timeout`. La valeur sélectionnée doit être inférieure à celle de `health monitor delay`.

## health_monitor_invalid_url_path
**Message** : The specified health URL path <health_monitor_url> is invalid.

Fournissez une URL valide pour le moniteur de santé. L'URL peut contenir des caractères alphanumériques, des traits d'union et des points. Les espaces et les barres obliques ne sont pas admis. La longueur maximale du chemin d'URL est fixée à 255 caractères.

## health_monitor_invalid_protocol
**Message** : The specified health monitor protocol <health_monitor_protocol> is invalid.

Fournissez une valeur valide pour le protocole du moniteur de santé. Les valeurs admises sont `http` et `tcp`.

## health_monitor_missing_protocol
**Message** :  Health monitor protocol is missing

La zone du protocole du moniteur de santé est requise. Indiquez ce protocole dans votre demande. Les valeurs admises sont `http` et `tcp`.

## health_monitor_missing_url_path
**Message** :  Health monitor URL path is missing. It is required for a HTTP type health monitor.

Pour un moniteur de santé utilisant le protocole HTTP, la zone de chemin d'URL du moniteur de santé est requise. Fournissez un chemin d'URL valide.

## health_monitor_timeout_delay_conflict
**Message** :  Health Monitor timeout <health_monitor_timeout> should not be greater than or equal to delay <health_monitor_delay>.

La valeur du paramètre `health monitor delay` doit être supérieure à la valeur du paramètre `health monitor timeout`.

## http_request_size_exceeded
**Message** : The HTTP request is too large.

Ce problème survient lorsque le contenu que vous avez envoyé dans votre demande comporte un nombre trop élevé de caractères. Réessayez avec un contenu plus petit. Par exemple, au lieu de tout effectuer dans une seule demande, essayez de créer une ressource minimale dans une demande, puis ajoutez l'état progressivement dans plusieurs demandes consécutives.

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir de l'aide supplémentaire. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
**Message** : None

Une erreur s'est produite dans le service IAM, l'authentification ou l'autorisation est en cours de vérification. L'indisponibilité du service IAM est peut-être momentanée. Relancez la demande après quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
**Message** : The IKE policy cannot be created because your account has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les stratégies IKE appliquées, utilisez l'API `GET /ike_policies`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**Message** : The name `<ike_policy_name>` is in use already by IKE policy `<ike_policy_id>`.

Fournissez un autre nom de stratégie IKE. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
**Message** : The IKE policy `<ike_policy_id>` is in use by one or more connections.

Une stratégie IKE ne peut pas être supprimée si elle est en cours d'utilisation par une ou plusieurs connexions.

Pour voir les connexions utilisant la stratégie IKE, utilisez l'API `GET /ike_policies/<ike_policy_id>/connections`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**Message** : The name `<ike_policy_name>` is not a valid IKE policy name.

Un nom de stratégie IKE valide commence par une lettre, suivie d'autres lettres, de chiffres, de traits de soulignement ou de traits d'union.

## ike_policy_not_created
**Message** : The IKE policy `<ike_policy_id>` could not be created.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
**Message** : The IKE policy `<ike_policy_id>` could not be deleted.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_found
**Message** : The IKE policy `<ike_policy_id>` could not be found.

Vous avez référencé une stratégie IKE qui n'existe pas. Vérifiez votre demande pour vous assurer que vous avez spécifié l'ID de stratégie IKE approprié. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
**Message** : The IKE policy `<ike_policy_id>` could not be updated.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
**Message** : The instance cannot be deleted in the current status.

Vous ne pouvez pas supprimer une instance avec le statut `deleting`, `pending`, `starting`, `stopping` ou `restarting`. L'instance doit avoir le statut `running` ou `stopped`. Si le statut ne change pas, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
**Message** : The instance cannot be created because the hostname is not valid.

Le nom d'hôte de l'instance doit respecter les exigences suivantes :
* Le nom d'hôte ne doit contenir que des caractères alphanumériques et des tirets
* Les majuscules ne sont pas autorisées
* Les tirets au début et à la fin ne sont pas admis
* Les nombres au début ne sont pas admis
* Les tirets consécutifs ne sont pas admis
* La longueur maximale est fixée à 15 caractères pour Windows
* La longueur maximale est fixée à 40 caractères pour les autres systèmes d'exploitation

## instance_invalid_port_speed
**Message** : The instance cannot be created because the specified network interface port speed is not valid.

La vitesse du port de l'interface réseau doit être `100` ou `1000` Mbit/s.

## instance_name_exists
**Message** : The same instance name already exists.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
**Message** : The instance cannot be created because the number of volume attachments would exceed the quota.

Les quotas par ressource du cloud privé virtuel sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}.

## instance_too_many_keys
**Message** : The Windows instance cannot be created because the request contains multiple keys.

Les instances Windows ne prennent en charge qu'une seule clé.

## insufficient_space_for_subnet
**Message** : Insufficient space for subnet in address prefix.

Impossible de créer le sous-réseau car le nombre d'adresses requis ne peut pas être attribué. Réessayez en utilisant un nombre inférieur d'adresses ou avec un CIDR supérieur.

## internal_error
**Message** : An internal error occurred.

Une erreur inattendue s'est produite. Ce problème peut être temporaire. Relancez la demande dans quelques minutes. Si cette erreur persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
**Message** : None

Une erreur inattendue s'est produite. Ce problème peut être temporaire. Relancez la demande dans quelques minutes. Si cette erreur persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
**Message** : Please contact your administrator.

Une erreur inattendue s'est produite. Ce problème peut être temporaire. Relancez la demande dans quelques minutes. Si cette erreur persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_generation_parameter
**Message** : The generation query parameter must be set to 1.

Pour les versions à partir du 31/05/2019, le paramètre de requête `generation` doit être défini avec la valeur 1 pour autoriser les demandes d'API VPC on Classic.

## invalid_id_format
**Message** : Bad ID format. Ensure format is correct.

Assurez-vous que l'ID que vous avez fourni ne contient pas de données incorrectement formées.

Vous pouvez obtenir ce message d'erreur si vous fournissez une requête de démarrage incorrectement formée lorsque vous effectuez une demande de pagination. Par exemple,
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` contient deux signes `?`. Corrigez la requête et réessayez.

## invalid_route
**Message** : The requested route does not exist.

La route demandée dans l'URL d'API que vous avez fournie n'existe pas. Vérifiez que l'URL que vous avez indiquée pour appeler le noeud final d'API est correcte.

## invalid_request_field
**Message** : A field provided in the request is not valid.

Par exemple, pour mettre à jour la liste de contrôle d'accès au réseau utilisée par un sous-réseau, utilisez l'API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’`.
La demande suivante ne sera pas valide car "networkacl" n'est pas une zone valide, `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir les valeurs admises. 

## invalid_state
**Message** : An action was requested on a resource which is not supported at the current status of the resource.

Si la ressource est une instance :

1. Il se peut que l'instance soit en cours de redémarrage. Consultez les [actions autorisées](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance) en fonction du statut de l'instance.

2. Lorsque le statut est `pending`, vous ne pouvez pas effectuer les actions suivantes :
  * supprimer l'instance
  * connecter un volume à l'instance
  * déconnecter un volume de l'instance
  * mettre à jour l'instance

Renouvelez votre action ultérieurement. Si le statut de la ressource ne change pas, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

Si la ressource est une image :

Lorsque le statut de l'instance est `deleting`, vous ne pouvez pas effectuer les actions suivantes :
  * corriger l'image
  * supprimer l'image

Ne renouvelez pas la demande. La suppression de l'image se terminera le moment venu. Une fois l'image supprimée, toutes les demandes effectuées sur cette image échouent avec l'erreur `not_found`.

## invalid_version
**Message** : The `version` parameter is invalid, it must be of the form `YYYY-MM-DD`.

Le format de la version doit être _AAAA-MM-JJ_. Pour les mois ou les dates à un seul chiffre, comme le 1er janvier, la version est similaire à `2019-01-01`. Le paramètre de version doit être présent dans l'adresse URL. La date indiquée dans le paramètre version doit être ultérieure à 2019-01-01 mais antérieure à la date en cours.

## invalid_version_range
**Message** : The `version` value cannot be set at a future date nor before `2019-01-01`.

La date indiquée dans le paramètre version doit être ultérieure à 2019-01-01 mais antérieure à la date en cours.

## invalid_zone
**Message** : Please check whether the resources you are requesting are in the same zone.

Vous pouvez voir ce message lors d'une tentative de connexion d'une passerelle publique de la zone-1 à un sous-réseau de la zone-2.

## ipsec_policies_quota_exceeded
**Message** : The IPsec policy cannot be created because your account has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les stratégies IPSec appliquées, utilisez l'API `GET /ipsec_policies`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**Message** : The name `<ipsec_policy_name>` is in use already by IPsec policy `<ipsec_policy_id>`.

Fournissez un autre nom de stratégie IPsec. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
**Message** : The IPsec policy `<ipsec_policy_id>` is in use by one or more connections.

Une stratégie IPSec ne peut pas être supprimée si elle est en cours d'utilisation par une ou plusieurs connexions.

Pour voir les connexions utilisant la stratégie IPsec, utilisez l'API `GET /ipsec_policies/<ipsec_policy_id>/connections`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**Message** : The name `<ipsec_policy_name>` is not a valid IPsec policy name.

Un nom de stratégie IPSec valide commence par une lettre, suivie d'autres lettres, de chiffres, de traits de soulignement ou de traits d'union.

## ipsec_policy_not_created
**Message** : The IPsec policy `<ipsec_policy_id>` could not be created.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
**Message** : The IPsec policy `<ipsec_policy_id>` could not be deleted.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_found
**Message** : The IPsec policy `<ipsec_policy_id>` could not be found.

Vous avez référencé une stratégie IPSec qui n'existe pas. Vérifiez votre demande et indiquez un ID de stratégie IPSec valide. Si vous êtes certain que la stratégie existe, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
**Message** : The IPsec policy `<ipsec_policy_id>` could not be updated.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
**Message** : The same key content already exists.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
**Message** : The same key name already exists.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
**Message** : The key cannot be created because the name is not valid.

Le nom de la clé doit respecter les exigences suivantes :
* il doit commencer par un caractère alphabétique, [A-Za-z]
* il ne doit contenir que des caractères alphanumériques, des tirets ou des traits de soulignement, [-A-Za-z0-9_]
* il ne doit pas dépasser 65 caractères

## key_invalid_type
**Message** : The key cannot be created because the type is not valid.

Le seul type de clé pris en charge est `rsa`.

## key_parse_failure
**Message** : The key cannot be created because the public key value is not valid.

La clé publique fournie dans la demande n'est pas valide. Vérifiez la valeur ou régénérez la clé publique et recommencez.

Dans les systèmes d'exploitation Linux, vous pouvez exécuter la commande suivante pour valider la clé publique : `ssh-keygen -l -v -f <key_file>`.

## listener_certificate_not_found
**Message** : Certificate instance with CRN `<listener_certificate_crn>` cannot be found or no permission to access the certificate instance.

Indiquez un nom de ressource de cloud d'instance de certificat existant ou demandez à votre administrateur de compte de vous accorder des droits d'accès à l'instance de certificat indiquée.

## listener_duplicate_port
**Message** : Port `<listener_port>` is used by another listener instance. Please choose a different port.

Choisissez un autre port.

## listener_invalid_certificate
**Message** : CRN of certificate instance for the HTTPS listener is invalid.

Fournissez un CRN d'instance de certificat valide.

## listener_invalid_connection_limit
**Message** : Listener connection limit `<listener_connection_limit>` in invalid.

Fournissez un nombre limite de connexions valide. Ce nombre doit être un entier compris entre 1 et 15000.

## listener_invalid_port
**Message** : Listener port `<listener_port>` is invalid.

Choisissez un autre port.

## listener_invalid_protocol
**Message** : Listener protocol `<listener_protocol>` is invalid.

Fournissez un protocole d'écouteur valide. Les valeurs admises pour le protocole d'écouteur sont `http`, `https` et `tcp`.

## listener_missing_certificate_crn
**Message** : CRN of the certificate instance for the `https` listener is missing.

La zone CRN de l'instance de certificat est requise.

## listener_missing_port
**Message** : Listener port is missing.

La zone de port d'écoute est requise.

## listener_missing_protocol
**Message** : Listener protocol is missing.

La zone de protocole d'écouteur est requise. Indiquez le protocole d'écouteur dans votre demande. Les valeurs admises sont `http`, `https` et `tcp`.

## listener_not_found
**Message** : The listener with ID `<listener_id>` cannot be found.

Indiquez un ID d'écouteur existant.

## listener_over_quota
**Message** : Listener cannot be created. Quota of listeners for the load balancer resource has reached the maximum limit.

Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## listener_pool_protocols_conflict
**Message** : Listener protocol(`<listener_protocol>`) and pool protocol(`<pool_protocol>`) are in conflict.

Un écouteur dont le protocole est `https` ou `http` ne peut être associé qu'à un pool dont le protocole est `http`. De même, un écouteur `tcp` ne peut être associé qu'à un pool `tcp`.

## listener_reserved_port_conflict
**Message** : Listener port `<listener_port>` is one of the reserved ports. The port range of 56500 to 56520 is reserved for management purposes. Choose a different port.

Choisissez un autre port. 

## load_balancer_delete_conflict
**Message** : The load balancer with ID <load_balancer_id> cannot be deleted because its status is one of these:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Supprimez l'équilibreur de charge lorsque son statut est : ACTIVE.

## load_balancer_duplicate_name
**Message** : Name `<load_balancer_name>` is used by another load balancer instance. Please choose a different name.

Indiquez un autre nom pour l'instance d'équilibreur de charge. Le nom de l'équilibreur de charge doit être unique dans le VPC et dans le compte.

Votre service Load Balancer for VPC n'entrera pas en conflit avec votre service Cloud Load Balancer, car les noms de domaine DNS sont différents pour ces services et le nom du service Load Balancer for VPC n'est pas inclus dans le nom DNS.
{: note}

## load_balancer_insufficient_ips
**Message** : The subnet with ID(s) <subnet_id> has insufficient available IPv4 addresses.

Dans votre demande, fournissez un sous-réseau valide avec des adresses IPv4 disponibles, dans lequel vous souhaitez créer l'équilibreur de charge.

## load_balancer_invalid_description
**Message** : Load balancer description `<load_balancer_description>` is not valid.

Une description d'équilibreur de charge ne peut pas dépasser 255 caractères.

## load_balancer_invalid_is_public
**Message** : Value of is_public `<load_balancer_is_public>` is not valid.

La valeur du paramètre d'équilibreur de charge `is_public` doit être _true_ ou _false_.

## load_balancer_invalid_name
**Message** : Name `<load_balancer_name>` is invalid.

Les noms ne peuvent pas être vides. Un nom d'équilibreur de charge valide commence par une lettre, suivie de lettres, de chiffres ou de traits de soulignement. Le nom ne doit pas dépasser 40 caractères.

## load_balancer_invalid_subnet
**Message** : The subnet with ID <subnet_id>  is not valid. Make sure to use an existing subnet with 'available' status.

Fournissez des sous-réseaux valides ayant le statut `available` lorsque vous entrez votre demande de création d'équilibreur de charge.

## load_balancer_missing_is_public
**Message** : 'is_public' field is missing.

La zone `is_public` est obligatoire. Fournissez une valeur pour la zone `is_public`.

## load_balancer_missing_name
**Message** : Load balancer name is missing.

La zone de nom (`name`) de l'équilibreur de charge est requise. Indiquez un nom pour l'équilibreur de charge.

## load_balancer_missing_subnets
**Message** : Load balancer subnets is missing.

La zone `subnets` est obligatoire. Dans votre demande de création d'équilibreur de charge, fournissez les informations sur les sous-réseaux dans lesquels vous souhaitez créer l'équilibreur de charge.

## load_balancer_missing_subnet_id
**Message** : Subnet ID is missing.

La zone **Subnet ID** est requise. Fournissez un ID de sous-réseau avec votre commande.

## load_balancer_not_found
**Message** : The load balancer with ID `<load_balancer_id>` cannot be found.

Indiquez l'ID d'un équilibreur de charge existant.

## load_balancer_over_quota
**Message** : Load balancer cannot be created. Quota of load balancer instances under your account has reached maximum limit.

Supprimez un équilibreur de charge existant ou contactez le support pour augmenter le quota pour les équilibreurs de charge dans votre compte.

Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## load_balancer_subnet_not_found
**Message** : The subnet with ID <subnet_id>  cannot be found.

Dans votre demande, vous devez indiquer les ID des sous-réseaux existants dans lesquels vous souhaitez créer l'équilibreur de charge.

## load_balancer_unchanged_update
**Message** : There is nothing to update the load balancer with ID `<load_balancer_id>`.

Aucune information n'a été fournie pour mettre à jour une instance d'équilibreur de charge avec l'ID que vous avez indiqué.

## load_balancer_update_conflict
**Message** : The load balancer with ID <load_balancer_id> cannot be updated because its status is one of these:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Mettez à jour l'équilibreur de charge lorsque son statut est : ACTIVE.

## member_duplicate_address_and_port
**Message** :  Member with address <member_address> and port <member_port> already exists in the pool.

Indiquez une combinaison d'adresse et de port unique pour le membre dans votre demande.

## member_invalid_address
**Message** : The specified member IP address <member_ip> is invalid.

Dans votre demande, indiquez une adresse CIDR IPv4 valide pour le membre.

## member_invalid_port
**Message** : The specified member port `<member_port>` is invalid.

Indiquez un numéro de port valide. Les numéros de port valides sont compris entre 1 et 65535.

## member_invalid_weight
**Message** : The specified member weight `<member_weight>` is invalid.

Indiquez un poids valide pour le membre. La valeur doit être un nombre entier compris entre 0 et 100.

## member_ip_not_found
**Message** : Member with IP address <member_IP> does not belong to any subnets in the Region and VPC of the load balancer.

Dans votre demande, indiquez l'adresse d'un membre qui appartient à un sous-réseau de la même région et du même VPC que l'équilibreur de charge.

## member_missing_address
**Message** : Member address is missing.

La zone d'adresse de membre est requise. Indiquez une valeur pour l'adresse de membre.

## member_missing_protocol_port
**Message** : Member port is missing.

La zone de port de membre est requise. Indiquez une valeur pour le port de membre.

## member_not_found
**Message** :  Member with ID <member_id> cannot be found.

Indiquez un ID de membre existant.

## member_over_quota
**Message** : Member cannot be created. Quota of member instances under the pool has reached maximum limit.

Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## missing_generation_parameter
**Message** : The generation query parameter is required.

Pour les versions à partir du 31/05/2019, le paramètre de requête `generation` est obligatoire pour autoriser les demandes d'API VPC on Classic.

## missing_ims_account_id
**Message** : None

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## missing_version
**Message** : The `version` parameter is required, and it must be of the form `YYYY-MM-DD`.

Un paramètre de version est requis pour toutes les demandes d'API. Le format de la version doit être _AAAA-MM-JJ_. Pour les mois ou les dates à un seul chiffre, comme le 1er janvier, la version est similaire à `2019-01-01`. La date indiquée dans le paramètre version doit être ultérieure à 2019-01-01 mais antérieure à la date en cours. Consultez ces [exemples d'API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) pour savoir comment fournir le paramètre de version.

## network_conflict
**Message** : CIDR conflicts with existing Address Prefix in VPC.

Ce message peut s'afficher si vous indiquez un bloc CIDR de réseau qui entre en conflit avec un bloc CIDR de réseau existant dans le même espace d'IP. 

Pour rechercher les blocs CIDR existants dans les préfixes d'adresse pour le VPC, exécutez la commande d'API `GET /v1/vpcs/<vpc-id>/address_prefixes` et examinez les valeurs `cidr` de la réponse. Vérifiez la valeur de `cidr` pour vous assurer que le CIDR que vous avez fourni n'est pas utilisé par d'autres préfixes d'adresse.

Si vous utilisez l'interface de ligne de commande, exécutez la commande `ibmcloud is vpc-addrs <vpc-id>` et vérifiez la valeur de `CIDR Block` pour détecter les conflits.

Pour recherche les CIDR existants dans les sous-réseaux, exécutez la commande d'API `GET /v1/subnets` pour répertorier tous les sous-réseaux du VPC. Exécutez ensuite la commande d'API `GET /v1/subnets/<subnet-id>` pour chaque sous-réseau du VPC et vérifiez la valeur de `ipv4_cidr_block` dans la réponse. Vérifiez la valeur de `ipv4_cidr_block` pour vous assurer que le CIDR que vous avez fourni n'est pas utilisé par d'autres préfixes d'adresse.

Si vous utilisez l'interface de ligne de commande, exécutez la commande `ibmcloud is subnets` pour répertorier tous les sous-réseaux du VPC. Exécutez ensuite la commande `ibmcloud is subnet <subnet-id>` pour chaque sous-réseau du VPC et recherchez les valeurs qui entrent en conflit avec la valeur de `IPv4 CIDR` dans la sortie de l'interface de ligne de commande.

## not_authorized
**Message** : The request is not authorized.

Cette erreur s'affiche si votre jeton IAM est manquant ou a expiré. Pour des instructions de génération d'un jeton, voir [Création d'un cloud privé virtuel à l'aides des API REST](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis). Si le jeton n'a pas expiré, assurez-vous que le compte que vous utilisez est autorisé à utiliser cette fonction et que vous disposez des [droits appropriés](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Si vous bénéficiez de l'autorisation adéquate et que vous recevez toujours cette erreur, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_found
**Message** : Please check whether the resource you are requesting exists.

Vous avez référencé une ressource qui n'existe pas ou à laquelle vous n'avez pas accès. Vérifiez votre demande pour vous assurer que vous avez spécifié les références et les ID appropriés.

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir de l'aide supplémentaire. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implemented
**Message** : None

La méthode n'est pas implémentée. Vérifiez votre demande pour vous assurer que vous appelez une [API valide](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. [Contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si cette API est censée être implémentée.

## not_in_address_prefix
**Message** : The provided CIDR does not fit in any of the address prefixes in the provided zone.

Cette erreur survient si un utilisateur tente de créer un sous-réseau avec un bloc CIDR qui ne correspond à aucun des préfixes d'adresse de la zone indiquée.

Exécutez la commande `GET /vpcs/{vpc_id}/address_prefixes` pour obtenir la liste des préfixes d'adresse du VPC. Si vous utilisez l'interface de ligne de commande, vous pouvez utiliser la commande `ibmcloud is vpc-address-prefixes` pour répertorier tous les préfixes d'adresse de votre VPC. Examinez les valeurs `cidr` et `zone` de la réponse et assurez-vous que la valeur `cidr` du sous-réseau correspond à un sous-ensemble de la valeur `cidr` du préfixe d'adresse de la zone dans laquelle vous essayez de le créer.

## over_quota
**Message** : The request would exceed the quota for a resource type.

Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## password_not_ready
**Message** : None

Votre instance doit être en cours d'exécution pour que vous puissiez extraire le mot de passe. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
**Message** : Pool cannot be deleted because it is still associated with one or more listeners.

Vérifiez que le pool est dissocié de tout écouteur et réessayez.

## pool_duplicate_name
**Message** : Pool name `<pool_name>` is already used by another pool. Choose a different name.

Choisissez un autre nom de pool.

## pool_invalid_name
**Message** :  Name <pool_name> is invalid.

Fournissez un nom de pool valide.  Un nom de pool d'équilibreur de charge valide commence par une lettre, suivie de lettres, de chiffres et de traits d'union. Le nom ne doit pas dépasser 40 caractères.

## pool_invalid_session_persistence
**Message** : Pool session persistence value is not valid.

Indiquez une valeur de persistance de session valide. Valeur admise : `SOURCE_IP`.

## pool_missing_algorithm
**Message** : Load balancing algorithm is missing.

La zone d'algorithme d'équilibrage de charge est requise. Indiquez l'algorithme d'équilibrage de charge dans votre demande. Les valeurs admises sont `round_robin`, `weighted_round_robin` et `least_connections`.

## pool_missing_health_monitor
**Message** : Pool health monitor is missing.

La zone de moniteur de santé du pool est requise.

## pool_missing_members
**Message** : Pool members are missing.

Indiquez des membres de pool.

## pool_missing_name
**Message** : Pool name is missing.

La zone de nom du pool est requise.

## pool_missing_protocol
**Message** : Pool protocol is missing.

La zone de protocole du pool est requise. Indiquez le protocole du pool dans votre demande. Les valeurs admises sont `http` et `tcp`.

## pool_not_found
**Message** : The pool with ID `<pool_id>` cannot be found.

Indiquez un ID de pool existant.

## pool_over_quota
**Message** : Pool cannot be created. Quota of pools for the load balancer resource has reached maximum limit.

Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## public_gateway_in_use
**Message** : Cannot delete a public gateway when it is in use.

Actuellement, la passerelle publique est connectée à un ou plusieurs sous-réseaux. Vous devez déconnecter la passerelle publique de tous les sous-réseaux pour pouvoir la supprimer.

Pour voir les sous-réseaux qui utilisent la passerelle publique, utilisez l'API `GET /v1/subnets?version=2019-05-31&generation=1`. Commande de l'interface de ligne de commande équivalente : `ibmcloud is subnets`. Pour déconnecter la passerelle publique du sous-réseau, utilisez la commande d'API `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` ou la commande de l'interface de ligne de commande `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
**Message** : Too many requests within a short time.

Ce message d'erreur est renvoyé si un nombre trop élevé de demandes est reçu dans un intervalle de temps spécifié. Attendez un peu, puis réessayez.

## security_group_active_transactions
**Message** : The interface cannot be attached or detached until the instance appears in Active state.

L'instance doit être active pour que ses interfaces réseau puissent être associées à un groupe de sécurité. Utilisez la commande `GET /v1/instances/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance` pour vérifier le statut de l'instance. Une fois que le statut est `available`, réessayez. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
**Message** : The interface is already attached to the security group.

L'interface est déjà associée au groupe de sécurité. Utilisez la commande `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` pour afficher les interfaces connectées.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_exists
**Message** : The security group already exists.

Les noms de groupe de sécurité doivent être uniques dans un VPC. Un groupe de sécurité de ce nom existe déjà dans le VPC ciblé. Utilisez la commande `GET /v1/security_groups?version=2019-05-31&generation=1` ou `ibmcloud is security-groups` pour afficher les groupes de sécurité actuels.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic) ou le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_attached
**Message** : Cannot delete the security group while interfaces are attached.


Détachez toutes les interfaces avant de supprimer les groupes de sécurité. Utilisez la commande `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interface-remove` pour déconnecter une interface.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic) ou le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Message** : Exceeded limit of interfaces per security group.

L'association d'une autre interface au groupe de sécurité entraînerait un dépassement du nombre maximal d'interfaces par groupe de sécurité. Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evaluez votre stratégie et envisagez de créer un autre groupe de sécurité avec des règles similaires.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic) ou le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_last_security_group_is_default
**Message** : The default security group cannot be removed when it is the only security group attached.

Une interface réseau doit être associée à un groupe de sécurité au moins.
Elle est associée au groupe de sécurité par défaut du cloud privé virtuel si aucun groupe de sécurité n'est spécifié.
Associez l'interface à un autre groupe de sécurité, puis essayez à nouveau de la dissocier du groupe de sécurité par défaut.
Pour des informations sur le groupe de sécurité par défaut, voir le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
**Message** : Exceeded security group limit.

Vous avez tenté de créer un groupe de sécurité, mais vous avez déjà atteint le quota défini pour votre compte. Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evaluez votre stratégie d'affectation d'instances à des groupes de sécurité. Souvent, il est possible de réduire le nombre global de groupes de sécurité en affectant plusieurs instances à un même groupe de sécurité. Cette stratégie permet de réduire le nombre de groupes de sécurité et de passer sous le quota défini pour votre compte. Rarement, en général pour les grandes organisations, il est nécessaire d'augmenter le quota. Dans ce cas, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) pour savoir si l'opération est possible.

## security_group_network_interface_not_active
**Message** : The interface cannot be attached because it is not active.

Les interfaces réseau doivent être actives pour pouvoir être associées à des groupes de sécurité. Utilisez la commande `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance-network-interface` pour afficher le statut d'une interface. Une fois que le statut est 'available', réessayez. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
**Message** : The interface is not attached.

L'interface n'est pas associée à un groupe de sécurité. Utilisez la commande `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` pour afficher les interfaces connectées.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic) ou le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_not_in_vpc
**Message** : The interface and security group are in different VPCs.

Pour que vous puissiez associer une interface réseau à un groupe de sécurité, l'instance doit se trouver dans le même cloud privé virtuel que le groupe de sécurité. Utilisez la commande `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` ou `ibmcloud is security-group` pour afficher les détails et VPC des groupes de sécurité. Utilisez la commande `GET /v1/instances/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance` pour afficher les détails et VPC de l'instance.

## security_group_order_bindings
**Message** : Cannot delete the security group while it has pending instance orders.

Si une instance a été créée avec un ou plusieurs groupes de sécurité spécifiés pour les interfaces réseau, l'instance et les interfaces réseau doivent être actives pour que le groupe de sécurité puisse être supprimé. Utilisez la commande `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` pour afficher les interfaces réseau connectées et leur statut. Une fois que le statut des interfaces est 'available', réessayez. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Message** : TCP/UDP max port cannot be less than min port.

La valeur de port maximale ne peut pas être inférieure à la valeur de port minimale. Spécifiez une valeur de port maximale supérieure à la valeur de port minimale.

## security_group_port_range_both_or_neither
**Message** : Port range must be unset, or both a minimum and maximum port must be set for 'tcp' and 'udp'.

Les règles associées à un protocole 'tcp' ou 'udp' peuvent ne pas avoir de plage de ports définie (pour qu'elles puissent être appliquées à l'intégralité du trafic) ou spécifier une plage de ports. Pour restreindre le trafic à un seul port, définissez la valeur de port comme valeur de port minimale et valeur de port maximale. Par exemple, la commande suivante crée une règle pour autoriser SSH sur le port 22 : `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`.

Pour plus d'informations sur les configurations de règles de groupe de sécurité, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic).


## security_group_port_range_invalid_protocol
**Message** : A port range was specified with a protocol of 'icmp'. A port range is only valid for a protocol of 'tcp' or 'udp'.

Les règles associées au protocole 'icmp' possèdent un type et un code. Les règles associées au protocole 'tcp' ou 'udp' possèdent une plage de ports. Pour plus d'informations sur les configurations de règles de groupe de sécurité, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_remote_group_not_in_vpc
**Message** : The remote group is not in the same VPC as this security group.

Les règles de groupe de sécurité peuvent faire référence à un groupe de sécurité distant et permettre le trafic entre les interfaces associées des deux groupes. Les groupes de sécurité distants doivent se trouver dans le même cloud privé virtuel. Utilisez `GET /v1/security_groups?2019-01-01` ou `ibmcloud is security-groups` pour afficher les groupes de sécurité en cours et leurs clouds privés virtuels.

Pour plus d'instructions relatives à la résolution de ce problème, voir le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Message** : Cannot delete the security group while remoting rules are attached.

Le groupe de sécurité contient une ou plusieurs règles qui référencent un groupe de sécurité distant. Utilisez la commande `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` ou `ibmcloud is security-group-rules` pour afficher les règles. La zone 'remote' spécifie les groupes de sécurité distants. Réessayez après avoir supprimé ou édité la règle d'accès distant. 

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic) ou le document [Utilisation de groupes de sécurité](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

## security_group_remoting_rules_per_sg_exceeded
**Message** : Exceeded limit of remoting rules per security group.

L'ajout d'une autre règle faisant référence à un groupe de sécurité distant entraînerait le dépassement du nombre maximal de règles d'accès distant par groupe de sécurité. Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.

## security_group_rules_per_sg_exceeded
**Message** : Exceeded limit of rules per security group.

L'ajout d'une règle entraînerait le dépassement du nombre maximal de règles par groupe de sécurité. Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evaluez votre stratégie et envisagez de créer un autre groupe de sécurité.

## security_group_sgs_per_interface_exceeded
**Message** : Exceeded limit of security groups per interface.

L'association de cette interface à un autre groupe de sécurité entraînerait le dépassement du nombre maximal de groupes de sécurité par interface. Les quotas par ressource sont indiqués dans la rubrique [Quotas et limites pour VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Evaluez votre stratégie et envisagez d'ajouter des règles à un groupe de sécurité existant. 


## security_group_type_code_invalid_protocol
**Message** : An 'icmp' type/code was given, but the requested protocol was not 'icmp'. Set the protocol to 'icmp' or specify a port range.

Seules les règles associées au protocole 'icmp' possèdent un type et un code. Les règles associées au protocole 'tcp' ou 'udp' possèdent une plage de ports. Pour plus d'informations sur les configurations de règles de groupe de sécurité, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_vpc_default
**Message** : Cannot delete the default security group for a VPC.

Le groupe de sécurité par défaut ne peut pas être supprimé. Pour des informations sur le groupe de sécurité par défaut, voir le guide relatif à la [mise à jour du groupe de sécurité par défaut](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
**Message** : None

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
**Message** : CIDR conflicts with existing Subnet in VPC.

Exécutez l'API `GET /subnets` pour extraire tous les sous-réseaux dans VPC. Vérifiez la valeur de `ipv4_cidr_block` pour vous assurer que le bloc CIDR que vous avez fourni n'est pas utilisé par d'autres sous-réseaux.

Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter `ibmcloud is subnets` et consulter la valeur de "Subnet CIDR" pour déterminer s'il existe des conflits.

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir de l'aide supplémentaire. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
**Message** : Cannot delete the subnet until it is empty. Delete any resources in the subnet and retry.

Une demande de suppression d'un sous-réseau a été émise, mais des ressources existent encore sur le sous-réseau en question. Les sous-réseaux peuvent héberger des instances, des réseaux privés locaux, des équilibreurs de charge ou des passerelles publiques. Vous devez supprimer les ressources qui se trouvent dans le sous-réseau pour pouvoir supprimer le sous-réseau. 

Dans certains cas, cette erreur peut survenir même si la console indique qu'il n'existe pas d'instance de serveur virtuel ni d'équilibreur de charge. La suppression est asynchrone et le changement du statut interne peut prendre quelques minutes. Attendez quelques minutes et essayez à nouveau de supprimer votre sous-réseau.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Message** : Cannot delete the subnet while it is attached to a public gateway. Detach the public gateway and retry.

Une demande de suppression d'un sous-réseau a été émise, mais une passerelle publique est encore connectée au sous-réseau. Vous devez supprimer ou déconnecter la passerelle publique pour pouvoir supprimer le sous-réseau. 

Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter `ibmcloud is public-gateways` pour répertorier les passerelles publiques et `ibmcloud is subnet-public-gateway-detach` pour détacher la passerelle publique d'un sous-réseau.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Message** : Cannot delete the subnet while it contains IP addresses. Please delete any server instance associated with the IP address and retry.

Une demande de suppression d'un sous-réseau a été émise, mais il existe encore des adresses IP sur le sous-réseau en question. Vous devez supprimer l'instance de serveur associée à l'adresse IP pour pouvoir supprimer le sous-réseau. 

Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter la commande `ibmcloud is instances` pour répertorier les instances de serveur. Examinez les valeurs "Address" pour déterminer son adresse IP et "Status" pour déterminer son statut. Exécutez la commande `ibmcloud is instance-delete` pour supprimer une instance de serveur dont le statut n'est pas déjà "deleting".

Dans certains cas, cette erreur peut survenir même si la console indique qu'il n'existe aucune instance de serveur virtuel (0 VSIs). La suppression est asynchrone et le changement du statut interne peut prendre quelques minutes. Attendez quelques minutes et essayez à nouveau de supprimer votre sous-réseau.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Message** : Cannot delete the subnet while it contains a load balancer. Delete the load balancer and retry.

Une demande de suppression d'un sous-réseau a été émise, mais il existe encore un équilibreur de charge sur le sous-réseau en question. Vous devez supprimer l'équilibreur de charge pour pouvoir supprimer le sous-réseau. 

Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter la commande `ibmcloud is load-balancers` pour répertorier les équilibreurs de charge. Examinez les valeurs "Subnets" pour déterminer son sous-réseau et "Status" pour déterminer son statut. Exécutez la commande `ibmcloud is load-balancer-delete` pour supprimer un équilibreur de charge dont le statut n'est pas déjà "deleting".

Dans certains cas, cette erreur peut survenir même si la console indique qu'il n'existe aucun équilibreur de charge. La suppression est asynchrone et le changement du statut interne peut prendre quelques minutes. Attendez quelques minutes et essayez à nouveau de supprimer votre sous-réseau.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Message** : Cannot delete the subnet while it contains a VPN gateway. Delete the VPN gateway and retry.

Une demande de suppression d'un sous-réseau a été émise, mais une passerelle VPN est encore associée au sous-réseau. Vous devez supprimer la passerelle VPN pour pouvoir supprimer le sous-réseau. 

Si vous utilisez l'interface de ligne de commande, vous pouvez exécuter la commande `ibmcloud is vpn-gateways` pour répertorier les passerelles VPN. Examinez les valeurs "Subnets" pour déterminer son sous-réseau et "Status" pour déterminer son statut. Exécutez la commande `ibmcloud is vpn-gateway-delete` pour supprimer une passerelle VPN dont le statut n'est pas déjà "deleting". 

Dans certains cas, cette erreur peut survenir même si la console indique qu'il n'existe aucune passerelle VPN. La suppression est asynchrone et le changement du statut interne peut prendre quelques minutes. Attendez quelques minutes et essayez à nouveau de supprimer votre sous-réseau.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_unknown_state
**Message** : The subnet `<subnet_id>` is in an invalid state for the requested operation.

Le sous-réseau doit avoir le statut `available` pour que vous puissiez vous en servir. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
**Message** : The target already has floating IP attached.

Une demande d'association d'adresse IP flottante à l'interface réseau d'un serveur a été émise, mais l'interface réseau est déjà associée à une adresse IP flottante.

Pour identifier l'adresse IP flottante qui est actuellement associée à une interface réseau, exécutez la commande d'API `GET /v1/floating_ips?version=2019-05-31&generation=1` et recherchez l'ID de l'interface réseau dans la zone `target.id`.  

Si vous utilisez l'interface de ligne de commande, exécutez la commande `ibmcloud is floating-ips` et examinez la valeur de `Target`. Notez que l'interface de ligne de commande tronque les ID d'interface dès le premier caractère tiret (“-“). Par exemple, l'interface réseau principale d'un serveur avec l'ID `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` apparaît ainsi : `primary(abdfcb29-.)` dans la sortie de l'interface de ligne de commande.

## token_invalid
**Message** : The service token was expired or invalid.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
**Message** : The service token was empty or did not exist in the request.

Recréez un jeton et réessayez.

## validation_enum
**Message** : The value supplied was not a valid option.

L'une des zones fournies possède une valeur qui doit provenir d'une énumération de valeurs possibles.

Pour les règles de liste de contrôle d'accès au réseau, vous pouvez spécifier une direction :

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

Par exemple, la valeur suivante n'est pas valide car `northbound` n'est pas une option valide dans l'énumération `[ inbound, outbound ]`.

```json
{
    "direction":"northbound"
}
```

Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} ou le guide de [référence de l'interface de ligne de commande](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window} pour obtenir les valeurs admises.

## validation_failure
**Message** : The JSON provided did not match the expected structure.

Pour résoudre ce problème, assurez-vous que le contenu de votre demande est au format JSON et que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_invalid_cidr
**Message** : The value is not a valid CIDR.

La valeur doit être un bloc CIDR interne valide avec une partie hôte 0.

Certaines plages d'adresses IP sont réservées. Vous trouverez plus d'informations sur les plages d'IP réservées dans la présentation de l'[utilisation de votre cloud privé virtuel avec des régions et des sous-réseaux](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Message** : The value is not a valid IPv4 CIDR.

Il doit s'agir d'un bloc CIDR interne IPv4 avec une partie hôte 0.

Certaines plages d'adresses IP sont réservées. Vous trouverez plus d'informations sur les plages d'IP réservées dans la présentation de l'[utilisation de votre cloud privé virtuel avec des régions et des sous-réseaux](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Message** : The value is not a valid IPv6 CIDR.

Actuellement, IPv6 n'est pas pris en charge. Utilisez une adresse IPv4.

## validation_invalid_address
**Message** : The value is not a valid address.

L'adresse IP doit être valide.

La liste des adresses IP réservées individuellement est indiquée dans le document relatif aux [aux régions et aux sous-réseaux](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv4_address
**Message** : The value is not a valid IPv4 address.

Indiquez une adresse IPv4 valide.

## validation_invalid_ipv6_address
**Message** : The value is not a valid IPv6 address.

Indiquez une adresse IPv6 valide. Actuellement, IPv6 n'est pas pris en charge ; utilisez une adresse IPv4.

## validation_invalid_field_type
**Message** : The value type does not match the field type.

Pour résoudre ce problème, assurez-vous que le contenu de votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_max_value
**Message** : A value provided for a parameter is larger than allowed.

Fournissez une valeur inférieure qui convienne à la valeur maximale indiquée par la [spécification](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_min_value
**Message** : A value provided for a parameter is smaller than allowed.

Vous pouvez obtenir cette erreur si vous tentez de créer un sous-réseau avec un nombre total d'adresses IPv4 (`total_ipv4_address_count`) inférieur à 8.

Indiquez une valeur supérieure qui convienne à la valeur minimale indiquée par la [spécification](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_not_null
**Message** : The field supplied must be null.

Assurez-vous que la valeur est null. Consultez la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

Exemple non valide :

```json
{
    "name": null
}
```

## validation_only_one
**Message** : The value must match one of the subschemas.

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_discriminator_forbidden
**Message** : The discriminator field forbids this substructure.

Par exemple, si vous spécifiez
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

le protocole est `icmp`, et _port_min_ est une zone `tcp` ; par conséquent, une erreur est générée.
1. validation_discriminator_required pour les règles `icmp` manquantes
2. validation_discriminator_forbidden pour les zones `tcp` avec `icmp` spécifié

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
**Message** : None

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
**Message** : The value is not a valid name.

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
**Message** : The value is not a valid HREF.

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
**Message** : None

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
**Message** : Missing a required field.

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
**Message** : The field supplied must be unique.

Il se peut que le nom que vous avez spécifié soit déjà utilisé. Essayez une autre valeur.

Vérifiez que votre demande est conforme à la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Message** : The volume attachment cannot be deleted.

Cette opération n'est pas autorisée. La connexion de volume du volume d'amorçage ne peut pas être supprimée car le volume d'amorçage est requis par l'instance.

## volume_attachment_update_invalid_request
**Message** : The volume attachment could not be updated because the request is invalid.

La connexion de volume n'a pas pu être mise à jour pour l'une des raisons suivantes :

* La connexion de volume du volume d'amorçage n'a pas pu être renommée
* La zone `delete_volume_on_instance_delete` de la connexion de volume du volume d'amorçage ne peut pas être définie avec la valeur `true`

## volume_capacity_max
**Message** : The maximum volume capacity allowed is 2000 GB.

La capacité du volume que vous avez spécifiée dépasse la capacité de volume maximale. Indiquez une valeur comprise entre 10 et 2000 Go. Voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) pour obtenir les plages de capacité admises pour un profil personnalisé.

## volume_capacity_missing
**Message** : The required volume capacity value is missing in the request.

Lorsque vous créez un volume, vous devez indiquer une capacité de volume basée sur la définition de ce profil. Pour plus d'informations, voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_capacity_zero_or_negative
**Message** : The volume capacity should be greater than zero.

Lors de la création d'un volume, la valeur de la capacité spécifiée dans la demande doit être un nombre positif compris entre 10 Go et 2000 Go. Voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) pour obtenir les valeurs de capacité de volume prises en charge.

## volume_crn_account_id_mismatch
**Message** : The CRN specified in the request does not belong to current user's account.

Le CRN de la clé racine Key Protect ne correspond pas à l'ID de votre compte d'autorisation IAM. Indiquez un autre CRN de clé racine Key Protect pour votre compte IAM. Voir la documentation de [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) pour plus d'informations.

## volume_crn_cname_mismatch
**Message** : The CRN specified in the request cannot be used for the current environment.

La valeur CRN que vous avez indiquée dans la demande ne peut pas être utilisée dans l'environnement actuel. Fournissez une valeur CRN applicable à votre environnement.

## volume_encryption_key_crn_invalid
**Message** : The volume encryption key CRN specified in the request is not valid.

La valeur CRN Key Protect que vous avez fournie n'est pas valide. Obtenez la valeur CRN de la clé racine dans le
service Cloud Identity and Access Management. Pour plus d'informations, voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption).

## volume_encryption_key_not_active
**Message** : The volume encryption key specified in the request is not active.

Activez la clé existante ou fournissez une nouvelle clé. Si vous ne pouvez pas activer votre propre clé, contactez votre administrateur système ou le [service clients](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
**Message** : You are not authorized to access the provided volume encryption key.

Fournissez une autre clé de chiffrement à laquelle vous avez accès ou contactez votre administrateur système pour obtenir l'accès à la clé actuelle.

## volume_encryption_key_not_implemented
**Message** : The volume encryption key feature is not supported.

Impossible de créer un volume à l'aide de votre clé de chiffrement. Cette version ne prend en charge que les volumes avec un chiffrement géré par le fournisseur. Pour utiliser votre propre clé de chiffrement, utilisez une version de Block Storage for VPC prenant en charge le chiffrement géré par le client.
Contactez le [service clients](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) pour plus d'informations.

## volume_id_invalid
**Message** : The volume ID specified in the request parameter is not valid.

L'ID de volume que vous avez indiqué n'est pas au format correct. Vérifiez que vous avez saisi l'ID de volume correctement et réessayez. Si vous n'êtes pas sûr de l'ID de volume, spécifiez `ibmcloud is volumes` dans l'interface de ligne de commande ou `GET volumes` dans l'API et recherchez-le dans la liste des volumes.

## volume_id_missing
**Message** : The volume ID is missing in the request parameter.

Vous devez indiquer l'ID de volume dans le paramètre de la demande lorsque vous obtenez un volume spécifique.

## volume_id_not_found
**Message** : Volume not found. Volume ID `<volume_id>` does not exist, where `<volume_id>` is the provided volume ID.

L'ID de volume indiqué n'existe pas. Fournissez un ID de volume valide.

## volume_iops_zero_or_negative
**Message** : The volume IOPS should be greater than zero.

Lorsque vous créez un volume, la valeur d'E-S/s (IOPS) indiquée dans la demande doit être un nombre positif. Voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) pour obtenir les valeurs d'E-S/s prises en charge.

## volume_name_duplicate
**Message** : The volume name is a duplicate. Volume name `<volume_name>` provided in the request already exists, where `<volume_name>` is the provided volume name.

Le nom de volume indiqué dans la demande existe déjà. Fournissez un nom de volume qui n'est pas utilisé actuellement.

## volume_not_available
**Message**: The Volume is not available. Volume can only be modified in available status. Current volume `<volume_id>` status is `<volume_status>`, where `<volume_id>` is the volume ID provided in the request and `<volume_status>` is the current volume status.

Un volume ne peut être modifié que lorsque son statut correspond à `available` (disponible). Vérifiez que le volume est disponible avant de le modifier.

## volume_not_deletable
**Message** : Delete failed.

Le volume ne peut être supprimé que s'il a le statut `available` ou `failed`. Vérifiez que le volume a un statut valide pour être supprimé.

## volume_not_found
**Message** : A volume with the specified ID is not found.

Vérifiez que l'ID de volume que vous avez entré est correct et réessayez. Pour obtenir la liste des volumes, spécifiez `ibmcloud is volumes` dans l'interface de ligne de commande ou `GET volumes` dans l'API.

## volume_not_implemented
**Message** : The requested operation is not supported in this release.

Contactez le [service clients](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) pour plus d'informations.

## volume_profile_capacity_iops_invalid
**Message** : The volume profile specified in the request is not valid for the provided capacity and/or IOPS.

La capacité du volume et/ou la valeur d'E-S/s que vous avez indiquées dans la demande ne sont pas autorisées par le profil du volume. Voir la documentation d'[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) pour obtenir les valeurs minimale et maximale, ainsi que les valeurs d'E-S/s valides pour un profil personnalisé.

## volume_profile_iops_invalid
**Message** : The volume profile specified in the request cannot accept custom IOPS.

Votre profil de volume ne prend pas en charge une valeur d'E-S/s personnalisée. Vous devez disposer d'un profil d'E-S/s hiérarchisé, ce qui ne nécessite pas la spécification d'une valeur d'E-S/s. Pour fournir une valeur d'E-S/s spécifique, utilisez le profil d'E-S/s personnalisé (Custom IOPS).

## volume_profile_name_missing
**Message** : The required volume profile name is missing in the request.

Le nom de profil du volume est manquant dans le corps de la demande lors de la création d'un volume ou dans le paramètre de la demande lors de l'obtention d'un volume. Fournissez un nom de profil de volume dans votre demande et réessayez. Si vous ne connaissez pas le nom de profil du volume, spécifiez `ibmcloud is volume-profiles` dans l'interface de ligne de commande ou utilisez `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` dans la demande d'API et effectuez une recherche dans la liste.

## volume_profile_not_found
**Message**: A volume profile with the specified name is not found.

Impossible de trouver un profil de volume de ce nom. Vérifiez que vous avez fourni le nom de profil de volume correct. Si vous ne connaissez pas le nom de profil du volume, spécifiez `ibmcloud is volume-profiles` dans l'interface de ligne de commande ou utilisez `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` dans la demande d'API et effectuez une recherche dans la liste.

## volume_quota_reached
**Message** : The volume quota has been reached.

Vous dépassez le quota prévu pour le compte actuel. Essayez de supprimer des volumes ou contactez le [service clients](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) pour augmenter votre quota. Voir la documentation pour obtenir des informations sur les [quotas applicables à l'infrastructure Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
**Message** : The resource group ID specified in the request is not valid.

L'ID du groupe de ressources que vous avez indiqué dans la demande n'est pas valide. Vérifiez que l'ID du groupe de ressources est correct. Dans l'interface de ligne de commande, utilisez la commande `ibmcloud is volume VOLUME_ID`. Les informations obtenues incluront le groupe de ressources et l'ID du groupe de ressources. Pour plus d'informations, voir [Référence de l'interface de ligne de commande d'IBM Cloud CLI for VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_resource_group_not_authorized
**Message** : The current action is not authorized on the specified resource group.

Vous n'êtes pas autorisé à répertorier ou créer des volumes dans le groupe de ressources spécifié. Utilisez un autre groupe de ressources auquel vous avez accès ou demandez à votre administrateur les droits nécessaires pour répertorier et créer des privilèges sur le groupe de ressources.

## volume_resource_group_not_found
**Message** : A resource group with the specified ID could not be found.

L'ID du groupe de ressources est introuvable parce que vous l'avez mal indiqué ou qu'il n'existe pas. Vérifiez que l'ID du groupe de ressources est correct. Dans l'interface de ligne de commande, utilisez la commande `ibmcloud is volume VOLUME_ID`. Les informations obtenues incluront le groupe de ressources et l'ID du groupe de ressources. Pour plus d'informations, voir [Référence de l'interface de ligne de commande d'IBM Cloud CLI for VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage).

## volume_start_not_found
**Message** : A volume with the ID specified as the page start parameter is not found.

L'ID de volume que vous avez indiqué dans le paramètre de début de l'appel pour obtenir la liste des volumes est introuvable. Vérifiez que l'ID de volume est correct et si vous avez accès au volume.

## volume_status_not_available
**Message** : The volume with the specified ID is not available.

Le volume n'est pas à l'état Available (disponible), il doit avoir être à l'état Pending (en attente). Attendez que le volume soit disponible et réessayez. Si le volume est supprimé, utilisez un autre volume. Pour vérifier le statut du volume, spécifiez `ibmcloud is volume VOLUME_ID` dans l'interface de ligne de commande ou utilisez `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` dans la demande d'API.

## volume_still_attached
**Message** : The volume is still attached to an instance.

Vous ne pouvez pas supprimer un volume qui est toujours connecté à une instance de serveur virtuel. Si vous définissez la suppression automatique du volume, attendez que l'instance de serveur virtuel soit supprimée et le volume sera déconnecté et supprimé. Si vous n'avez pas défini la suppression automatique, déconnectez le volume manuellement.

## volume_template_invalid
**Message** : Invalid volume template provided.

Vous avez fourni un modèle de volume non valide pour la création d'un volume. Voir [Référence d'API VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour vérifier les paramètres à fournir dans le corps de la demande.

## volume_update_invalid_request
**Message** : The volume update request is not valid.

La propriété du corps de la demande de mise à jour que vous avez indiquée n'est pas valide. Voir [Référence d'API VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} pour obtenir des informations et la syntaxe correcte pour la demande d'API.

## vpc_not_empty
**Message** : The VPC cannot be deleted because it is not empty.

Toutes les ressources dans le cloud privé virtuel (VPC) doivent être supprimées pour pouvoir supprimer le VPC. Un VPC contient des instances, des sous-réseaux, des passerelles publiques, des équilibreurs de charge et des passerelles VPN, et certaines de ces ressources contiennent aussi des ressources. Consultez la documentation sur la [suppression d'un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) pour obtenir des détails.

Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
**Message** : The resources are in different VPCs.

Les ressources doivent se trouver dans le même cloud privé virtuel (VPC). Par exemple, si vous essayez de connecter une passerelle publique à un sous-réseau, cette passerelle doit figurer dans le même VPC que le sous-réseau.

Pour déterminer le VPC qui contient la passerelle publique, exécutez la commande `GET /public_gateways/{id}` et examinez la valeur "vpc". La commande de l'interface de ligne de commande équivalente est `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
**Message** : The `<cidr_type>` CIDR blocks have duplicated CIDR blocks `<cidr_blocks>`.

Des blocs CIDR en double ont été fournis lors de la création de la connexion. Vérifiez que les blocs CIDR sont uniques.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si le problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_created
**Message** : A CIDR block could not be added to the VPN connection `<vpn_connection_id>`.

Fournissez un bloc CIDR valide qui satisfait aux exigences fournies par la spécification.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Message** : A CIDR block could not be deleted from the VPN connection `<vpn_connection_id>`.

Indiquez un bloc CIDR valide qui existe dans la connexion. Une connexion doit posséder au moins un bloc CIDR local et un bloc CIDR homologue.

Pour afficher les blocs CIDR de la connexion, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez les zones `local_cidrs` et `peer_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**Message** : The CIDR block was not found in the VPN connection `<vpn_connection_id>`.

Fournissez un bloc CIDR valide qui se trouve dans la connexion.

Pour afficher les blocs CIDR de la connexion, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez les zones `local_cidrs` et `peer_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**Message** : A CIDR block could not be updated in the VPN connection `<vpn_connection_id>`.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
**Message** : The CIDR block `<cidr_block>` does not represent a valid address.

La valeur doit être un bloc CIDR interne valide avec aucun bit d'hôte défini.

Pour afficher les blocs CIDR de la connexion, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez les zones `local_cidrs` et `peer_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**Message** : The CIDR block `<cidr_block_1>` overlaps with `<cidr_block_2>`. Two peer CIDR blocks cannot overlap on the same VPC and two local CIDR blocks cannot overlap on the same connection.

Fournissez un bloc CIDR valide qui satisfait aux exigences fournies par la spécification.

Pour afficher les blocs CIDR de la connexion, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez les zones `local_cidrs` et `peer_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Message** : The name `<vpn_connection_name>` is already in use by VPN connection `<vpn_connection_id>`.

Indiquez un autre nom de connexion. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
**Message** : The name `<vpn_connection_name>` is not a valid VPN connection name.

Un nom de connexion valide commence par une lettre, suivie d'autres lettres, de chiffres, de traits de soulignement ou de traits d'union.

## vpn_connection_invalid_psk_format
**Message** : The pre-shared key (PSK) `<psk>` is not in the valid format.

Une clé pré-partagée valide contient entre 6 et 128 caractères, qui peuvent être des lettres, des chiffres, et les signes `-`,`+`,`&`,`!`,`@`,`#`,`$`,`%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Message** : At least one local CIDR block is required when creating a VPN connection.

Fournissez un bloc CIDR local valide qui satisfait aux exigences fournies par la spécification.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Message** : Local subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les sous-réseaux locaux actuels sur les connexions VPN, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections` et vérifiez la zone `local_cidrs` pour chaque connexion.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Message** : Local subnets for the VPN connection has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}.

Pour afficher les sous-réseaux locaux actuels d'une connexion VPN, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez la zone `local_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**Message** : The VPN connection `<vpn_connection_id>` could not be created.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
**Message**: The VPN connection `<vpn_connection_id>` could not be deleted.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_found
**Message**: The VPN connection `<vpn_connection_id>` could not be found.

Indiquez si l'ID de connexion est correct. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
**Message** : The VPN connection `<vpn_connection_id>` could not be updated.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
**Message** : At least one pair of local CIDR and peer CIDR with the same peer gateway has been duplicated.

Il existe déjà une autre connexion avec un routage CIDR local et un routage CIDR homologue correspondant sur cette passerelle. Fournissez un ensemble de CIDR local/homologue valide.

## vpn_connection_peer_cidrs_required
**Message** : At least one peer CIDR block is required when creating a VPN connection.

Fournissez un bloc CIDR homologue valide qui satisfait aux exigences fournies par la spécification.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Message** : Peer subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les sous-réseaux homologues actuels sur les connexions VPN, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections` et vérifiez la zone `peer_cidrs` pour chaque connexion.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Message** : Peer subnets for the VPN connection has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les sous-réseaux homologues actuels d'une connexion VPN, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` et vérifiez la zone `peer_cidrs`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**Message** : Failed to add a static route for the peer CIDR block `<peer_cidr>`.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Message** : Failed to remove a static route for the peer CIDR block `<peer_cidr>`.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitted
**Message** : CIDR blocks cannot be changed when updating a connection. Please use the correct API when creating or deleting CIDR blocks.

Fournissez une valeur de demande valide qui satisfait aux exigences fournies par la spécification.

Pour plus d'instructions relatives à la résolution de ce problème, voir la [documentation d'API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connections_quota_exceeded
**Message** : The VPN connection cannot be created because the VPN gateway `<vpn_gateway_id>` has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les connexions VPN actuelles d'une passerelle VPN, utilisez l'API `GET /vpn_gateways/<vpn_gateway_id>/connections`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**Message** : The name `<vpn_gateway_name>` is already in use by VPN gateway `<vpn_gateway_id>`.

Indiquez un autre nom de passerelle VPN. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
**Message** : The VPN gateway could not be initialized.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
**Message** : The name `<vpn_gateway_name>` is not a valid VPN gateway name.

Un nom de passerelle VPN valide commence par une lettre, suivie d'autres lettres, de chiffres, de traits de soulignement ou de traits d'union.

## vpn_gateway_invalid_state
**Message** : The VPN gateway `<vpn_gateway_id>` is in an invalid state for the requested operation.

La passerelle VPN doit être associée au statut `available` pour que vous puissiez vous en servir. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
**Message** : Unable to create an IP address for the VPN gateway.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
**Message** : Could not find a subnet identifier for the given VPN gateway template.

La zone **Subnet ID** est requise. Fournissez un ID de sous-réseau avec votre commande.

## vpn_gateway_missing_name
**Message** : Could not find a name for the given VPN gateway template.

La zone **name** est requise. Indiquez un nom avec votre commande.

## vpn_gateway_not_created
**Message** : The VPN gateway `<vpn_gateway_id>` could not be created.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
**Message** : The VPN gateway `<vpn_gateway_id>` could not be deleted.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_found
**Message** : The VPN gateway `<vpn_gateway_id>` could not be found.

Vérifiez que l'ID de passerelle VPN est correct. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
**Message** : The VPN gateway `<vpn_gateway_id>` could not be updated.

Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_found
**Message** : Could not find the subnet `<subnet_id>` for the given VPN gateway.

Vous avez référencé un sous-réseau qui n'existe pas. Vérifiez votre demande pour vous assurer que vous avez spécifié l'ID de sous-réseau approprié. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Message** : The VPN gateway could not be created due to subnet status.

Indiquez un sous-réseau dont le statut est `available`. Réessayez dans quelques minutes. Si ce problème persiste, [contactez le support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Message** : The VPN Gateway cannot be created because your account and/or the region has reached the quota.

Les quotas par ressource sont indiqués sur la page [Quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Pour afficher les passerelles VPN en cours, utilisez l'API `GET /vpn_gateways`.
Commande de l'interface de ligne de commande équivalente : `ibmcloud is vpn-gateways`

## zone_inconsistency
**Message**: The resources must belong to the same zone.

* Lorsque vous connectez un volume, assurez-vous que le volume et l'instance sont situés dans la même zone.
* Lorsque vous associez une adresse IP flottante, assurez-vous que l'adresse IP flottante et le sous-réseau sont situés dans la même zone.
