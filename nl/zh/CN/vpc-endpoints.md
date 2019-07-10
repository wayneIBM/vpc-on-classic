---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-12"

keywords: endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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

# 可用于 IBM Cloud VPC 的服务端点
{: #service-endpoints-available-for-ibm-cloud-vpc}

准备好运行工作负载时，可以访问两种类型的端点：基础架构即服务 (IaaS) 端点和云服务端点 (CSE)。IaaS 端点已预先供应并可随时使用；但是，CSE 必须单独供应后才能使用。

这些服务的端点使用_可路由地址_；即，RFC 1918 中指定的地址之外的地址，这些地址可能看起来好像是通过公用因特网进行通信，但实际进出这些端点的流量不会离开云。因此，这种流量不会发生与从云流出并流入公用因特网的流量关联的带宽费用。

## 基础架构即服务 (IaaS) 端点
{: #infrastructure-as-a-service-iaas-endpoints}

基础架构服务使用 `adn.networklayer.com` 域中的特定 DNS 名称提供，并解析为 `161.26.0.0/16` 地址。

可以访问的服务包括：

* DNS 解析器
* Ubuntu APT（高级打包工具）镜像
* Cloud Object Storage
* NTP

## DNS（域名系统）解析器端点
{: #dns-domain-name-system-resolver-endpoints}

DNS 解析器使用的是 IP 地址，而不是名称。对于共享云服务端点，应该使用 DNS 服务器地址 `161.26.0.10` 和 `161.26.0.11`。

## Ubuntu APT 镜像
{: #ubuntu-apt-mirrors}

用于更新 Ubuntu 映像的 APT 镜像可从 `mirrors.adn.networklayer.com` 获取，其应该解析为 `161.26.0.6`。 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

例如，要访问 IBM Cloud 专用网络上 Object Storage 服务的专用端点，请将 URL 的“domain”部分替换为 `cloud-object-storage.appdomain.cloud`。要访问在其他区域中创建的 COS 存储区，请使用该存储区的端点，而不是在其中创建 VSI 的区域的端点。

## 网络时间协议 (NTP) 服务器
{: #network-time-protocol-ntp-servers}

NTP 服务器可从 `time.adn.networklayer.com` 获取，其应该解析为 `161.26.0.6`。

## 云服务端点
{: #cloud-service-endpoints}

云服务端点是由其他云用户提供的服务。这些端点很快将通过 `cloud.ibm.com` 域中的 DNS 名称可用。它们会解析为 `166.8.0.0/14` 地址。
