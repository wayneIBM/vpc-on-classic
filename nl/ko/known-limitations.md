---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-12"

keywords: limitations, bugs, known, known issues, Beta, services, capabilities, use cases

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 알려진 제한사항
{: #known-limitations}

이 문서에는 지원되지 않는 기능 및 API에 대한 요약, 아직 지원되지 않는 기능 및 유스 케이스, 현재 릴리스의 알려진 문제 및 베타 서비스로만 제공되는 기능 목록이 포함되어 있습니다. 알려진 제한사항은 {{site.data.keyword.vpc_full}}(VPC)에 기능을 추가함에 따라 변경될 수 있으므로 가끔씩 이 문서를 다시 확인하는 것이 좋습니다. 

## 지원되지 않는 기능에 대한 요약
{: #summary-of-features-not-supported}

* VPC에 대한 Direct Link 액세스는 [**클래식 액세스**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)를 통해서만 지원됩니다. 
* VPC에는 개인 서비스 엔드포인트의 서브세트를 사용할 수 있지만 모든 엔드포인트가 사용 가능한 것은 아닙니다. 사용 가능한 서비스는 [IBM Cloud VPC에서 사용할 수 있는 서비스 엔드포인트](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)를 참조하십시오.
* 이미지 저장 및 복원은 지원되지 않습니다.
* {{site.data.keyword.cloud_notm}} VPC는 지역적이므로 "클래식 액세스"가 사용으로 설정되거나 VPN 연결을 사용하여 연결되지 않으면 한 지역의 VPC를 다른 지역의 VPC에 연결할 수 없습니다. [**클래식 액세스**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)를 참조하십시오.

## 아직 지원되지 않는 기능 및 유스 케이스
{: #features-and-use-cases-not-yet-supported}

이 절에는 {{site.data.keyword.cloud_notm}} 서비스에 따라 분류된 지원되지 않는 기능 및 유스 케이스의 세부사항이 나열되어 있습니다.

### Virtual Private Cloud
{: vpc-unsupported-features-and-use-cases}

* {{site.data.keyword.vpc_short}}는 멀티캐스트 또는 브로드캐스트 도메인을 지원하지 않습니다.
* {{site.data.keyword.vpc_short}}는 엔드-투-엔드 암호화를 제공하지 않습니다.
* VPC는 다른 VPC와 피어 관계가 될 수 없습니다.
* 클라우드 서비스 엔드포인트는 VPC에서 지원되지 않습니다. 사용 가능한 서비스는 [IBM Cloud VPC에서 사용할 수 있는 서비스 엔드포인트](/docs/vpc-on-classic?topic=vpc-on-classic-service-endpoints-available-for-ibm-cloud-vpc)를 참조하십시오.

### 컴퓨팅
{: VSI-unsupported-features-and-use-cases}

* 기존 {{site.data.keyword.cloud_notm}} 가상 서버 인스턴스(VSI) 또는 Bare Metal Server를 VPC로 마이그레이션할 수 없습니다.
* Bare Metal Server를 VPC에 프로비저닝할 수 없습니다.

### 네트워크
{: network-unsupported-features-and-use-cases}

* 사용자 정의 라우트는 VPC에 추가될 수 없습니다. VPC의 모든 서브넷은 기본적으로 서로 통신할 수 있습니다.
* 하나의 서브넷은 여러 구역에 있을 수 없습니다.
* 서브넷을 한 구역에서 다른 구역으로 이동할 수는 없습니다.
* 서브넷을 작성한 후에는 해당 크기를 조정할 수 없습니다.
* {{site.data.keyword.cloud_notm}} 클래식 리소스는 VPC의 동일한 서브넷에 있을 수 없습니다. [**클래식 액세스**](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)를 사용하여 클래스 리소스와 사용자 계정에 있는 하나의 VPC 간에 연결할 수 있습니다. 
* IPv6은 지원되지 않습니다.
* 사용자가 자신이 소유한 공인 IP를 유동 IP로 사용할 수는 없습니다. 유동 IP 풀은 IBM에서 제공합니다.
* 하나의 유동 IP는 하나의 구역에 바인드됩니다. 예를 들어, 구역 1에서 예약된 유동 IP는 동일한 VPC의 구역 2에 있는 VSI에 지정될 수 없습니다.
* 사설 IP 주소는 VSI 간에 이동할 수 없습니다.
* 네트워크 인터페이스는 VSI 간에 이동할 수 없습니다.
* VSI의 특정 IP 주소를 지정할 수는 없습니다. 서브넷의 IP 범위만 지정할 수 있습니다. VSI를 서브넷에 연결하고 나면 시스템이 해당 서브넷으로부터 사설 IP를 지정합니다.
* {{site.data.keyword.vpc_short}}는 LBaaS, ACL 및 보안 그룹과 같은 고유 NaaS(Network as a Service) 도구와 함께 제공됩니다. 자신이 소유한 네트워크 어플라이언스(예: Vyatta Gateway 또는 기타 로드 밸런서)를 사용하여 VPC에 있는 리소스를 제어할 수는 없습니다. 마찬가지로, {{site.data.keyword.cloud_notm}} 클래식 인프라에 있는 자기 소유의 로드 밸런서 또는 보안 그룹을 사용하여 {{site.data.keyword.vpc_short}}에 있는 리소스를 제어할 수는 없습니다. 
* 공용 게이트웨이는 인터넷에서 IBM Cloud VPC에 있는 VSI로 트래픽 전송을 시작하는 것을 허용하지 않습니다. 이를 위해서는 유동 IP를 사용해야 합니다.
* ACL은 Stateless이므로 리턴 트래픽은 ACL 규칙에서 명시적으로 허용되어야 합니다.

## 이 릴리스의 알려진 문제
{: #known-issues-in-this-release}

다음 조건이 모두 충족되면 {{site.data.keyword.block_storage_is_short}}가 볼륨 이름의 고유성을 정확히 검증하지 않을 수도 있습니다.

* 프로비저닝 요청이 동시에 발생함
* 볼륨 이름이 동일함
* 볼륨이 동일한 지역에 있음

## 사용 가능한 베타 서비스

어플라이언스에 안전하게 피어링하는 기능은 베타 서비스로 사용 가능합니다. 여러 유형의 어플라이언스와의 피어링에 대한 문서가 사용 가능합니다.

* [원격 Vyatta 피어](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-vyatta-peer)
* [원격 StrongSwan 피어](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-strongswan-peer)
* [원격 FortiGate 피어](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-fortigate-peer)
* [원격 Juniper vSRX 피어](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-juniper-vsrx-peer)
* [원격 Cisco ASAv 피어](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network-creating-a-secure-connection-with-a-remote-cisco-asav-peer)
