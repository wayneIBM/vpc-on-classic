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

# REST API를 사용하여 VPC 작성
{: #creating-a-vpc-using-the-rest-apis}

이 문서에서는 `curl`을 사용하여 REST API를 통해 {{site.data.keyword.cloud}} Virtual Private Cloud 리소스를 작성하는 방법을 보여줍니다. Go 및 Python을 사용하여 REST API를 호출하는 코드 스니펫은 이 [예제 코드](https://github.com/IBM-Cloud/vpc-api-samples)를 참조하십시오.

## 전제조건

1. 가상 서버 인스턴스에 연결하는 데 사용될 공개 SSH 키가 있는지 확인하십시오.

   공개 SSH 키를 이미 보유하고 있는 경우가 있습니다. 홈 디렉토리 아래의 `.ssh` 디렉토리에서 `id_rsa.pub`라는 파일을 찾으십시오(예: `/Users/<USERNAME>/.ssh/id_rsa.pub`). 이 파일은 `ssh-rsa`로 시작하며 사용자의 이메일 주소로 끝납니다.

   공개 SSH 키가 없거나 기존 키의 비밀번호를 잊어버린 경우에는 `ssh-keygen` 명령을 실행한 후 프롬프트에 따라 새 키를 생성하십시오.
2.  IBM Cloud 계정에 대한 API 키가 있는지 확인하십시오. API 키가 없는 경우 [API 키 작성](/docs/iam?topic=iam-userapikey#create_user_key)을 참조하십시오. 1단계에서 이 API 키를 환경 변수에 저장해야 합니다.

## 1단계: API 키를 변수로 저장

다음 명령을 실행하여 API 키를 환경 변수에 저장하십시오.

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## 2단계: IBM Identity and Access Management(IAM) 토큰 가져오기

IAM 토큰을 가져오거나 다음 예제 명령을 사용하는 방법은 [API 키를 사용하여 IBM Cloud IAM 토큰 가져오기](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) 주제를 참조하십시오.

다음 명령을 실행하여 IAM 토큰을 가져와서 `jq` 유틸리티를 통해 구문 분석하십시오. 다른 구문 분석 도구를 사용하도록 명령을 수정하거나 직접 수동으로 토큰을 구문 분석하려는 경우 명령의 마지막 부분을 제거할 수 있습니다.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

IAM 토큰을 보려면 `echo $iam_token`을 실행하십시오. 결과는 다음과 같이 표시되어야 합니다.

```
Bearer <your token>
```

권한 부여 헤더는 IAM 토큰이 `Bearer`로 시작될 것으로 예상합니다. 받은 결과에 `Bearer`가 포함되어 있지 않으면 이를 포함하도록 `iam_token` 변수를 업데이트하십시오. 이 문서의 예에서는 `Bearer`가 `iam_token`에 포함되어 있다고 가정합니다.

IAM 토큰은 만료되므로 각 시간마다 이전 단계를 반복하여 토큰을 새로 고쳐야 합니다.
{: important}

## 3단계: API 엔드포인트 및 version 매개변수를 변수로 저장

API 엔드포인트는 지역에 따라 다르며 `https://<region>.iaas.cloud.ibm.com` 규칙을 따릅니다. 지역별 엔드포인트는 다음과 같습니다.

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

전체 URL을 입력할 필요 없이 API 요청에서 사용할 수 있도록 API 엔드포인트를 변수에 저장하십시오. 예를 들어, us-south 엔드포인트를 변수에 저장하려면 다음 명령을 실행하십시오.

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

이 변수가 저장되었는지 확인하려면 `echo $rias_endpoint`를 실행하고 응답이 비어 있지 않은지 확인하십시오.

모든 API 요청에 `YYYY-MM-DD` 형식의 `version` 매개변수가 포함되어야 합니다. 다음 명령을 실행하여 세션에서 재사용할 수 있도록 버전 날짜를 변수에 저장하십시오. `version` 매개변수 설정에 대한 자세한 정보는 [VPC에 대한 API 참조](https://{DomainName}/apidocs/vpc-on-classic#versioning)의 **버전화**를 참조하십시오.

```bash
version="2019-05-31"
 ```
{: pre}

이 변수가 저장되었는지 확인하려면 `echo $version`을 실행하고 응답이 비어 있지 않은지 확인하십시오. 

## 4단계: GET regions API 실행

다음 명령은 VPC에 대해 사용할 수 있는 지역을 JSON 형식으로 리턴합니다. 하나 이상의 오브젝트가 리턴되어야 합니다.

각 API 요청에서 `version` 및 `generation` 조회 매개변수는 필수입니다. 세부사항은 [VPC에 대한 API 참조](https://{DomainName}/apidocs/vpc-on-classic)를 참조하십시오.
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

예상치 않은 결과가 발생하는 경우에는 `curl` 명령 뒤에 `--verbose`(디버그) 플래그를 추가하여 자세한 로깅 정보를 얻으십시오. 또한 [문제점 해결 페이지](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc)에서 일반적으로 발생하는 오류를 참조하십시오.
{: tip}


## 5단계: GET zones API 실행

다음 명령은 `us-south` 지역에서 VPC에 사용할 수 있는 모든 구역을 JSON 형식으로 리턴합니다. 세 개 이상의 오브젝트가 리턴되어야 합니다.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

사용 중인 지역 엔드포인트에 따라 위에 있는 매개변수의 지역 이름 `us-south`를 업데이트하십시오. 지역 엔드포인트는 자체 구역에 대해서만 알고 있으므로 도쿄의 엔드포인트를 사용 중인 경우 `jp-tok`를 사용하십시오.
{: note}

## 6단계: GET profiles API 실행

다음 명령은 인스턴스에 대해 사용할 수 있는 프로파일을 JSON 형식으로 리턴합니다. 하나 이상의 오브젝트가 리턴되어야 합니다.

읽을 수 있는 JSON 문자열을 얻으려면 curl 명령 뒤에 ` | json_pp `를 추가하십시오.
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

페이지네이션으로 인해 API 출력이 제한되는 경우에는 한계를 `total_count`보다 더 높게 설정하십시오. 예를 들면 다음과 같습니다.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## 7단계: GET images API 실행

다음 명령은 인스턴스에 대해 사용할 수 있는 이미지를 JSON 형식으로 리턴합니다. 하나 이상의 오브젝트가 리턴되어야 합니다.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 8단계: GET VPCs API 실행

다음 명령은 사용자의 계정에서 작성된 VPC를 JSON 형식으로 리턴합니다.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 9단계: Virtual Private Cloud 작성

`my-vpc`라는 {{site.data.keyword.cloud_notm}} VPC를 작성하십시오.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

나머지 호출을 위해서는 새로 작성된 {{site.data.keyword.cloud_notm}} VPC의 ID를 알아야 합니다. 해당 ID를 변수에 저장하십시오. 이 명령은 다음과 같이 표시됩니다. `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## 10단계: 서브넷 작성

{{site.data.keyword.cloud_notm}} VPC에 서브넷을 작성하십시오. 다음 예에서는 `us-south-2` 구역에 VPC를 작성합니다.

다음 예에서는 `us-south-2` 구역에 대해 [기본 주소 접두부](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)를 사용합니다. 사용자 계정에 대한 기본값이 변경되었을 수 있습니다. [VPC 주소 계획 방법](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design)을 참조하십시오.
{: note}

vpc에 대한 모든 주소 접두부를 가져오려면 `curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"` 명령을 실행하십시오.
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

서브넷 ID를 변수에 저장하십시오.

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## 11단계: 서브넷의 상태 확인

서브넷에 리소스를 프로비저닝하려면 서브넷이 `available` 상태여야 합니다. 계속하기 전에 서브넷 리소스를 조회하여 상태가 `available`인지 확인하십시오. 상태가 `failed`인 경우에는 세부사항을 [지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 전달하여 문의하십시오. 다른 서브넷을 프로비저닝하여 진행할 수도 있습니다.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## 12단계: 공용 게이트웨이 작성

서브넷의 가상 서버 인스턴스가 공용 인터넷에 액세스할 수 있도록 하려면 서브넷에 공용 게이트웨이(PGW)를 추가하십시오.  

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

공용 게이트웨이 ID를 변수에 저장하십시오.

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

공용 게이트웨이를 연결하고 사용하려면 공용 게이트웨이가 `available` 상태여야 합니다. 계속하기 전에 공용 게이트웨이 리소스를 조회하여 상태가 `available`인지 확인하십시오. 상태가 `failed`인 경우에는 세부사항을 [지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 전달하여 문의하십시오. 다른 공용 게이트웨이를 프로비저닝하여 계속하려고 시도할 수 있습니다.

공용 게이트웨이의 상태를 확인하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## 13단계: 공용 게이트웨이를 서브넷에 연결

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## 14단계: SSH 키 작성

공개 SSH 키로 키를 작성하십시오. 이 키는 가상 서버 인스턴스를 작성할 때 사용됩니다. 또한 이 키는 가상 서버 인스턴스에 로그인하는 데 필요합니다.

UI 또는 CLI에서 처음으로 VPC를 작성할 때 키를 추가할 수 있습니다. 나중에 키를 추가할 수 있는 도구를 없습니다.
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

키 ID를 변수에 저장하십시오.

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## 15단계: 가상 서버 인스턴스의 프로파일 및 이미지 선택

API를 실행하여 가상 서버 인스턴스에 대해 사용할 수 있는 모든 프로파일 및 이미지를 나열하고 조합을 선택하십시오.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

프로파일에 대한 추가 세부사항을 얻으려는 경우에는 {{site.data.keyword.cloud_notm}} CLI 명령 `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`을 사용하여 글로벌 카탈로그를 조회할 수 있습니다. 예를 들면 다음과 같습니다.

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

다음 명령은 사용 가능한 이미지를 나열합니다.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

가상 서버 인스턴스를 프로비저닝하는 데 사용할 변수에 프로파일 이름 및 이미지 ID를 저장하십시오.

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## 16단계: 가상 서버 인스턴스 프로비저닝

새로 작성된 서브넷에서 가상 서버 인스턴스(VSI)를 프로비저닝하십시오. 인스턴스가 프로비저닝된 후 로그인할 수 있도록 공개 SSH 키를 전달하십시오.

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

가상 서버 인스턴스 ID를 변수에 저장하십시오.

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## 17단계: 가상 서버 인스턴스의 상태 확인

가상 서버 인스턴스를 프로비저닝하는 데 최대 몇 분이 걸릴 수 있습니다. 계속하기 전에 서버의 상태를 조회하여 `running`인지 확인하십시오.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

이전 API 호출에서 리턴된 가상 서버 인스턴스의 기본 네트워크 인터페이스 ID를 변수에 저장하십시오.  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

특정 인스턴스를 조회하기 전까지는 기본 네트워크 인터페이스를 가져올 수 없습니다.
{: note}

## 18단계: 유동 IP 작성

가상 서버 인스턴스의 유동 IP를 작성하려면 인스턴스의 기본 네트워크 인터페이스를 새 유동 IP 주소의 대상으로 사용하십시오.

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


유동 IP ID를 변수에 저장하십시오.

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## 19단계: 기본 보안 그룹에 SSH를 허용하기 위한 규칙 추가

보안 그룹을 구성하여 인스턴스에 허용되는 인바운드 및 아웃바운드 트래픽을 정의하십시오.

VPC의 보안 그룹을 찾으십시오.

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

보안 그룹 ID를 변수에 저장하십시오.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

이제 인바운드 SSH 트래픽을 허용하는 규칙을 작성하십시오. 

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

## 20단계: 가상 서버 인스턴스에 SSH로 연결

서버에 SSH로 연결하려면 이전에 작성한 유동 IP의 `address`를 사용하십시오. 유동 IP의 `address`를 가져오려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

유동 IP의 `address`를 사용하여 SSH로 가상 서버 인스턴스에 연결하십시오.

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

Virtual Server에 대한 SSH 액세스는 보안 그룹에 의해 차단될 수 있습니다. SSH 액세스가 필요한 경우에는 [적절한 보안 그룹](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)을 사용해야 합니다.
{: note}

인스턴스에 연결하는 방법에 대한 자세한 정보는 [Linux를 사용하는 인스턴스에 연결](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance)을 참조하십시오.

## 21단계: 블록 스토리지 볼륨 작성 및 연결

이 예제와 유사한 요청으로 블록 스토리지 볼륨을 작성하고 볼륨 이름, 구역 및 프로파일을 지정하십시오.

볼륨 프로파일의 목록을 보려면 이 요청을 제공하십시오.

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

프로파일은 general-purpose(3IOPS/GB), 5iops-tier, 10iops-tier 및 custom일 수 있습니다. 사용자가 선택한 볼륨 프로파일을 기반으로 하는 볼륨 용량 및 IOPS 범위에 대한 정보는 [Block Storage for VPC 정보](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)를 참조하십시오.

요청에 볼륨 이름을 제공할 필요가 없습니다. 기본적으로 시스템에서 작성합니다. 이 예제에서는 볼륨 이름을 `helloworld-vol`로 지정합니다.  모든 볼륨 이름은 고유해야 합니다.

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

나머지 호출을 위해서는 새로 작성된 볼륨의 ID를 알아야 합니다. 볼륨의 ID를 변수로 저장하십시오. 예를 들면 다음과 같습니다. `vol=640774d7-2adc-4609-add9-6dfd96167a8f`

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

처음 작성될 때 볼륨의 상태는 `pending`입니다. 진행하려면 먼저 볼륨이 `available` 상태로 변환되어야 하며, 여기에는 몇 초가 걸립니다.
{: note}


볼륨의 상태를 확인하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

새 볼륨을 가상 서버 인스턴스에 연결하려면 볼륨 연결을 작성하십시오. 요청에서 이전에 작성한 인스턴스 ID 변수를 사용하십시오. 볼륨 ID, 볼륨의 CRN 또는 URL을 사용하여 데이터 매개변수에 볼륨을 지정하십시오. 이 예에서는 볼륨 ID에 대해 작성한 변수를 사용합니다.

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

볼륨 연결을 작성한 후 볼륨 연결 ID를 가져오십시오.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

연결 ID를 변수로 저장하십시오.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

새 볼륨을 가상 서버 인스턴스에 연결하려면 볼륨 연결 ID를 지정하십시오.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## 22단계(선택사항): 리소스 삭제

선택적으로 리소스를 삭제하십시오. 다른 리소스를 포함하는 리소스는 삭제할 수 없습니다. 예를 들어, 서브넷을 포함하는 Virtual Private Cloud는 삭제할 수 없으며, 가상 서버 인스턴스를 포함하는 서브넷은 삭제할 수 없습니다. 삭제 오퍼레이션 시, API는 빠르게 리턴될 수 있지만 리소스 삭제는 계속 진행 중일 수 있습니다. 삭제 요청을 발행한 후 상위 리소스를 삭제하려고 시도하기 전에 리소스가 삭제되었는지 확인하십시오. 세부사항은 [VPC 삭제](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)를 참조하십시오.

### 유동 IP 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 가상 서버 인스턴스 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 키 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 서브넷 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 공용 게이트웨이 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### VPC 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### 볼륨 삭제

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## 축하합니다!

REST API를 사용하여 Virtual Private Cloud 인스턴스를 프로비저닝했습니다. 더 많은 API 명령을 사용해 보려면 전체 참조를 살펴보십시오.

* [VPC에 대한 API 참조](https://{DomainName}/apidocs/vpc-on-classic)
