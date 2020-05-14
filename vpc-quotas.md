---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-11"

keywords:

subcollection: vpc-on-classic

---
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Quotas
{: #quotas}

This document covers quotas and limits for your {{site.data.keyword.cloud}} Virtual Private Cloud and the resources available within it. Unless noted, the quotas cannot be changed.

Newer generation available. For more information, see [Quotas and service limits](/docs/vpc?topic=vpc-quotas) for generation 2 compute resources.
{:important}

## VPC quotas
{: #vpc-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Virtual Private Clouds | 5 per region <sup>1</sup> |
| VPCs with Classic Access | 1 per region |
| Public Gateways (PGWs) | 1 per VPC per zone |
| Address prefixes | 5 per VPC per zone |

<sup>1</sup> You can request to increase the VPC limit by submitting an [IBM Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) case.

## Subnet quotas
{: #subnet-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Subnets | 15 per Virtual Private Cloud <sup>1</sup> |

<sup>1</sup> You can request to increase the subnet limit by submitting an [IBM Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) case.

## Virtual server instance quotas
{: #vsi-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| vCPUs | 200 per region <sup>1</sup> |
| vNICs per instance | 5 per instance |
| Floating IP addresses | 100 per zone per account <sup>2</sup> |
| SSH Keys | 100 per account |

<sup>1</sup> You can request to increase the vCPU limit by submitting an [IBM Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) case. Be aware that (except for the `us-south` region) the vCPU quota must be the same in every region. Also, consider other related resource limits that need to be increased, such as floating IPs. 

<sup>2</sup> You can request to increase the floating IP limit by submitting an [IBM Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) case.

## Security groups quotas
{: #security-groups-quotas}

Some account-based quotas (limits) exist for security groups and their associated rules.

|Resource|Quota|
|--------|-----|
|Security groups|5 per network interface|
|Security groups|500 per account|
|Rules|50 per security group|
|Remote rules |5 per security group|
|Network interfaces|100 per security group|

## ACL quotas
{: #acl-quotas}

|Resource|Quota|
|--------|-----|
|ACLs| 2 x number of subnets (30 by default) per Virtual Private Cloud |
|Inbound rules|20 per ACL |
|Outbound rules |20 per ACL |

## VPN quotas
{: #vpn-quotas}

Here are the current VPN resource limitations per account:

|Resource|Quota|
|--------|-----------|
| VPN gateways| 9 per region |
| VPN gateways| 3 per zone |
| VPN connections | 10 per VPN gateway |
| IKE policies | 20 per region |
| IPsec policies | 20 per region |
| Peer subnets | 50 across all connections of a VPN gateway, 15 per individual VPN connection |
| Local subnets | 50 across all connections of a VPN gateway, 15 per individual VPN connection |

## Load balancer quotas
{: #load-balancer-quotas}

Here are the current load balancer resource quotas:

|Resource|Quota|
|--------|-----|
| Load balancers | 20 per account |
| Listeners | 10 per Load Balancer |
| Pools | 10 per Load Balancer |
| Members | 50 per Pool |

### Block storage volumes
{: #block-storage-quotas}

|Resource|Quota| 
|--------|-----| 
| Boot and secondary volumes | 300 total volumes per account in a region<sup>1</sup> |  
| Secondary volumes per instance, when creating an instance |  4 secondary volumes |
| Secondary volumes per instance, for existing instances with fewer than 4 cores | 4 secondary volumes |
| Secondary volumes per instance, for existing instances with 4 cores or more | up to 12 secondary volumes |

<sup>1</sup> You can request to increase the block storage volume limit by submitting an [IBM Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) case.
