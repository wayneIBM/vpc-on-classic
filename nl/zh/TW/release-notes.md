---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# 版本注意事項
{: #release-notes}

使用這些版本注意事項可瞭解有關 {{site.data.keyword.cloud}} Virtual Private Cloud 最新變更的資訊。
{:shortdesc}

## 2019 年 6 月 14 日
{: #june-14-2019}

**對 IBM Cloud VPC 使用者介面的更新**

- **SSH 金鑰和資源群組**。
    * 資源群組會顯示在 SSH 金鑰清單視圖中。
    * 資源群組在 SSH 金鑰佈建限制模式中可供選擇。
- **設定檔和頻寬**。指派給每個設定檔的頻寬上限會顯示在**熱門設定檔**和**所有設定檔**顯示頁面中。
- **資源群組**。VSI 佈建頁面會顯示資源群組下拉清單。

**對 Block Storage for VPC 的更新**
- **磁區名稱長度**增加到了 63 個字母數字字元。
- 下列**磁區錯誤碼**已重新命名，並且已更新或刪除相應訊息：
    * `volume_encryption_key_account_id_mismatch` 重新命名為 `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` 重新命名為 `volume_crn_cname_mismatch`
    * 移除了 `volume_encryption_key_region_not_found`

**對 SDK 的更新**

- 發佈了 **Terraform Provider V0.17.1**。請下載[最新的二進位檔](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1)。
- **docker 映像檔**也已使用最新的 Terraform 提供者進行更新。可以使用以下指令取回最新的 docker 映像檔：`docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- 請務必記住將 `generation` 設定為提供者引數，或者匯出 `IC_GENERATION= 1`，以便程式碼使用標準基礎架構上現行發佈的 VPC 版本。依預設，該值設定為 2（表示即將發佈的 VPC 現在處於測試版階段）。
- 提供者引數（`bluemix_api_key` 和 `bluemix_timeout`）已淘汰，因此在執行 Terraform 方案或套用 Terraform 方案時可能會擲出警告。

## 2019 年 6 月 7 日
{: #june-07-2019}

- **IBM Cloud VPC 現在已正式上市**。所有[隨收隨付制帳戶](/docs/account?topic=account-accounts)都可以佈建 VPC 資源。登入到 [IBM Cloud 主控台](https://{DomainName}/vpc/overview)，立即開始使用吧！

- **現在可以對實例、金鑰、磁區和安全群組設定個別的授權原則**。在 [API 和 CLI 呼叫所需的資源授權](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls)中瞭解管理這些資源需要的角色。搶先體驗使用者不受現有原則的影響。

- **從使用者介面、CLI 和 API 中移除了虛擬網路介面的埠速度參數**。

- **現在，使用者介面中提供了可監視度量的多地區支援**。


## 2019 年 5 月 31 日
{: #may-31-2019}

**新的 API 版本可用**。VPC on Classic API 版本已從 `2019-01-01` 更新為 `2019-05-31`。如需準則和最佳作法，請參閱 Virtual Private Cloud on Classic API 中的[版本化](https://{DomainName}/apidocs/vpc-on-classic#versioning)。

## 2019 年 5 月 24 日
{: #may-24-2019}

- **現在，處於 deleting 狀態的負載平衡器實例會顯示在使用者介面的清單視圖中**。在使用者介面中要求刪除負載平衡器時，清單視圖會繼續顯示該負載平衡器，直到刪除程序完成為止。

- **現在，使用者介面中提供了第 7 層負載平衡器特性**。您現在可以在使用者介面中配置第 7 層負載平衡器特性。

- **現在，使用者介面中提供了負載平衡器檢視和編輯原則**。使用者介面中提供了用於檢視和編輯負載平衡器原則的新功能。

如需相關資訊，請參閱[負載平衡器文件](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)。


## 2019 年 5 月 10 日
{: #may-10-2019}


- **變更了設定檔名稱**。實例設定檔名稱已變更。需要更新用於佈建實例的指令才能使用新的設定檔名稱。請在[這裡](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)閱讀有關設定檔的更多資訊。

- **使用者介面中提供了預估的按月浮動 IP 定價**。現在，保留浮動 IP 位址時，可以看到一行顯示該浮動 IP 的按月預估價格。

- **Hyper Protect 支援可用**。

- **子網路名稱和區域顯示為鏈結，可將您連接至使用者介面中的關聯子網路**。現在，子網路按名稱（而不是按子網路 ID）列出，並且名稱充當關聯子網路的子網路詳細資料頁面的鏈結。

- **使用者介面中提供了成本摘要估算工具**。

- **對負載平衡器儲存區和接聽器的輪詢會反映在使用者介面中**。

    * 現在，在負載平衡器儲存區頁面的資源表格下方，會看到顯示前次更新時間的計數器。
    * 預設輪詢間隔為 30 秒。
    * 可以顯示四 (4) 種不同的狀態：`newProvision`、`active`、`failed` 和`其他所有狀態`。
        * 對於 newProvision：時間間隔為 15 秒。儲存區和接聽器立即進入作用中狀態。
        * 對於 active：資源清單和標頭每 60 秒更新一次。
        * 對於 failed：輪詢已停止。
        * 對於其他所有狀態：輪詢繼續以 30 秒時間間隔執行。
    * 如果刪除接聽器，您可以觸發標頭狀態以顯示`正在更新（動作無法使用）`。
    * 另請注意，執行更新時，會更新標頭參考資訊。
    * 這些變更還適用於接聽器輪詢。

- **輪詢成員時，負載平衡器成員計數顯示在使用者介面的成員和詳細資料頁面中**。

    * 在性能狀態區段中，可看到成員計數，其旁邊是「正在提取實例詳細資料」。
    * 在資源表格的「實例」直欄中，可看到成員計數，其旁邊是「正在提取實例詳細資料」。
    * 成員載入後，如果包含無效的成員，您將看到黃色警告圖示。

- **使用者介面中的 VPC 詳細資料頁面會顯示所有連接的子網路**。
