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

# 配额
{: #quotas}

本文档涵盖了 {{site.data.keyword.cloud}} Virtual Private Cloud 及其中可用资源的配额和限制。

## VPC 配额
{: #vpc-quotas}

帐户具有以下配额：

|资源|最大数量|
| ------- | :------: |
|虚拟私有云|每个区域 5 个|
|使用经典访问的 VPC|每个区域 1 个|
|公共网关 (PGW)|每个专区每个 VPC 1 个|
|地址前缀|每个专区每个 VPC 5 个|

## 子网配额
{: #subnet-quotas}

|资源|最大数量|
| ------- | :------: |
|子网|每个虚拟私有云 15 个|


## VSI 配额
{: #vsi-quotas}

|资源|最大数量|
| ------- | :------: |
|虚拟服务器实例 (VSI)|缺省情况下，每个帐户 100 个|
|每个 VSI 的 vNIC|每个 VSI 5 个|
|浮动 IP 地址|每个 VSI 1 个|
|SSH 密钥|每个帐户 100 个|


## 安全组配额
{: #security-groups-quotas}

对于安全组及其关联的规则，存在一些基于帐户的配额（限制）。

|资源|配额|
|--------|-----|
|安全组|每个网络接口 5 个|
|安全组|每个帐户 500 个|
|规则|每个安全组 50 个|
|远程规则|每个安全组 5 个|
|网络接口|每个安全组 100 个|

## ACL 配额
{: #acl-quotas}

|资源|配额|
|--------|-----|
|ACL|每个区域 30 个|
|输入规则|每个 ACL 20 个|
|输出规则|每个 ACL 20 个|

## VPN 配额
{: #vpn-quotas}

下面是每个帐户的当前 VPN 资源限制：

|资源|配额|
|--------|-----|
|VPN 网关|每个帐户 20 个|
|VPN 网关|每个专区 3 个|
|VPN 连接|每个 VPN 网关 10 个|
|IKE 策略|每个帐户 20 个|
|IPSec 策略|每个帐户 20 个|
|任何单个 VPN 网关上的同级子网|所有 VPN 连接中 50 个|
|同级子网|任何单个 VPN 连接上 15 个|
|任何单个 VPN 网关上的本地子网|所有 VPN 连接中 50 个|
|本地子网|任何单个 VPN 连接上 15 个|

## 负载均衡器配额
{: #load-balancer-quotas}

下面是当前负载均衡器资源配额：

|资源|配额|
|--------|-----|
|负载均衡器|每个帐户 20 个|
|侦听器|每个负载均衡器 10 个|
|池|每个负载均衡器 10 个|
|成员|每个池 50 个|

## 辅助卷配额
{: #secondary-volume-quotas}

|资源|配额|
|--------|----- |
|每个实例的辅助卷（创建实例时）|可请求 4 个辅助卷|
|每个实例的辅助卷（对于核心数少于 4 个的现有实例）|4 个辅助卷|
|每个实例的辅助卷（对于核心数不少于 4 个的现有实例）|最多 12 个辅助卷|


