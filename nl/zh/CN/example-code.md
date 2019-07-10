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

# 使用 REST API 创建 VPC
{: #creating-a-vpc-using-the-rest-apis}

本文档说明了如何使用 REST API 通过 `curl` 来创建 {{site.data.keyword.cloud}} Virtual Private Cloud 资源。有关使用 Go 和 Python 调用 REST API 的代码片段，请参阅此[示例代码](https://github.com/IBM-Cloud/vpc-api-samples)。

## 先决条件

1. 确保您有公用 SSH 密钥，该密钥将用于连接到虚拟服务器实例。

   您可能已有公用 SSH 密钥。在主目录的 `.ssh` 目录下查找名为 `id_rsa.pub` 的文件，例如 `/Users/<USERNAME>/.ssh/id_rsa.pub`。该文件以 `ssh-rsa` 开头，以您的电子邮件地址结尾。

   如果您没有公用 SSH 密钥，或者如果忘记了现有 SSH 密钥的密码，请通过运行 `ssh-keygen` 命令并遵循提示来生成新密钥。
2.  确保您有 IBM Cloud 帐户的 API 密钥。如果您没有 API 密钥，请参阅[创建 API 密钥](/docs/iam?topic=iam-userapikey#create_user_key)。您需要在步骤 1 中将此 API 密钥存储在环境变量中。

## 步骤 1：将 API 密钥存储为变量

运行以下命令将 API 密钥存储在环境变量中：

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## 步骤 2：获取 IBM Identity and Access Management (IAM) 令牌

请参阅[使用 API 密钥获取 IBM Cloud IAM 令牌](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)主题以了解如何获取 IAM 令牌，或者使用以下示例命令。

运行以下命令通过 `jq` 实用程序来获取并解析 IAM 令牌。可以修改此命令以使用其他解析工具，也可以除去命令的最后一部分（如果希望自行手动解析令牌）。

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

要查看 IAM 令牌，请运行 `echo $iam_token`。结果应该类似于以下内容：

```
Bearer <your token>
```

Authorization 头需要 IAM 令牌以 `Bearer` 开头。如果您收到的结果不包含 `Bearer`，请更新 `iam_token` 变量以包含 Bearer。这些示例假定 `iam_token` 中包含 `Bearer`。

必须重复上述步骤以每小时刷新一次 IAM 令牌，因为 IAM 令牌会到期。
{: important}

## 步骤 3：将 API 端点和 version 参数存储为变量

API 端点是基于区域的，并遵循约定 `https://<region>.iaas.cloud.ibm.com`。各区域的端点如下：

* US-SOUTH：`https://us-south.iaas.cloud.ibm.com`
* EU-DE：`https://eu-de.iaas.cloud.ibm.com`
* JP-TOK：`https://jp-tok.iaas.cloud.ibm.com`

将 API 端点存储在变量中，以便可以在 API 请求中使用，而不必输入完整 URL。例如，要将 us-south 端点存储在变量中，请运行以下命令：

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

要验证此变量是否已保存，请运行 `echo $rias_endpoint`，并确保响应不为空。

每个 API 请求都必须包含 `version` 参数，格式为 `YYYY-MM-DD`。运行以下命令将版本日期存储在变量中，以便可以在会话中复用。有关设置 `version` 参数的更多信息，请参阅 [VPC 的 API 参考](https://{DomainName}/apidocs/vpc-on-classic#versioning)中的**版本控制**。

```bash
version="2019-05-31"
 ```
{: pre}

要验证此变量是否已保存，请运行 `echo $version`，并确保响应不为空。

## 步骤 4：运行 GET regions API

以下命令以 JSON 格式返回可用于 VPC 的区域。应该会返回至少一个对象。

每个 API 请求中都需要 `version` 和 `generation` 查询参数。有关详细信息，请参阅 [VPC 的 API 参考](https://{DomainName}/apidocs/vpc-on-classic)。
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

如果遇到意外结果，请在 `curl` 命令后面添加 `--verbose`（调试）标志，以获取详细的日志记录信息。另请参阅[故障诊断页面](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc)中的常见错误。
{: tip}


## 步骤 5：运行 GET zones API

以下命令以 JSON 格式返回 `us-south` 区域中可用于 VPC 的所有专区。应该会返回至少三个对象。

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

根据使用的区域端点，更新上述参数中的区域名称 `us-south`。区域端点只知道其自己的专区，因此如果要使用东京的端点，请使用 `jp-tok`。
{: note}

## 步骤 6：运行 GET profiles API

以下命令以 JSON 格式返回可用于实例的概要文件。应该会返回至少一个对象。

在 curl 命令之后添加 ` | json_pp` 可获取可读的 JSON 字符串
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

如果 API 输出受分页限制，请将限制设置为高于 `total_count`，例如：

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步骤 7：运行 GET images API

以下命令以 JSON 格式返回可用于实例的映像。应该会返回至少一个对象。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步骤 8：运行 GET VPCs API

以下命令以 JSON 格式返回在您的帐户下创建的所有 VPC。

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步骤 9：创建虚拟私有云

创建名为 `my-vpc` 的 {{site.data.keyword.cloud_notm}} VPC。

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

对于其余调用，您需要知道新创建的 {{site.data.keyword.cloud_notm}} VPC 的标识。请将其标识存储在变量中。该命令将类似于以下内容：`vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## 步骤 10：创建子网

在 {{site.data.keyword.cloud_notm}} VPC 中创建子网。以下示例在 `us-south-2` 专区中创建 VPC。

以下示例将[缺省地址前缀](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)用于 `us-south-2` 专区。您帐户的缺省值可能已更改，请参阅[如何规划 VPC 地址](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design)。
{: note}

要获取 VPC 的地址前缀列表，请运行以下命令：`curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`。
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

将子网标识存储在变量中。

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## 步骤 11：检查子网的状态

要在子网中供应资源，子网必须处于 `available` 状态。继续操作之前，请查询子网资源，并确保状态为 `available`。如果状态为 `failed`，请联系[支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)并提供详细信息。您可以尝试继续供应其他子网。

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## 步骤 12：创建公共网关

要允许子网中的虚拟服务器实例有权访问公用因特网，请将公共网关 (PGW) 添加到子网。  

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

将公共网关标识存储在变量中。

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

为了连接并使用公共网关，公共网关必须处于 `available` 状态。继续操作之前，请查询公共网关资源，并确保状态为 `available`。如果状态为 `failed`，请联系[支持人员](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)并提供详细信息。您可以尝试继续供应其他公共网关。

要检查公共网关的状态，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 步骤 13：将公共网关连接到子网

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## 步骤 14：创建 SSH 密钥

使用公用 SSH 密钥创建密钥。创建虚拟服务器实例时，将使用此密钥。此外，登录到虚拟服务器实例时，也需要此密钥。

在 UI 或 CLI 中创建 VPC 之初，可以添加密钥。以后没有任何工具可用于添加密钥。
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

将密钥标识存储在变量中。

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## 步骤 15：为虚拟服务器实例选择概要文件和映像

运行 API 以列出可用于虚拟服务器实例的所有概要文件和映像，然后选择组合。

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

要获取有关概要文件的其他详细信息，可以使用 {{site.data.keyword.cloud_notm}} CLI 命令 `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]` 来查询全局目录。例如：

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

以下命令将列出可用的映像。

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

将概要文件名称和映像标识存储在变量中，供应虚拟服务器实例时将使用这些变量。

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## 步骤 16：供应虚拟服务器实例

在新创建的子网中供应虚拟服务器实例 (VSI)。请传入公用 SSH 密钥，以便可以在供应实例后登录。

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

将虚拟服务器实例标识存储在变量中。

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## 步骤 17：检查虚拟服务器实例的状态

供应虚拟服务器实例可能需要最长几分钟时间。继续操作之前，请查询服务器的状态，并确保状态为 `running`。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

将虚拟服务器实例的主网络接口标识（在先前的 API 调用中返回）存储在变量中。  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

除非查询特定实例，否则无法获取主网络接口。
{: note}

## 步骤 18：创建浮动 IP

要为虚拟服务器实例创建浮动 IP，请将实例的主网络接口用作新浮动 IP 地址的目标。

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


将浮动 IP 标识存储在变量中。

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## 步骤 19：向缺省安全组添加允许 SSH 流量的规则

配置安全组，以定义针对实例允许的入站和出站流量。

查找 VPC 的安全组：

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

将安全组标识存储在变量中。

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

现在，创建允许入站 SSH 流量的规则：

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

## 步骤 20：通过 SSH 登录到虚拟服务器实例

要通过 SSH 登录到服务器，请使用先前创建的浮动 IP 的 `address`。要获取浮动 IP 的 `address`，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

使用浮动 IP 的 `address` 可通过 SSH 连接到虚拟服务器实例：

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

安全组可能会阻止通过 SSH 访问虚拟服务器。如果需要进行 SSH 访问，那么必须使用[相应的安全组](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)。
{: note}

请参阅[连接到使用 Linux 的实例](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance)，以获取有关如何连接到实例的更多信息。

## 步骤 21：创建并连接块存储卷

使用类似于此示例的请求来创建块存储卷，并指定卷名、专区和概要文件。

要查看卷概要文件的列表，请提供以下请求：

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

概要文件可以是 general-purpose (3 IOPS/GB)、5iops-tier、10iops-tier 和 custom。有关基于所选卷概要文件的卷容量和 IOPS 范围的信息，请参阅[关于 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)。

您不必在请求中提供卷名，缺省情况下系统会创建卷名。此示例指定了卷名 `helloworld-vol`。所有卷名都必须唯一。

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

对于其余调用，您需要知道新创建的卷的标识。将卷的标识保存为变量，例如：`vol=640774d7-2adc-4609-add9-6dfd96167a8f`。

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

卷创建之初，其状态为 `pending`。卷的状态必须移至 `available` 后（这需要几分钟时间），您才能继续操作。
{: note}


要检查卷的状态，请运行以下命令：

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

创建卷连接，以将新卷连接到虚拟服务器实例。使用您先前在请求中创建的实例标识变量。在数据参数中使用卷标识、卷的 CRN 或 URL 来指定卷。此示例使用的是为卷标识创建的变量。

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

创建卷连接后，获取卷连接标识。

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

将连接标识保存为变量。

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

指定卷连接标识，以将新卷连接到虚拟服务器实例。

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## 步骤 22（可选）：删除资源

（可选）删除资源。如果某个资源包含其他资源，那么无法删除该资源。例如，如果虚拟私有云包含子网，那么无法删除该虚拟私有云；如果子网包含虚拟服务器实例，那么无法删除该子网。执行删除操作时，API 可能会快速返回，但资源删除操作可能仍在进行中。发出删除请求后，请确保资源已删除，然后再尝试删除父资源。有关更多详细信息，请参阅[删除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)。

### 删除浮动 IP

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 删除虚拟服务器实例

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 删除密钥

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 删除子网

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 删除公共网关

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### 删除 VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 删除卷

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## 祝贺您！

您已使用 REST API 成功供应了虚拟私有云实例。要试用更多 API 命令，请探索完整的参考：

* [VPC 的 API 参考](https://{DomainName}/apidocs/vpc-on-classic)
