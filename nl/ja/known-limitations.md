---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

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

# 既知の制限事項
{: #known-limitations}

この資料には、サポートされていないフィーチャーおよび API、まだサポートされていないフィーチャーおよびユース・ケース、現行リリースでの既知の問題、およびベータ版サービスでしか利用できないフィーチャーのリストが記載されています。 {{site.data.keyword.vpc_full}} (VPC) に機能が追加されていくに伴って、既知の制限事項は変わることがありますので、時折このドキュメントでご確認ください。

## サポートされていない機能の要約
{: #summary-of-features-not-supported}

* VPC への直接リンク・アクセスは、[**クラシック・アクセス**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)を使用する場合にのみサポートされます。
* 一部のプライベート・サービス・エンドポイントは VPC で使用可能ですが、すべてのエンドポイントが使用可能というわけではありません。 使用可能なサービスについては、[IBM Cloud VPC 用のサービス・エンドポイント](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)を参照してください。
* イメージの保存とリストアはサポートされていません。
* {{site.data.keyword.cloud_notm}} VPC は地域的なものであるため、「クラシック・アクセス」が有効になっているか、VPN 接続で接続する場合を除き、ある地域の VPC から別の地域の VPC に接続することはできません。[**クラシック・アクセス**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)を参照してください。

## 現時点でサポートされていない機能とユース・ケース
{: #features-and-use-cases-not-yet-supported}

このセクションでは、サポートされないフィーチャーおよびユース・ケースの詳細を {{site.data.keyword.cloud_notm}} サービス別に分類してリストします。

### 仮想プライベート・クラウド
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}} は、マルチキャスト・ドメインもブロードキャスト・ドメインもサポートしていません。
* {{site.data.keyword.vpc_short}} はエンドツーエンド暗号化は提供しません。
* VPC を他の VPC とピアリングすることはできません。
* クラウド・サービス・エンドポイントは VPC ではサポートされていません。使用可能なサービスについては、[IBM Cloud VPC 用のサービス・エンドポイント](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)を参照してください。

### コンピュート
{: VSI-unsupported-features-and-use-cases}

* 既存の {{site.data.keyword.cloud_notm}} 仮想サーバー・インスタンス (VSI) またはベアメタル・サーバーを VPC にマイグレーションすることはできません。
* VPC にベアメタル・サーバーをプロビジョンすることはできません。

### ネットワーク
{: network-unsupported-features-and-use-cases}

* VPC にカスタム経路を追加することはできません。デフォルトでは、VPC 内のすべてのサブネットが相互に通信できます。
* 1 つのサブネットを複数のゾーンに置くことはできません。
* サブネットをゾーン間で移動することはできません。
* サブネットを作成した後にサイズを変更することはできません。
* 複数の {{site.data.keyword.cloud_notm}} クラシック・リソースを 1 つの VPC 内の同じサブネットに含めることはできません。[**クラシック・アクセス**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)を使用すると、複数のクラシック・リソースとアカウント内の 1 つの VPC を接続できます。
* IPv6 はサポートされていません。
* 所有しているパブリック IP を浮動 IP として持ち込むことはできません。 浮動 IP プールは IBM から提供されます。
* 浮動 IP は 1 つのゾーンに結び付けられています。 例えば、ゾーン 1 のプールから予約した浮動 IP を、同じ VPC 内のゾーン 2 の VSI に割り当てることはできません。
* VSI 間でプライベート IP アドレスを移動することはできません。
* VSI 間でネットワーク・インターフェースを移動することはできません。
* VSI の特定の IP アドレスを指定することはできません。 サブネットの IP 範囲のみ指定できます。 VSI をサブネットに接続すると、システムによってそのサブネット内のプライベート IP が割り当てられます。
* {{site.data.keyword.vpc_short}} には、LBaaS、ACL、セキュリティー・グループなどの独自の Network as a Service ツールが用意されています。独自に所有しているネットワーク・アプライアンス (Vyatta ゲートウェイやその他のロード・バランサーなど) を使用して VPC のリソースを制御することはできません。同様に、{{site.data.keyword.cloud_notm}} クラシック・インフラストラクチャー内に独自に所有しているロード・バランサー・サービスやセキュリティー・グループ・サービスを使用して、{{site.data.keyword.vpc_short}} のリソースを制御することもできません。
* パブリック・ゲートウェイは、インターネットから発信された IBM Cloud VPC 内の VSI へのトラフィックを許可しません。 この目的には、浮動 IP を使用する必要があります。
* ACL はステートレスであるため、戻りのトラフィックを ACL ルールで明示的に許可する必要があります。

## このリリースの既知の問題
{: #known-issues-in-this-release}

以下の条件がすべて揃っていると、{{site.data.keyword.block_storage_is_short}} はボリューム名の固有性を正確に検証しない場合があります。

* プロビジョン要求が同時に実行されている
* ボリュームの名前が同じである
* ボリュームが同じ地域にある

## 使用可能なベータ版サービス

アプライアンスへのセキュアなピアリングが、ベータ版サービスとして利用可能です。いくつかのタイプのアプライアンスでのピアリングについては、以下の資料を参照できます。

* [リモート Vyatta ピア](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [リモート StrongSwan ピア](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [リモート FortiGate ピア](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [リモート Juniper vSRX ピア](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [リモート Cisco ASAv ピア](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
