---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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

# VPC から IBM Cloud オブジェクト・ストレージへの接続
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

この資料では、仮想プライベート・クラウドから {{site.data.keyword.cloud}} オブジェクト・ストレージに接続する方法を示します。エンドポイント情報は特に、クラシック・インフラストラクチャー環境の {{site.data.keyword.cloud_notm}} 仮想プライベート・クラウドに適用されます。
{: shortdesc}


## IBM Cloud オブジェクト・ストレージ (COS) とは
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} オブジェクト・ストレージ (COS) は、非構造化データを保管する Web スケールのプラットフォームです。複製なしで、信頼性、セキュリティー、可用性、および災害復旧が提供されます。

{{site.data.keyword.cloud_notm}} オブジェクト・ストレージに保管された情報は暗号化され、複数の地理的位置に分散されます。これは、S3 API の実装を介してアクセス可能です。 このサービスは、IBM Cloud オブジェクト・ストレージ・システムが備える分散ストレージ・テクノロジーを利用します。

IBM Cloud オブジェクト・ストレージには、耐障害性のために、**クロス地域**、**地域**、**単一データ・センター**という 3 種類の構成があります。 
* **クロス地域**サービスでは、単一地域を使用した場合よりも耐久性と可用性が高くなりますが、その代わりに待ち時間が若干長くなります。このサービスは、現在、米国、EU、アジア太平洋の地域で提供されています。 
* **地域**サービスでは、このトレードオフが逆になります。単一地域内の複数のアベイラビリティー・ゾーンにオブジェクトが分散されます。これは、米国、EU、アジア太平洋の地域で提供されています。特定の地域またはゾーンが使用不可になっても、オブジェクト・ストアは障害なく機能し続けます。
**単一データ・センター**・サービスでは、同一物理ロケーション内の複数のマシンにオブジェクトが分散されます。対応地域については、この資料を定期的に確認してください。

### VPC で使用する COS ダイレクト・エンドポイント
{: #cos-direct-endpoints-for-use-with-vpc}

REST API 要求を送信するには、ターゲット・エンドポイントつまり URL を設定する必要があります。また、COS ストレージ・ロケーションごとに、独自の URL セットが必要です。ダイレクト・エンドポイントにより、サーバーから {{site.data.keyword.cloud_notm}} サービスに直接の高速接続を確立できます。帯域幅の追加費用は一切発生しません。COS ダイレクト・エンドポイントを使用して、VPC は世界中の COS バケットに接続できます。 

このページにリストしているすべての COS エンドポイントへの VPC からのトラフィックには課金されません。
{: note}

* VPC クライアントは、パブリック・エンドポイントを介して COS バケットにアクセスすることもできます。
* VPC クライアントから [COS 構成 API](https://{DomainName}/apidocs/cos/cos-configuration) にアクセスする場合には、ダイレクト・エンドポイントではなく、パブリック・エンドポイントを使用する必要があります。COS 構成 API を使用して、バケットのいくつかの COS 機能を構成したり、バケット・メタデータを表示したりできます。
* ファイアウォールが有効になっている場合は、VPC クライアントは COS バケットにアクセスできません。

## VPC から IBM Cloud オブジェクト・ストレージ (COS) に接続する方法
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. [カタログ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window} から COS をプロビジョンします。
2. 以下のセクションにリストしている地域のいずれかに COS バケットを作成します。
3. エンドポイントを使用して COS バケットと通信します。

### 地域エンドポイント
{: #regional-endpoints}

地域エンドポイントで作成したバケットでは、3 つのデータ・センターにデータが分散されます。それらのデータ・センターは大都市圏に分散されています。これらのデータ・センターのいずれか 1 つが停止したり破壊されたりしても、可用性に影響はありません。

| **地域** | **エンドポイント** |
|------------|-------------------------------|
| 米国南部 | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| 米国東部 | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| EU 英国 | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| EU ドイツ | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| アジア太平洋 オーストラリア | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| アジア太平洋 日本 | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### クロス地域エンドポイント
{: #cross-region-endpoints}

クロス地域エンドポイントで作成したバケットでは、3 つの地域にデータが分散されます。これらの地域のいずれかが停止したり破壊されたりしても、可用性に影響はありません。Border Gateway Protocol (BGP) ルーティングを使用して、最も近い地域のデータ・センターに要求が転送されます。

停止した場合は、アクティブな地域に自動的に要求が再転送されます。独自のフェイルオーバー・ロジックを作成したい上級者は、特定の地域のエンドポイントに要求を送信して BGP ルーティングをバイパスすることができます。

| **米国クロス地域** | **エンドポイント** |
|------------|-------------------------------|
| 米国クロス地域 | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| ダラス・アクセス・ポイント | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| サンノゼ・アクセス・ポイント | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **EU クロス地域** | **エンドポイント** |
|------------|-------------------------------|
| EU クロス地域 | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| アムステルダム・アクセス・ポイント | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| フランクフルト・アクセス・ポイント | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| ミラノ・アクセス・ポイント | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **アジア太平洋クロス地域** | **エンドポイント** |
|------------|-------------------------------|
| アジア太平洋クロス地域 | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| 東京アクセス・ポイント | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| ソウル・アクセス・ポイント | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| 香港アクセス・ポイント | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### 単一データ・センター・エンドポイント
 {: #single-datacenter-endpoints}

単一データ・センターは、IAM や Key Protect などの IBM クラウド・サービスと同じ場所には配置されていません。サイトが停止したり破壊されたりした場合の耐障害性はありません。

ネットワーク障害が原因でパーティションが発生し、データ・センターがコア IBM Cloud 地域に到達できず、IAM にアクセスできない場合は、無効になった可能性のあるキャッシュから認証情報と許可情報が読み取られます。このアクションの結果、新規または変更された IAM ポリシーが最大 24 時間適用されない可能性があります。
{:important}

| **地域** | **エンドポイント** |
|------------|-------------------------------|
| アムステルダム、オランダ | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| チェンナイ、インド | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
| 香港 | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| メルボルン、オーストラリア | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| メキシコ・シティー、メキシコ | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| ミラノ、イタリア | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| モントリオール、カナダ | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| オスロ、ノルウェー | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| サンノゼ、米国 | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| サンパウロ、ブラジル | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| ソウル、韓国 | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| トロント、カナダ | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
