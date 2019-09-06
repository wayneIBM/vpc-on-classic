---

copyright:
  years: 2019
lastupdated: "2019-09-06"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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
{:external: target="_blank" .external}

# Release notes
{: #release-notes}

Use these release notes to learn about the latest changes to {{site.data.keyword.vpc_full}}.
{:shortdesc}

## 6 September 2019

- **New API version** is available as of `2019-08-06`, and includes enhanced field and parameter validation. Refer to the [VPC API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for the correct usage.
- **Activity Tracker with LogDNA** is available in new locations. These locations will be used by VPC starting on **September 26, 2019**. The list of [supported locations](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations) will be updated and instructions will be provided on how to get events in the new locations.

## 23 August 2019
{: #aug-23-2019}

- **Live Chat** is now available from the VPC Infrastructure user interfaces pages.
- **User Interface improvements** allow you to create a subnet from the VPC detail page and attach an existing public gateway to a subnet.
- **Custom VPC Routes** can now be created via the command line interface. Learn more in the [CLI reference](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-routes).

## 2 August 2019
{: #aug-02-2019}

**Custom Images:** You can now import your own images (in VHD format).  After the image has been uploaded to a region, you can use that image to provision new instances in that region. You can rename the image or delete it when you no longer need it.
- Learn about [importing and managing images](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-managing-images).
- Start [importing your custom images using the UI](https://{DomainName}/vpc/compute/images){: external}.
- Learn how to import and use [custom images using the CLI](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#image-create).
- Use version `2019-07-30` when you use the [Create Image API](https://{DomainName}/apidocs/vpc-on-classic#create-an-image){: external}.

## 26 July 2019
{: #july-26-2019}

- **Advanced Routing** is now available in Networking for VPC. Refer to [Setting Up Advanced Routing in VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-advanced-routing-in-vpc)
for use cases and examples.

- **Improved instance monitoring data** with filtering is available in the User Interface.

## 12 July 2019
{: #july-12-2019}

**Updates to the CLI**


- **The load balancer listener policy is enabled**, including commands for `list`, `get`, `create`, `update`, and `delete` of a policy.

- **New commands added for policy rules** in the load balancer listener, including commands for `list`, `get`, `create`, `update`, and `delete` of a rule.

- **Support added for tilde to designate the home directory** for SSH key file path and also for VSI provisioning of volume or user data file paths.

- **Added CPU, memory, and network performance** information about the profile to the `instance-profile` list command.

**Updates to the UI**

- **The live chat widget is available.**

- **Public gateways now are represented in the UI as first-class objects.**

## 21 June 2019
{: #june-21-2019}

- **London and Sydney** regions are [now available](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region).
- **Rule Priority** has been added to the UI in the ACL provisioning page. You can see and set the priority for an ACL rule when you create the rule.
- **The UI has been updated** so that 3 IOPS/GB tier is followed by the tier name, in this case, **general-purpose**.

## 14 June 2019
{: #june-14-2019}

**Updates to the IBM Cloud VPC UI**

- **SSH keys and resource groups.**
    * Resource groups are shown in the SSH key list view.
    * Resource groups are selectable in the SSH key provisioning modal.
- **Profiles and bandwidth.** The bandwidth cap assigned to each profile is shown in the **Popular profiles** and the **All profiles** display pages.
- **Resource groups.** The VSI provisioning page shows the resource group dropdown.

**Updates to Block Storage for VPC**
- The **volume name length** has been increased to 63 alpha-numeric characters.
- The following **volume error codes** have been renamed and messages updated, or deleted:
    * `volume_encryption_key_account_id_mismatch` is renamed to `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` is renamed to `volume_crn_cname_mismatch`
    * `volume_encryption_key_region_not_found` has been removed

**Updates to the SDK**

- **Terraform provider v0.17.1** has been released. Please download the [latest binary](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1){: external}.
- **Docker Image** also has been updated with the latest Terraform provider. You can pull the latest Docker image by using this command:  `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Remember to set `generation` as a provider argument or export `IC_GENERATION= 1` so that your code will work with the currently released version of VPC on classic infrastructure. By default the value is set to 2 (VPC, coming soon, now in Beta).
- Provider arguments(`bluemix_api_key` and `bluemix_timeout`) have been deprecated, so a warning could be thrown when you run Terraform plan or apply.

## 7 June 2019
{: #june-07-2019}

- **IBM Cloud VPC is now generally available.** All [Pay-As-You-Go accounts](/docs/account?topic=account-accounts) can provision VPC resources.
Log in to the [IBM Cloud Console](https://{DomainName}/vpc/overview){: external} and get started today!

- **Individual authorization policies on instances, keys, volumes, and security groups can now be set.** Learn what roles are required to manage these resources in [Resource authorizations required for API and CLI calls](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Early access users are not affected with existing policies.

- **The port speed parameter for a virtual network interface has been removed from the User Interface, CLI and API.**

- **Multi-region support for monitoring metrics in now available in the User Interface.**


## 31 May 2019
{: #may-31-2019}

**New API version available.** The VPC on Classic API version has been updated from `2019-01-01` to `2019-05-31`. For guidelines and best practices, see  [Versioning](https://{DomainName}/apidocs/vpc-on-classic#versioning){: external} in the Virtual Private Cloud on Classic API.

## 24 May 2019
{: #may-24-2019}

- **Load Balancer instances in deleting status now show in the list view in the User Interface.** When you request to delete a load balancer in the UI, the list view continues to show the load balancer until the delete process is completed.

- **The Layer-7 Load Balancer features are available in the User Interface.** You can now configure Layer-7 load balancer features in the User Interface.

- **Load Balancer view and edit policies are now available in the User Interface.** A new function, to view and edit load balancer policies, is available in the user interface.

For more information, see the [Load balancer documentation](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10 May 2019
{: #may-10-2019}


- **Profile names have changed**. Instance profile names have changed. Commands to provision instances need to be updated to use the new profile names. Read more [about profiles here](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **Estimated monthly Floating IP pricing is available in the User Interface**. As you reserve a Floating IP address, you now can see a line showing the estimated monthly price for that Floating IP.

- **Hyper protect support is available**.

- **Subnet name and zone appear as links that will take you to the associated subnet in the User Interface**. Subnets are now listed by name rather than subnet ID, and the name acts as a link to the subnet details page for the associated subnet.

- **The cost summary estimator is available in the User Interface**.

- **Polling for Load Balancer pools and listeners is reflected in the User Interface**.

    * On the load balancer pools page, you now see a counter that shows the last update, below the resource table.
    * The default polling interval is 30 seconds.
    * Four (4) different states can be displayed: `newProvision`, `active`, `failed`, and `all other states`.
        * For a new provision: the interval is 15 seconds. The pools and listeners go to active state right away.
        * For active: the resource list and header are updated  every 60 seconds.
        * For failed: Polling is stopped.
        * For all other states: polling continues at 30 second intervals.
    * If you delete a listener, you can trigger the header status to show `Updating (Actions Unavailable)`.
    * Also notice that the header info is updated when an update occurs.
    * These same changes apply to polling for listeners.

- **Load Balancer member count is shown in the members and details page in the User Interface, while polling of members is happening**.

    * In the health status section, you see the member count while the "Fetching instance details" is next to it.
    * In the Instances column in the resource table, you see the member count while the "Fetching instance details" is next to it.
    * Once loaded, if you have invalid members, you'll see the yellow caution icon.

- **The VPC details page in the User Interface shows all attached subnets**.
