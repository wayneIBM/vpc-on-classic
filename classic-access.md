---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-27"

keywords: vpc, classic, access, API, CLI, limitations

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

# Setting up access to your Classic Infrastructure from VPC
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (linked help topic)

You may set up access to your {{site.data.keyword.cloud}} Classic Infrastructure, including Direct Link connectivity, from one VPC in each region, for any account. These special "Classic Access VPCs" use the same routing capability (implicit router) as your {{site.data.keyword.cloud}} classic infrastructure. Readers are expected to be familiar with Classic Infrastructure networking.

When you've set up a VPC for classic access, every compute host (VSI or Bare Metal) without a public interface in your classic account can send and receive packets to and from the classic access VPC. However, remember that firewalls, gateways, Network ACLs, or security groups can filter this traffic. As a best practice, we recommend that you allow only the traffic that's required for your applications to function properly.

In hosts with a public interface, you must add a route back to your Classic-enabled VPC.
{: important}

## Pre-requisites:
{: #vpc-prerequisites}

1. Your classic account must be linked to your IBM Cloud account. See [Linking IBMid accounts](/docs/account?topic=account-unifyingaccounts) for instructions on how to do this.
1. Your classic account must be enabled for VRF.
    * If you already have Direct Link on your account, you are ready.
    * If your account is not VRF-enabled, open a ticket to request "VRF Migration" for your account. See [Converting to VRF](/docs/vpc-on-classic?topic=vpc-on-classic-what-happens-during-the-account-conversion-process#how-you-can-initiate-the-conversion) to learn more about the conversion process.

Firewalls, gateways, Network ACLs, or security groups can filter out some or all of the network traffic between Classic and VPC resources.
{: important}

All subnets in a VPC with classic access will be shared into the Classic Infrastructure VRF, which uses IP addresses in the `10.0.0.0/8` space. To avoid IP address conflicts, **do not use** IP addresses on the `10.0.0.0/8` space when creating subnets in a Classic Access VPC.
{: important}

## Create a Classic Access VPC
{: #create-a-classic-access-vpc}

You can create a VPC with Classic Access capability by using the User Interface (UI), Command Line Interface (CLI), or Application Programming Interface (API).

A VPC must be set up for Classic Access when it's created: you cannot update a VPC to add or remove the Classic Access capability.
{: note}

### Create a Classic Access VPC from the User Interface
{: #create-a-classic-access-vpc-from-the-user-interface}

Create a Classic Access VPC from the **Create VPC** page, by clicking on the checkbox titled **Enable access to classic resource**, under the **Classic access** label.

![classic-access-ui](/images/classic-access-ui.png)

### Create a Classic Access VPC using the CLI
{: #create-a-classic-access-vpc-using-the-cli}

Use the flag `--classic-access` when creating the VPC. For example,

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### Create a Classic Access VPC using the API
{: #create-a-classic-access-vpc-using-the-api}

Pass in the `classic_access` parameter when creating the VPC. For example,

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### Classic Access VPC default address prefixes
{: #classic-access-default-address-prefixes}

All subnets in a VPC with classic access will be shared into the Classic Infrastructure VRF, which uses IP addresses in the `10.0.0.0/8` space. To avoid IP address conflicts, **do not use** IP addresses on the `10.0.0.0/8` space when creating subnets in a Classic Access VPC.
{: important}

When a new Classic Access VPC is created, the default address prefixes are assigned, based on the region and zone, as shown in the table that follows:

Zone         | Address Prefix
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## Limitations
{: #vpc-limitations}

* Only your private (or "back" in old documentation) networks will be connected to your account's private implicit router.
* Only subnets allocated with classic APIs are connected to the classic side of your private implicit router. The routing function of the implicit router does not work between subnets on classic VLANs.
* Only one VPC per region, per account can be enabled for Classic Access.

Depending on the OS you've installed on your classic VSIs or bare metal servers, your configuration procedure will vary. For more information, see [About public virtual servers](/docs/vsi?topic=virtual-servers-about-public-virtual-servers).
{: note}
