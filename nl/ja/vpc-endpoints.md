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

# IBM Cloud VPC 用のサービス・エンドポイント
{: #service-endpoints-available-for-ibm-cloud-vpc}

ワークロードを実行する準備ができたら、Infrastructure as a Service (IaaS) エンドポイントとクラウド・サービス・エンドポイント (CSE) の 2 つのタイプのエンドポイントにアクセスできます。IaaS エンドポイントは事前にプロビジョンされており、実行する準備ができていますが、これを使用する前に、別に CSE をプロビジョンする必要があります。

これらのサービスのエンドポイントは、_ルーティング可能アドレス_、つまり、RFC 1918 で指定されたアドレス以外のアドレスを使用します。これらのアドレスはパブリック・インターネット経由で通信しているかのように見えますが、これらのエンドポイントとの間のトラフィックはクラウドの外に出ることはありません。したがって、このトラフィックには、クラウドから出てパブリック・インターネットに向かうトラフィックに関連した帯域幅の料金がかかりません。

## Infrastructure as a Service (IaaS) エンドポイント
{: #infrastructure-as-a-service-iaas-endpoints}

インフラストラクチャー・サービスは、`adn.networklayer.com` ドメインの特定の DNS 名を使用することによって利用可能で、`161.26.0.0/ 16` アドレスに解決されます。

以下のサービスにアクセスできます。

* DNS リゾルバー
* Ubuntu APT (拡張パッケージ化ツール) ミラー
* Cloud Object Storage
* NTP

## DNS (ドメイン・ネーム・システム) リゾルバー・エンドポイント
{: #dns-domain-name-system-resolver-endpoints}

DNS リゾルバーは、名前ではなく IP アドレスを使用します。共有クラウド・サービス・エンドポイントの場合、DNS サーバー・アドレス `161.26.0.10` および `161.26.0.11` を使用する必要があります。

## Ubuntu APT ミラー
{: #ubuntu-apt-mirrors}

Ubuntu イメージを更新するための APT ミラーは、`mirrors.adn.networklayer.com` から入手できます。これは、`161.26.0.6` に解決されます。 

## IBM Cloud オブジェクト・ストレージ
{: #ibm-cloud-object-storage}

例えば、IBM Cloud プライベート・ネットワーク上のオブジェクト・ストレージ・サービスのプライベート・エンドポイントにアクセスするには、URL の「ドメイン」の部分を `cloud-object-storage.appdomain.cloud` に置き換えます。別の地域で作成された COS バケットにアクセスするには、VSI が作成された地域のエンドポイントではなく、バケットのエンドポイントを使用します。

## Network Time Protocol (NTP) サーバー
{: #network-time-protocol-ntp-servers}

NTP サーバーは、`time.adn.networklayer.com` から入手可能です。これは、`161.26.0.6` に解決されます。

## クラウド・サービス・エンドポイント
{: #cloud-service-endpoints}

クラウド・サービス・エンドポイントは、他のクラウド・ユーザーによって提供されるサービスです。これらは、まもなく `cloud.ibm.com` ドメイン内の DNS 名を使用して利用可能になる予定です。これらは `166.8.0.0/14` アドレスに解決されます。
