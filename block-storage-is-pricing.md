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

# Pricing for Block Storage for VPC
{: #block-storage-pricing}

Pricing for {{site.data.keyword.block_storage_is_short}} is based on the capacity or the IOPS that was provisioned, depending on which storage profile you chose.

| IOPS Tier  | Published Monthly rate^1 |
|------------|--------------|
|  3 IOPS/GB |  0.13 USD/GB |
|  5 IOPS/GB |  0.25 USD/GB |
| 10 IOPS/GB |  0.58 USD/GB |
| Custom IOPS| 0.10 USD/GB + 0.07 USD/IOPS |

^1 Published price per month, [calculated hourly](#how-are-charges-calculated).

## How are charges calculated?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} is calculated hourly, based on the total number of hours that the block storage volume exists on the account, until the volume is deleted or the end of a billing cycle, whichever comes first. Hourly billing is calculated differently for a predefined IOPS tier than for custom IOPS. See the following examples.

### Billing calculation for a volume with the general-purpose IOPS tier
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

You provision a 1000-GB volume with the general-purpose 3 IOPS/GB tier, then use the volume for 72 hours before you delete it. The total price for the volume is billed by the hour, as follows:

((1000 GB x 0.13 USD/GB)/730 hrs) x 72 hrs = $12.82

### Billing calculation for a volume with custom IOPS
{: #billing-calculation-for-a-volume-with-custom-IOPS}

You provision a 1000-GB volume with 2500 IOPS, then use the volume for 72 hours before you delete it. The total price for the volume is billed by the hour, as follows:

(((1000 x 0.10 USD/GB) + (2500 x 0.07 USD)) / 730 hrs) x 72 hrs = $27.12
