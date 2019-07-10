---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-07"
keywords: quota, resource, classic, access, gateway, address, prefix, VSI, vNIC, floating, SSH, key, security, group, rule, remote, peer, ACL, region, ingress, egress, VPN, policies, load balancer, listener, pool, per

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

# 할당량
{: #quotas}

이 문서는 {{site.data.keyword.cloud}} Virtual Private Cloud와 여기서 사용 가능한 리소스의 할당량 및 한계에 대해 설명합니다.

## VPC 할당량
{: #vpc-quotas}

계정에는 다음과 같은 할당량이 있습니다.

| 리소스     | 최대 수 |
| ------- | :------: |
| Virtual Private Cloud | 지역당 5개 |
| 클래식 액세스를 사용하는 VPC | 지역당 1개 |
| 공용 게이트웨이(PGW) | 구역당 VPC당 1개 |
| 주소 접두부 | 구역당 VPC당 5개 |

## 서브넷 할당량
{: #subnet-quotas}

| 리소스     | 최대 수 |
| ------- | :------: |
| 서브넷 | Virtual Private Cloud당 15개 |


## VSI 할당량
{: #vsi-quotas}

| 리소스     | 최대 수 |
| ------- | :------: |
| 가상 서버 인스턴스(VSI) | 기본적으로 계정당 100개 |
| VSI당 vNIC | VSI당 5개 |
| 유동 IP 주소 | VSI당 1개 |
| SSH 키 | 계정당 100개 |


## 보안 그룹 할당량
{: #security-groups-quotas}

보안 그룹 및 여기에 연관된 규칙에 대해서는 몇 가지 계정 기반 할당량(한계)이 있습니다.

| 리소스| 할당량|
|--------|-----|
| 보안 그룹| 네트워크 인터페이스당 5개|
| 보안 그룹| 계정당 500개|
| 규칙| 보안 그룹당 50개|
| 원격 규칙 | 보안 그룹당 5개|
| 네트워크 인터페이스| 보안 그룹당 100개|

## ACL 할당량
{: #acl-quotas}

| 리소스| 할당량|
|--------|-----|
| ACL| 지역당 30개 |
| 수신 규칙| ACL당 20개 |
| 송신 규칙 | ACL당 20개 |

## VPN 할당량
{: #vpn-quotas}

현재 계정당 VPN 리소스 제한사항은 다음과 같습니다.

| 리소스| 할당량|
|--------|-----|
| VPN 게이트웨이| 계정당 20개 |
| VPN 게이트웨이 | 구역당 3개 |
| VPN 연결 | VPN 게이트웨이당 10개 |
| IKE 정책 | 계정당 20개 |
| IPsec 정책 | 계정당 20개 |
| 단일 VPN 게이트웨이의 피어 서브넷 | 모든 VPN 연결에서 50개|
| 피어 서브넷  | 단일 VPN 연결에서 15개|
| 단일 VPN 게이트웨이의 로컬 서브넷 | 모든 VPN 연결에서 50개|
| 로컬 서브넷 | 단일 VPN 연결에서 15개 |

## Load Balancer 할당량
{: #load-balancer-quotas}

현재 로드 밸런서 리소스 할당량은 다음과 같습니다.

| 리소스| 할당량|
|--------|-----|
| Load Balancer | 계정당 20개 |
| 리스너 | Load Balancer당 10개 |
| 풀 | Load Balancer당 10개 |
| 멤버 | 풀당 50개 |

## 보조 볼륨 할당량
{: #secondary-volume-quotas}

| 리소스 | 할당량 |
|--------|----- |
| 인스턴스 작성 시 인스턴스당 보조 볼륨 | 4개의 보조 볼륨이 요청될 수 있음 |
| 4개 미만의 코어가 있는 기존 인스턴스의 경우 인스턴스당 보조 볼륨 | 4개의 보조 볼륨 |
| 4개 이상의 코어가 있는 기존 인스턴스의 경우 인스턴스당 보조 볼륨 | 최대 12개의 보조 볼륨 |


