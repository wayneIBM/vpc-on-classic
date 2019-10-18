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

To minimize regressions from changes, we recommend the following best practices when you call the API:

* Catch and log any `4xx` or `5xx` HTTP status codes, along with the included `trace` property
* Follow HTTP redirect rules for any `3xx` HTTP status code
* Consume only the resources and properties your application needs to function
* Avoid any behavior that is not explicitly documented

## 2019-10-08
{: #2019-10-08}

The `3des` enumeration value encryption algorithms on IKE and IPsec policies has been changed to `triple_des` to improve compatibility with various programming environments. **Version 2019-10-08 or later required.**

## 2019-09-17
{: #2019-09-17}

VPCs can be created without any default address prefixes by specifying the option `"address_prefix_management": "manual"` and default address prefixes can be deleted. **Version 2019-09-17 or later required.**

Learn about [designing an addressing plan for a VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
