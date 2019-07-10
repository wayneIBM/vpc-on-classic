---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"
keywords: features, benefits, isolation, provisioning, security, cloud-native, workloads, BYOIP, vpc

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Virtual Private Cloud 정보
{: #about}

{{site.data.keyword.cloud}} Virtual Private Cloud(VPC)는 차세대 IBM Cloud 플랫폼입니다. VPC를 사용하면 IBM Cloud에 고유한 공간을 작성하고 퍼블릭 클라우드 내에서 격리된 환경을 실행할 수 있습니다. VPC는 프라이빗 클라우드의 보안과 퍼블릭 클라우드의 민첩성 및 사용 용이성을 동시에 제공합니다.

중요 업무용, 클라우드 허용 및 클라우드 고유 애플리케이션을 지원하기 위해 필요에 따라 네트워크 서비스를 관리하고 애플리케이션을 실행할 수 있습니다. 사용 가능한 주요 기능은 다음과 같습니다.

* API를 통해 격리된 애플리케이션 환경을 작성하고 관리
* 보안 및 편리한 액세스를 위해 디자인된 고유 네트워크 정책 정의
* BYOIP(Bring Your Own IP)를 사용하여 네트워크 토폴로지 디자인
* 리소스를 프로비저닝하고 이를 서로 연결하거나 서로에게서 격리
* 재해 복구 및 복원성을 위해 여러 지역을 활용
* HA를 사용하여 지역 간에 고속의 대기 시간이 짧은 연결을 허용하는 가용성 구역 사용
* 고속 네트워킹 및 스토리지 디바이스 이용
* 항상 작동 서비스 사용 가능(제어 평면)
* 주요 서비스 제공 및 이용: IPAM, VPN, 방화벽, SSH, DNS, L4 로드 밸런싱
* 기타 서비스 설정: Auto-Scaling, CDN, 서드파티 NFV

다음에 나오는 VPC 개요 그림은 사용 가능한 VPC 컴포넌트와 이러한 컴포넌트가 함께 작동하여 필요할 수 있는 서비스 및 연결을 제공하는 방법을 설명합니다. 각 컴포넌트에 대한 세부사항을 제공하는 주제를 자세히 살펴볼 때 이 그림을 다시 참조하십시오.

![IBM Cloud VPC 개요](images/vpc-experience-simple.svg "IBM Cloud VPC 개요"){: caption="Figure: IBM Cloud VPC Overview" caption-side="top"}

다음 절에서는 IBM Cloud VPC에서 사용 가능한 기능에 대한 개요를 제공합니다.

## 네트워킹 기능

{{site.data.keyword.cloud_notm}} VPC는 [전 세계 범위에서 사용 가능한 다중 구역 지역](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)에서 다중 Virtual Private Cloud를 작성할 수 있는 기능, 여러 구역의 서브넷, [IP 주소 범위 선택](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets), 가상 방화벽([보안 그룹](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups) 및 [네트워크 ACL](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)), 사이트 대 사이트 가상 사설망([VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)) 및 탄력적 로드 밸런싱([LBaaS](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc))을 포함한 쉽고 포괄적인 네트워킹 기능을 제공합니다.

VPC는 사설 IP 주소 범위를 사용하여 서브넷으로 분할됩니다. 그러나 기본적으로 동일한 VPC 내의 모든 리소스(예: VSI)는 서브넷에 관계없이 서로 통신할 수 있습니다. 서브넷은 하나의 구역에 포함되고 여러 구역에 걸쳐 있을 수 없으며, 이는 보안, 대기 시간 감소 및 고가용성에 도움이 됩니다. 

제안된 접두부 범위 및 사전 구성된 네트워크 보안 정책을 사용하여 쉽게 다중 VPC 및 서브넷을 작성하거나 고유 주소 접두부 및 사용자 정의 보안 정책을 디자인하고 정의하십시오. 

CIDR 블록 161.26/16 및 166.8/14는 모두 예약되어 있으며 모든 서브넷으로 라우팅됩니다. [IBM Cloud VPC에서 사용할 수 있는 서비스 엔드포인트](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)에 대해 읽으십시오. 또한 CIDR 블록 10.240/13은 VPC 주소 접두부용으로 예약되어 있고 [사전 정의된 방식](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#ibm-cloud-vpc-and-address-prefixes)으로 구역 간에 분배되며 변경할 수 없습니다.
{: important}

### 인터넷 액세스

가상 서버 인스턴스(VSI)에서 공용 인터넷과 통신할 수 있도록 하는 데는 세 가지 옵션이 있습니다. 
* 공용 게이트웨이(PGW)를 사용하여 연결된 서브넷에 있는 모든 가상 서버 인스턴스의 인터넷 통신을 가능하게 합니다. 사용되는 대역폭을 제외하고 PGW를 사용하여 발생하는 비용은 없습니다.
* 유동 IP(FIP)를 사용하여 단일 가상 서버 인스턴스(VSI)와 인터넷 간의 통신을 가능하게 합니다. 
* VPN 게이트웨이를 사용합니다. 자세한 정보는 [VPC용 VPN 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc#--using-vpn-with-your-vpc)를 참조하십시오.
{: note}

### 클래식 액세스

기존 {{site.data.keyword.cloud_notm}} 인프라 사용자는 [클래식 액세스](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)를 사용하여 Direct Link 연결을 포함한 클래식 인프라를 지역당 하나의 VPC에 연결할 수 있습니다.

### BYOIP

IBM Cloud VPC 계정에서 자신이 소유한 공인 IPv4 주소 범위를 사용(BYOIP)할 수 있습니다.

BYOIP를 사용하는 경우 {{site.data.keyword.cloud_notm}}가 {{site.data.keyword.cloud_notm}} 리소스에서 해당 IPv4 주소를 구성해야 합니다. 그러면 사용자가 제공한 주소에 대해 패킷이 송수신됩니다. 따라서 {{site.data.keyword.cloud_notm}}에서 제공된 IPv4 범위를 사용하면 해당 IP 주소가 IBM 지원 센터의 담당자 및 서드파티에 공개됩니다.
{: important}

BYOIP를 사용하려면 API 또는 CLI를 사용해야 합니다. 이 기능은 IBM Cloud 콘솔 UI를 통해 사용할 수 없습니다.
{: note}

### 글로벌 연결

애플리케이션 및 사용 가능한 리소스를 여러 지역에서 사용할 수 있도록 범위 지정할 수 있습니다. [VPN](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc)을 사용하면 하이브리드 클라우드 배치의 다른 프로젝트 또는 다른 부분으로의 사설 연결을 작성할 수 있습니다.

쉽게 확장하고 공유할 수 있는 VPC 네트워크 및 리소스를 온프레미스 설비에서 클라우드로 확장할 수 있습니다.

## 보안

인스턴스 레벨 보호를 위한 가상 방화벽 역할을 하는 [보안 그룹](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups)과 서브넷 레벨 보호를 위한[네트워크 액세스 제어 목록](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls)(ACL)을 사용하여 보안이 {{site.data.keyword.cloud_notm}} VPC에 통합됩니다.

## 고가용성

{{site.data.keyword.cloud_notm}} Virtual Private Cloud(VPC)는 사용자의 애플리케이션에 확장성 및 보안을 제공하는 동시에 모든 지역의 다른 네트워크로부터 논리적으로 격리시킵니다. [Load Balancers](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)를 사용하여 특정 대상 집합에 네트워크 트래픽을 분배함으로써 성능 및 HA를 향상시키십시오. Load Balancers는 애플리케이션 및 서비스의 상태 또한 모니터합니다. 한 구역 내의 인스턴스 또는 지역 내 여러 구역의 인스턴스에 수신 애플리케이션 트래픽을 분배하도록 로드 밸런서를 구성할 수 있습니다.

## 컴퓨팅 기능

[Virtual Private Cloud용 IBM Virtual Server](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-virtual-private-cloud)를 사용하여 IBM Cloud에서 확장 가능한 컴퓨팅 리소스를 프로비저닝할 수 있습니다. 특정 워크로드에 최적화된 사전 정의된 [프로파일](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)을 사용하여 가상 서버 인스턴스(VSI)를 빠르게 작성하십시오. 다중 홈, [다중 vNIC](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-network-security-options) 인스턴스를 프로비저닝하는 경우 지원되는 저장 [이미지](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-images) 중에서 선택하거나 사용자 정의 이미지를 업로드하십시오.

{{site.data.keyword.cloud_notm}} VPC는 가상 서버 인스턴스의 팟(Pod) 경계를 제거하는 네트워크 오케스트레이션 계층을 추가하여 인스턴스 스케일링을 위한 무한 용량을 작성합니다. 네트워크 오케스트레이션 계층은 지역 및 구역 전체의 IBM Cloud VPC 내에 있는 모든 가상 서버 인스턴스에 대한 모든 네트워킹을 처리합니다. {{site.data.keyword.cloud_notm}} VPC에서 제공하는 소프트웨어 정의 네트워킹 기능을 사용하는 경우 VPN, LBaaS, 다중 vNIC 인스턴스 및 더 큰 서브넷 크기에 대한 추가 옵션이 있습니다.

## 스토리지 기능

Virtual Private Cloud용 {{site.data.keyword.cloud_notm}} Virtual Server 인스턴스를 프로비저닝할 때 100GB의 범용 IOPS(3IOPS/GB) 블록 스토리지 볼륨이 자동으로 기본 부트 볼륨으로 작성되어 인스턴스에 연결됩니다. VPC 네트워크에서 가상 서버 인스턴스를 프로비저닝할 때 블록 스토리지 볼륨을 작성하거나 VSI 라이프사이클과 독립적으로 새 볼륨을 작성할 수 있습니다.

추가 VPC용 블록 스토리지를 프로비저닝할 때 [IOPS 티어](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#tiers)를 블록 스토리지 볼륨으로 선택하거나 [사용자 정의 IOPS 프로파일](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#custom)을 지정할 수 있습니다.

## 클라우드 고유 및 하이브리드 워크로드를 대상으로 디자인됨

VPC는 클라우드 고유 워크로드 및 기존 인프라를 {{site.data.keyword.cloud_notm}}에 연결하는 데 있어서 이상적입니다. 코그너티브, AI 및 기계 학습 컴퓨팅과 같은 워크로드에 대한 최상의 클라우드를 작성할 수 있습니다. 

VPC가 하이브리드, 클라우드 허용 및 클라우드 고유 워크로드를 지원하는 추가 방법은 다음과 같습니다. 

* 수평적 Auto-Scaling을 사용한 로드 밸런싱
* 모니터링 및 상태 검사
* GPU에 대한 지원
* 선호도 및 비선호도 그룹을 사용한 VSI 프로비저닝

## 자세히 보기

* [**VPC 용어**](/docs/vpc-on-classic?topic=vpc-on-classic-vpc-glossary)
* [**VPC 보안**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-security-in-your-ibm-cloud-vpc#security-in-your-ibm-cloud-vpc)
* [**사용 가능한 VPC 지역 및 서브넷**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets)
* [**VPC에서의 네트워크 리소스 작성 및 관리**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-network-resources-in-vpc)
* [**VPC에서의 가상 서버 인스턴스 작성 및 관리**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-virtual-server-instances)
* [**VPC에서의 블록 스토리지 작성 및 관리**](/docs/vpc-on-classic?topic=vpc-on-classic-creating-and-managing-storage-in-vpc)
