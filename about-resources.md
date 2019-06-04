---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# About VPC Infrastructure resources
{: #about-vpc-infrastructure-resources}

The term _resources_ refers to the component parts of a {{site.data.keyword.cloud}} Virtual Private Cloud deployment. Each VPC may contain resources of the following types, which can be grouped together as needed:

* **vpc**
* **instance**
* **key**
* **image**
* **subnet**
* **volume**
* **acl**
* **security group**
* **floating IP**
* **vnic**
* **gateway**
* **loadbalancer**
* **vpn**

You could think of most of these types of resources as "children" of their VPC, because they acquire many of their authorization policies, which govern their usage, from their "parent" VPC (as shown in the summary table in the next section).

An exception is that the `loadbalancer` and `vpn` resource types maintain their own authorization policies. More detail about resource authorization for VPN and Load Balancer resources is given later in this document.
{: note}

## Resource policies
{: #resource-policies}

Generally, access to resources in a VPC is controlled by _policies_. If you want to customize access to resources for your VPC, you can create customized policies. A resource policy can target:

* all resources
* all resources of a type
* all resources in a group
* an individual resource

## Resources and resource groups
{: #resources-and-resource-groups}

A _resource group_ is a collection of resources, say an entire VPC, or a single subnet and its ACL, that are associated for the purpose of establishing authorization and usage. You could think of a resource group as a collection of infrastructure that might be used by a project, a department, or a team.

You can use the search CLI to display tags attached to a resource. With the appropriate filters, you can display only the instances you care about, such as `ibmcloud resource search name:` or `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Large enterprises may wish to divide a VPC into resource groups. Smaller companies may not need to divide their VPC into resource groups, because all of their team would be using the entire VPC. If you are familiar with _OpenStack_, a resource group is similar in concept to a _Project_ in _OpenStack Keystone_.

Assignment of a resource to a resource group can be done ONLY when creating your VPC. Resources cannot change resource groups once they are created.
{: .note}

Resource groups are designed primarily for large organizations. For most companies and teams, the resource breakout is on a VPC boundary. This general rule of thumb may change when it is possible to assign individual VSIs (instances) to a resource group and individual users to a VSI (instance) resource.

When you're setting up your IBM Cloud VPC, if you want to use resource groups, it’s good to have a plan in place beforehand for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

## Specifics for VPC
{: #specifics-for-vpc}

Currently, VPC assigns roles and access at the VPC boundary only, for any or all resources within that particular VPC. For instance, you cannot assign access to individual subnets within that VPC. Instead, the authorizations for all of the types of resources with the VPC--subnets, instances, floating IP, security groups, ACLs, and so forth--are inherited from the VPC "parent" of that resource. Certain resources, such as **load balancers** and **vpn**, maintain authorization separately from the VPC to which they are attached.

### VPC resource authorization by resource type
{: #vpc-resource-authorization-by-resource-type}

The table summarizes how VPC resources are authorized by default.

| Type of resource | Where it gets its default authorization |
|-----------|------------|
| VPC | Authorization check when created |
| Load Balancer | Authorization check when created |
| VPN | Authorization check when created |
| Subnet | Parent VPC |
| Public gateway | Parent VPC |
| Instance | Authorization check when created |
| Security Group | Parent VPC |
| Key | Authorization check when created |
| Image | Account |
| Floating IP | Account |
| ACL | Account|
| Volume | N/A |

Floating IP and ACLs can be created with scoping at the account level, if unassigned. However, as soon as a floating IP is assigned to an instance or an ACL is assigned to a subnet, these resources become subject to the VPC's authorization scope.
{: note}

### VPC coverage of roles and authorized actions on resources
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

In a scenario with 2 VPCs, here are some examples of what happens when any user with certain roles attempts to perform certain actions:

* User _admin_, with **Administrator** permission on all resources
  * Creates `vpc1`: ok

* User _editor_, with **Editor** permission on all resource
  * Creates `vpc2`: ok
  * Updates `vpc2`: ok
  * Deletes `vpc2`: ok

* User _operator_, with **Operator** permission on all resources
  * Creates `vpc`: denied
  * Updates `vpc1`: denied
  * Deletes `vpc1`: denied

* User _viewer_, with **Viewer** permission on all resources
  * Lists VPCs: sees [vpc1]
  * Reads `vpc1`: ok

* User _noRole_, with no policies
  * Lists VPCs: sees []
  * Reads `vpc1`: denied

### VPC coverage summary table
{: #vpc-coverage-summary-table}

For any policy that grants access by role (Administrator, Editor, Operator, Viewer), the following actions are given (X=denied, o=OK)

 role:      | none | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | X     | o      | o
Read        | X    | o      | X     | o      | o
Update      | X    | X      | X     | o      | o
Delete      | X    | X      | X     | o      | o


## Resource authorization of VPN for VPC
{: #resource-authorizations-of-vpn-for-vpc}

Resource authorization of **VPN for VPC** is set up separately from other types of VPC resource authorization, but in a similar manner.

Policies in VPN for VPC control access to VPN resources, especially VPN Gateways. For access to the child resources of a VPN Gateway, such as VPN connections and local or peer CIDRs, you must have **Read** access (or more) to the parent VPN Gateway. Resource policies can only be used with VPN connections that also have **Read** access or higher for the parent VPN Gateway. A resource policy in VPN can target:

* all VPN resources (VPN Gateways, VPN connections, IKE and IPsec policies)
* all VPN resources in a resource group
* an individual VPN Gateway

VPN usage is billed separately as well.
{: note}

### VPN for VPC coverage of roles and authorized actions on resources
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

The same user roles are supported as are supported in VPC resource authorization, but with different actions enabled for each role.

Here are some examples of what happens when users with certain roles attempt to perform certain VPN-related actions:

* User _admin_, with **Administrator** permission on all resources
  * Creates, updates, deletes, reads, lists VPN Gateways: ok
  * Creates, updates, deletes, reads, lists VPN Connections: ok
  * Creates, updates, deletes, reads, lists VPN Connection Peer & Local CIDRs: ok
  * Creates, updates, deletes, reads, lists IKE Policies: ok
  * Creates, updates, deletes, reads, lists IPSec Policies: ok

* User _editor_, with **Editor** permission on all resources
  * Same as those of Administrator

* User _operator_, with **Operator** permission on all resources
  * Creates, updates, deletes VPN Gateways: denied
  * Creates, updates, deletes VPN Connections: denied
  * Creates, updates, deletes VPN Connection Peer & Local CIDRs: denied
  * Creates, updates, deletes IKE Policies: denied
  * Creates, updates, deletes IPSec Policies: denied
  * Reads, Lists VPN Gateways: ok - sees actual data
  * Reads, Lists VPN Connections: ok - sees actual data
  * Reads, Lists VPN Connection Peer & Local CIDRs: ok - sees actual data
  * Reads, Lists IKE Policies: ok - sees actual data
  * Reads, Lists IPSec Policies: ok - sees actual data

* User _viewer_, with **Viewer** permission on all resources
  * Same as those of Operator

* User _noRole_, with no policies
  * Reads, Lists VPN Gateways: denied - list shows empty data
  * Reads, Lists VPN Connections: denied
  * Reads, Lists VPN Connection Peer & Local CIDRs: denied
  * Reads, Lists IKE Policies: denied - list shows empty data
  * Reads, Lists IPSec Policies: denied - list shows empty data

### VPN for VPC coverage summary table
{: #vpn-for-vpc-coverage-summary-table}

The action role mapping in VPN for VPC can be visualized by the following table (X=denied, o=OK):

role:       | none | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | o     | o      | o
Read        | X    | o      | o     | o      | o
Update      | X    | X      | X     | o      | o
Delete      | X    | X      | X     | o      | o

## Resource authorization for Load Balancer for VPC
{: #resource-authorization-for-load-balancer-for-vpc}

Resource authorization for **Load Balancer for VPC** usage is set up separately from other resource authorization in your VPC, but in a similar manner.

Load Balancer usage is billed separately as well.
{: note}

### Load Balancer coverage of roles and authorized actions on resources
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

Here are some examples of what happens when users with certain roles attempt to perform certain actions related to the VPC Load Balancer:

* User _admin_, with **Administrator** permission on all resources
  * Creates, updates, deletes, reads, lists Load Balancers: ok
  * Creates, updates, deletes, reads, lists Listeners: ok
  * Creates, updates, deletes, reads, lists Pools: ok
  * Creates, updates, deletes, reads, lists Members: ok
  * Creates, updates, deletes, reads, lists Load Balancer Statistics: ok

* User _editor_, with **Editor** permission on all resource
  * Same as those of Administrator

* User _operator_, with **Operator** permission on all resources
  * Creates, updates, deletes, reads, lists Load Balancers: denied
  * Creates, updates, deletes, reads, lists Listeners: denied
  * Creates, updates, deletes, reads, lists Pools: denied
  * Creates, updates, deletes, reads, lists Members: denied
  * Creates, updates, deletes, reads, lists Load Balancer Statistics: denied

* User _viewer_, with **Viewer** permission on all resources
  * Reads, lists Load Balancers: ok
  * Reads, lists Listeners: ok
  * Reads, lists Pools: ok
  * Reads, lists Members: ok
  * Reads Load Balancer Statistics: ok

* User _noRole_, with no policies
  * Reads, lists Load Balancers: denied - list shows empty data
  * Reads, lists Listeners: denied
  * Reads, lists Pools: denied
  * Reads, lists Members: denied
  * Reads Load Balancer Statistics: denied

### Load Balancer coverage summary table
{: #load-balancer-coverage-summary-table}

The action role mapping in Load Balancer for VPC can be visualized by the following table (X=denied, o=OK):

role:       | none | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | X     | o      | o
Read        | X    | o      | X     | o      | o
Update      | X    | X      | X     | o      | o
Delete      | X    | X      | X     | o      | o


## Resource authorization for Virtual Server for VPC
{: #planning-virtual-servers-for-vpc-permissions}

Set the resource authorization for **Virtual Server for VPC** separately from other resource authorizations in your VPC. 

Virtual servers for VPC usage is billed separately.
{: note}

* As an administrator you can define roles and take any available actions on {{site.data.keyword.vsi_is_short}}.
* As an editor you can modify the state and create or delete subresources.
* As an operator you can take actions that don't change the state of resources.
* As a viewer you can take actions that don't change the state of the resources.

### Virtual server coverage summary table
{: #vsi-coverage-summary-table}

The action role mapping in Virtual Server for VPC can be visualized by the following table (X=denied, o=OK):

role:       | none | Viewer | Operator | Editor | Administrator
-----------:|------|--------|-------|--------|-------
Create      | X    | X      | X     | o      | o
List        | X    | o      | o     | o      | o
Read        | X    | o      | o     | o      | o
Update      | X    | X      | X     | o      | o
Delete      | X    | X      | X     | o      | o

When you create an instance, you must also have Operator access for the VPC and Security Group resources, if those resources are specified. Subnet and Floating IP resources inherit permissions from the associated VPC.  
{: tip} 
