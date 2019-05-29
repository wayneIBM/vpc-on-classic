---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Pricing for {{site.data.keyword.vsi_is_short}} 
{: #pricing-for-virtual-servers-for-vpc}


{{site.data.keyword.vsi_is_full}} is offered in select regions with up to 62 vCPU and 248 GB RAM to fit any workload need. You're billed at an hourly rate only, with discounts applied the longer your instance is running. Virtual server usage times are calculated per second, for both the in use time and suspended time of your instance. For example, if your instance runs for 45 minutes and 32 seconds, you're billed for 45 minutes and 32 seconds.
{:shortdesc}

## Sustained usage
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

## Base instance prices
{: #base-instance-prices}

Base instance prices start at $0.087 per hour. When you create a virtual server, you are prompted to select a virtual server family and select a profile configuration. When you make your selection, the associated hourly rate is displayed in the table. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## Included operating systems
{: #included-operating-systems}

The following operating systems are included free of charge:

* CentOS 7.latest
* Ubuntu 16.04 LTS, 18.04 (minimal)
* Debian 8.latest, 9.latest (minimal)

There are premium operating systems and other add-ons available. You'll see pricing reflected in your Cost Summary.

## Suspend billing
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
