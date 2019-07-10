---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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
{:faq: data-hd-content-type='faq'}


# 常见问题
{: #faqs}

## 可以将 VPC 连接到其他 IBM Cloud 工作负载吗？  
{: #faq-0}
{:faq}

可以，您可以在每个区域设置一个 VPC 对 {{site.data.keyword.cloud}} 经典基础架构的访问权。有关更多信息，请参阅[设置对经典基础架构的访问权](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)。

## VPC 名称中的字符数限制为多少？
{: #faq-1}
{:faq}

目前，限制为 100 个字符。如果超过此限制，您可能会收到“内部错误”消息。

## 有任何 VPC 资源名称能以数字开头吗？
{: #faq-2}
{:faq}

没有，虽然名称可以包含数字，但必须以字母开头。

## 对名称中可以使用的字符有限制吗？
{: #faq-3}
{:faq}

有，UI 禁止 VSI 名称中包含连续的两个短划线、下划线和句点。

## 可以创建没有子网，而只使用浮动 IP 的 VSI 吗？
{: #faq-9}
{:faq}

不能。

## 可以将一个 VSI 连接到多个 VPC 吗？
{: #faq-10}
{:faq}

不能。

## 在 VPC 中创建子网大小后，可以更改子网大小吗？
{: #faq-11}
{:faq}

不能。

## 对于从 VPC 到 COS 的流量，是否有出口带宽费用？
{: #faq-17}
{:faq}

只要将 ADN 端点用于从 VPC 连接到 COS，就没有任何出口带宽费用，但仍然有 API 调用费用。

 ## 在设置 VPN for VPC 时，为什么需要指定子网？应该指定什么内容？
{: #faq-16}
{:faq}

设置 VPN 网关时，必须指定子网，以便 VPN 网关可以获取它需要用于 VPN 服务的 IP 地址。通过指定子网，您可以保留对如何处理 VPN 流量的控制权，并且可以灵活地指定更靠近利用 VPN 的资源的位置。通过这种方式，可以缩短等待时间并提高性能。

## 如何知道要将负载均衡器实例部署到的位置？
{: #faq-18}
{:faq}

选择的子网可确定将部署 VPC 负载均衡器的位置。VPC 负载均衡器是一种区域资源。最佳做法是从给定区域下的不同专区中选择子网，以提高冗余度。


## 在 VPC 中的浮动 IP 分配期间，客户必须指定 VSI 的 vNIC，这是正确做法吗？
{: #faq-4}
{:faq}

是的，事实上，必须指定的是服务器的主网络接口。

但是，如果在联网区域下的 API 中添加“地址”资源，那么可以顺利将浮动 IP 的关联更改为关联到地址，而不是关联到接口。

## VPC 中虚拟服务器实例上的 vNIC 能同时具有浮动 IP 和专用 IP 吗？
{: #faq-5}
{:faq}
 
可以。

## 在 IBM Cloud 控制台的 VPC UI 中，每个子网有一个“切换开关”用于连接到公共网关 (PGW) 或从公共网关拆离。PGW 是谁创建的？在该 UI 中，客户要将子网连接到 PGW 时，PGW 的状态会为准备就绪吗？
{: #faq-6}
{:faq}

目前，PGW 必须通过 API 来显式进行创建，因为 API 需要子网与特定公共网关显式关联。

## 假设 VPC 中的 VSI1 只有 vNIC1，并且 VSI1 已连接到 Subnet1。Subnet1 已连接到公共网关 (PGW)。在这种情况下，客户仍可以将浮动 IP 分配给 VSI1 吗？
{: #faq-7}
{:faq}

可以，服务器可以位于连接到公共网关的子网上，并且还可以具有浮动 IP。将浮动 IP 分配给 VSI 与是否存在连接到子网的公共网关无关。与 VSI 关联的浮动 IP 的优先级高于连接到子网的公共网关。

## 在 VPC 中，VSI1 具有 2 个 vNIC，名为 vNIC1 和 vNIC2。这两个 vNIC 可以连接到同一子网吗？
{: #faq-8}
{:faq}
 
可以，对于在同一服务器上连接到同一子网的网络接口数，没有任何限制。但是，不支持一个 VSI 上有多个 NIC。

## 是哪个服务强制 VPC 的每个专区只能有 1 个公共网关？
{: #faq-13}
{: faq}

VPC API 强制实施此限制。

## 在 PGW 创建期间，我需要保留 FIP，还是系统会自动保留 FIP？在查询所有浮动 IP 时，会看到该浮动 IP 吗？
{: #faq-12}
{: #faq}

如果未指定现有浮动 IP，那么 API 会自动创建浮动 IP 以及公共网关。所以，该浮动 IP 会显示在列表中。


## VRF 对我的其他联网功能和我的子网有何影响？
{: #faq-14}
{:faq}

关于 VLAN 和 VRF 的警告：

* 在 VRF 环境中，不允许使用帐户间 VLAN 生成。
* IPSec VPN 服务受到限制。

VRF 不会阻止 SSL 或 PPTP VPN 访问，但其行为会更改。不使用 VRF 时，一个 VPN 连接就足以查看帐户上的所有服务器。使用 VRF 时，您只能访问市场中与 VPN 关联的资源。因此，如果连接到 DAL VPN，那么只能连接到 DAL 服务器。
{: note}

## 在 VPC 中使用 `instance-network-interface-floating-ip-add` 子命令的正确方法是什么？要预先创建/分配浮动 IP 地址吗？
{: #faq-15}
{:faq}

 您必须先分配一个浮动 IP，然后在接口浮动 IP `add` 命令上提供该 FIP 地址。

 例如：`ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`。请提供专区。
