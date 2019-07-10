---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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
{:faq: data-hd-content-type='faq'}


# Foires aux questions
{: #faqs}

## Puis-je connecter mon cloud privé virtuel à mes autres charges de travail IBM Cloud ?  
{: #faq-0}
{:faq}

Oui, vous pouvez configurer l'accès à votre infrastructure classique {{site.data.keyword.cloud}} à partir d'un cloud privé virtuel dans chaque région. Pour plus d'informations, voir [Configuration de l'accès à votre infrastructure classique](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Quel est le nombre maximal de caractères qu'un nom de cloud privé virtuel peut comporter ?
{: #faq-1}
{:faq}

Actuellement, la limite est de 100 caractères. Si elle est dépassée, il se peut que vous receviez un message de type "erreur interne".

## Un nom de ressource de cloud privé virtuel peut-il commencer par un chiffre ?
{: #faq-2}
{:faq}

Non, bien que le nom puisse contenir des chiffres, il doit commencer par une lettre.

## Existe-t-il des restrictions relatives aux caractères pouvant être utilisés dans un nom ?
{: #faq-3}
{:faq}

Oui, l'interface utilisateur n'admet pas que deux traits d'union, deux traits de soulignement ou deux points se suivent dans un nom d'instance de serveur virtuel.

## Est-il possible de créer une instance de serveur virtuel sans sous-réseau, uniquement avec une adresse IP flottante ?
{: #faq-9}
{:faq}

Non.

## Est-il possible d'associer une instance de serveur virtuel à plusieurs clouds privés virtuels ?
{: #faq-10}
{:faq}

Non.

## Est-il possible de changer la taille d'un sous-réseau après sa création dans un cloud privé virtuel ?
{: #faq-11}
{:faq}

Non.

## La bande passante sortante est-elle facturée pour le trafic entre VPC et COS ?
{: #faq-17}
{:faq}

La bande passante sortante n'est pas facturée tant que le noeud final de réseau de distribution d'applications est utilisé pour la connexion entre VPC et COS, mais les appels API sont facturés.

 ## Pourquoi dois-je spécifier un sous-réseau lorsque je configure mon réseau privé virtuel pour VPC, et que dois-je spécifier ?
{: #faq-16}
{:faq}

Lorsque vous configurez une passerelle VPN, vous devez spécifier un sous-réseau pour que cette passerelle puisse obtenir les adresses IP dont elle a besoin pour le service VPN. En spécifiant le sous-réseau, vous gardez le contrôle sur le mode de traitement de votre trafic VPN et pouvez spécifier un emplacement plus proche des ressources qui utilisent le réseau privé virtuel. Ainsi, vous pouvez réduire le temps d'attente et améliorer les performances.

## Comment puis-je savoir où mes instances d'équilibreur de charge seront déployées ?
{: #faq-18}
{:faq}

Les sous-réseaux choisis déterminent à quel endroit l'équilibreur de charge de cloud privé virtuel est déployé. L'équilibreur de charge de cloud privé virtuel est une ressource régionale. Il est recommandé de choisir des sous-réseaux se trouvant dans des zones différentes d'une région donnée afin d'accroître la redondance. 


## Au cours de l'affectation d'une adresse IP flottante dans un cloud privé virtuel, un client doit spécifier la carte d'interface réseau virtuel de l'interface de serveur virtuel, est-ce correct ?
{: #faq-4}
{:faq}

Oui. De plus, il doit s'agir de l'interface réseau principale d'un serveur.

Toutefois, si vous ajoutez une ressource d'adresse dans l'API, dans la zone de mise en réseau, vous pouvez tout à fait changer l'association de l'adresse IP flottante et l'associer à l'adresse plutôt qu'à l'interface.

## Une carte d'interface réseau virtuel dans une instance de serveur virtuel dans mon cloud privé virtuel peut-elle être associée à la fois à une adresse IP flottante et à une adresse IP privée ?
{: #faq-5}
{:faq}
 
Oui.

## Dans l'interface utilisateur de la console IBM Cloud pour VPC, il existe un bouton à bascule permettant de connecter chaque sous-réseau à la passerelle publique ou de le déconnecter de la passerelle publique. Qui crée la passerelle publique ? La passerelle publique sera-t-elle prête si un client souhaite lui associer le sous-réseau dans l'interface utilisateur ? 
{: #faq-6}
{:faq}

La passerelle publique doit être créée explicitement via l'API, car l'API requiert l'association explicite d'un sous-réseau à une passerelle publique particulière. 

## Imaginez que l'instance de serveur virtuel nommée VSI1 dans un cloud privé virtuel possède une seule carte d'interface réseau virtuel nommée vNIC1 et qu'elle est associée au sous-réseau nommé Sous-réseau1. Sous-réseau1 est associé à une passerelle publique. Un client peut-il affecter une adresse IP flottante à VSI1 ?
{: #faq-7}
{:faq}

Oui, un serveur peut se trouver sur un sous-réseau associé à une passerelle publique et posséder également une adresse IP flottante. L'affectation d'une adresse IP flottante à une instance de serveur virtuel ne dépend pas du fait qu'une passerelle publique soit associée ou non au sous-réseau. Une adresse IP flottante associée à une instance de serveur virtuel sera prioritaire sur la passerelle publique associée au sous-réseau.

## Dans mon cloud privé virtuel, l'instance de serveur virtuel nommée VSI1 possède deux cartes d'interface réseau virtuel nommées vNIC1 et vNIC2. Ces deux cartes d'interface réseau virtuel peuvent-elles être associées à un même sous-réseau ?
{: #faq-8}
{:faq}
 
Oui, vous pouvez associer à un même sous-réseau un nombre illimité d'interfaces réseau sur le même serveur. Toutefois, vous ne pouvez pas intégrer plusieurs cartes d'interface réseau dans une instance de serveur virtuel. 

## Qui décide qu'il ne doit exister qu'une seule passerelle publique par zone pour un cloud privé virtuel ?
{: #faq-13}
{: faq}

Cette limite est appliquée dans l'API VPC.

## Au cours de la création de la passerelle publique, dois-je réserver l'adresse IP flottante ou celle-ci est-elle réservée automatiquement par le système ? Cette adresse IP flottante apparaîtra-t-elle lorsque j'afficherai la liste de toutes les adresses IP flottantes ?
{: #faq-12}
{: #faq}

L'API crée automatiquement une adresse IP flottante avec la passerelle publique si aucune adresse IP flottante existante n'est spécifiée. Et oui, cette adresse IP flottante figurera dans la liste.


## Comment la technologie VRF (Routage et transfert virtuel) affecte-t-elle mes autres fonctions de mise en réseau et mes sous-réseaux ?
{: #faq-14}
{:faq}

Mises en garde relatives aux réseaux locaux virtuels et à la technologie VRF :

* La couverture par un réseau local virtuel de plusieurs comptes n'est pas admise dans l'environnement VRF.
* Le service de réseau privé virtuel IPSec est limité.

VRF n'empêche pas l'accès SSL ou PPTP au réseau privé virtuel, mais son comportement change. Sans VRF, une connexion VPN suffit pour voir tous les serveurs sur votre compte. Avec VRF, vous ne pouvez accéder qu'aux ressources présentes sur le marché qui sont associées à votre réseau privé virtuel. Par conséquent, si vous vous connectez au réseau privé virtuel DAL, vous ne pouvez vous connecter qu'aux serveurs DAL.
{: note}

## Quelle est la bonne façon d'utiliser la sous-commande `instance-network-interface-floating-ip-add` dans VPC ? Faut-il d'abord créer/allouer une adresse IP flottante ?
{: #faq-15}
{:faq}

 Vous devez d'abord allouer une adresse IP flottante, puis fournir l'adresse de l'IP flottante dans la commande `add` de l'adresse IP flottante de l'interface.

 Exemple : `ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`. Indiquez la zone.
