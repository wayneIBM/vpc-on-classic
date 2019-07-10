---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Tarification pour Block Storage for VPC
{: #block-storage-pricing}

La tarification pour {{site.data.keyword.block_storage_is_short}} est basée sur la capacité ou les opérations d'E-S/s mises à disposition, en fonction du profil de stockage que vous avez choisi.

| Niveau d'E-S/s  | Tarification mensuelle publiée^1 |
|------------|--------------|
|  3 E-S/s par Go |  0,13 USD/Go |
|  5 E-S/s par Go |  0,25 USD/Go |
| 10 E-S/s par Go |  0,58 USD/Go |
| E-S/s personnalisé| 0,10 USD/Go + 0,07 USD par E-S/s |

^1 Tarif mensuel publié, [calculé sur une base horaire](#how-are-charges-calculated).

## Comment sont calculés les frais ?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} est calculé à l'heure, en fonction du nombre total d'heures d'existence du volume de stockage par blocs dans le compte, jusqu'à ce que le volume soit supprimé ou jusqu'à la fin d'un cycle de facturation, selon ce qui survient en premier. La facturation à l'heure est calculée différemment selon qu'il s'agisse d'un niveau d'E-S/s prédéfini ou personnalisé. Consultez les exemples suivants.

### Calcul de la facturation pour un volume avec le niveau d'E-S/s general-purpose
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

Vous mettez à disposition un volume de 1000 Go avec le niveau d'E-S/s general-purpose (3 E-S/s par Go), puis vous utilisez le volume pour une durée de 72 heures avant de le supprimer. Le prix total du volume est facturé à l'heure, comme suit :

((1000 Go x 0,13 USD/Go)/730 heures) x 72 heures = 12,82 $

### Calcul de la facturation pour un volume avec un niveau d'E-S/s personnalisé
{: #billing-calculation-for-a-volume-with-custom-IOPS}

Vous mettez à disposition un volume de 1000 Go avec 2500 E-S/s, puis vous utilisez ce volume pour une durée de 72 heures avant de le supprimer. Le prix total du volume est facturé à l'heure, comme suit :

(((1000 x 0,10 USD/Go) + (2500 x 0,07 USD)) / 730 heures) x 72 heures = 27,12 $
