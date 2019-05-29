---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-06"

keywords: resource, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

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

# Resource authorizations required for API and CLI calls
{: #resource-authorizations-required-for-api-and-cli-calls}

The table below lists the authorizations required to interact with {{site.data.keyword.cloud}} Virtual Private Cloud Infrastructure objects. This information is particularly helpful to know when you're making API calls. Here's what you need to know to use this table:

The terms _attached_ or _unattached_ refer to whether the resource is associated with a VPC or VPCs (either directly or indirectly through the resource's subnets). An unattached floating IP or Network ACL has no subnets, and therefore it is not associated with any VPC, so authorization by VPC is not applicable.

* **View** and **List** actions correspond to the **Viewer** roles.
* **Operate** actions correspond to the **Operator** role.
* **Update** and related actions correspond to **Editor** or **Administrator** roles.
* Resources are protected by authorization, resource reference objects are not (ID, CRN, and so forth).


| Resource | Operation | Required Authorization |
|--------|--------|---------|
| VPC | Create | View authorization on the Resource Group for that VPC, and Update authorization on VPC Resources|
| VPC | Update, Delete |  Update authorization on VPC |
| VPC |  View, List | View authorization on VPC  |
| VPC default ACL, default SG|  View, List | View authorization on VPC |
| VPC address prefixes |  Create, Update, Delete | Update authorization on VPC |
| VPC address prefixes |  View, List | View authorization on VPC  |
|—————|——————|———————|
| Floating IP (unassociated) | Create, Update, Delete | Any Account user and View authorization on all Account Management Services, if the resource is created in the default resource group. Click [here](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources#setting-up-viewer-access) for instruction on setting up viewer access. **Note: Associating and disassociating are part of the Floating IP Update operation**|
| Floating IP (unassociated) | View, List | Account user |
| Floating IP (associated) | Update | Update authorization for the associated subnet and its VPC (you cannot Create or Delete a floating IP after it is associated) |
| Floating IP (associated) | View, List | View authorization for the floating IP’s subnet | 
|——————|———————|————————|
| Network ACL (unattached), ACL rules | Create, Update, Delete | Any Account user |
| Network ACL (unattached), ACL rules | View, List | Any Account user |
| Network ACL (attached), ACL rules | Create, Update, Delete | Update authorization on all of the attached subnets and VPC |
| Network ACL (attached), ACL rules | View, List | View authorization on at least 1 of the attached subnets and VPC |
|——————|———————|————————|
| Public gateway | Create, Update, Delete |  Update authorization on the PGW’s VPC |
| Public gateway | View, List | View authorization on the PGW’s VPC |
|—————————|————————|———————————|
| Geography | View, List |  For regions and zones, any Account user |
|———————|————————|—————————|
| SSH key | Create, Update, Delete | Update authorization for the SSH key |
| SSH key | View, List | View authorization for the SSH key |
|————————|—————————|————————|
| Subnet | Create, Update, Delete | Update authorization for the subnet’s VPC |
| Subnet | View, List | View authorization for the subnet’s VPC |
| Subnet ACL or PGW | Attach, Detach | Update authorization for the subnet’s VPC |
| Subnet ACL or PGW | View, List | View authorization for the subnet’s VPC |
|——————|—————————|————————|
| Security group | Get     | View authorization on the security group.
| Security group | List    | View authorization on the security group (or else the group is omitted from the response.)
| Security group | Create  | Edit authorization on the security group that will be created (for example, Edit authorization on all security groups, or Edit authorization on the resource group in which the security group will be created.)<br />View authorization on the VPC in which the group will be created.
| Security group | Update / Delete  | Edit authorization on the security group.
| Security group rule | Get / List | View authorization on the security group.
| Security group rule | Create / Update / Delete | Edit authorization on the security group.
| Security group network interface | Get     | View authorization on the security group.<br />View authorization on the instance.
| Security group network interface | List    | View authorization on the security group (or else a 404 response results.)<br />View authorization on the instance to which the network interface belongs (or else the interface is omitted from the response.)
| Security group network interface | Attach / Detach | Operate authorization on the security group.<br />Edit authorization on the instance to which the network interface belongs.
|—————————|—————————|—————————|
| Images | Create, Update, Delete | Any Account user |
| Images | View, List  | Any Account user |
| Instances | Create| Update authorization for the Instance and Volume<br />Operate authorization for VPC, Subnet, Floating IP, Security Group (Only if they are specified)|
| Instances | Update, Delete | Update authorization for the Instance |
| Instances | View, List  | View authorization for the Instance |
| Instance actions | Create, Update, Delete | Update authorization for the Instance|
| Instance actions, Initialization, NICs| View, List  | View authorization for the Instance |
| Instance FIPs | View, List | View authorization for the Instance and the FIP |
| Instance FIPs | Associate | Update authorization for the Instance<br />Operate authorization for FIP |
| Instance FIPs | Disassociate | Update authorization for the Instance |
|————————|——————|————————|
| VPN gateway | Create, Update, Delete | Update authorization for the VPN |
| VPN gateway | View, List  | View authorization for the VPN |
| VPN gateway connections | Create, Update, Delete | Update authorization for the VPN |
| VPN gateway connections | View, List  | View authorization for the VPN |
| VPN gateway ike_policies, ipsec_policies and connections | Create, Update, Delete | Update authorization for the VPN |
| VPN gateway ike_policies, ipsec_policies and connections|View, List  | View authorization for the VPN |
|————————|——————|————————|
| Load Balancer | Create, Update, Delete | Update authorization for the Load Balancer |
| Load Balancer | View, List  | View authorization for the Load Balancer |
| Load Balancer  pools and listeners | Create, Update, Delete | Update authorization for the Load Balancer |
| Load Balancer pools and listeners | View, List  | View authorization for the Load Balancer |
|————————|——————|————————|
| Volumes | Create, Update, Delete | Update authorization for the Volume
| Volumes | View, List  | View authorization for the volume |
| Volume profiles | View, List  | Any Account user |

