---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-08-26"

keywords: vpc, vpc resources, resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

{{site.data.keyword.vpc_full}} uses [{{site.data.keyword.iamlong}}](/docs/iam?topic=iam-iamoverview) (IAM) for identity and access management. The service is called `VPC Infrastructure` and contains resource types as described in the following table. An access control level of `itself` means the resource has its own individual policies that can be applied.

Not all VPC resources are controlled by individual resource level access control policies. Some resources inherit the access control from the account or parent VPC.
{: important}

Service | Resource type | Resource instance | Access Control Level
---------------|---------------|---------------
VPC Infrastructure | Virtual Private Cloud  | vpc | itself
| Block Storage for VPC  | volume | itself  
| _Coming soon_     | floating ip* | Account
| Image Service for VPC  | image | itself
| Load Balancer for VPC  | load balancer | itself
| Network ACL  | network acl* | Account
| _Coming soon_     | public gateway | parent vpc
| Security Group for VPC  | security group | itself
| SSK Key for VPC  | key | itself
| Subnet  | subnet | parent vpc
| Virtual Server for VPC  | instance | itself
| VPN for VPC  | vpn | itself

Floating IPs and Network ACLs can be created with authorization at the account level if unassigned. However, as soon as a floating IP is assigned to an instance or an ACL is assigned to a subnet, these resources become subject to the VPC's authorization level.
{: note}

VPC Infrastructure uses an inherited model for access control. A role on the resource hierarchy, such as group, service, type or instance, is inherited to the resources down the hierarchy. The access to a resource is determined by the resulting union of roles on the resource.

## Resource policies
{: #resource-policies}

Generally, access to resources in a VPC is controlled by policies. If you want to customize access to resources for your VPC, you can create customized policies. A resource policy can target:

* all resources
* all resources of a type
* all resources in a group
* an individual resource

## Resources and resource groups
{: #resources-and-resource-groups}

A resource group is a collection of resources, say an entire VPC, or a single subnet and its ACL, that are associated for the purpose of establishing authorization and usage. You could think of a resource group as a collection of infrastructure that might be used by a project, a department, or a team.

You can use the search CLI to display tags attached to a resource. With the appropriate filters, you can display only the instances you care about, such as `ibmcloud resource search name:` or `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Large enterprises may wish to divide a VPC into resource groups. Smaller companies may not need to divide their VPC into resource groups, because all of their team would be using the entire VPC. You can learn more about [Working with resources](/docs/resources?topic=resources-resource).

Assignment of a resource to a resource group can be done ONLY when creating your VPC. Resources cannot change resource groups once they are created.
{: note}

When you're setting up your IBM Cloud VPC, if you want to use resource groups, it’s good to have a plan in place beforehand for how you want to assign the resources and the users in your organization to each resource group.
{: tip}

## VPC coverage summary table
{: #vpc-coverage-summary-table}

The following table summarizes the roles required to perform specific actions on VPC Infrastructure resources.

* As an **Administrator**, you can assign roles and take any available actions on resources.
* As an **Editor**, you can modify the state and create or delete resources.
* As an **Operator**, you can take actions that don't change the state of resources, for example, attaching or detaching network interfaces to a security group.
* As a **Viewer**, you can take actions that don't change the state of the resources.

For any policy that grants access by role (Administrator, Editor, Operator, Viewer), the following actions are given (X=denied, o=OK)

 role:        | none | Viewer | Operator | Editor | Administrator
-------------:|------|--------|----------|--------|-------
Assign roles  | X    | X      | X        | X      | o
Create        | X    | X      | X        | o      | o
List          | X    | o      | o        | o      | o
Read          | X    | o      | o        | o      | o
Attach/Detach | X    | X      | o        | o      | o
Update        | X    | X      | X        | o      | o
Delete        | X    | X      | X        | o      | o

The **Operator** role can view and list a resource and take some actions, such as attach and detach, that don't change the state of the resource. For example, an **Operator** role in `Security Group for VPC` and `Virtual Server for VPC` can attach or detach network interfaces to a security group.

For details on resource-specific actions, see [Roles required to manage VPC resources](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls).
