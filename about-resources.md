---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-11-27"

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

# About VPC infrastructure resources
{: #about-vpc-infrastructure-resources}

{{site.data.keyword.vpc_full}} uses [{{site.data.keyword.iamlong}}](/docs/iam?topic=iam-iamoverview) (IAM) for identity and access management. The service is called `VPC Infrastructure` and contains resource types as described in the following table. An access control level of `itself` means the resource has its own individual policies that can be applied.

Not all VPC resources are controlled by individual resource level access control policies. Some resources inherit the access control from the account or parent VPC.
{: important}

| Service | Resource type | Resource | Access Control Level | Support of custom resource groups |
| ---------------|---------------|---------------|-----|-----|
| VPC Infrastructure | Virtual Private Cloud  | vpc | itself | Yes |
| | Block Storage for VPC  | volume | itself  |  Yes |
| | Floating IP for VPC    | floating ip* | Account | No |
| | Image Service for VPC  | image | itself | Yes |
| | Load Balancer for VPC  | load balancer | itself | Yes  |
| | Network ACL  | network acl* | Account | No |
| | Public Gateway for VPC     | public gateway | parent vpc | No  |
| | Security Group for VPC  | security group | itself | Yes |
| | SSK Key for VPC  | key | itself | Yes |
| | Subnet  | subnet | parent vpc | No |
| | Virtual Server for VPC  | instance | itself | Yes |
| | VPN for VPC  | vpn | itself | Yes |

Floating IPs and network ACLs can be created with authorization at the account level if unassigned. However, as soon as a floating IP is assigned to an instance or an ACL is assigned to a subnet, these resources become subject to the VPC's authorization level.
{: note}

Floating IPs are always created in the _Default_ resource group so a user must have **Viewer** privileges on this resource group in order to have authorization to create Floating IPs.
{: important}

VPC infrastructure uses an inherited model for access control. A role on the resource hierarchy, such as group, service, type, or instance, is inherited to the resources down the hierarchy. The access to a resource is determined by the resulting union of roles on the resource. The following diagram is a visual representation of the model.

![IAM model for VPC Infrastructure](images/vpc-iam.svg "IAM model for VPC Infrastructure"){: caption="Figure: IAM model for VPC Infrastructure" caption-side="bottom"}

## Resource policies
{: #resource-policies}

Generally, access to resources in a VPC is controlled by policies. To customize access to resources for your VPC, create customized policies. A resource policy can target:

* All resources
* All resources of a type
* All resources in a group
* An individual resource

## Resources and resource groups
{: #resources-and-resource-groups}

A resource group is a collection of resources that is associated to establish authorization and usage. A resource group is a collection of infrastructure that a project, a department, or a team might use.

Use the search CLI to display tags attached to a resource. Use filters to display only the instances you care about, such as `ibmcloud resource search name:` or `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Large enterprises may want to divide a VPC into resource groups. Smaller companies may not need to divide their VPC into resource groups, because all of their team would be using the entire VPC. Learn more about [Working with resources](/docs/resources?topic=resources-resource).

Not all VPC resources can be assigned to a resource group. See the [table above](#about-vpc-infrastructure-resources) for specifics. In addition, assignment of a resource to a resource group can be done ONLY when you create it. Resources cannot change resource groups after they are created.
{: important}

Defining resource groups requires advance planning. Before you set up your {{site.data.keyword.cloud_notm}} VPC, know how you'll assign the resources and the users in your organization to each resource group.
{: tip}

## VPC coverage summary table
{: #vpc-coverage-summary-table}

The following table summarizes the roles required to perform specific actions on VPC Infrastructure resources.

* **Administrator** - Assign roles and take any available actions on resources
* **Editor** - Modify the state and create or delete resources
* **Operator** -  Perform actions that don't change the state of resources, such as attaching or detaching network interfaces to/from a security group
* **Viewer** - Take actions that don't change the state of the resources

For any policy that grants access by role (Administrator, Editor, Operator, Viewer), the following actions are given (X=denied, o=OK)

 Role         | None | Viewer | Operator | Editor | Administrator
-------------:|------|--------|----------|--------|-------
Assign roles  | X    | X      | X        | X      | o
Create        | X    | X      | X        | o      | o
List          | X    | o      | o        | o      | o
Read          | X    | o      | o        | o      | o
Attach/Detach | X    | X      | o        | o      | o
Update        | X    | X      | X        | o      | o
Delete        | X    | X      | X        | o      | o

The **Operator** role can view and list a resource and perform some actions, such as attach and detach, that don't change the state of the resource. For example, an **Operator** role in `Security Group for VPC` and `Virtual Server for VPC` can attach or detach network interfaces to a security group.

For details on resource-specific actions, see [Roles required to manage VPC resources](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls).
