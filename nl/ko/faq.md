---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: FAQ, faqs, limit, resource, vNIC, VSI, PGW, console, VRF, bandwidth, COS, egress, load balancer

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
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faqs}

## 내 VPC를 내 다른 IBM Cloud 워크로드에 연결할 수 있습니까?  
{: #faq-0}
{:faq}

예, 각 지역에 있는 하나의 VPC에서 {{site.data.keyword.cloud}} 클래식 인프라로의 액세스를 설정할 수 있습니다. 자세한 정보는 [클래식 인프라에 대한 액세스 설정](/docs/vpc-on-classic?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc)을 참조하십시오.

## VPC 이름에 문자 수 한계가 있습니까?
{: #faq-1}
{:faq}

현재 한계는 100자입니다. 이 한계를 초과하면 "내부 오류" 메시지를 수신할 수 있습니다.

## VPC 리소스 이름은 숫자로 시작할 수 있습니까?
{: #faq-2}
{:faq}

아니오, 이름은 숫자를 포함할 수 있지만 문자로 시작해야 합니다.

## 이름에 사용할 수 있는 문자에 대한 제한사항이 있습니까?
{: #faq-3}
{:faq}

예, UI는 VSI 이름에서 연속된 이중 대시, 밑줄 및 마침표를 금지합니다.

## VSI를 서브넷 없이 유동 IP만 사용하여 작성할 수 있습니까?
{: #faq-9}
{:faq}

아니오.

## VSI를 둘 이상의 VPC에 연결할 수 있습니까?
{: #faq-10}
{:faq}

아니오.

## VPC에서 서브넷을 작성한 후 해당 크기를 변경할 수 있습니까?
{: #faq-11}
{:faq}

아니오.

## VPC에서 COS로의 트래픽에 대한 송신 대역폭 비용이 있습니까?
{: #faq-17}
{:faq}

VPC에서 COS로 연결하는 데 ADN 엔드포인트가 사용되는 한 송신 대역폭 비용은 발생하지 않지만, API 호출 비용은 여전히 적용됩니다.

 ## VPC용 VPN을 설정할 때 서브넷을 지정해야 하는 이유와 지정해야 하는 항목은 무엇입니까?
{: #faq-16}
{:faq}

VPN 게이트웨이를 설정할 때는 해당 VPN 게이트웨이가 VPN 서비스를 수행하는 데 필요한 IP 주소를 얻을 수 있도록 서브넷을 지정해야 합니다. 서브넷을 지정함으로써 VPN 트래픽이 처리되는 방식을 제어할 수 있으며, VPN을 이용하는 리소스에 가까운 위치를 지정하는 유연성을 발휘할 수 있습니다. 이렇게 하면 대기 시간을 줄이고 성능을 향상시킬 수 있습니다.

## 내 로드 밸런서 인스턴스가 배치되는 위치는 어떻게 알 수 있습니까?
{: #faq-18}
{:faq}

선택된 서브넷이 VPC 로드 밸런서가 배치되는 위치를 결정합니다. VPC 로드 밸런서는 지역 리소스입니다. 우수 사례는 중복성을 증가시키기 위해 해당 지역의 다른 구역에서 서브넷을 선택하는 것입니다. 


## VPC에서의 유동 IP 지정 중에 고객이 VSI의 vNIC를 지정해야 합니까?
{: #faq-4}
{:faq}

예, 그리고 이는 반드시 서버의 기본 네트워크 인터페이스여야 합니다.

그러나 네트워킹 영역의 API에 "주소" 리소스를 추가하면 유동 IP의 연관이 인터페이스가 아니라 주소에 대한 것으로 변경될 수 있습니다.

## VPC에 있는 가상 서버 인스턴스의 vNIC가 유동 IP와 사설 IP를 둘 다 가질 수 있습니까?
{: #faq-5}
{:faq}
 
예.

## VPC용 IBM Cloud 콘솔 UI에는 각 서브넷을 공용 게이트웨이(PGW)에 연결하거나 여기에서 분리하는 "토글" 옵션이 있습니다. PGW는 누가 작성합니까? 고객이 UI에서 서브넷을 PGW에 연결하려고 할 때 PGW가 준비됩니까?
{: #faq-6}
{:faq}

API에는 서브넷과 특정 공용 게이트웨이 간의 명시적 연관이 필요하므로 PGW가 API를 통해 명시적으로 작성되어야 합니다. 

## 예를 들어 VPC의 VSI1에 vNIC1만 있으며 이것이 Subnet1에 연결되어 있습니다. Subnet1은 공용 게이트웨이(PGW)에 연결되어 있습니다. 이 경우 고객이 여전히 유동 IP를 VSI1에 지정할 수 있습니까?
{: #faq-7}
{:faq}

예, 서버는 공용 게이트웨이에 연결된 서브넷에 있는 동시에 유동 IP를 가질 수 있습니다. 유동 IP를 VSI에 지정하는 것은 서브넷에 연결된 공용 게이트웨이가 있는지 여부와 관련되어 있지 않습니다. VSI와 연관된 유동 IP가 서브넷에 연결된 공용 게이트웨이에 우선합니다.

## 어떤 VPC에서 VSI1에 vNIC1 및 vNIC2라는 2개의 vNIC가 있습니다. 이 두 vNIC를 동일한 서브넷에 연결할 수 있습니까?
{: #faq-8}
{:faq}
 
예, 동일한 서버에 있는 여러 네트워크 인터페이스를 동일한 서브넷에 연결할 수 없도록 하는 제한사항은 없습니다. 그러나 하나의 VSI에 여러 NIC를 두는 것은 지원되지 않습니다. 

## VPC의 구역당 하나의 공용 게이트웨이만 있을 수 있다는 제한사항은 무엇에 의해 적용됩니까?
{: #faq-13}
{: faq}

VPC API가 이 한계를 적용합니다.

## PGW를 작성하는 중에 직접 FIP를 예약해야 합니까, 아니면 시스템이 FIP를 자동으로 예약합니까? 모든 유동 IP를 조회하는 중에 해당 유동 IP를 확인할 수 있습니까?
{: #faq-12}
{: #faq}

기존 유동 IP가 지정되지 않은 경우 API가 공용 게이트웨이와 함께 유동 IP를 자동으로 작성합니다. 그리고 해당 유동 IP는 목록에 표시됩니다.


## VRF는 내 다른 네트워킹 기능 및 서브넷에 어떤 영향을 줍니까?
{: #faq-14}
{:faq}

VLAN 및 VRF의 제한사항은 다음과 같습니다.

* 계정 간 VLAN Spanning은 VRF 환경에서 허용되지 않습니다.
* IPsec VPN 서비스는 제한됩니다.

VRF는 SSL 또는 PPTP VPN 액세스를 차단하지 않지만 해당 작동은 변경됩니다. VRF를 사용하지 않는 경우에는 하나의 VPN 연결만 있으면 계정에 있는 모든 서버를 볼 수 있습니다. VRF를 사용하는 경우에는 사용자의 VPN과 연관된 마켓의 리소스에만 액세스할 수 있습니다. 따라서 DAL VPN에 연결하는 경우에는 DAL 서버에만 연결할 수 있습니다.
{: note}

## VPC에서 `instance-network-interface-floating-ip-add` 하위 명령을 사용하는 올바른 방법은 무엇입니까? 유동 IP 주소를 미리 작성/할당해야 합니까?
{: #faq-15}
{:faq}

 유동 IP를 먼저 할당한 후 인터페이스 유동 IP `add` 명령에 FIP 주소를 제공해야 합니다.

 예: `ibmcloud is floating-ip-reserve FLOATING_IP_NAME (--zone ZONE | --nic NIC_ID)`. 구역을 제공하십시오.
