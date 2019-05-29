---
copyright:
  years: 2018-2019
lastupdated: "2019-05-17"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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

# Connecting to IBM Cloud Object Storage from VPC 
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

This document shows you how to connect to IBM® Cloud Object Storage from your Virtual Private Cloud.

## What is IBM Cloud Object Storage (COS)?
{: #what-is-ibm-cloud-object-storage-cos}

IBM Cloud Object Storage (COS) is a web-scale platform that stores unstructured data. It provides reliability, security, availability, and disaster recovery without manual replication.
Information stored within IBM Cloud Object Storage is encrypted and dispersed across multiple geographic locations. It is accessible through an implementation of the S3 API. This service makes use of the distributed storage technologies provided by the IBM Cloud Object Storage service.

IBM COS is available in three configurations: **Cross-Region**, **Regional** and **Single-Site**.
 * **Cross Region** service provides higher durability and availability than using a single region, but at the cost of slightly higher latency.
 * **Regional** service provides the reverse: It distributes objects across multiple availability zones within a single region. If a given region or availability zone is inaccessible, the object store continues to function smoothly. Any missed changes are applied when the inaccessible datacenter comes back online.
 * **Single Site** service offers access to Cloud Object Storage in a selected datacenter.

### COS Direct Endpoints for use with VPC
{: #cos-direct-endpoints-for-use-with-vpc}

Endpoints are URLs that applications use to issue COS commands and exchange data with COS. Every endpoint uses the same Application Programming Interface (API) to interact with COS.
Servers provisioned within IBM Cloud use
API endpoints for services, including COS. Direct endpoints provide customers' IBM Cloud servers with high-speed, direct connections to services with no added bandwidth costs.

There is no charge for traffic from VPCs to all COS Endpoints listed on this page.
{: note}

## How to connect to IBM Cloud Object Storage (COS) from a VPC
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

 1. Provision COS from the [catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}.
 2. Create a COS bucket in one of the Regions listed in the following section.
 3. Use the Direct Endpoint for VPC to create an interface with your COS bucket.

## U.S. Cross-region Endpoints
{: #u-s-cross-region-endpoints}

| **U.S. Cross Region** | **Direct Endpoint for VPC** |
|------------|-------------------------------|
| US Cross Region | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| Dallas Access Point | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud`
| San Jose Access Point | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud`
| Washington, DC Access Point | `s3.direct.wdc.us.cloud-object-storage.appdomain.cloud` |

 ## U.S. Regional Endpoints
 {: #u-s-regional-endpoints}

| **Region** | **Direct Endpoint for VPC** |
|------------|-------------------------------|
| US South | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| US East | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|

 ## Single Datacenter Endpoints
 {: #single-datacenter-endpoints}

| **Region** | **Direct Endpoint for VPC** |
|------------|-------------------------------|
| Toronto, Canada | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
| Montreal, Canada | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
