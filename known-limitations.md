---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-08-02"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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
{:DomainName: data-hd-keyref="DomainName"}

# Known limitations
{: #known-limitations}

This document contains a summary of features and APIs that are not supported, features and use cases not yet supported, known issues in the current release, and a list of features offered as Beta services only. Known limitations might change as we add capabilities to {{site.data.keyword.vpc_full}} (VPC), so feel free to check back with this document from time to time.

## Summary of features not supported
{: #summary-of-features-not-supported}

* Direct Link access to VPC is supported through [**Classic Access**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc) only.
* Although a subset of Private Service Endpoints are available to VPC, not all endpoints are available. See [Service endpoints available for IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) for services that are available.
* Saving and restoring images is not supported.
* {{site.data.keyword.cloud_notm}} VPCs are regional, therefore a VPC from one region cannot connect to a VPC in another region unless they are "classic access" enabled, or connected by means of a VPN connection. See [**Classic Access**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

## Features and Use Cases not yet supported
{: #features-and-use-cases-not-yet-supported}

This section lists more details of unsupported features and use cases, categorized according to their {{site.data.keyword.cloud_notm}} service.

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} does not support multicast or broadcast domains.
* {{site.data.keyword.vpc_short}} does not provide end-to-end encryption.
* A VPC cannot be peered with other VPCs. While it is possible to connect VPCs
  with either VPN Gateways or Floating IPs, there is no automatic route
  advertisement between the two VPCs. Static routes must be used in each VPC
  to enable layer 3 connectivity between the two VPCs. See [How to use a VPN Gateway to connect two VPCs](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#vpn-example) for how you can achieve VPC-to-VPC connectivity.
* Cloud Service Endpoints are not supported by VPC. See [Service endpoints available for IBM Cloud VPC](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc) for services that are available.

### Compute
{: VSI-unsupported-features-and-use-cases}

* Existing {{site.data.keyword.cloud_notm}} virtual server instances (VSIs) or bare metal servers cannot be migrated into a VPC.
* Bare metal servers cannot be provisioned in a VPC.

### Network
{: network-unsupported-features-and-use-cases}

* A subnet cannot be on multiple zones.
* A subnet cannot be moved from one zone to another.
* Subnets cannot be resized after they are created.
* {{site.data.keyword.cloud_notm}} Classic resources cannot be in the same subnet in a VPC. Connectivity between classic resources and one VPC in your account is possible using [**Classic Access**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).
* IPv6 is not supported.
* You cannot use your own public IP as a Floating IP. The Floating IP pool is provided by IBM.
* A Floating IP is bound to a zone. For example, a Floating IP reserved from a pool in Zone 1 cannot be assigned to a VSI in Zone 2 in the same VPC.
* Private IP addresses cannot be moved between VSIs.
* Network interfaces cannot be moved between VSIs.
* You cannot designate the specific IP address of the VSI. You can only specify the IP range for the subnet. Once you attach a VSI to a subnet, the system assigns a private IP from that subnet.
* {{site.data.keyword.vpc_short}} comes with its own Network as a Service tools such as LBaaS, ACLs, and security groups. You cannot use your own network appliances (for example, a Vyatta Gateway or other load balancer) to control any resource in the VPC. Similarily, you cannot use your own load balancer or security groups services in {{site.data.keyword.cloud_notm}} Classic Infrastructure to control resources in the {{site.data.keyword.vpc_short}}.
* The public gateway does not allow the traffic to be initiated from the Internet to a VSI in the IBM Cloud VPC. For that purpose, a Floating IP must be used.
* ACL is stateless, so return traffic must be allowed explicitly in ACL rules.

## Known issues
{: #known-issues-in-this-release}

{{site.data.keyword.block_storage_is_short}} might not accurately validate the volume name for uniqueness when all of the following conditions are met:

* The provision request is concurrent
* The volume has the same name
* The volume is in the same region
