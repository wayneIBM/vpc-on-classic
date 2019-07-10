---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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

# VPC 인프라 리소스 정보
{: #about-vpc-infrastructure-resources}

용어 _리소스_는 {{site.data.keyword.cloud}} Virtual Private Cloud 배치의 컴포넌트 부분을 가리킵니다. 각 VPC는 필요에 따라 그룹화될 수 있는, 다음 유형의 리소스를 포함할 수 있습니다.

* **vpc**
* **instance**
* **key**
* **image**
* **subnet**
* **volume**
* **network access control lists (acl)**
* **security group**
* **floating IP**
* **vnic**
* **gateway**
* **loadbalancer**
* **vpn**

일부 VPC 인프라 리소스는 상위 VPC에서 사용을 통제하는 권한 부여 정책을 상속하지만 다른 VPC 인프라 리소스는 별도로 관리됩니다.

현재 `instance`, `key`, `security-group`, `volume`, `loadbalancer` 및 `vpn` 리소스 유형은 고유한 권한 부여 정책을 유지합니다. 이러한 리소스에 대한 리소스 권한 부여 관련 세부사항은 이 문서의 뒷부분에 제공됩니다.
{: note}

## 리소스 정책
{: #resource-policies}

일반적으로 VPC에 있는 리소스에 대한 액세스는 _정책_에 의해 제어됩니다. VPC의 리소스에 대한 액세스를 사용자 정의하려는 경우에는 사용자 정의된 정책을 작성할 수 있습니다. 리소스 정책은 다음 항목을 대상으로 할 수 있습니다.

* 모든 리소스
* 특정 유형의 모든 리소스
* 특정 그룹의 모든 리소스
* 개별 리소스

## 리소스 및 리소스 그룹
{: #resources-and-resource-groups}

_리소스 그룹_은 권한 부여 및 사용 설정 목적으로 연관된, 전체 VPC 또는 단일 서브넷 및 해당 ACL 등과 같은 리소스의 집합입니다. 리소스 그룹은 특정 프로젝트, 부서 또는 팀이 사용하는 인프라의 집합으로 생각할 수 있습니다.

검색 CLI를 사용하여 리소스에 첨부된 태그를 표시할 수 있습니다. 적절한 필터를 사용하여 `ibmcloud resource search name:` 또는 `ibmcloud resource search 'service_name:is AND type:vpc'`와 같은 관심 있는 인스턴스만 표시할 수 있습니다.
{: tip}

큰 엔터프라이즈는 하나의 VPC를 리소스 그룹으로 나누려 할 수 있습니다. 작은 회사는 회사의 모든 팀이 VPC 전체를 사용하므로 VPC를 리소스 그룹으로 나눌 필요가 없을 수도 있습니다. _OpenStack_에 익숙한 경우에는 리소스 그룹을 _OpenStack Keystone_의 _프로젝트_와 유사한 개념으로 볼 수 있습니다.

VPC를 작성할 때만 리소스를 리소스 그룹에 지정할 수 있습니다. 리소스는 작성된 후 리소스 그룹을 변경할 수 없습니다.
{: .note}

리소스 그룹은 주로 큰 조직을 위해 디자인되었습니다. 대부분의 회사 및 팀의 경우 리소스는 VPC 단위로 분할됩니다. 이 일반적 경험 법칙은 개별 VSI(인스턴스)를 리소스 그룹에 지정하거나 개별 사용자를 VSI(인스턴스) 리소스에 지정할 수 있는 경우 변경될 수 있습니다.

IBM Cloud VPC를 설정할 때 리소스 그룹을 사용하려는 경우에는 조직 내의 리소스 및 사용자를 각 리소스 그룹에 지정하는 방식에 대한 계획을 미리 세우는 것이 좋습니다.
{: tip}

## VPC 관련 고유 특성
{: #specifics-for-vpc}

일부 VPC 인프라 리소스는 계정 또는 상위 VPC에서 사용을 통제하는 권한 부여 정책을 상속하는 반면 다른 리소스에는 개별 정책이 있습니다.

### 리소스 유형별 VPC 리소스 권한 부여
{: #vpc-resource-authorization-by-resource-type}

이 표에는 VPC 리소스가 기본적으로 권한 부여되는 방식이 요약되어 있습니다.

| 리소스 유형 | 기본 권한 부여를 가져오는 위치 |
|-----------|------------|
| VPC | 작성 시 권한 부여 확인 |
| Load Balancer | 작성 시 권한 부여 확인 |
| VPN | 작성 시 권한 부여 확인 |
| 서브넷 | 상위 VPC |
| 공용 게이트웨이 | 상위 VPC |
| 인스턴스 | 작성 시 권한 부여 확인 |
| 보안 그룹 | 작성 시 권한 부여 확인 |
| 키 | 작성 시 권한 부여 확인 |
| 이미지 | 계정 |
| 유동 IP | 계정 |
| 네트워크 ACL | 계정|
| 볼륨 | 작성 시 권한 부여 확인 |

유동 IP 및 네트워크 ACL은 지정되지 않은 경우 계정 레벨의 범위를 사용하여 작성될 수 있습니다. 그러나 유동 IP가 인스턴스에 지정되거나 ACL이 서브넷에 지정되는 즉시 이들 리소스에는 VPC의 권한 부여 범위가 적용됩니다.
{: note}

### VPC 역할 적용 범위 및 리소스에 대해 권한 부여되는 조치
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

2개의 VPC가 있는 시나리오에서, 다음 내용은 특정 역할이 있는 사용자가 특정 조치를 수행하려 하는 경우 발생하는 상황에 대한 몇 가지 예입니다.

* 모든 리소스에 대한 **관리자** 권한이 있는 사용자 _admin_
  * `vpc1` 작성: 가능

* 모든 리소스에 대한 **편집자** 권한이 있는 사용자 _editor_
  * `vpc2` 작성: 가능
  * `vpc2` 업데이트: 가능
  * `vpc2` 삭제: 가능

* 모든 리소스에 대한 **운영자** 권한이 있는 사용자 _operator_
  * `vpc1` 작성: 거부됨
  * `vpc1` 업데이트: 거부됨
  * `vpc1` 삭제: 거부됨

* 모든 리소스에 대한 **뷰어** 권한이 있는 사용자 _viewer_
  * VPC 나열: [vpc1]이 표시됨
  * `vpc1` 읽기: 가능

* 정책이 없는 사용자 _noRole_
  * VPC 나열: []이 표시됨
  * `vpc1` 읽기: 거부됨

### VPC 적용 범위 요약 표
{: #vpc-coverage-summary-table}

역할(관리자, 편집자, 운영자, 뷰어)에 의해 액세스를 허용하는 정책의 경우 다음 조치가 제공됩니다(X = 거부됨, o = 가능).

 역할:      | 없음 |  뷰어 | 운영자 | 편집자 | 관리자
-----------:|------|--------|-------|--------|-------
작성      | X    | X      | X     | o      | o
나열        | X    | o      | X     | o      | o
읽기        | X    | o      | X     | o      | o
업데이트      | X    | X      | X     | o      | o
삭제      | X    | X      | X     | o      | o


## VPC용 VPN의 리소스 권한 부여
{: #resource-authorizations-of-vpn-for-vpc}

**VPC용 VPN**의 리소스 권한 부여는 다른 VPC 리소스 권한 부여 유형과 별도로 설정되지만 방식은 유사합니다.

VPC용 VPN의 정책이 VPN 리소스(특히 VPC 게이트웨이)에 대한 액세스를 제어합니다. VPN 연결 및 로컬 또는 피어 CIDR과 같은 VPN 게이트웨이의 하위 리소스에 액세스하려면 상위 VPN 게이트웨이에 대한 **읽기** 액세스 권한(또는 그 이상)이 있어야 합니다. 리소스 정책은 상위 VPN 게이트웨이에 대한 **읽기** 이상의 액세스 권한이 있는 VPN 연결에만 사용될 수 있습니다. VPN의 리소스 정책은 다음 항목을 대상으로 할 수 있습니다.

* 모든 VPN 리소스(VPN 게이트웨이, VPN 연결, IKE 및 IPsec 정책)
* 리소스 그룹의 모든 VPN 리소스
* 개별 VPN 게이트웨이

VPN 사용 또한 별도로 청구됩니다.
{: note}

### VPC용 VPN 역할 적용 범위 및 리소스에 대해 권한 부여되는 조치
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

VPC 리소스 권한 부여에서 지원되는 것과 동일한 사용자 역할이 지원되지만 각 역할에서 사용할 수 있는 조치는 다릅니다.

특정 역할의 사용자가 특정 VPN 관련 조치를 수행하려 시도하는 경우 발생하는 상황에 대한 몇 가지 예는 다음과 같습니다.

* 모든 리소스에 대한 **관리자** 권한이 있는 사용자 _admin_
  * VPN 게이트웨이 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * VPN 연결 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * VPN 피어 & 로컬 CIDR 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * IKE 정책 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * IPsec 정책 작성, 업데이트, 삭제, 읽기, 나열: 가능

* 모든 리소스에 대한 **편집자** 권한이 있는 사용자 _editor_
  * 관리자와 동일

* 모든 리소스에 대한 **운영자** 권한이 있는 사용자 _operator_
  * VPN 게이트웨이 작성, 업데이트, 삭제: 거부됨
  * VPN 연결 작성, 업데이트, 삭제: 거부됨
  * VPN 피어 & 로컬 CIDR 작성, 업데이트, 삭제: 거부됨
  * IKE 정책 연결 작성, 업데이트, 삭제: 거부됨
  * IPsec 정책 연결 작성, 업데이트, 삭제: 거부됨
  * VPN 게이트웨이 읽기, 나열: 가능 - 실제 데이터가 표시됨
  * VPN 연결 읽기, 나열: 가능 - 실제 데이터가 표시됨
  * VPN 피어 & 로컬 CIDR 읽기, 나열: 가능 - 실제 데이터가 표시됨
  * IKE 정책 읽기, 나열: 가능 - 실제 데이터가 표시됨
  * IPsec 정책 읽기, 나열: 가능 - 실제 데이터가 표시됨

* 모든 리소스에 대한 **뷰어** 권한이 있는 사용자 _viewer_
  * 운영자와 동일

* 정책이 없는 사용자 _noRole_
  * VPN 게이트웨이 읽기, 나열: 거부됨 - 목록에 빈 데이터가 표시됨
  * VPN 연결 읽기, 나열: 거부됨
  * VPN 피어 & 로컬 CIDR 읽기, 나열: 거부됨
  * IKE 정책 읽기, 나열: 거부됨 - 목록에 빈 데이터가 표시됨
  * IPSec 정책 읽기, 나열: 거부됨 - 목록에 빈 데이터가 표시됨

### VPC용 VPN 적용 범위 요약 표
{: #vpn-for-vpc-coverage-summary-table}

VPC용 VPN의 조치 역할 맵핑은 다음 표와 같이 시각화할 수 있습니다(X = 거부됨, o = 가능).

 역할:       | 없음 |  뷰어 | 운영자 | 편집자 | 관리자
-----------:|------|--------|-------|--------|-------
작성      | X    | X      | X     | o      | o
나열        | X    | o      | o     | o      | o
읽기        | X    | o      | o     | o      | o
업데이트      | X    | X      | X     | o      | o
삭제      | X    | X      | X     | o      | o

## VPC용 Load Balancer의 리소스 권한 부여
{: #resource-authorization-for-load-balancer-for-vpc}

**VPC용 Load Balancer** 사용에 대한 리소스 권한 부여는 VPC의 다른 리소스 권한 부여와 별도로 설정되지만 방식은 유사합니다.

Load Balancer 사용 또한 별도로 청구됩니다.
{: note}

### Load Balancer 역할 적용 범위 및 리소스에 대해 권한 부여되는 조치
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

특정 역할의 사용자가 VPC Load Balancer와 관련된 특정 조치를 수행하려 시도하는 경우 발생하는 상황에 대한 몇 가지 예는 다음과 같습니다.

* 모든 리소스에 대한 **관리자** 권한이 있는 사용자 _admin_
  * Load Balancers 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * 리스너 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * 풀 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * 멤버 작성, 업데이트, 삭제, 읽기, 나열: 가능
  * Load Balancer 통계 작성, 업데이트, 삭제, 읽기, 나열: 가능

* 모든 리소스에 대한 **편집자** 권한이 있는 사용자 _editor_
  * 관리자와 동일

* 모든 리소스에 대한 **운영자** 권한이 있는 사용자 _operator_
  * Load Balancers 작성, 업데이트, 삭제, 읽기, 나열: 거부됨
  * 리스너 작성, 업데이트, 삭제, 읽기, 나열: 거부됨
  * 풀 작성, 업데이트, 삭제, 읽기, 나열: 거부됨
  * 멤버 작성, 업데이트, 삭제, 읽기, 나열: 거부됨
  * Load Balancer 통계 작성, 업데이트, 삭제, 읽기, 나열: 거부됨

* 모든 리소스에 대한 **뷰어** 권한이 있는 사용자 _viewer_
  * Load Balancers 읽기, 나열: 가능
  * 리스너 읽기, 나열: 가능
  * 풀 읽기, 나열: 가능
  * 멤버 읽기, 나열: 가능
  * Load Balancer 통계 읽기: 가능

* 정책이 없는 사용자 _noRole_
  * Load Balancers 읽기, 나열: 거부됨 - 목록에 빈 데이터가 표시됨
  * 리스너 읽기, 나열: 거부됨
  * 풀 읽기, 나열: 거부됨
  * 멤버 읽기, 나열: 거부됨
  * Load Balancer 통계 읽기: 거부됨

### Load Balancer 적용 범위 요약 표
{: #load-balancer-coverage-summary-table}

VPC용 Load Balancer의 조치 역할 맵핑은 다음 표와 같이 시각화할 수 있습니다(X = 거부됨, o = 가능).

 역할:       | 없음 |  뷰어 | 운영자 | 편집자 | 관리자
-----------:|------|--------|-------|--------|-------
작성      | X    | X      | X     | o      | o
나열        | X    | o      | X     | o      | o
읽기        | X    | o      | X     | o      | o
업데이트      | X    | X      | X     | o      | o
삭제      | X    | X      | X     | o      | o


## VPC용 Virtual Server의 리소스 권한 부여
{: #planning-virtual-servers-for-vpc-permissions}

VPC의 다른 리소스 권한 부여와 별도로 **VPC용 Virtual Server**에 대한 리소스 권한 부여를 설정하십시오. 

VPC용 Virtual Server 사용은 별도로 청구됩니다.
{: note}

* 관리자는 역할을 정의하고 {{site.data.keyword.vsi_is_short}}에 대해 사용 가능한 조치를 수행할 수 있습니다. 
* 편집자는 상태를 수정하고 하위 리소스를 작성 또는 삭제할 수 있습니다.
* 운영자는 리소스의 상태를 변경하지 않는 조치를 수행할 수 있습니다. 
* 뷰어는 리소스의 상태를 변경하지 않는 조치를 수행할 수 있습니다. 

### Virtual Server 적용 범위 요약 표
{: #vsi-coverage-summary-table}

VPC용 Virtual Server의 조치 역할 맵핑은 다음 표와 같이 시각화할 수 있습니다(X = 거부됨, o = 가능). 

 역할:       | 없음 |  뷰어 | 운영자 | 편집자 | 관리자
-----------:|------|--------|-------|--------|-------
작성      | X    | X      | X     | o      | o
나열        | X    | o      | o     | o      | o
읽기        | X    | o      | o     | o      | o
업데이트      | X    | X      | X     | o      | o
삭제      | X    | X      | X     | o      | o

인스턴스를 작성할 때 VPC 및 보안 그룹 리소스가 지정된 경우 이러한 리소스에 대한 운영자 액세스 권한도 있어야 합니다. 서브넷 및 유동 IP 리소스는 연관된 VPC에서 권한을 상속합니다.  
{: tip} 
