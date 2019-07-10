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

# IBM Cloud VPC 可用的服務端點
{: #service-endpoints-available-for-ibm-cloud-vpc}

準備好執行工作負載時，可以連接兩種類型的端點：基礎架構即服務 (IaaS) 端點和雲端服務端點 (CSE)。IaaS 端點已預先佈建並可隨時使用；但是，CSE 必須個別佈建後才能使用。

這些服務的端點使用_可遞送位址_；即，RFC 1918 中指定的位址之外的位址，這些位址可能看起來好像是透過公用網際網路進行通訊，但實際進出這些端點的資料流量不會離開雲端。因此，這種資料流量不會發生與從雲端流出並流入公用網際網路的資料流量關聯的頻寬計費。

## 基礎架構即服務 (IaaS) 端點
{: #infrastructure-as-a-service-iaas-endpoints}

基礎架構服務使用 `adn.networklayer.com` 網域中的特定 DNS 名稱提供，並解析為 `161.26.0.0/16` 位址。

您可以連接的服務包括：

* DNS 解析器
* Ubuntu APT（進階包裝工具）鏡映
* Cloud Object Storage
* NTP

## DNS（網域名稱系統）解析器端點
{: #dns-domain-name-system-resolver-endpoints}

DNS 解析器使用的是 IP 位址，而不是名稱。對於共用雲端服務端點，應該使用 DNS 伺服器位址 `161.26.0.10` 和 `161.26.0.11`。

## Ubuntu APT 鏡映
{: #ubuntu-apt-mirrors}

用於更新 Ubuntu 映像檔的 APT 鏡映可從 `mirrors.adn.networklayer.com` 取得，應該解析為 `161.26.0.6`。 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

例如，若要連接 IBM Cloud 專用網路上 Object Storage 服務的專用端點，請將 URL 的 "domain" 部分取代為 `cloud-object-storage.appdomain.cloud`。若要連接其他地區中建立的 COS 儲存區，請使用該儲存區的端點，而不是在其中建立 VSI 的地區的端點。

## 網路時間通訊協定 (NTP) 伺服器
{: #network-time-protocol-ntp-servers}

NTP 伺服器可從 `time.adn.networklayer.com` 取得，應該解析為 `161.26.0.6`。

## 雲端服務端點
{: #cloud-service-endpoints}

雲端服務端點是由其他雲端使用者提供的服務。很快地，將透過 `cloud.ibm.com` 網域中的 DNS 名稱來提供這些端點。它們會解析為 `166.8.0.0/14` 位址。
