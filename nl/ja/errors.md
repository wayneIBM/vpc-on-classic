---

copyright:

  years: 2018, 2019
lastupdated: "2019-06-11"

keywords: error, message, API, limitations, rias, support

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# IBM Cloud Virtual Private Cloud API のエラー・メッセージ
{: #rias-error-messages}

{{site.data.keyword.cloud}} Virtual Private Cloud API からエラー・メッセージを受信した場合は、メッセージ ID を使用してその問題の解決方法に関する詳細情報を得ることができます。
{:shortdesc}

## account_type_invalid
**メッセージ**: 仮想プライベート・クラウドをプロビジョンするためには従量課金 (PAYG) アカウントが必要です。(You must have a Pay-As-You-Go account to provision a Virtual Private Cloud.)

VPC をプロビジョンするためにはご使用のアカウントが従量課金 (PAYG) プランに加入していなければなりません。アカウントのアップグレード方法について詳しくは、[アカウントのアップグレード](/docs/account?topic=account-accounts#upgrade-lite-account)を参照してください。

## acl_in_use
**メッセージ**: このネットワーク ACL はリソースに付加されているため削除できません。(The network ACL cannot be deleted because it is attached to resources.)

このネットワーク ACL はサブネットまたは VPC に接続されているため削除できません。

サブネットがネットワーク ACL を使用しているかどうか確認するには、`GET /v1/subnets?version=2019-05-31&generation=1` API を使用します。
`ibmcloud is subnets` が同等の CLI コマンドです。サブネットが使用するネットワーク ACL を更新するには、`PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API または `ibmcloud is subnet-update` CLI を使用します。

VPC がネットワーク ACL を使用しているかどうか確認するには、`GET /v1/vpcs?version=2019-05-31&generation=1` API を使用します。`ibmcloud is vpcs` が同等の CLI コマンドです。VPC が使用するデフォルトのネットワーク ACL は、更新することも削除することもできません。デフォルトのネットワーク ACL は、VPC の削除時に自動的に削除されます。

## address_prefix_conflict
**メッセージ**: この CIDR を使用したアドレス接頭部は使用中です。(The address prefix with this CIDR is in use.)

要求されたアドレス接頭部の CIDR は既存のアドレス接頭部と競合します。

VPC のアドレス接頭部のリストを表示するには、`GET /vpcs/{vpc-id}/address_prefixes` API を使用して、要求された CIDR アドレスが別のアドレス接頭部の `cidr` フィールドによって使用されていないことを確認します。
`ibmcloud is vpc-address-prefix` が同等の CLI コマンドです。

## address_prefix_in_use
**メッセージ**: 使用中のアドレス接頭部を削除することはできません。(Cannot delete an address prefix in use.)

1 つ以上のサブネットでこのアドレス接頭部が使用されています。  アドレス接頭部を使用しているサブネットを判別するには、`GET /v1/subnets?version=2019-05-31&generation=1` API コマンドを使用して `ipv4_cidr_block` フィールドをチェックします。同等の CLI コマンドは `ibmcloud is subnets` で、`Subnet CIDR` 列をチェックします。アドレス接頭部を削除するには、その前にアドレス接頭部を使用しているすべてのサブネットを削除する必要があります。

## backend_service_unavailable
**メッセージ**: バックエンド・サービスを使用できません。(The backend service is unavailable.)

VPC が使用しているバックエンド・クラウド・サービスが応答しませんでした。VPC は、以下のような複数の IBM Cloud サービスを使用します。

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [グローバル・カタログ](https://{DomainName}/catalog)
- リソース・コントローラー
- リソース・マネージャー

IBM Cloud サービスの[状況](https://{DomainName}/status)を確認し、数分後に再試行できます。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## bad_field
**メッセージ**: 正しいインスタンス UUID を指定する必要があります。(Correct instance UUID should be provided)

要求に指定された値の 1 つが正しくありません。返されたエラーの `target`値を調べて、どのパラメーターが正しくなかったのかを確認してください。場合によっては、要求内の UUID またはボリュームが見つからないことがあります。有効な値を指定して、再試行してください。

詳しいヘルプについては、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## bad_request
**メッセージ**: 提供された情報は無効であるか、誤った形式であるか、必須フィールドが欠落しています。(The information given was invalid, malformed, or missing a required field.)

要求は予期したものではありませんでした。[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window} を使用して、要求のフォーマットに役立ててください。

仕様に従ってもエラーが解決しない場合は、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## classic_access_vpc_conflict_duplicate_res
**メッセージ**: リージョンごとに作成できる、クラシック・アクセスを備えた VPC は 1 つのみです。(Only one VPC with Classic Access can be created per region.)

リージョンごとに作成できる、クラシック・アクセスを備えた VPC は 1 つのみです。クラシック・アクセスを備えた VPC をリストするには、`ibmcloud is vpcs --classic-access` を実行します。クラシック・アクセスを備えた別の VPC を作成するには、その前にクラシック・アクセスを備えた既存の VPC を削除する必要があります。

## classic_access_vpc_account_not_VRF_enabled
**メッセージ**: クラシック・アクセス VPC を作成するには、アカウントで VRF が有効になっている必要があります。(Account must be VRF-enabled to create a classic access VPC.)

リンクされたクラシック・アカウントで VRF が有効ではありません。クラシック・アクセス VPC では、リンクされたクラシック・アカウントで VRF が有効になっている必要があります。アカウントで VRF を有効にする方法については、[IBM Cloud 上の VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) を参照してください。

## default_address_prefix_not_found
**メッセージ**: デフォルトのアドレス接頭部が見つかりません。(Default address prefix not found.)

このエラー・メッセージが表示される可能性があるのは、デフォルトのアドレス接頭部が見つからない場合です。

## duplicate_error
**メッセージ**: 指定された入力内容は既に存在しています。(The input provided already exists.)

指定されたリソースは既に存在しています。作成するリソースに別の名前を使用してください。例えば、新しい VPC を作成する場合、`ibmcloud is vpcs` を実行して既存の VPC をリストできます。競合しない名前を選択してください。

## floating_ip_in_use
**メッセージ**: この浮動 IP は使用中です。(The floating IP is in use.)

この浮動 IP は、仮想サーバー・インスタンスまたはパブリック・ゲートウェイに関連付けられています。

浮動 IP が使用されている場所を確認するには、`GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` API を使用し、"target" 値を調べます。同等の CLI コマンドは `ibmcloud is floating-ip FLOATINGIP_ID` です。

## gateway_too_many
**メッセージ**: 1 つの VPC 内でゾーンあたり 1 つのパブリック・ゲートウェイのみを保持できます。(Can only have one public gateway per zone in a VPC.)

1 つの VPC 内でゾーンあたり 1 つのパブリック・ゲートウェイのみが許可されていますが、その 1 つのパブリック・ゲートウェイをゾーン内の複数のサブネットに接続できます。 ゾーンのパブリック・ゲートウェイを見つけるには、GET public_gateways API を実行して、「vpc」と「zone」の値を確認してください。 CLI を使用している場合は、`ibmcloud is public-gateways` コマンドを実行して、「VPC」と「Zone」の値を確認できます。

パブリック・ゲートウェイの ID を使用して、パブリック・ゲートウェイをサブネットに接続します。詳しくは、[API の例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-)を参照してください。 CLI を使用している場合は、サブネット更新コマンドを使用してサブネットをパブリック・ゲートウェイに接続できます (例: `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`)。

## health_monitor_invalid_delay
**メッセージ**: 指定されたヘルス・モニター遅延 <health_monitor_delay> は無効です (The specified health monitor delay <health_monitor_delay> is invalid)

パラメーター `health monitor delay` に 2 から 60 までの範囲の値を指定します。

## health_monitor_invalid_max_retries
**メッセージ**: 指定されたヘルス・モニター最大再試行回数 <health_monitor_max_retries> が無効です (The specified health monitor max retries <health_monitor_max_retries> is invalid)

`health max retries` に 1 から 10 までの範囲の値を指定します。

## health_monitor_invalid_timeout
**メッセージ**: 指定されたヘルス・モニター・タイムアウト <health_monitor_timeout> が無効です。(The specified health monitor timeout <health_monitor_timeout> is invalid.)

`health timeout` に 1 から 59 までの範囲の値を指定します。選択する値は、`health monitor delay` の値より小さくなければなりません。

## health_monitor_invalid_url_path
**メッセージ**: 指定されたヘルス・モニター URL <health_monitor_url> が無効です。(The specified health URL path <health_monitor_url> is invalid.)

ヘルス・モニターの有効な URL を指定してください。URL には英数字、ハイフン、ピリオドを含めることができます。空白またはバックスラッシュは使用できません。URL パスの最大長は 255 文字です。

## health_monitor_invalid_protocol
**メッセージ**: 指定されたヘルス・モニター・プロトコル <health_monitor_protocol> が無効です。(The specified health monitor protocol <health_monitor_protocol> is invalid.)

ヘルス・モニター・プロトコルの有効な値を指定してください。使用できる値は `http` および `tcp` です。

## health_monitor_missing_protocol
**メッセージ**: ヘルス・モニター・プロトコルが欠落しています (Health monitor protocol is missing)

ヘルス・モニター・プロトコルは必須フィールドです。要求で、ヘルス・モニター・プロトコルを指定してください。有効な値は、`http` および `tcp` です。

## health_monitor_missing_url_path
**メッセージ**: ヘルス・モニター URL パスが欠落しています。(Health monitor URL path is missing.) HTTP タイプのヘルス・モニターには必須です。(It is required for a HTTP type health monitor.)

HTTP プロトコルを使用するヘルス・モニターの場合、ヘルス・モニター URL パスは必須フィールドです。有効な URL パスを指定してください。

## health_monitor_timeout_delay_conflict
**メッセージ**: ヘルス・モニター・タイムアウト <health_monitor_timeout> は遅延 <health_monitor_delay> 以上にすることはできません。(Health Monitor timeout <health_monitor_timeout> should not be greater than or equal to delay <health_monitor_delay>.)

パラメーター `health monitor delay` の値は `health monitor timeout` の値よりも大きくする必要があります。

## http_request_size_exceeded
**メッセージ**: この HTTP 要求は大きすぎます。(The HTTP request is too large.)

この問題が発生するのは、要求に格納して送信したペイロードに含まれている文字が多すぎる場合です。 ペイロードのサイズを小さくして再試行してください。 例えば、単一の要求ですべての処理を実行しようとする代わりに、1 つ目の要求で最小限のリソースを作成してから、それ以降の複数の要求でそのリソースに状態を段階的に付加してみてください。

詳しいヘルプについては、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## iam_failure
**メッセージ**: なし

認証または許可を確認する IAM サービスで障害が発生しました。IAM サービスの停止は一時的である可能性があります。数分後に要求を再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ike_policies_quota_exceeded
**メッセージ**: ご使用のアカウントが割り当て量に達したため、IKE ポリシーを作成できません。(The IKE policy cannot be created because your account has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

現在の IKE ポリシーを表示するには、`GET /ike_policies` API を使用します。
同等の CLI コマンドは `ibmcloud is ike-policies` です。

## ike_policy_duplicate_name
**メッセージ**: `<ike_policy_name>` という名前は IKE ポリシー `<ike_policy_id>` によって既に使用されています。(The name `<ike_policy_name>` is in use already by IKE policy `<ike_policy_id>`.)

別の IKE ポリシー名を指定します。数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ike_policy_in_use
**メッセージ**: IKE ポリシー `<ike_policy_id>` は 1 つ以上の接続で使用中です。(The IKE policy `<ike_policy_id>` is in use by one or more connections.)

1 つ以上の接続で使用中の IKE ポリシーを削除することはできません。

IKE ポリシーを使用している接続を確認するには、`GET /ike_policies/<ike_policy_id>/connections` API を使用します。
同等の CLI コマンドは `ibmcloud is ike-policy-connections IKE_POLICY_ID` です。

## ike_policy_invalid_name
**メッセージ**: `<ike_policy_name>` という名前は有効な IKE ポリシー名ではありません。(The name `<ike_policy_name>` is not a valid IKE policy name.)

有効な IKE ポリシー名は、英字で始まり、その後ろに英字、数字、下線、またはハイフンが続きます。

## ike_policy_not_created
**メッセージ**: IKE ポリシー `<ike_policy_id>` を作成できませんでした。(The IKE policy `<ike_policy_id>` could not be created.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ike_policy_not_deleted
**メッセージ**: IKE ポリシー `<ike_policy_id>` を削除できませんでした。(The IKE policy `<ike_policy_id>` could not be deleted.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ike_policy_not_found
**メッセージ**: IKE ポリシー `<ike_policy_id>` が見つかりませんでした。(The IKE policy `<ike_policy_id>` could not be found.)

参照対象の IKE ポリシーは存在しません。 要求を見直して、適切な IKE ポリシー ID を指定したことを確認してください。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ike_policy_not_updated
**メッセージ**: IKE ポリシー `<ike_policy_id>` を更新できませんでした。(The IKE policy `<ike_policy_id>` could not be updated.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## instance_delete_conflict
**メッセージ**: 現在の状況ではインスタンスを削除できません。(The instance cannot be deleted in the current status.)

インスタンスが、`deleting`、`pending`、`starting`、`stopping`、`restarting` のいずれかの状況の場合には削除できません。インスタンスは `running` 状況または `stopped` 状況でなければなりません。状況が変わらない場合には、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_invalid_hostname
**メッセージ**: ホスト名が無効なためインスタンスが作成できません。(The instance cannot be created because the hostname is not valid.)

インスタンスのホスト名は以下の要件を満たしていなければなりません。
* ホスト名に使用できるのは英数字とダッシュのみです
* 大文字は使用できません
* 先頭および末尾にダッシュを使用することはできません
* 先頭に数字を使用することはできません
* ダッシュを連続して使用することはできません
* Windows における最大長は 15 文字です
* 他のオペレーティング・システムにおける最大長は 40 文字です

## instance_invalid_port_speed
**メッセージ**: 指定されたネットワーク・インターフェース・ポート速度が無効なため、インスタンスを作成できません。(The instance cannot be created because the specified network interface port speed is not valid.)

ネットワーク・インターフェース・ポート速度は `100` Mbps または `1000` Mbps でなければなりません。

## instance_name_exists
**メッセージ**: 同じインスタンス名が既に存在しています。(The same instance name already exists.)

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## instance_sec_volume_over_quota
**メッセージ**: ボリューム接続数が割り当て量を超えるため、インスタンスを作成できません。(The instance cannot be created because the number of volume attachments would exceed the quota.)

リソースあたりの VPC 割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}ページで指定します。

## instance_too_many_keys
**メッセージ**: 要求に複数の鍵が含まれているため、Windows インスタンスを作成できません。(The Windows instance cannot be created because the request contains multiple keys.)

Windows インスタンスでサポートされるのは 1 つの鍵のみです。

## insufficient_space_for_subnet
**メッセージ**: アドレス接頭部内のサブネットのスペースが不足しています。(Insufficient space for subnet in address prefix.)

要求されたアドレスの数を割り振ることができないため、サブネットを作成できません。アドレス数を減らすか、大きな CIDR を使用して再試行してください。

## internal_error
**メッセージ**: 内部エラーが発生しました。(An internal error occurred.)

予期しないエラーが発生しました。この問題は一時的なものである可能性があります。 数分後に要求を再試行してください。この問題が解決しない場合は、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_server_error
**メッセージ**: なし

予期しないエラーが発生しました。この問題は一時的なものである可能性があります。 数分後に要求を再試行してください。この問題が解決しない場合は、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_solution
**メッセージ**: 管理者にお問い合わせください。(Please contact your administrator.)

予期しないエラーが発生しました。この問題は一時的なものである可能性があります。 数分後に要求を再試行してください。この問題が解決しない場合は、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## invalid_generation_parameter
**メッセージ**: generation 照会パラメーターは 1 に設定する必要があります。(The generation query parameter must be set to 1.)

2019 年 5 月 31 日以降のバージョンでクラシック API 要求において VPC を使用できるようにするには、`generation` 照会パラメーターを 1 に設定する必要があります。

## invalid_id_format
**メッセージ**: 誤った ID 形式です。(Bad ID format.) 形式が正しいことを確認してください。(Ensure format is correct.)

指定した ID に誤った形式のデータが含まれていないことを確認してください。

このエラー・メッセージが表示される可能性があるのは、ページ編集要求を発行する際に誤った形式の開始照会を指定した場合です。例えば、
`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` には `?` が 2 つ含まれています。照会を修正して再試行してください。

## invalid_route
**メッセージ**: 要求された経路が存在しません。(The requested route does not exist.)

指定の API URL に要求された経路が存在しません。API エンドポイントを呼び出すために指定した URL が正しいことを確認してください。

## invalid_request_field
**メッセージ**: 要求で指定したフィールドが無効です。(A field provided in the request is not valid.)

例えば、サブネットで使用するネットワーク ACL を更新するには、`PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API を使用します。
以下の場合、「networkacl」フィールドが有効ではないため、要求が無効です。`PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

使用できる値については、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

## invalid_state
**メッセージ**: 現在の状況のリソースではサポートされていないアクションがリソースで要求されました。(An action was requested on a resource which is not supported at the current status of the resource.)

リソースがインスタンスの場合:

1. 対象インスタンスでリブート操作が進行中である可能性があります。インスタンスの状況に応じて[許可されるアクション](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance)をご確認ください。

2. インスタンスの状況が `pending` の場合、以下のアクションを実行することはできません。
  * インスタンスを削除する
  * ボリュームをインスタンスに接続する
  * ボリュームをインスタンスから切り離す
  * インスタンスを更新する

後でアクションをやり直してください。リソースの状況が変わらない場合、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

リソースがイメージの場合:

インスタンスの状況が `deleting` の場合、以下のアクションを実行することはできません。
  * イメージをパッチする
  * イメージを削除する

要求を再発行しないでください。イメージの削除は所定の時間で完了します。削除が完了すると、対象イメージに対するすべての要求は、代わりにエラー `not_found` で失敗します。

## invalid_version
**メッセージ**: `version` パラメーターが無効です。このパラメーターは `YYYY-MM-DD` という形式である必要があります。(The `version` parameter is invalid, it must be of the form `YYYY-MM-DD`.)

バージョンは _YYYY-MM-DD_ という形式に従っている必要があります。 1 桁の月や日の場合は (1 月 1 日など)、バージョンは `2019-01-01` のように指定される必要があります。 version パラメーターは URL に含まれている必要があります。 version パラメーターで指定される日付は、2019-01-01 より後で、現在の日付より前でなければなりません。

## invalid_version_range
**メッセージ**: `version` の値を将来の日付や `2019-01-01` より前の日付に設定することはできません。(The `version` value cannot be set at a future date nor before `2019-01-01`.)

version パラメーターで指定される日付は、2019-01-01 より後で、現在の日付より前でなければなりません。

## invalid_zone
**メッセージ**: 要求対象のリソースが同じゾーン内にあるかどうかを確認してください。(Please check whether the resources you are requesting are in the same zone.)

ゾーン 1 のパブリック・ゲートウェイをゾーン 2 のサブネットに接続しようとすると、このメッセージが表示されることがあります。

## ipsec_policies_quota_exceeded
**メッセージ**: ご使用のアカウントが割り当て量に達したため、IPsec ポリシーを作成できません。(The IPsec policy cannot be created because your account has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

現在の IPsec ポリシーを表示するには、`GET /ipsec_policies` API を使用します。
同等の CLI コマンドは `ibmcloud is ipsec-policies` です。

## ipsec_policy_duplicate_name
**メッセージ**: `<ipsec_policy_name>` という名前は IPsec ポリシー `<ipsec_policy_id>` で既に使用中です。(The name `<ipsec_policy_name>` is in use already by IPsec policy `<ipsec_policy_id>`.)

別の IPsec ポリシー名を指定します。数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ipsec_policy_in_use
**メッセージ**: IPsec ポリシー `<ipsec_policy_id>` は 1 つ以上の接続で使用中です。(The IPsec policy `<ipsec_policy_id>` is in use by one or more connections.)

1 つ以上の接続で使用中の IPsec ポリシーを削除することはできません。

IPsec ポリシーを使用している接続を確認するには、`GET /ipsec_policies/<ipsec_policy_id>/connections` API を使用します。同等の CLI コマンドは `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID` です。

## ipsec_policy_invalid_name
**メッセージ**: `<ipsec_policy_name>` という名前は有効な IPsec ポリシー名ではありません。(The name `<ipsec_policy_name>` is not a valid IPsec policy name.)

有効な IPsec ポリシー名は、英字で始まり、その後ろに英字、数字、下線、またはハイフンが続きます。

## ipsec_policy_not_created
**メッセージ**: IPsec ポリシー `<ipsec_policy_id>` を作成できませんでした。(The IPsec policy `<ipsec_policy_id>` could not be created.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ipsec_policy_not_deleted
**メッセージ**: IPsec ポリシー `<ipsec_policy_id>` を削除できませんでした。(The IPsec policy `<ipsec_policy_id>` could not be deleted.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## ipsec_policy_not_found
**メッセージ**: IPsec ポリシー `<ipsec_policy_id>` が見つかりませんでした。(The IPsec policy `<ipsec_policy_id>` could not be found.)

参照対象の IPsec ポリシーは存在しません。 要求を確認し、有効な IPsec ポリシー ID を指定してください。ポリシーが確かに存在する場合には、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_updated
**メッセージ**: IPsec ポリシー `<ipsec_policy_id>` を更新できませんでした。(The IPsec policy `<ipsec_policy_id>` could not be updated.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## key_content_exists
**メッセージ**: 同じ鍵コンテンツが既に存在しています。(The same key content already exists.)

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## key_name_exists
**メッセージ**: 同じ鍵名が既に存在します。(The same key name already exists.)

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## key_invalid_name
**メッセージ**: 名前が無効なため、鍵を作成できません。(The key cannot be created because the name is not valid.)

鍵名は、以下の要件を満たしている必要があります。
* 先頭は英字 [A-Za-z] でなければならない
* 使用できるのは英数字、ダッシュ、アンダースコアー [-A-Za-z0-9_] のみ
* 長さは 65 文字以下

## key_invalid_type
**メッセージ**: タイプが無効なため、鍵を作成できません。(The key cannot be created because the type is not valid.)

サポートされる唯一の鍵タイプは `rsa` です。

## key_parse_failure
**メッセージ**: 公開鍵が無効なため、鍵を作成できません。(The key cannot be created because the public key value is not valid.)

要求で指定した公開鍵が無効です。値を確認するか、公開鍵を再生成し、再試行してください。

Linux オペレーティング・システムの場合、`ssh-keygen -l -v -f <key_file>` というコマンドを実行して、有効な公開鍵を生成できます。

## listener_certificate_not_found
**メッセージ**: CRN `<listener_certificate_crn>` を持つ証明書インスタンスが見つからないか、この証明書インスタンスにアクセスするための許可がありません。(Certificate instance with CRN `<listener_certificate_crn>` cannot be found or no permission to access the certificate instance.)

既存の証明書インスタンスの CRN を指定するか、アカウント管理者に依頼して、指定した証明書インスタンスに対するアクセス許可を付与してもらってください。

## listener_duplicate_port
**メッセージ**: ポート `<listener_port>` は別のリスナー・インスタンスで使用されています。(Port `<listener_port>` is used by another listener instance.) 異なるポートを選択してください。

別のポートを選択してください。

## listener_invalid_certificate
**メッセージ**: HTTPS リスナーの証明書インスタンスの CRN が無効です。(CRN of certificate instance for the HTTPS listener is invalid.)

有効な証明書インスタンス CRN を指定してください。

## listener_invalid_connection_limit
**メッセージ**: リスナー接続制限 `<listener_connection_limit>` は無効です。(Listener connection limit `<listener_connection_limit>` in invalid.)

有効な接続制限を指定してください。接続制限は 1 から 15000 までの範囲の整数でなければなりません。

## listener_invalid_port
**メッセージ**: リスナー・ポート `<listener_port>` は無効です。(Listener port `<listener_port>` is invalid.)

別のポートを選択してください。

## listener_invalid_protocol
**メッセージ**: リスナー・プロトコル `<listener_protocol>` は無効です。(Listener protocol `<listener_protocol>` is invalid.)

有効なリスナー・プロトコルを指定してください。有効なリスナー・プロトコル値は、`http`、`https`、および `tcp` です。

## listener_missing_certificate_crn
**メッセージ**: `https` リスナーの証明書インスタンスの CRN が欠落しています。(CRN of the certificate instance for the `https` listener is missing.)

証明書インスタンスの CRN は必須フィールドです。

## listener_missing_port
**メッセージ**: リスナー・ポートが欠落しています。(Listener port is missing.)

リスナー・ポートは必須フィールドです。

## listener_missing_protocol
**メッセージ**: リスナー・プロトコルが欠落しています。(Listener protocol is missing.)

リスナー・プロトコルは必須フィールドです。 要求内でリスナー・プロトコルを指定してください。 有効な値は、`http`、`https`、および `tcp` です。

## listener_not_found
**メッセージ**: ID `<listener_id>` を持つリスナーが見つかりません。(The listener with ID `<listener_id>` cannot be found.)

既存のリスナー ID を指定してください。

## listener_over_quota
**メッセージ**: リスナーを作成できません。(Listener cannot be created.) ロード・バランサー・リソースのリスナーの割り当て量が上限に達しました。(Quota of listeners for the load balancer resource has reached the maximum limit.)

リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}で指定されています。

## listener_pool_protocols_conflict
**メッセージ**: リスナー・プロトコル (`<listener_protocol>`) とプール・プロトコル (`<pool_protocol>`) が競合しています。(Listener protocol(`<listener_protocol>`) and pool protocol(`<pool_protocol>`) are in conflict.)

`https` プロトコルまたは `http` プロトコルを使用したリスナーは、`http` プロトコルを使用したプールのみに関連付けることができます。 同様に、`tcp` リスナーは、`tcp` プールのみに関連付けることができます。

## listener_reserved_port_conflict
**メッセージ**: リスナー・ポート `<listener_port>` は予約済みポートの 1 つです。(Listener port `<listener_port>` is one of the reserved ports.) 56500 から 56520 のポート範囲は管理目的用に予約されています。(The port range of 56500 to 56520 is reserved for management purposes.) 別のポートを選択してください。(Choose a different port.)

異なるポートを選択してください。

## load_balancer_delete_conflict
**メッセージ**: ID <load_balancer_id> のロード・バランサーの状況が以下のいずれかのため、削除できません。(The load balancer with ID <load_balancer_id> cannot be deleted because its status is one of these:)  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

ロード・バランサーの状況が ACTIVE のときに削除してください。

## load_balancer_duplicate_name
**メッセージ**: `<load_balancer_name>` という名前は別のロード・バランサー・インスタンスで使用されています。(Name `<load_balancer_name>` is used by another load balancer instance.) 異なる名前を選択してください。(Please choose a different name.)

異なるロード・バランサー・インスタンス名を指定してください。ロード・バランサー名は VPC とアカウント内で固有でなければなりません。

VPC のロード・バランサーがクラウド・ロード・バランサーと競合することはありません。これらのサービスでは DNS ドメイン・ネームが異なり、VPC のロード・バランサーの名前は DNS 名に含まれないためです。
{: note}

## load_balancer_insufficient_ips
**メッセージ**: ID <subnet_id> のサブネットで使用できる IPv4 アドレスが不十分です。(The subnet with ID(s) <subnet_id> has insufficient available IPv4 addresses.)

要求で、ロード・バランサーを作成する、使用可能な IPv4 アドレスを持つ有効なサブネットを指定してください。

## load_balancer_invalid_description
**メッセージ**: ロード・バランサーの説明 `<load_balancer_description>` が無効です。(Load balancer description `<load_balancer_description>` is not valid.)

ロード・バランサーの説明は 255 文字以下でなければなりません。

## load_balancer_invalid_is_public
**メッセージ**: is_public `<load_balancer_is_public>` の値が無効です。(Value of is_public `<load_balancer_is_public>` is not valid.)

ロード・バランサー・パラメーター `is_public` の値は _true_ または _false_ でなければなりません。

## load_balancer_invalid_name
**メッセージ**: `<load_balancer_name>` という名前は無効です。(Name `<load_balancer_name>` is invalid.)

名前を空にすることはできません。有効なロード・バランサー名は、英字で始まり、英字、数字、または下線が続きます。名前の長さは 40 文字以下にする必要があります。

## load_balancer_invalid_subnet
**メッセージ**: ID <subnet_id> のサブネットが無効です。(The subnet with ID <subnet_id> is not valid.)「available」状況の既存のサブネットを使用してください。(Make sure to use an existing subnet with 'available' status.)

ロード・バランサーを作成する要求を入力するときに、`available` 状況の有効なサブネットを指定してください。

## load_balancer_missing_is_public
**メッセージ**: 「is_public」フィールドが欠落しています。('is_public' field is missing.)

フィールド `is_public` は必須です。`is_public` フィールドの値を指定してください。

## load_balancer_missing_name
**メッセージ**: ロード・バランサー名が欠落しています。(Load balancer name is missing.)

ロード・バランサーの `name` は必須フィールドです。ロード・バランサー名を指定してください。

## load_balancer_missing_subnets
**メッセージ**: ロード・バランサーのサブネットが欠落しています。(Load balancer subnets is missing.)

フィールド `subnets` は必須です。ロード・バランサーを作成する要求で、ロード・バランサーを作成するサブネットに関する情報を指定してください。

## load_balancer_missing_subnet_id
**メッセージ**: サブネット ID が欠落しています。(Subnet ID is missing.)

**「サブネット ID」**は必須フィールドです。コマンドでサブネット ID を指定してください。

## load_balancer_not_found
**メッセージ**: ID `<load_balancer_id>` を持つロード・バランサーが見つかりません。(The load balancer with ID `<load_balancer_id>` cannot be found.)

既存のロード・バランサーの ID を指定してください。

## load_balancer_over_quota
**メッセージ**: ロード・バランサーを作成できません。(Load balancer cannot be created.) このアカウントでのロード・バランサー・インスタンスの割り当て量が上限に達しました。(Quota of load balancer instances under your account has reached maximum limit.)

既存のロード・バランサーを削除するか、サポートに問い合わせてご使用のアカウントでのロード・バランサー割り当て量を増やしてください。

リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}で指定されています。

## load_balancer_subnet_not_found
**メッセージ**: ID <subnet_id> のサブネットが見つかりません。(The subnet with ID <subnet_id> cannot be found.)

要求で、ロード・バランサーを作成する既存のサブネットの ID を指定する必要があります。

## load_balancer_unchanged_update
**メッセージ**: ID `<load_balancer_id>` を持つロード・バランサーの更新内容はありません。(There is nothing to update the load balancer with ID `<load_balancer_id>`.)

指定した ID のロード・バランサー・インスタンスを更新するための情報が提供されていませんでした。

## load_balancer_update_conflict
**メッセージ**: ID <load_balancer_id> のロード・バランサーの状況が以下のいずれかのため、更新できません。(The load balancer with ID <load_balancer_id> cannot be updated because its status is one of these:)
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

ロード・バランサーの状況が ACTIVE のときに更新してください。

## member_duplicate_address_and_port
**メッセージ**: プール内にアドレス <member_address>、ポート <member_port> のメンバーが既に存在します。(Member with address <member_address> and port <member_port> already exists in the pool.)

固有なメンバー・アドレスとポートの組み合わせを要求で指定してください。

## member_invalid_address
**メッセージ**: 指定されたメンバー IP アドレス <member_ip> が無効です。(The specified member IP address <member_ip> is invalid.)

要求で、メンバーの有効な IPv4 CIDR アドレスを指定してください。

## member_invalid_port
**メッセージ**: 指定されたメンバー・ポート `<member_port>` は無効です。(The specified member port `<member_port>` is invalid.)

有効なポート番号を指定してください。有効なポート番号の範囲は 1 から 65535 です。

## member_invalid_weight
**メッセージ**: 指定されたメンバー重みづけ `<member_weight>` は無効です。(The specified member weight `<member_weight>` is invalid.)

有効なメンバー重みづけを指定してください。値は、0 から 100 までの整数でなければなりません。

## member_ip_not_found
**メッセージ**:IP アドレス <member_IP> のメンバーは、ロード・バランサーのリージョンおよび VPC のサブネットに属していません。(Member with IP address <member_IP> does not belong to any subnets in the Region and VPC of the load balancer.)

要求で、ロード・バランサーと同じリージョンおよび VPC にサブネットに属するメンバーのアドレスを指定してください。

## member_missing_address
**メッセージ**: メンバー・アドレスが欠落しています。(Member address is missing.)

メンバー・アドレスは必須フィールドです。 メンバー・アドレスの値を指定してください。

## member_missing_protocol_port
**メッセージ**: メンバー・ポートが欠落しています。(Member port is missing.)

メンバー・ポートは必須フィールドです。 メンバー・ポートの値を指定してください。

## member_not_found
**メッセージ**: ID <member_id> のメンバーが見つかりません。(Member with ID <member_id> cannot be found.)

既存のメンバー ID を指定してください。

## member_over_quota
**メッセージ**: メンバーを作成できません。(Member cannot be created.) このプールでのメンバー・インスタンスの割り当て量が上限に達しました。(Quota of member instances under the pool has reached maximum limit.)

リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}で指定されています。

## missing_generation_parameter
**メッセージ**: generation 照会パラメーターは必須です。(The generation query parameter is required.)

2019 年 5 月 31 日以降のバージョンでクラシック API 要求の VPC では、`generation` 照会パラメーターが必須です。

## missing_ims_account_id
**メッセージ**: なし

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## missing_version
**メッセージ**: `version` パラメーターは必須であり、`YYYY-MM-DD` という形式である必要があります。(The `version` parameter is required, and it must be of the form `YYYY-MM-DD`.)

すべての API 要求で version パラメーターが必須です。バージョンは _YYYY-MM-DD_ という形式に従っている必要があります。 1 桁の月や日の場合は (1 月 1 日など)、バージョンは `2019-01-01` のように指定される必要があります。 version パラメーターで指定される日付は、2019-01-01 より後で、現在の日付より前でなければなりません。version パラメーターの指定方法については、こちらの [API 例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)をご確認ください。

## network_conflict
**メッセージ**: CIDR が VPC 内の既存の接頭部と競合しています。(CIDR conflicts with existing Address Prefix in VPC.)

このメッセージが表示される可能性があるのは、同じ IP 空間内の既存のネットワーク CIDR と競合するネットワーク CIDR を指定した場合です。

VPC のアドレス接頭部にある既存の CIDR を確認するには、API コマンド `GET /v1/vpcs/<vpc-id>/address_prefixes` を実行し、応答の `cidr` 値を調べます。`cidr` の値を調べ、指定した CIDR が他のアドレス接頭部で使用されていないことを確認してください。

CLI を使用している場合、コマンド `ibmcloud is vpc-addrs <vpc-id>` を実行し、`CIDR Block` の値を調べて競合しているかどうかを確認します。

サブネットの既存の CIDR を確認するには、API コマンド `GET /v1/subnets` を実行して VPC のすべてのサブネットをリスト表示します。その後、API コマンド `GET /v1/subnets/<subnet-id>` を VPC 内のサブネットごとに実行し、応答で `ipv4_cidr_block` の値を調べます。`ipv4_cidr_block` の値を調べ、指定した CIDR が他のアドレス接頭部で使用されていないことを確認してください。

CLI を使用している場合、コマンド `ibmcloud is subnets` を実行して VPC のサブネットすべてをリスト表示します。その後、コマンド `ibmcloud is subnet <subnet-id>` を VPC 内のサブネットごとに実行し、CLI 出力の `IPv4 CIDR` の値と競合していないかどうか確認してください。

## not_authorized
**メッセージ**: この要求は許可されていません。(The request is not authorized.)

IAM トークンが欠落または失効している場合に、このエラーが表示されることがあります。トークンの生成方法について詳しくは、[REST API を使用した VPC の作成](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)を参照してください。 トークンが失効していない場合、使用しているアカウントがこの機能の使用を認可されていること、および[適切な許可](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)があることを確認してください。

適切な権限があるものの、エラーが引き続き表示される場合、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_found
**メッセージ**: 要求対象のリソースが存在しているかどうかを確認してください。(Please check whether the resource you are requesting exists.)

参照対象のリソースは存在していないか、自身がアクセス権を持たないリソースです。 要求を見直して、適切な ID と参照を指定したことを確認してください。

詳しいヘルプについては、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## not_implemented
**メッセージ**: なし

このメソッドは実装されていません。要求を調べ、[有効な API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} を呼び出していることを確認してください。この API を実装する必要がある場合、[サポートにお問い合わせください](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_in_address_prefix
**メッセージ**: 指定された CIDR は指定されたゾーン内のどのアドレス接頭部にも適合しません。(The provided CIDR does not fit in any of the address prefixes in the provided zone.)

このエラーが発生するのは、ユーザーが作成しようとしているサブネットの CIDR が、指定されたゾーンのどのアドレス接頭部にも該当しない場合です。

`GET /vpcs/{vpc_id}/address_prefixes` を実行して、VPC のアドレス接頭部のリストを取得します。CLI を使用している場合、`ibmcloud is vpc-address-prefixes` を実行して VPC のアドレス接頭部すべてをリスト表示できます。応答の `cidr` と `zone` の値を参照して、サブネットの `cidr` が、当該サブネットを作成しようとしているゾーンのアドレス接頭部の `cidr` のサブセットであることを確認してください。

## over_quota
**メッセージ**: この要求により、リソース・タイプの割り当て量を超過します。(The request would exceed the quota for a resource type.)

リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}で指定されています。

## password_not_ready
**メッセージ**: なし

パスワードを取得するには、このユーザーのインスタンスが実行されている必要があります。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## pool_delete_conflict
**メッセージ**: プールは、現在も 1 つ以上のリスナーに関連付けられているため削除できません。(Pool cannot be deleted because it is still associated with one or more listeners.)

プールですべてのリスナーとの関連付けが解除されていることを確認し、再試行してください。

## pool_duplicate_name
**メッセージ**: プール名 `<pool_name>` は別のプールで既に使用されています。(Pool name `<pool_name>` is already used by another pool.) 別の名前を選択してください。(Choose a different name.)

別のプール名を選択してください。

## pool_invalid_name
**メッセージ**: 名前 <pool_name> が無効です。(Name <pool_name> is invalid.)

有効なプール名を指定してください。有効なロード・バランサー名は、英字で始まり、その後ろに英字、数字、またはハイフンが続きます。名前の長さは 40 文字以下にする必要があります。

## pool_invalid_session_persistence
**メッセージ**: プール・セッション・パーシスタンス値が無効です。(Pool session persistence value is not valid.)

有効なセッション・パーシスタンス値を指定してください。許可される値は `SOURCE_IP` です。

## pool_missing_algorithm
**メッセージ**: ロード・バランシング・アルゴリズムが欠落しています。(Load balancing algorithm is missing.)

ロード・バランシング・アルゴリズムは必須フィールドです。 要求でロード・バランシング・アルゴリズムを指定してください。有効な値は、`round_robin`、`weighted_round_robin`、`least_connections` です。

## pool_missing_health_monitor
**メッセージ**: プール・ヘルス・モニターが欠落しています。(Pool health monitor is missing.)

プール・ヘルス・モニターは必須フィールドです。

## pool_missing_members
**メッセージ**: プール・メンバーが欠落しています。(Pool members are missing.)

プール・メンバーを指定してください。

## pool_missing_name
**メッセージ**: プール名が欠落しています。(Pool name is missing.)

プール名は必須フィールドです。

## pool_missing_protocol
**メッセージ**: プール・プロトコルが欠落しています。(Pool protocol is missing.)

プール・プロトコルは必須フィールドです。 要求でプール・プロトコルを指定してください。有効な値は、`http` および `tcp` です。

## pool_not_found
**メッセージ**: ID `<pool_id>` を持つプールが見つかりません。(The pool with ID `<pool_id>` cannot be found.)

既存のプール ID を指定してください。

## pool_over_quota
**メッセージ**: プールを作成できません。(Pool cannot be created.) ロード・バランサー・リソースのプールの割り当て量が上限に達しました。(Quota of pools for the load balancer resource has reached maximum limit.)

リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}で指定されています。

## public_gateway_in_use
**メッセージ**: 使用中のパブリック・ゲートウェイを削除することはできません。(Cannot delete a public gateway when it is in use.)

このパブリック・ゲートウェイは現在 1 つ以上のサブネットに接続されています。 パブリック・ゲートウェイを削除するには、そのパブリック・ゲートウェイをすべてのサブネットから切り離す必要があります。

パブリック・ゲートウェイを使用しているサブネットを確認するには、`GET /v1/subnets?version=2019-05-31&generation=1` API を使用します。`ibmcloud is subnets` が同等の CLI コマンドです。パブリック・ゲートウェイをサブネットから切り離すには、API コマンド `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` または CLI コマンド `ibmcloud is subnet-public-gateway-detach` を使用します。

## rate_limit_exceeded
**メッセージ**: 短時間に受信された要求が多すぎます。(Too many requests within a short time.)

このエラー・メッセージが返されるのは、指定された時間内に受信された要求が多すぎる場合です。 しばらく待ってから再試行してください。

## security_group_active_transactions
**メッセージ**: インスタンスがアクティブ状態として表示されるまで、インターフェースを接続することも切り離すこともできません。(The interface cannot be attached or detached until the instance appears in Active state.)

インスタンスのネットワーク・インターフェースをセキュリティー・グループに接続するには、当該インスタンスがアクティブである必要があります。 `GET /v1/instances/{id}?version=2019-05-31&generation=1` または `ibmcloud is instance` を使用して、インスタンスのステータスを確認します。ステータスが `running` になったら、再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## security_group_already_attached
**メッセージ**: このインターフェースはセキュリティー・グループに既に接続されています。(The interface is already attached to the security group.)

このインターフェースはセキュリティー・グループに既に接続されています。 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` または `ibmcloud is security-group-network-interfaces` を使用して、接続されているインターフェースを表示します。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。


## security_group_exists
**メッセージ**: このセキュリティー・グループは既に存在しています。(The security group already exists.)

セキュリティー・グループ名は VPC 内で固有である必要があります。この名前のセキュリティー・グループは、ターゲットの VPC に既に存在しています。`GET /v1/security_groups?version=2019-05-31&generation=1` または `ibmcloud is security-groups` を使用して、現在のセキュリティー・グループを表示します。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)または[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。


## security_group_interfaces_attached
**メッセージ**: インターフェースが接続されている間は、セキュリティー・グループを削除できません。(Cannot delete the security group while interfaces are attached.)


セキュリティー・グループを削除するには、その前にすべてのインターフェースを切り離す必要があります。 `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` または `ibmcloud is security-group-network-interface-remove` を使用して、インターフェースを切り離します。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)または
[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。


## security_group_interfaces_per_sg_exceeded
**メッセージ**: セキュリティー・グループあたりのインターフェースの制限を超えました。(Exceeded limit of interfaces per security group.)

このセキュリティー・グループにインターフェースをもう 1 つ接続すると、セキュリティー・グループあたりのインターフェースの制限を超えます。 リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}で指定されています。現在の方針を検証して、類似するルールが付与されたセキュリティー・グループをもう 1 つ作成することを検討してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)または
[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。


## security_group_last_security_group_is_default
**メッセージ**: デフォルトのセキュリティー・グループが唯一の接続されたセキュリティー・グループである場合は、デフォルトのセキュリティー・グループを削除することはできません。(The default security group cannot be removed when it is the only security group attached.)

ネットワーク・インターフェースは少なくとも 1 つのセキュリティー・グループに接続されている必要があります。
接続先のセキュリティー・グループが指定されていない場合は、ネットワーク・インターフェースは VPC のデフォルト・セキュリティー・グループに接続されます。
インターフェースを異なるセキュリティー・グループに接続してから、当該インターフェースをデフォルトのセキュリティー・グループから切り離すことを再試行してください。
デフォルトのセキュリティー・グループについては、[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## security_group_limit_exceeded
**メッセージ**: セキュリティー・グループの制限を超えました。(Exceeded security group limit.)

新しいセキュリティー・グループを作成しようとしましたが、このユーザーは現在アカウント割り当て量に達しています。 リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}で指定されています。セキュリティー・グループへのインスタンス割り当てに関する方針を検証してください。 多くの場合は、同じセキュリティー・グループに複数のインスタンスを割り当てることで、セキュリティー・グループの総数を減らすことができます。 この方針により、セキュリティー・グループの数を減らして、アカウント割り当て量に達することを回避できます。 まれなケースとして、一般に大きな組織の場合は、割り当て量の増大が必要になります。 その場合は、[サポートに問い合わせて](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)、そのことが可能かどうかをご確認ください。

## security_group_network_interface_not_active
**メッセージ**: このインターフェースはアクティブではないため接続できません。(The interface cannot be attached because it is not active.)

セキュリティー・グループに接続するネットワーク・インターフェースはアクティブである必要があります。 `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` または `ibmcloud is instance-network-interface` を使用して、インターフェースのステータスを表示します。ステータスが「使用可能 (available)」になったら、再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## security_group_not_attached
**メッセージ**: このインターフェースは接続されていません。(The interface is not attached.)

このインターフェースはセキュリティー・グループに接続されていません。 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` または `ibmcloud is security-group-network-interfaces` を使用して、接続されているインターフェースを表示します。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)または[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。


## security_group_not_in_vpc
**メッセージ**: このインターフェースとセキュリティー・グループは異なる VPC 内にあります。(The interface and security group are in different VPCs.)

ネットワーク・インターフェースをセキュリティー・グループに接続するには、当該インスタンスが当該セキュリティー・グループと同じ VPC 内にある必要があります。 `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` または `ibmcloud is security-group` を使用して、セキュリティー・グループの詳細と VPC を表示します。`GET /v1/instances/{id}?version=2019-05-31&generation=1` または `ibmcloud is instance` を使用して、インスタンスの詳細と VPC を表示します。

## security_group_order_bindings
**メッセージ**: 保留中のインスタンス注文があるセキュリティー・グループを削除することはできません。(Cannot delete the security group while it has pending instance orders.)

ネットワーク・インターフェース上で指定されたセキュリティー・グループを使用してインスタンスが作成された場合、そのセキュリティー・グループを削除するには、そのインスタンスとネットワーク・インターフェースはアクティブである必要があります。 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` または `ibmcloud is security-group-network-interfaces` を使用して、接続されているネットワーク・インターフェースとそのステータスを表示します。それらのインターフェースのステータスが「使用可能 (available)」になったら、再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## security_group_port_max_less_than_port_min
**メッセージ**: TCP/UDP の最大ポートは最小ポートを下回ることはできません。(TCP/UDP max port cannot be less than min port.)

最大ポート値を最小ポート値より小さくすることはできません。 最小ポート値より大きい最大ポート値を指定してください。

## security_group_port_range_both_or_neither
**メッセージ**: ポート範囲は未設定である必要があるか、最小ポートと最大ポートの両方が「tcp」と「udp」について設定されている必要があります。(Port range must be unset, or both a minimum and maximum port must be set for 'tcp' and 'udp'.)

「tcp」または「udp」のプロトコルを使用したルールでは、ポート範囲を未設定にすることも (そのルールをすべてのトラフィックに適用するために)、ポート範囲を指定することもできます。 トラフィックを単一ポートに制限するには、最小ポートと最大ポートの両方をそのポート値に設定します。 例えば、次のコマンドはポート 22 上で SSH を許可するためのルールを作成します。`ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`

セキュリティー・グループ・ルールの構成について詳しくは、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)を参照してください。


## security_group_port_range_invalid_protocol
**メッセージ**: ポート範囲は「icmp」のプロトコルを使用して指定されました。(A port range was specified with a protocol of 'icmp'.) ポート範囲は「tcp」または「udp」のプロトコルについてのみ有効です。(A port range is only valid for a protocol of 'tcp' or 'udp'.)

「icmp」プロトコルを使用したルールではタイプとコードが設定されます。 「tcp」または「udp」のプロトコルを使用したルールではポート範囲が設定されます。 セキュリティー・グループ・ルールの構成について詳しくは、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)を参照してください。

## security_group_remote_group_not_in_vpc
**メッセージ**: リモート・グループはこのセキュリティー・グループと同じ VPC 内にありません。(The remote group is not in the same VPC as this security group.)

セキュリティー・グループのルールではリモート・セキュリティー・グループを参照でき、その結果としてこれら 2 つのグループの接続されたインターフェース間でトラフィックが許可されます。 リモート・セキュリティー・グループは同じ VPC 内にある必要があります。 `GET /v1/security_groups?2019-01-01` または `ibmcloud is security-groups` を使用して、現在のセキュリティー・グループとそれらの VPC を表示してください。

この問題を修正する詳しい手順については、[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。


## security_group_remoting_rules
**メッセージ**: リモート処理ルールが付加されている間は、セキュリティー・グループを削除することはできません。(Cannot delete the security group while remoting rules are attached.)

このセキュリティー・グループには、リモート・セキュリティー・グループを参照している 1 つ以上のルールが含まれています。 `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` または `ibmcloud is security-group-rules` を使用して、ルールを表示します。「remote」フィールドでは任意のリモート・セキュリティー・グループを指定します。 リモート処理ルールの削除または編集後に再試行してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)または[セキュリティー・グループの使用に関する資料](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}を参照してください。

## security_group_remoting_rules_per_sg_exceeded
**メッセージ**: セキュリティー・グループあたりのリモート処理ルールの制限を超えました。(Exceeded limit of remoting rules per security group.)

リモート・セキュリティー・グループを参照しているルールをもう 1 つ追加すると、セキュリティー・グループあたりのリモート処理ルールの制限を超えます。 リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}で指定されています。

## security_group_rules_per_sg_exceeded
**メッセージ**: セキュリティー・グループあたりのルールの制限を超えました。(Exceeded limit of rules per security group.)

ルールをもう 1 つ追加すると、セキュリティー・グループあたりのルールの制限を超えます。 リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}で指定されています。現在の方針を検証して、セキュリティー・グループをもう 1 つ作成することを検討してください。

## security_group_sgs_per_interface_exceeded
**メッセージ**: インターフェースあたりのセキュリティー・グループの制限を超えました。(Exceeded limit of security groups per interface.)

このインターフェースを別のセキュリティー・グループに接続すると、インターフェースあたりのセキュリティー・グループの制限を超えます。 リソースあたりの割り当て量については、[VPC の割り当て量と制限](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}で指定されています。現在の方針を検証して、既存のセキュリティー・グループにルールを追加することを検討してください。


## security_group_type_code_invalid_protocol
**メッセージ**: 「icmp」のタイプ/コードが指定されましたが、要求されたプロトコルは「icmp」ではありませんでした。(An 'icmp' type/code was given, but the requested protocol was not 'icmp'.) プロトコルを「icmp」に設定するか、ポート範囲を指定してください。(Set the protocol to 'icmp' or specify a port range.)

「icmp」プロトコルを使用したルールのみで、タイプとコードが設定されます。 「tcp」または「udp」のプロトコルを使用したルールではポート範囲が設定されます。 セキュリティー・グループ・ルールの構成について詳しくは、[API 資料](https://{DomainName}/apidocs/vpc-on-classic)を参照してください。

## security_group_vpc_default
**メッセージ**: VPC のデフォルト・セキュリティー・グループを削除することはできません。(Cannot delete the default security group for a VPC.)

デフォルト・セキュリティー・グループを削除することはできません。 デフォルト・セキュリティー・グループについては、[デフォルト・セキュリティー・グループの更新](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group)に関するガイドを参照してください。

## service_manager_service_failure
**メッセージ**: なし

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_conflict
**メッセージ**: CIDR が VPC 内の既存のサブネットと競合しています。(CIDR conflicts with existing Subnet in VPC.)

`GET /subnets` API を実行して VPC 内のすべてのサブネットを取得します。`ipv4_cidr_block` の値を調べ、指定した CIDR が他のサブネットで使用されていないことを確認してください。

CLI を使用している場合は、`ibmcloud is subnets` を実行して「Subnet CIDR」の値を参照することで、競合が発生しているかどうかを確認できます。

詳しいヘルプについては、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_not_empty
**メッセージ**: 空でない限り、サブネットを削除することはできません。(Cannot delete the subnet until it is empty.) サブネット内のリソースを削除してから再試行してください。(Delete any resources in the subnet and retry.)

サブネットの削除が要求されましたが、このサブネット内にはリソースがまだ存在しています。 サブネットには、インスタンス、VPN、ロード・バランサー、またはパブリック・ゲートウェイを保持できます。 サブネットを削除するには、その前に対象サブネット内のリソースを削除する必要があります。

状況によっては、コンソールに表示されている VSI とロード・バランサーの数が 0 であっても、このエラーが発生する可能性があります。削除操作は非同期的で、内部ステータスが変化するまで数分かかることがあります。数分後にサブネットの削除を再試行してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_not_empty_pgway_exists
**メッセージ**: パブリック・ゲートウェイに接続されているサブネットを削除することはできません。(Cannot delete the subnet while it is attached to a public gateway.) パブリック・ゲートウェイを切り離してから再試行してください。(Detach the public gateway and retry.)

サブネットの削除が要求されましたが、このサブネットにはパブリック・ゲートウェイがまだ接続されています。 サブネットを削除するには、その前に対象パブリック・ゲートウェイを削除するか切り離す必要があります。

CLI を使用している場合は、`ibmcloud is public-gateways` を実行してパブリック・ゲートウェイを一覧表示できるとともに、`ibmcloud is subnet-public-gateway-detach` を実行してパブリック・ゲートウェイをサブネットから切り離すことができます。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_not_empty_ipaddr_exists
**メッセージ**: IP アドレスが含まれているサブネットを削除することはできません。(Cannot delete the subnet while it contains IP addresses.) この IP アドレスに関連付けられたサーバー・インスタンスを削除してから、再試行してください。(Please delete any server instance associated with the IP address and retry.)

サブネットの削除が要求されましたが、このサブネットには IP アドレスがまだ含まれています。 サブネットを削除するには、その前に対象 IP アドレスに関連付けられたサーバー・インスタンスを削除する必要があります。

CLI を使用している場合は、`ibmcloud is instances` を実行してサーバー・インスタンスをリスト表示できます。「Address」値を調べてその IP アドレスを判別し、「Status」を調べてステータスを確認します。`ibmcloud is instance-delete` を実行して、ステータスがまだ「deleting」ではないサーバー・インスタンスを削除します。

状況によっては、コンソールに表示されている VSI の数が 0 であっても、このエラーが発生する可能性があります。削除操作は非同期的で、内部ステータスが変化するまで数分かかることがあります。数分後にサブネットの削除を再試行してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_not_empty_loadbalancer_exists
**メッセージ**: ロード・バランサーが含まれているサブネットを削除することはできません。(Cannot delete the subnet while it contains a load balancer.) ロード・バランサーを削除してから再試行してください。(Delete the load balancer and retry.)

サブネットの削除が要求されましたが、このサブネットにはロード・バランサーがまだ含まれています。 サブネットを削除するには、その前に対象ロード・バランサーを削除する必要があります。

CLI を使用している場合、`ibmcloud is load-balancers` を実行してロード・バランサーをリスト表示できます。「Subnets」値を調べサブネットを判別し、「Status」を調べてそのステータスを判別します。`ibmcloud is load-balancer-delete` を実行して、ステータスがまだ「deleting」ではないロード・バランサーを削除します。

状況によっては、コンソールに表示されているロード・バランサーの数が 0 であっても、このエラーが発生する可能性があります。削除操作は非同期的で、内部ステータスが変化するまで数分かかることがあります。数分後にサブネットの削除を再試行してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_not_empty_vpn_gway_exists
**メッセージ**: VPN ゲートウェイが含まれているサブネットを削除することはできません。(Cannot delete the subnet while it contains a VPN gateway.) VPN ゲートウェイを削除してから再試行してください。(Delete the VPN gateway and retry.)

サブネットの削除が要求されましたが、このサブネットには VPN ゲートウェイがまだ接続されています。 サブネットを削除するには、その前に対象 VPN ゲートウェイを削除する必要があります。

CLI を使用している場合、`ibmcloud is vpn-gateways` を実行して VPN ゲートウェイをリスト表示できます。「Subnets」値を調べサブネットを判別し、「Status」を調べてそのステータスを判別します。`ibmcloud is vpn-gateway-delete` を実行して、ステータスがまだ「deleting」ではない VPN ゲートウェイを削除します。

状況によっては、コンソールに表示されている VPN ゲートウェイの数が 0 であっても、このエラーが発生する可能性があります。削除操作は非同期的で、内部ステータスが変化するまで数分かかることがあります。数分後にサブネットの削除を再試行してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## subnet_unknown_state
**メッセージ**: サブネット `<subnet_id>` が要求された操作に関して無効な状態です。(The subnet `<subnet_id>` is in an invalid state for the requested operation.)

サブネットを操作するには、サブネットのステータスが `available` でなければなりません。数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## target_in_use
**メッセージ**: このターゲットには浮動 IP が既に割り当てられています。(The target already has floating IP attached.)

浮動 IP アドレスをサーバーのネットワーク・インターフェースに接続する要求がありましたが、ネットワーク・インターフェースは既に浮動 IP が接続されています。

現在ネットワーク・インターフェースに接続されている浮動 IP アドレスを確認するには、API コマンド `GET /v1/floating_ips?version=2019-05-31&generation=1` を実行し、`target.id` フィールドのネットワーク・インターフェース ID を探します。  

CLI を使用している場合、コマンド `ibmcloud is floating-ips` を実行し、`Target` 値を調べます。CLI では、ネットワーク・インターフェース ID は最初のダッシュ (“-“) 文字で切り捨てられることに注意してください。例えば、サーバーのプライマリー・ネットワーク・インターフェースの ID が `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` の場合、CLI 出力では `primary(abdfcb29-.)` と表示されます。

## token_invalid
**メッセージ**: このサービス・トークンは失効しているか無効です。(The service token was expired or invalid.)

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## token_missing
**メッセージ**: このサービス・トークンは空であるか要求内に存在していません。(The service token was empty or did not exist in the request.)

トークンを再作成してから再試行してください。

## validation_enum
**メッセージ**: 指定された値は有効な選択肢ではありません。(The value supplied was not a valid option.)

指定されたフィールドの 1 つには、指定可能な値の列挙体から得られる必要がある値が含まれています。

ネットワーク ACL ルールについては、次のように方向を指定できます。

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

例えば、`northbound` は列挙体 `[ inbound, outbound ]` 内の有効な選択肢ではないため、次の値は無効になります。

```json
{
    "direction":"northbound"
}
```

有効な値については、[API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}または [CLI リファレンス](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}を参照してください。

## validation_failure
**メッセージ**: 指定された JSON は想定された構造に一致しませんでした。(The JSON provided did not match the expected structure.)

この問題を修正するには、要求の内容が有効な JSON であること、および要求が [API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に従っていることを確認してください。

## validation_invalid_cidr
**メッセージ**: この値は有効な CIDR ではありません。(The value is not a valid CIDR.)

この値は、0 というホスト部を持つ有効な内部 CIDR ブロックである必要があります。

特定の IP アドレス範囲は予約されています。 予約済みの IP 範囲について詳しくは、[リージョンおよびサブネットと組み合わせた VPC の使用](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}の概要を参照してください。

## validation_invalid_ipv4_cidr
**メッセージ**: この値は有効な IPv4 CIDR ではありません。(The value is not a valid IPv4 CIDR.)

この値は、0 というホスト部を持つ IPv4 内部 CIDR ブロックである必要があります。

特定の IP アドレス範囲は予約されています。 予約済みの IP 範囲について詳しくは、[リージョンおよびサブネットと組み合わせた VPC の使用](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}の概要を参照してください。

## validation_invalid_ipv6_cidr
**メッセージ**: この値は有効な IPv6 CIDR ではありません。(The value is not a valid IPv6 CIDR.)

現在、IPv6 はサポートされていません。 IPv4 アドレスを使用してください。

## validation_invalid_address
**メッセージ**: この値は有効なアドレスではありません。(The value is not a valid address.)

有効な IP アドレスでなければなりません。

個別に予約されている IP アドレスのリストについては、[リージョンおよびサブネット](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)に関する資料を参照してください。

## validation_invalid_ipv4_address
**メッセージ**: この値は有効な IPv4 アドレスではありません。(The value is not a valid IPv4 address.)

有効な IPv4 アドレスを指定してください。

## validation_invalid_ipv6_address
**メッセージ**: この値は有効な IPv6 アドレスではありません。(The value is not a valid IPv6 address.)

有効な IPv6 アドレスを指定してください。 現在 IPv6 はサポートされていないため、IPv4 アドレスを使用してください。

## validation_invalid_field_type
**メッセージ**: 値タイプがフィールド・タイプと一致しません。(The value type does not match the field type.)

この問題を修正するには、要求の内容が呼び出し先エンドポイントの [API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に従っていることを確認してください。

## validation_max_value
**メッセージ**: パラメーターに指定した値が許容値を超えています。(A value provided for a parameter is larger than allowed.)

[仕様](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に示されている最大値より小さい値を指定してください。

## validation_min_value
**メッセージ**: パラメーターに指定した値が許容値未満です。(A value provided for a parameter is smaller than allowed.)

`total_ipv4_address_count` が 8 未満のサブネットを作成しようとすると、このエラーが表示されることがあります。

[仕様](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に示されている最小値より大きい値を指定してください。

## validation_not_null
**メッセージ**: 指定されたフィールドはヌルである必要があります。(The field supplied must be null.)

この値がヌルであることを確認してください。 [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

次に無効な例を示します。

```json
{
    "name": null
}
```

## validation_only_one
**メッセージ**: この値はいずれかのサブスキーマに一致する必要があります。(The value must match one of the subschemas.)

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。

## validation_discriminator_forbidden
**メッセージ**: 弁別子フィールドではこの副構造は禁止されています。(The discriminator field forbids this substructure.)

例えば、次のように指定したとします。
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

プロトコルは `icmp` であり、_port_min_ は `tcp` フィールドであるため、エラーが発生します。
1. 欠落している `icmp` ルールについては validation_discriminator_required
2. `icmp` が指定された `tcp` フィールドについては validation_discriminator_forbidden

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_internal_error
**メッセージ**: なし

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_invalid_name
**メッセージ**: この値は有効な名前ではありません。(The value is not a valid name.)

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_invalid_ref
**メッセージ**: この値は有効な HREF ではありません。(The value is not a valid HREF.)

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_non_empty_uuid
**メッセージ**: なし

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_required_field
**メッセージ**: 必須フィールドが欠落しています。(Missing a required field.)

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## validation_unique_failed
**メッセージ**: 指定されたフィールドは固有である必要があります。(The field supplied must be unique.)

指定した名前は既に使用されている可能性があります。 異なる値を指定してみてください。

要求が [API の資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}に準拠していることを確認してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## volume_attachment_delete_invalid_request
**メッセージ**: ボリューム接続を削除することはできません。(The volume attachment cannot be deleted.)

この操作は許可されていません。インスタンスでブート・ボリュームが必要なため、ブート・ボリュームのボリューム接続は削除できません。

## volume_attachment_update_invalid_request
**メッセージ**: 要求が無効なため、ボリューム接続を更新できませんでした。(The volume attachment could not be updated because the request is invalid.)

以下のいずれかの理由により、ボリューム接続を更新できませんでした。

* ブート・ボリュームのボリューム接続を名前変更できない
* ブート・ボリュームのボリューム接続 `delete_volume_on_instance_delete` フィールドを `true` に設定できない

## volume_capacity_max
**メッセージ**: ボリューム容量の最大許容値は 2000 GB です。(The maximum volume capacity allowed is 2000 GB.)

指定したボリューム容量が最大ボリューム容量を超えています。10 GB から 2000 GB までの範囲の値を指定します。カスタム・プロファイルで可能な容量範囲については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) の資料を参照してください。

## volume_capacity_missing
**メッセージ**: 要求に、必要なボリューム容量値が欠落しています。(The required volume capacity value is missing in the request.)

ボリュームを作成するときには、このプロファイル定義に基づいてボリューム容量を指定する必要があります。詳細については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) の資料を参照してください。

## volume_capacity_zero_or_negative
**メッセージ**: ボリューム容量はゼロより大きくする必要があります。(The volume capacity should be greater than zero.)

ボリュームを作成する場合、要求で指定する容量値は 10 GB から 2000 GB までの範囲の正数でなければなりません。サポートされているボリューム容量値については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) の資料を参照してください。

## volume_crn_account_id_mismatch
**メッセージ**: 要求で指定した CRN が現行ユーザーのアカウントに属していません。(The CRN specified in the request does not belong to current user's account.)

鍵の保護ルート鍵 CRN が IAM 許可アカウント ID と一致しません。IAM アカウント用の別の鍵の保護ルート鍵 CRN を指定してください。詳しくは、[鍵の保護](/docs/services/key-protect?topic=key-protect-getting-started-tutorial)資料を参照してください。

## volume_crn_cname_mismatch
**メッセージ**: 要求で指定した CRN を現在の環境に使用することはできません。(The CRN specified in the request cannot be used for the current environment.)

要求で指定した CRN を現在の環境で使用することはできません。ご使用の環境に適用可能な CRN を指定してください。

## volume_encryption_key_crn_invalid
**メッセージ**: 要求で指定したボリューム暗号化鍵 CRN が無効です。(The volume encryption key CRN specified in the request is not valid.)

指定した鍵の保護 CRN が無効です。Cloud Identity and Access Management サービスで、ルート鍵の CRN を取得してください。詳細については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption) の資料を参照してください。

## volume_encryption_key_not_active
**メッセージ**: 要求で指定したボリューム暗号鍵が非アクティブです。(The volume encryption key specified in the request is not active.)

既存の鍵をアクティブ化するか、新しい鍵を指定してください。独自の鍵をアクティブ化できない場合、システム管理者に連絡するか、[お客様サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)にお問い合わせください。

## volume_encryption_key_not_authorized
**メッセージ**: 指定したボリューム暗号鍵へのアクセスが許可されていません。(You are not authorized to access the provided volume encryption key.)

アクセス権のある別の暗号鍵を指定するか、システム管理者に連絡して現在の鍵に対するアクセス権を取得してください。

## volume_encryption_key_not_implemented
ボリューム暗号鍵機能がサポートされていません。

暗号鍵を使用してボリュームを作成できませんでした。このバージョンでサポートされているのは、プロバイダー管理対象の暗号化を使用したボリュームのみです。独自の暗号鍵を使用するには、カスタマー管理対象の暗号化をサポートする、Block Storage for VPC バージョンを使用します。詳しくは、[お客様サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)にお問い合わせください。

## volume_id_invalid
**メッセージ**: 要求パラメーターで指定したボリューム ID が無効です。(The volume ID specified in the request parameter is not valid.)

指定したボリューム ID の形式が正しくありません。入力したボリューム ID が正しいことを確認し、再試行してください。ボリューム ID が不明な場合、CLI で `ibmcloud is volumes` を指定するか、API で `GET volumes` を指定して、ボリュームのリストを検索します。

## volume_id_missing
**メッセージ**: 要求パラメーターにボリューム ID が欠落しています。(The volume ID is missing in the request parameter.)

特定のボリュームを取得する場合、要求パラメーターでボリューム ID を指定する必要があります。

## volume_id_not_found
**メッセージ**: ボリュームが見つかりません。(Volume not found.) ボリューム ID `<volume_id>` が存在しません。`<volume_id>` は指定したボリューム ID です。(Volume ID `<volume_id>` does not exist, where `<volume_id>` is the provided volume ID.)

指定したボリューム ID が存在しません。有効なボリューム ID を指定してください。

## volume_iops_zero_or_negative
**メッセージ**: ボリューム IOPS はゼロより大きくなければなりません。(The volume IOPS should be greater than zero.)

ボリュームを作成する場合、要求で指定する IOPS 値は正数でなければなりません。サポートされている IOPS 値については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) の資料を参照してください。

## volume_name_duplicate
**メッセージ**: ボリューム名が重複しています。(The volume name is a duplicate.) 要求で指定したボリューム名 `<volume_name>` は既に存在します。`<volume_name>` は指定したボリューム名です。(Volume name `<volume_name>` provided in the request already exists, where `<volume_name>` is the provided volume name.)

要求で指定したボリューム名は既に存在します。使用中ではないボリューム名を指定してください。

## volume_not_available
**メッセージ**: ボリュームが使用不可です。(The Volume is not available.) 変更できるのは、available ステータスのボリュームのみです。(Volume can only be modified in available status.) 現行ボリューム `<volume_id>` のステータスは `<volume_status>` です。`<volume_id>` は要求で指定したボリューム ID、`<volume_status>` は現在のボリューム・ステータスです。(Current volume `<volume_id>` status is `<volume_status>`, where `<volume_id>` is the volume ID provided in the request and `<volume_status>` is the current volume status.)

ボリュームを変更できるのは、`available` ステータスの場合のみです。ボリュームを変更する前に available であることを確認してください。

## volume_not_deletable
**メッセージ**: 削除失敗。

ボリュームを削除できるのは、ステータスが `available` または `failed` の場合のみです。ボリュームが、削除を行えるステータスであることを確認してください。

## volume_not_found
**メッセージ**: 指定した ID のボリュームが見つかりません。(A volume with the specified ID is not found.)

入力したボリューム ID が正しいことを確認し、再試行してください。ボリュームのリストを表示するには、`ibmcloud is volumes` (CLI) または `GET volumes` (API) を指定します。

## volume_not_implemented
**メッセージ**: このリリースでは要求された操作はサポートされていません。(The requested operation is not supported in this release.)

詳しくは、[お客様サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)にお問い合わせください。

## volume_profile_capacity_iops_invalid
**メッセージ**: 要求で指定したボリューム・プロファイルが、指定の容量または IOPS、あるいはその両方で無効です。(The volume profile specified in the request is not valid for the provided capacity and/or IOPS.)

要求で指定したボリューム容量または IOPS 値、あるいはその両方が、ボリューム・プロファイルで許可されていません。カスタム・プロファイルで有効な容量と IOPS の最大値と最小値については、[IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) の資料を参照してください。

## volume_profile_iops_invalid
**メッセージ**: 要求で指定したボリューム・プロファイルはカスタム IOPS を受け入れません。(The volume profile specified in the request cannot accept custom IOPS.)

ご使用のボリューム・プロファイルはカスタム IOPS 値を受け入れません。IOPS 値を指定する必要のない、層化 IOPS プロファイルを指定した可能性があります。特定の IOPS 値を指定する必要がある場合、カスタム IOPS プロファイルを使用してください。

## volume_profile_name_missing
**メッセージ**: 要求に、必要なボリューム・プロファイル名が欠落しています。(The required volume profile name is missing in the request.)

ボリューム・プロファイル名が、ボリュームの作成時の要求本体で欠落しているか、ボリュームの取得時の要求パラメーターで欠落しています。要求でボリューム・プロファイル名を指定して、再試行してください。ボリューム・プロファイル名が分からない場合、CLI で `ibmcloud is volume-profiles` を指定するか、API 要求で `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` を使用して、リストを検索します。

## volume_profile_not_found
**メッセージ**: 指定の名前のボリューム・プロファイルが見つかりません。(A volume profile with the specified name is not found.)

その名前のボリューム・プロファイルが見つかりませんでした。指定したボリューム・プロファイル名が正しいことを確認してください。ボリューム・プロファイル名が分からない場合、CLI で `ibmcloud is volume-profiles` を指定するか、API 要求で `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` を使用して、リストを検索します。

## volume_quota_reached
**メッセージ**: ボリューム割り当て量に達しました。(The volume quota has been reached.)

現行アカウントの割り当て量がなくなりました。一部のボリュームを削除するか、[お客様サポート](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)に連絡して割り当て量を増やしてください。[仮想プライベート・クラウド・インフラストラクチャーの割り当て量](/docs/vpc-on-classic?topic=vpc-on-classic-quotas)についての資料を参照してください。

## volume_resource_group_id_invalid
**メッセージ**: 要求で指定したリソース・グループ ID が無効です。(The resource group ID specified in the request is not valid.)

要求で指定したリソース・グループ ID が無効です。リソース・グループ ID が正しいことを確認してください。CLI から `ibmcloud is volume VOLUME_ID` コマンドを使用します。結果の情報には、リソース・グループとリソース・グループ ID が含まれます。詳しくは、[IBM Cloud CLI for VPC リファレンス](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)を参照してください。

## volume_resource_group_not_authorized
**メッセージ**: 指定したリソース・グループでは現在のアクションは許可されていません。(The current action is not authorized on the specified resource group.)

指定したリソース・グループでボリュームをリスト表示または作成することは許可されていません。許可を持つ別のリソース・グループを使用するか、対象リソース・グループでリスト表示および作成の特権の許可を管理者に要求してください。

## volume_resource_group_not_found
**メッセージ**: 指定の ID のリソース・グループが見つかりませんでした。(A resource group with the specified ID could not be found.)

入力内容が間違っているか、存在しないため、リソース・グループ ID が見つかりませんでした。リソース・グループ ID が正しいことを確認してください。CLI から `ibmcloud is volume VOLUME_ID` コマンドを使用します。結果の情報には、リソース・グループとリソース・グループ ID が含まれます。詳しくは、[IBM Cloud CLI for VPC リファレンス](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)を参照してください。

## volume_start_not_found
**メッセージ**: ページ開始パラメーターとして指定した ID のボリュームが見つかりません。(A volume with the ID specified as the page start parameter is not found.)

ボリュームのリスト作成呼び出しの開始パラメーターで指定したボリューム ID が見つかりませんでした。ボリューム ID が正しいこと、および対象ボリュームへのアクセス権を持っていることを確認してください。

## volume_status_not_available
**メッセージ**: 指定した ID のボリュームが使用不可です。(The volume with the specified ID is not available.)

ボリュームの状態が Available ではありません。ステータスが Pending 状態の可能性があります。ボリュームが available になるまで待ってから、再試行してください。ボリュームが削除中の場合、別のボリュームを使用します。ボリュームのステータスを調べるには、`ibmcloud is volume VOLUME_ID` (CLI) または `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` (API 要求) を指定します。

## volume_still_attached
**メッセージ**: ボリュームがまだインスタンスに接続されています。(The volume is still attached to an instance.)

VSI に接続しているボリュームを削除することはできません。ボリュームに自動削除を設定している場合、VSI が削除され、ボリュームが切り離されて削除されるのを待ちます。自動削除を設定していない場合、ボリュームを手動で切り離してください。

## volume_template_invalid
**メッセージ**: 無効なボリューム・テンプレートが指定されました。(Invalid volume template provided.)

ボリュームを作成するために無効なボリューム・テンプレートが指定されました。[VPC API リファレンス](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照して、要求本文に正しいパラメーターが指定されていることを確認してください。

## volume_update_invalid_request
**メッセージ**: ボリューム更新要求が無効です。(The volume update request is not valid.)

指定した更新要求本体プロパティーが無効です。正しい API 要求構文については、[VPC API リファレンス](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

## vpc_not_empty
**メッセージ**: この VPC は空ではないため削除できません。(The VPC cannot be deleted because it is not empty.)

VPC を削除するには、その前に VPC 内のすべてのリソースを削除しておく必要があります。VPC には、インスタンス、サブネット、パブリック・ゲートウェイ、ロード・バランサー、VPN ゲートウェイが含まれ、それらのリソースの一部にもリソースが含まれます。詳しくは、[VPC の削除方法](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)に関する資料を参照してください。

この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpc_resource_separation
**メッセージ**: これらのリソースは異なる VPC 内にあります。(The resources are in different VPCs.)

リソースは同じ VPC 内になければなりません。例えば、パブリック・ゲートウェイをサブネットに接続しようとしている場合、パブリック・ゲートウェイはサブネットと同じ VPC 内にある必要があります。

パブリック・ゲートウェイが含まれる VPC を判別するには、`GET /public_gateways/{id}` コマンドを実行して「vpc」値を探します。同等の CLI コマンドは `ibmcloud is public-gateway <id>` です。

## vpn_connection_cidr_duplicated
**メッセージ**: `<cidr_type>` CIDR ブロックに重複した CIDR ブロック `<cidr_blocks>` が含まれています。(The `<cidr_type>` CIDR blocks have duplicated CIDR blocks `<cidr_blocks>`.)

接続の作成時に、重複した CIDR ブロックが指定されました。CIDR ブロックが固有であることを確認してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_cidr_not_created
**メッセージ**: VPN 接続 `<vpn_connection_id>` に CIDR ブロックを追加できません。(A CIDR block could not be added to the VPN connection `<vpn_connection_id>`.)

指定された要件を満たす有効な CIDR を指定してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_cidr_not_deleted
**メッセージ**: VPN 接続 `<vpn_connection_id>` から CIDR ブロックを削除できませんでした。(A CIDR block could not be deleted from the VPN connection `<vpn_connection_id>`.)

この接続上に存在する有効な CIDR を指定してください。どの接続も、少なくとも 1 つのローカル CIDR とピア CIDR を有している必要があります。

接続の CIDR ブロックを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`local_cidrs` フィールドと `peer_cidrs` フィールドを確認してください。
同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_cidr_not_found
**メッセージ**: VPN 接続 `<vpn_connection_id>` 内でこの CIDR ブロックが見つかりませんでした。(The CIDR block was not found in the VPN connection `<vpn_connection_id>`.)

この接続内にある有効な CIDR を指定してください。

接続の CIDR ブロックを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`local_cidrs` フィールドと `peer_cidrs` フィールドを確認してください。
同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_cidr_not_updated
**メッセージ**: VPN 接続 `<vpn_connection_id>` で CIDR ブロックを更新できませんでした。(The CIDR block could not be updated in the VPN connection `<vpn_connection_id>`.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_cidr_not_valid
**メッセージ**: CIDR ブロック `<cidr_block>` は有効なアドレスを表していません。(The CIDR block `<cidr_block>` does not represent a valid address.)

この値は、ホスト・ビットが設定されていない有効な内部 CIDR である必要があります。

接続の CIDR ブロックを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`local_cidrs` フィールドと `peer_cidrs` フィールドを確認してください。
同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_cidr_overlap
**メッセージ**: CIDR ブロック `<cidr_block_1>` は `<cidr_block_2>` と重なり合っています。(The CIDR block `<cidr_block_1>` overlaps with `<cidr_block_2>`.) 2 つのピア CIDR ブロックが同じ VPC 上で重なり合うことはできず、2 つのローカル CIDR ブロックが同じ接続上で重なり合うことはできません。(Two peer CIDR blocks cannot overlap on the same VPC and two local CIDR blocks cannot overlap on the same connection.)

指定された要件を満たす有効な CIDR を指定してください。

接続の CIDR ブロックを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`local_cidrs` フィールドと `peer_cidrs` フィールドを確認してください。
同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_duplicate_name
**メッセージ**: `<vpn_connection_name>` という名前は VPN 接続 `<vpn_connection_id>` で既に使用されています。(The name `<vpn_connection_name>` is already in use by VPN connection `<vpn_connection_id>`.)

別の接続名を指定してください。数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_invalid_name
**メッセージ**: `<vpn_connection_name>` という名前は有効な VPN 接続名ではありません。(The name `<vpn_connection_name>` is not a valid VPN connection name.)

有効な接続名は、英字で始まり、その後ろに英字、数字、下線、またはハイフンが続きます。

## vpn_connection_invalid_psk_format
**メッセージ**: 事前共有鍵 `<psk>` は有効な形式ではありません。(The pre-shared key (PSK) `<psk>` is not in the valid format.)

有効な PSK は 6 文字から 128 文字で構成されている必要があり、これらの文字は英字、数字、および次の記号である必要があります。`-`、`+`、`&`、`!`、`@`、`#`、`$`、`%`、`^`、`*`、`(`、`)`、`,`、`.`、`:`、`_`。

## vpn_connection_local_cidrs_required
**メッセージ**: VPN 接続の作成時には少なくとも 1 つのローカル CIDR ブロックが必要です。(At least one local CIDR block is required when creating a VPN connection.)

指定された要件を満たす有効なローカル CIDR を指定してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

## vpn_connection_local_subnets_quota_exceeded
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` の VPN 接続間にあるローカル・サブネットが割り当て量に達しました。(Local subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

複数の VPN 接続にまたがる現在のローカル・サブネットを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections` API を使用して、各接続の `local_cidrs` フィールドを確認してください。同等の CLI コマンドは `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID` です。

## vpn_connection_local_subnets_quota_exceeded_for_connection
**メッセージ**: VPN 接続のローカル・サブネットが割り当て量に達しました。(Local subnets for the VPN connection has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}ページで指定します。

単一の VPN 接続の現在のローカル・サブネットを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`local_cidrs` フィールドを確認してください。同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_not_created
**メッセージ**: VPN 接続 `<vpn_connection_id>` を作成できませんでした。(The VPN connection `<vpn_connection_id>` could not be created.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_not_deleted
**メッセージ**: VPN 接続 `<vpn_connection_id>` を削除できませんでした。(The VPN connection `<vpn_connection_id>` could not be deleted.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_not_found
**メッセージ**: VPN 接続 `<vpn_connection_id>` が見つかりませんでした。(The VPN connection `<vpn_connection_id>` could not be found.)

接続 ID が正しいかどうか確認してください。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_not_updated
**メッセージ**: VPN 接続 `<vpn_connection_id>` を更新できませんでした。(The VPN connection `<vpn_connection_id>` could not be updated.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_pair_cidrs_duplicated
**メッセージ**: 同じピア・ゲートウェイを使用する、ローカル CIDR とピア CIDR の少なくとも 1 つのペアが重複しています。(At least one pair of local CIDR and peer CIDR with the same peer gateway has been duplicated.)

これと同じローカル CIDR とピア CIDR が含まれる別の接続が同じゲートウェイ上に存在します。有効なローカル CIDR/ピア CIDR のセットを指定してください。

## vpn_connection_peer_cidrs_required
**メッセージ**: VPN 接続の作成時には少なくとも 1 つのピア CIDR ブロックが必要です。(At least one peer CIDR block is required when creating a VPN connection.)

指定された要件を満たす有効なピア CIDR を指定してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

## vpn_connection_peer_subnets_quota_exceeded
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` の VPN 接続間にあるピア・サブネットが割り当て量に達しました。(Peer subnets across VPN connections for the VPN gateway `<vpn_gateway_id>` has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

複数の VPN 接続にまたがる現在のピア・サブネットを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections` API を使用して、各接続の `peer_cidrs` フィールドを確認してください。
同等の CLI コマンドは `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID` です。

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**メッセージ**: VPN 接続のピア・サブネットが割り当て量に達しました。(Peer subnets for the VPN connection has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

単一の VPN 接続の現在のピア・サブネットを表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API を使用して、`peer_cidrs` フィールドを確認してください。同等の CLI コマンドは `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID` です。

## vpn_connection_static_route_not_created
**メッセージ**: ピア CIDR ブロック `<peer_cidr>` の静的経路を追加できませんでした。(Failed to add a static route for the peer CIDR block `<peer_cidr>`.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_static_route_not_deleted
**メッセージ**: ピア CIDR ブロック `<peer_cidr>` の静的経路を削除できませんでした。(Failed to remove a static route for the peer CIDR block `<peer_cidr>`.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_connection_update_cidrs_not_permitted
**メッセージ**: 接続の更新時に CIDR ブロックを変更することはできません。(CIDR blocks cannot be changed when updating a connection.) CIDR ブロックの作成時または削除時に、正しい API を使用してください。(Please use the correct API when creating or deleting CIDR blocks.)

指定された要件を満たす有効な要求値を指定してください。

この問題を修正する詳しい手順については、[API 資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}を参照してください。

## vpn_connections_quota_exceeded
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` が割り当て量に達したため、VPN 接続を作成できません。(The VPN connection cannot be created because the VPN gateway `<vpn_gateway_id>` has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

単一の VPN ゲートウェイの現在の VPN 接続を表示するには、`GET /vpn_gateways/<vpn_gateway_id>/connections` API を使用してください。同等の CLI コマンドは `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID` です。

## vpn_gateway_duplicate_name
**メッセージ**: `<vpn_gateway_name>` という名前は VPN ゲートウェイ `<vpn_gateway_id>` で既に使用されています。(The name `<vpn_gateway_name>` is already in use by VPN gateway `<vpn_gateway_id>`.)

異なる VPN ゲートウェイ名を指定してください。数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_initialization_error
**メッセージ**: この VPN ゲートウェイを初期化できませんでした。(The VPN gateway could not be initialized.)

数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_invalid_name
**メッセージ**: `<vpn_gateway_name>` という名前は有効な VPN ゲートウェイ名ではありません。(The name `<vpn_gateway_name>` is not a valid VPN gateway name.)

有効な VPN ゲートウェイ名は、英字で始まり、その後ろに英字、数字、下線、またはハイフンが続きます。

## vpn_gateway_invalid_state
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` は、要求された操作に対して無効な状態になっています。(The VPN gateway `<vpn_gateway_id>` is in an invalid state for the requested operation.)

VPN ゲートウェイを操作するには、その VPN ゲートウェイは `available` ステータスになっている必要があります。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_ip_create_error
**メッセージ**: この VPN ゲートウェイの IP アドレスを作成できません。(Unable to create an IP address for the VPN gateway.)

数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_missing_subnet_id
**メッセージ**: 指定された VPN ゲートウェイ・テンプレートのサブネット ID が見つかりませんでした。(Could not find a subnet identifier for the given VPN gateway template.)

**「サブネット ID」**は必須フィールドです。コマンドでサブネット ID を指定してください。

## vpn_gateway_missing_name
**メッセージ**: 指定された VPN ゲートウェイ・テンプレートの名前が見つかりませんでした。(Could not find a name for the given VPN gateway template.)

**name** は必須フィールドです。コマンドで名前を指定してください。

## vpn_gateway_not_created
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` を作成できませんでした。(The VPN gateway `<vpn_gateway_id>` could not be created.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_not_deleted
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` を削除できませんでした。(The VPN gateway `<vpn_gateway_id>` could not be deleted.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_not_found
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` が見つかりませんでした。(The VPN gateway `<vpn_gateway_id>` could not be found.)

VPN ゲートウェイ ID が正しいかどうか確認してください。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_not_updated
**メッセージ**: VPN ゲートウェイ `<vpn_gateway_id>` を更新できませんでした。(The VPN gateway `<vpn_gateway_id>` could not be updated.)

数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_subnet_not_found
**メッセージ**: 指定された VPN ゲートウェイのサブネット `<subnet_id>` が見つかりませんでした。(Could not find the subnet `<subnet_id>` for the given VPN gateway.)

参照対象のサブネットは存在していません。 要求を見直して、適切なサブネット ID を指定したことを確認してください。 数分後に再試行してください。 この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateway_subnet_status_error
**メッセージ**: サブネットのステータスが原因で、この VPN ゲートウェイを作成できませんでした。(The VPN gateway could not be created due to subnet status.)

`available` ステータスであるサブネットを指定してください。数分後に再試行してください。この問題が解決しない場合は、[サポートにお問い合わせ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)ください。

## vpn_gateways_quota_exceeded
**メッセージ**: ご使用のアカウントまたはリージョンあるいはその両方で割り当て量に達したため、VPN ゲートウェイを作成できません。(The VPN Gateway cannot be created because your account and/or the region has reached the quota.)

リソースあたりの割り当て量は、[「割り当て量」](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}ページで指定します。

現在の VPN ゲートウェイを表示するには、`GET /vpn_gateways` API を使用してください。
同等の CLI コマンドは `ibmcloud is vpn-gateways` です。

## zone_inconsistency
**メッセージ**: リソースは同じゾーンに属していなければなりません。(The resources must belong to the same zone.)

* ボリュームを接続するときは、ボリュームとインスタンスが同じゾーンにあることを確認してください。
* 浮動 IP を関連付けるときは、浮動 IP とサブネットが同じゾーンにあることを確認してください。
