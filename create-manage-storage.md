---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-07-01"

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
{:note: .note}
{:download: .download}

# Creating and managing block storage in VPC
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud provides a primary boot volume and allows additional block storage data volumes. These volumes are hypervisor-mounted, high-performance data storage for your virtual server instances. The VPC infrastructure provides rapid scaling of storage across multiple regions and zones, with extra performance and security.

## Block storage volume characteristics
{: #summary-of-characteristics}

When you provision an {{site.data.keyword.vsi_is_full}} instance, 100 GB general purpose block storage boot volume is automatically created and attached to the instance. You can rename this volume and choose to encrypt it using your own encryption keys, or use the default IBM-managed encryption.  As the primary volume, you can't manually detach and delete a boot volume, but the boot volume is deleted when you delete the instance.
{:shortdesc}

{{site.data.keyword.block_storage_is_short}} also provides secondary data volumes that have the following characteristics:

* You can create a block storage volume whenever you provision a virtual server instance in a VPC network. The volume is automatically attached to the instance. 
* You can create new volumes independent of the instance lifecycle and later attach them to an instance.
* Secondary data volumes persist when detached from the instance. This lets you to later attach the volume to another instance.
* You can specify whether data volumes are deleted automatically when you delete an instance.  
* Like boot volumes, data volumes are encrypted with IBM-managed encryption by default. You can choose to encrypt data volumes using your own encryption keys, during instance provisioning or when creating a standalone volume.

## Information about {{site.data.keyword.block_storage_is_short}}
{: #block-storage-volumes}

For overview information about {{site.data.keyword.block_storage_is_short}}, see [About Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

Complete information about creating and managing {{site.data.keyword.block_storage_is_short}} is available our [Block Storage for VPC documentation](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-getting-started).

## Best practices for creating and naming your {{site.data.keyword.block_storage_is_short}} volumes:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Determine your storage requirements before provisioning a volume. Allow for [adequate capacity and IOPS performance](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-capacity-performance).
* Decide whether a [predefined IOPS profile](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles#tiers) best meets your capacity and performance needs, or whether specifying [custom settings](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-profiles#custom) is better.
* Decide whether you want to create secondary data volumes during virtual server instance provisioning. These volumes are automatically attached to the instance.
* Decide whether you want to automatically delete a volume attached to an instance when you delete the instance.
* When encrypting volumes with your own encryption key, verify that the key is valid, that you have authorization to use the key, and that it can be used in your current region.
* Name all of your volumes at provision time, including the boot volume.
* Name all of your volume attachments at attachment time.
* Provide a unique name for each volume within a region within an account. All though you can reuse a volume name after deleting the original volume, re-using a volume name might cause billing confusion. As best practice, use a different name whenever possible.
{:note}
