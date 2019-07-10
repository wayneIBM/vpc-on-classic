---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-07"
keywords: quota, resource, classic, access, gateway, address, prefix, VSI, vNIC, floating, SSH, key, security, group, rule, remote, peer, ACL, region, ingress, egress, VPN, policies, load balancer, listener, pool, per

subcollection: vpc-on-classic

---
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# 配額
{: #quotas}

本文件涵蓋 {{site.data.keyword.cloud}} Virtual Private Cloud 及其中可用資源的配額及限制。

## VPC 配額
{: #vpc-quotas}

帳戶具有下列配額：

|   資源         | 數目上限       |
| ------- | :------: |
| Virtual Private Cloud |每個地區 5 個|
| 具有「標準存取」的 VPC |每個地區 1 個|
| 公用閘道 (PGW) |每個區域每個 VPC 1 個|
| 位址字首 | 每個區域每個 VPC 5 個 |

## 子網路配額
{: #subnet-quotas}

|   資源         | 數目上限       |
| ------- | :------: |
| 子網路 | 每個 Virtual Private Cloud 15 個 |


## VSI 配額
{: #vsi-quotas}

|   資源         | 數目上限       |
| ------- | :------: |
| 虛擬伺服器實例 (VSI) | 依預設，每個帳戶 100 個 |
| 每個 VSI 的 vNIC | 每個 VSI 5 個 |
| 浮動 IP 位址 | 每個 VSI 1 個 |
| SSH 金鑰 | 每個帳戶 100 個 |


## 安全群組配額
{: #security-groups-quotas}

安全群組及其關聯規則具有某些以帳戶為基礎的配額（限制）。

|資源|配額|
|--------|-----|
|安全群組|每個網路介面 5 個|
|安全群組|每個帳戶 500 個|
|規則|每個安全群組 50 個|
|遠端規則|每個安全群組 5 個|
|網路介面|每個安全群組 100 個|

## ACL 配額
{: #acl-quotas}

|資源|配額|
|--------|-----|
|ACL|每個地區 30 個|
|進入規則|每個 ACL 20 個|
|輸出規則|每個 ACL 20 個|

## VPN 配額
{: #vpn-quotas}

以下是每個帳戶的現行 VPN 資源限制：

|資源|配額|
|--------|-----|
| VPN 閘道 | 每個帳戶 20 個 |
| VPN 閘道 | 每個區域 3 個 |
| VPN 連線 | 每個 VPN 閘道 10 個 |
| IKE 原則 | 每個帳戶 20 個 |
| IPSec 原則 | 每個帳戶 20 個 |
| 任何單一 VPN 閘道上的對等節點子網路 | 跨所有 VPN 連線 50 個 |
| 對等節點子網路 | 任何單一 VPN 連線上 15 個 |
| 任何單一 VPN 閘道上的本端子網路 | 跨所有 VPN 連線 50 個 |
| 本端子網路 | 任何單一 VPN 連線上 15 個 |

## 負載平衡器配額
{: #load-balancer-quotas}

以下是現行負載平衡器資源配額：

|資源|配額|
|--------|-----|
|負載平衡器| 每個帳戶 20 個 |
|接聽器| 每個負載平衡器 10 個 |
|儲存區| 每個負載平衡器 10 個 |
|成員| 每個儲存區 50 個 |

## 次要磁區配額
{: #secondary-volume-quotas}

| 資源     |配額|
|--------|----- |
|每個實例的次要磁區（建立實例時）|可要求 4 個次要磁區|
|每個實例的次要磁區（對於核心數數少於 4 個的現有實例）|4 個次要磁區|
|每個實例的次要磁區（對於核心數數不少於 4 個的現有實例）|最多 12 個次要磁區|


