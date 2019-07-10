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


# Activity Tracker with LogDNA のイベント
{: #at-events}

{{site.data.keyword.at_full}} サービスを使用して、ユーザーとアプリケーションが {{site.data.keyword.cloud}} 内の {{site.data.keyword.cloud}} 仮想プライベート・クラウド (VPC) とどのように対話しているかを追跡できます。
{:shortdesc}

{{site.data.keyword.at_full}} サービスは、{{site.data.keyword.cloud}} 内のサービスの状態を変更するアクティビティーをユーザーが開始した場合にそのアクティビティーを記録します。詳しくは、『[{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)』を参照してください。

## イベントのリスト: ネットワーク・リソース
{: #events-volumes}

| リソース  | アクション  | 説明  |
|:----------------|:-----------------------|:-----------------------|
| VPC  | is.vpc.vpc.create   | VPC が作成された。 |
| VPC  | is.vpc.vpc.update   | VPC が更新された。 |
| VPC  | is.vpc.vpc.delete   | VPC が削除された。 |
| VPC  | is.vpc.address-prefix.create  | VPC にアドレス接頭部が追加された。  |
| VPC  | is.vpc.address-prefix.update  | VPC アドレス接頭部が更新された。  |
| VPC  | is.vpc.address-prefix.delete  | VPC からアドレス接頭部が削除された。  |
| VPC  | is.vpc.vpc-route.create   | VPC に経路が追加された。   |
| VPC  | is.vpc.vpc-route.update   | VPC 経路が更新された。  |
| VPC  | is.vpc.vpc-route.delete   | VPC から経路が削除された。   |
| 浮動 IP  | is.floating-ip.floating-ip.delete   | 浮動 IP が作成された。  |
| 浮動 IP  | is.floating-ip.floating-ip.delete   | 浮動 IP が更新された。  |
| 浮動 IP  | is.floating-ip.floating-ip.delete   | 浮動 IP が削除された。  |
| ネットワーク ACL  | is.network-acl.network-acl.create   | ネットワーク ACL が作成された。  |
| ネットワーク ACL  | is.network-acl.network-acl.update   | ネットワーク ACL が更新された。  |
| ネットワーク ACL  | is.network-acl.network-acl.delete   | ネットワーク ACL が削除された。  |
| ネットワーク ACL  | is.network-acl.rule.create  | ネットワーク ACL にルールが追加された。  |
| ネットワーク ACL  | is.network-acl.rule.update  | ネットワーク ACL ルールが更新された。   |
| ネットワーク ACL  | is.network-acl.rule.delete  | ネットワーク ACL からルールが削除された。  |
| パブリック・ゲートウェイ | is.public-gateway.public-gateway.create   | パブリック・ゲートウェイが作成された。   |
| パブリック・ゲートウェイ | is.public-gateway.public-gateway.update   | パブリック・ゲートウェイが更新された。   |
| パブリック・ゲートウェイ | is.public-gateway.public-gateway.delete   | パブリック・ゲートウェイが削除された。   |
| セキュリティー・グループ | is.security-group.security-group.create   |セキュリティー・グループ作成   |
| セキュリティー・グループ | is.security-group.security-group.delete   |セキュリティー・グループ削除   |
| セキュリティー・グループ | is.security-group.security-group.update   |セキュリティー・グループ更新   |
| セキュリティー・グループ | is.security-group.security-group.rule-create  | セキュリティー・グループのルール作成  |
| セキュリティー・グループ | is.security-group.security-group.rule-delete  | セキュリティー・グループのルール削除  |
| セキュリティー・グループ | is.security-group.security-group.rule-update  | セキュリティー・グループのルール更新  |
| セキュリティー・グループ | is.security-group.security-group.interface-attach | セキュリティー・グループのインターフェースの割り当て   |
| セキュリティー・グループ | is.security-group.security-group.interface-detach | セキュリティー・グループのインターフェースの割り当ての解除   |
| サブネット   | is.subnet.subnet.create   | サブネットが作成された。   |
| サブネット   | is.subnet.subnet.update   | サブネットが更新された。   |
| サブネット   | is.subnet.subnet.delete   | サブネットが削除された。   |
| サブネット   | is.subnet.network-acl.update  | サブネットのネットワーク ACL が置き換えられた。   |
| サブネット   | is.subnet.public-gateway.operate  | サブネットにパブリック・ゲートウェイが接続された。  |
| サブネット   | is.subnet.public-gateway.operate  | サブネットからパブリック・ゲートウェイが切り離された。  |

## イベントのリスト: コンピュート・リソース
{: #events-computes}

コンピュート・リソースとイベント生成に関連するアクションを次の表にリストします。

| リソース  | アクション  | 説明  |
|:----------------|:-----------------------|:-----------------------|
| インスタンス   | is.instance.instance.create   | インスタンスが作成された。   |
| インスタンス   | is.instance.instance.delete   | インスタンスが削除された。   |
| インスタンス   | is.instance.instance.update   | インスタンスが更新された。   |
| インスタンス   | is.instance.action.create   | インスタンス操作が作成された。  |
| インスタンス   | is.instance.action.delete   | 保留中のインスタンス操作が削除された。  |
| インスタンス   | is.instance.network_interface.floating_ip.create  | インスタンス・ネットワーク・インターフェースに浮動 IP が関連付けられた  |
| インスタンス   | is.instance.network_interface.floating_ip.delete  | インスタンス・ネットワーク・インターフェースから浮動 IP の関連付けが解除された |
| インスタンス   | is.instance.volume_attachement.create   | インスタンスのボリューム接続が作成された  |
| インスタンス   | is.instance.volume_attachement.delete   | インスタンスのボリューム接続が削除された  |
| インスタンス   | is.instance.volume_attachement.update   | インスタンスのボリューム接続が更新された  |
| 鍵  | is.key.key.create   | 鍵が作成された。  |
| 鍵  | is.key.key.delete   | 鍵が削除された。  |
| 鍵  | is.key.key.update   | 鍵が更新された。  |

## イベントのリスト: ストレージ・リソース
{: #events-storage}

ストレージ・リソースとイベント生成に関連するアクションを次の表にリストします。

| リソース  | アクション  | 説明  |
|:----------------|:-----------------------|:-----------------------|
| ボリューム  | is.volume.volume.create  |  ボリュームが作成された。  |
| ボリューム  | is.volume.volume.update  |  ボリュームが更新された。  |
| ボリューム  | is.volume.volume.delete  |  ボリュームが削除された。  |


## イベントのリスト: ロード・バランサー
{: #events-load-balancers}

ロード・バランサーとイベント生成に関連するアクションを次の表にリストします。

| リソース  | アクション  | 説明  |
|:----------------|:-----------------------|:-----------------------|
| ロード・バランサー |  is.load-balancer.load-balancer.create | ロード・バランサーが作成された |
| ロード・バランサー |  is.load-balancer.load-balancer.update | ロード・バランサーが更新された |
| ロード・バランサー |  is.load-balancer.load-balancer.delete | ロード・バランサーが削除された |
| リスナー |  is.load-balancer.load-balancer.listener.create | リスナーが作成された |
| リスナー |  is.load-balancer.load-balancer.listener.update | リスナーが更新された |
| リスナー |  is.load-balancer.load-balancer.listener.delete | リスナーが削除された |
| プール |  is.load-balancer.load-balancer.pool.create | プールが作成された |
| プール |  is.load-balancer.load-balancer.pool.update | プールが更新された |
| プール |  is.load-balancer.load-balancer.pool.delete | プールが削除された |
| メンバー |  is.load-balancer.load-balancer.pool.member.create | メンバーが作成された |
| メンバー |  is.load-balancer.load-balancer.pool.member.update | メンバーが更新された |
| メンバー |  is.load-balancer.load-balancer.pool.member.delete | メンバーが削除された |
| ポリシー |  is.load-balancer.load-balancer.listener.policy.create | ポリシーが作成された |
| ポリシー |  is.load-balancer.load-balancer.listener.policy.update | ポリシーが更新された |
| ポリシー |  is.load-balancer.load-balancer.listener.policy.delete | ポリシーが削除された |
| ルール |  is.load-balancer.load-balancer.listener.policy.rule.create | ルールが作成された |
| ルール |  is.load-balancer.load-balancer.listener.policy.rule.update | ルールが更新された |
| ルール |  is.load-balancer.load-balancer.listener.policy.rule.delete | ルールが削除された |

ロード・バランサーの監査イベントは、`us-south` 地域の {{site.data.keyword.at_full}} に記録されます。ロード・バランサー・サービスをどの地域にプロビジョンするかは問題ではありません。
{:note}

## イベントのリスト: VPN
{: #events-vpn}

VPN とイベント生成に関連するアクションを次の表にリストします。

| リソース  | アクション  | 説明  |
|:----------------|:-----------------------|:-----------------------|
| VPN  | is.vpn.vpn.create   | VPN が作成された |
| VPN  | is.vpn.vpn.delete   | VPN が削除された |
| VPN  | is.vpn.vpn.update   | VPN が更新された |
| VPN  | is.vpn.vpn.update   | VPN 接続が作成された  |
| VPN  | is.vpn.vpn.update   | VPN 接続が削除された  |
| VPN  | is.vpn.vpn.update   | VPN 接続が更新された  |


## サポートされるロケーション
{: #at-supported-locations}

{{site.data.keyword.at_full}} は、現在、ダラスとフランクフルトのロケーションでサポートされています。次の表に、各 VPC 地域の Activity Tracker with LogDNA のロケーションをリストします。

| VPC 地域 | Activity Tracker with LogDNA のロケーション |
|--------------|---------------------------------------|
| us-south   | ダラス  |
| jp-tok   | 東京  |
| eu-de  | フランクフルト   |

## イベントを確認できる場所
{: #at-events-ui}

{{site.data.keyword.at_full}} インスタンスをプロビジョンすると、[サポートされるロケーション](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`)にある Activity Tracker with LogDNA サービス・インスタンスにイベントが自動的に転送されます。
