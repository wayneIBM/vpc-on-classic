---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: vpc, pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# Pricing for VPC
{: #pricing-for-vpc}

The table summarizes the pricing for internet data transfer with {{site.data.keyword.cloud}} Virtual Private Cloud. Remember that there is no charge for traffic between VPC and other Classic IBM Cloud services. Also, for PayGo services, the service tiers are bound to your account, not to any specific VPC instance. Amounts are shown in U.S. dollars.

Separate pricing applies for [virtual server instances](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) and [block storage](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) used within your IBM Cloud VPC.

## Free allowances for internet data transfer
{: #free-allowances-for-internet-data-transfer}

| Data transfer |  Cost for all IBM Cloud VPC Customers |
|---------------|------------------|
| Within the zone | Free |
| Between zones in the same region | Free |
| Use of public gateway | Free (Charged only for the floating IP used by the PGW) |

## Floating IP pricing
{: #floating-ip-pricing}

A floating IP is charged at the rate of $1 (U.S.) per month, starting when it is reserved. The fee is charged even if the floating IP is not associated to a VSI or not in use. The $1 for the monthly fee is charged even if the floating IP is reserved for only a few days.

## Basic PayGo pricing for internet data transfer
{: #basic-paygo-pricing-for-internet-transfer}

| Data transfer | Amount of data | PayGo pricing |
|-----------|-----------|------------------|
| Egress to Internet |  0 to 5 GB | Free |
|  | 6 to 10,000 GB | $0.087 per GB |
|  | 10,001 to 50,000 GB | $0.083 per GB |
|  | 50,001 to 150,000 GB | $0.07 per GB |
|  | 150,001 GB and over | $0.05 per GB |


When you create a new VPC, it may take up to an hour for initial billing charges to appear in the Console UI or API.
{: tip}

If you have a public gateway or Floating IP, you may still see some minimal egress charges, even if you have not sent out any egress traffic during that time. These charges are for ARP traffic, which is necessary to operate your account.
{: important}

## LBaaS for VPC pricing
{: #lb-for-vpc-pricing}

Load Balancers for VPC pricing is based on the following metrics, calculated monthly:
* Service Usage Hours
* Data Processed


| Region | LB service usage (per hour) | LB data processed per GigaByte (GB) |
|------------|--------------------------|-------------------------|
| Dallas | $0.025 | $0.008 |
| Frankfurt | $0.027 | $0.009 |
| Tokyo | $0.028 | $0.009 |

Prices vary by multi-zone regions.
{: note}

The following chart shows an example for a customer using 4500 GB per month for public load balancing, in U.S. dollars, based in the Dallas region.

| Item | Usage | Rate | Cost |
|---------|--------|---------|---------|          
| LB Service Usage| 720 hours| $0.025/hour | $18 per month |
| Data Processed | 4500GB  |  $0.008/GB | $36 per month|

**Table 1: Monthly Cost Example. The total charge for this scenario is $54 (USD) per month.**


## VPN for VPC pricing
{: #vpn-for-vpc-pricing}

| Region | Connection (Peer) per hour | Instance (Gateway) per hour |
|------------|--------------------------|-------------------------|
| Dallas | $0.04 | $0.045 |
| Frankfurt | $0.044 | $0.0495 |
| Tokyo | $0.0452 | $0.0585 |

Data transfer to the internet as a result of using VPNaaS is charged as regular data transfer, outbound to the internet, billed at the VPC level.
{: note}
