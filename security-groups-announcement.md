---

copyright:
  years: 2019

lastupdated: "2019-05-28"

keywords: security groups, authorization, enhancement, policies, release, phase 2

subcollection: vpc

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

# Announcing Enhanced Authorization for Security Groups, Instances, and Keys
{: #security-groups-announcement}

{{site.data.keyword.cloud}} is improving authorization by adding more granularity for instances, keys, and security groups. Due to this enhancement, users need to be granted additional privileges to continue managing these resources. Refer to [Impact and Actions](#impact-and-actions) for more information. 

VPC resource authorization updates released on **June 7, 2019** impact all customers in all regions.
{: important}

## Impact and actions
{: #impact-and-actions}

The authorization enhancements require new roles and policies for customers to manage instances, keys, and security groups.

For about an hour after the updates are released, users will receive errors (with code 404 or 500) when calling APIs on newly created security groups. The errors should clear after an hour.
{: important}

**Access to resources authorized by policies on the parent VPC will be lost**

Customers will lose access to security group, instance, and key resources that are authorized only by policies on the parent VPC. These policies will no longer be sufficient. To restore access, new policies must be created, using the new authorization model described in this document.

**How to minimize disruption**

To minimize disruption over this transition, consider temporarily granting users overly-broad policies to **All VPC Infrastructure services** before the release happens. Users with higher-level permissions will be unaffected by the release. Once the release is complete, you can restrict access policies using the new authorization model.

Access groups can be used to apply policy changes like this to many users at once.
{: tip}

## Authorization change details

Currently, access to a resource is determined by your access to the parent VPC. In other words, if you have access to a VPC, you have access to all the resources within that VPC. For more details, please refer to [About VPC Infrastructure resources](/docs/infrastructure/vpc?topic=vpc-about-vpc-infrastructure-resources).

**Policies now apply directly to resources, not to parent VPC**

With the upcoming release, access to instances, keys, and security groups will be determined by policies granted directly on the resource itself, instead of its parent VPC. See the [Examples](#auth-change-examples) section that follows.

In future releases, other VPC resource types will have similar authorization models.
{: note}

**Changes to roles and permissions for multiple resources**

As before, the role of a policy (Administrator, Editor, Operator, Viewer) determines the actions that are authorized. For example, some operations will deny (x) or allow (OK) operations with the following roles:


|  | **Role:**  | **None** | **Viewer** | **Operator** | **Editor** | **Administrator** |
|------|------|------|-------|-------|------|---------|
|**Desired Action** |  | | | | | |
| Create|  | x | x | x | OK | OK |
| List|  | x | OK | OK | OK | OK |
| Read|  | x | OK | OK | OK | OK |
|Update |  | x | x | x | OK | OK |
| Delete|  | x | x | x | OK | OK |
|Attach Security Group |  | x | x | OK | OK | OK |
|Associate Floating IP |  | x | x | x | OK | OK |


**Changes needed to permissions**

After this release, some actions will require access to more than a single resource. For example, to attach a security group to an instance's network interface will require two permissions:

* **Operator** role on the security group
* **Editor** role on the instance

To create an instance will require these resource permissions:

* **Editor** role on the instance
* **Editor** role on the volume
* **Operator** role on the VPC and on the security group


An administrator can authorize this access using a single policy; for example, by granting **Editor** permission on a resource group containing the affected instances, keys, and security groups. Because the **Editor** role's access is more powerful than the Operator role's, the **Editor** role suffices to fulfill both requirements.

Alternatively, an administrator could use two separate policies; for example, by granting the **Operator** role on all security groups, the **Editor** role on particular instances, and the **Editor** role on specific key instances.

For details of each authorization that will be required for each operation, see [Resource authorizations required for API and CLI calls](/docs/infrastructure/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls).

## Examples
{: #auth-change-examples}

Consider a user who has a policy granting the **Editor** role on a VPC. This role currently allows that user to edit all resources within that VPC.

After this release, such a user will lose access to the instances, keys and security groups in the VPC. The user's access to other resource types in the VPC will be unaffected.

To restore that user's access to instances, keys, and security groups, administrators should create policies that grant the user access to those resources directly. This change can be accomplished in a variety of ways. For example:

* Grant **Editor** role over all VPC Infrastructure resources. This role has a very broad scope, giving the user access to all resources in the account.
* Create one policy granting **Editor** role to all instances, another granting **Editor** role to all security groups, and also granting **Editor** role to all keys.
* Create a resource group containing the user's instances, keys, and security groups, and create a policy granting the user **Editor** role on that resource group.
* Create policies granting the user **Editor** role on individual security groups,instances and keys by ID. This policy is very granular, and it could be hard to maintain if the user's set of resources changes over time.
