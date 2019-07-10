---

copyright:
  years: 2019

lastupdated: "2019-06-13"

keywords: activity tracker, vpc, events, logdna 

subcollection: vpc-on-classic

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Activity Tracker with LogDNA 이벤트
{: #at-events}

{{site.data.keyword.at_full}} 서비스를 사용하여 사용자 및 애플리케이션이 {{site.data.keyword.cloud}}의 {{site.data.keyword.cloud}} Virtual Private Cloud(VPC)와 상호작용하는 방법을 추적할 수 있습니다.
{:shortdesc}

{{site.data.keyword.at_full}} 서비스는 {{site.data.keyword.cloud}}에서 서비스의 상태를 변경하기 위해 사용자가 시작한 활동을 기록합니다. 자세한 정보는 [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)의 내용을 참조하십시오.

## 이벤트 목록: 네트워크 리소스
{: #events-volumes}

| 리소스  | 조치  |설명  |
|:----------------|:-----------------------|:-----------------------|
| vpc  | is.vpc.vpc.create   | VPC가 작성되었습니다. |
| vpc  | is.vpc.vpc.update   | VPC가 업데이트되었습니다. |
| vpc  | is.vpc.vpc.delete   | VPC가 삭제되었습니다. |
| vpc  | is.vpc.address-prefix.create  | 주소 접두부가 VPC에 추가되었습니다. |
| vpc  | is.vpc.address-prefix.update  | VPC 주소 접두부가 업데이트되었습니다. |
| vpc  | is.vpc.address-prefix.delete  | 주소 접두부가 VPC에서 제거되었습니다. |
| vpc  | is.vpc.vpc-route.create   | 라우트가 VPC에 추가되었습니다. |
| vpc  | is.vpc.vpc-route.update   | VPC 라우트가 업데이트되었습니다. |
| vpc  | is.vpc.vpc-route.delete   | 라우트가 VPC에서 제거되었습니다. |
| floating-ip  | is.floating-ip.floating-ip.delete   | 유동 IP가 작성되었습니다. |
| floating-ip  | is.floating-ip.floating-ip.delete   | 유동 IP가 업데이트되었습니다. |
| floating-ip  | is.floating-ip.floating-ip.delete   | 유동 IP가 삭제되었습니다. |
| network-acl  | is.network-acl.network-acl.create   | 네트워크 ACL이 작성되었습니다. |
| network-acl  | is.network-acl.network-acl.update   | 네트워크 ACL이 업데이트되었습니다. |
| network-acl  | is.network-acl.network-acl.delete   | 네트워크 ACL이 삭제되었습니다. |
| network-acl  | is.network-acl.rule.create  | 규칙이 네트워크 ACL에 추가되었습니다. |
| network-acl  | is.network-acl.rule.update  | 네트워크 ACL 규칙이 업데이트되었습니다. |
| network-acl  | is.network-acl.rule.delete  | 규칙이 네트워크 ACL에서 제거되었습니다. |
| public-gateway | is.public-gateway.public-gateway.create   | 공용 게이트웨이가 작성되었습니다. |
| public-gateway | is.public-gateway.public-gateway.update   | 공용 게이트웨이가 업데이트되었습니다. |
| public-gateway | is.public-gateway.public-gateway.delete   | 공용 게이트웨이가 삭제되었습니다. |
| security-group | is.security-group.security-group.create   | 보안 그룹 작성 |
| security-group | is.security-group.security-group.delete   | 보안 그룹 삭제 |
| security-group | is.security-group.security-group.update   | 보안 그룹 업데이트  |
| security-group | is.security-group.security-group.rule-create  | 보안 그룹 규칙 작성 |
| security-group | is.security-group.security-group.rule-delete  | 보안 그룹 규칙 삭제 |
| security-group | is.security-group.security-group.rule-update  | 보안 그룹 규칙 업데이트 |
| security-group | is.security-group.security-group.interface-attach | 보안 그룹 인터페이스 연결 |
| security-group | is.security-group.security-group.interface-detach | 보안 그룹 인터페이스 분리 |
| 서브넷   | is.subnet.subnet.create   | 서브넷이 작성되었습니다. |
| 서브넷   | is.subnet.subnet.update   | 서브넷이 업데이트되었습니다. |
| 서브넷   | is.subnet.subnet.delete   | 서브넷이 삭제되었습니다. |
| 서브넷   | is.subnet.network-acl.update  | 서브넷의 네트워크 ACL이 대체되었습니다. |
| 서브넷   | is.subnet.public-gateway.operate  | 공용 게이트웨이가 서브넷에 연결되었습니다. |
| 서브넷   | is.subnet.public-gateway.operate  | 공용 게이트웨이가 서브넷에서 분리되었습니다. |

## 이벤트 목록: 컴퓨팅 리소스
{: #events-computes}

다음 표에는 컴퓨팅 리소스 및 이벤트 생성과 관련된 조치가 나열되어 있습니다.

| 리소스  | 조치  |설명  |
|:----------------|:-----------------------|:-----------------------|
| 인스턴스 | is.instance.instance.create   | 인스턴스가 작성되었습니다. |
| 인스턴스 | is.instance.instance.delete   | 인스턴스가 삭제되었습니다. |
| 인스턴스 | is.instance.instance.update   | 인스턴스가 업데이트되었습니다. |
| 인스턴스 | is.instance.action.create   | 인스턴스 조치가 작성되었습니다. |
| 인스턴스 | is.instance.action.delete   | 보류 중인 인스턴스 조치가 삭제되었습니다. |
| 인스턴스 | is.instance.network_interface.floating_ip.create  | 유동 IP가 인스턴스 네트워크 인터페이스와 연관되었습니다. |
| 인스턴스 | is.instance.network_interface.floating_ip.delete  | 유동 IP가 인스턴스 네트워크 인터페이스에서 연관 해제되었습니다. |
| 인스턴스 | is.instance.volume_attachement.create   | 인스턴스 볼륨 연결이 작성되었습니다. |
| 인스턴스 | is.instance.volume_attachement.delete   | 인스턴스 볼륨 연결이 삭제되었습니다. |
| 인스턴스 | is.instance.volume_attachement.update   | 인스턴스 볼륨 연결이 업데이트되었습니다. |
| 키 | is.key.key.create   | 키가 작성되었습니다. |
| 키 | is.key.key.delete   | 키가 삭제되었습니다. |
| 키 | is.key.key.update   | 키가 업데이트되었습니다. |

## 이벤트 목록: 스토리지 리소스
{: #events-storage}

다음 표에는 스토리지 리소스 및 이벤트 생성과 관련된 조치가 나열되어 있습니다.

| 리소스  | 조치 |설명  |
|:----------------|:-----------------------|:-----------------------|
| 볼륨  | is.volume.volume.create  | 볼륨이 작성되었습니다. |
| 볼륨  | is.volume.volume.update  | 볼륨이 업데이트되었습니다. |
| 볼륨  | is.volume.volume.delete  | 볼륨이 삭제되었습니다. |


## 이벤트 목록: 로드 밸런서
{: #events-load-balancers}

다음 표에는 로드 밸런서 및 이벤트 생성과 관련된 조치가 나열되어 있습니다.

| 리소스  | 조치  |설명  |
|:----------------|:-----------------------|:-----------------------|
| 로드 밸런서 | is.load-balancer.load-balancer.create | 로드 밸런서가 작성됨 |
| 로드 밸런서 | is.load-balancer.load-balancer.update | 로드 밸런서가 업데이트됨 |
| 로드 밸런서 | is.load-balancer.load-balancer.delete | 로드 밸런서가 삭제됨 |
| 리스너 | is.load-balancer.load-balancer.listener.create | 리스너가 작성됨 |
| 리스너 | is.load-balancer.load-balancer.listener.update | 리스너가 업데이트됨 |
| 리스너 | is.load-balancer.load-balancer.listener.delete | 리스너가 삭제됨 |
| 풀 | is.load-balancer.load-balancer.pool.create | 풀이 작성됨 |
| 풀 | is.load-balancer.load-balancer.pool.update | 풀이 업데이트됨 |
| 풀 | is.load-balancer.load-balancer.pool.delete | 풀이 삭제됨 |
| 멤버 | is.load-balancer.load-balancer.pool.member.create | 멤버가 작성됨 |
| 멤버 | is.load-balancer.load-balancer.pool.member.update | 멤버가 업데이트됨 |
| 멤버 | is.load-balancer.load-balancer.pool.member.delete | 멤버가 삭제됨 |
| 정책 | is.load-balancer.load-balancer.listener.policy.create | 정책이 작성됨 |
| 정책 | is.load-balancer.load-balancer.listener.policy.update | 정책이 업데이트됨 |
| 정책 | is.load-balancer.load-balancer.listener.policy.delete | 정책이 삭제됨 |
| 규칙 | is.load-balancer.load-balancer.listener.policy.rule.create | 규칙이 작성됨 |
| 규칙 | is.load-balancer.load-balancer.listener.policy.rule.update | 규칙이 업데이트됨 |
| 규칙 | is.load-balancer.load-balancer.listener.policy.rule.delete | 규칙이 삭제됨 |

로드 밸런서 감사 이벤트는 `us-south` 지역의 {{site.data.keyword.at_full}}에 기록됩니다. 로드 밸런서 서비스를 프로비저닝하는 지역은 중요하지 않습니다.
{:note}

## 이벤트 목록: VPN
{: #events-vpn}

다음 표에는 VPN 및 이벤트 생성과 관련된 조치가 나열되어 있습니다.

| 리소스  | 조치 |설명  |
|:----------------|:-----------------------|:-----------------------|
| vpn  | is.vpn.vpn.create   | VPN이 작성됨 |
| vpn  | is.vpn.vpn.delete   | VPN이 삭제됨 |
| vpn  | is.vpn.vpn.update   | VPN이 업데이트됨 |
| vpn  | is.vpn.vpn.update   | VPN 연결이 작성됨 |
| vpn  | is.vpn.vpn.update   | VPN 연결이 삭제됨 |
| vpn  | is.vpn.vpn.update   | VPN 연결이 업데이트됨 |


## 지원되는 위치
{: #at-supported-locations}

{{site.data.keyword.at_full}} 지원은 현재 댈러스 및 프랑크푸르트 위치에 대해 사용 가능합니다. 다음 표에는 각 VPC 지역에 대한 Activity Tracker with LogDNA 위치가 나열되어 있습니다.

| VPC 지역 | Activity Tracker with LogDNA 위치 |
|--------------|---------------------------------------|
| us-south   | 댈러스  |
| jp-tok   | 도쿄  |
| eu-de  | 프랑크푸르트   |

## 이벤트를 찾는 위치
{: #at-events-ui}

{{site.data.keyword.at_full}} 인스턴스를 프로비저닝할 때 이벤트가 자동으로 [지원되는 위치](/docs/vpc-on-classic?topic=vpc-on-classic-at-events#at-supported-locations`)의 Activity Tracker with a LogDNA 서비스 인스턴스로 전달됩니다.
