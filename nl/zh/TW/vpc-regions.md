---

copyright:
  years: 2019
lastupdated: "2019-05-22"

keywords: region, zone, deploy, datacenter, data, center, federated, CLI, API, account, failover, disaster, recovery, DR

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 在不同地區中建立 VPC
{: #creating-a-vpc-in-a-different-region}

地區是一種特定的地理位置，可以在其中部署 APP、服務和其他 {{site.data.keyword.cloud}} 資源。地區由一個以上區域組成，區域是實體資料中心，用於容納主機服務和應用程式的運算、網路和儲存空間資源以及相關冷卻系統和電源。區域彼此隔離，以確保地區內不會共用單一失敗點。

IBM Cloud VPC 服務是地區性的。若要容許進行災難回復 (DR)，您必須在我們的其他某個地區中建立 VPC，以便能以失效接手到其他替代地區的方式進行回復。
{: note}

Virtual Private Cloud 將分階段部署到所有 {{site.data.keyword.cloud}} 地區。

|   位置         | 地區   | API 端點     | 狀態   |
| ------- | :------: | :------: |:------: |
| 達拉斯 | us-south | `us-south.iaas.cloud.ibm.com`| 可用      |
| 法蘭克福  | eu-de | `eu-de.iaas.cloud.ibm.com`| 可用      |
| 東京  | jp-tok | `jp-tok.iaas.cloud.ibm.com`| 可用      |
| 倫敦   | eu-gb | `eu-gb.iaas.cloud.ibm.com`| 即將推出    |
| 雪梨   | au-syd | `au-syd.iaas.cloud.ibm.com`| 即將推出    |
| 華盛頓特區    | us-east | `us-east.iaas.cloud.ibm.com`| 即將推出    |

當您登入特定地區時，IBM Cloud CLI 會自動設定「地區 API (VPC)」端點。
{: note}

## 使用 CLI 登入特定地區
{: #log-in-to-a-specific-region-using-the-cli}

登入 IBM Cloud 時，可以指定地區，也可以稍後選擇地區。例如，要直接登入到達拉斯 (`us-south`) 地區中的廣域 API 端點，請根據您是具有聯合帳戶 (SSO) 還是非聯合帳戶，執行下列相應指令。

若為聯合帳戶，請執行下列指令：

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

若為非聯合帳戶，請執行下列指令：

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

若要稍後選擇地區，請不要指定 `-r <region>` 參數，CLI 將會提示您選擇地區。

輸出範例：

```
API endpoint: cloud.ibm.com

Get One Time Code from https://identity-2.eu-central.iam.cloud.ibm.com/identity/passcode to proceed.
Open the URL in the default browser? [Y/n]> y
One Time Code >
Authenticating...
OK

Select an account:
1. MyAccount (00a11aa1a11aa11a1111a1111aaa11aa) <-> 1234567
2. TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321
Enter a number> 2
Targeted account TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321


Targeted resource group Default

Select a region (or press enter to skip):
1. au-syd
2. jp-tok
3. eu-de
4. eu-gb
5. us-south
6. us-east
Enter a number> 5
Targeted region us-south


API endpoint:      https://cloud.ibm.com   
Region:            us-south   
User:              first.last@email.com   
Account:           TeamAccount (2bb222bb2b22222bbb2b2222bb2bb222) <-> 7654321  
Resource group:    Default   
CF API endpoint:      
Org:                  
Space:                

...
```
{: screen}

## 使用 CLI 切換地區
{: #switch-regions-using-the-cli}

若要取得 VPC 地區的最新狀態，請執行下列指令：

```
ibmcloud is regions
```
{: pre}

若要切換至不同的地區，請執行 `ibmcloud target -r <region>` 指令。例如，若要切換至「法蘭克福」地區，請執行下列指令：

```
ibmcloud target -r eu-de
```
{: pre}

若要檢查現行位置，請執行下列指令：

```
ibmcloud target
```
{: pre}

## 使用 API 切換地區  
{: #switch-regions-using-the-api}

若要使用 REST 與地區 VPC API 互動，請將要求定向到與要在其中建立資源的地區關聯的 API 端點。地區的 API 端點如上表格所示。此外，透過執行下列指令，可以尋找與地區關聯的端點：

```
ibmcloud is regions
```
{: pre}


例如，若要取得 `us-south` 地區中的 VPC 清單，請執行下列指令：

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## 取得區域
{: #get-zones}

若要取得每個地區可用的區域的清單，請執行 `ibmcloud is zones <region>` 指令。例如，若要取得 `us-south` 地區中的區域清單，請執行下列指令：

```
ibmcloud is zones us-south
```
{: pre}
