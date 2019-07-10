---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Virtual Private Cloud  について
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) は、次世代の IBM Cloud プラットフォームです。VPC は、IBM Cloud 内にユーザー独自のスペースを作成し、パブリック・クラウドの中で隔離された環境を実行することを可能にします。VPC は、プライベート・クラウドのセキュリティーと、パブリック・クラウドの俊敏性や使いやすさを兼ね備えています。

基幹業務アプリケーション、クラウド耐性アプリケーション、クラウド・ネイティブ・アプリケーションをサポートするために、必要に応じて、ネットワーク・サービスの管理およびインスタンスの起動を実行できます。使用可能な主な機能は以下のとおりです。

* API を使用して、分離したアプリケーション環境を作成および管理できます
* セキュリティーやアクセスの利便性のために設計したユーザー独自のネットワーク・ポリシーを定義できます
* BYOIP (お客様所有の IP の持ち込み) を使用して、ネットワーク・トポロジーを設計できます
* リソースをプロビジョンして、相互に接続したり分離したりできます
* 災害復旧と回復力のために複数の地域をカバーできます
* 高速かつ低遅延の地域間接続が可能なアベイラビリティー・ゾーンを使用して HA を実現できます
* 高速のネットワーク・デバイスとストレージ・デバイスを利用できます
* 常時オンのサービスを提供できます (制御プレーン)
* コア・サービス (IPAM、VPN、ファイアウォール、SSH、DNS、L4 ロード・バランシング) を利用できます
* その他のサービス (自動スケーリング、CDN、サード・パーティー NFV) もセットアップできます

次の VPC 概要図は、VPC の使用可能なコンポーネントと、ユーザーに必要なサービスと接続を提供するために各コンポーネントが連携する方法を示しています。各コンポーネントの詳細が記載されたトピックを読むときには、この図をもう一度確認してください。

![IBM Cloud VPC の概要](images/vpc-experience-simple.svg "IBM Cloud VPC の概要")

これ以降のセクションでは、IBM Cloud VPC で使用可能な機能の概要を説明します。

## ネットワーク機能

{{site.data.keyword.cloud_notm}} VPC では、簡単で包括的なネットワーク機能を利用できます。例えば、複数の仮想プライベート・クラウドを、[世界中に用意されたマルチゾーン地域](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)に作成する機能、異なる複数のゾーンのサブネットに作成する機能、[IP アドレス範囲選択](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)、仮想ファイアウォール、([セキュリティー・グループ](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)および[ネットワーク ACL](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls))、サイト間仮想プライベート・ネットワーク ([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc))、柔軟なロード・バランシング ([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)) などがあります。

VPC は、1 つの範囲のプライベート IP アドレスを使用して、複数のサブネットに分割されます。ただし、デフォルトでは、サブネットに関係なく、同じ VPC 内のすべてのリソース (VSI など) が相互に通信できます。サブネットは 1 つのゾーンの範囲内に制限されるので、複数のゾーンにまたがることはありません。これは、セキュリティー、待ち時間の減少、高可用性の役に立ちます。


推奨される接頭部範囲と事前構成されたネットワーク・セキュリティー・ポリシーを使用すれば、複数の VPC とサブネットを簡単に作成できます。また、ユーザー独自のアドレス接頭部とカスタム・セキュリティー・ポリシーを設計して定義することもできます。

CIDR ブロックの 161.26/16 と 166.8/14 はどちらも予約されており、すべてのサブネットにルーティングされます。[IBM Cloud VPC 用のサービス・エンドポイント](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)を参照してください。さらに、CIDR ブロック 10.240/13 は VPC アドレス接頭部用に予約されており、[事前定義された方法で](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes)ゾーン間で分けられているので、変更できません。
{: important}

### インターネット・アクセス

仮想サーバー・インスタンス (VSI) からパブリック・インターネットへの通信を可能にする方法として、次の 3 つの選択肢があります。
* パブリック・ゲートウェイ (PGW) を使用して、接続されているサブネット上のすべての仮想サーバー・インスタンスでインターネットへの通信ができるようにする。PGW は無料で使用できます。ただし、使用した帯域幅については課金されます。
* 浮動 IP (FIP) を使用して、単一の仮想サーバー・インスタンス (VSI) とインターネット間の通信を可能にする。
* VPN ゲートウェイを使用する。詳しくは、[VPC の VPN に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)を参照してください。
{: note}

### クラシック・アクセス

既存の {{site.data.keyword.cloud_notm}} インフラストラクチャー・ユーザーは、[クラシック・アクセス](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)を使用して、Direct Link 接続などのクラシック・インフラストラクチャーを地域ごとに 1 つの VPC に接続できます。

### BYOIP

所有しているパブリック IPv4 アドレス範囲を IBM Cloud VPC アカウントに持ち込むことができます (BYOIP)。

BYOIP を使用する場合は、{{site.data.keyword.cloud_notm}} 側で、提供されたアドレスとの間でパケットを送受信する {{site.data.keyword.cloud_notm}} リソースに、それらの IPv4 アドレスを構成する必要があります。そのため、提供された IPv4 範囲を {{site.data.keyword.cloud_notm}} で使用した結果、それらの IP アドレスが IBM のサポート・スタッフおよび第三者に知られる可能性があります。
{: important}

BYOIP を利用するには、API または CLI を使用する必要があります。 この機能は、IBM Cloud のコンソール UI では利用できません。
{: note}

### グローバル接続

複数の地域にまたがる形でアプリケーションや利用可能なリソースの範囲を設定できます。 [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc) を使用して、ハイブリッド・クラウド・デプロイメントの他のプロジェクトや他の部分へのプライベート接続を作成できます。

簡単に拡張して共有できるので、VPC のネットワークとリソースは、オンプレミスの施設からクラウドまで拡張することができます。

## セキュリティー

{{site.data.keyword.cloud_notm}} VPC にはセキュリティーが組み込まれています。インスタンス・レベルの保護には、仮想ファイアウォールの役割を果たす[セキュリティー・グループ](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)を、サブネット・レベルの保護には、[ネットワーク・アクセス制御リスト](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) (ACL) を利用できます。

## 高可用性

{{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) では、どの地域でも、スケーラビリティーとセキュリティーを維持しながらアプリケーションを他のネットワークから論理的に分離することができます。[ロード・バランサー](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)を使用してネットワーク・トラフィックを一連のターゲット間に分散させ、パフォーマンスと HA を向上させることができます。ロード・バランサーでアプリケーションとサービスの正常性をモニターすることも可能です。 ロード・バランサーのセットアップによって、着信アプリケーション・トラフィックを 1 つのゾーンのインスタンス間で分散させることも、1 つの地域の複数のゾーン間で分散させることもできます。

## コンピュート能力

[IBM Cloud Virtual Servers for Virtual Private Cloud](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud) を使用して、スケーラブルなコンピュート・リソースを IBM Cloud にプロビジョンします。特定のワークロードに合わせて最適化された事前定義[プロファイル](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)を使用すれば、仮想サーバー・インスタンス (VSI) をすぐに作成できます。マルチホームの [マルチ vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) インスタンスをプロビジョンする場合は、サポートされるストック・[イメージ](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) から選択するか、カスタム・イメージをアップロードします。

{{site.data.keyword.cloud_notm}} VPC は、仮想サーバー・インスタンスのポッド境界をなくすネットワーク・オーケストレーション層を追加して、インスタンスをスケーリングする能力を無限にします。このネットワーク・オーケストレーション層が、地域やゾーンをまたいで、IBM Cloud VPC 内にあるすべての仮想サーバー・インスタンスのすべてのネットワーキングを処理します。{{site.data.keyword.cloud_notm}} VPC が備えるソフトウェア定義のネットワーキング機能により、VPN、LBaaS、マルチ vNIC インスタンス、大きなサブネット・サイズの選択肢が広がります。

## ストレージ能力

{{site.data.keyword.cloud_notm}} Virtual Servers for Virtual Private Cloud インスタンスをプロビジョンすると、プライマリー・ブート・ボリュームとして 100 GB の汎用 IOPS (3 IOPS/GB) ブロック・ストレージ・ボリュームが自動的に作成され、インスタンスに接続されます。ブロック・ストレージ・ボリュームは、VPC ネットワークに仮想サーバー・インスタンスをプロビジョンするときに作成することも、VSI ライフサイクルとは関係なく新規に作成することもできます。

VPC 用に追加のブロック・ストレージをプロビジョンするときには、ブロック・ストレージ・ボリュームの [IOPS ティア](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers)を選択することも、[カスタム IOPS プロファイル](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom)を指定することもできます。

## クラウド・ネイティブ・ワークロードおよびハイブリッド・ワークロード用に設計

VPC は、クラウド・ネイティブ・ワークロードを実行する場合や、既存のインフラストラクチャーを {{site.data.keyword.cloud_notm}} にリンクする場合に理想的です。コグニティブ、AI、機械学習の計算などのワークロードに最適なクラウドを作成できます。

ハイブリッド・ワークロード、クラウド耐性ワークロード、クラウド・ネイティブ・ワークロードをサポートする VPC のその他の機能を以下に示します。

* 水平自動スケーリングを使用したロード・バランシング
* モニターとヘルス・チェック
* GPU のサポート
* アフィニティー・グループとアンチアフィニティー・グループを使用した VSI プロビジョニング

## 詳細はこちら

* [**VPC の用語**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**VPC のセキュリティー**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**使用可能な VPC の地域とサブネット**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**VPC のネットワーク・リソースの作成と管理**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**VPC の仮想サーバー・インスタンスの作成と管理**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**VPC のブロック・ストレージの作成と管理**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
