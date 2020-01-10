---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-10-30"
keywords: vpc, quota, resource, classic, access, gateway, address, prefix, VSI, vNIC, floating, SSH, key, security, group, rule, remote, peer, ACL, region, ingress, egress, VPN, policies, load balancer, listener, pool, per

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

## VPC quotas
{: #vpc-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Virtual Private Clouds | 5 per region|
| VPCs with Classic Access | 1 per region |
| Public Gateways (PGWs) | 1 per VPC per zone |
| Address prefixes | 5 per VPC per zone |

## Subnet quotas
{: #subnet-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Subnets | 15 per Virtual Private Cloud |


## VSI quotas
{: #vsi-quotas}

|   Resource     | Maximum Number |
| ------- | :------: |
| Virtual Server Instances (VSIs) | 100 per region <sup>1</sup> |
| vNICs per VSI | 5 per VSI |
| Floating IP addresses | 100 per zone per account <sup>2</sup> |
| SSH Keys | 100 per account |

<sup>1</sup> You can request to increase the VSI limit by submitting a [support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) ticket. Consider other related resource limits that need to be increased as well, such as floating IPs.

<sup>2</sup> You can request to increase the floating IP limit by submitting a [support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) ticket.

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
|ACLs| 30 per region |
|Ingress rules|20 per ACL |
|Egress rules |20 per ACL |

## VPN quotas
{: #vpn-quotas}

Here are the current VPN resource limitations per account:

|Resource|Quota|
|--------|-----------|
| VPN gateways| 9 per region |
| VPN gateways| 3 per zone |
| VPN connections | 10 per VPN gateway |
| IKE policies | 20 per region |
| IPSec policies | 20 per region |
| Peer subnets | 50 across all connections of a VPN gateway, 15 per indvidual VPN connection |
| Local subnets | 50 across all connections of a VPN gateway, 15 per indvidual VPN connection |

## Load Balancer quotas
{: #load-balancer-quotas}

Here are the current load balancer resource quotas:

|Resource|Quota|
|--------|-----|
| Load Balancers | 20 per account |
| Listeners | 10 per Load Balancer |
| Pools | 10 per Load Balancer |
| Members | 50 per Pool |

## Secondary volume quotas
{: #secondary-volume-quotas}

| Resource | Quota |
|--------|----- |
| Secondary volumes per instance, when creating an instance |  4 secondary volumes |
| Secondary volumes per instance, for existing instances with fewer than 4 cores | 4 secondary volumes |
| Secondary volumes per instance, for existing instances with 4 cores or more | up to 12 secondary volumes |
