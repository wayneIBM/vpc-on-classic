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

# 使用 REST API 建立 VPC
{: #creating-a-vpc-using-the-rest-apis}

本文件說明如何使用 REST API 透過 `curl` 來建立 {{site.data.keyword.cloud}} Virtual Private Cloud 資源。如需使用 Go 和 Python 呼叫 REST API 的程式碼 Snippet，請參閱此[程式碼範例](https://github.com/IBM-Cloud/vpc-API-samples)。

## 必要條件

1. 確保您有公用 SSH 金鑰，該金鑰將用於連接至虛擬伺服器實例。

   您可能已有公開 SSH 金鑰。在起始目錄的 `.ssh` 目錄下尋找稱為 `id_rsa.pub` 的檔案，例如，`/Users/<USERNAME>/.ssh/id_rsa.pub`。檔案的開頭為 `ssh-rsa`，結尾為電子郵件位址。

   如果您沒有公開 SSH 金鑰，或忘記現有 SSH 金鑰的密碼，請執行 `ssh-keygen` 指令並遵循提示來產生新的 SSH 金鑰。
2.  確保您有 IBM Cloud 帳戶的 API 金鑰。如果您沒有 API 金鑰，請參閱[建立 API 金鑰](/docs/iam?topic=iam-userapikey#create_user_key)。您需要在步驟 1 中將此 API 金鑰儲存在環境變數中。

## 步驟 1：將 API 金鑰儲存為變數

請執行下列指令，將 API 金鑰儲存在環境變數中：

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## 步驟 2：取得 IBM Identity and Access Management (IAM) 記號

請參閱[使用 API 金鑰取得 IBM Cloud IAM 記號](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)主題，以瞭解如何取得 IAM 記號，或者使用下列範例指令。

請執行下列指令，以利用 `jq` 公用程式來取得並剖析 IAM 記號。可以修改此指令以使用其他剖析工具，也可以移除指令的最後一部分（如果希望自行手動剖析記號）。

```bash
iam_token='curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)''
```
{: pre}

若要檢視 IAM 記號，請執行 `echo $iam_token`。結果應該看起來如下：

```
Bearer <your token>
```

Authorization 標頭預期 IAM 記號以 `Bearer` 開頭。如果您收到的結果不包含 `Bearer`，請更新 `iam_token` 變數以包含 Bearer。這些範例假設 `iam_token` 中包含 `Bearer`。

您必須重複之前的步驟，每小時重新整理 IAM 記號，因為記號到期。
{: important}

## 步驟 3：將 API 端點和 version 參數儲存為變數

API 端點是根據地區，並遵循慣例 `https://<region>.iaas.cloud.ibm.com`。各地區的端點如下：

* US-SOUTH：`https://us-south.iaas.cloud.ibm.com`
* EU-DE：`https://eu-de.iaas.cloud.ibm.com`
* JP-TOK：`https://jp-tok.iaas.cloud.ibm.com`

將 API 端點儲存在變數中，以便可以在 API 要求中使用，而不必輸入完整 URL。例如，要將 us-south 端點儲存在變數中，請執行以下指令：

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

若要驗證已儲存此變數，請執行 `echo $rias_endpoint`，並確定回應不是空的。

每個 API 要求都必須包含 `version` 參數，格式為 `YYYY-MM-DD`。請執行下列指令，將版本日期儲存在變數中，以便可以在階段作業中重複使用。如需設定 `version` 參數的相關資訊，請參閱 [VPC 的 API 參考資料](https://{DomainName}/apidocs/vpc-on-classic#versioning)中的**版本化**。

```bash
version="2019-05-31"
 ```
{: pre}

若要驗證此變數是否已儲存，請執行 `echo $version`，並確保回應不是空的。

## 步驟 4：執行 GET regions API

下列指令會以 JSON 格式傳回 VPC 可用的地區。應該傳回至少一個物件。

每個 API 要求中都需要 `version` 和 `generation` 查詢參數。如需詳細資料，請參閱 [VPC 的 API 參考資料](https://{DomainName}/apidocs/vpc-on-classic)。
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

如果遇到非預期結果，請在 `curl` 指令後面新增 `--verbose`（除錯）旗標，以取得詳細的記載資訊。另請參閱[疑難排解頁面](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc)中的常見錯誤。
{: tip}


## 步驟 5：執行 GET zones API

下列指令以 JSON 格式傳回 `us-south` 地區中可用於 VPC 的所有區域。應該會傳回至少三個物件。

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

根據使用的地區端點，更新上述參數中的地區名稱 `us-south`。地區端點只知道其自己的區域，因此如果要使用東京的端點，請使用 `jp-tok`。
{: note}

## 步驟 6：執行 GET profiles API

下列指令會以 JSON 格式傳回您實例可用的設定檔。應該傳回至少一個物件。

在 curl 指令後面新增 ` | json_pp `，以取得可讀取的 JSON 字串
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

如果 API 輸出受限於分頁，請將限制設為高於 `total_count`，例如：

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步驟 7：執行 GET images API

下列指令會以 JSON 格式傳回您實例可用的映像檔。應該傳回至少一個物件。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步驟 8：執行 GET VPC API

下列指令以 JSON 格式傳回在您的帳戶下建立的所有 VPC。

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步驟 9：建立虛擬專用雲端

建立名為 `my-vpc` 的 {{site.data.keyword.cloud_notm}} VPC。

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

對於其餘呼叫，您需要知道新建的 {{site.data.keyword.cloud_notm}} VPC 的 ID。請將其 ID 儲存在變數中。此指令應如下所示：`vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## 步驟 10：建立子網路

在 {{site.data.keyword.cloud_notm}} VPC 中建立子網路。下列範例在 `us-south-2` 區域中建立 VPC。

下列範例將[預設位址字首](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)用於 `us-south-2` 區域。對於您的帳戶來說，預設值可能已變更，請參閱[如何規劃 VPC 位址](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design)。
{: note}

若要取得 VPC 的位址字首清單，請執行下列指令：`curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`。
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

將子網路 ID 儲存在變數中。

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## 步驟 11：檢查子網路的狀態

若要佈建子網路中的資源，子網路必須處於 `available` 狀態。在繼續之前，請查詢子網路資源，並確保狀態為 `available`。如果狀態為 `failed`，請與[支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)聯絡，以取得詳細資料。您可以嘗試佈建另一個子網路以試著繼續進行。

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## 步驟 12：建立公用閘道

若要容許子網路中的虛擬伺服器實例存取公用網際網路，請將公用閘道 (PGW) 新增至子網路。  

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

將公用閘道 ID 儲存在變數中。

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

為了連接並使用公用閘道，公用閘道必須處於 `available` 狀態。在繼續之前，請查詢公用閘道資源，並確保狀態為 `available`。如果狀態為 `failed`，請與[支援中心](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)聯絡，以取得詳細資料。您可以試圖繼續佈建其他公用閘道。

若要檢查公用閘道的狀態，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步驟 13：將公用閘道連接到子網路

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## 步驟 14：建立 SSH 金鑰

使用公開 SSH 金鑰來建立金鑰。建立虛擬伺服器實例時，將使用此金鑰。此外，登入到虛擬伺服器實例時，也需要此金鑰。

可以在首次於使用者介面或 CLI 中建立 VPC 時，新增金鑰。沒有任何工具可供之後再新增金鑰。
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

將金鑰 ID 儲存在變數中。

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## 步驟 15：為虛擬伺服器實例選擇設定檔和映像檔

執行這些 API，以列出可供虛擬伺服器實例使用的所有設定檔及映像檔，並選擇一個組合。

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

若要取得設定檔的其他詳細資料，您可以使用 {{site.data.keyword.cloud_notm}} CLI 指令 `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]` 來查詢全球型錄。範例：

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

下列指令會列出可用的映像檔。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

將設定檔名稱和映像檔 ID 儲存在變數中，佈建虛擬伺服器實例時將使用這些變數。

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## 步驟 16：佈建虛擬伺服器實例

在新建的子網路中佈建虛擬伺服器實例 (VSI)。傳入公開 SSH 金鑰，以在佈建實例之後登入。

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

將虛擬伺服器實例 ID 儲存在變數中。

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## 步驟 17：檢查虛擬伺服器實例的狀態

佈建虛擬伺服器實例可能需要最長幾分鐘時間。在繼續之前，請查詢伺服器的狀態，並確保狀態為 `running`。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

將虛擬伺服器實例的主要網路介面 ID（在先前的 API 呼叫中傳回）儲存在變數中。  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

除非您查詢特定實例，否則無法取得主要網路介面。
{: note}

## 步驟 18：建立浮動 IP

若要為虛擬伺服器實例建立浮動 IP，請將實例的主要網路介面用作新浮動 IP 位址的目標。

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


將浮動 IP ID 儲存在變數中。

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## 步驟 19：對預設安全群組新增容許 SSH 資料流量的規則

配置安全群組，以定義針對實例容許的入埠和出埠資料流量。

尋找 VPC 的安全群組：

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

將安全群組 ID 儲存在變數中。

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

現在，建立容許入埠 SSH 資料流量的規則：

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

## 步驟 20：透過 SSH 登入到虛擬伺服器實例

若要以 SSH 方式登入伺服器，請使用您先前所建立的浮動 IP 的 `address`。若要取得浮動 IP 的 `address`，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

使用浮動 IP 的 `address` 可透過 SSH 連接至虛擬伺服器實例：

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

安全群組可以防止以 SSH 方式存取虛擬伺服器。如果需要 SSH 存取，您必須使用[適當的安全群組](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)。
{: note}

如需如何連接至實例的相關資訊，請參閱[使用 Linux 連接至實例](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance)。

## 步驟 21：建立並連接區塊儲存空間磁區

使用類似於此範例的要求來建立區塊儲存空間磁區，並指定磁區名稱、區域和設定檔。

若要查看磁區設定檔的清單，請提供以下要求：

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

設定檔可以是 general-purpose (3 IOPS/GB)、5iops-tier、10iops-tier 和 custom。如需根據您所選磁區設定檔的磁區容量及 IOPS 範圍相關資訊，請參閱[關於 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)。

您不必在要求中提供磁區名稱，依預設系統會建立磁區名稱。此範例指定了磁區名稱 `helloworld-vol`。所有磁區名稱都必須是唯一的。

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

對於其餘呼叫，您需要知道新建立的磁區的 ID。將磁區的 ID 儲存為變數，例如：`vol=640774d7-2adc-4609-add9-6dfd96167a8f`。

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

首次建立磁區時，其狀態為 `pending`。磁區的狀態必須變成 `available` 後（這需要幾分鐘時間），您才能繼續操作。
{: note}


若要檢查磁區的狀態，請執行下列指令：

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

建立磁區連接，以將新磁區連接到虛擬伺服器實例。使用您先前在要求中建立的實例 ID 變數。在資料參數中使用磁區 ID、磁區的 CRN 或 URL 來指定磁區。此範例使用的是為磁區 ID 建立的變數。

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

建立磁區連接後，取得磁區連接 ID。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

將連接 ID 儲存為變數。

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

指定磁區連接 ID，以將新磁區連接到虛擬伺服器實例。

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## 步驟 22（選用）：刪除資源

選擇性地刪除資源。如果資源包含其他資源，則無法予以刪除。例如，如果虛擬專用雲端包含子網路，則無法予以刪除，而且，如果子網路包含虛擬伺服器實例，則無法予以刪除。進行刪除作業時，API 可能很快即會返回，但可能仍在進行資源刪除。發出刪除要求之後，請確定已刪除資源，然後才嘗試刪除母項資源。如需詳細資料，請參閱[刪除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)。

### 刪除浮動 IP

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 刪除虛擬伺服器實例

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 刪除金鑰

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 刪除子網路

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 刪除公用閘道

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### 刪除 VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 刪除磁區

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## 恭喜！

您已使用 REST API 順利佈建了虛擬專用雲端實例。若要嘗試其他 API 指令，請探索完整參考資料：

* [VPC 的 API 參考資料](https://{DomainName}/apidocs/vpc-on-classic)
