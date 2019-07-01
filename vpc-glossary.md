---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: vpc, glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}

# VPC Glossary
{: #vpc-glossary}

The following terminology is commonly used and is specific to {{site.data.keyword.cloud}} Virtual Private Cloud. See [Glossary terms for IBM Cloud](/docs/overview/glossary?topic=overview-glossary) for broader and more terms.

## Access Control List
{: #access-control-list}

An Access Control List (ACL) provides security controls for inbound and outbound traffic for a [subnet](#subnet). An ACL is stateless. Each ACL consists of rules, based upon a _source IP_, _source port_, _destination IP_, _destination port_, and _protocol_.

In IBM Cloud VPC, every subnet is created with a default ACL, which allows inbound and outbound traffic, but customers can create custom ACLs. Only one ACL is attached to a subnet at any time, but one ACL can be attached to multiple subnets.

Sometimes also called a _Network ACL_ or NACL.

## Floating IP
{: #floating-ip}

Floating IP is a method to provide inbound and outbound access to the internet for VPC resources such as instances, a load balancer, or a VPN tunnel, using assigned _Floating IP addresses_ from a pool.

Floating IP addresses are public IP addresses that are provided by the system from the pre-existing pool. You cannot bring your own public IP addresses. Floating IP addresses are reachable from the internet, and they can be associated to an instance (for example, a VSI, a load balancer, or a VPN gateway) by means of a vNIC.

You can reserve a Floating IP address from the pool of available Floating IP addresses provided by IBM, and you can associate it to or disassociate it from any instance in the same Virtual Private Cloud. Floating IP makes use of 1-to-1 [NAT](#nat) to allow a server to communicate with the public internet and also with a private subnet within your cloud environment.

## Geography
{: #geography}

In a VPC, the geography designation provides information about the region and zone within which the VPC and its associated resources are deployed.

## Image
{: #image}

The information required to launch a virtual server instance, or _instance_. Typically, it is a snapshot image of a commercially-available operating system, used for booting.

## Implicit Router
{: #implicit-router}

The inherent network connectivity between all subnets created within a VPC.

## Instance
{: #instance}

A short name for a virtual server, or VSI, that runs within a VPC.

## LBaaS
{: #lbaas}

Load Balancer as a Service (LBaaS) provides the ability to distribute traffic in a balanced allocation among instances in a VPC.

## NAT
{: #nat}

Network Address Translation (NAT) is an addressing method described in [Internet RFC 1631 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc1631){: new_window} so that one IP address can be used to communicate with several other IP addresses, such as those on a private subnet, essentially by means of a lookup table. NAT has two main types: 1-to-1 NAT and [Many-to-1 NAT ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}. NAT was originally intended as a way to extend the useful life of IPv4 IP addresses, but now it is commonly used in cloud environments as a way of creating communication with private subnets.

## Profile
{: #profile}

A Profile is a popular combination of vCPU and RAM that can be instantiated quickly to start up a virtual server instance (VSI). Three families of profiles are supported: Balanced, Compute, and Memory.


## Public Gateway
{: #public-gateway}

A Public Gateway (PGW) enables outbound-only access for a subnet (with all the VSIs attached to the subnet) to connect to the internet. Note that subnets are private by default; however, optionally, you can create a PGW and attach a subnet to the PGW. After a subnet is attached to the PGW, all the VSIs in that subnet can connect to the internet.

PGW uses Many-to-1 NAT, which means that thousands of VSIs with private addresses will use one public IP address to talk to the public internet. PGW does not enable the internet to initiate a connection with those instances.

## Region
{: #region}

A geographic area within which a VPC is deployed. Each region contains multiple zones, which represent independent fault domains. IBM Cloud VPC spans multiple zones within its assigned region.

## Resource
{: #resource}

A type of asset that is part of a VPC deployment and can incur billing charges, for example, a VSI, a floating IP, or the VPC itself. Resources can be combined into _resource groups_ to make accounting easier when certain resources are always used in combination in a VPC.

## Security Group
{: #security-group}

A security group acts as a virtual firewall that controls the inbound and outbound traffic for one or more servers (VSIs). A security group is a collection of rules that specify whether to allow traffic for an associated VSI. When a customer launches a VSI, he or she can associate one or more security groups with that VSI. Given the correct permissions, customers can modify security group rules.

## Shares
{: #shares}

A way to provide file storage in a VPC.

## Subnet
{: #subnet}

A subnet is an IP address range, bound to a single Zone, which cannot span multiple Zones or Regions. A subnet can span the entirety of a zone in an IBM Cloud VPC.

For the purposes of IBM Cloud VPC, the important characteristic for a subnet is the fact that subnets can be isolated from one another, as well as being interconnected in the usual way. Subnet isolation can be accomplished by Network Access Control Lists (ACLs) that act as firewalls to control the flow of data packets among subnets. Similarly, security groups act as virtual firewalls to control the flow of data packets to and from individual virtual server instances (VSIs).

It is the isolation of subnets that allows you to create a private space within the public cloud.

## SSH Key
{: #ssh-key}

Automatically generated, cryptographic access credentials that control access to virtual server instances in a VPC.

## Volumes
{: #volumes}

Hypervisor-mounted, high-performance data storage for virtual server instances in a VPC.

## VPC
{: #vpc}

A virtual network tied to an account. It provides fine-grained control over virtual infrastructure and network traffic segmentation, along with security and the ability to scale dynamically.

## VPN
{: #vpn}

A Virtual Private Network (VPN) allows for a private connection between two endpoints, even when the data is transferred across a public network. Customers can share data as if they were connected to a private network. Usually, a VPN is used in combination with security methods such as authentication and encryption to provide maximum data security and privacy.

## VPN Connection
{: #vpn-connection}

A private connection between two endpoints, which remains private and can be encrypted even when the data is transferred across a public network.

## VPNaaS
{: #vpnaas}

VPN as a Service (VPNaaS) provides for setting up a VPN with endpoints inside and outside a VPC.

## Zone
{: #zone}

An independent fault domain. A Zone is an abstraction designed to assist with improved fault tolerance and decreased latency. A Zone guarantees the following properties:

 * Because each zone is an independent fault domain, it is extremely unlikely for two zones in a region to fail simultaneously.
 * Traffic between zones in a region will experience less than 2ms in latency.
