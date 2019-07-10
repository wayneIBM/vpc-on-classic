---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# VPC でのブロック・ストレージの作成と管理
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} 仮想プライベート・クラウドはブート・ストレージ・ボリュームを備えていますが、さらに、ハイパーバイザーにマウントされた高性能データ・ストレージであるブロック・ストレージ・ボリュームを仮想サーバー・インスタンス (VSI) 用に追加することもできます。VPC インフラストラクチャーでは、優れたパフォーマンスとセキュリティーを維持しながら、複数の地域とゾーンにわたって迅速にストレージをスケーリングできます。

## 特性の要約
{: #summary-of-characteristics}

{{site.data.keyword.vsi_is_full}} インスタンスをプロビジョンすると、プライマリー・ブート・ボリュームとして 100 GB の汎用 IOPS (3 IOPS/GB) ブロック・ストレージ・ボリュームが自動的に作成され、VSI に接続されます。プロビジョニング時に、ブート・ボリュームの名前とサイズを変更できます。
{:shortdesc}

* VPC ネットワークに仮想サーバー・インスタンスをプロビジョンするときはいつでもブロック・ストレージ・ボリュームを作成できます。  
* VSI のライフサイクルとは無関係に新規ボリュームを作成し、後で VSI に接続できます。
* VSI に接続される 2 次データ・ボリュームを自動的に作成できます。 
* データ・ボリュームは VSI から切り離されても残ります。そのため、後で新しいインスタンスにボリュームを接続できます。 
* VSI の削除時にデータ・ボリュームを自動的に削除するかどうかを指定できます。  
* ブート・ボリュームは、VSI の削除時に削除されます。
* デフォルトでは、ブート・ボリュームとデータ・ボリュームは IBM 管理の暗号化を使用して暗号化されます。 
* VSI のプロビジョニング時またはスタンドアロン・ボリュームの作成時に、独自の暗号鍵を使用してボリュームを暗号化できます。


## ブロック・ストレージ・ボリューム
{: #block-storage-volumes}

ブロック・ストレージ・ボリュームや VPC の操作について詳しくは、[VPC のブロック・ストレージについての資料](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)を参照してください。

VPC のブロック・ストレージ・ボリュームに関する概要については、[About Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) を参照してください。 

VSI のプロビジョニングとは無関係にボリュームの作成を開始するには、[Getting Started with {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started) を参照してください。



## VPC ブロック・ストレージ・ボリュームの作成と命名のベスト・プラクティスを以下に示します。
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* ボリュームをプロビジョニングする前に、ストレージ要件を決定します。容量と IOPS パフォーマンスには十分な余裕をみておいてください。
* 容量とパフォーマンスの要件に最適な事前定義済みの IOPS プロファイルがあるか、それとも、カスタム設定を指定するほうが良いかを判断します。
* 仮想サーバー・インスタンスのプロビジョニング時に 2 次データ・ボリュームを作成するかどうかを決めます。これらのボリュームは自動的にインスタンスに接続されます。
* VSI の削除時に、その VSI に接続されているボリュームを自動的に削除するかどうかを決めます。
* 独自の暗号鍵を使用してボリュームを暗号化する場合は、鍵が有効であること、鍵を使用する権限があること、現在の地域で使用できる鍵であることを確認します。
* プロビジョン時にブート・ボリュームを含むすべてのボリュームが命名されていることを確認します。
* 接続時にすべてのボリューム接続に命名するようにします。
* 各ボリュームの名前は、アカウント内および地域内で一意にする必要があります。

ボリュームが削除された後、そのボリュームの名前は再使用できます。 ただし、ボリューム名を再使用すると、請求処理で混乱が発生する可能性があるので注意してください。
{:note}
