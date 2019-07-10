---
copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: vpc, classic, access, API, CLI, limitations

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# VPC에서 클래식 인프라로의 액세스 설정
{: #setting-up-access-to-your-classic-infrastructure-from-vpc}
[comment]: # (링크된 도움말 항목)

임의의 계정에 대해, 각 지역에 있는 하나의 VPC에서 {{site.data.keyword.cloud}} 클래식 인프라로의 액세스(Direck Link 연결 포함)를 설정할 수 있습니다. 이러한 특수 "클래식 액세스 VPC"는 {{site.data.keyword.cloud}} 클래식 인프라와 동일한 라우팅 기능(내재적 라우터)을 사용합니다. 독자는 클래식 인프라 네트워킹에 익숙해야 합니다.

클래식 액세스를 위한 VPC를 설정하고 나면 클래식 계정의 공용 인터페이스가 없는 모든 컴퓨팅 호스트(VSI 또는 베어메탈)가 클래식 액세스 VPC를 대상으로 패킷을 송수신할 수 있습니다. 그러나 방화벽, 게이트웨이, 네트워크 ACL 또는 보안 그룹이 이 트래픽을 필터링할 수 있다는 점을 기억하십시오. 애플리케이션이 올바르게 작동하는 데 필요한 트래픽만 허용하는 것이 좋습니다.

공용 인터페이스가 있는 호스트에서는 클래식 사용 VPC에 라우트를 다시 추가해야 합니다.
{: important}

## 전제조건:
{: #vpc-prerequisites}

1. 클래식 계정이 IBM Cloud 계정과 연결되어 있어야 합니다. 이를 수행하는 방법에 대한 지시사항은 [IBMid 계정 연결](/docs/account?topic=account-unifyingaccounts)을 참조하십시오.
1. 클래식 계정이 VRF를 사용하도록 설정되어 있어야 합니다.
    * 계정에 이미 Direct Link가 있는 경우에는 준비가 된 것입니다.
    * 계정이 VRF를 사용하도록 설정되지 않은 경우에는 계정에 대한 "VRF 마이그레이션"을 요청하는 티켓을 여십시오. 변환 프로세스에 대해 더 자세히 알아보려면 Direck Link 문서의 [변환 시작 방법](/docs/infrastructure/direct-link?topic=direct-link-how-you-can-initiate-the-conversion#how-you-can-initiate-the-conversion)을 참조하십시오.

방화벽, 게이트웨이, 네트워크 ACL 또는 보안 그룹은 클래식 리소스와 VPC 리소스 간의 네트워크 트래픽을 일부 또는 전부 필터링할 수 있습니다.
{: important}

클래식 액세스를 사용하는 VPC의 모든 서브넷이 `10.0.0.0/8` 공간의 IP 주소를 사용하는 클래식 인프라 VRF로 공유됩니다. IP 주소 충돌을 방지하려면 클래식 액세스 VPC에서 서브넷을 작성할 때 `10.0.0.0/8` 공간의 IP 주소를 **사용하지 마십시오**.
{: important}

## 클래식 액세스 VPC 작성
{: #create-a-classic-access-vpc}

사용자 인터페이스(UI), 명령행 인터페이스(CLI) 또는 API(Application Programming Interface)를 사용하여 클래식 액세스 기능이 있는 VPC를 작성할 수 있습니다.

VPC는 작성될 때 클래식 액세스를 위해 설정되어야 합니다. 클래식 액세스 기능을 추가하거나 제거하기 위해 VPC를 업데이트할 수는 없습니다.
{: note}

### 사용자 인터페이스에서 클래식 액세스 VPC 작성
{: #create-a-classic-access-vpc-from-the-user-interface}

**VPC 작성** 페이지에서 **클래식 액세스** 레이블 아래에 있는 **클래식 리소스에 대한 액세스 사용**이라는 선택란을 클릭하여 클래식 액세스 VPC를 작성하십시오.

![classic-access-ui](/images/classic-access-ui.png)

### CLI를 사용하여 클래식 액세스 VPC 작성
{: #create-a-classic-access-vpc-using-the-cli}

VPC를 작성할 때 플래그 `--classic-access`를 사용하십시오. 예를 들면 다음과 같습니다.

```
ibmcloud is vpc-create my-access-vpc --classic-access
```
{: pre}


### API를 사용하여 클래식 액세스 VPC 작성
{: #create-a-classic-access-vpc-using-the-api}

VPC를 작성할 때 `classic_access` 매개변수를 전달하십시오. 예를 들면 다음과 같습니다.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=2019-05-31&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-access-vpc",
        "classic_access": true
      }'
```
{: pre}


### 클래식 액세스 VPC 기본 주소 접두부
{: #classic-access-default-address-prefixes}

클래식 액세스를 사용하는 VPC의 모든 서브넷이 `10.0.0.0/8` 공간의 IP 주소를 사용하는 클래식 인프라 VRF로 공유됩니다. IP 주소 충돌을 방지하려면 클래식 액세스 VPC에서 서브넷을 작성할 때 `10.0.0.0/8` 공간의 IP 주소를 **사용하지 마십시오**.
{: important}

새 클래식 액세스 VPC가 작성될 때 다음 표에 표시된 대로 지역 및 구역을 기본으로 하여 기본 주소 접두부가 지정됩니다.

구역         | 주소 접두부 
---------------|---------------
`us-south-1`   | `172.16.0.0/18`
`us-south-2`   | `172.16.64.0/18`
`us-south-3`   | `172.16.128.0/18`
`us-east-1`    | `172.17.0.0/18`
`us-east-2`    | `172.17.64.0/18`
`us-east-3`    | `172.17.128.0/18`
`eu-gb-1`      | `172.18.0.0/18`
`eu-gb-2`      | `172.18.64.0/18`
`eu-gb-3`      | `172.18.128.0/18`
`eu-de-1`      | `172.19.0.0/18`
`eu-de-2`      | `172.19.64.0/18`
`eu-de-3`      | `172.19.128.0/18`
`jp-tok-1`     | `172.20.0.0/18`
`jp-tok-2`     | `172.20.64.0/18`
`jp-tok-3`     | `172.20.128.0/18`
`au-syd-1`     | `172.21.0.0/18`
`au-syd-2`     | `172.21.64.0/18`
`au-syd-3`     | `172.21.128.0/18`


## 제한사항
{: #vpc-limitations}

* 사설(이전 문서에서는 "백") 네트워크만 계정의 사설 내재적 라우터에 연결됩니다. 
* 클래식 API를 사용하여 할당된 서브넷만 사설 내재적 라우터의 클래식 부분에 연결됩니다. 내재적 라우터의 라우팅 기능은 클래식 VLAN의 서브넷 간에 작동하지 않습니다.
* 계정 및 지역당 하나의 VPC만 클래식 액세스를 사용하도록 설정될 수 있습니다.

클래식 VSI 또는 Bare Metal Server에 설치한 OS에 따라 구성 프로시저가 다릅니다. 자세한 정보는 [공용 가상 서버 정보](https://cloud.ibm.com/docs/vsi?topic=virtual-servers-about-public-virtual-servers)를 참조하십시오.
{: note}
