---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# Tarification de VPC
{: #pricing-for-vpc}

Le tableau récapitule la tarification des transferts de données Internet avec {{site.data.keyword.cloud}} Virtual Private Cloud. Notez que le trafic entre VPC et d'autres services IBM Cloud Classic n'est pas facturé. De plus, pour les services de paiement à la carte (PayGo), les niveaux de service sont liés à votre compte et non à une instance de VPC spécifique. Les montants sont affichés en dollars américains.

Une tarification distincte s'applique pour les [instances de serveur virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) et le [stockage par blocs](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) utilisés avec votre cloud privé virtuel IBM Cloud. 

## Franchises pour le transfert de données Internet
{: #free-allowances-for-internet-data-transfer}

| Transfert de données |  Coût pour tous les clients IBM Cloud VPC |
|---------------|------------------|
| Dans la zone | Gratuit |
| Entre les zones d'une même région | Gratuit |
| Utilisation de la passerelle publique | Gratuit (seule l'adresse IP flottante utilisée par la passerelle publique est facturée) |

## Tarification des adresses IP flottantes
{: #floating-ip-pricing}

Une adresse IP flottante est facturée 1 dollar (américain) par mois, à partir de sa réservation. Ces frais sont facturés même si l'adresse IP flottante n'est pas associée à une instance de serveur virtuel ou si elle n'est pas utilisée. Les frais mensuels d'1 dollar sont facturés même si l'adresse IP flottante n'est réservée que pendant quelques jours.

## Tarification à la carte (PayGo) de base pour le transfert de données Internet
{: #basic-paygo-pricing-for-internet-transfer}

| Transfert de données | Quantité de données | Tarification PayGo |
|-----------|-----------|------------------|
| Sortie vers Internet |  0 à 5 Go | Gratuit |
|  | 6 à 10000 Go | 0,087 $ par Go |
|  | 10001 à 50000 Go | 0,083 $ par Go |
|  | 50001 à 150000 Go | 0,07 $ par Go |
|  | 150001 Go et plus | 0,05 $ par Go |


Lorsque vous créez un cloud privé virtuel, il peut s'écouler jusqu'à une heure avant que les frais facturés initiaux n'apparaissent dans l'interface utilisateur de la console ou l'API.
{: tip}

Si vous disposez d'une passerelle publique ou d'une adresse IP flottante, vous pouvez constater des frais de sortie minimaux, même si vous n'avez pas envoyé de trafic sortant. Ces frais concernent le trafic ARP (protocole de résolution d'adresse), qui est nécessaire pour faire fonctionner votre compte.
{: important}

## Tarification du service LBaaS for VPC
{: #lb-for-vpc-pricing}

La tarification des équilibreurs de charge pour VPC est basée sur les métriques suivantes, calculées tous les mois :
* Heures d'utilisation du service
* Données traitées


| Région | Utilisation du service d'équilibreur de charge (par heure) | Données d'équilibreur de charge traitées par gigaoctet (Go) |
|------------|--------------------------|-------------------------|
| Dallas | 0,025 $ | 0,008 $ |
| Francfort | 0,027 $ | 0,009 $ |
| Tokyo | 0,028 $ | 0,009 $ |

Les prix varient selon les régions à zones multiples.
{: note}

Le graphique suivant prend l'exemple d'un client utilisant 4500 Go par mois pour l'équilibrage de charge public, en dollars US, basé dans la région Dallas.

| Article | Utilisation | Taux | Coût |
|---------|--------|---------|---------|          
| Utilisation du service d'équilibreur de charge| 720 heures| 0,025 $/heure | 18 $ par mois |
| Données traitées | 4500 Go  |  0.,008 $/Go | 36 $ par mois|

**Tableau 1 : exemple de coûts mensuels. Le coût total de ce scénario s'élève à 54 $ (USD) par mois.**


## Tarification du service VPN for VPC
{: #vpn-for-vpc-pricing}

| Région | Connexion (homologue) par heure | Instance (passerelle) par heure |
|------------|--------------------------|-------------------------|
| Dallas | 0,04 $ | 0,045 $ |
| Francfort | 0,044 $ | 0,0495 $ |
| Tokyo | 0,0452 $ | 0,0585 $ |

Le transfert de données sur Internet résultant de l'utilisation de VPNaaS est facturé comme un transfert de données standard, en sortie vers Internet, au niveau du VPC.
{: note}
