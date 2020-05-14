---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-01"

keywords: 

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:external: target="_blank" .external}

# Release notes
{: #release-notes}

Use these release notes to learn about the latest changes to {{site.data.keyword.vpc_full}}.
{:shortdesc}

## 1 May 2020
{: #may-1-2020}

**New version `0.5.14` of the CLI plug-in `vpc-infrastructure` has been released**, which includes the following enhancements: 
* Added HTTPS protocol and HTTPS health-type support for the **load-balancer-pool-create** command.
* Added HTTPS protocol and HTTPS health-type support for the **load-balancer-pool-update** command.
* Added floating IP data to the **instance** command JSON output.

To update the `vpc-infrastructure` plug-in, see the [CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#cli-prerequisites).

## 17 April 2020
{: #april-17-2020}

**New version `0.5.13` of the CLI plug-in `vpc-infrastructure` has been released**, which includes the following enhancements:
- Usage examples for **key-create** and **key-update** commands.
- Command output includes Cloud Resource Name (CRN) fields now.

To update the `vpc-infrastructure` plug-in, see the [CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#cli-prerequisites).

## 3 April 2020
{: #april-3-2020}

**Updates to Load Balancer for VPC**

- You can now access load balancer monitoring metrics (throughput, active connections, connection rate) using [IBM Cloud Monitoring with Sysdig](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-monitoring-metrics-sysdig).
- The following cipher suites are supported for load balancer HTTPS listeners:
    - `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
    - `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
    - `TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256`
    - `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
    - `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
    - `TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256`

**Updates to Block Storage for VPC**

- Block storage volumes quota has been decreased to 300 per account per region. You can request that your storage quota be increased by opening a [support case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}. See [Block storage volume quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#block-storage-quotas) for more quota information.

## 27 March 2020
{: #march-27-2020}

**Updates to the API**
- Starting with API version `2020-03-24`, the requirement to include the `generation=1` query parameter will be enforced for the {{site.data.keyword.vpc_full}} (Gen1 Compute) API.

**Updates to the UI**
- You can now monitor virtual server instances using {{site.data.keyword.mon_full_notm}}. Use the new **Add monitoring** button on the instance's **Monitoring** page to provision an instance of the monitoring service. If a monitoring instance is already provisioned for the region, use the **Launch monitoring** button to view metrics associated with the instance.
- Virtual Private Cloud UI pages were updated to use [Carbon 10](https://www.carbondesignsystem.com/){: external} --IBM's open-source design system, which improves UI consistency and quality.

## 6 March 2020
{: #march-6-2020}

**Virtual server instance quota has been decreased** to 20 per region. You can always request that your quota be increased by opening a [support case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}. See [VSI quotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vsi-quotas) for more quota information.

**New version `0.5.11` of the CLI plug-in `vpc-infrastructure` has been released**, which includes the following enhancements:
- Command output for volume details `ibmcloud is volume` includes resource tags now.
- Usage examples have been added to the [CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference).
- Non-English usage help has been updated.

## 28 February 2020
{: #feb-28-2020}

- **Washington, DC** region is [now available](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region).
- **Updates to the UI**
   - New detail pages have been added for SSL keys and Floating IPs.
   - The attach instance action for Load Balancers backend pool creation has been moved to a side panel.

## 21 February 2020
{: #feb-21-2020}

**Updates to the UI**
- More accurate pricing estimates available for Load Balancers based on selected region.
- More accurate pricing estimates available for instances based on selected GPU profile.
- Enhanced network monitoring data available for instances with added support for aggregated metrics.

**Upcoming update to the API**
- Starting with API version `2020-03-24`, the requirement to include the `generation=1` query parameter will be enforced for the {{site.data.keyword.vpc_full}} (Gen1 Compute) API.

## 31 January 2020
{: #jan-31-2020}

**Updates to the UI**
- The maximum subnet size was increased from `/20` to `/8`.
- Public gateway details are now available in the public gateway page.
- The load balancer pool creation form has been moved to a side panel.
- The SSH key provision form has been moved to a side panel.
- The floating IP reserve and associate form has been moved to a side panel.


## 24 January 2020
{: #jan-24-2020}

**VPN for Virtual Private Cloud log update**
- You can now use {{site.data.keyword.la_full_notm}} to view application and connection logs from your VPN for a VPC gateway. See [Using LogDNA to view VPN logs](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-logdna-to-view-vpn-logs) for instructions on how to do this.

## 10 January 2020
{: #jan-10-2020}

**CLI enhancements**

- Interactive mode to create an instance (for example, `ibmcloud is instance-create --interactive`)
- Help command example section for create and update (for example, `ibmcloud is help instance-create`, `ibmcloud is help instance-update`, and `ibmcloud is help volume-create`)
- Resource group filter for list commands (for example, `ibmcloud is vpcs --resource-group-name Littleton`)
- JSON output format support for the `ibmcloud is target --json` command
- `--ipv4` and `--primary-network-interface` options for `instance-create`, which allows you to specify the primary private IP for the primary network interface while creating an instance.

Refer to the [IBM Cloud CLI for VPC reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference) for the new options.

## 13 December 2019
{: #dec-13-2019}

- **Monitoring data for block storage volumes** is now available in the {{site.data.keyword.cloud_notm}} console. For more information, see [Monitoring block storage volumes](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-monitor-block-storage-vpc).
- **Cloud Service Endpoint (CSE) source addresses** are now available in the VPC overview page on the {{site.data.keyword.cloud_notm}} console. To view the data, select a VPC from the [list](https://cloud.ibm.com/vpc/network/vpcs){: external}.

## 6 December 2019
{: #dec-6-2019}

**New version `0.5.9` of the CLI plug-in `vpc-infrastructure` has been released**, which includes the following enhancements:
- The command `ibmcloud target -g YOUR_GROUP` can be used to filter VPC resources in that resource group only
- Get commands now show Name and ID in separate columns
- The instance list command now shows floating IP data
- The instance details command now shows network interface data

Not all VPC resources can be assigned to a resource group. See the [resource table](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources) for specifics.
{: important}

## 22 November 2019
{: #nov-22-2019}

- **VPC layout** You can quickly view the resources that are associated with a VPC in the {{site.data.keyword.cloud_notm}} console. For more information, see [Viewing resources associated with a VPC](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console#vpc-layout).

## 15 November 2019
{: #nov-15-2019}

- **Cost estimates** with more detail are now available when you order [Load Balancers for VPC](https://cloud.ibm.com/vpc/provision/loadBalancer).
- **View attached resources of a VPC or subnet using the CLI** with the `--show-attached` option. See the [VPC CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference) for details.


## 18 October 2019
{: #oct-18-2019}

- **General availability of generation 2 virtual server profiles in a VPC**. For more information, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).
- **Security Group names no longer have to be unique in an account**. However, the Security Group name must still be unique in the VPC. To learn more about Security Groups, see [Using Security Groups](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).

## 11 October 2019
{: #oct-11-2019}

- **Cost estimates** are now available from the [VPC instance provisioning](https://cloud.ibm.com/vpc/provision/vs){: external} user interface. Press the **Add to estimate** button to add the cost to the estimate and then you can **Review estimate** or **Clear estimate** when finished.

## 04 October 2019
{: #oct-04-2019}

**Activity Tracker with LogDNA Location update**
- Activity Tracker with LogDNA is now available in the London and Sydney data centers. These locations are now being used by VPC. See [Supported Locations](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations) for more information.

## 27 September 2019
{: #sept-27-2019}

**Updates to the UI**
- Tags can now be added to Floating IPs.
- Custom network routes can now be added to a Virtual Private Cloud from the user interface. Click on the **Routes** menu option in the Virtual Private Cloud details page.
- A metric overview for CPU, memory, and traffic is now available in the instance overview page.
- Price estimates for VPC, VPN, Load Balancer, volumes, and custom images are now available in the provisioning pages.

**Activity Tracker with LogDNA Location update**
- Activity Tracker with LogDNA is now available in the London and Sydney data centers. These locations will be used by VPC starting on **October 3, 2019**. Currently, VPC events in London and Sydney were being sent to Frankfurt but after October 3, these events will be sent to London and Sydney accordingly.

## 13 September 2019
{: #sept-13-2019}

- **Default address prefixes for a VPC** can now be customized using the [API](https://{DomainName}/apidocs/vpc-on-classic#create-a-vpc){: external}. Starting with API version `2019-08-27`, a VPC can be created without a default address prefix by specifying the option `address_prefix_management=manual` in the request. Learn more about [Designing an Addressing Plan for a VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).

## 6 September 2019
{: #sept-06-2019}

- **New API version** is available as of `2019-08-06`, and includes enhanced field and parameter validation. Refer to the [VPC API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for the correct usage.

## 23 August 2019
{: #aug-23-2019}

- **Live Chat** is now available from the VPC Infrastructure user interfaces pages.
- **User Interface improvements** allow you to create a subnet from the VPC detail page and attach an existing public gateway to a subnet.
- **Custom VPC Routes** can now be created using the command line interface. Learn more in the [CLI reference](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#vpc-routes).

## 2 August 2019
{: #aug-02-2019}

**Custom Images:** You can now import your own images (in VHD format).  After the image has been uploaded to a region, you can use that image to provision new instances in that region. You can rename the image or delete it when you no longer need it.
- Learn about [importing and managing images](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-managing-images).
- Start [importing your custom images using the UI](https://{DomainName}/vpc/compute/images){: external}.
- Learn how to import and use [custom images using the CLI](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-reference#image-create).
- Use version `2019-07-30` when you use the [Create Image API](https://{DomainName}/apidocs/vpc-on-classic#create-an-image){: external}.

## 26 July 2019
{: #july-26-2019}

- **Advanced Routing** is now available in Networking for VPC. Refer to [Setting Up Advanced Routing in VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-advanced-routing-in-vpc)
for information on how to create these custom routes.

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

- **Terraform provider v0.17.1** has been released, download the [latest binary](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1){: external}.
- **Docker Image** also has been updated with the latest Terraform provider. You can pull the latest Docker image by using this command:  `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Remember to set `generation` as a provider argument or export `IC_GENERATION= 1` so that your code will work with the currently released version of VPC for generation 1 compute resources. By default the value is set to 2 (VPC, coming soon, now in Beta).
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

**New API version available.** The VPC API version for use with generation 1 compute resources has been updated from  `2019-01-01` to `2019-05-31`. For guidelines and best practices, see  [Versioning](https://{DomainName}/apidocs/vpc-on-classic#versioning){: external}.

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
