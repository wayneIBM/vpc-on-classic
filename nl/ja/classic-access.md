---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# VPC からクラシック・インフラストラクチャーへのアクセスのセットアップ
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (リンクされたヘルプ・トピック)

どのアカウントでも、各地域の VPC から {{site.data.keyword.cloud}} クラシック・インフラストラクチャーへのアクセス (直接リンク接続など) をセットアップできます。 このような特殊な「クラシック・アクセス VPC」では、{{site.data.keyword.cloud}} クラシック・インフラストラクチャーと同じルーティング機能 (暗黙ルーター) が使用されます。読者は、クラシック・インフラストラクチャー・ネットワーキングに精通している必要があります。

VPC にクラシック・アクセスをセットアップすると、クラシック・アカウント内のパブリック・インターフェースを持たないすべてのコンピュート・ホスト (VSI やベアメタル) が、クラシック・アクセス VPC との間でパケットを送受信できるようになります。ただし、ファイアウォール、ゲートウェイ、ネットワーク ACL、セキュリティー・グループによって、そのトラフィックをフィルタリングできることを忘れないでください。 ベスト・プラクティスとして、アプリケーションの正常な機能に必要なトラフィックだけを許可することをお勧めします。

パブリック・インターフェースを持つホストでは、クラスに対応した VPC に戻る経路を追加する必要があります。
{: important}

## 前提条件:
{: #vpc-prerequisites}

1. クラシック・アカウントを IBM Cloud アカウントにリンクしておく必要があります。 方法については、[IBMid アカウントのリンク](/docs/account?topic=account-unifyingaccounts)を参照してください。
1. クラシック・アカウントの VRF を有効にしておく必要があります。
    * アカウントに直接リンクが既にある場合は、準備完了です。
    * アカウントの VRF が有効になっていない場合は、チケットを開いてアカウントの「VRF マイグレーション」を依頼してください。 変換プロセスの詳細については、直接リンクの資料に記載している[変換の開始方法](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion)を参照してください。

ファイアウォール、ゲートウェイ、ネットワーク ACL、セキュリティー・グループを使用して、クラシック・リソースと VPC リソースの間のネットワーク・トラフィックの一部または全部を除外できます。
{: important}

クラシック・アクセスを使用する VPC 内のすべてのサブネットは、`10.0.0.0/8` スペース内の IP アドレスを使用するクラシック・インフラストラクチャー VRF で共有されます。IP アドレスの競合を避けるため、クラシック・アクセス VPC にサブネットを作成するときは、`10.0.0.0/8` スペースの IP アドレスを**使用しないでください**。
{: important}

## クラシック・アクセス VPC の作成
{: #create-a-classic-access-vpc}

ユーザー・インターフェース (UI)、コマンド・ライン・インターフェース (CLI)、アプリケーション・プログラミング・インターフェース (API) のいずれかを使用して、クラシック・アクセス機能を備えた VPC を作成できます。

クラシック・アクセスのセットアップは、VPC の作成時に行う必要があります。VPC を更新してクラシック・アクセス機能を追加/削除することはできません。
{: note}

### ユーザー・インターフェースによるクラシック・アクセス VPC の作成
{: #create-a-classic-access-vpc-from-the-user-interface}

クラシック・アクセス VPC を作成するには、**「VPC の作成」**ページで、**「クラシック・アクセス」**ラベルの下にある**「クラシック・リソースに対するアクセスを有効にする (Enable access to classic resource)」**というタイトルのチェック・ボックスをクリックします。

![クラシック・アクセス UI](/images/classic-access-ui.png)

### CLI によるクラシック・アクセス VPC の作成
{: #create-a-classic-access-vpc-using-the-cli}

VPC の作成時に `--classic-access` フラグを使用します。 以下に例を示します。

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### API によるクラシック・アクセス VPC の作成
{: #create-a-classic-access-vpc-using-the-api}

VPC の作成時に `classic_access` パラメーターを渡します。 以下に例を示します。

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### クラシック・アクセス VPC のデフォルトのアドレス接頭部
{: #classic-access-default-address-prefixes}

クラシック・アクセスを使用する VPC 内のすべてのサブネットは、`10.0.0.0/8` スペース内の IP アドレスを使用するクラシック・インフラストラクチャー VRF で共有されます。IP アドレスの競合を避けるため、クラシック・アクセス VPC にサブネットを作成するときは、`10.0.0.0/8` スペースの IP アドレスを**使用しないでください**。
{: important}

新しいクラシック・アクセス VPC が作成されると、以下の表に示されているように、リージョンとゾーンに基づいてデフォルトのアドレス接頭部が割り当てられます。

ゾーン         | アドレス接頭部
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## 制限
{: #vpc-limitations}

* アカウントのプライベート暗黙ルーターには、プライベート (以前の資料では「バック」) ネットワークだけが接続します。
* プライベート暗黙ルーターのクラシック側には、クラシック API で割り当てたサブネットだけが接続されます。 暗黙ルーターのルーティング機能は、クラシック VLAN 上のサブネット間では機能しません。
* クラシック・アクセスを有効にできる VPC は、1 アカウントの 1 地域につき 1 つだけです。

クラシック VSI またはベアメタル・サーバーにインストールした OS に応じて、構成手順は異なります。詳しくは、[パブリック Virtual Server について](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers)を参照してください。
{: note}
