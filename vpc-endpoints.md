---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-12"

keywords: vpc, CSE, endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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
{:DomainName: data-hd-keyref="DomainName"}

# Service endpoints available for IBM Cloud VPC
{: #service-endpoints-available-for-ibm-cloud-vpc}

When you're ready to run workloads, you can reach two types of endpoints: Infrastructure as a Service (IaaS) endpoints and Cloud Service Endpoints (CSE). The IaaS endpoints are pre-provisioned and ready to go; however, the CSEs must be provisioned separately before they can be used.

Endpoints for these services use _routable addresses_; that is, addresses outside of those specified in RFC 1918, and these addresses might look as if they are communicating through the public Internet, but traffic to and from these endpoints does not leave the cloud. Therefore, this traffic avoids the bandwidth charges associated with traffic that exits the cloud and goes onto the public Internet.

## Infrastructure as a Service (IaaS) endpoints
{: #infrastructure-as-a-service-iaas-endpoints}

Infrastructure services are available by using certain DNS names from the `adn.networklayer.com` domain, and they resolve to `161.26.0.0/16` addresses.

Services you can reach include:

* DNS resolvers
* Ubuntu APT (Advanced Packaging Tool) Mirrors
* Cloud Object Storage
* NTP

## DNS (Domain Name System) resolver endpoints
{: #dns-domain-name-system-resolver-endpoints}

DNS resolvers use IP address, rather than names. For shared cloud service endpoints, the DNS server addresses `161.26.0.10` and `161.26.0.11` should be used.

## Ubuntu APT Mirrors
{: #ubuntu-apt-mirrors}

APT mirrors for updating Ubuntu images are available from `mirrors.adn.networklayer.com`, which should resolve to `161.26.0.6`.

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

For example, to reach a private endpoint for an object storage service on the IBM Cloud private network, replace the "domain" portion of the URL with `cloud-object-storage.appdomain.cloud`. To reach a COS bucket created in another region,
use the endpoint for the bucket, not the endpoint of the region in which the VSI has been created.

## Network Time Protocol (NTP) servers
{: #network-time-protocol-ntp-servers}

An NTP server is available from `time.adn.networklayer.com`, which should resolve to `161.26.0.6`.

## Cloud Service Endpoints
{: #cloud-service-endpoints}

Cloud service endpoints are services provided by other cloud users. They will be available soon through DNS names in the `cloud.ibm.com` domain. They resolve to `166.8.0.0/14` addresses.
