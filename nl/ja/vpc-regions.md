---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

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

# 別の地域での VPC の作成
{: #creating-a-vpc-in-a-different-region}

地域は、アプリ、サービス、およびその他の {{site.data.keyword.cloud}} リソースをデプロイできる特定の地理的な場所です。 地域は 1 つ以上のゾーンで構成されます。ゾーンは物理データ・センターであり、ホスト・サービスおよびホスト・アプリケーション用に、関連冷却機器および電源機器を備えたコンピュート・リソース、ネットワーク・リソース、ストレージ・リソースを格納しています。 地域内で単一障害点を共有しないようにゾーンは互いに独立しています。

IBM Cloud VPC サービスは地域的なものです。災害復旧 (DR) を可能にするには、他の地域のいずれかで VPC を確立することにより、代替地域へのフェイルオーバーを使用したリカバリー機能を提供する必要があります。
{: note}

仮想プライベート・クラウドは、すべての {{site.data.keyword.cloud}} 地域で段階的にデプロイされています。

|   場所     | 地域 | API エンドポイント | 状況 |
| ------- | :------: | :------: |:------: |
| ダラス | us-south | `us-south.iaas.cloud.ibm.com`| 使用可能 |
| フランクフルト | eu-de | `eu-de.iaas.cloud.ibm.com`| 使用可能 |
| 東京 | jp-tok | `jp-tok.iaas.cloud.ibm.com`| 使用可能 |
| ロンドン | eu-gb | `eu-gb.iaas.cloud.ibm.com`| 近日対応 |
| シドニー | au-syd | `au-syd.iaas.cloud.ibm.com`| 近日対応 |
| ワシントン DC | us-east | `us-east.iaas.cloud.ibm.com`| 近日対応 |

特定の地域にログインすると、IBM Cloud CLI によって地域 API (VPC) エンドポイントが自動的に設定されます。
{: note}

## CLI を使用した特定の地域へのログイン
{: #log-in-to-a-specific-region-using-the-cli}

地域は、IBM Cloud にログインするときに指定することも、後から選択することもできます。例えば、ダラス (`us-south`) 地域のグローバル API エンドポイントに直接ログインするには、統合アカウント (SSO) と非統合アカウントのどちらを持っているかに応じて、次のいずれかのコマンドを実行します。

統合アカウントの場合

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

統合アカウントでない場合

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

後で地域を選択する場合は、`-r <region>` パラメーターを指定しないでください。そうすれば、地域を選択するプロンプトが CLI に表示されます。

出力例:

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## CLI を使用した地域の切り替え
{: #switch-regions-using-the-cli}

VPC 地域の最新の状況を取得するには、次のコマンドを実行します。

```
ibmcloud is regions
```
{: pre}

別の地域に切り替えるには、`ibmcloud target -r <region>` コマンドを実行します。 例えば、フランクフルト地域に切り替えるには、次のコマンドを実行します。

```
ibmcloud target -r eu-de
```
{: pre}

現在の場所を確認するには、次のコマンドを実行します。

```
ibmcloud target
```
{: pre}

## API を使用した地域の切り替え  
{: #switch-regions-using-the-api}

REST を使用して地域 VPC API と対話するには、リソースを作成したい地域と関連付けられた API エンドポイントに要求を送信します。地域の API エンドポイントは、前の表に記載しています。また、次のコマンドを実行して、地域に関連付けられたエンドポイントを見つけることもできます。

```
ibmcloud is regions
```
{: pre}


例えば、`us-south` 地域の VPC のリストを取得するには、次のコマンドを実行します。

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## ゾーンの取得
{: #get-zones}

地域ごとの使用可能なゾーンのリストを取得するには、コマンド `ibmcloud is zones <region>` を実行します。例えば、地域 `us-south` のゾーンのリストを取得するには、次のコマンドを実行します。

```
ibmcloud is zones us-south
```
{: pre}
