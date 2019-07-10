---

copyright:
  years: 2019

lastupdated: "2019-06-13"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc-on-classic

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Activity Tracker with LogDNA 事件
{: #at-events}

使用 {{site.data.keyword.at_full}} 服务可跟踪用户和应用程序如何与 {{site.data.keyword.cloud}} 中的 {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) 进行交互。
{:shortdesc}

{{site.data.keyword.at_full}} 服务记录的是用户启动且更改 {{site.data.keyword.cloud}} 中服务状态的活动。有关更多信息，请参阅 [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)。

## 事件列表：网络资源
{: #events-volumes}

|资源|操作|描述|
|:----------------|:-----------------------|:-----------------------|
|VPC|is.vpc.vpc.create|创建了 VPC。|
|VPC|is.vpc.vpc.update|更新了 VPC。|
|VPC|is.vpc.vpc.delete|删除了 VPC。|
|VPC|is.vpc.address-prefix.create|向 VPC 添加了地址前缀。|
|VPC|is.vpc.address-prefix.update|更新了 VPC 地址前缀。|
|VPC|is.vpc.address-prefix.delete|从 VPC 中除去了地址前缀。|
|VPC|is.vpc.vpc-route.create|向 VPC 添加了路径。|
|VPC|is.vpc.vpc-route.update|更新了 VPC 路径。|
|VPC|is.vpc.vpc-route.delete|从 VPC 中除去了路径。|
|浮动 IP|is.floating-ip.floating-ip.delete|创建了浮动 IP。|
|浮动 IP|is.floating-ip.floating-ip.delete|更新了浮动 IP。|
|浮动 IP|is.floating-ip.floating-ip.delete|删除了浮动 IP。|
|网络 ACL|is.network-acl.network-acl.create|创建了网络 ACL。|
|网络 ACL|is.network-acl.network-acl.update|更新了网络 ACL。|
|网络 ACL|is.network-acl.network-acl.delete|删除了网络 ACL。|
|网络 ACL|is.network-acl.rule.create|向网络 ACL 添加了规则。|
|网络 ACL|is.network-acl.rule.update|更新了网络 ACL 规则。|
|网络 ACL|is.network-acl.rule.delete|从网络 ACL 中除去了规则。|
|公共网关|is.public-gateway.public-gateway.create|创建了公共网关。|
|公共网关|is.public-gateway.public-gateway.update|更新了公共网关。|
|公共网关|is.public-gateway.public-gateway.delete|删除了公共网关。|
|安全组|is.security-group.security-group.create|创建安全组|
|安全组|is.security-group.security-group.delete|删除安全组|
|安全组|is.security-group.security-group.update|更新安全组|
|安全组|is.security-group.security-group.rule-create|创建安全组规则|
|安全组|is.security-group.security-group.rule-delete|删除安全组规则|
|安全组|is.security-group.security-group.rule-update|更新安全组规则|
|安全组|is.security-group.security-group.interface-attach|连接安全组接口|
|安全组|is.security-group.security-group.interface-detach|拆离安全组接口|
|子网|is.subnet.subnet.create|创建了子网。|
|子网|is.subnet.subnet.update|更新了子网。|
|子网|is.subnet.subnet.delete|删除了子网。|
|子网|is.subnet.network-acl.update|替换了子网的网络 ACL。|
|子网|is.subnet.public-gateway.operate|将公共网关连接到了子网。|
|子网|is.subnet.public-gateway.operate|从子网拆离了公共网关。|

## 事件列表：计算资源
{: #events-computes}

下表列出了与计算资源相关的操作和生成的事件。

|资源|操作|描述|
|:----------------|:-----------------------|:-----------------------|
|实例|is.instance.instance.create|创建了实例。|
|实例|is.instance.instance.delete|删除了实例。|
|实例|is.instance.instance.update|更新了实例。|
|实例|is.instance.action.create|创建了实例操作。|
|实例|is.instance.action.delete|删除了暂挂实例操作。|
|实例|is.instance.network_interface.floating_ip.create|将浮动 IP 与实例网络接口相关联。|
|实例|is.instance.network_interface.floating_ip.delete|解除了浮动 IP 与实例网络接口的关联。|
|实例|is.instance.volume_attachement.create|创建了实例卷连接。|
|实例|is.instance.volume_attachement.delete|删除了实例卷连接。|
|实例|is.instance.volume_attachement.update|更新了实例卷连接。|
|密钥|is.key.key.create|创建了密钥。|
|密钥|is.key.key.delete|删除了密钥。|
|密钥|is.key.key.update|更新了密钥。|

## 事件列表：存储资源
{: #events-storage}

下表列出了与存储资源相关的操作和生成的事件。

|资源|操作|描述|
|:----------------|:-----------------------|:-----------------------|
|卷|is.volume.volume.create|创建了卷。|
|卷|is.volume.volume.update|更新了卷。|
|卷|is.volume.volume.delete|删除了卷。|


## 事件列表：负载均衡器
{: #events-load-balancers}

下表列出了与负载均衡器相关的操作和生成的事件。

|资源|操作|描述|
|:----------------|:-----------------------|:-----------------------|
|负载均衡器|is.load-balancer.load-balancer.create|创建了负载均衡器|
|负载均衡器|is.load-balancer.load-balancer.update|更新了负载均衡器|
|负载均衡器|is.load-balancer.load-balancer.delete|删除了负载均衡器|
|侦听器|is.load-balancer.load-balancer.listener.create|创建了侦听器|
|侦听器|is.load-balancer.load-balancer.listener.update|更新了侦听器|
|侦听器|is.load-balancer.load-balancer.listener.delete|删除了侦听器|
|池|is.load-balancer.load-balancer.pool.create|创建了池|
|池|is.load-balancer.load-balancer.pool.update|更新了池|
|池|is.load-balancer.load-balancer.pool.delete|删除了池|
|成员|is.load-balancer.load-balancer.pool.member.create|创建了成员|
|成员|is.load-balancer.load-balancer.pool.member.update|更新了成员|
|成员|is.load-balancer.load-balancer.pool.member.delete|删除了成员|
|策略|is.load-balancer.load-balancer.listener.policy.create|创建了策略|
|策略|is.load-balancer.load-balancer.listener.policy.update|更新了策略|
|策略|is.load-balancer.load-balancer.listener.policy.delete|删除了策略|
|规则|is.load-balancer.load-balancer.listener.policy.rule.create|创建了规则|
|规则|is.load-balancer.load-balancer.listener.policy.rule.update|更新了规则|
|规则|is.load-balancer.load-balancer.listener.policy.rule.delete|删除了规则|

负载均衡器审计事件会记录到 `us-south` 区域中的 {{site.data.keyword.at_full}}。供应 LoadBalancer 服务的区域无关紧要。
{:note}

## 事件列表：VPN
{: #events-vpn}

下表列出了与 VPN 相关的操作和生成的事件。

|资源|操作|描述|
|:----------------|:-----------------------|:-----------------------|
|VPN|is.vpn.vpn.create|创建了 VPN|
|VPN|is.vpn.vpn.delete|删除了 VPN|
|VPN|is.vpn.vpn.update|更新了 VPN|
|VPN|is.vpn.vpn.update|创建了 VPN 连接|
|VPN|is.vpn.vpn.update|删除了 VPN 连接|
|VPN|is.vpn.vpn.update|更新了 VPN 连接|


## 支持的位置
{: #at-supported-locations}

目前，{{site.data.keyword.at_full}} 支持可用于达拉斯和法兰克福位置。下表列出了每个 VPC 区域的 Activity Tracker with LogDNA 位置。

|VPC 区域|Activity Tracker with LogDNA 位置|
|--------------|---------------------------------------|
|us-south|达拉斯|
|jp-tok|东京|
|eu-de|法兰克福|

## 查找事件的位置
{: #at-events-ui}

供应 {{site.data.keyword.at_full}} 实例后，事件会自动转发到位于其[支持的位置](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`)中的 Activity Tracker with LogDNA 服务实例。
