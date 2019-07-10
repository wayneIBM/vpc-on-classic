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

# 다른 지역에 VPC 작성
{: #creating-a-vpc-in-a-different-region}

지역은 앱, 서비스 및 기타 {{site.data.keyword.cloud}} 리소스를 배치할 수 있는 특정한 지리적 위치입니다. 지역은 하나 이상의 구역으로 구성되어 있으며, 구역은 호스트 서비스 및 애플리케이션에 대한 컴퓨팅, 네트워크 및 스토리지 리소스와 관련 냉각 및 전원 공급 장치를 보관하는 실제 데이터 센터입니다. 구역은 지역 내에 공유 단일 장애 지점이 없도록 서로 격리되어 있습니다. 

IBM Cloud VPC 서비스는 지역적입니다. 재해 복구(DR)를 허용하려면 다른 지역 중 하나에서 VPC를 설정하여 대체 지역으로의 장애 복구를 통해 복구할 수 있는 기능을 제공해야 합니다.
{: note}

Virtual Private Cloud는 모든 {{site.data.keyword.cloud}} 지역에 단계적으로 배치됩니다. 

| 위치     | 지역 | API 엔드포인트 |상태 |
| ------- | :------: | :------: |:------: |
|Dallas | us-south | `us-south.iaas.cloud.ibm.com`| 사용 가능 |
| 프랑크푸르트 | eu-de | `eu-de.iaas.cloud.ibm.com`| 사용 가능 |
| 도쿄 | jp-tok | `jp-tok.iaas.cloud.ibm.com`| 사용 가능 |
| 런던 | eu-gb | `eu-gb.iaas.cloud.ibm.com`| 출시 예정 |
| 시드니 | au-syd | `au-syd.iaas.cloud.ibm.com`| 출시 예정 |
| 워싱턴 DC | us-east | `us-east.iaas.cloud.ibm.com`| 출시 예정 |

지역 API(VPC) 엔드포인트는 특정 지역에 로그인하면 IBM Cloud CLI에 의해 자동으로 설정됩니다.
{: note}

## CLI를 사용하여 특정 지역에 로그인
{: #log-in-to-a-specific-region-using-the-cli}

IBM Cloud에 로그인할 때 지역을 지정하거나 나중에 이를 선택할 수 있습니다. 예를 들어, 댈러스(`us-south`) 지역의 글로벌 API 엔드포인트에 직접 로그인하려면 연합 계정(SSO) 또는 비연합 계정이 있는지에 따라 다음 명령을 실행하십시오. 

연합 계정의 경우:

```
ibmcloud login -a https://cloud.ibm.com -r us-south --sso
```
{: pre}

비연합 계정의 경우:

```
ibmcloud login -a https://cloud.ibm.com -r us-south
```
{: pre}

지역을 나중에 선택하려면 `-r <region>` 매개변수를 지정하지 마십시오. 그러면 CLI에서 지역을 선택하라는 프롬프트를 표시합니다.

출력 예:

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

## CLI를 사용하여 지역 전환
{: #switch-regions-using-the-cli}

VPC 지역의 최신 상태를 가져오려면 다음 명령을 실행하십시오.

```
ibmcloud is regions
```
{: pre}

다른 지역으로 전환하려면 `ibmcloud target -r <region>` 명령을 실행하십시오. 예를 들어, 프랑크푸르트 지역으로 전환하려면 다음 명령을 실행하십시오.

```
ibmcloud target -r eu-de
```
{: pre}

현재 위치를 확인하려면 다음 명령을 실행하십시오.

```
ibmcloud target
```
{: pre}

## API를 사용하여 지역 전환  
{: #switch-regions-using-the-api}

REST를 사용하여 지역 VPC API와 상호작용하려면 리소스를 작성할 지역과 연관된 API 엔드포인트에 요청을 전달하십시오. 지역의 API 엔드포인트는 이전 표에 표시되어 있습니다. 또한 다음 명령을 실행하여 지역과 연관된 엔드포인트를 찾을 수 있습니다.

```
ibmcloud is regions
```
{: pre}


예를 들어, `us-south` 지역에 있는 VPC의 목록을 얻으려면 다음 명령을 실행하십시오.

```
curl "https://us-south.iaas.cloud.ibm.com/v1/vpcs?version=2019-05-31&generation=1" -H "Authorization: $iam_token"
```
{: pre}


## 구역 가져오기
{: #get-zones}

각 지역에 사용 가능한 구역의 목록을 가져오려면 `ibmcloud is zones <region>` 명령을 실행하십시오. 예를 들어, `us-south` 지역에 있는 구역의 목록을 얻으려면 다음 명령을 실행하십시오.

```
ibmcloud is zones us-south
```
{: pre}
