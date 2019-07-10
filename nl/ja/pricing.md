---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# VPC の料金
{: #pricing-for-vpc}

{{site.data.keyword.cloud}} 仮想プライベート・クラウドを使用したインターネット・データ転送に関する料金を、以下の表にまとめます。 VPC と他のクラシック IBM Cloud サービスの間のトラフィックは無料です。また、PayGo サービスの場合、サービス層は特定の VPC インスタンスではなくアカウントに結び付けられます。 金額は米ドルで示しています。

IBM Cloud VPC 内で使用された[仮想サーバー・インスタンス](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc)および[ブロック・ストレージ](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing)には、別料金が適用されます。

## インターネット・データ転送の無料枠
{: #free-allowances-for-internet-data-transfer}

| データ転送 |  IBM Cloud VPC のすべてのお客様のコスト |
|---------------|------------------|
| ゾーン内 | 無料 |
| 同一地域内のゾーン間 | 無料 |
| パブリック・ゲートウェイの使用 | 無料 (PGW で使用した浮動 IP のみが課金対象) |

## 浮動 IP の料金
{: #floating-ip-pricing}

浮動 IP は、予約した時点から開始して 1 カ月あたり $1 (米) のレートで課金されます。 浮動 IP が VSI に関連付けられていない場合や使用されていない場合でも、料金がかかります。 浮動 IP を数日予約しただけでも、1 カ月分の料金 $1 が課金されます。

## インターネット・データ転送に関する基本的な PayGo 料金
{: #basic-paygo-pricing-for-internet-transfer}

| データ転送 | データの量 | PayGo 料金 |
|-----------|-----------|------------------|
| インターネットへの発信 |  0 から 5 GB | 無料 |
|  | 6 から 10,000 GB | $0.087/GB |
|  | 10,001 から 50,000 GB | $0.083/GB |
|  | 50,001 から 150,000 GB | $0.07/GB |
|  | 150,001 GB 以上 | $0.05/GB |


VPC を新規作成してから、最初に請求料金がコンソール UI または API に表示されるまでに、最大 1 時間かかることがあります。
{: tip}

パブリック・ゲートウェイまたは浮動 IP を所有している場合は、発信トラフィックをまったく送信していない時間であっても、わずかな発信料金が表示されることがあります。 これは、アカウントの運用に必要な ARP トラフィックにかかる料金です。
{: important}

## LBaaS for VPC の料金
{: #lb-for-vpc-pricing}

Load Balancers for VPC の料金は、月次で計算される以下のメトリックに基づきます。
* サービス利用時間
* データ処理量


| 地域 | LB サービス使用量 (1 時間あたり) | 1 ギガバイト(GB) あたりの LB データ処理量 |
|------------|--------------------------|-------------------------|
| ダラス | 0.025 ドル | 0.008 ドル |
| フランクフルト | 0.027 ドル | 0.009 ドル |
| 東京 | 0.028 ドル | 0.009 ドル |

価格はマルチゾーン地域によって異なります。
{: note}

次のグラフは、パブリック・ロード・バランシングのために 1 カ月当たり 4500 GB を使用する顧客の例です。ダラス地域に基づいて米ドルで示しています。

| 項目 | 使用法 | レート |コスト|
|---------|--------|---------|---------|          
| LB サービス使用量| 720 時間 | 0.025 ドル/時間 | 1 カ月当たり 18 ドル |
|データ処理量| 4500GB  |  0.008 ドル/GB | 1 カ月当たり 36 ドル |

**表 1: 月次コストの例。このシナリオの合計料金は、1 カ月当たり 54 ドル (米ドル) です。**


## VPN for VPC の料金
{: #vpn-for-vpc-pricing}

| 地域 | 1 時間当たりの接続 (ピア) | 1 時間当たりのインスタンス (ゲートウェイ) |
|------------|--------------------------|-------------------------|
| ダラス | 0.04 ドル | 0.045 ドル |
| フランクフルト | 0.044 ドル | 0.0495 ドル |
| 東京 | 0.0452 ドル | 0.0585 ドル |

VPNaaS の使用に起因したインターネットへのデータ転送は、通常のデータ転送 (インターネットへのアウトバウンド) として課金され、VPC レベルで請求されます。
{: note}
