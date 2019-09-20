---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: vpc, FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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


# FAQs
{: #faqs}

## Can I connect my VPC to my other IBM Cloud workloads?  
{: #faq-0}
{:faq}

Yes, you can set up access to your {{site.data.keyword.cloud}} classic infrastructure from one VPC in each region. For more information, see [Setting up access to your classic infrastructure](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## What is the limit on the number of characters in a VPC name?
{: #faq-1}
{:faq}

Currently, the limit is 100. If this limit is exceeded, you might receive an "internal error" message.

## Can any of my VPC resource names begin with a number?
{: #faq-2}
{:faq}

No, although the name can contain numbers, it must begin with a letter.

## Are there restrictions on what characters I can use in a name?
{: #faq-3}
{:faq}

Yes, the UI blocks consecutive double dashes, underscores, and periods from being part of a VSI name.

## Can a VSI be created without a subnet, just with Floating IP?
{: #faq-9}
{:faq}

No.

## Can a VSI be attached to more than one VPC?
{: #faq-10}
{:faq}

No.

## Can a subnet size be changed after it is created in a VPC?
{: #faq-11}
{:faq}

No.

## Are there egress bandwidth charges for traffic from VPC to COS?
{: #faq-17}
{:faq}

There are no egress bandwidth charges as long as the ADN endpoint is used to connect from VPC to COS, but API call charges still apply.

 ## Why do I need to specify a subnet when I set up my VPN for VPC, and what should I specify?
{: #faq-16}
{:faq}

When you’re setting up a VPN gateway, you must specify a subnet, so that the VPN gateway can obtain the IP addresses it requires for the VPN service. By specifying the subnet, you retain control over how your VPN traffic is handled, and you have the flexibility to specify a location that’s closer to the resources that utilize the VPN. In this way, you can reduce latency and improve performance.

## How do I know where my load balancer instances will be deployed?
{: #faq-18}
{:faq}

The subnets chosen determine where the VPC load balancer is going to be deployed. VPC load balancer is a regional resource. The best practice is to choose subnets from different zones under a given region to increase redundancy.


## During Floating IP assignment in a VPC, a customer must specify the vNIC of the VSI, is that correct?
{: #faq-4}
{:faq}

Yes, and in fact, it must be the primary network interface of a server.

However, if we add an "address" resource in the API under the networking area, we could very well change the association of the floating IP to the address rather than the interface.

## Can a vNIC on a virtual server instance in my VPC have both floating IP and private IP?
{: #faq-5}
{:faq}
 
Yes.

## In the IBM Cloud Console UI for VPC, there is a ”toggle" for each subnet to attach or detach to or from the Public Gateway (PGW). Who creates the PGW? Will the PGW be ready when a customer wants to attach the subnet to the PGW in the UI?
{: #faq-6}
{:faq}

The PGW must be created explicitly through the API, because the API requires an explicit association of a subnet to a particular public gateway.

## Imagine that VSI1 in a VPC has only vNIC1 and it is attached to Subnet1. Subnet1 is attached to a Public Gateway (PGW). Can a customer still assign a Floating IP to VSI1?
{: #faq-7}
{:faq}

Yes, a server can be on a subnet attached to a public gateway and also have a floating IP. The assignment of a floating IP to a VSI is not related to whether there is a public gateway attached to the subnet. A floating IP associated to a VSI will take precedence over the public gateway attached to the subnet.

## In my VPC, VSI1 has 2 vNICs, called vNIC1 and vNIC2. Can these two vNICs be attached to the same subnet?
{: #faq-8}
{:faq}
 
Yes, there is no limitation on having multiple network interfaces on the same server attached to the same subnet. However, multiple NICs on a VSI are not supported.

## Who enforces that there must be only 1 Public Gateway per zone for a VPC?
{: #faq-13}
{: faq}

The VPC API enforces this limit.

## During the PGW creation, do I need to reserve the FIP, or does the system automatically reserve the FIP? Will I see that Floating IP when I query all of the Floating IPs?
{: #faq-12}
{: #faq}

The API automatically creates a floating IP along with the public gateway if an existing floating IP is not specified. And yes, that floating IP will show up in the list.


## What is the correct way to use the `instance-network-interface-floating-ip-add` subcommand in VPC? Does one create/allocate a floating IP address beforehand?
{: #faq-14}
{:faq}

 You have to allocate a floating IP first, and then provide the FIP address on the interface floating IP `add` command.

 For example: `ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`. Provide the zone.

## If I use 10.x addresses to access an infrastructure service from my Classic VRF, can I use this same address to access this service from a Classic Access VPC?
{: #faq-15}
{:faq}

No, you have to use the 161.26 address to access infrastructure services from a Classic Access VPC.
