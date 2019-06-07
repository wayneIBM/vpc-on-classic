---

copyright:
  years: 2019
lastupdated: "2019-06-06"

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

# Release notes
{: #release-notes}

Use these release notes to learn about the latest changes to {{site.data.keyword.cloud}} Virtual Private Cloud.
{:shortdesc}

## 7 June 2019
{: #june-07-2019}

- **IBM Cloud VPC is now generally available.** All [Pay-As-You-Go accounts](/docs/account?topic=account-accounts) can provision VPC resources.
Log in to the [IBM Cloud Console](https://{DomainName}/vpc/overview) and get started today!

- **Individual authorization policies on instances, keys, volumes, and security groups can now be set.** Learn what roles are required to manage these resources in [Resource authorizations required for API and CLI calls](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Early access users are not affected with existing policies.

- **The port speed parameter for a virtual network interface has been removed from the User Interface, CLI and API.**

- **Multi-region support for monitoring metrics in now available in the User Interface.**


## 31 May 2019
{: #may-31-2019}

**New API version available.** The VPC on Classic API version has been updated from `2019-01-01` to `2019-05-31`. For guidelines and best practices, see  [Versioning](https://{DomainName}/apidocs/vpc-on-classic#versioning) in the Virtual Private Cloud on Classic API.

## 24 May 2019
{: #may-24-2019}

- **Load Balancer instances in deleting status now show in the list view in the User Interface.** When you request to delete a load balancer in the UI, the list view continues to show the load balancer until the delete process is completed.

- **The Layer-7 Load Balancer features are available in the User Interface.** You can now configure Layer-7 load balancer features in the User Interface.

- **Load Balancer view and edit policies are now available in the User Interface.** A new function, to view and edit load balancer policies, is available in the user interface.

For more information, see the [Load balancer documentation](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


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
