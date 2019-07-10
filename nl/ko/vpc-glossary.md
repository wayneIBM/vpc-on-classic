---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# VPC 용어집
{: #vpc-glossary}

다음 용어는 {{site.data.keyword.cloud}} Virtual Private Cloud 한정으로 일반적으로 사용됩니다. 보다 넓은 범위의 더 많은 용어는 [IBM Cloud 용어집](/docs/overview/glossary?topic=overview-glossary)을 참조하십시오.

## 공용 게이트웨이(public gateway)
{: #public-gateway}

공용 게이트웨이(PGW)는 서브넷(그리고 서브넷에 연결된 모든 VSI)이 인터넷에 연결할 수 있도록 하는 아웃바운드 전용 액세스를 가능하게 합니다. 서브넷은 기본적으로 사설이라는 점을 참고하십시오. 그러나 사용자는 선택적으로 PGW를 작성하여 서브넷을 PGW에 연결할 수 있습니다. 서브넷이 PGW에 연결되고 나면 해당 서브넷의 모든 VSI가 인터넷에 연결할 수 있습니다.

PGW는 대대일 NAT을 사용하며, 이는 사설 주소를 가진 수천 개의 VSI가 공용 인터넷과 통신하는 데 하나의 공인 IP 주소를 사용함을 의미합니다. PGW는 인터넷 측에서 이러한 인스턴스에 대한 연결을 시작할 수 없도록 합니다.

## 공유(share)
{: #shares}

VPC 내에서 파일 스토리지를 제공하는 방법입니다.

## 구역(zone)
{: #zone}

독립적인 결함 구역입니다. 구역은 결함 허용을 개선하고 대기 시간을 줄이기 위해 디자인된 개념입니다. 구역은 다음 특성을 보장합니다.

 * 각 구역은 독립적인 결함 구역이므로, 한 지역의 두 구역에 동시에 장애가 발생할 가능성은 극히 낮습니다.
 * 한 지역에 속한 구역 간의 트래픽에서 발생하는 대기 시간은 2ms 미만입니다.

## 내재적 라우터(implicit router)
{: #implicit-router}

VPC 내에 작성된 모든 서브넷 간의 내재적 네트워크 연결입니다.

## 리소스(Resource)
{: #resource}

VSI, 유동 IP, VPC 자체와 같이 VPC 배치의 일부이며 청구 비용을 발생시킬 수 있는 자산 유형입니다. 리소스는 특정 리소스가 항상 VPC와 조합으로 사용되는 경우 계산을 쉽게 하기 위해 _리소스 그룹_으로 결합될 수 있습니다.

## 보안 그룹(security group)
{: #security-group}

보안 그룹은 하나 이상의 서버(VSI)에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 수행합니다. 보안 그룹은 연관된 VSI에 대한 트래픽의 허용 여부를 지정하는 규칙의 집합입니다. 고객은 VSI를 시작할 때 해당 VSI와 하나 이상의 보안 그룹을 연관시킬 수 있습니다. 올바른 권한이 부여되면 고객이 보안 그룹 규칙을 수정할 수도 있습니다.

## 볼륨(volume)
{: #volumes}

VPC 내 가상 서버 인스턴스를 위한, 하이퍼바이저 마운트된 고성능 데이터 스토리지입니다.

## 서브넷(subnet)
{: #subnet}

서브넷은 여러 구역 또는 지역에 걸쳐 존재할 수 없는, 하나의 구역에 바인드된 IP 주소 범위입니다. 하나의 서브넷은 IBM Cloud VPC에서 한 구역 전체를 포함할 수 있습니다.

IBM Cloud VPC의 목적을 위한 서브넷의 중요한 특성은 일반적인 방식으로 서로 연결할 수 있는 동시에 서로 격리시킬 수 있다는 점입니다. 서브넷 격리는 서브넷 간의 데이터 패킷 플로우를 제어하는 방화벽 역할을 수행하는 네트워크 액세스 제어 목록(ACL)을 통해 수행할 수 있습니다. 마찬가지로, 보안 그룹 또한 개별 가상 서버 인스턴스(VSI)의 데이터 패킷 송수신 플로우를 제어하는 가상 방화벽 역할을 수행합니다.

사용자는 서브넷 격리를 통해 퍼블릭 클라우드 내에 개인용 영역을 작성합니다.

## 액세스 제어 목록(access control list)
{: #access-control-list}

액세스 제어 목록(ACL)은 [서브넷](#subnet)의 인바운드 및 아웃바운드 트래픽에 대한 보안 제어를 제공합니다. ACL은 Stateless입니다. 각 ACL은 _소스 IP_, _소스 포트_, _대상 IP_, _대상 포트_ 및 _프로토콜_을 기반으로 하는 규칙으로 구성됩니다.

IBM Cloud VPC에서 모든 서브넷은 인바운드 및 아웃바운드 트래픽을 허용하는 기본 ACL을 포함하여 작성되지만, 고객이 사용자 정의 ACL을 작성할 수도 있습니다. 하나의 서브넷에는 한 번에 하나의 ACL만 연결되지만, 하나의 ACL은 여러 서브넷에 연결될 수 있습니다.

_네트워크 ACL_ 또는 NACL이라고 하기도 합니다.

## 유동 IP(floating IP)
{: #floating-ip}

유동 IP는 특정 풀의 지정된 _유동 IP 주소_를 사용하여 인스턴스, 로드 밸런서 또는 VPN 터널과 같은 VPC 리소스에 인터넷에 대한 인바운드 및 아웃바운드 액세스를 제공하는 방법입니다.

유동 IP 주소는 시스템이 기존 풀로부터 제공하는 공인 IP 주소입니다. 사용자가 자신이 소유한 공인 IP 주소를 사용할 수는 없습니다. 유동 IP 주소는 인터넷에서 접근할 수 있으며, vNIC를 통해 인스턴스(VSI, 로드 밸런서 또는 VPN 게이트웨이 등)와 연관될 수 있습니다.

사용자는 IBM에서 제공하는 사용 가능한 유동 IP 주소의 풀에서 유동 IP 주소를 예약할 수 있으며, 이를 동일한 Virtual Private Cloud에 있는 임의의 인스턴스와 연관시키거나 연관을 해제할 수 있습니다. 유동 IP는 서버가 공용 인터넷뿐만 아니라 클라우드 환경 내의 사설 서브넷과 통신할 수 있도록 하기 위해 일대일 [NAT](#nat)를 사용합니다.

## 이미지(image)
{: #image}

가상 서버 인스턴스, 또는 _인스턴스_를 시작하는 데 필요한 정보입니다. 일반적으로 이는 부팅에 사용되는 상용 운영 체제의 스냅샷 이미지입니다.

## 인스턴스(instance)
{: #instance}

VPC 내에서 실행되는 Virtual Server 또는 VSI의 간단한 이름입니다.

## 지역(geography)
{: #geography}

VPC에서 지역 지정은 VPC와 여기에 연관된 리소스가 배치되는 지역 및 구역에 대한 정보를 제공합니다.

## 지역(region)
{: #region}

VPC가 배치되는 지리적 지역입니다. 각 지역은 독립적인 결함 구역을 나타내는 여러 구역을 포함합니다. IBM Cloud VPC는 지정된 해당 지역 내의 여러 구역에 걸쳐 존재합니다.

## 프로파일(profile)
{: #profile}

프로파일은 가상 서버 인스턴스(VSI)를 시작하기 위해 빠르게 인스턴스화할 수 있는, 널리 사용되는 vCPU 및 RAM 조합입니다. Balanced, Compute 및 Memory의 세 가지 프로파일 제품군이 지원됩니다.

## LBaaS
{: #lbaas}

LBaaS(Load Balancer as a Service)는 VPC에 있는 인스턴스 간에 균형있게 할당되도록 트래픽을 분배하는 기능을 제공합니다.

## NAT
{: #nat}

네트워크 주소 변환(NAT)은 [인터넷 RFC 1631 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://tools.ietf.org/html/rfc1631){: new_window}에 설명되어 있는 주소 지정 방법으로, 하나의 IP 주소를 사용하여 여러 다른 IP 주소(사설 서브넷에 있는 주소 등)와 통신할 수 있도록 합니다(특히 룩업 테이블을 통해). NAT에는 두 가지 유형(일대일 NAT 및 [다대일 NAT ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window})이 있습니다. NAT는 원래 IPv4 IP 주소의 유효 수명을 늘리기 위한 방법이었지만 지금은 사설 서브넷과의 통신을 설정하는 방법으로 클라우드 환경에서 일반적으로 사용됩니다.

## SSH 키(SSH key)
{: #ssh-key}

VPC 내 가상 서버 인스턴스에 대한 액세스를 제어하는, 자동으로 생성되는 암호화된 액세스 인증 정보입니다.

## VPC
{: #vpc}

계정에 연결된 가상 네트워크입니다. 이는 가상 인프라 및 네트워크 트래픽 세분화를 세밀하게 제어할 수 있게 해 주며, 보안 및 동적 스케일링 기능도 제공합니다.

## VPN
{: #vpn}

가상 사설망(VPN)은 데이터가 공용 네트워크를 통해 전송되는 경우에도 두 엔드포인트 간의 사설 연결을 가능하게 합니다. 고객은 사설 네트워크에 연결된 것처럼 데이터를 공유할 수 있습니다. 보통 VPN은 최대한의 데이터 보안 및 개인정보 보호를 제공하기 위해 인증 및 암호화와 같은 보안 방법과 조합되어 사용됩니다.

## VPN 연결(VPN connection)
{: #vpn-connection}

데이터가 공용 네트워크를 통해 전송되는 경우에도 사설 연결로 유지되며 암호화될 수 있는, 두 엔드포인트 간의 사설 연결입니다.

## VPNaaS
{: #vpnaas}

VPNaaS(VPN as a Service)는 VPC 내부 및 외부의 엔드포인트로 VPN을 설정하는 기능을 제공합니다.


