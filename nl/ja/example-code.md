---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# REST API を使用した VPC の作成
{: #creating-a-vpc-using-the-rest-apis}

この資料では、REST API を使用して {{site.data.keyword.cloud}} 仮想プライベート・クラウドのリソースを作成する方法について説明します。ここでは、`curl` を使用します。Go および Python を使用して REST API を呼び出すコード・スニペットについては、こちらの[サンプル・コード](https://github.com/IBM-Cloud/vpc-api-samples)を参照してください。

## 前提条件

1. 仮想サーバー・インスタンスへの接続に使用するパブリック SSH 鍵が存在していることを確認します。

   公開 SSH 鍵が既に存在している可能性があります。 ホーム・ディレクトリーの `.ssh` ディレクトリー内で `id_rsa.pub` というファイル (例: `/Users/<USERNAME>/.ssh/id_rsa.pub`) を探します。 このファイルは `ssh-rsa` で始まり、E メール・アドレスで終わります。

   公開 SSH 鍵がない場合、または既存の SSH 鍵のパスワードを忘れた場合は、`ssh-keygen` コマンドを実行し、プロンプトの指示に従って新しい鍵を生成します。
2.  IBM Cloud アカウントの API キーが存在していることを確認します。API キーがない場合は、[API キーの作成](/docs/iam?topic=iam-userapikey#create_user_key)を参照してください。この API キーは、ステップ 1 で環境変数に保管する必要があります。

## ステップ 1: API キーを変数として保管する

以下のコマンドを実行して、API キーを環境変数に保管します。

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## ステップ 2: IBM IAM (ID およびアクセス管理) トークンを取得する

IAM トークンの取得方法について [API キーを使用した IBM Cloud IAM トークンの取得](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)のトピックを参照するか、以下のコマンド例を使用してください。

以下のコマンドを実行し、ユーティリティー `jq` を使用して、IAM トークンを取得および構文解析します。別の構文解析ツールを使用するようコマンドを変更することも、コマンドの最後の部分を手動で削除して自分でトークンを構文解析することもできます。

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

IAM トークンを表示するには、`echo $iam_token` を実行します。結果は次のようになります。

```
Bearer <your token>
```

Authorization ヘッダーでは、IAM トークンが `Bearer` で始まると想定されています。受け取った結果に `Bearer` が含まれていない場合は、`iam_token` 変数を更新して、この文字列が含まれるようにしてください。これらの例では、`Bearer` が `iam_token` に含まれていると想定されています。

IAM トークンは失効するため、上記のステップを繰り返して、IAM トークンを 1 時間ごとに更新する必要があります。
{: important}

## ステップ 3: API エンドポイントとバージョンのパラメーターを変数として保管する

API エンドポイントはリージョンごとに存在し、`https://<region>.iaas.cloud.ibm.com` という形式に従っています。リージョンごとのエンドポイントは、次のとおりです。

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

API エンドポイントを変数に保管することで、そのエンドポイントを API 要求で使用するときに URL 全体を入力せずにすみます。例えば、us-south エンドポイントを変数に保管するには、次のコマンドを実行します。

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

この変数が保存されたことを確認するには、`echo $rias_endpoint` を実行して、その応答が空ではないことを確認します。

すべての API 要求に `version` パラメーターを含める必要があります。形式は、`YYYY-MM-DD` にします。以下のコマンドを実行してバージョンの日付を変数に保管し、セッションで再利用できるようにします。`version` パラメーターの設定について詳しくは、[VPC の API リファレンス](https://{DomainName}/apidocs/vpc-on-classic#versioning)の**バージョン管理**を参照してください。

```bash
version="2019-05-31"
 ```
{: pre}

この変数が保存されたことを確認するには、`echo $version` を実行して、その応答が空ではないことを確認します。

## ステップ 4: GET regions API を実行する

次のコマンドを実行すると、VPC で使用可能なリージョンが JSON 形式で返されます。 少なくともオブジェクトが 1 つ返される必要があります。

API 要求にはそれぞれ、`version` 照会パラメーターと `generation` 照会パラメーターが必要です。詳しくは、[VPC の API リファレンス](https://{DomainName}/apidocs/vpc-on-classic)を参照してください。
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

予期しない結果が生じた場合は、`--verbose` (デバッグ) フラグを `curl` コマンドの後ろに追加して、詳細なロギング情報を取得してください。[トラブルシューティング・ページ](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc)にある、よくあるエラーも参照してください。
{: tip}


## ステップ 5: GET zones API を実行する

次のコマンドを実行すると、リージョン `us-south` 内の VPC で使用可能なすべてのゾーンが JSON 形式で返されます。少なくともオブジェクトが 3 つ返される必要があります。

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

使用しているリージョンのエンドポイントに応じて、上記のパラメーター `us-south` のリージョン名を更新します。リージョンのエンドポイントは独自のゾーンのみを認識するため、
東京のエンドポイントを使用する場合は、`jp-tok` を使用します。
{: note}

## ステップ 6: GET profiles API を実行する

次のコマンドを実行すると、ユーザーのインスタンスで使用可能なプロファイルが JSON 形式で返されます。 少なくともオブジェクトが 1 つ返される必要があります。

curl コマンドの後ろに ` | json_pp ` を追加して、読み取り可能な JSON ストリングを取得します。
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

API の出力がページ編集によって制限されている場合は、次の例のように `total_count` より大きい制限を設定してください。

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## ステップ 7: GET images API を実行する

次のコマンドを実行すると、ユーザーのインスタンスで使用可能なイメージが JSON 形式で返されます。 少なくともオブジェクトが 1 つ返される必要があります。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## ステップ 8: GET VPCs API を実行する

次のコマンドを実行すると、ユーザーのアカウントで作成された VPC が JSON 形式で返されます。

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## ステップ 9: 仮想プライベート・クラウドを作成する

`my-vpc` という名前の {{site.data.keyword.cloud_notm}} VPC を作成します。

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

これ以降の呼び出しでは、この新規作成した {{site.data.keyword.cloud_notm}} VPC の ID が必要になります。その ID を変数に保管します。コマンドは次のようになります。`vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## ステップ 10: サブネットを作成する

{{site.data.keyword.cloud_notm}} VPC 内でサブネットを作成します。以下の例では、`us-south-2` ゾーンに VPC を作成します。

以下の例では、ゾーン `us-south-2` に[デフォルトのアドレス接頭部](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)を使用しています。アカウントのデフォルトが変更されている可能性があります。[VPC アドレスを計画する方法](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design)を参照してください。
{: note}

vpc のアドレス接頭部のリストを取得するには、コマンド
`curl -X GET "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"` を実行します。
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

サブネット ID を変数に保管します。

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## ステップ 11: サブネットの状況を確認する

サブネット内でリソースをプロビジョンするには、このサブネットが `available` ステータスになっている必要があります。 作業を進める前に、サブネットのリソースを照会して、ステータスが `available` であることを確認してください。ステータスが `failed` である場合は、[サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)に連絡して詳細情報を提供してください。別のサブネットのプロビジョンを試行することで、作業を進めてみることもできます。

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## ステップ 12: パブリック・ゲートウェイを作成する

サブネット内の仮想サーバー・インスタンスがパブリック・インターネットにアクセスできるようにするには、パブリック・ゲートウェイ (PGW) をサブネットに追加します。  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

パブリック・ゲートウェイ ID を変数に保管します。

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

パブリック・ゲートウェイを接続して使用するには、パブリック・ゲートウェイが `available` 状況になっていなければなりません。作業を進める前に、パブリック・ゲートウェイのリソースを照会して、ステータスが `available` であることを確認してください。ステータスが `failed` である場合は、[サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)に連絡して詳細情報を提供してください。別のパブリック・ゲートウェイのプロビジョンを試行することで、作業を進めてみることもできます。

パブリック・ゲートウェイのステータスを確認するには、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## ステップ 13: パブリック・ゲートウェイをサブネットに接続する

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## ステップ 14: SSH 鍵を作成する

公開 SSH 鍵を使用して鍵を作成します。 この鍵は、仮想サーバー・インスタンスの作成時に使用されます。この鍵は、仮想サーバー・インスタンスにログインするためにも必要です。

鍵は、最初に VPC を UI または CLI で作成するときに追加できます。後で鍵を追加する方法はありません。
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

鍵 ID を変数に保管します。

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## ステップ 15: 仮想サーバー・インスタンスのプロファイルとイメージを選択する

API を実行して、仮想サーバー・インスタンスで使用可能なすべてのプロファイルとイメージを一覧表示して、組み合わせを選択します。

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

プロファイルの追加の詳細を取得するには、{{site.data.keyword.cloud_notm}} CLI コマンド `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]` を使用して、グローバル・カタログを照会します。例えば次のようにします。

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

次のコマンドを実行すると、使用可能なイメージが一覧表示されます。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

プロファイル名とイメージ ID は、変数に保管します。その変数を、仮想サーバー・インスタンスのプロビジョンに使用します。

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## ステップ 16: 仮想サーバー・インスタンスをプロビジョンする

新しく作成したサブネットで仮想サーバー・インスタンス (VSI) をプロビジョンします。公開 SSH 鍵を渡して、インスタンスのプロビジョン後にログインできるようにします。

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-south-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

仮想サーバー・インスタンス ID を変数に保管します。

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## ステップ 17: 仮想サーバー・インスタンスの状況を確認する

仮想サーバー・インスタンスのプロビジョニングには、数分かかることがあります。作業を進める前に、サーバーのステータスを照会して、サーバーが `running` ステータスであることを確認してください。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

(前の API 呼び出しで返された) 仮想サーバー・インスタンスの 1 次ネットワーク・インターフェース ID を変数に保管します。  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

当該インスタンスを照会するまで、1 次ネットワーク・インターフェースを取得することはできません。
{: note}

## ステップ 18: 浮動 IP を作成する

仮想サーバー・インスタンスの浮動 IP を作成するには、このインスタンスの 1 次ネットワーク・インターフェースをこの新しい浮動 IP アドレスのターゲットとして使用します。

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


浮動 IP の ID を変数に保管します。

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## ステップ 19: SSH トラフィックを許可するデフォルト・セキュリティー・グループにルールを追加する

セキュリティー・グループを構成して、インスタンスで許可されるインバウンドとアウトバウンドのトラフィックを定義します。

VPC のセキュリティー・グループを見つけます。

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

セキュリティー・グループの ID を変数に保管します。

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

ここで、インバウンド SSH トラフィックを許可するルールを作成します。

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## ステップ 20: 仮想サーバー・インスタンスに SSH で接続する

サーバーに SSH で接続するには、先ほど作成した浮動 IP の `address` を使用します。浮動 IP の `address` を取得するには、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

浮動 IP の `address` を使用して、仮想サーバー・インスタンスに SSH で接続します。

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

仮想サーバーへの SSH アクセスはセキュリティー・グループによってブロックされることがあります。 SSH アクセスが必要な場合は、[適切なセキュリティー・グループ](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)を使用する必要があります。
{: note}

インスタンスへの接続方法について詳しくは、[Connecting to your instance using Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) を参照してください。

## ステップ 21: ブロック・ストレージ・ボリュームを作成して接続する

以下の例のような要求を使用してブロック・ストレージ・ボリュームを作成し、ボリューム名、ゾーン、およびプロファイルを指定します。

ボリューム・プロファイルのリストを表示するには、次の要求を指定します。

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

プロファイルには、general-purpose (3 IOPS/GB)、5iops-tier、10iops-tier、custom を指定できます。選択したボリューム・プロファイルに基づくボリューム容量と IOPS 範囲については、[VPC のブロック・ストレージについて](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)を参照してください。


要求にボリューム名を指定する必要はありません。デフォルトで、ボリューム名はシステムによって作成されます。この例では、ボリューム名 `helloworld-vol` を指定します。ボリューム名はすべて固有でなければなりません。

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

これ以降の呼び出しでは、この新規作成したボリュームの ID が必要になります。ボリュームの ID を変数として保存します。例えば、`vol=640774d7-2adc-4609-add9-6dfd96167a8f` です。

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

ボリュームを初めて作成する場合、ボリュームの状況が `pending` です。続行する前に、ボリュームの状況が `available` になる必要があります。これには数分かかります。
{: note}


ボリュームの状況を確認するには、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

ボリューム接続を作成して、新規ボリュームを仮想サーバー・インスタンスに接続します。先ほど作成したインスタンス ID 変数を要求で使用します。ボリューム ID、ボリュームの CRN、または URL を使用して、データ・パラメーターでボリュームを指定します。この例では、ボリューム ID 用に作成した変数を使用します。

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

ボリューム接続を作成した後、ボリューム接続 ID を取得します。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

接続 ID を変数として保存します。

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

ボリューム接続 ID を指定して、新規ボリュームを仮想サーバー・インスタンスに接続します。

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## ステップ 22 (オプション): リソースを削除する

オプションで、リソースを削除します。他のリソースが含まれているリソースを削除することはできません。 例えば、サブネットが含まれている仮想プライベート・クラウドを削除することはできず、仮想サーバー・インスタンスが含まれているサブネットを削除することはできません。削除操作を行うと、API は素早く復帰する可能性がありますが、リソースの削除自体はまだ進行中である可能性があります。削除要求を出した後、親リソースの削除を試行する前に、リソースが削除されていることを確認してください。詳しくは、[VPC の削除](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)を参照してください。

### 浮動 IP の削除

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 仮想サーバー・インスタンスの削除

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 鍵の削除

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### サブネットの削除

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### パブリック・ゲートウェイの削除

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### VPC の削除

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### ボリュームの削除

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## これで完了です。

REST API を使用して、仮想プライベート・クラウド・インスタンスを正常にプロビジョンできました。さらに別の API コマンドを試すには、詳細なリファレンスを参照してください。

* [VPC の API リファレンス](https://{DomainName}/apidocs/vpc-on-classic)
