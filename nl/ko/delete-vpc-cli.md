---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# IBM Cloud CLI를 사용하여 VPC 삭제
{: #deleting-using-cli}

이 주제에서는 모든 VPC 리소스에 대해 제안된 순서대로 {{site.data.keyword.cloud}} Virtual Private Cloud에서 리소스를 삭제하는 방법에 대한 예제를 제공합니다. 

삭제에 대해 기억해야 할 몇 가지 중요한 사실은 다음과 같습니다.

* 비어 있어야 VPC를 삭제할 수 있습니다. 
* 상위 리소스를 삭제하기 전에 먼저 모든 포함 리소스를 삭제해야 합니다. 
* 리소스 삭제가 보류 중이면 목록 조회에서 볼 때 `deleting` 상태로 표시됩니다.  
* 대부분의 삭제 요청은 _비동기_입니다. 즉, 리소스가 아직 완전히 삭제되지 않은 경우에는 목록 조회에 **삭제 중** 상태로 표시될 수 있습니다. 
* 상위 리소스를 삭제하려고 시도하기 전에 폴링하여 하위 리소스가 더 이상 목록 보기에 없는지 확인해야 합니다. 

리소스의 상태가 `deleting`에서 `failed`로 변경되는 경우 리소스 삭제를 다시 시도할 수 있습니다. `failed` 상태의 리소스를 삭제할 수 없으면 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.
{: tip}

## 1단계: 삭제할 VPC 내의 모든 서브넷 찾기
{: #deleting-find-subnets-cli}

VPC를 삭제하기 전에 먼저 각각의 서브넷을 삭제해야 합니다. 다음 명령을 실행하고 `ID` 값을 확인하여 삭제할 VPC의 ID를 가져오십시오.

```
ibmcloud is vpcs
```
{: pre}

나중에 사용할 수 있도록 VPC의 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

계정에 있는 모든 서브넷의 목록을 가져오려면 다음 명령을 실행하십시오.

```
ibmcloud is subnets
```
{: pre}

`VPC` 값을 확인하여 삭제되어야 하는 서브넷을 판별하십시오. 나중에 사용할 수 있도록 서브넷의 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

삭제할 VPC에 다중 서브넷이 있는 경우 이러한 단계를 반복하여 각 서브넷을 삭제하십시오.
{: important}

## 2단계: VPC의 각 서브넷 삭제
{: #deleting-subnet-resources-cli}

### 서브넷의 모든 VPN 게이트웨이 삭제(있는 경우)

계정에 있는 모든 VPN 게이트웨이를 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is vpn-gateways
```
{: pre}

삭제할 서브넷의 각 VPN 게이트웨이에 대해 다음 명령을 실행하십시오. 여기서 `$vpnid`는 VPN 게이트웨이의 ID입니다.

```
ibmcloud is vpn-gateway-delete $vpnid
```

VPN 게이트웨이의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 결과에 표시됩니다. VPN 게이트웨이를 삭제하는 데 최대 30분이 걸릴 수 있습니다.
{: note}

VPN 게이트웨이가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있습니다. 그러나 VPN 게이트웨이 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회 결과에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

### 서브넷의 모든 로드 밸런서 삭제(있는 경우)
{: #deleting-lbs-cli}

계정에 있는 모든 로드 밸런서를 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is load-balancers
```
{: pre}

삭제할 서브넷의 각 로드 밸런서에 대해 다음 명령을 실행하십시오. 여기서 `$lbid`는 로드 밸런서의 ID입니다.

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

로드 밸런서의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 결과에 표시됩니다. 로드 밸런서를 삭제하는 데 최대 30분이 걸릴 수 있습니다.
{: note}

로드 밸런서가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있습니다. 그러나 로드 밸런서 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회 결과에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

### 서브넷의 모든 네트워크 인터페이스 삭제(있는 경우)
{: #deleting-nics-cli}

인스턴스에 다중 네트워크 인터페이스가 있을 수 있으며 해당 네트워크 인터페이스가 다중 VPC 서브넷에 속할 수 있습니다. 서브넷을 삭제하려면 먼저 서브넷의 네트워크 인터페이스를 삭제해야 합니다. 

서브넷의 네트워크 인터페이스를 삭제하는 유일한 방법은 인스턴스를 삭제하는 것입니다.
{: note}

계정의 있는 모든 인스턴스를 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is instances
```
{: pre}

VPC의 모든 인스턴스를 삭제할 경우 VPC의 각 인스턴스에 대해 다음 명령을 실행할 수 있습니다. 여기서 `$vsi`는 삭제할 인스턴스의 ID입니다. 인스턴스가 삭제되면 다른 서브넷의 모든 네트워크 인터페이스(있는 경우)가 자동으로 삭제됩니다.

```
ibmcloud is instance-delete $vsi
```
{: pre}

인스턴스의 상태가 즉시 `deleting`으로 변경되어야 하지만 여전히 목록 조회 결과에 표시됩니다. 인스턴스를 삭제하는 데 최대 30분이 걸릴 수 있습니다.
{: note}

인스턴스가 삭제될 때까지 대기하는 동안 병렬로 다른 서브넷 리소스가 삭제되도록 요청할 수 있습니다. 그러나 인스턴스 및 서브넷의 다른 모든 리소스가 더 이상 목록 조회 결과에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.

인스턴스의 모든 네트워크 인스턴스를 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

삭제하려는 서브넷에 두 번째 네트워크 인터페이스가 있는 경우 해당 인스턴스를 삭제해야 합니다. 인스턴스를 삭제하지 않고 인스턴스에서 네트워크 인터페이스를 삭제할 수 없습니다.

### 서브넷 삭제
{: #deleting-subnet-cli}

서브넷 내부의 모든 리소스가 삭제되면(즉, 목록 조회 결과에 리턴되지 않음) 다음 명령을 실행하여 서브넷을 삭제하십시오.

```
ibmcloud is subnet-delete $subnet
```
{: pre}

서브넷의 상태가 즉시 `deleting`으로 변경되어야 하지만 서브넷이 실제로 삭제되고 더 이상 목록 조회 결과에 표시되지 않을 때까지 몇 분이 걸릴 수 있습니다.

## 3단계: VPC의 모든 공용 게이트웨이 삭제(있는 경우)
{: #deleting-pgs-cli}

계정에 있는 모든 공용 게이트웨이를 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is public-gateways
```
{: pre}

삭제할 VPC의 각 공용 게이트웨이에 대해 다음 명령을 실행하여 공용 게이트웨이를 삭제하십시오. 여기서 `$gateway`는 공용 게이트웨이 ID입니다.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## 4단계: VPC 삭제
{: #deleting-single-vpc-cli}

VPC의 모든 서브넷, VPN 게이트웨이, 로드 밸런서, 공용 게이트웨이가 삭제되면 다음 명령을 실행하여 VPC를 삭제하십시오. 여기서 `$vpc`는 삭제할 VPC의 ID입니다.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

VPC의 상태가 즉시 `deleting`으로 변경되어야 하지만 VPC가 실제로 삭제되고 더 이상 목록 조회 결과에 표시되지 않을 때까지 몇 분이 걸릴 수 있습니다.
