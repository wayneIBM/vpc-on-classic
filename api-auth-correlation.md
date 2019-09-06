---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-06"

keywords: vpc, resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

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

The table below lists the authorizations required to interact with {{site.data.keyword.vpc_full}} Infrastructure resources.

The terms _attached_ or _unattached_ refer to whether the resource is associated with a VPC or VPCs (either directly or indirectly through the resource's subnets). An unattached floating IP or Network ACL has no subnets, and therefore it is not associated with any VPC, so authorization by VPC is not applicable.

In order for a user to be allowed to **create** resources in any resource group, including `Default`, the user must have **Viewer** on the resource group in addition to the role required on the resource type.
{: tip}

The permission details are available both by resource operation below and [by role](#byrole) further down.

## By resource action 
{: #byaction}

For each resource and desired operation, see in the table below what is the required role.

| Resource | Operation | Required Role |
|--------|--------|---------|
| VPC | Create | Viewer on the Resource Group and Editor on Virtual Private Cloud |
| VPC | Update, Delete |  Editor on the VPC |
| VPC |  View, List | Viewer on the VPC  |
| VPC default ACL, default SG |  View, List | Viewer on the VPC |
| VPC address prefixes |  Create, Update, Delete | Editor on the VPC |
| VPC address prefixes |  View, List | Viewer on the VPC  |
|—————|——————|———————|
| Floating IP (unattached) | Create, Update, Delete | Account user **Note: Associating and disassociating are part of the Floating IP Update operation**|
| Floating IP (unattached) | View, List | Account user |
| Floating IP (attached) | Update | Editor on the parent VPC |
| Floating IP (attached) | View, List | Viewer on the parent VPC |
|——————|———————|————————|
| Network ACL (unattached), ACL rules | Create, Update, Delete | Account user |
| Network ACL (unattached), ACL rules | View, List | Account user |
| Network ACL (attached), ACL rules | Create, Update, Delete | Editor on all parent VPCs |
| Network ACL (attached), ACL rules | View, List | Viewer on one parent VPC |
|——————|———————|————————|
| Public gateway | Create, Update, Delete | Editor on parent VPC |
| Public gateway | View, List | Viewer on parent VPC |
|—————————|————————|———————————|
| Geography (Regions, Zones) | Create, Update, Delete |  Operations not available |
| Geography (Regions, Zones) | View, List |  Account user |
|———————|————————|—————————|
| SSH key | Create, Update, Delete | Editor on key |
| SSH key | View, List | Viewer on key |
|————————|—————————|————————|
| Subnet | Create, Update, Delete | Editor on the parent VPC |
| Subnet | View, List | Viewer on the parent VPC |
| Subnet ACL or PGW | Attach, Detach | Editor on the parent VPC |
| Subnet ACL or PGW | View, List | Viewer on the parent VPC |
|——————|—————————|————————|
| Security group | Create  | Viewer on the resource group for that security group, Editor on the security group, and Viewer on the parent VPC |
| Security group | Update, Delete  | Editor on the security group |
| Security group | View, List   | Viewer on the security group |
| Security group rule | View,  List | Viewer on the security group |
| Security group rule | Create, Update, Delete | Editor on the security group |
| Security group network interface | View, List | Viewer on the security group, and Viewer on the instance |
| Security group network interface | Attach, Detach | Operator on the security group and Editor on the instance |
|—————————|—————————|—————————|
| Images | Create, Update, Delete  | Editor on the image |
| Images | View, List | Viewer on the image |
|—————————|—————————|—————————|
| Instance | Create| Editor on the instance and volume (if specified), Operator on the VPC, and Operator on the security group (if specified) |
| Instance | Update, Delete | Editor on the instance |
| Instance | View, List  | Viewer on the instance |
| Instance actions | Create, Update, Delete | Editor on the instance |
| Instance actions, Initialization, NICs| View, List | Viewer on the instance |
| Instance FIPs | View, List | Viewer on the instance and Viewer on the VPC |
| Instance FIPs | Attach | Editor on the instance and Operator on the VPC|
| Instance FIPs | Detach | Editor on the instance |
|————————|——————|————————|
| VPN gateway | Create, Update, Delete | Editor on the VPN gateway|
| VPN gateway | View, List  | Viewer on the VPN gateway |
| VPN gateway connections | Create, Update, Delete | Editor on the VPN gateway |
| VPN gateway connections | View, List  | Viewer on the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections | Create, Update, Delete | Editor on the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections|View, List  | Viewer on the VPN gateway |
|————————|——————|————————|
| Load Balancer | Create, Update, Delete | Editor on the load balancer |
| Load Balancer | View, List  | Viewer on the load balancer |
| Load Balancer  pools and listeners | Create, Update, Delete | Editor on the load balancer |
| Load Balancer pools and listeners | View, List  | Viewer on the load balancer |
|————————|——————|————————|
| Volumes | Create, Update, Delete | Editor on the volume
| Volumes | View, List  | Viewer on the volume |
| Volume profiles | View, List  |  Account user |
{: caption="Table 1. Roles required per VPC resource action" caption-side="bottom"}

## By user role 
{: #byrole}

For each resource, see in the table below what each role allows the user to perform.

| Resource | Account User | Resource Viewer Role | Resource Operator Role | Resource Editor Role |
|--------|------|---------|--------|---------|
| **VPC** | | View/List VPC <br> View/List default ACL and default Security Group <br> View/List address prefixes| | Create/Update/Delete VPC <br> Create/Update/Delete address prefix | 
| **Floating IP** | View/List unattached FIP <br> Create/Update/Delete unattached FIP | View/List attached FIP if VPC Viewer | | Update attached FIP if VPC Editor |
| **Network ACL** | View/List/Create/Update/Delete unattached ACLs and ACL rules | View/List attached ACLs, rules if Viewer on one VPC with attached subnets | |Create/Update/Delete attached ACL, ACL rules if Editor on all VPCs with attached subnets | 
| **Public gateway** | |View/List public gateway if VPC Viewer | |Create/Update/Delete public gateway if VPC Editor | 
| **Geography** | View/List regions <br> View/List zones | | | | 
| **SSH Key** | | View/List key | | Create/Update/Delete key | 
| **Subnet** | | View/List subnet if VPC Viewer <br> View/List subnet ACL, public gateway if VPC Viewer | | Create/Update/Delete subnet if VPC Editor <br> Attach/Detach subnet ACL, public gateway if VPC Editor | 
| **Security Group** | | View/List security group <br> View/List security group rules <br> View/List security group network interfaces if instance viewer | Attach/Detach security group network interfaces if instance Editor | Create security group <br> Update/Delete security group <br> Create/Update/Delete security group rule |
| **Images** | View/List image | | | | 
| **Instance** | | View/List instance <br> View/List instance actions, initialization, NICs <br> View/List instance FIPs if VPC Viewer | | Create instance if volume Editor <br> Update/Delete instance <br> Create/Update/Delete instance actions <br> Attach instance FIP if VPC Operator <br> Detach instance FIP |
| **Volume** | View/List volume profiles | View/List volume | | Create/Update/Delete volume|
| **VPN Gateway** | |View/List VPN gateway, connections, policies | | Create/Update/Delete VPN gateway, connections, policies |
| **Load Balancer** | | View/List load balancer, pools, listeners | |Create/Update/Delete load balancer, pools, listeners |
{: caption="Table 2. Operations allowed on VPC resources by role" caption-side="bottom"}


