---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

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

# REST API を使用した VPC の削除
{: #deleting-using-api}

REST API を使用して {{site.data.keyword.cloud}} 仮想プライベート・クラウドを削除するときの[削除](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)プロセスの大まかな手順は、[CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli) を使用して削除するときと同じです。

このプロセスの主な手順を以下に示します。

1. 削除する VPC 内のすべてのサブネットを確認します。
2. VPC 内の各サブネットを削除します。つまり、以下を削除します。
    - サブネット内の VPN ゲートウェイをすべて削除します。
    - サブネット内のロード・バランサーをすべて削除します。
    - サブネット内のネットワーク・インターフェース (インスタンス) をすべて削除します。
    - サブネットを削除します。
3. VPC 内のパブリック・ゲートウェイをすべて削除します。
4. VPC を削除します。

以下のセクションでは、VPC を削除するために実行できる、`curl` を使用した API 呼び出しの例をいくつか示します。

## 手順 1: 削除する VPC のすべてのサブネットを確認する
{: #deleting-find-subnets-api}

VPC を削除する前に、その VPC の各サブネットを削除する必要があります。次のコマンドを実行し、`id` 値を参照して、削除する VPC の ID を確認します。

この curl コマンドの後ろに ` | json_pp ` または ` | jq ` を追加すると、読み取り可能な JSON ストリングを取得できます。
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

後で利用できるように、VPC の ID を変数に保存します。次に例を示します。

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

アカウント内のすべてのサブネットのリストを取得するために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

`vpc` 値を参照して、削除する必要があるサブネットを確認します。後で利用するために、サブネットの ID を変数に保存します。次に例を示します。

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

削除する VPC に複数のサブネットがある場合は、すべてのサブネットを削除するために、この手順を繰り返してください。
{: important}

## 手順 2: VPC 内の各サブネットを削除する
{: #deleting-subnet-resources-api}

### サブネット内のすべての VPN ゲートウェイを削除する (存在する場合)

アカウント内のすべての VPN ゲートウェイをリストするために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

削除するサブネット内の VPN ゲートウェイごとに、次のコマンドを実行します。`$vpnid` は VPN ゲートウェイの ID です。

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

すぐに VPN ゲートウェイの状況が `deleting` に変わるはずですが、リスト照会を実行すると、まだ戻されるはずです。VPN ゲートウェイの削除には、最大で 30 分かかる場合があります。VPN ゲートウェイの削除を待つ間に、他のサブネット・リソースの削除を並行して要求できますが、サブネットの削除は、リスト照会でそのサブネット内の VPN ゲートウェイとその他のすべてのリソースが表示されなくなるまで実行できません。

### サブネット内のすべてのロード・バランサーを削除する (存在する場合)
{: #deleting-lbs-api}

アカウント内のすべてのロード・バランサーをリストするために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

削除するサブネット内のロード・バランサーごとに、次のコマンドを実行します。`$lbid` はロード・バランサーの ID です。

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

すぐにロード・バランサーの状況が `deleting` に変わるはずですが、リスト照会を実行すると、まだ戻されるはずです。ロード・バランサーの削除には、最大で 30 分かかる場合があります。ロード・バランサーの削除を待つ間に、他のサブネット・リソースの削除を並行して要求できますが、サブネットの削除は、リスト照会でそのサブネット内のロード・バランサーとその他のすべてのリソースが表示されなくなるまで実行できません。

### サブネット内のすべてのネットワーク・インターフェースを削除する (存在する場合)
{: #deleting-nics-api}

インスタンスには、VPC の複数のサブネットに属する複数のネットワーク・インターフェースが含まれている場合があります。サブネットを削除するには、先に、そのサブネット内のすべてのネットワーク・インターフェースを削除しておく必要があります。サブネット内のネットワーク・インターフェースを削除するには、そのインスタンスを削除するしかありません。

アカウント内のすべてのインスタンスをリストするために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPC 内のインスタンスをすべて削除する場合は、VPC 内のインスタンスごとに次のコマンドを実行します。`$vsi` は削除するインスタンスの ID です。インスタンスを削除すると、他のサブネットのネットワーク・インターフェースが (ある場合) すべて自動的に削除されます。

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

すぐにインスタンスの状況が `deleting` に変わるはずですが、リスト照会の結果にはまだ表示されるはずです。インスタンスの削除には、最大で 30 分かかる場合があります。インスタンスの削除を待つ間に、他のサブネット・リソースの削除を並行して要求できますが、サブネットの削除は、リスト照会でそのサブネット内のインスタンスとその他のすべてのリソースが表示されなくなるまで実行できません。

インスタンスのネットワーク・インターフェースをすべてリストするために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

削除するサブネット内に 2 次ネットワーク・インターフェースがある場合は、そのインスタンスを削除する必要があります。インスタンスを削除せずにネットワーク・インターフェースをインスタンスから削除することはできません。

### サブネットを削除する
{: #deleting-subnet-api}

サブネット内のリソースがすべて削除され、リスト照会を実行しても戻されなくなったら、次のコマンドを実行してサブネットを削除します。

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

すぐにサブネットの状況が `deleting` に変わるはずですが、サブネットが削除されてリスト照会で戻されなくなるまでは、数分かかる可能性があります。

## 手順 3: VPC 内のすべてのパブリック・ゲートウェイを削除する (存在する場合)
{: #deleting-pgs-api}

アカウント内のすべてのパブリック・ゲートウェイをリストするために、次のコマンドを実行します。

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

削除する VPC 内のパブリック・ゲートウェイごとに、次のコマンドを実行してパブリック・ゲートウェイを削除します。`$gateway` はパブリック・ゲートウェイの ID です。

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## 手順 4: VPC を削除する
{: #deleting-single-vpc-api}

VPC 内のサブネット、VPN ゲートウェイ、ロード・バランサー、パブリック・ゲートウェイをすべて削除したら、次のコマンドを実行して VPC を削除します。`$vpc` は削除する VPC の ID です。

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

すぐに VPC の状況が `deleting` に変わるはずですが、VPC が削除されてリスト照会で表示されなくなるまでは、数分かかる可能性があります。
