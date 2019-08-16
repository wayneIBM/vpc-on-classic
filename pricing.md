---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-08-15"

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

Pricing of IBM Cloud Virtual Private Cloud is applied separately for [internet data transfer](#pricing-for-data-transfer), [load balancers](#lb-for-vpc-pricing), [VPNs](#vpn-for-vpc-pricing), [virtual server instances](#pricing-for-virtual-servers-for-vpc), and [block storage](#pricing-for-block-storage-for-vpc) used within your IBM Cloud VPC. The following charges apply to your use of {{site.data.keyword.cloud}} Virtual Private Cloud. For PayGo services, the service tiers are bound to your account, not to any specific VPC instance. Amounts are shown in U.S. dollars.

## Pricing for internet data transfer with IBM Cloud™ Virtual Private Cloud
{: #pricing-for-data-transfer}

The table summarizes the pricing for internet data transfer with {{site.data.keyword.cloud}} Virtual Private Cloud. Remember that there is no charge for traffic within a VPC and other Classic IBM Cloud services (within the IBM data centers). 

### Free allowances for internet data transfer
{: #free-allowances-for-internet-data-transfer}

Traffic within your VPC is free as well as the use of public gateways. 

| Data transfer |  Cost for all IBM Cloud VPC Customers |
|---------------|------------------|
| Within the zone | Free |
| Between zones in the same region | Free |
| Use of public gateway | Free (Charged only for the floating IP used by the PGW) |

### Basic PayGo pricing for internet data transfer
{: #basic-paygo-pricing-for-internet-transfer}

For traffic leaving the VPC, the following pricing applies.

| Data transfer | Amount of data | PayGo pricing (usa)| PayGo pricing (eu-de)|PayGo pricing (eu-gb)|PayGo pricing (jp-tok)|PayGo pricing (au-syd)|
|-----------|-----------|------------------|------------------|------------------|------------------|------------------|
| Egress to Internet |  0 to 5 GB | Free |Free |Free |Free |Free |
|  | 6 to 10,000 GB | $0.087 per GB |$0.096 per GB |$0.093 per GB |$0.098 per GB |$0.104 per GB |
|  | 10,001 to 50,000 GB | $0.083 per GB |$0.091 per GB |$0.089 per GB |$0.094 per GB |$0.100 per GB |
|  | 50,001 to 150,000 GB | $0.07 per GB |$0.077 per GB |$0.075 per GB |$0.079 per GB |$0.084 per GB |
|  | 150,001 GB and over | $0.05 per GB |$0.055 per GB |$0.054 per GB |$0.057 per GB |$0.060 per GB |


When you create a new VPC, it may take up to an hour for initial billing charges to appear in the Console UI or API.
{: tip}

If you have a public gateway or Floating IP, you may still see some minimal egress charges, even if you have not sent out any egress traffic during that time. These charges are for ARP traffic, which is necessary to operate your account.
{: important}

### Pricing for Floating IPs
{: #floating-ip-pricing}

A floating IP is charged at the rate of $1 (U.S.) per month, starting when it is reserved. The fee is charged even if the floating IP is not associated to a VSI or not in use. The $1 for the monthly fee is charged even if the floating IP is reserved for only a few days.

## Pricing for Load Balancers for VPC 
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


## Pricing for VPN for VPC 
{: #vpn-for-vpc-pricing}

| Region | Connection (Peer) per hour | Instance (Gateway) per hour |
|------------|--------------------------|-------------------------|
| Dallas | $0.04 | $0.045 |
| Frankfurt | $0.044 | $0.0495 |
| Tokyo | $0.0452 | $0.0585 |

Data transfer to the internet as a result of using VPNaaS is charged as regular data transfer, outbound to the internet, billed at the VPC level.
{: note}


## Pricing for Virtual Servers for VPC
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (linked help topic)


{{site.data.keyword.vsi_is_full}} is offered in select regions with up to 62 vCPU and 248 GB RAM to fit any workload need. You're billed at an hourly rate only, with discounts applied the longer your instance is running. Virtual server usage times are calculated per second, for both the in use time and suspended time of your instance. For example, if your instance runs for 45 minutes and 32 seconds, you're billed for 45 minutes and 32 seconds.
{:shortdesc}


### Sustained usage
{: #sustained-usage}

While the instances are charged at an hourly rate, the longer your instance is running, the less expensive the rate is. As the billing month progresses, you receive the following hourly discounts.

| Time elapsed in a month       | Billing discount  | 
| ----------------------------- | ----------------- | 
| 0-20%                         | regular retail rate |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="Table 1. Tiered discounts" caption-side="top"}  

These discounted tiers provide you with a 10% savings for keeping instances running monthly versus hourly. This discount only applies to base hourly rates; it doesn't apply to software, storage, network, or other charges.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### Sustained usage example
{: #sustained-usage-example}

Say you purchase an instance from the Balanced virtual server family, with 16 CPUs and 64 GB RAM, at a base price of $0.795 per hour. As the month progresses, your rate decreases as follows:

| Time elapsed in a month       | Billing discount  |  Example rate     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | regular retail rate ($.795) | $116.07    |                
| 21-40%                        | 5%        |   $110.27   |                 
| 41-60%                        | 10%       |    $104.46  |            
| 61-80%                        | 15%        |    $98.66    |                
| 81-100%                       | 20% |       $92.86      |
{: caption="Table 2. Tiered discounts example" caption-side="top"}  

Your total bill, if left running for the entire month, with this model is $522.32. The discounts result in an overall monthly savings of 10%, compared to the hourly rate.

### Base instance prices
{: #base-instance-prices}

Base instance prices start at $0.087 per hour. When you create a virtual server, you are prompted to select a virtual server family and select a profile configuration. When you make your selection, the associated hourly rate is displayed in the table. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

### Included operating systems
{: #included-operating-systems}

The following operating systems are included free of charge:

* CentOS 7.latest
* Ubuntu 16.04 LTS, 18.04 (minimal)
* Debian 8.latest, 9.latest (minimal)

There are premium operating systems and other add-ons available. You'll see pricing reflected in your Cost Summary.

### Suspend billing
{: #suspend-billing}

When you power off an instance, you don't accrue costs for certain compute resources. Billing stops automatically when you stop the instance. The suspend billing feature helps you reduce cost and prevents you from having to re-create an instance when you need its resources again.

In situations where you want to scale your infrastructure up and down in response to workload needs, you can use the suspend billing feature as a faster alternative to creating and deleting instances.
{:tip}

### Billing details
{: #billing-details}

It's important to understand what costs stop accruing and what costs persist when your virtual server instance is powered off.

Billing is suspended only when you power off your virtual server instance through the IBM Cloud console, CLI, or API. If you power off your virtual server instance directly through the OS, billing isn't suspended for that instance.
{:note}

Review the following table for details on how suspend billing impacts various resource charges.

| Resource                      | Billing stopped   | Billing persists |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Bandwidth upgrades            |          X        |                  |
| Operating system licenses     |          X        |                  |
| Floating IPs, Load balancers, or other attached networking offerings |                   |         X        |
| Storage                       |                   |         X        |
{: caption="Table 1. Resource billing details" caption-side="top"}   

Usage times are calculated per second, for both the in use time and suspended time of your virtual server instance. Even if you never initiate the suspend billing feature by powering off your instance, the billing is calculated per second of the instance's lifecycle. 
{:note}


#### Suspend billing and sustained usage discounts
{: #suspend-billing-and-sustained-usage-discounts}

For sustained usage discounts, a suspended image picks up where it left off on the discount tier. In other words, a usage discount applies only to the time the image is in use, not to the time the image was suspended.

#### Minimum usage charge
{: #minimum-usage-charge}

Virtual server instances have a minimum usage charge per month. If usage is greater than 25% in the billing cycle, you're billed for the actual usage. If usage is less than 25% of the time it existed in the billing cycle, then the minimum charge of 25% applies. 

For example, let's say you have an instance running for an entire billing cycle (720 hours). Of that time, the instance was suspended for 577 hours and running for 143 hours. The instance is charged for 180 hours (the minimum of the available hours in that billing period).  

However, let's say you had an instance that was both created and stopped within a billing cycle that lasted for 400 hours total. Of that time, the instance was suspended for 120 hours and ran for 280 hours. The instance would be charged for its usage of 280 hours.

#### Billing invoice
{: #billing-invoice}

When you suspend billing on a virtual server, you'll see a few changes in your billing invoice. The relevant charges now appear as usage-based details. For example, you might see the following additions that reflect hours available, hours used, and total number of hours charged:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### Resource details
{: #resource-details}

#### Storage
{: #suspend-billing-storage}

When you suspend billing on a virtual server instance, the associated storage persists, but you can't access data while the virtual server instance is powered off. When you resume billing on the instance, you can then access your data and normal billing begins again.

#### IP addresses
{: #suspend-billing-ip-addresses}

All network configurations and IPs (private IPs from the subnet range) remain unchanged while the instance is suspended.

You can view whether your device is stopped on the instance details page. To see when the status changed, click **Activity** in the navigation pane. 

#### Limitations
{: #suspend-billing-limitations}

Suspended virtual servers will continue to count towards your account-wide device quota.







## Pricing for Block Storage for VPC
{: #pricing-for-block-storage-for-vpc}

Pricing for {{site.data.keyword.block_storage_is_short}} is based on the capacity or the IOPS that was provisioned, depending on which storage profile you chose.  The published monthly rate is [calculated on an hourly basis](#how-are-charges-calculated).

| IOPS Tier  | Published Monthly rate |
|------------|--------------|
|  3 IOPS/GB |  0.13 USD/GB |
|  5 IOPS/GB |  0.25 USD/GB |
| 10 IOPS/GB |  0.58 USD/GB |
| Custom IOPS| 0.10 USD/GB + 0.07 USD/IOPS |

### How are charges calculated?
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
