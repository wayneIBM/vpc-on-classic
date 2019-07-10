---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Block Storage for VPC の料金
{: #block-storage-pricing}

{{site.data.keyword.block_storage_is_short}} の料金は、選択したストレージ・プロファイルに応じ、プロビジョンされたキャパシティーまたは IOPS に基づきます。

| IOPS 層    |公開月極レート^1 |
|------------|--------------|
|  3 IOPS/GB |  0.13 USD/GB |
|  5 IOPS/GB |  0.25 USD/GB |
| 10 IOPS/GB |  0.58 USD/GB |
| カスタム IOPS| 0.10 USD/GB + 0.07 USD/IOPS |

^1 1 カ月当たりの公開価格、[時間単位で計算](#how-are-charges-calculated)。

## 料金の計算方法
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} は、アカウントにブロック・ストレージ・ボリュームが存在する合計時間数に基づいて、ボリュームが削除されるかまたは請求サイクルの終了のいずれか早い方まで、毎時計算されます。事前定義された IOPS 層では、カスタム IOPS とは異なる方法で毎時の請求が計算されます。次の例を参照してください。

### 汎用 IOPS 層を使用したボリュームの請求計算
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

汎用の 3 IOPS/GB 層を使用して 1000 GB のボリュームをプロビジョンし、そのボリュームを 72 時間使用した後削除します。このボリュームの合計料金は、使用時間により次のように請求されます。

((1000 GB x 0.13 USD/GB)/730 時間) x 72 時間 = $12.82

### カスタム IOPS を使用したボリュームの請求計算
{: #billing-calculation-for-a-volume-with-custom-IOPS}

2500 IOPS を使用して 1000 GB のボリュームをプロビジョンし、そのボリュームを 72 時間使用した後に削除します。このボリュームの合計料金は、使用時間により次のように請求されます。

(((1000 x 0.10 USD/GB) + (2500 x 0.07 USD)) / 730 時間) x 72 時間 = $27.12
