---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-12"

keywords: endpoint, service, DNS, resolver, mirror, object, storage, bandwidth, charges

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

# IBM Cloud VPC에서 사용할 수 있는 서비스 엔드포인트
{: #service-endpoints-available-for-ibm-cloud-vpc}

워크로드를 실행할 준비가 되면 두 가지 유형의 엔드포인트, laaS(Infrastructure as a Service) 엔드포인트 및 클라우드 서비스 엔드포인트(CSE)에 접근할 수 있습니다. IaaS 엔드포인트는 사전 프로비저닝되고 준비가 되어 있지만 CSE는 사용하기 전에 별도로 프로비저닝해야 합니다.

이러한 서비스에 대한 엔드포인트는 _라우팅 가능한 주소_, 즉 RFC 1918에 지정된 주소 외부에 있는 주소를 사용하며 이러한 주소는 공용 인터넷을 통해 통신하는 것처럼 보일 수 있지만 이러한 엔드포인트에서 들어오고 나가는 트래픽은 클라우드를 나가지 않습니다. 따라서 이 트래픽은 클라우드에서 나가고 공용 인터넷으로 이동하는 트래픽과 연관된 대역폭 비용을 방지합니다.

## IaaS(Infrastructure as a Service) 엔드포인트
{: #infrastructure-as-a-service-iaas-endpoints}

인프라 서비스는 `adn.networklayer.com` 도메인의 특정 DNS 이름을 통해 사용할 수 있으며 `161.26.0.0/16` 주소로 분석됩니다.

접근할 수 있는 서비스는 다음 항목을 포함합니다.

* DNS 분석기
* Ubuntu APT(고급 패키징 도구) 미러
* Cloud Object Storage
* NTP

## DNS(Domain Name System) 분석기 엔드포인트
{: #dns-domain-name-system-resolver-endpoints}

DNS 분석기는 이름이 아닌 IP 주소를 사용합니다. 공유 클라우드 서비스 엔드포인트의 경우 DNS 서버 주소 `161.26.0.10` 및 `161.26.0.11`이 사용되어야 합니다.

## Ubuntu APT 미러
{: #ubuntu-apt-mirrors}

Ubuntu 이미지를 업데이트하기 위한 APT 미러는 `mirrors.adn.networklayer.com`에서 사용할 수 있으며, 이는 `161.26.0.6`으로 분석되어야 합니다. 

## IBM Cloud Object Storage
{: #ibm-cloud-object-storage}

예를 들어, IBM Cloud 사설 네트워크에 있는 Object Storage 서비스에 대한 사설 엔드포인트에 접근하려면 URL의 "도메인" 부분을 `cloud-object-storage.appdomain.cloud`로 대체하십시오. 다른 지역에서 작성된 COS 버킷에 연결하려면 VSI가 작성된 지역의 엔드포인트가 아니라 버킷에 대한 엔드포인트를 사용하십시오.

## NTP(Network Time Protocol) 서버
{: #network-time-protocol-ntp-servers}

NTP 서버는 `time.adn.networklayer.com`에서 사용할 수 있으며, 이는 `161.26.0.6`으로 분석되어야 합니다.

## 클라우드 서비스 엔드포인트
{: #cloud-service-endpoints}

클라우드 서비스 엔드포인트는 다른 클라우드 사용자가 제공하는 서비스입니다. `cloud.ibm.com` 도메인의 DNS 이름을 통해 곧 사용할 수 있게 됩니다. 이는 `166.8.0.0/14` 주소로 분석됩니다.
