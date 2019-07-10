---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: troubleshoot, tips, limitations, debug, mode, error, bearer, token, API, CLI, endpoint, problem, reboot, 409, status, instance, reset, asynchronous

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
{:troubleshoot: .troubleshoot}

# IBM Cloud VPC のトラブルシューティング
{: #troubleshooting-your-ibm-cloud-vpc}

この資料では、よく見られる問題と役に立つヒントを紹介します。

## CLI の使用時に `TRACE` モードをオンにする
{: #troubleshoot}

CLI の使用時に `TRACE` (デバッグ) モードをオンにするには、CLI コマンドの前に `IBMCLOUD_TRACE=true` を設定します。

例えば次のようにします。

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Using DEBUG mode or `verbose` mode when using the API
{: #troubleshoot}

例えば、「curl」コマンドに「--verbose」を追加し、「X-Request-Id:」値を IBM に送信していただければ、IBM で問題をトラブルシューティングすることができます。特に接続の問題が発生している場合に、「--verbose」フラグは役に立ちます。

また、「-i」フラグを「curl」コマンドに追加して、サポート・チームが要求の応答内のヘッダーを参照できるようにする方法もあります。 このフラグは、サポートへの問い合わせが必要なほとんどの状況で役に立ちます。

## API calls require a Bearer token
{: #troubleshoot}

cURL コマンドで API を使用する場合は、「$iam_token」の内容によっては、許可ヘッダー内に「Bearer」を指定する必要がある場合があります。「Bearer」という語を指定した場合は、ヘッダー内にはもう指定しません。 ほとんどの例では、ヘッダー内に「Bearer」が指定されていることが想定されます。

## Common problems
{: #troubleshoot}

発生する可能性がある問題をいくつか次に示します。
### 許可されていない (401 または 403 エラー)

{: #troubleshoot}

アカウントに VPC に対する権限が与えられていない可能性があります。オンボード済みのアカウントを使用していることを確認してください。

### リソースを作成できない
{: #troubleshoot}

VPC またはその他のリソースを作成できない場合は、アカウントの所有者から正しい [権限](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) を付与されていることを確認してください。
### インスタンスがすべて不明状況になっている
{: #troubleshoot}

アカウントの所有者から、インスタンスを表示および管理するための正しい [権限](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) を付与されていることを確認してください。
### API から応答がない
{: #troubleshoot}

API が JSON を戻さなくなった場合は、IAM トークンの期限が切れていて更新する必要がある可能性があります。再び IBM Cloud にログインするか、「iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')」を実行してトークンを更新してください。

### インスタンスに対する操作を呼び出すと、エラー: 409 競合が発生する
{: #troubleshoot}

いくつかのインスタンス操作は、インスタンスがその操作と競合する状況にあると、呼び出せません。例えば、インスタンスの状況が「stopped」の場合に「reboot」操作を実行しようとすると、システムから 409 エラーが戻されます。

| 状況        | 操作 | 競合     |
| ----------- | ---------- | -------- |
| Running     | start      | 発生     |
| Stopped     | start 以外の操作| 発生     |
| Not running | pause      | 発生     |
| Not running | reboot     | 発生     |
| Not paused  | resume     | 発生     |
| Paused      | resume 以外の操作| 発生     |


### インスタンスが「instance-reboot」要求に応答しない
{: #troubleshoot}

インスタンスが「instance-reboot」要求に応答しない場合は、「instance-reset」要求を試してください。「instance-reboot」はソフトな要求、「instance-reset」はハードな要求と見なすことができます。 「instance-reboot」要求は OS リブート要求をインスタンスに送信しますが、「instance-reset」要求は VSI インスタンスのハード・リセットを実行します。 コンピューターのキーボードで「ctrl-alt-delete」と入力するか、リセット・ボタンや電源ボタンを押すかの違いと見なすこともできます。 「instance-reset」要求は「instance-reboot」要求よりも完了までに時間がかかることに注意してください。

### リソースを削除できない
{: #troubleshoot}

VSI の作成/削除、サブネットの作成/削除などのいくつかの操作は、API を通して非同期で実行されます。そのため、削除対象のリソースをポーリングして、削除されたことを確認してから次に進むことをお勧めします。

この非同期操作のために、システムからリソースが削除されるまでに数分かかることがあります。 速く削除するために、以下の順序で行うことがベスト・プラクティスです。

1. インスタンスを削除する
2. パブリック・ゲートウェイを削除する
3. サブネットを削除する
4. VPC を削除する
具体的な情報については、[VPC の削除方法](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) の説明を参照してください。
