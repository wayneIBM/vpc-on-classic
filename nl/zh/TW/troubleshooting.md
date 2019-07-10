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

# IBM Cloud VPC 疑難排解
{: #troubleshooting-your-ibm-cloud-vpc}

本文件涵蓋您可能遇到的常見困難，並提供一些有用的提示。

## 在使用 CLI 時開啟 `TRACE` 模式
{: #troubleshoot}

若要在使用 CLI 時開啟 `TRACE`（除錯）模式，請在 CLI 指令之前設定 `IBMCLOUD_TRACE=true`。

範例：

 ```
 IBMCLOUD_TRACE=true ibmcloud is pubgws
 ```

## 使用 API 時採用 DEBUG 模式或 `verbose` 模式
{: #troubleshoot}

例如，可以對 `curl` 指令新增 `--verbose`，然後對我們傳送傳回的 `X-Request-Id:` 值，以便我們可以對問題進行疑難排解。如果您遇到連線問題，`--verbose` 旗標特別有用。

另一個選項是將 `-i` 旗標新增至 `curl` 指令，讓支援團隊可以查看要求回應中的標頭。當您需要與支援中心聯絡時，此旗標通常十分有用。

## API 呼叫需要持有人記號
{: #troubleshoot}

透過 cURL 指令使用 API 時，可能需要在 Authorization 標頭中包含 "Bearer" ，取決於 `$iam_token` 中的內容。如果它已包括 "Bearer" 這個字，則不需要再次將它包括在標頭中。大部分範例都假設 "Bearer" 內含在標頭中。

## 常見問題
{: #troubleshoot}

下面是您可能會遇到的一些問題。

### 未獲授權（401 或 403 錯誤）
{: #troubleshoot}

您的帳戶可能無權使用 VPC。請確定您使用已加入的帳戶。

### 無法建立資源
{: #troubleshoot}

如果您無法建立 VPC 或其他資源，請確保帳戶擁有者已授與您正確的[許可權](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

### 實例全部處於不明狀態
{: #troubleshoot}

確保帳戶擁有者已授與您正確的[許可權](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)，以用來檢視及管理實例。

### API 沒有回應
{: #troubleshoot}

如果 API 不再傳回任何 JSON，則您的 IAM 記號可能已到期，並且需要重新整理。請重新登入 IBM Cloud，或執行 `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')` 來重新整理記號。

### 錯誤：對實例呼叫動作時，發生 409 衝突
{: #troubleshoot}

如果您實例的狀態與動作衝突，則無法呼叫特定實例動作。例如，如果您的實例狀態為「已停止」，而且您嘗試執行「重新開機」動作，則系統會傳回 409 錯誤。

| 狀態        | 動作       | 衝突     |
| ----------- | ---------- | -------- |
| 執行中      | 啟動       | 是       |
| 已停止      | 啟動以外的任何動作    | 是       |
| 未執行      | 暫停       | 是       |
| 未執行      | 重新開機   | 是       |
| 未暫停      | 繼續       | 是       |
| 已暫停      | 繼續以外的任何動作    | 是       |


### 實例未回應 `instance-reboot` 要求
{: #troubleshoot}

如果您的實例未回應 `instance-reboot` 要求，則可以嘗試 `instance-reset` 要求。您可以將 `instance-reboot` 視為軟性要求，並將 `instance-reset` 視為硬性要求。`instance-reboot` 要求會將 OS 重新開機要求傳送至實例，但 `instance-reset` 要求會執行 VSI 實例的強迫重設。您可以將差異想成在電腦鍵盤按下 "ctrl-alt-delete" 鍵，以及按下重設或電源按鈕。最好記住 `instance-reset` 要求的完成時間比 `instance-reboot` 要求還要久。

### 無法刪除資源
{: #troubleshoot}

特定作業（例如，建立及刪除 VSI，以及建立及刪除子網路）是透過 API 非同步完成。基於此事實，建議您輪詢所要刪除的資源，先檢查刪除作業，再繼續進行。

由於這些非同步作業，從系統刪除資源可能需要數分鐘的時間。為了協助刪除，最佳作法是依下列次序執行作業：

1. 刪除實例
2. 刪除公用閘道
3. 刪除子網路
4. 刪除 VPC

如需特定資訊，請參閱有關[如何刪除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 的指示。
