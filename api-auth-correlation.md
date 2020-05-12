---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-20"

keywords:

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

# Roles required to manage VPC resources
{: #resource-authorizations-required-for-api-and-cli-calls}

This page describes the authorization levels required to interact with {{site.data.keyword.vpc_full}} Infrastructure resources.

The terms _attached_ or _unattached_ refer to whether the resource is associated with one or more VPCs, either directly or indirectly through the resource's subnets. When an unattached floating IP or Network ACL has no subnets, it is not associated with any VPC, so authorization by VPC is not applicable.

In order for a user to be allowed to **create** resources in any resource group, including _Default_ group, the user must have **Viewer** privileges on the resource group, in addition to the role required on the resource type. In order for a user to be allowed to create Floating IPs, the user must have **Viewer** privileges on the _Default_ resource group since that is where the Floating IPs are created.
{: tip}

Not all VPC resources can be assigned to a resource group. See the [resource table](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources) for specifics.
{: important}

Permission details are available by both resource action and [by role](#byrole).

## By resource action
{: #byaction}

The following table defines the resource action allowed for each role.

| Resource | Operation | Required Role |
|--------|--------|---------|
| VPC | Create | Editor on the Virtual Private Cloud resource|
| VPC | Update, Delete |  Editor on the VPC |
| VPC |  View, List | Viewer on the VPC  |
| VPC default ACL, default SG |  View, List | Viewer on the VPC |
| VPC address prefixes |  Create, Update, Delete | Editor on the VPC |
| VPC address prefixes |  View, List | Viewer on the VPC  |
|____________|____________|____________|
| Floating IP (unattached) | Create, Update, Delete | Account user <br>_Note: Associating and disassociating are part of the Floating IP Update operation_|
| Floating IP (unattached) | View, List | Account user |
| Floating IP (attached) | Update | Editor on the parent VPC |
| Floating IP (attached) | View, List | Viewer on the parent VPC |
|____________|____________|____________|
| Network ACL (unattached), ACL rules | Create, Update, Delete | Account user |
| Network ACL (unattached), ACL rules | View, List | Account user |
| Network ACL (attached) | Update, Delete | Editor on all parent VPCs  <br> _Note: Network ACLs cannot be created attached_ |
| Network ACL (attached), ACL rules | View, List | Viewer on one parent VPC |
| Network ACL rules when Network ACL is attached | Create, Update, Delete | Editor on all parent VPCs |
| Network ACL rules when Network ACL is attached | View, List | Viewer on one parent VPC |
|____________|____________|____________|
| Public gateway | Create, Update, Delete | Editor on parent VPC |
| Public gateway | View, List | Viewer on parent VPC |
|____________|____________|____________|
| Geography (Regions, Zones) | Create, Update, Delete |  Operations not available |
| Geography (Regions, Zones) | View, List |  Account user |
|____________|____________|____________|
| SSH key | Create, Update, Delete | Editor on key |
| SSH key | View, List | Viewer on key |
|____________|____________|____________|
| Subnet | Create, Update, Delete | Editor on the parent VPC |
| Subnet | View, List | Viewer on the parent VPC |
| Subnet ACL or PGW | Attach, Detach | Editor on the parent VPC |
| Subnet ACL or PGW | View, List | Viewer on the parent VPC |
|____________|____________|____________|
| Security group | Create  | Viewer on the resource group for that security group, Editor on the security group, and Viewer on the parent VPC |
| Security group | Update, Delete  | Editor on the security group |
| Security group | View, List   | Viewer on the security group |
| Security group rule | View,  List | Viewer on the security group |
| Security group rule | Create, Update, Delete | Editor on the security group |
| Security group network interface | View, List | Viewer on the security group, and Viewer on the instance |
| Security group network interface | Attach, Detach | Operator on the security group and Editor on the instance |
|____________|____________|____________|
| Images | Create, Update, Delete  | Editor on the image |
| Images | View, List | Viewer on the image |
|____________|____________|____________|
| Instance | Create| Editor on the instance and volume (if specified), Operator on the VPC, and Operator on the security group (if specified) |
| Instance | Update, Delete | Editor on the instance |
| Instance | View, List  | Viewer on the instance |
| Instance actions | Create, Update, Delete | Editor on the instance |
| Instance actions, Initialization, NICs| View, List | Viewer on the instance |
| Instance FIPs | View, List | Viewer on the instance and Viewer on the VPC |
| Instance FIPs | Attach | Editor on the instance and Operator on the VPC|
| Instance FIPs | Detach | Editor on the instance |
|____________|____________|____________|
| VPN gateway | Create, Update, Delete | Editor on the VPN gateway|
| VPN gateway | View, List  | Viewer on the VPN gateway |
| VPN gateway connections | Create, Update, Delete | Editor on the VPN gateway |
| VPN gateway connections | View, List  | Viewer on the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections | Create, Update, Delete | Editor on the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections|View, List  | Viewer on the VPN gateway |
|____________|____________|____________|
| Load Balancer | Create, Update, Delete | Editor on the load balancer |
| Load Balancer | View, List  | Viewer on the load balancer |
| Load Balancer  pools and listeners | Create, Update, Delete | Editor on the load balancer |
| Load Balancer pools and listeners | View, List  | Viewer on the load balancer |
|____________|____________|____________|
| Volumes | Create, Update, Delete | Editor on the volume  |
| Volumes | View, List  | Viewer on the volume |
| Volume profiles | View, List  |  Account user |
{: caption="Table 1. Roles required per VPC resource action" caption-side="bottom"}

## By user role
{: #byrole}

The following table describes what actions can be performed on each resource by role.

| Resource | Account User | Resource Viewer Role | Resource Operator Role | Resource Editor Role |
|--------|------|---------|--------|---------|
| **VPC** | | View/List VPC <br> View/List default ACL and default security group <br> View/List address prefixes| | Create/Update/Delete VPC <br> Create/Update/Delete address prefix |
| **Floating IP** | View/List unattached FIP <br> Create/Update/Delete unattached FIP | View/List attached FIP if VPC Viewer | | Update attached FIP if VPC Editor |
| **Network ACL** | View/List/Create/Update/Delete unattached ACLs and ACL rules | View/List attached ACLs, rules if Viewer on one VPC with attached subnets | |Create/Update/Delete attached ACL, ACL rules if Editor on all VPCs with attached subnets |
| **Public gateway** | |View/List public gateway if VPC Viewer | |Create/Update/Delete public gateway if VPC Editor |
| **Geography** | View/List regions <br> View/List zones | | | |
| **SSH Key** | | View/List key | | Create/Update/Delete key |
| **Subnet** | | View/List subnet if VPC Viewer <br> View/List subnet ACL, public gateway if VPC Viewer | | Create/Update/Delete subnet if VPC Editor <br> Attach/Detach subnet ACL, public gateway if VPC Editor |
| **Security Group** | | View/List security group <br> View/List security group rules <br> View/List security group network interfaces if instance viewer | Attach/Detach security group network interfaces if instance Editor | Create security group <br> Update/Delete security group <br> Create/Update/Delete security group rule |
| **Images** | View/List image | View/List image | | Create/Update/Delete image|
| **Instance** | | View/List instance <br> View/List instance actions, initialization, NICs <br> View/List instance FIPs if VPC Viewer | | Create instance if volume Editor <br> Update/Delete instance <br> Create/Update/Delete instance actions <br> Attach instance FIP if VPC Operator <br> Detach instance FIP |
| **Volume** | View/List volume profiles | View/List volume | | Create/Update/Delete volume|
| **VPN Gateway** | |View/List VPN gateway, connections, policies | | Create/Update/Delete VPN gateway, connections, policies |
| **Load Balancer** | | View/List load balancer, pools, listeners | |Create/Update/Delete load balancer, pools, listeners |
{: caption="Table 2. Operations allowed on VPC resources by role" caption-side="bottom"}
