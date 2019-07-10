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

# IBM Cloud Virtual Private Cloud API 錯誤訊息
{: #rias-error-messages}

當您收到來自 {{site.data.keyword.cloud}} Virtual Private Cloud API 的錯誤訊息時，可以使用訊息 ID 來尋找如何解決問題的相關資訊。
{:shortdesc}

## account_type_invalid
**訊息**：您必須具有隨收隨付制帳戶才能佈建 Virtual Private Cloud。

只有隨收隨付制方案的帳戶才能佈建 VPC。您可以在下面找到有關升級帳戶的更多詳細資料：[升級帳戶](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**訊息**：無法刪除網路 ACL，因為它連接至資源。

無法刪除網路 ACL，因為此 ACL 連接至子網路或 VPC。

若要查看子網路是否正在使用網路 ACL，請使用 `GET /v1/subnets?version=2019-05-31&generation=1` API。相等於 CLI 指令：`ibmcloud is subnets`。若要更新子網路使用的網路 ACL，請使用 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": "{network_acl_id}"  } }’` API 或 `ibmcloud is subnet-update` CLI。

若要查看 VPC 是否正在使用網路 ACL，請使用 `GET /v1/vpcs?version=2019-05-31&generation=1` API。相等於 CLI 指令：`ibmcloud is vpcs`。無法更新或刪除 VPC 使用的預設網路 ACL。刪除 VPC 時，會自動刪除預設網路 ACL。

## address_prefix_conflict
**訊息**：具有此 CIDR 的位址字首正在使用中。

所要求位址字首的 CIDR 與現有位址字首衝突。

若要檢視 VPC 的位址字首的清單，請使用 `GET /vpcs/{vpc-id}/address_prefixes` API，然後檢查要求的 CIDR 位址是否未由其他位址字首的 `cidr` 欄位使用。相等於 CLI 指令：`ibmcloud is vpc-address-prefix`。

## address_prefix_in_use
**訊息**：無法刪除使用中的位址字首。

一個以上的子網路正在使用位址字首。若要確定哪些子網路在使用該位址字首，請使用 `GET /v1/subnets?version=2019-05-31&generation=1` API 指令，然後檢查 `ipv4_cidr_block` 欄位。相等於 CLI 指令：`ibmcloud is subnets`，然後檢查 `Subnet CIDR` 直欄。必須先刪除使用該位址字首的所有子網路，然後才能刪除該位址字首。

## backend_service_unavailable
**訊息**：後端服務無法使用。

VPC 使用的後端雲端服務無法回應。VPC 使用了多個 IBM Cloud 服務，例如：

- [Identity and Access Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [全球型錄](https://{DomainName}/catalog)
- 資源控制器
- 資源管理程式

您可以檢查 IBM Cloud 服務的[狀態](https://{DomainName}/status)，並在幾分鐘後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## bad_field
**訊息**：應該提供正確的實例 UUID

要求中提供的某個值不正確。請檢查傳回的錯誤中的 `target` 值，以取得有關哪個參數不正確的線索。在某些情況下，系統找不到要求中的 UUID 或磁區。請提供有效值並重試。

如需其他說明，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## bad_request
**訊息**：給定的資訊無效、格式錯誤，或遺漏必要欄位。

要求不符合預期要求。請使用 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以協助設定要求的格式。

如果遵循了規格，但仍收到錯誤，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## classic_access_vpc_conflict_duplicate_res
**訊息**：每個地區只能建立一個使用標準存取的 VPC。

每個地區只能建立一個使用標準存取的 VPC。若要列出使用標準存取的 VPC，請執行 `ibmcloud is vpcs --classic-access`。必須先刪除使用標準存取的現有 VPC，然後才能建立另一個使用標準存取的 VPC。

## classic_access_vpc_account_not_VRF_enabled
**訊息**：帳戶必須啟用 VRF，才能建立標準存取 VPC。

鏈結的標準帳戶未啟用 VRF。標準存取 VPC 需要鏈結的標準帳戶才能啟用 VRF。若要對帳戶啟用 VRF，請參閱 [IBM Cloud 上的 VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)。

## default_address_prefix_not_found
**訊息**：找不到預設位址字首。

找不到預設位址字首時，您可能會看到此錯誤訊息。

## duplicate_error
**訊息**：提供的輸入已存在。

指定的資源已存在。請嘗試對要建立的資源使用其他名稱。例如，建立新的 VPC 時，可以透過執行 `ibmcloud is vpcs` 列出現有 VPC。請選擇不衝突的名稱。

## floating_ip_in_use
**訊息**：浮動 IP 正在使用中。

此浮動 IP 已與虛擬伺服器實例或公用閘道相關聯。

若要查看使用浮動 IP 的位置，請使用 `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` API，然後查看 "target" 值。相等於 CLI 指令：`ibmcloud is floating-ip FLOATINGIP_ID`。

## gateway_too_many
**訊息**：VPC 中每個區域都只能有一個公用閘道。

VPC 中允許每個區域都只有一個公用閘道，但可以將一個公用閘道連接至區域中的多個子網路。若要尋找區域的公用閘道，請執行 GET public_gateways API，並查看 "vpc" 及 "zone" 值。如果使用 CLI，您可以執行 `ibmcloud is public-gateways` 指令，並查看 "VPC" 及 "Zone" 值。

使用公用閘道 ID 將其連接至子網路，請參閱 [API 範例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-)中的範例。如果使用 CLI，您可以使用 subnet update 指令將它連接至公用閘道，例如，`ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`。

## health_monitor_invalid_delay
**訊息**：指定的性能監視器延遲 <health_monitor_delay> 無效。

請為 `health monitor delay` 參數提供 2 到 60 之間的值。

## health_monitor_invalid_max_retries
**訊息**：指定的性能監視器最大重試次數 <health_monitor_max_retries> 無效。

請為 `health max retries` 提供 1 到 10 之間的值。

## health_monitor_invalid_timeout
**訊息**：指定的性能監視器逾時 <health_monitor_timeout> 無效。

請為 `health timeout` 提供 1 到 59 之間的值。所選值必須小於 `health monitor delay` 的值。

## health_monitor_invalid_url_path
**訊息**：指定的性能 URL 路徑 <health_monitor_url> 無效。

請為性能監視器提供有效的 URL。URL 可以包含英數字元、連字號和句點。不容許使用空格或反斜線。URL 路徑的最大長度為 255 個字元。

## health_monitor_invalid_protocol
**訊息**：指定的性能監視器通訊協定 <health_monitor_protocol> 無效。

請為性能監視器通訊協定提供有效值。可接受的值為 `http` 和 `tcp`。

## health_monitor_missing_protocol
**訊息**：缺少性能監視器通訊協定。

性能監視器通訊協定是必要欄位。在要求中，指定性能監視器通訊協定。可接受的值是 `http` 及 `tcp`。

## health_monitor_missing_url_path
**訊息**：缺少性能監視器 URL 路徑。對於 HTTP 類型的性能監視器，此項是必要的。

對於使用 HTTP 通訊協定的性能監視器，性能監視器 URL 路徑是必要欄位。請提供有效的 URL 路徑。

## health_monitor_timeout_delay_conflict
**訊息**：性能監視器逾時 <health_monitor_timeout> 不應大於或等於延遲 <health_monitor_delay>。

`health monitor delay` 參數的值必須大於 `health monitor timeout` 的值。

## http_request_size_exceeded
**訊息**：HTTP 要求太大。

您在要求中傳送的有效負載包含太多字元時，會發生此問題。請使用較小的有效負載重試。例如，嘗試在一個要求中建立最少資源，然後在數個後續要求中漸進地附加其狀態，而不是嘗試在單一要求中執行所有作業。

如需其他說明，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## iam_failure
**訊息**：無

IAM 服務中發生了故障，正在驗證鑑別或授權。IAM 服務中斷可能是暫時的。請在幾分鐘之後重試要求。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policies_quota_exceeded
**訊息**：無法建立 IKE 原則，因為帳戶已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視現行 IKE 原則，請使用 `GET /ike_policies` API。相等於 CLI 指令：`ibmcloud is ike-policies`

## ike_policy_duplicate_name
**訊息**：IKE 原則 `<ike_policy_id>` 已使用名稱 `<ike_policy_name>`。

請提供其他 IKE 原則名稱。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_in_use
**訊息**：一個以上的連線正在使用 IKE 原則 `<ike_policy_id>`。

如果一個以上的連線正在使用 IKE 原則，則無法將其刪除。

若要查看正在使用 IKE 原則的連線，請使用 `GET /ike_policies/<ike_policy_id>/connections` API。相等於 CLI 指令：`ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**訊息**：名稱 `<ike_policy_name>` 不是有效的 IKE 原則名稱。

有效的 IKE 原則名稱以字母開頭，後面接著字母、數字、底線或連字號。

## ike_policy_not_created
**訊息**：無法建立 IKE 原則 `<ike_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_deleted
**訊息**：無法刪除 IKE 原則 `<ike_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_found
**訊息**：找不到 IKE 原則 `<ike_policy_id>`。

您參照的 IKE 原則不存在。請檢閱您的要求，確定您已指定適當的 IKE 原則 ID。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ike_policy_not_updated
**訊息**：無法更新 IKE 原則 `<ike_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_delete_conflict
**訊息**：無法刪除處於現行狀態的實例。

無法刪除處於 `deleting`、`pending`、`starting`、`stopping` 或 `restarting` 狀態的實例。實例必須處於 `running` 或 `stopped` 狀態，才能將其刪除。如果狀態保持不變，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_invalid_hostname
**訊息**：無法建立實例，因為主機名稱無效。

實例主機名稱必須符合下列需求：
* 主機名稱只能包含英數字元和橫線
* 不容許使用大寫字元
* 不容許使用前導和尾端橫線
* 不容許使用前導數字
* 不容許使用連續的橫線
* 對於 Windows，最大長度為 15 個字元
* 對於其他作業系統，最大長度為 40 個字元

## instance_invalid_port_speed
**訊息**：無法建立實例，因為指定的網路介面埠速度無效。

網路介面埠速度必須為 `100` 或 `1000` Mbps。

## instance_name_exists
**訊息**：相同的實例名稱已存在。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## instance_sec_volume_over_quota
**訊息**：無法建立實例，因為磁區連接數將超出配額。

每個資源的 VPC 配額在[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}頁面上指定。

## instance_too_many_keys
**訊息**：無法建立 Windows 實例，因為要求包含多個金鑰。

Windows 實例僅支援一個金鑰。

## insufficient_space_for_subnet
**訊息**：位址字首中子網路的空間不足。

無法建立子網路，因為無法配置所要求的位址數。請使用更小的位址計數或更大的 CIDR 進行重試。

## internal_error
**訊息**：發生內部錯誤。

發生非預期的錯誤。此問題可能是暫時的。請在幾分鐘之後重試要求。如果此錯誤持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_server_error
**訊息**：無

發生非預期的錯誤。此問題可能是暫時的。請在幾分鐘之後重試要求。如果此錯誤持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## internal_solution
**訊息**：請與管理者聯絡。

發生非預期的錯誤。此問題可能是暫時的。請在幾分鐘之後重試要求。如果此錯誤持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## invalid_generation_parameter
**訊息**：generation 查詢參數必須設定為 1。

對於 2019 年 5 月 31 日及之後的版本，`generation` 查詢參數必須設定為 1，以容許發出 VPC on Classic API 要求。

## invalid_id_format
**訊息**：不當的 ID 格式。請確定格式正確無誤。

請確定您提供的 ID 未包含任何形態異常的資料。

如果在進行分頁要求時提供形態異常的起始查詢，您可能會收到此錯誤訊息。例如，`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` 包含兩個 `?`。請修正查詢，然後再試一次。

## invalid_route
**訊息**：要求的路徑不存在。

所提供的 API URL 上要求的路徑不存在。請驗證指定用於呼叫 API 端點的 URL 是否正確。

## invalid_request_field
**訊息**：要求中提供的欄位無效。

例如，要更新子網路使用的網路 ACL，請使用 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": "{network_acl_id}"  } }’` API。
下列要求無效，因為 "networkacl" 不是有效欄位：`PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": "{network_acl_id}"  } }’`

請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以取得可接受的值。

## invalid_state
**訊息**：資源的現行狀態不支援對資源要求的動作。

如果資源是實例：

1. 可能已經在對實例執行重新開機作業。請參閱[容許的動作](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance)，其取決於實例的狀態。

2. 實例的狀態為 `pending` 時，無法執行下列動作：
  * 刪除實例
  * 將磁區連接到實例
  * 從實例分離磁區
  * 更新實例

請稍後再重試動作。如果資源的狀態保持不變，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

如果資源是映像檔：

映像的狀態為 `deleting` 時，無法執行下列動作：
  * 修補映像檔
  * 刪除映像檔

不要再次發出該要求。映像檔刪除作業將在其自己的時間內完成。刪除作業完成後，對該映像檔的所有要求都會失敗，並傳回錯誤 `not_found`。

## invalid_version
**訊息**：`version` 參數無效，其格式必須為 `YYYY-MM-DD`。

版本必須符合格式 _YYYY-MM-DD_。對於 1 月 1 日這類單一位數的月份或日期，版本應該類似於 `2019-01-01`。URL 中必須有版本參數。版本參數中給定的日期必須晚於 2019-01-01 但早於現行日期。

## invalid_version_range
**訊息**：`version` 值不能設定在未來日期，也不能早於 `2019-01-01`。

版本參數中給定的日期必須晚於 2019-01-01 但早於現行日期。

## invalid_zone
**訊息**：請檢查您要求的資源是否位於相同區域中。

試圖將 zon-1 中的公用閘道連接到 zone-2 中的子網路時，可能會看到此訊息。

## ipsec_policies_quota_exceeded
**訊息**：無法建立 IPsec 原則，因為帳戶已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視現行 IPsec 原則，請使用 `GET /ipsec_policies` API。相等於 CLI 指令：`ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**訊息**：IPsec 原則 `<ipsec_policy_id>` 已使用名稱 `<ipsec_policy_name>`。

請提供其他 IPsec 原則名稱。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_in_use
**訊息**：一個以上的連線正在使用 IPsec 原則 `<ipsec_policy_id>`。

如果一個以上的連線正在使用 IPsec 原則，則無法將其刪除。

若要查看正在使用 IPsec 原則的連線，請使用 `GET /ipsec_policies/<ipsec_policy_id>/connections` API。相等於 CLI 指令：`ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**訊息**：名稱 `<ipsec_policy_name>` 不是有效的 IPsec 原則名稱。

有效的 IPsec 原則名稱以字母開頭，後面接著字母、數字、底線或連字號。

## ipsec_policy_not_created
**訊息**：無法建立 IPsec 原則 `<ipsec_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_deleted
**訊息**：無法刪除 IPsec 原則 `<ipsec_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_found
**訊息**：找不到 IPsec 原則 `<ipsec_policy_id>`。

您參照的 IPsec 原則不存在。請檢閱要求，並指定有效的 IPsec 原則 ID。如果您確定該原則存在，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## ipsec_policy_not_updated
**訊息**：無法更新 IPsec 原則 `<ipsec_policy_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_content_exists
**訊息**：相同的金鑰內容已存在。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_name_exists
**訊息**：相同的金鑰名稱已存在。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## key_invalid_name
**訊息**：無法建立金鑰，因為名稱無效。

金鑰名稱必須符合下列需求：
* 必須以字母字元 [A-Za-z] 開頭
* 只能包含英數字元、橫線或底線：[-A-Za-z0-9_]
* 長度不能超過 65 個字元

## key_invalid_type
**訊息**：無法建立金鑰，因為類型無效。

唯一支援的金鑰類型是 `rsa`。

## key_parse_failure
**訊息**：無法建立金鑰，因為公開金鑰值無效。

要求中提供的公開金鑰無效。請檢查該值或重新產生公開金鑰，然後重試。

在 Linux 作業系統中，可以執行下列指令來驗證公開金鑰是否有效：`ssh-keygen -l -v -f <key_file>`。

## listener_certificate_not_found
**訊息**：找不到具有 CRN `<listener_certificate_crn>` 的憑證實例，或沒有可存取憑證實例的許可權。

請提供現有憑證實例 CRN，或要求帳戶管理者授與您對所提供憑證實例的存取權。

## listener_duplicate_port
**訊息**：另一個接聽器實例已使用埠 `<listener_port>`。請選擇不同的埠。

請選擇不同的埠。

## listener_invalid_certificate
**訊息**：HTTPS 接聽器的憑證實例 CRN 無效。

請提供有效的憑證實例 CRN。

## listener_invalid_connection_limit
**訊息**：接聽器連線限制 `<listener_connection_limit>` 無效。

請提供有效的連線限制。連線限制必須是 1 到 15000 之間的整數。

## listener_invalid_port
**訊息**：接聽器埠 `<listener_port>` 無效。

請選擇不同的埠。

## listener_invalid_protocol
**訊息**：接聽器通訊協定 `<listener_protocol>` 無效。

請提供有效的接聽器通訊協定。接聽器通訊協定的可接受值為 `http`、`https` 及 `tcp`。

## listener_missing_certificate_crn
**訊息**：遺漏 `https` 接聽器的憑證實例 CRN。

憑證實例的 CRN 是必要欄位。

## listener_missing_port
**訊息**：遺漏接聽器埠。

接聽器埠是必要欄位。

## listener_missing_protocol
**訊息**：遺漏接聽器通訊協定。

接聽器通訊協定是必要欄位。請在您的要求中提供接聽器通訊協定。可接受的值是 `http`、`https` 及 `tcp`。

## listener_not_found
**訊息**：找不到 ID 為 `<listener_id>` 的接聽器。

請提供現有的接聽器 ID。

## listener_over_quota
**訊息**：無法建立接聽器。負載平衡器資源的接聽器配額已達到上限。

[VPC 的配額及限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定了每個資源的配額。

## listener_pool_protocols_conflict
**訊息**：接聽器通訊協定 (`<listener_protocol>`) 與儲存區通訊協定 (`<pool_protocol>`) 衝突。

具有 `https` 或 `http` 通訊協定的接聽器只能與具有 `http` 通訊協定的儲存區相關聯。同樣地，`tcp` 接聽器只能與 `tcp` 儲存區相關聯。

## listener_reserved_port_conflict
**訊息**：接聽器埠 `<listener_port>` 是其中一個保留埠。埠範圍 56500 到 56520 保留供管理用途。請選擇不同的埠。

請選擇不同的埠。

## load_balancer_delete_conflict
**訊息**：無法刪除 ID 為 <load_balancer_id> 的負載平衡器，因為它處於下列其中一個狀態：  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

請在負載平衡器處於 ACTIVE 狀態時，再將其刪除。

## load_balancer_duplicate_name
**訊息**：另一個負載平衡器實例已使用名稱 `<load_balancer_name>`。請選擇不同的名稱。

請為負載平衡器實例提供不同的名稱。負載平衡器名稱在 VPC 和帳戶中應該是唯一的。

Load Balancer for VPC 不會與 Cloud Load Balancer 相衝突，因為這兩個服務的 DNS 網域名稱不同，並且 DNS 名稱中不包含 Load Balancer for VPC 的名稱。
{: note}

## load_balancer_insufficient_ips
**訊息**：ID 為 <subnet_id> 的子網路的可用 IPv4 位址不足。

在要求中，提供要在其中建立負載平衡器且具有可用 IPv4 位址的有效子網路。

## load_balancer_invalid_description
**訊息**：負載平衡器說明 `<load_balancer_description>` 無效。

負載平衡器說明不得超出 255 個字元。

## load_balancer_invalid_is_public
**訊息**：is_public `<load_balancer_is_public>` 的值無效。

負載平衡器參數 `is_public` 的值必須為 _true_ 或 _false_。

## load_balancer_invalid_name
**訊息**：名稱 `<load_balancer_name>` 無效。

名稱不能為空。有效的負載平衡器名稱以字母開頭，其後接著字母、數字或底線。名稱長度不應該超出 40 個字元。

## load_balancer_invalid_subnet
**訊息**：ID 為 <subnet_id> 的子網路無效。請確保使用狀態為 "available" 的現有子網路。

請在輸入建立負載平衡器的要求時，提供狀態為 `available` 的有效子網路。

## load_balancer_missing_is_public
**訊息**：遺漏 'is_public' 欄位。

`is_public` 欄位是必要的。請提供 `is_public` 欄位的值。

## load_balancer_missing_name
**訊息**：遺漏負載平衡器名稱。

負載平衡器 `name` 是必要欄位。請提供負載平衡器的名稱。

## load_balancer_missing_subnets
**訊息**：遺漏負載平衡器子網路。

`subnets` 欄位是必要的。在建立負載平衡器的要求中，提供有關要在其中建立負載平衡器的子網路的資訊。

## load_balancer_missing_subnet_id
**訊息**：缺少子網路 ID。

**子網路 ID** 是必要欄位。請在指令中提供子網路 ID。

## load_balancer_not_found
**訊息**：找不到 ID 為 `<load_balancer_id>` 的負載平衡器。

請提供現有負載平衡器的 ID。

## load_balancer_over_quota
**訊息**：無法建立負載平衡器。您帳戶下的負載平衡器實例配額已達到上限。

請刪除現有負載平衡器，或與支援中心聯絡以增加您帳戶的負載平衡器配額。

[VPC 的配額及限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定了每個資源的配額。

## load_balancer_subnet_not_found
**訊息**：找不到 ID 為 <subnet_id> 的子網路。

在要求中，必須提供要在其中建立負載平衡器的現有子網路的 ID。

## load_balancer_unchanged_update
**訊息**：沒有任何項目可以更新 ID 為 `<load_balancer_id>` 的負載平衡器。

未提供用於更新具有指定 ID 的負載平衡器實例的資訊。

## load_balancer_update_conflict
**訊息**：無法更新 ID 為 <load_balancer_id> 的負載平衡器，因為它處於下列其中一個狀態：
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

請在負載平衡器處於 ACTIVE 狀態時，再對其進行更新。

## member_duplicate_address_and_port
**訊息**：儲存區中已存在位址為 <member_address> 且埠為 <member_port> 的成員。

請在要求中提供唯一的成員位址和埠組合。

## member_invalid_address
**訊息**：指定的成員 IP 位址 <member_ip> 無效。

在要求中，為成員提供有效的 IPv4 CIDR 位址。

## member_invalid_port
**訊息**：指定的成員埠 `<member_port>` 無效。

請提供有效的埠號。有效的埠號是在 1 到 65535 的範圍內。

## member_invalid_weight
**訊息**：指定的成員加權 `<member_weight>` 無效。

請提供有效的成員加權。值必須是 0 到 100 之間的整數。

## member_ip_not_found
**訊息**：IP 位址為 <member_IP> 的成員不屬於負載平衡器的地區和 VPC 中的任何子網路。

在要求中，提供屬於負載平衡器所在地區和 VPC 中子網路的成員的位址。

## member_missing_address
**訊息**：遺漏成員位址。

成員位址是必要欄位。請提供成員位址的值。

## member_missing_protocol_port
**訊息**：遺漏成員埠。

成員埠是必要欄位。請提供成員埠的值。

## member_not_found
**訊息**：找不到 ID 為 <member_id> 的成員。

請提供現有的成員 ID。

## member_over_quota
**訊息**：無法建立成員。儲存區下的成員實例配額已達到上限。

[VPC 的配額及限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定了每個資源的配額。

## missing_generation_parameter
**訊息**：generation 查詢參數是必要的。

對於 2019 年 5 月 31 日及之後的版本，在 VPC on Classic API 要求中，`generation` 查詢參數是必要的。

## missing_ims_account_id
**訊息**：無

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## missing_version
**訊息**：`version` 是必要參數，且其格式必須為 `YYYY-MM-DD`。

version 參數對於所有 API 要求都是必要的。版本必須符合格式 _YYYY-MM-DD_。對於 1 月 1 日這類單一位數的月份或日期，版本應該類似於 `2019-01-01`。版本參數中給定的日期必須晚於 2019-01-01 但早於現行日期。請查看這些 [API 範例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)，以瞭解如何提供 version 參數。

## network_conflict
**訊息**：CIDR 與 VPC 中的現有位址字首相衝突。

如果您提供與相同 IP 空間中的現有網路 CIDR 有衝突的網路 CIDR，則可能會看到此訊息。

若要在 VPC 的位址字首中尋找現有 CIDR，請執行 API 指令 `GET /v1/vpcs/<vpc-id>/address_prefixes`，然後查看回應的 `cidr` 值。檢查 `cidr` 的值，以確保所提供的 CIDR 未由其他位址字首使用。

如果使用的是 CLI，請執行 `ibmcloud is vpc-addrs <vpc-id>` 指令，然後檢查 `CIDR Block` 的值是否相衝突。

若要在子網路中尋找現有 CIDR，請執行 API 指令 `GET /v1/subnets` 來列出 VPC 的所有子網路。然後，針對 VPC 中的每個子網路，執行 API 指令 `GET /v1/subnets/<subnet-id>`，並檢查回應中 `ipv4_cidr_block` 的值。檢查 `ipv4_cidr_block` 的值，以確保所提供的 CIDR 未由其他位址字首使用。

如果使用的是 CLI，請執行 `ibmcloud is subnets` 指令來列出 VPC 的所有子網路。然後，針對 VPC 中的每個子網路，執行 `ibmcloud is subnet <subnet-id>` 指令，並檢查 CLI 輸出中的 `IPv4 CIDR` 的值是否相衝突。

## not_authorized
**訊息**：要求未獲授權。

如果缺少 IAM 記號或 IAM 記號到期，您可能會看到此錯誤。如需如何產生記號的指示，請參閱[使用 REST API 建立 VPC](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)。如果記號未到期，請確保使用的帳戶有權使用此功能，並且您具有[正確許可權](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

如果您具有正確的授權，但仍收到此錯誤，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_found
**訊息**：請檢查您要求的資源是否存在。

您參照的資源不存在，或您沒有存取權。請檢閱您的要求，確定您已指定適當的 ID 及參照。

如需其他說明，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_implemented
**訊息**：無

該方法未實作。請檢查要求，以確保呼叫的是[有效 API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果需要實作此 API，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## not_in_address_prefix
**訊息**：所提供的 CIDR 不適合所提供區域中的任何位址字首。

如果使用者嘗試以不屬於給定區域的任何位址字首的 CIDR 來建立子網路，則會發生此錯誤。

請執行 `GET /vpcs/{vpc_id}/address_prefixes` 來取得 VPC 的位址字首清單。如果使用 CLI，您可以執行 `ibmcloud is vpc-address-prefixes` 來列出 VPC 的所有位址字首。請查看回應的 `cidr` 及 `zone` 值，確定子網路的 `cidr` 是您嘗試在其中建立子網路之區域的位址字首中 `cidr` 的子集。

## over_quota
**訊息**：要求會超出資源類型的配額。

[VPC 的配額及限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定了每個資源的配額。

## password_not_ready
**訊息**：無

您的實例必須先執行，您才能擷取密碼。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## pool_delete_conflict
**訊息**：無法刪除儲存區，因為它仍與一個以上的接聽器相關聯。

請確保解除儲存區與任何接聽器的關聯，然後重試。

## pool_duplicate_name
**訊息**：另一個儲存區已使用儲存區名稱 `<pool_name>`。請選擇不同的名稱。

請選擇不同的儲存區名稱。

## pool_invalid_name
**訊息**：名稱 <pool_name> 無效。

請提供有效的儲存區名稱。有效的負載平衡器名稱以字母開頭，其後接著字母、數字或連字號。名稱長度不應該超出 40 個字元。

## pool_invalid_session_persistence
**訊息**：儲存區階段作業持續性值無效。

請提供有效的階段作業持續性值。可接受的值為 `SOURCE_IP`。

## pool_missing_algorithm
**訊息**：遺漏負載平衡演算法。

負載平衡演算法是必要欄位。請在要求中提供負載平衡演算法。可接受的值為 `round_robin`、`weighted_round_robin` 和 `least_connections`。

## pool_missing_health_monitor
**訊息**：遺漏儲存區性能監視器。

儲存區性能監視器是必要欄位。

## pool_missing_members
**訊息**：缺少儲存區成員。

請提供儲存區成員。

## pool_missing_name
**訊息**：遺漏儲存區名稱。

儲存區名稱是必要欄位。

## pool_missing_protocol
**訊息**：遺漏儲存區通訊協定。

儲存區通訊協定是必要欄位。請在要求中提供儲存區通訊協定。可接受的值是 `http` 及 `tcp`。

## pool_not_found
**訊息**：找不到 ID 為 `<pool_id>` 的儲存區。

請提供現有儲存區 ID。

## pool_over_quota
**訊息**：無法建立儲存區。負載平衡器資源的儲存區配額已達到上限。

[VPC 的配額及限制](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}中指定了每個資源的配額。

## public_gateway_in_use
**訊息**：無法刪除使用中的公用閘道。

目前已將公用閘道連接至一個以上子網路。您必須先將公用閘道與所有子網路分離，才能予以刪除。

若要查看哪個子網路在使用公用閘道，請使用 `GET /v1/subnets?version=2019-05-31&generation=1` API。相等於 CLI 指令：`ibmcloud is subnets`。若要從子網路分離公用閘道，請使用 API 指令 `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` 或 CLI 指令 `ibmcloud is subnet-public-gateway-detach`。

## rate_limit_exceeded
**訊息**：短時間內的若要求太多。

如果在指定的時間間隔內接收到太多要求，則會傳回此錯誤訊息。請等待一段時間，然後再試一次。

## security_group_active_transactions
**訊息**：除非實例顯示為「作用中」狀態，否則無法連接或分離介面。

實例必須先處於作用中狀態，才能將其網路介面連接至安全群組。請使用 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance` 來檢查實例的狀態。狀態處於 `running` 狀態之後，請重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_already_attached
**訊息**：介面已連接至安全群組。

介面已連接至安全群組。請使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 來檢視連接的介面。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_exists
**訊息**：安全群組已存在。

安全群組名稱必須在 VPC 內是唯一的。目標 VPC 中已存在具有該名稱的安全群組。請使用 `GET /v1/security_groups?version=2019-05-31&generation=1` 或 `ibmcloud is security-groups` 來檢視現行安全群組。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_interfaces_attached
**訊息**：連接介面時，無法刪除安全群組。


請先分離所有介面，再刪除安全群組。請使用 `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interface-remove` 來分離介面。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_interfaces_per_sg_exceeded
**訊息**：超出每個安全群組的介面限制。

將另一個介面連接至安全群組，會超出每個安全群組的介面限制。[VPC 的配額及限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定了每個資源的配額。請評估您的策略，並考慮建立另一個具有類似規則的安全群組。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_last_security_group_is_default
**訊息**：預設安全群組是唯一連接的安全群組時，則無法予以移除。

必須將網路介面至少連接至一個安全群組。
如果未指定安全群組，則會將它連接至 VPC 的預設安全群組。
將介面連接至不同的安全群組，然後重試以將它與預設安全群組分離。
如需預設安全群組的相關資訊，請參閱[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_limit_exceeded
**訊息**：超出安全群組限制。

您已嘗試建立新的安全群組，但目前位於您的帳戶配額。[VPC 的配額及限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定了每個資源的配額。請評估將實例指派給安全群組的策略。藉由將多個實例指派給相同的安全群組，通常可以減少整體安全群組數目。此策略將減少安全群組數目，並降到低於您的帳戶配額。在極少數情況下（通常是針對大型組織），需要擴充配額。在此情況下，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)，以查詢這是否可能。

## security_group_network_interface_not_active
**訊息**：無法連接介面，因為它非作用中。

連接至安全群組之前，網路介面必須處於作用中狀態。請使用 `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance-network-interface` 來檢視介面的狀態。狀態處於 'available' 狀態之後，請重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_not_attached
**訊息**：未連接介面。

介面未連接至安全群組。請使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 來檢視連接的介面。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。


## security_group_not_in_vpc
**訊息**：介面及安全群組位於不同的 VPC 中。

若要將網路介面連接至安全群組，實例必須位於與安全群組相同的 VPC 中。請使用 `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is security-group` 來檢視安全群組詳細資料和 VPC。使用 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 或 `ibmcloud is instance` 來檢視實例詳細資料和 VPC。

## security_group_order_bindings
**訊息**：安全群組擱置實例訂單時，即無法予以刪除。

如果是使用網路介面上指定的安全群組建立實例，則實例及網路介面必須先處於作用中狀態，才能刪除安全群組。請使用 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-network-interfaces` 來檢視連接的網路介面及其狀態。介面的狀態處於 'available' 狀態之後，請重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## security_group_port_max_less_than_port_min
**訊息**：TCP/UDP 最大埠不能小於最小埠。

埠值上限不能小於埠值下限。請指定大於埠值下限的埠值上限。

## security_group_port_range_both_or_neither
**訊息**：必須對 'tcp' 和 'udp' 取消設定埠範圍，或同時設定最小及最大埠。

具有 'tcp' 或 'udp' 通訊協定的規則可能有取消設定的埠範圍（以將規則套用至所有資料流量），或者它們可能指定埠範圍。若要將資料流量限制為單一埠，請同時將最小及最大埠設為埠值。例如，下列指令將建立規則以容許埠 22 上的 SSH：`ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`。

如需安全群組規則配置的進一步資訊，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)。


## security_group_port_range_invalid_protocol
**訊息**：已指定通訊協定為 'icmp' 的埠範圍。埠範圍僅適用於 'tcp' 或 'udp' 通訊協定。

具有 'icmp' 通訊協定的規則包含類型及代碼。具有 'tcp' 或 'udp' 通訊協定的規則包含埠範圍。如需安全群組規則配置的進一步資訊，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)。

## security_group_remote_group_not_in_vpc
**訊息**：遠端群組與此安全群組不在同一個 VPC 中。

安全群組規則可能參照遠端安全群組，以容許在兩個群組的連接介面之間進行傳輸。遠端安全群組必須在同一個 VPC 中。使用 `GET /v1/security_groups?2019-01-01` 或 `ibmcloud is security-groups`，來檢視現行安全群組及其 VPC。

如需修正此問題的進一步指示，請參閱[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。


## security_group_remoting_rules
**訊息**：連接遠端規則時，無法刪除安全群組。

安全群組包含一個以上參照遠端安全群組的規則。請使用 `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` 或 `ibmcloud is security-group-rules` 來檢視規則。'remote' 欄位將指定任何遠端安全群組。請在刪除或編輯遠端規則之後重試。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)或[使用安全群組文件](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}。

## security_group_remoting_rules_per_sg_exceeded
**訊息**：超出每個安全群組的遠端規則限制。

新增另一個參照遠端安全群組的規則，會超出每個安全群組的遠端規則限制。[VPC 的配額及限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定了每個資源的配額。

## security_group_rules_per_sg_exceeded
**訊息**：超出每個安全群組的規則限制。

新增規則會超出每個安全群組的規則限制。[VPC 的配額及限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定了每個資源的配額。請評估您的策略，並考慮建立另一個安全群組。

## security_group_sgs_per_interface_exceeded
**訊息**：超出每個介面的安全群組限制。

將此介面連接至另一個安全群組，會超出每個介面的安全群組限制。[VPC 的配額及限制](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}中指定了每個資源的配額。請評估您的策略，並考慮將規則新增至現有安全群組。


## security_group_type_code_invalid_protocol
**訊息**：已給定 'icmp' 類型/代碼，但所要求的通訊協定不是 'icmp'。請將通訊協定設為 'icmp'，或指定埠範圍。

只有具有 'icmp' 通訊協定的規則才包含類型及代碼。具有 'tcp' 或 'udp' 通訊協定的規則包含埠範圍。如需安全群組規則配置的進一步資訊，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic)。

## security_group_vpc_default
**訊息**：無法刪除 VPC 的預設安全群組。

無法刪除預設安全群組。如需預設安全群組的相關資訊，請參閱[更新預設安全群組](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group)的相關手冊。

## service_manager_service_failure
**訊息**：無

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_conflict
**訊息**：CIDR 與 VPC 中的現有「子網路」衝突。

請執行 `GET /subnets` API 來擷取 VPC 中的所有子網路。檢查 `ipv4_cidr_block` 的值，以確保所提供的 CIDR 未由其他子網路使用。

如果使用 CLI，您可以執行 `ibmcloud is subnets`，並查看 "Subnet CIDR" 值以尋找衝突。

如需其他說明，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty
**訊息**：在子網路是空的之前，無法刪除子網路。請刪除子網路中的所有資源，然後重試。

已要求刪除子網路，但子網路中仍有資源。子網路中可能有實例、VPN、負載平衡器或公用閘道。必須先刪除子網路中的資源，然後才能刪除子網路。

在某些狀況下，即使主控台顯示 VSI 和負載平衡器的數量都為 0，也可能會發生此錯誤。刪除是非同步的，而且可能需要幾分鐘的時間，才能變更內部狀態。請在幾分鐘之後重試子網路刪除。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_pgway_exists
**訊息**：在子網路連接至公用閘道時，無法予以刪除。請分離公用閘道，然後重試。

已要求刪除子網路，但子網路仍連接公用閘道。您必須先刪除或分離公用閘道，才能刪除子網路。

如果使用 CLI，您可以執行 `ibmcloud is public-gateways` 來列出公用閘道，以及執行 `ibmcloud is subnet-public-gateway-detach` 來分離公用閘道與子網路。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_ipaddr_exists
**訊息**：在子網路包含 IP 位址時，無法予以刪除。請刪除與 IP 位址相關聯的任何伺服器實例，然後重試。

已要求刪除子網路，但子網路仍包含 IP 位址。必須先刪除與 IP 位址關聯的伺服器實例，然後才能刪除子網路。

如果使用 CLI，您可以執行 `ibmcloud is instances` 來列出伺服器實例。請查看 "Address" 值以確定其 IP 位址，並查看 "Status" 以確定其狀態。執行 `ibmcloud is instance-delete` 以刪除其狀態尚不為 "deleting" 的伺服器實例。

在某些狀況下，即使主控台顯示 VSI 的數量為 0，也可能會發生此錯誤。刪除是非同步的，而且可能需要幾分鐘的時間，才能變更內部狀態。請在幾分鐘之後重試子網路刪除。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_loadbalancer_exists
**訊息**：在子網路包含負載平衡器時，無法予以刪除。請刪除負載平衡器，然後重試。

已要求刪除子網路，但子網路仍包含負載平衡器。必須先刪除負載平衡器，然後才能刪除子網路。

如果使用 CLI，您可以執行 `ibmcloud is load-balancers` 來列出負載平衡器。請查看 "Subnets" 值以確定其子網路，並查看 "Status" 以確定其狀態。執行 `ibmcloud is load-balancer-delete` 以刪除其狀態尚不為 "deleting" 的負載平衡器。

在某些狀況下，即使主控台顯示負載平衡器的數量為 0，也可能會發生此錯誤。刪除是非同步的，而且可能需要幾分鐘的時間，才能變更內部狀態。請在幾分鐘之後重試子網路刪除。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_not_empty_vpn_gway_exists
**訊息**：在子網路包含 VPN 閘道時，無法予以刪除。請刪除 VPN 閘道，然後重試。

已要求刪除子網路，但子網路仍連接 VPN 閘道。必須先刪除 VPN 閘道，然後才能刪除子網路。

如果使用 CLI，您可以執行 `ibmcloud is vpn-gateways` 來列出 VPN 閘道。請查看 "Subnets" 值以確定其子網路，並查看 "Status" 以確定其狀態。執行 `ibmcloud is vpn-gateway-delete` 以刪除其狀態尚不為 "deleting" 的 VPN 閘道。

在某些狀況下，即使主控台顯示 VPN 閘道的數量為 0，也可能會發生此錯誤。刪除是非同步的，而且可能需要幾分鐘的時間，才能變更內部狀態。請在幾分鐘之後重試子網路刪除。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## subnet_unknown_state
**訊息**：對於要求的作業，子網路 `<subnet_id>` 處於無效狀態。

子網路必須處於 `available` 狀態，然後才能對其進行操作。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## target_in_use
**訊息**：目標已連接浮動 IP。

有要求要將浮動 IP 位址連接到伺服器網路介面，但網路介面已連接該浮動 IP。

若要尋找現行連接到網路介面的浮動 IP 位址，請執行 API 指令 `GET /v1/floating_ips?version=2019-05-31&generation=1`，然後在 `target.id` 欄位中尋找網路介面 ID。  

如果使用的是 CLI，請執行 `ibmcloud is floating-ips` 指令，然後查看 `Target` 值。請注意，CLI 會在第一個橫線（ "-" ）字元處截斷網路介面 ID。例如，ID 為 `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` 的伺服器主要網路介面會顯示為 `primary(abdfcb29-.)`。

## token_invalid
**訊息**：服務記號過期或無效。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## token_missing
**訊息**：服務記號是空的，或不存在於要求中。

請重建記號，然後再試一次。

## validation_enum
**訊息**：所提供的值不是有效的選項。

所提供的其中一個欄位具有的值必須來自可能值的列舉。

對於「網路 ACL」規則，您可以指定方向：

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

例如，下列值無效，因為 `northbound` 不是列舉 `[ inbound, outbound ]` 中的有效選項。

```json
{
    "direction":"northbound"
}
```

請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}或 [CLI 參考資料](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}，以取得可接受的值。

## validation_failure
**訊息**：提供的 JSON 不符合預期的結構。

若要修正此問題，請確定您的要求內容是有效的 JSON，而且您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## validation_invalid_cidr
**訊息**：值不是有效的 CIDR。

值必須是具有 0 個主機組件的有效內部 CIDR 區塊。

特定 IP 位址範圍已保留。如需保留 IP 範圍的相關資訊，請參閱[搭配使用 VPC 與地區及子網路](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}的概觀。

## validation_invalid_ipv4_cidr
**訊息**：值不是有效的 IPv4 CIDR。

必須是具有 0 個主機組件的 IPv4 內部 CIDR 區塊。

特定 IP 位址範圍已保留。如需保留 IP 範圍的相關資訊，請參閱[搭配使用 VPC 與地區及子網路](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}的概觀。

## validation_invalid_ipv6_cidr
**訊息**：值不是有效的 IPv6 CIDR。

目前不支援 IPv6。請使用 IPv4 位址。

## validation_invalid_address
**訊息**：值不是有效的位址。

必須是有效的 IP 位址。

[地區及子網路](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)文件中提供個別保留 IP 位址的清單。

## validation_invalid_ipv4_address
**訊息**：值不是有效的 IPv4 位址。

請提供有效的 IPv4 位址。

## validation_invalid_ipv6_address
**訊息**：值不是有效的 IPv6 位址。

請提供有效的 IPv6 位址。目前不支援 IPv6；請使用 IPv4 位址。

## validation_invalid_field_type
**訊息**：值類型不符合欄位類型。

若要修正此問題，請確定您的要求內容符合所呼叫端點的 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## validation_max_value
**訊息**：為參數提供的值大於容許值。

請提供較小值，以符合[規格](https://{DomainName}/apidocs/vpc-on-classic){: new_window}所給定的最大要求。

## validation_min_value
**訊息**：為參數提供的值小於容許值。

如果嘗試建立 `total_ipv4_address_count` 小於 8 的子網路，您可能會收到此錯誤。

請提供較大值，以符合[規格](https://{DomainName}/apidocs/vpc-on-classic){: new_window}所給定的下限要求。

## validation_not_null
**訊息**：所提供的欄位必須是空值。

請確定值是空值。請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

以下是無效的範例：

```json
{
    "name": null
}
```

## validation_only_one
**訊息**：值必須符合其中一個子綱目。

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## validation_discriminator_forbidden
**訊息**：鑑別器欄位禁止此結構。

例如，如果您指定：
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

通訊協定為 `icmp`，且 _port_min_ 是 `tcp` 欄位，因此您將收到錯誤。
1. validation_discriminator_required 適用於遺漏 `icmp` 規則
2. validation_discriminator_forbidden 適用於已指定 `icmp` 的 `tcp` 欄位

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_internal_error
**訊息**：無

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_invalid_name
**訊息**：值不是有效的名稱。

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_invalid_ref
**訊息**：值不是有效的 HREF。

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_non_empty_uuid
**訊息**：無

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_required_field
**訊息**：遺漏必要欄位。

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## validation_unique_failed
**訊息**：所提供的欄位必須是唯一的。

您可能指定了已在使用中的名稱。請嘗試不同的值。

請確定您的要求符合 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## volume_attachment_delete_invalid_request
**訊息**：無法刪除磁區連接。

不容許執行此作業。無法刪除開機磁區的磁區連接，因為實例需要開機磁區。

## volume_attachment_update_invalid_request
**訊息**：無法更新磁區連接，因為要求無效。

由於以下任一原因，無法更新磁區連接：

* 無法重新命名開機磁區的磁區連接
* 開機磁區的磁區連接 `delete_volume_on_instance_delete` 欄位無法設定為 `true`

## volume_capacity_max
**訊息**：容許的最大磁區容量為 2000 GB。

指定的磁區容量超過最大磁區容量。請指定 10 到 2000 GB 之間的值。請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文件，以取得自訂設定檔中可接受的容量範圍。

## volume_capacity_missing
**訊息**：要求中缺少必要的磁區容量值。

建立磁區時，必須根據此設定檔定義來指定磁區容量。如需相關資訊，請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文件。

## volume_capacity_zero_or_negative
**訊息**：磁區容量應該大於零。

建立磁區時，要求中指定的容量值必須是 10 GB 到 2000 GB 之間的正數。請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文件，以取得支援的磁區容量值。

## volume_crn_account_id_mismatch
**訊息**：要求中指定的 CRN 不屬於現行使用者的帳戶。

Key Protect 根金鑰 CRN 與 IAM 授權帳戶 ID 不符合。請為 IAM 帳戶指定其他 Key Protect 根金鑰 CRN。請參閱 [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) 文件，以取得相關資訊。

## volume_crn_cname_mismatch
**訊息**：要求中指定的 CRN 無法用於現行環境。

要求中指定的 CRN 無法在現行環境中使用。請提供適用於您環境的 CRN。

## volume_encryption_key_crn_invalid
**訊息**：要求中指定的磁區加密金鑰 CRN 無效。

提供的 Key Protect CRN 無效。請取得 Cloud Identity and Access Management 服務中根金鑰的 CRN。如需相關資訊，請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption) 文件。

## volume_encryption_key_not_active
**訊息**：要求中指定的磁區加密金鑰不處於作用中狀態。

啟動現有金鑰或提供新金鑰。如果您無法啟動自己的金鑰，請聯絡系統管理者或[客戶支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## volume_encryption_key_not_authorized
**訊息**：您無權存取提供的磁區加密金鑰。

請提供您有權存取的其他加密金鑰，或聯絡系統管理者以取得對現行金鑰的存取權。

## volume_encryption_key_not_implemented
不支援磁區加密金鑰特性。

無法使用加密金鑰來建立磁區。此版本僅支援使用提供者管理的加密的磁區。若要使用您自己的加密金鑰，請使用支援客戶管理的加密的 Block Storage for VPC 版本。如需相關資訊，請與[客戶支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)聯絡。

## volume_id_invalid
**訊息**：要求參數中指定的磁區 ID 無效。

指定的磁區 ID 的格式不正確。請驗證是否正確輸入了磁區 ID，然後重試。如果不確定磁區 ID，請指定 `ibmcloud is volumes`（在 CLI 中）或 `GET volumes`（在 API 中），並搜尋磁區清單。

## volume_id_missing
**訊息**：要求參數中缺少磁區 ID。

取得特定磁區時，必須在要求參數中提供磁區 ID。

## volume_id_not_found
**訊息**：找不到磁區。磁區 ID `<volume_id>` 不存在，其中 `<volume_id>` 是提供的磁區 ID。

指定的磁區 ID 不存在。請提供有效的磁區 ID。

## volume_iops_zero_or_negative
**訊息**：磁區 IOPS 應該大於零。

建立磁區時，要求中指定的 IOPS 值必須是正數。請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文件，以取得支援的 IOPS 值。

## volume_name_duplicate
**訊息**：磁區名稱重複。要求中提供的磁區名稱 `<volume_name>` 已存在，其中 `<volume_name>` 是提供的磁區名稱。

要求中指定的磁區名稱已存在。請提供現行未使用的磁區名稱。

## volume_not_available
**訊息**：磁區無法使用。只能修改處於 available 狀態的磁區。現行磁區 `<volume_id>` 的狀態為 `<volume_status>`，其中 `<volume_id>` 是要求中提供的磁區 ID，`<volume_status>` 是現行磁區狀態。

僅當磁區處於 `available` 狀態時，才能修改該磁區。在修改磁區之前，請確保磁區處於 available 狀態。

## volume_not_deletable
**訊息**：刪除失敗。

僅當磁區處於 `available` 或 `failed` 狀態時，才能刪除該磁區。確保要刪除的磁區處於有效的狀態。

## volume_not_found
**訊息**：找不到具有指定 ID 的磁區。

請驗證輸入的磁區 ID 是否正確，然後重試。要取得磁區的清單，請指定 `ibmcloud is volumes`（在 CLI 中）或 `GET volumes`（在 API 中）。

## volume_not_implemented
**訊息**：此版本中不支援要求的作業。

如需相關資訊，請與[客戶支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)聯絡。

## volume_profile_capacity_iops_invalid
**訊息**：對於提供的容量和/或 IOPS，要求中指定的磁區設定檔無效。

磁區設定檔不容許使用要求中指定的磁區容量和/或 IOPS 值。請參閱 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 文件，以取得自訂設定檔的容量和 IOPS 的有效最小值和最大值。

## volume_profile_iops_invalid
**訊息**：要求中指定的磁區設定檔無法接受自訂 IOPS。

磁區設定檔不接受自訂 IOPS 值。您可能指定的是分層級 IOPS 設定檔，這種設定檔無需指定 IOPS 值。如果要提供特定 IOPS 值，請使用自訂 IOPS 設定檔。

## volume_profile_name_missing
**訊息**：要求中缺少必要的磁區設定檔名稱。

建立磁區時，要求主體中缺少磁區設定檔名稱，或者取得磁區時，要求參數中缺少磁區設定檔名稱。請在要求中提供磁區設定檔名稱，然後重試。如果您不知道磁區設定檔名稱，請在 CLI 指定 `ibmcloud is volume-profiles`，或在 API 要求中使用 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`，然後搜尋清單。

## volume_profile_not_found
**訊息**：找不到具有指定名稱的磁區設定檔。

找不到具有該名稱的磁區設定檔。請驗證是否提供了正確的磁區設定檔名稱。如果您不知道磁區設定檔名稱，請在 CLI 指定 `ibmcloud is volume-profiles`，或在 API 要求中使用 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`，然後搜尋清單。

## volume_quota_reached
**訊息**：已達到磁區配額。

您已超出現行帳戶的配額。請嘗試刪除一些磁區，或者聯絡[客戶支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)以增加配額。請參閱相關文件，以取得有關[虛擬專用雲端基礎架構配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas)的資訊。

## volume_resource_group_id_invalid
**訊息**：要求中指定的資源群組 ID 無效。

要求中指定的資源群組 ID 無效。請驗證正確的資源群組 ID。從 CLI，使用 `ibmcloud is volume VOLUME_ID` 指令。結果的資訊將包含資源群組及資源群組 ID。如需相關資訊，請參閱 [IBM Cloud CLI for VPC 參考資料](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)。

## volume_resource_group_not_authorized
**訊息**：無權對指定的資源群組執行現行動作。

您無權列出指定資源群組中的磁區或在該群組中建立磁區。請使用您對其具有許可權的其他資源群組，或者向管理者要求許可權，以取得對資源群組的列出和建立專用權。

## volume_resource_group_not_found
**訊息**：找不到具有指定 ID 的資源群組。

找不到資源群組 ID，因為輸入的資源群組 ID 可能錯誤，或者該 ID 不存在。請驗證正確的資源群組 ID。從 CLI，使用 `ibmcloud is volume VOLUME_ID` 指令。結果的資訊將包含資源群組及資源群組 ID。如需相關資訊，請參閱 [IBM Cloud CLI for VPC 參考資料](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)。

## volume_start_not_found
**訊息**：找不到其 ID 指定為頁面開始參數的磁區。

找不到在「列出磁區」呼叫的開始參數中指定的磁區 ID。請驗證磁區 ID 是否正確，以及您是否有權存取該磁區。

## volume_status_not_available
**訊息**：具有指定 ID 的磁區無法使用。

磁區不處於 Available 狀態；它可能處於 Pending 狀態。請等待磁區變為可用，然後重試。如果正在刪除該磁區，請使用其他磁區。若要檢查磁區的狀態，請在 CLI 中指定 `ibmcloud is volume VOLUME_ID`，或在 API 要求中使用 `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD`。

## volume_still_attached
**訊息**：磁區仍連接到實例。

無法刪除仍連接到 VSI 的磁區。如果為磁區設定了自動刪除，請等待 VSI 刪除，這將分離並刪除磁區。如果未設定自動刪除，請手動分離磁區。

## volume_template_invalid
**訊息**：提供的磁區範本無效。

提供的用於建立磁區的磁區範本無效。請參閱 [VPC API 參考資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以驗證是否在要求主體中提供了正確的參數。

## volume_update_invalid_request
**訊息**：磁區更新要求無效。

指定的更新要求主體內容無效。請參閱 [VPC API 參考資料](https://{DomainName}/apidocs/vpc-on-classic){: new_window}，以取得相關資訊以及正確的 API 要求語法。

## vpc_not_empty
**訊息**：無法刪除 VPC，因為它不是空的。

必須先刪除 VPC 中的所有資源，然後才能刪除 VPC。VPC 包含實例、子網路、公用閘道、負載平衡器和 VPN 閘道，其中一些資源還包含其他資源。請參閱[如何刪除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 文件，以取得詳細資料。

如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpc_resource_separation
**訊息**：資源位於不同的 VPC 中。

資源必須在同一個 VPC 中。例如，如果試圖將公用閘道連接到子網路，則公用閘道必須與子網路位於相同 VPC 中。

若要確定哪個 VPC 包含公用閘道，請執行 `GET /public_gateways/{id}` 指令，然後查看 "vpc" 值。相等於 CLI 指令 `ibmcloud is public-gateway <id>`。

## vpn_connection_cidr_duplicated
**訊息**：`<cidr_type>` CIDR 區塊具有重複的 CIDR 區塊 `<cidr_blocks>`。

建立連線時，提供了重複的 CIDR 區塊。請確保 CIDR 區塊是唯一的。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_created
**訊息**：無法將 CIDR 區塊新增至 VPN 連線 `<vpn_connection_id>`。

請提供有效的 CIDR，以符合規格所給定的需求。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_deleted
**訊息**：無法刪除 VPN 連線 `<vpn_connection_id>` 中的 CIDR 區塊。

請提供存在於連線上的有效 CIDR。連線必須至少有一個本端及對等節點 CIDR。

若要檢視連線的 CIDR 區塊，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `local_cidrs` 及 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**訊息**：在 VPN 連線 `<vpn_connection_id>` 中找不到 CIDR 區塊。

請提供位於連線上的有效 CIDR。

若要檢視連線的 CIDR 區塊，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `local_cidrs` 及 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**訊息**：無法更新 VPN 連線 `<vpn_connection_id>` 中的 CIDR 區塊。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_cidr_not_valid
**訊息**：CIDR 區塊 `<cidr_block>` 不代表有效位址。

值必須是未設定主機位元的有效內部 CIDR。

若要檢視連線的 CIDR 區塊，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `local_cidrs` 及 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**訊息**：CIDR 區塊 `<cidr_block_1>` 與 `<cidr_block_2>` 重疊。相同 VPC 上的兩個對等節點 CIDR 區塊不能重疊，而且相同連線上的兩個本端 CIDR 區塊不能重疊。

請提供有效的 CIDR，以符合規格所給定的需求。

若要檢視連線的 CIDR 區塊，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `local_cidrs` 及 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**訊息**：VPN 連線 `<vpn_connection_id>` 已使用名稱 `<vpn_connection_name>`。

請提供不同的連線名稱。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_invalid_name
**訊息**：名稱 `<vpn_connection_name>` 不是有效的 VPN 連線名稱。

有效的連線名稱以字母開頭，後面接著字母、數字、底線或連字號。

## vpn_connection_invalid_psk_format
**訊息**：預先共用金鑰 (PSK) `<psk>` 的格式無效。

有效的 PSK 只應該包含 6 到 128 個字元，它們為字母、數字、`-`、`+`、`&`、`!`、`@`、`#`、`$`、`%`、`^`、`*`、`(`、`)`、`,`、`.`、`:`、`_`。

## vpn_connection_local_cidrs_required
**訊息**：建立 VPN 連線時，至少需要一個本端 CIDR 區塊。

請提供符合規格所給定需求的有效本端 CIDR。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connection_local_subnets_quota_exceeded
**訊息**：VPN 閘道 `<vpn_gateway_id>` 的各 VPN 連線上的本端子網路已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視跨 VPN 連線的現行本端子網路，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API，並檢查每個連線的 `local_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**訊息**：VPN 連線的本端子網路已達到配額。

[配額](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視 VPN 連線的現行本端子網路，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `local_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**訊息**：無法建立 VPN 連線 `<vpn_connection_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_deleted
**訊息**：無法刪除 VPN 連線 `<vpn_connection_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_found
**訊息**：找不到 VPN 連線 `<vpn_connection_id>`。

請檢查連線 ID 是否正確。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_not_updated
**訊息**：無法更新 VPN 連線 `<vpn_connection_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_pair_cidrs_duplicated
**訊息**：至少有一對使用相同同層級閘道的本端 CIDR 和同層級 CIDR 重複。

此閘道上存在另一個連線具有與此連線符合的本端 CIDR 和同層級 CIDR。請提供有效的一組本端/同層級 CIDR。

## vpn_connection_peer_cidrs_required
**訊息**：建立 VPN 連線時，至少需要一個對等節點 CIDR 區塊。

請提供符合規格所給定需求的有效同層級 CIDR。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connection_peer_subnets_quota_exceeded
**訊息**：VPN 閘道 `<vpn_gateway_id>` 的各 VPN 連線上的同層級子網路已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視跨 VPN 連線的現行對等節點子網路，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API，並檢查每個連線的 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**訊息**：VPN 連線的同層級子網路已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視 VPN 連線的現行對等節點子網路，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API，並檢查 `peer_cidrs` 欄位。相等於 CLI 指令：`ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**訊息**：無法為同層級 CIDR 區塊 `<peer_cidr>` 新增靜態路徑。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_static_route_not_deleted
**訊息**：無法移除同層級 CIDR 區塊 `<peer_cidr>` 的靜態路徑。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_connection_update_cidrs_not_permitted
**訊息**：更新連線時，無法變更 CIDR 區塊。建立或刪除 CIDR 區塊時，請使用正確的 API。

請提供符合規格所給定需求的有效要求值。

如需修正此問題的進一步指示，請參閱 [API 文件](https://{DomainName}/apidocs/vpc-on-classic){: new_window}。

## vpn_connections_quota_exceeded
**訊息**：無法建立 VPN 連線，因為 VPN 閘道 `<vpn_gateway_id>` 已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視 VPN 閘道的現行 VPN 連線，請使用 `GET /vpn_gateways/<vpn_gateway_id>/connections` API。相等於 CLI 指令：`ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**訊息**：VPN 閘道 `<vpn_gateway_id>` 已使用名稱 `<vpn_gateway_name>`。

請提供其他 VPN 閘道名稱。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_initialization_error
**訊息**：無法起始設定 VPN 閘道。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_invalid_name
**訊息**：名稱 `<vpn_gateway_name>` 不是有效的 VPN 閘道名稱。

有效的 VPN 閘道名稱以字母開頭，後面接著字母、數字、底線或連字號。

## vpn_gateway_invalid_state
**訊息**：所要求作業的 VPN 閘道 `<vpn_gateway_id>` 處於無效狀態。

「VPN 閘道」必須先處於 `available` 狀態，您才能對其進行操作。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_ip_create_error
**訊息**：無法建立 VPN 閘道的 IP 位址。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_missing_subnet_id
**訊息**：找不到給定 VPN 閘道範本的子網路 ID。

**子網路 ID** 是必要欄位。請在指令中提供子網路 ID。

## vpn_gateway_missing_name
**訊息**：找不到給定 VPN 閘道範本的名稱。

**name** 是必要欄位。請在指令中提供名稱。

## vpn_gateway_not_created
**訊息**：無法建立 VPN 閘道 `<vpn_gateway_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_deleted
**訊息**：無法刪除 VPN 閘道 `<vpn_gateway_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_found
**訊息**：找不到 VPN 閘道 `<vpn_gateway_id>`。

請檢查 VPN 閘道 ID 是否正確。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_not_updated
**訊息**：無法更新 VPN 閘道 `<vpn_gateway_id>`。

請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_subnet_not_found
**訊息**：找不到所給定 VPN 閘道的子網路 `<subnet_id>`。

您參照的子網路不存在。請檢閱您的要求，確定您已指定適當的子網路 ID。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateway_subnet_status_error
**訊息**：因子網路狀態，而無法建立 VPN 閘道。

請提供處於 `available` 狀態的子網路。請在幾分鐘之後重試。如果此問題持續發生，請[與支援中心聯絡](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)。

## vpn_gateways_quota_exceeded
**訊息**：無法建立 VPN 閘道，因為帳戶和/或地區已達到配額。

[配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}頁面上指定了每個資源的配額。

若要檢視現行 VPN 閘道，請使用 `GET /vpn_gateways` API。相等於 CLI 指令：`ibmcloud is vpn-gateways`

## zone_inconsistency
**訊息**：資源必須屬於相同區域。

* 連接磁區時，確保磁區和實例位於相同區域中。
* 關聯浮動 IP 時，確保浮動 IP 和子網路位於相同區域中。
