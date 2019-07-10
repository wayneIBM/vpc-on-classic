---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

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

# REST API를 사용하여 VPC 삭제
{: #deleting-using-api}

REST API를 사용하여 {{site.data.keyword.cloud}} Virtual Private Cloud를 삭제하는 경우 [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)를 사용한 삭제와 동일한 [삭제](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 프로세스의 일반 단계를 따릅니다.

이 프로세스의 기본 단계는 다음과 같습니다.

1. 삭제할 VPC의 모든 서브넷을 찾으십시오.
2. VPC의 각 서브넷을 삭제하십시오. 즉, 다음과 같습니다.
    - 서브넷의 모든 VPN 게이트웨이를 삭제하십시오.
    - 서브넷의 모든 로드 밸런서를 삭제하십시오.
    - 서브넷의 모든 네트워크 인터페이스(인스턴스)를 삭제하십시오.
    - 서브넷을 삭제하십시오.
3. VPC의 모든 공용 게이트웨이를 삭제하십시오.
4. VPC를 삭제하십시오.

다음 섹션에서는 VPC를 삭제하기 위해 실행할 수 있는 `curl`을 사용한 몇 가지 예제 API 호출을 제공합니다.

## 1단계: 삭제할 VPC의 모든 서브넷 찾기
{: #deleting-find-subnets-api}

VPC를 삭제하기 전에 먼저 각각의 서브넷을 삭제해야 합니다. 다음 명령을 실행하고 `id` 값을 확인하여 삭제할 VPC의 id를 가져오십시오.

읽을 수 있는 JSON 문자열을 얻으려면 curl 명령 뒤에 ` | json_pp` 또는 ` | jq`를 추가하십시오.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

나중에 사용할 수 있도록 VPC의 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

계정에 있는 모든 서브넷의 목록을 가져오려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

`vpc` 값을 확인하여 삭제되어야 하는 서브넷을 판별하십시오. 나중에 사용할 수 있도록 서브넷의 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

삭제할 VPC에 다중 서브넷이 있는 경우 이 단계를 반복하여 각 서브넷을 삭제하십시오.
{: important}

## 2단계: VPC의 각 서브넷 삭제
{: #deleting-subnet-resources-api}

### 서브넷의 모든 VPN 게이트웨이 삭제(있는 경우)

계정에 있는 모든 VPN 게이트웨이를 나열하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

삭제할 서브넷의 각 VPN 게이트웨이에 대해 다음 명령을 실행하십시오. 여기서 `$vpnid`는 VPN 게이트웨이의 ID입니다.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPN 게이트웨이의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 수행 시 리턴됩니다. VPN 게이트웨이를 삭제하는 데 최대 30분이 걸릴 수 있습니다. VPN 게이트웨이가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있지만 VPN 게이트웨이 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

### 서브넷의 모든 로드 밸런서 삭제(있는 경우)
{: #deleting-lbs-api}

계정에 있는 모든 로드 밸런서를 나열하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

삭제할 서브넷의 각 로드 밸런서에 대해 다음 명령을 실행하십시오. 여기서 `$lbid`는 로드 밸런서의 ID입니다.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

로드 밸런서의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 수행 시 리턴됩니다. 로드 밸런서를 삭제하는 데 최대 30분이 걸릴 수 있습니다. 로드 밸런서가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있지만 로드 밸런서 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

### 서브넷의 모든 네트워크 인터페이스 삭제(있는 경우)
{: #deleting-nics-api}

인스턴스에 다중 네트워크 인터페이스가 있을 수 있으며 해당 네트워크 인터페이스가 다중 VPC 서브넷에 속할 수 있습니다. 서브넷을 삭제하려면 먼저 서브넷의 네트워크 인터페이스를 삭제해야 합니다. 서브넷의 네트워크 인터페이스를 삭제하는 유일한 방법은 인스턴스를 삭제하는 것입니다.


계정의 있는 모든 인스턴스를 나열하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPC의 모든 인스턴스를 삭제할 경우 VPC의 각 인스턴스에 대해 다음 명령을 실행할 수 있습니다. 여기서 `$vsi`는 삭제할 인스턴스의 id입니다. 인스턴스가 삭제되면 다른 서브넷의 모든 네트워크 인터페이스(있는 경우)가 자동으로 삭제됩니다.

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

인스턴스의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 결과에 표시될 수 있습니다. 인스턴스를 삭제하는 데 최대 30분이 걸릴 수 있습니다. 인스턴스가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있지만 인스턴스 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

인스턴스의 모든 네트워크 인스턴스를 나열하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

삭제하려는 서브넷에 두 번째 네트워크 인터페이스가 있는 경우 해당 인스턴스를 삭제해야 합니다. 인스턴스를 삭제하지 않고 인스턴스에서 네트워크 인터페이스를 삭제할 수 없습니다.

### 서브넷 삭제
{: #deleting-subnet-api}

서브넷 내부의 모든 리소스가 삭제되고 목록 조회 실행 시 리턴되지 않으면 다음 명령을 실행하여 서브넷을 삭제하십시오.

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

서브넷의 상태가 즉시 `deleting`으로 변경되어야 하지만 서브넷이 더 이상 목록 조회에서 리턴되지 않도록 삭제되는 데 몇 분이 걸릴 수 있습니다.

## 3단계: VPC의 모든 공용 게이트웨이 삭제(있는 경우)
{: #deleting-pgs-api}

계정에 있는 모든 공용 게이트웨이를 나열하려면 다음 명령을 실행하십시오.

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

삭제할 VPC의 각 공용 게이트웨이에 대해 다음 명령을 실행하여 공용 게이트웨이를 삭제하십시오. 여기서 `$gateway`는 공용 게이트웨이 ID입니다.

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## 4단계: VPC 삭제
{: #deleting-single-vpc-api}

VPC의 모든 서브넷, VPN 게이트웨이, 로드 밸런서, 공용 게이트웨이가 삭제되면 다음 명령을 실행하여 VPC를 삭제하십시오. 여기서 `$vpc`는 삭제할 VPC의 ID입니다.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

VPC의 상태가 즉시 `deleting`으로 변경되어야 하지만 VPC가 더 이상 목록 조회에 표시되지 않도록 삭제되는 데 몇 분이 걸릴 수 있습니다.
