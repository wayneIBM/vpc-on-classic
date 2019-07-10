---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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

# 入門チュートリアル
{: #getting-started}
[comment]: # (リンクされたヘルプ・トピック)


{{site.data.keyword.cloud}} 仮想プライベート・クラウドの使用を開始するには、以下を行います。

1. 仮想プライベート・クラウドを作成します。
2. 1 つ以上のゾーンで仮想プライベート・クラウドに 1 つ以上のサブネットを作成します。
3. サブネット上のリソースがインターネットにアクセスできるようにするには (インターネットからのアクセスの場合も同様)、サブネット上にパブリック・ゲートウェイ (PGW) を作成します。
4. 実行する仮想サーバー・インスタンス (VSI) のプロファイルを選択し、インスタンス化します。
5. インターネットからアクセスする場合は、浮動 IP アドレスを予約し、仮想サーバー・インスタンスに関連付けます。
5. 仮想サーバー・インスタンスにサービスまたはアプリケーションをデプロイします。

## 前提条件

 * **ユーザー許可**: VPC でリソースを作成および管理するための十分な許可がユーザーに付与されていることを確認してください。 必要な許可のリストについては、[VPC ユーザーに必要な許可の付与](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)を参照してください。

 * **SSH 鍵が利用できる状態にある**: 仮想サーバー・インスタンスへの接続には SSH 鍵を使用します。

   * ホーム・ディレクトリーの `.ssh` ディレクトリー内で `id_rsa.pub` というファイル (例: `/Users/<USERNAME>/.ssh/id_rsa.pub`) を探します。 このファイルは `ssh-rsa` で始まり、E メール・アドレスで終わります。

   * SSH 鍵の生成: 公開 SSH 鍵がない場合、または既存の SSH 鍵のパスワードを忘れた場合は、`ssh-keygen` コマンドを実行し、プロンプトの指示に従って新しい鍵を生成します。 例えば、Linux サーバーで SSH 鍵を生成するには、コマンド `ssh-keygen -t rsa -C"user_ID "` を実行します。 このコマンドにより 2 つのファイルが生成されます。 生成された公開鍵は `<your key>.pub` ファイルに含まれています。

## UI、CLI、または REST API の使用

すべての VPC リソースは、UI、CLI、または REST API を使用してプロビジョンおよび管理できます。

IBM Cloud Virtual Private Cloud を初めて利用する場合は、以下のリンクから、IBM Cloud VPC とそのリソースを作成するプロセスを最初から最後まで学習できます。 コンソール UI、コマンド・ライン (CLI)、REST API のどれを使用して開始するかを選択できます。

* ユーザー・インターフェースを使用してアクセスする場合は、[IBM Cloud コンソール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")]( https://{DomainName}/vpc){: new_window} にログインします。 詳細については、[IBM Cloud コンソールを使用した VPC の作成](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console)を参照してください。
* コマンド・ライン・インターフェースを使用するには、[Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli) の例に従ってください。
* 上級者の場合は、[REST API](https://{DomainName}/apidocs/vpc-on-classic) を直接呼び出すと良いでしょう。 [サンプル・コード](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)のチュートリアルに従って、REST API の使用を開始してください。

## 次のステップ
さらに詳しく学習する準備ができたら、**VPC のネットワーキング**、**VPC の VSI**、および **VPC のブロック・ストレージ**について詳述している以下の資料に直接進んでください。

* [**仮想プライベート・クラウドでのネットワーキング**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**仮想プライベート・クラウドの仮想サーバー**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**仮想プライベート・クラウドのブロック・ストレージ**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
