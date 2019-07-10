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

# IBM Cloud Virtual Private Cloud API 错误消息
{: #rias-error-messages}

从 {{site.data.keyword.cloud}} Virtual Private Cloud API 收到错误消息时，可以使用消息标识来查找有关如何解决问题的更多信息。
{:shortdesc}

## account_type_invalid
**消息**：您必须具有现收现付帐户才能供应虚拟私有云。

只有现收现付套餐的帐户才能供应 VPC。您可以在下面找到有关升级帐户的更多详细信息：[升级帐户](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**消息**：无法删除网络 ACL，因为此 ACL 已连接到资源。

无法删除网络 ACL，因为此 ACL 已连接到子网或 VPC。

要查看子网是否在使用网络 ACL，请使用 `GET /v1/subnets?version=2019-05-31&generation=1` API。等效 CLI 命令：`ibmcloud is subnets`。要更新子网使用的网络 ACL，请使用 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API 或 `ibmcloud is subnet-update` CLI。

要查看 VPC 是否在使用网络 ACL，请使用 `GET /v1/vpcs?version=2019-05-31&generation=1` API。等效 CLI 命令：`ibmcloud is vpcs`。无法更新或删除 VPC 使用的缺省网络 ACL。删除 VPC 时，会自动删除缺省网络 ACL。

## address_prefix_conflict
**消息**：具有此 CIDR 的地址前缀已在使用中。

所请求地址前缀的 CIDR 与现有地址前缀冲突。

要查看 VPC 的地址前缀的列表，请使用 `GET /vpcs/{vpc-id}/address_prefixes` API，然后检查请求的 CIDR 地址是否未由其他地址前缀的 `cidr` 字段使用。等效 CLI 命令：`ibmcloud is vpc-address-prefix`。

## address_prefix_in_use
**消息**：无法删除使用中的地址前缀。

一个或多个子网正在使用该地址前缀。要确定哪些子网在使用该地址前缀，请使用 `GET /v1/subnets?version=2019-05-31&generation=1` API 命令，然后检查 `ipv4_cidr_block` 字段。等效 CLI 命令：`ibmcloud is subnets`，运行此命令后检查 `Subnet CIDR` 列。必须先删除使用该地址前缀的所有子网，然后才能删除该地址前缀。

## backend_service_unavailable
**消息**：后端服务不可用。

VPC 使用的后端云服务无法响应。VPC 使用了多个 IBM Cloud 服务，例如：

- [Identity and Access Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [全局目录](https://{DomainName}/catalog)
- 资源控制器
- 资源管理器

您可以检查 IBM Cloud 服务的[状态](https://{DomainName}/status)，并在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## bad_field
**消息**：应该提供正确的实例 UUID。

请求中提供的某个值不正确。请检查返回的错误中的 `target` 值，以获取有关哪个参数不正确的线索。在某些情况下，系统找不到请求中的 UUID 或卷。请提供有效值并重试。

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取其他帮助。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## bad_request
**消息**：给定的信息无效、格式错误或缺少必需字段。

请求不符合预期要求。请使用 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以帮助设置请求格式。

如果遵循了规范，但仍收到错误，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## classic_access_vpc_conflict_duplicate_res
**消息**：每个区域只能创建一个使用经典访问的 VPC。

每个区域只能创建一个使用经典访问的 VPC。要列出使用经典访问的 VPC，请运行 `ibmcloud is vpcs --classic-access`。必须先删除使用经典访问的现有 VPC，然后才能创建另一个使用经典访问的 VPC。

## classic_access_vpc_account_not_VRF_enabled
**消息**：帐户必须启用 VRF，才能创建经典访问 VPC。

链接的经典帐户未启用 VRF。经典访问 VPC 需要链接的经典帐户才能启用 VRF。要对帐户启用 VRF，请参阅 [IBM Cloud 上的 VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。

## default_address_prefix_not_found
**消息**：找不到缺省地址前缀。

找不到缺省地址前缀时，可能会看到此错误消息。

## duplicate_error
**消息**：提供的输入已存在。

指定的资源已存在。请尝试对要创建的资源使用其他名称。例如，创建新的 VPC 时，可以通过运行 `ibmcloud is vpcs` 列出现有 VPC。请选择不冲突的名称。

## floating_ip_in_use
**消息**：浮动 IP 已在使用中。

此浮动 IP 已与虚拟服务器实例或公共网关相关联。

要查看使用浮动 IP 的位置，请使用 `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` API，然后查看“target”值。等效 CLI 命令：`ibmcloud is floating-ip FLOATINGIP_ID`。

## gateway_too_many
**消息**：在 VPC 中，每个专区只能有一个公共网关。

在 VPC 中，每个专区只允许有一个公共网关，但一个公共网关可以连接到所在专区中的多个子网。要查找专区的公共网关，请运行 GET public_gateways API，并查看“vpc”和“zone”值。如果使用的是 CLI，那么可以运行 `ibmcloud is public-gateways` 命令，并查看“VPC”和“Zone”值。

请使用公共网关的标识将其连接到子网，具体请参阅 [API 示例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-)中的示例。如果使用的是 CLI，那么可以使用 subnet update 命令将子网连接到公共网关，例如 `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`。

## health_monitor_invalid_delay
**消息**：指定的运行状况监视器延迟 <health_monitor_delay> 无效。

请为 `health monitor delay` 参数提供 2 到 60 之间的值。

## health_monitor_invalid_max_retries
**消息**：指定的运行状况监视器最大重试次数 <health_monitor_max_retries> 无效。

请为 `health max retries` 提供 1 到 10 之间的值。

## health_monitor_invalid_timeout
**消息**：指定的运行状况监视器超时 <health_monitor_timeout> 无效。

请为 `health timeout` 提供 1 到 59 之间的值。所选值必须小于 `health monitor delay` 的值。

## health_monitor_invalid_url_path
**消息**：指定的运行状况 URL 路径 <health_monitor_url> 无效。

请为运行状况监视器提供有效的 URL。URL 可以包含字母数字字符、连字符和句点。不允许使用空格或反斜杠。URL 路径的最大长度为 255 个字符。

## health_monitor_invalid_protocol
**消息**：指定的运行状况监视器协议 <health_monitor_protocol> 无效。

请为运行状况监视器协议提供有效值。可接受的值为 `http` 和 `tcp`。

## health_monitor_missing_protocol
**消息**：缺少运行状况监视器协议。

运行状况监视器协议是必需字段。在请求中，指定运行状况监视器协议。可接受的值为 `http` 和 `tcp`。

## health_monitor_missing_url_path
**消息**：缺少运行状况监视器 URL 路径。对于 HTTP 类型的运行状况监视器，此项是必需的。

对于使用 HTTP 协议的运行状况监视器，运行状况监视器 URL 路径是必需字段。请提供有效的 URL 路径。

## health_monitor_timeout_delay_conflict
**消息**：运行状况监视器超时 <health_monitor_timeout> 不应大于或等于延迟 <health_monitor_delay>。

`health monitor delay` 参数的值必须大于 `health monitor timeout` 的值。

## http_request_size_exceeded
**消息**：HTTP 请求太大。

在请求中发送的有效内容包含太多字符时，会发生此问题。请使用较小的有效内容重试。例如，不要尝试在单个请求中执行所有操作，请尝试在一个请求中创建最小资源，然后在多个后续请求中以递增方式向其附加状态。

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取其他帮助。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## iam_failure
**消息**：无

IAM 服务中发生了故障，正在验证认证或授权。IAM 服务中断可能是暂时的。请在几分钟后重试请求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policies_quota_exceeded
**消息**：无法创建 IKE 策略，因为帐户已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看当前 IKE 策略，请使用 `GET /ike_policies` API。等效 CLI 命令：`ibmcloud is ike-policies`

## ike_policy_duplicate_name
**消息**：名称 `<ike_policy_name>` 已由 IKE 策略 `<ike_policy_id>` 使用。

请提供其他 IKE 策略名称。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_in_use
**消息**：IKE 策略 `<ike_policy_id>` 已由一个或多个连接使用。

如果 IKE 策略已由一个或多个连接使用，那么无法将其删除。

要查看在使用 IKE 策略的连接，请使用 `GET /ike_policies/<ike_policy_id>/connections` API。等效 CLI 命令：`ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**消息**：名称 `<ike_policy_name>` 不是有效的 IKE 策略名称。

有效的 IKE 策略名称以字母开头，后跟字母、数字、下划线或连字符。

## ike_policy_not_created
**消息**：无法创建 IKE 策略 `<ike_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_deleted
**消息**：无法删除 IKE 策略 `<ike_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_found
**消息**：找不到 IKE 策略 `<ike_policy_id>`。

您引用了不存在的 IKE 策略。请复查请求，以确保指定的 IKE 策略标识正确。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_updated
**消息**：无法更新 IKE 策略 `<ike_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_delete_conflict
**消息**：无法删除处于当前状态的实例。

无法删除处于 `deleting`、`pending`、`starting`、`stopping` 或 `restarting` 状态的实例。实例必须处于 `running` 或 `stopped` 状态，才能将其删除。如果状态保持不变，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_invalid_hostname
**消息**：无法创建实例，因为主机名无效。

实例主机名必须满足以下需求：
* 主机名只能包含字母数字字符和短划线
* 不允许使用大写字符
* 不允许使用前导和尾部短划线
* 不允许使用前导数字
* 不允许连续使用短划线
* 对于 Windows，最大长度为 15 个字符
* 对于其他操作系统，最大长度为 40 个字符

## instance_invalid_port_speed
**消息**：无法创建实例，因为指定的网络接口端口速度无效。

网络接口端口速度必须为 `100` 或 `1000` Mbps。

## instance_name_exists
**消息**：相同的实例名称已存在。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_sec_volume_over_quota
**消息**：无法创建实例，因为卷连接数将超出配额。

每个资源的 VPC 配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}页面上指定。

## instance_too_many_keys
**消息**：无法创建 Windows 实例，因为请求包含多个密钥。

Windows 实例仅支持一个密钥。

## insufficient_space_for_subnet
**消息**：地址前缀中子网的空间不足。

无法创建子网，因为无法分配所请求的地址数。请使用更小的地址计数或更大的 CIDR 重试。

## internal_error
**消息**：发生内部错误。

发生意外错误。此问题可能是暂时的。请在几分钟后重试请求。如果此错误仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_server_error
**消息**：无

发生意外错误。此问题可能是暂时的。请在几分钟后重试请求。如果此错误仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_solution
**消息**：请联系管理员。

发生意外错误。此问题可能是暂时的。请在几分钟后重试请求。如果此错误仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## invalid_generation_parameter
**消息**：generation 查询参数必须设置为 1。

对于 2019 年 5 月 31 日及之后的版本，`generation` 查询参数必须设置为 1，以允许发出 VPC on Classic API 请求。

## invalid_id_format
**消息**：标识格式错误。请确保格式正确。

请确保提供的标识不包含任何格式错误的数据。

如果发出分页请求时，提供的启动查询的格式不正确，那么可能会收到此错误消息。例如，`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` 包含两个 `?`。请修正查询，然后重试。

## invalid_route
**消息**：请求的路径不存在。

所提供的 API URL 上请求的路径不存在。请验证指定用于调用 API 端点的 URL 是否正确。

## invalid_request_field
**消息**：请求中提供的字段无效。

例如，要更新子网使用的网络 ACL，请使用 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API。
以下请求无效，因为“networkacl”不是有效字段：`PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取可接受的值。

## invalid_state
**消息**：资源的当前状态不支持对资源请求的操作。

如果资源是实例：

1. 可能已经在对实例执行重新引导操作。请参阅[允许的操作](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance)，具体允许的操作取决于实例的状态。

2. 实例的状态为 `pending` 时，无法执行以下操作：
  * 删除实例
  * 将卷连接到实例
  * 从实例拆离卷
  * 更新实例

请稍后重试操作。如果资源的状态保持不变，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

如果资源是映像：

映像的状态为 `deleting` 时，无法执行以下操作：
  * 修补映像
  * 删除映像

不要再次发出该请求。映像删除操作将在其自己的时间内完成。删除操作完成后，对该映像的所有请求都会失败，并返回错误 `not_found`。

## invalid_version
**消息**：`version` 参数无效，其格式必须为 `YYYY-MM-DD`。

版本必须符合格式 _YYYY-MM-DD_。对于一位数的月份或日期，例如 1 月 1 日，版本应该类似于 `2019-01-01`。version 参数必须在 URL 中提供。version 参数中提供的日期必须晚于 2019 年 1 月 1，但早于当前日期。

## invalid_version_range
**消息**：`version` 值不能设置为未来日期，也不能设置为早于 `2019-01-01`。

version 参数中提供的日期必须晚于 2019 年 1 月 1，但早于当前日期。

## invalid_zone
**消息**：请检查请求的资源是否位于同一专区中。

尝试将 zon-1 中的公共网关连接到 zone-2 中的子网时，可能会看到此消息。

## ipsec_policies_quota_exceeded
**消息**：无法创建 IPsec 策略，因为帐户已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看当前 IPsec 策略，请使用 `GET /ipsec_policies` API。等效 CLI 命令：`ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**消息**：名称 `<ipsec_policy_name>` 已由 IPsec 策略 `<ipsec_policy_id>` 使用。

请提供其他 IPsec 策略名称。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_in_use
**消息**：IPsec 策略 `<ipsec_policy_id>` 已由一个或多个连接使用。

如果 IPsec 策略已由一个或多个连接使用，那么无法将其删除。

要查看在使用 IPsec 策略的连接，请使用 `GET /ipsec_policies/<ipsec_policy_id>/connections` API。等效 CLI 命令：`ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**消息**：名称 `<ipsec_policy_name>` 不是有效的 IPsec 策略名称。

有效的 IPsec 策略名称以字母开头，后跟字母、数字、下划线或连字符。

## ipsec_policy_not_created
**消息**：无法创建 IPsec 策略 `<ipsec_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_deleted
**消息**：无法删除 IPsec 策略 `<ipsec_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_found
**消息**：找不到 IPsec 策略 `<ipsec_policy_id>`。

您引用了不存在的 IPsec 策略。请复查请求，并指定有效的 IPsec 策略标识。如果您确定该策略存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_updated
**消息**：无法更新 IPsec 策略 `<ipsec_policy_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_content_exists
**消息**：相同的密钥内容已存在。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_name_exists
**消息**：相同的密钥名称已存在。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_invalid_name
**消息**：无法创建密钥，因为名称无效。

密钥名称必须满足以下需求：
* 必须以字母字符 [A-Za-z] 开头
* 只能包含字母数字字符、短划线或下划线：[-A-Za-z0-9_]
* 长度不能超过 65 个字符

## key_invalid_type
**消息**：无法创建密钥，因为类型无效。

唯一支持的密钥类型是 `rsa`。

## key_parse_failure
**消息**：无法创建密钥，因为公用密钥值无效。

请求中提供的公用密钥无效。请检查该值或重新生成公用密钥，然后重试。

在 Linux 操作系统中，可以运行以下命令来验证公用密钥是否有效：`ssh-keygen -l -v -f <key_file>`。

## listener_certificate_not_found
**消息**：找不到 CRN 为 `<listener_certificate_crn>` 的证书实例，或者无权访问该证书实例。

请提供现有证书实例 CRN，或者要求帐户管理员授予您对所提供证书实例的访问许可权。

## listener_duplicate_port
**消息**：端口 `<listener_port>` 已由其他侦听器实例使用。请选择其他端口。

请选择其他端口。

## listener_invalid_certificate
**消息**：HTTPS 侦听器的证书实例的 CRN 无效。

请提供有效的证书实例 CRN。

## listener_invalid_connection_limit
**消息**：侦听器连接限制 `<listener_connection_limit>` 无效。

请提供有效的连接限制。连接限制必须是 1 到 15000 之间的整数。

## listener_invalid_port
**消息**：侦听器端口 `<listener_port>` 无效。

请选择其他端口。

## listener_invalid_protocol
**消息**：侦听器协议 `<listener_protocol>` 无效。

请提供有效的侦听器协议。可接受的侦听器协议值为 `http`、`https` 和 `tcp`。

## listener_missing_certificate_crn
**消息**：缺少 `https` 侦听器的证书实例的 CRN。

证书实例的 CRN 是必需字段。

## listener_missing_port
**消息**：缺少侦听器端口。

侦听器端口是必需字段。

## listener_missing_protocol
**消息**：缺少侦听器协议。

侦听器协议是必需字段。请在请求中提供侦听器协议。可接受的值为 `http`、`https` 和 `tcp`。

## listener_not_found
**消息**：找不到标识为 `<listener_id>` 的侦听器。

请提供现有侦听器标识。

## listener_over_quota
**消息**：无法创建侦听器。负载均衡器资源的侦听器配额已达到最大限制。

每个资源的配额在 [VPC 的配额和限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定。

## listener_pool_protocols_conflict
**消息**：侦听器协议 (`<listener_protocol>`) 和池协议 (`<pool_protocol>`) 相冲突。

使用 `https` 或 `http` 协议的侦听器只能与使用 `http` 协议的池相关联。与此类似，`tcp` 侦听器只能与 `tcp` 池相关联。

## listener_reserved_port_conflict
**消息**：侦听器端口 `<listener_port>` 是其中一个保留端口。端口范围 56500 到 56520 保留用于管理目的。请选择其他端口。

请选择其他端口。

## load_balancer_delete_conflict
**消息**：无法删除标识为 <load_balancer_id> 的负载均衡器，因为它处于下列其中一个状态：  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

请在负载均衡器处于 ACTIVE 状态时，再将其删除。

## load_balancer_duplicate_name
**消息**：名称 `<load_balancer_name>` 已由其他负载均衡器实例使用。请选择其他名称。

请为负载均衡器实例提供其他名称。负载均衡器名称在 VPC 和帐户中应该唯一。

Load Balancer for VPC 不会与 Cloud Load Balancer 相冲突，因为这两个服务的 DNS 域名不同，并且 DNS 名称中不包含 Load Balancer for VPC 的名称。
{: note}

## load_balancer_insufficient_ips
**消息**：标识为 <subnet_id> 的子网的可用 IPv4 地址不足。

在请求中，提供要在其中创建负载均衡器且具有可用 IPv4 地址的有效子网。

## load_balancer_invalid_description
**消息**：负载均衡器描述 `<load_balancer_description>` 无效。

负载均衡器描述不能超过 255 个字符。

## load_balancer_invalid_is_public
**消息**：is_public `<load_balancer_is_public>` 的值无效。

负载均衡器参数 `is_public` 的值必须为 _true_ 或 _false_。

## load_balancer_invalid_name
**消息**：名称 `<load_balancer_name>` 无效。

名称不能为空。有效的负载均衡器名称以字母开头，后跟字母、数字或下划线。名称长度不能超过 40 个字符。

## load_balancer_invalid_subnet
**消息**：标识为 <subnet_id> 的子网无效。请确保使用状态为“available”的现有子网。

请在输入创建负载均衡器的请求时，提供状态为 `available` 的有效子网。

## load_balancer_missing_is_public
**消息**：缺少“is_public”字段。

`is_public` 字段是必需的。请提供 `is_public` 字段的值。

## load_balancer_missing_name
**消息**：缺少负载均衡器名称。

负载均衡器 `name` 是必需字段。请提供负载均衡器的名称。

## load_balancer_missing_subnets
**消息**：缺少负载均衡器子网。

`subnets` 字段是必需的。在创建负载均衡器的请求中，提供有关要在其中创建负载均衡器的子网的信息。

## load_balancer_missing_subnet_id
**消息**：缺少子网标识。

**Subnet ID** 是必需字段。请在命令中提供子网标识。

## load_balancer_not_found
**消息**：找不到标识为 `<load_balancer_id>` 的负载均衡器。

请提供现有负载均衡器的标识。

## load_balancer_over_quota
**消息**：无法创建负载均衡器。您帐户下的负载均衡器实例的配额已达到最大限制。

请删除现有负载均衡器，或联系支持人员以增加您帐户的负载均衡器配额。

每个资源的配额在 [VPC 的配额和限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定。

## load_balancer_subnet_not_found
**消息**：找不到标识为 <subnet_id> 的子网。

在请求中，必须提供要在其中创建负载均衡器的现有子网的标识。

## load_balancer_unchanged_update
**消息**：标识为 `<load_balancer_id>` 的负载均衡器实例没有要更新的内容。

未提供用于更新具有指定标识的负载均衡器实例的信息。

## load_balancer_update_conflict
**消息**：无法更新标识为 <load_balancer_id> 的负载均衡器，因为它处于下列其中一个状态：
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

请在负载均衡器处于 ACTIVE 状态时，再对其进行更新。

## member_duplicate_address_and_port
**消息**：池中已存在地址为 <member_address> 且端口为 <member_port> 的成员。

请在请求中提供唯一的成员地址和端口组合。

## member_invalid_address
**消息**：指定的成员 IP 地址 <member_ip> 无效。

在请求中，为成员提供有效的 IPv4 CIDR 地址。

## member_invalid_port
**消息**：指定的成员端口 `<member_port>` 无效。

请提供有效的端口号。有效端口号的范围为 1 到 65535。

## member_invalid_weight
**消息**：指定的成员权重 `<member_weight>` 无效。

请提供有效的成员权重。值必须是 0 到 100 之间的整数。

## member_ip_not_found
**消息**：IP 地址为 <member_IP> 的成员不属于负载均衡器的区域和 VPC 中的任何子网。

在请求中，提供属于负载均衡器所在区域和 VPC 中子网的成员的地址。

## member_missing_address
**消息**：缺少成员地址。

成员地址是必需字段。请提供成员地址的值。

## member_missing_protocol_port
**消息**：缺少成员端口。

成员端口是必需字段。请提供成员端口的值。

## member_not_found
**消息**：找不到标识为 <member_id> 的成员。

请提供现有成员标识。

## member_over_quota
**消息**：无法创建成员。池下成员实例的配额已达到最大限制。

每个资源的配额在 [VPC 的配额和限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定。

## missing_generation_parameter
**消息**：generation 查询参数是必需的。

对于 2019 年 5 月 31 日及之后的版本，在 VPC on Classic API 请求中 `generation` 查询参数是必需的。

## missing_ims_account_id
**消息**：无

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## missing_version
**消息**：`version` 参数是必需的，其格式必须为 `YYYY-MM-DD`。

version 参数对于所有 API 请求都是必需的。版本必须符合格式 _YYYY-MM-DD_。对于一位数的月份或日期，例如 1 月 1 日，版本应该类似于 `2019-01-01`。version 参数中提供的日期必须晚于 2019 年 1 月 1，但早于当前日期。请查看这些 [API 示例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)，以了解如何提供 version 参数。

## network_conflict
**消息**：CIDR 与 VPC 中的现有地址前缀相冲突。

如果提供的网络 CIDR 与同一 IP 空间中的现有网络 CIDR 相冲突，那么可能会看到此消息。

要在 VPC 的地址前缀中查找现有 CIDR，请运行 API 命令 `GET /v1/vpcs/<vpc-id>/address_prefixes`，然后查看响应的 `cidr` 值。检查 `cidr` 的值，以确保所提供的 CIDR 未由其他地址前缀使用。

如果使用的是 CLI，请运行 `ibmcloud is vpc-addrs <vpc-id>` 命令，然后检查 `CIDR Block` 的值是否相冲突。

要在子网中查找现有 CIDR，请运行 API 命令 `GET /v1/subnets` 来列出 VPC 的所有子网。然后，针对 VPC 中的每个子网，运行 API 命令 `GET /v1/subnets/<subnet-id>`，并检查响应中 `ipv4_cidr_block` 的值。检查 `ipv4_cidr_block` 的值，以确保所提供的 CIDR 未由其他地址前缀使用。

如果使用的是 CLI，请运行 `ibmcloud is subnets` 命令来列出 VPC 的所有子网。然后，针对 VPC 中的每个子网，运行 `ibmcloud is subnet <subnet-id>` 命令，并检查 CLI 输出中的 `IPv4 CIDR` 的值是否相冲突。

## not_authorized
**消息**：请求未获授权。

如果缺少 IAM 令牌或 IAM 令牌到期，那么您可能会看到此错误。有关如何生成令牌的指示信息，请参阅[使用 REST API 创建 VPC](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)。如果令牌未到期，请确保使用的帐户有权使用此功能，并且您具有[正确许可权](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

如果您具有正确的权限，但仍收到此错误，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_found
**消息**：请检查请求的资源是否存在。

您引用的资源不存在或您无权访问该资源。请复查请求，以确保指定的标识和引用正确。

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取其他帮助。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_implemented
**消息**：无

此方法未实现。请检查请求，以确保调用的是[有效 API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果需要实现此 API，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_in_address_prefix
**消息**：提供的 CIDR 不适合所提供专区中的任何地址前缀。

如果用户尝试使用不属于给定专区的任何地址前缀的 CIDR 来创建子网，那么会发生此错误。

请运行 `GET /vpcs/{vpc_id}/address_prefixes` 来获取 VPC 的地址前缀列表。如果使用的是 CLI，那么可以运行 `ibmcloud is vpc-address-prefixes` 来列出 VPC 的所有地址前缀。查看响应的 `cidr` 和 `zone` 值，并确保子网的 `cidr` 是尝试在其中创建子网的专区的地址前缀中 `cidr` 的子集。

## over_quota
**消息**：请求将超过资源类型的配额。

每个资源的配额在 [VPC 的配额和限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定。

## password_not_ready
**消息**：无

实例必须正在运行，然后才能检索密码。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## pool_delete_conflict
**消息**：无法删除池，因为该池仍与一个或多个侦听器相关联。

请确保解除池与任何侦听器的关联，然后重试。

## pool_duplicate_name
**消息**：池名称 `<pool_name>` 已由其他池使用。请选择其他名称。

请选择其他池名称。

## pool_invalid_name
**消息**：名称 <pool_name> 无效。

请选择有效的池名称。有效的负载均衡器名称以字母开头，后跟字母、数字或连字符。名称长度不能超过 40 个字符。

## pool_invalid_session_persistence
**消息**：池会话持久性值无效。

请提供有效的会话持久性值。可接受的值为 `SOURCE_IP`。

## pool_missing_algorithm
**消息**：缺少负载均衡算法。

负载均衡算法是必需字段。请在请求中提供负载均衡算法。可接受的值为 `round_robin`、`weighted_round_robin` 和 `least_connections`。

## pool_missing_health_monitor
**消息**：缺少池运行状况监视器。

池运行状况监视器是必需字段。

## pool_missing_members
**消息**：缺少池成员。

请提供池成员。

## pool_missing_name
**消息**：缺少池名称。

池名称是必需字段。

## pool_missing_protocol
**消息**：缺少池协议。

池协议是必需字段。请在请求中提供池协议。可接受的值为 `http` 和 `tcp`。

## pool_not_found
**消息**：找不到标识为 `<pool_id>` 的池。

请提供现有池标识。

## pool_over_quota
**消息**：无法创建池。负载均衡器资源的池配额已达到最大限制。

每个资源的配额在 [VPC 的配额和限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定。

## public_gateway_in_use
**消息**：无法删除正在使用的公共网关。

公共网关当前连接到一个或多个子网。必须先断开公共网关与所有子网的连接，然后才能删除公共网关。

要查看哪个子网在使用公共网关，请使用 `GET /v1/subnets?version=2019-05-31&generation=1` API。等效 CLI 命令：`ibmcloud is subnets`。要从子网拆离公共网关，请使用 API 命令 `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` 或 CLI 命令 `ibmcloud is subnet-public-gateway-detach`。

## rate_limit_exceeded
**消息**：短时间内请求太多。

如果在指定的时间间隔内收到太多请求，那么会返回此错误消息。请稍候，然后重试。

## security_group_active_transactions
**消息**：在实例显示处于活动状态之前，无法连接或拆离接口。

实例必须处于活动状态，然后其网络接口才能连接到安全组。请使用 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance` 来检查实例的状态。状态为 `running` 后，请重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_already_attached
**消息**：接口已连接到安全组。

接口已连接到安全组。请使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 来查看连接的接口。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_exists
**消息**：安全组已存在。

安全组名称在 VPC 中必须唯一。目标 VPC 中已存在具有该名称的安全组。请使用 `GET /v1/security_groups?version=2019-05-31&generation=1` 或 `ibmcloud is security-groups` 来查看当前安全组。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_interfaces_attached
**消息**：连接了接口时，无法删除安全组。


删除安全组之前，请拆离所有接口。请使用 `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interface-remove` 来拆离接口。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_interfaces_per_sg_exceeded
**消息**：超过了每个安全组的接口数限制。

再将一个接口连接到安全组将超过每个安全组的接口数限制。每个资源的配额在 [VPC 的配额和限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定。请评估策略，并考虑创建具有类似规则的其他安全组。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_last_security_group_is_default
**消息**：缺省安全组是连接的唯一安全组时，无法将其除去。

一个网络接口必须至少连接到一个安全组。如果未指定安全组，那么会将其连接到 VPC 的缺省安全组。将接口连接到其他安全组，然后重试将其从缺省安全组拆离。有关缺省安全组的信息，请参阅[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_limit_exceeded
**消息**：超过了安全组限制。

您已尝试创建新的安全组，但当前安全组数已达到您的帐户配额。每个资源的配额在 [VPC 的配额和限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定。请评估用于将实例分配给安全组的策略。通常可以通过将多个实例分配给同一安全组来减少安全组总数。此策略将减少安全组数，使其低于您的帐户配额。在极少数情况下，通常对于大型组织，才需要增大配额。在这种情况下，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)以查询这是否可行。

## security_group_network_interface_not_active
**消息**：无法连接接口，因为该接口未处于活动状态。

网络接口在连接到安全组之前，必须处于活动状态。请使用 `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance-network-interface` 来查看接口的状态。状态为“available”后，请重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_not_attached
**消息**：接口未连接。

接口未连接到安全组。请使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 来查看连接的接口。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_not_in_vpc
**消息**：接口和安全组位于不同的 VPC 中。

要将网络接口连接到安全组，实例必须与安全组位于同一 VPC 中。请使用 `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is security-group` 来查看安全组详细信息和 VPC。使用 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance` 来查看实例详细信息和 VPC。

## security_group_order_bindings
**消息**：无法删除具有暂挂实例订单的安全组。

如果实例是使用网络接口上指定的安全组创建的，那么该实例和网络接口必须处于活动状态，然后才能删除该安全组。请使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 来查看连接的网络接口及其状态。接口的状态为“available”后，请重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_port_max_less_than_port_min
**消息**：TCP/UDP 最大端口不能小于最小端口。

最大端口值不能小于最小端口值。请指定大于最小端口值的最大端口值。

## security_group_port_range_both_or_neither
**消息**：必须取消设置端口范围，或者必须为“tcp”和“udp”同时设置最小端口和最大端口。

使用“tcp”或“udp”协议的规则可能具有未设置的端口范围（这会将规则应用于所有流量），或者可能指定了端口范围。要将流量限制为流至单个端口，请将最小端口和最大端口都设置为该端口值。例如，以下命令将创建规则以允许端口 22 上使用 SSH：`ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`。

有关安全组规则配置的更多信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)。


## security_group_port_range_invalid_protocol
**消息**：对“icmp”协议指定了端口范围。端口范围仅对“tcp”或“udp”协议有效。

使用“icmp”协议的规则具有类型和代码。使用“tcp”或“udp”协议的规则具有端口范围。有关安全组规则配置的更多信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)。

## security_group_remote_group_not_in_vpc
**消息**：远程组与此安全组不在同一 VPC 中。

安全组规则可能会引用远程安全组，以允许两个组的连接接口之间的流量。远程安全组必须位于同一 VPC 中。请使用 `GET /v1/security_groups?2019-01-01` 或 `ibmcloud is security-groups` 来查看当前安全组及其 VPC。

有关解决此问题的进一步指示信息，请参阅[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。


## security_group_remoting_rules
**消息**：连接了远程规则时，无法删除安全组。

安全组包含一个或多个引用远程安全组的规则。请使用 `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-rules` 来查看规则。“remote”字段将指定任何远程安全组。请在删除或编辑远程规则后重试。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全组文档](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

## security_group_remoting_rules_per_sg_exceeded
**消息**：超过了每个安全组的远程规则数限制。

再添加一个引用远程安全组的规则将超过每个安全组的远程规则数限制。每个资源的配额在 [VPC 的配额和限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定。

## security_group_rules_per_sg_exceeded
**消息**：超过了每个安全组的规则数限制。

添加规则将超过每个安全组的规则数限制。每个资源的配额在 [VPC 的配额和限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定。请评估策略，并考虑创建其他安全组。

## security_group_sgs_per_interface_exceeded
**消息**：超过了每个接口的安全组数限制。

将此接口连接到其他安全组将超过每个接口的安全组数限制。每个资源的配额在 [VPC 的配额和限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定。请评估策略，并考虑向现有安全组添加规则。


## security_group_type_code_invalid_protocol
**消息**：提供了“icmp”类型/代码，但请求的协议不是“icmp”。请将协议设置为“icmp”或指定端口范围。

只有使用“icmp”协议的规则具有类型和代码。使用“tcp”或“udp”协议的规则具有端口范围。有关安全组规则配置的更多信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic)。

## security_group_vpc_default
**消息**：无法删除 VPC 的缺省安全组。

无法删除缺省安全组。有关缺省安全组的信息，请参阅[更新缺省安全组](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group)的相关指南。

## service_manager_service_failure
**消息**：无

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_conflict
**消息**：CIDR 与 VPC 中的现有子网冲突。

请运行 `GET /subnets` API 来检索 VPC 中的所有子网。检查 `ipv4_cidr_block` 的值，以确保所提供的 CIDR 未由其他子网使用。

如果使用的是 CLI，那么可以运行 `ibmcloud is subnets`，然后查看“Subnet CIDR”值是否有冲突。

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取其他帮助。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty
**消息**：在子网为空之前，无法删除子网。请删除子网中的所有资源，然后重试。

有请求要删除子网，但该子网中仍有资源。子网中可能有实例、VPN、负载均衡器或公共网关。必须先删除子网中的资源，然后才能删除子网。

在某些情况下，即使控制台显示 VSI 和负载均衡器的数量都为 0，也可能会发生此错误。因为删除是异步操作，内部状态可能需要几分钟时间才会更改。请在几分钟后重试删除子网。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_pgway_exists
**消息**：子网连接到公共网关时，无法将其删除。请拆离公共网关，然后重试。

有请求要删除子网，但该子网仍连接有公共网关。必须先删除或拆离公共网关，然后才能删除子网。

如果使用的是 CLI，那么可以运行 `ibmcloud is public-gateways` 来列出公共网关，然后运行 `ibmcloud is subnet-public-gateway-detach` 以从子网拆离公共网关。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_ipaddr_exists
**消息**：子网包含 IP 地址时，无法将其删除。请删除与 IP 地址关联的任何服务器实例，然后重试。

有请求要删除子网，但该子网仍包含 IP 地址。必须先删除与 IP 地址关联的服务器实例，然后才能删除子网。

如果使用的是 CLI，那么可以运行 `ibmcloud is instances` 来列出服务器实例。请查看“Address”值以确定其 IP 地址，并查看“Status”以确定其状态。运行 `ibmcloud is instance-delete` 以删除其状态尚不为“deleting”的服务器实例。

在某些情况下，即使控制台显示 VSI 的数量为 0，也可能会发生此错误。因为删除是异步操作，内部状态可能需要几分钟时间才会更改。请在几分钟后重试删除子网。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_loadbalancer_exists
**消息**：子网包含负载均衡器时，无法将其删除。请删除负载均衡器，然后重试。

有请求要删除子网，但该子网仍包含负载均衡器。必须先删除负载均衡器，然后才能删除子网。

如果使用的是 CLI，那么可以运行 `ibmcloud is load-balancers` 来列出负载均衡器。请查看“Subnets”值以确定其子网，并查看“Status”以确定其状态。运行 `ibmcloud is load-balancer-delete` 以删除其状态尚不为“deleting”的负载均衡器。

在某些情况下，即使控制台显示负载均衡器的数量为 0，也可能会发生此错误。因为删除是异步操作，内部状态可能需要几分钟时间才会更改。请在几分钟后重试删除子网。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_vpn_gway_exists
**消息**：子网包含 VPN 网关时，无法将其删除。请删除 VPN 网关，然后重试。

有请求要删除子网，但该子网仍连接有 VPN 网关。必须先删除 VPN 网关，然后才能删除子网。

如果使用的是 CLI，那么可以运行 `ibmcloud is vpn-gateways` 来列出 VPN 网关。请查看“Subnets”值以确定其子网，并查看“Status”以确定其状态。运行 `ibmcloud is vpn-gateway-delete` 以删除其状态尚不为“deleting”的 VPN 网关。

在某些情况下，即使控制台显示 VPN 网关的数量为 0，也可能会发生此错误。因为删除是异步操作，内部状态可能需要几分钟时间才会更改。请在几分钟后重试删除子网。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_unknown_state
**消息**：对于请求的操作，子网 `<subnet_id>` 处于无效状态。

子网必须处于 `available` 状态，然后才能对其进行操作。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## target_in_use
**消息**：目标已连接浮动 IP。

有请求要将浮动 IP 地址连接到服务器网络接口，但网络接口已连接了浮动 IP。

要查找当前连接到网络接口的浮动 IP 地址，请运行 API 命令 `GET /v1/floating_ips?version=2019-05-31&generation=1`，然后在 `target.id` 字段中查找网络接口标识。  

如果使用的是 CLI，请运行 `ibmcloud is floating-ips` 命令，然后查看 `Target` 值。请注意，CLI 会在第一个短划线（“-”）字符处截断网络接口标识。例如，标识为 `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` 的服务器主网络接口在 CLI 输出中会显示为 `primary(abdfcb29-.)`。

## token_invalid
**消息**：服务令牌已到期或无效。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## token_missing
**消息**：在请求中，服务令牌为空或不存在。

请重新创建令牌，然后重试。

## validation_enum
**消息**：提供的值不是有效选项。

提供的其中一个字段的值必须来自可能值的枚举。

对于网络 ACL 规则，可指定方向：

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

例如，由于 `northbound` 不是枚举 `[ inbound, outbound ]` 中的有效选项，因此以下值将无效。

```json
{
    "direction":"northbound"
}
```

请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}或 [CLI 参考](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}，以获取可接受的值。

## validation_failure
**消息**：提供的 JSON 与预期结构不匹配。

要解决此问题，请确保请求的内容是有效的 JSON，并且请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## validation_invalid_cidr
**消息**：值不是有效的 CIDR。

值必须是不包含主机部分的有效内部 CIDR 块。

某些 IP 地址范围是保留的。有关保留的 IP 范围的更多信息，请参阅[将 VPC 与区域和子网配合使用](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}概述内容。

## validation_invalid_ipv4_cidr
**消息**：值不是有效的 IPv4 CIDR。

必须是不包含主机部分的 IPv4 内部 CIDR 块。

某些 IP 地址范围是保留的。有关保留的 IP 范围的更多信息，请参阅[将 VPC 与区域和子网配合使用](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}概述内容。

## validation_invalid_ipv6_cidr
**消息**：值不是有效的 IPv6 CIDR。

目前，不支持 IPv6。请使用 IPv4 地址。

## validation_invalid_address
**消息**：值不是有效地址。

必须是有效的 IP 地址。

[区域和子网](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)文档中提供了单独保留的 IP 地址的列表。

## validation_invalid_ipv4_address
**消息**：值不是有效的 IPv4 地址。

请提供有效的 IPv4 地址。

## validation_invalid_ipv6_address
**消息**：值不是有效的 IPv6 地址。

请提供有效的 IPv6 地址。目前，不支持 IPv6；请使用 IPv4 地址。

## validation_invalid_field_type
**消息**：值类型与字段类型不匹配。

要解决此问题，请确保对于要调用的端点，请求的内容符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## validation_max_value
**消息**：为参数提供的值大于允许值。

请提供较小值，以符合[规范](https://{DomainName}/apidocs/vpc-on-classic){: new_window}所给定的最大值要求。

## validation_min_value
**消息**：为参数提供的值小于允许值。

如果尝试创建 `total_ipv4_address_count` 小于 8 的子网，那么可能会收到此错误。

请提供较大值，以符合[规范](https://{DomainName}/apidocs/vpc-on-classic){: new_window}所给定的最小值要求。

## validation_not_null
**消息**：提供的字段必须为 null。

请确保值为 null。请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

下面是无效示例：

```json
{
    "name": null
}
```

## validation_only_one
**消息**：值必须与其中一个子模式相匹配。

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。

## validation_discriminator_forbidden
**消息**：鉴别器字段禁止此子结构。

例如，如果指定
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

协议为 `icmp`，_port_min_ 为 `tcp` 字段，因此您会收到错误。
1. validation_discriminator_required 表示缺少 `icmp` 规则
2. validation_discriminator_forbidden 表示 `tcp` 字段指定了 `icmp`

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_internal_error
**消息**：无

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_invalid_name
**消息**：值不是有效名称。

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_invalid_ref
**消息**：值不是有效的 HREF。

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_non_empty_uuid
**消息**：无

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_required_field
**消息**：缺少必需字段。

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_unique_failed
**消息**：提供的字段必须唯一。

您指定的名称可能已在使用。请尝试其他值。

请确保请求符合 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}中的要求。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## volume_attachment_delete_invalid_request
**消息**：无法删除卷连接。

不允许执行此操作。无法删除引导卷的卷连接，因为实例需要引导卷。

## volume_attachment_update_invalid_request
**消息**：无法更新卷连接，因为请求无效。

由于以下任一原因，无法更新卷连接：

* 无法重命名引导卷的卷连接
* 引导卷的卷连接 `delete_volume_on_instance_delete` 字段无法设置为 `true`

## volume_capacity_max
**消息**：允许的最大卷容量为 2000 GB。

指定的卷容量超过最大卷容量。请指定 10 到 2000 GB 之间的值。请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文档，以获取定制概要文件中可接受的容量范围。

## volume_capacity_missing
**消息**：请求中缺少必需的卷容量值。

创建卷时，必须根据此概要文件定义来指定卷容量。有关更多信息，请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文档。

## volume_capacity_zero_or_negative
**消息**：卷容量应该大于零。

创建卷时，请求中指定的容量值必须是 10 GB 到 2000 GB 之间的正数。请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文档，以获取支持的卷容量值。

## volume_crn_account_id_mismatch
**消息**：请求中指定的 CRN 不属于当前用户的帐户。

Key Protect 根密钥 CRN 与 IAM 授权帐户标识不匹配。请为 IAM 帐户指定其他 Key Protect 根密钥 CRN。请参阅 [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) 文档，以获取更多信息。

## volume_crn_cname_mismatch
**消息**：请求中指定的 CRN 无法用于当前环境。

请求中指定的 CRN 无法在当前环境中使用。请提供适用于您环境的 CRN。

## volume_encryption_key_crn_invalid
**消息**：请求中指定的卷加密密钥 CRN 无效。

提供的 Key Protect CRN 无效。请获取 Cloud Identity and Access Management 服务中根密钥的 CRN。有关更多信息，请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption) 文档。

## volume_encryption_key_not_active
**消息**：请求中指定的卷加密密钥不处于活动状态。

激活现有密钥或提供新密钥。如果您无法激活自己的密钥，请联系系统管理员或[客户支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## volume_encryption_key_not_authorized
**消息**：您无权访问提供的卷加密密钥。

请提供您有权访问的其他加密密钥，或联系系统管理员以获取对当前密钥的访问权。

## volume_encryption_key_not_implemented
不支持卷加密密钥功能。

无法使用加密密钥来创建卷。此版本仅支持使用提供者管理的加密的卷。要使用您自己的加密密钥，请使用支持客户管理的加密的 Block Storage for VPC 版本。请联系[客户支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)，以获取更多信息。

## volume_id_invalid
**消息**：请求参数中指定的卷标识无效。

指定的卷标识的格式不正确。请验证是否正确输入了卷标识，然后重试。如果不确定卷标识，请指定 `ibmcloud is volumes`（在 CLI 中）或 `GET volumes`（在 API 中），并搜索卷列表。

## volume_id_missing
**消息**：请求参数中缺少卷标识。

获取特定卷时，必须在请求参数中提供卷标识。

## volume_id_not_found
**消息**：找不到卷。卷标识 `<volume_id>` 不存在，其中 `<volume_id>` 是提供的卷标识。

指定的卷标识不存在。请提供有效的卷标识。

## volume_iops_zero_or_negative
**消息**：卷 IOPS 应该大于零。

创建卷时，请求中指定的 IOPS 值必须是正数。请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文档，以获取支持的 IOPS 值。

## volume_name_duplicate
**消息**：卷名重复。请求中提供的卷名 `<volume_name>` 已存在，其中 `<volume_name>` 是提供的卷名。

请求中指定的卷名已存在。请提供当前未使用的卷名。

## volume_not_available
**消息**：卷不可用。只能修改处于 available 状态的卷。当前卷 `<volume_id>` 的状态为 `<volume_status>`，其中 `<volume_id>` 是请求中提供的卷标识，`<volume_status>` 是当前卷状态。

仅当卷处于 `available` 状态时，才能修改该卷。在修改卷之前，请确保卷处于 available 状态。

## volume_not_deletable
**消息**：删除失败。

仅当卷处于 `available` 或 `failed` 状态时，才能删除该卷。确保要删除的卷处于有效的状态。

## volume_not_found
**消息**：找不到具有指定标识的卷。

请验证输入的卷标识是否正确，然后重试。要获取卷的列表，请指定 `ibmcloud is volumes`（在 CLI 中）或 `GET volumes`（在 API 中）。

## volume_not_implemented
**消息**：此发行版中不支持请求的操作。

请联系[客户支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)，以获取更多信息。

## volume_profile_capacity_iops_invalid
**消息**：对于提供的容量和/或 IOPS，请求中指定的卷概要文件无效。

卷概要文件不允许使用请求中指定的卷容量和/或 IOPS 值。请参阅 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文档，以获取定制概要文件的容量和 IOPS 的有效最小值和最大值。

## volume_profile_iops_invalid
**消息**：请求中指定的卷概要文件无法接受定制 IOPS。

卷概要文件不接受定制 IOPS 值。您可能指定的是分层 IOPS 概要文件，这种概要文件无需指定 IOPS 值。如果要提供特定 IOPS 值，请使用定制 IOPS 概要文件。

## volume_profile_name_missing
**消息**：请求中缺少必需的卷概要文件名称。

创建卷时，请求主体中缺省卷概要文件名称，或者获取卷时，请求参数中缺少卷概要文件名称。请在请求中提供卷概要文件名称，然后重试。如果您不知道卷概要文件名称，请在 CLI 中指定 `ibmcloud is volume-profiles` 或者在 API 请求中使用 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`，并搜索列表。

## volume_profile_not_found
**消息**：找不到具有指定名称的卷概要文件。

找不到具有该名称的卷概要文件。请验证是否提供了正确的卷概要文件名称。如果您不知道卷概要文件名称，请在 CLI 中指定 `ibmcloud is volume-profiles` 或者在 API 请求中使用 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`，并搜索列表。

## volume_quota_reached
**消息**：已达到卷配额。

您已超出当前帐户的配额。请尝试删除一些卷，或者联系[客户支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)以增加配额。请参阅相关文档，以获取有关[虚拟私有云基础架构配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas)的信息。

## volume_resource_group_id_invalid
**消息**：请求中指定的资源组标识无效。

请求中指定的资源组标识无效。请验证资源组标识是否正确。在 CLI 中，使用 `ibmcloud is volume VOLUME_ID` 命令。生成的信息将包含资源组和资源组标识。请参阅 [IBM Cloud CLI for VPC 参考](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)，以获取更多信息。

## volume_resource_group_not_authorized
**消息**：无权对指定的资源组执行当前操作。

您无权列出指定资源组中的卷或在该组中创建卷。请使用您对其具有许可权的其他资源组，或者向管理员请求许可权，以获取对资源组的列出和创建特权。

## volume_resource_group_not_found
**消息**：找不到具有指定标识的资源组。

找不到资源组标识，因为输入的资源组标识可能错误，或者该标识不存在。请验证资源组标识是否正确。在 CLI 中，使用 `ibmcloud is volume VOLUME_ID` 命令。生成的信息将包含资源组和资源组标识。请参阅 [IBM Cloud CLI for VPC 参考](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)，以获取更多信息。

## volume_start_not_found
**消息**：找不到其标识指定为页面开始参数的卷。

找不到在“列出卷”调用的开始参数中指定的卷标识。请验证卷标识是否正确，以及您是否有权访问该卷。

## volume_status_not_available
**消息**：具有指定标识的卷不可用。

卷不处于 Available 状态；它可能处于 Pending 状态。请等待卷变为可用，然后重试。如果正在删除该卷，请使用其他卷。要检查卷的状态，请在 CLI 中指定 `ibmcloud is volume VOLUME_ID`，或在 API 请求中使用 `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD`。

## volume_still_attached
**消息**：卷仍连接到实例。

无法删除仍连接到 VSI 的卷。如果为卷设置了自动删除，请等待 VSI 删除，这将拆离并删除卷。如果未设置自动删除，请手动拆离卷。

## volume_template_invalid
**消息**：提供的卷模板无效。

提供的用于创建卷的卷模板无效。请参阅 [VPC API 参考](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以验证是否在请求主体中提供了正确的参数。

## volume_update_invalid_request
**消息**：卷更新请求无效。

指定的更新请求主体属性无效。请参阅 [VPC API 参考](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以获取相关信息以及正确的 API 请求语法。

## vpc_not_empty
**消息**：无法删除 VPC，因为它不为空。

必须先删除 VPC 中的所有资源，然后才能删除 VPC。VPC 包含实例、子网、公共网关、负载均衡器和 VPN 网关，其中一些资源还包含其他资源。请参阅[如何删除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 文档，以获取详细信息。

如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpc_resource_separation
**消息**：资源位于不同的 VPC 中。

资源必须位于同一 VPC 中。例如，如果尝试将公共网关连接到子网，那么公共网关必须与子网位于同一 VPC 中。

要确定哪个 VPC 包含公共网关，请运行 `GET /public_gateways/{id}` 命令，然后查看“vpc”值。等效 CLI 命令为 `ibmcloud is public-gateway <id>`。

## vpn_connection_cidr_duplicated
**消息**：`<cidr_type>` CIDR 块具有重复的 CIDR 块 `<cidr_blocks>`。

创建连接时，提供了重复的 CIDR 块。请确保 CIDR 块唯一。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_created
**消息**：无法将 CIDR 块添加到 VPN 连接 `<vpn_connection_id>`。

请提供符合规范所给定需求的有效 CIDR。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_deleted
**消息**：无法从 VPN 连接 `<vpn_connection_id>` 删除 CIDR 块。

请提供连接上存在的有效 CIDR。连接必须至少具有一个本地和同级 CIDR。

要查看连接的 CIDR 块，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `local_ciddrs` 和 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**消息**：在 VPN 连接 `<vpn_connection_id>` 中找不到 CIDR 块。

请提供连接中的有效 CIDR。

要查看连接的 CIDR 块，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `local_ciddrs` 和 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**消息**：无法更新 VPN 连接 `<vpn_connection_id>` 中的 CIDR 块。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_valid
**消息**：CIDR 块 `<cidr_block>` 未表示有效地址。

值必须是未设置主机位的有效内部 CIDR。

要查看连接的 CIDR 块，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `local_ciddrs` 和 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**消息**：CIDR 块 `<cidr_block_1>` 与 `<cidr_block_2>` 重叠。两个同级 CIDR 块不能在同一 VPC 上重叠，两个本地 CIDR 块不能在同一连接上重叠。

请提供符合规范所给定需求的有效 CIDR。

要查看连接的 CIDR 块，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `local_ciddrs` 和 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**消息**：名称 `<vpn_connection_name>` 已由 VPN 连接 `<vpn_connection_id>` 使用。

请提供其他连接名称。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_invalid_name
**消息**：名称 `<vpn_connection_name>` 不是有效的 VPN 连接名称。

有效的连接名称以字母开头，后跟字母、数字、下划线或连字符。

## vpn_connection_invalid_psk_format
**消息**：预先共享密钥 (PSK) `<psk>` 的格式无效。

有效 PSK 只应包含 6 到 128 个字符，这些字符可以是字母、数字、`-`、`+`、`&`、`!`、`@`、`#`、`$`、`%`、`^`、`*`、`(`、`)`、`,`、`.`、`:` 或 `_`。
## vpn_connection_local_cidrs_required
**消息**：创建 VPN 连接时，至少需要一个本地 CIDR 块。

请提供符合规范所给定需求的有效本地 CIDR。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connection_local_subnets_quota_exceeded
**消息**：VPN 网关 `<vpn_gateway_id>` 的各 VPN 连接上的本地子网已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看各 VPN 连接上的当前本地子网，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API，然后检查每个连接的 `local_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**消息**：VPN 连接的本地子网已达到配额。

每个资源的配额在[配额](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}页面上指定。

要查看某个 VPN 连接的当前本地子网，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `local_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**消息**：无法创建 VPN 连接 `<vpn_connection_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_deleted
**消息**：无法删除 VPN 连接 `<vpn_connection_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_found
**消息**：找不到 VPN 连接 `<vpn_connection_id>`。

请检查连接标识是否正确。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_updated
**消息**：无法更新 VPN 连接 `<vpn_connection_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_pair_cidrs_duplicated
**消息**：至少一对使用相同同级网关的本地 CIDR 和同级 CIDR 重复。

此网关上存在另一个连接具有与此连接匹配的本地 CIDR 和同级 CIDR。请提供有效的一组本地/同级 CIDR。

## vpn_connection_peer_cidrs_required
**消息**：创建 VPN 连接时，至少需要一个同级 CIDR 块。

请提供符合规范所给定需求的有效同级 CIDR。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connection_peer_subnets_quota_exceeded
**消息**：VPN 网关 `<vpn_gateway_id>` 的各 VPN 连接上的同级子网已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看各 VPN 连接上的当前同级子网，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API，然后检查每个连接的 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**消息**：VPN 连接的同级子网已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看某个 VPN 连接的当前同级子网，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，然后检查 `peer_cidrs` 字段。等效 CLI 命令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**消息**：无法为同级 CIDR 块 `<peer_cidr>` 添加静态路由。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_static_route_not_deleted
**消息**：无法除去同级 CIDR 块 `<peer_cidr>` 的静态路由。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_update_cidrs_not_permitted
**消息**：更新连接时，无法更改 CIDR 块。创建或删除 CIDR 块时，请使用正确的 API。

请提供符合规范所给定需求的有效请求值。

有关解决此问题的进一步指示信息，请参阅 [API 文档](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connections_quota_exceeded
**消息**：无法创建 VPN 连接，因为 VPN 网关 `<vpn_gateway_id>` 已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看 VPN 网关的当前 VPN 连接，请使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API。等效 CLI 命令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**消息**：名称 `<vpn_gateway_name>` 已由 VPN 网关 `<vpn_gateway_id>` 使用。

请提供其他 VPN 网关名称。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_initialization_error
**消息**：无法初始化 VPN 网关。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_invalid_name
**消息**：名称 `<vpn_gateway_name>` 不是有效的 VPN 网关名称。

有效的 VPN 网关名称以字母开头，后跟字母、数字、下划线或连字符。

## vpn_gateway_invalid_state
**消息**：对于请求的操作，VPN 网关 `<vpn_gateway_id>` 处于无效状态。

VPN 网关必须处于 `available` 状态，然后才能对其进行操作。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_ip_create_error
**消息**：无法创建 VPN 网关的 IP 地址。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_missing_subnet_id
**消息**：找不到给定 VPN 网关模板的子网标识。

**Subnet ID** 是必需字段。请在命令中提供子网标识。

## vpn_gateway_missing_name
**消息**：找不到给定 VPN 网关模板的名称。

**name** 是必需字段。请在命令中提供名称。

## vpn_gateway_not_created
**消息**：无法创建 VPN 网关 `<vpn_gateway_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_deleted
**消息**：无法删除 VPN 网关 `<vpn_gateway_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_found
**消息**：找不到 VPN 网关 `<vpn_gateway_id>`。

请检查 VPN 网关标识是否正确。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_updated
**消息**：无法更新 VPN 网关 `<vpn_gateway_id>`。

请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_subnet_not_found
**消息**：找不到给定 VPN 网关的子网 `<subnet_id>`。

您引用了不存在的子网。请复查请求，以确保指定的子网标识正确。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_subnet_status_error
**消息**：由于子网状态，无法创建 VPN 网关。

请提供处于 `available` 状态的子网。请在几分钟后重试。如果此问题仍然存在，请[联系支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateways_quota_exceeded
**消息**：无法创建 VPN 网关，因为帐户和/或区域已达到配额。

每个资源的配额在[配额](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}页面上指定。

要查看当前 VPN 网关，请使用 `GET /vpn_gateways` API。等效 CLI 命令：`ibmcloud is vpn-gateways`

## zone_inconsistency
**消息**：资源必须属于同一专区。

* 连接卷时，确保卷和实例位于同一专区中。
* 关联浮动 IP 时，确保浮动 IP 和子网位于同一专区中。
