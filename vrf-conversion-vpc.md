---

copyright:
  years: 2019
lastupdated: "2019-06-27"

keywords: vpc, VRF, IP, routers, backbone, service, VLAN, multiple isolation, tenant, tenancy, datacenters, data, center, shared tenancy, private endpoint, Customer VRF, Private Network Question, support, ticket

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Converting to VRF
{: #what-happens-during-the-account-conversion-process}

Many {{site.data.keyword.cloud_notm}} customers currently operate with a shared tenancy model on the {{site.data.keyword.cloud_notm}} network. During conversion, your shared tenancy is converted to use a _Customer VRF_, most commonly with a new Direct Link subscription or a Classic Access VPC.

You must convert your account to VRF to connect your VPC to your Classic resources.
{: note}

## The conversion process
{: #process-description}

The conversion process involves a network disruption, while the VLANs and their subnets are detached from the ACL backbone and then attached to the _Customer VRF_. This process results in a few moments of packet loss for traffic entering or exiting the VLANs. Packets within the VLAN continue to flow. In the cases where a network gateway, such as a FortiGate Security Appliance or Virtual Router Appliance is involved, no disruption occurs among the VLANs attached to that gateway. The servers see no network outage themselves, and most workloads automatically recover when the traffic flow resumes. The total duration of the disruption depends on the extent of the tenant’s topology, that is, the number of subnets, VLANs, and pods that your tenancy includes.

During migration, the VLANs are disconnected from the backbone and reconnected to the _Customer VRF_.  The duration of disruption varies, depending on the quantity of VLANs, PODs, and data centers involved. Traffic among VLANs is disrupted, yet the servers stay connected to the network. The application may or may not be affected, depending on its sensitivity to packet loss.

## How to initiate the conversion
{: #how-you-can-initiate-the-conversion}

Existing {{site.data.keyword.cloud_notm}} customers can [open a support case](https://cloud.ibm.com/unifiedsupport/cases/add)![External link icon](../../icons/launch-glyph.svg "External link icon") through the {{site.data.keyword.cloud_notm}} console, requesting to be migrated to a VRF. The support case should state: “Private Network Question” and include the following text:

"We are requesting that account _your account number_ is moved to its own VRF. We understand the risks and approve the change. Please reply with the scheduled window(s) of time where this change will be made so we can prepare for the migration."

Migration is completed by the {{site.data.keyword.cloud_notm}} Network Engineering team. No other information is required from you, except an agreed-to schedule. Typically, packet loss might last 15 - 30 minutes, depending on the complexity of your account. It might be longer if your account has legacy Direct Link connections. The process is highly automated, requiring minimal interaction by the IBM team, and it should be transparent.

When you're opening the support case, it's recommended to select the "Technical" support type option, with category **VPC > Network**.

