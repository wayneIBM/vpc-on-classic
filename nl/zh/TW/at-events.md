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

使用 {{site.data.keyword.at_full}} 服務可追蹤使用者和應用程式如何與 {{site.data.keyword.cloud}} 中的 {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) 互動。
{:shortdesc}

{{site.data.keyword.at_full}} 服務會記錄由使用者起始並且會變更 {{site.data.keyword.cloud}} 中服務狀態的活動。如需相關資訊，請參閱 [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)。

## 事件清單：網路資源
{: #events-volumes}

| 資源                          |動作|說明|
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   |已建立 VPC。|
| vpc  |is.vpc.vpc.update|已更新 VPC。|
| vpc  |is.vpc.vpc.delete|已刪除 VPC。|
| vpc  |is.vpc.address-prefix.create|已對 VPC 新增位址字首。|
| vpc  |is.vpc.address-prefix.update|已更新 VPC 位址字首。|
| vpc  |is.vpc.address-prefix.delete|已從 VPC 移除位址字首。|
| vpc  |is.vpc.vpc-route.create|已對 VPC 新增路徑。|
| vpc  |is.vpc.vpc-route.update|已更新 VPC 路徑。|
| vpc  |is.vpc.vpc-route.delete|已從 VPC 移除路徑。|
| floating-ip  | is.floating-ip.floating-ip.delete   |已建立浮動 IP。|
| floating-ip  | is.floating-ip.floating-ip.delete   |已更新浮動 IP。|
| floating-ip  | is.floating-ip.floating-ip.delete   |已刪除浮動 IP。|
| network-acl  | is.network-acl.network-acl.create   |已建立網路 ACL。|
| network-acl  |is.network-acl.network-acl.update|已更新網路 ACL。|
| network-acl  |is.network-acl.network-acl.delete|已刪除網路 ACL。|
| network-acl  |is.network-acl.rule.create|已對網路 ACL 新增了規則。|
| network-acl  |is.network-acl.rule.update|已更新網路 ACL 規則。|
| network-acl  |is.network-acl.rule.delete|已從網路 ACL 移除規則。|
| public-gateway |is.public-gateway.public-gateway.create|已建立公用閘道。|
| public-gateway |is.public-gateway.public-gateway.update|已更新公用閘道。|
| public-gateway |is.public-gateway.public-gateway.delete|已刪除公用閘道。|
| security-group |is.security-group.security-group.create|建立安全群組|
| security-group |is.security-group.security-group.delete|刪除安全群組|
| security-group |is.security-group.security-group.update|更新安全群組|
| security-group |is.security-group.security-group.rule-create|建立安全群組規則|
| security-group |is.security-group.security-group.rule-delete|刪除安全群組規則|
| security-group |is.security-group.security-group.rule-update|更新安全群組規則|
| security-group |is.security-group.security-group.interface-attach|連接安全群組介面|
| security-group |is.security-group.security-group.interface-detach|分離安全群組介面|
| subnet   |is.subnet.subnet.create|已建立子網路。|
| subnet   |is.subnet.subnet.update|已更新子網路。|
| subnet   |is.subnet.subnet.delete|已刪除子網路。|
| subnet   |is.subnet.network-acl.update|已取代子網路的網路 ACL。|
| subnet   | is.subnet.public-gateway.operate  |已將公用閘道連接到子網路。|
| subnet   | is.subnet.public-gateway.operate  |已從子網路分離公用閘道。|

## 事件清單：運算資源
{: #events-computes}

下表列出了與運算資源相關的動作和產生的事件。

| 資源                          |動作|說明|
|:----------------|:-----------------------|:-----------------------|
| instance   |is.instance.instance.create|已建立實例。|
| instance   |is.instance.instance.delete|已刪除實例。|
| instance   |is.instance.instance.update|已更新實例。|
| instance   |is.instance.action.create|已建立實例動作。|
| instance   |is.instance.action.delete|已刪除擱置實例動作。|
| instance   |is.instance.network_interface.floating_ip.create|已將浮動 IP 與實例網路介面相關聯。|
| instance   |is.instance.network_interface.floating_ip.delete|已解除浮動 IP 與實例網路介面的關聯。|
| instance   |is.instance.volume_attachement.create|已建立實例磁區連接。|
| instance   |is.instance.volume_attachement.delete|已刪除實例磁區連接。|
| instance   |is.instance.volume_attachement.update|已更新實例磁區連接。|
| key  | is.key.key.create   |已建立金鑰。|
| key  |is.key.key.delete|已刪除金鑰。|
| key  |is.key.key.update|已更新金鑰。|

## 事件清單：儲存空間資源
{: #events-storage}

下表列出了與儲存空間資源相關的動作和產生的事件。

| 資源                          |動作|說明|
|:----------------|:-----------------------|:-----------------------|
| volume  | is.volume.volume.create  |已建立磁區。|
| volume  |is.volume.volume.update|已更新磁區。|
| volume  |is.volume.volume.delete|已刪除磁區。|


## 事件清單：負載平衡器
{: #events-load-balancers}

下表列出了與負載平衡器相關的動作和產生的事件。

| 資源                          |動作|說明|
|:----------------|:-----------------------|:-----------------------|
|負載平衡器|is.load-balancer.load-balancer.create|已建立負載平衡器|
|負載平衡器|is.load-balancer.load-balancer.update|已更新負載平衡器|
|負載平衡器|is.load-balancer.load-balancer.delete|已刪除負載平衡器|
| 接聽器 |is.load-balancer.load-balancer.listener.create|已建立接聽器|
| 接聽器 |is.load-balancer.load-balancer.listener.update|已更新接聽器|
| 接聽器 |is.load-balancer.load-balancer.listener.delete|已刪除接聽器|
| 儲存區 |is.load-balancer.load-balancer.pool.create|已建立儲存區|
| 儲存區 |is.load-balancer.load-balancer.pool.update|已更新儲存區|
| 儲存區 |is.load-balancer.load-balancer.pool.delete|已刪除儲存區|
| 成員 |is.load-balancer.load-balancer.pool.member.create|已建立成員|
| 成員 |is.load-balancer.load-balancer.pool.member.update|已更新成員|
| 成員 |is.load-balancer.load-balancer.pool.member.delete|已刪除成員|
|原則|is.load-balancer.load-balancer.listener.policy.create|已建立原則|
|原則|is.load-balancer.load-balancer.listener.policy.update|已更新原則|
|原則|is.load-balancer.load-balancer.listener.policy.delete|已刪除原則|
|規則|is.load-balancer.load-balancer.listener.policy.rule.create|已建立規則|
|規則|is.load-balancer.load-balancer.listener.policy.rule.update|已更新規則|
|規則|is.load-balancer.load-balancer.listener.policy.rule.delete|已刪除規則|

負載平衡器審核事件會記錄到 `us-south` 地區中的 {{site.data.keyword.at_full}}。佈建負載平衡器服務的地區無關緊要。
{:note}

## 事件清單：VPN
{: #events-vpn}

下表列出了與 VPN 相關的動作和產生的事件。

| 資源                          |動作|說明|
|:----------------|:-----------------------|:-----------------------|
| vpn  |is.vpn.vpn.create|已建立 VPN|
| vpn  |is.vpn.vpn.delete|已刪除 VPN|
| vpn  | is.vpn.vpn.update   |已更新 VPN|
| vpn  | is.vpn.vpn.update   |已建立 VPN 連線|
| vpn  | is.vpn.vpn.update   |已刪除 VPN 連線|
| vpn  | is.vpn.vpn.update   |已更新 VPN 連線|


## 支援的位置
{: #at-supported-locations}

目前，{{site.data.keyword.at_full}} 支援可用於達拉斯和法蘭克福位置。下表列出了每個 VPC 地區的 Activity Tracker with LogDNA 位置。

|VPC 地區|Activity Tracker with LogDNA 位置|
|--------------|---------------------------------------|
| us-south | 達拉斯 |
| jp-tok | 東京  |
| eu-de | 法蘭克福  |

## 尋找事件的位置
{: #at-events-ui}

佈建 {{site.data.keyword.at_full}} 實例後，事件會自動轉遞到位於其[支援的位置](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`)中的 Activity Tracker with LogDNA 服務實例。
