---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: resource, access, role, action, permission, assign, administrator, operator, editor, viewer, user, managing

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Planning {{site.data.keyword.vsi_is_short}} Permissions
{: #planning-virtual-servers-for-vpc-permissions}

When you're planning to provision {{site.data.keyword.vsi_is_full}}, you must understand the virtual server access available based on your user role.
{:shortdesc}

Review the following table to learn more about user roles and the specific level of access each role encompasses.

* As an administrator you can define roles and take any available actions on {{site.data.keyword.vsi_is_short}}.
* As an editor you can modify the state and create or delete subresources.
* As an operator you can take actions that don't change the state of resources.
* As a viewer you can take actions that don't change the state of the resources.

<table>
<CAPTION>Table 1. User permissions</CAPTION>
<THEAD>
<TR>
<th>Virtual Server for VPC role</th>
<th>Description</th>
<th>Actions</th>
</TR>
</THEAD>
<TBODY>
<tr>
<td>Admin</td>
<td>All actions including the ability to manage <br>
access control.</td>
<td>
Access control:
<ul>
<li>Add and remove users</li>
<li>Assign roles for each user</li>
</ul>
<p>
Virtual Servers:
<ul>
<li>Create virtual servers</li>
<!--  <li>Stop virtual servers</li> -->
<li>Delete virtual servers</li>
<!-- <li>Restart virtual servers</li> -->
<!-- <li>Duplicate virtual servers</li> -->
<!-- <li>Resize virtual servers</li> -->
<!-- <li>Add and delete vNICs</li> -->
<!-- <li>Attach and delete volumes</li> -->
<li>View and list virtual servers</li> 
<li>Update virtual servers</li>
<!--  <li>Rename virtual servers</li>
<!-- <li>Create image snapshots</li> -->
<!-- <li>Delete image snapshots</li> -->
<!-- <li>Create virtual servers off of image snapshots</li> -->
<!-- <li>Create and edit SSH keys</li> -->
<!-- <li>Delete SSH keys</li> -->
<!-- <li>Add autoscaling policies</li> -->
<!-- <li>Delete autoscaling policies</li> -->
<!-- <li>Modify autoscaling policies</li> -->
<!-- <li>View monitoring and log data</li> -->
<!-- <li>Modify alarms and notifications from monitoring</li> -->
</ul>
</p>
</td>
</tr>
<tr>
<td>Editor</td>
<td>Actions that can modify the state, as well <br>
as, create and delete sub-resources.</td>
<td>
Virtual Servers:
<ul>
<li>Create virtual servers</li>
<!-- <li>Stop virtual servers</li> -->
<li>Delete virtual servers</li>
<li>View and list virtual servers</li>
<li>Update virtual servers</li>
<!-- <li>Restart virtual servers</li> -->
<!-- <li>Duplicate virtual servers</li> -->
<!-- <li>Resize virtual servers</li> -->
<!-- <li>Add and delete vNICs</li> -->
<!-- <li>Attach and detach volumes</li> -->
<!-- <li>Rename virtual servers</li> -->
<!-- <li>Create image snapshots</li> -->
<!-- <li>Delete image snapshots</li> -->
<!-- <li>Create virtual servers off of image snapshots</li> -->
<!-- <li>Create SSH keys</li> -->
<!-- <li>Delete SSH keys</li> -->
<!-- <li>Add autoscaling policies</li> -->
<!-- <li>Delete autoscaling policies</li> -->
<!-- <li>Modify autoscaling policies</li> -->
<!-- <li>View monitoring and log data</li> -->
<!-- <li>Modify alarms and notifications from monitoring</li> -->
</ul>     
</td>
</tr>
<tr>
<td>Operator</td>
<td>Actions that do not change state</td>
<td>
Virtual Servers:
<ul>
<li>View and list virtual servers</li>
<!-- <li>View and list image snapshots</li> -->
<!-- <li>View monitoring and log data</li> -->
</ul>
</td>
</tr>
<tr>
<td>Viewer</td>
<td>Actions that do not change state</td>
<td>
Virtual Servers:
<ul>
<li>View and list virtual servers</li>
<!-- <li>View and list image snapshots</li> -->
<!-- <li>View monitoring and log data</li> -->
</ul>
</td>
</tr>
</TBODY>
</table>

When you create an instance, you must also have Operator access for the VPC and Security Group resources, if those resources are specified. Subnet and Floating IP resources inherit permissions from the associated VPC.  
{: tip}

## Next steps
{: #next-manage-vpc-resource-permissions}

For more information on how to change a user's permissions, see [Managing user permissions for VPC resources](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).
