---

copyright:
  years: 2019
lastupdated: "2019-10-17"

keywords: vpc, api, change log, new features, restrictions, migration, generation 2, gen2,

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


# API change log
{: #api-change-log}

This page contains information about {{site.data.keyword.vpc_full}} (VPC) [API](https://{DomainName}/apidocs/vpc-on-classic) improvements and fixes. It also contains guidance on the client code updates required to use a new date-based version. By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code.

If backward-incompatible changes require extensive client code changes in order to use an API version, this page links you to detailed migration instructions and examples about how to migrate client code.
{:note}

To minimize regressions from changes, we recommend the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status codes, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid any behavior that is not explicitly documented

## 2019-10-08
{: #2019-10-08}

The VPC API version for use with generation 1 compute resources has been updated from `2019-07-30` to `2019-10-08`. 


## 2019-08-13
{: #2019-08-13}

**Default address prefixes for a VPC** 

Create a VPC without a default address prefix by specifying the option `address_prefix_management=manual` in the request. 

Learn more about [Designing an Addressing Plan for a VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).

## 2019-08-06
{: #2019-08-06}

**New API version** `2019-07-30` includes enhanced field and parameter validation. 

See the [VPC API documentation](https://{DomainName}/apidocs/vpc-on-classic){: external} for information on usage.

## 2019-06-14
{: #2019-06-14}

**Updates to the SDK**

* Terraform provider v0.17.1 has been released. Download the [latest binary](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1){: external}.
* The docker image has been updated with the latest Terraform provider. You can pull the latest Docker image by using the command `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
Provider arguments (`bluemix_api_key` and `bluemix_timeout`) have been deprecated. As a result, a warning could appear when you run Terraform plan or apply.

Remember to set `generation` as a provider argument or export `IC_GENERATION= 1` so that your code will work with the currently released version of VPC for generation 1 compute resources. By default, the value is set to 2.
{: tip}

## 2019-06-07
{: #2019-06-07}

{{site.data.keyword.vpc_full}} (VPC) is now generally available.

The port speed parameter for a virtual network interface has been removed from the API.

## 2019-05-31
{: #2019-05-31}

The VPC API version for use with generation 1 compute resources has been updated from `2019-01-01` to `2019-05-31`. 

For guidelines and best practices, see [Versioning](https://{DomainName}/apidocs/vpc-on-classic#versioning){: external}.
