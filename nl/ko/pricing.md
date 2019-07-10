---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# VPC의 가격
{: #pricing-for-vpc}

이 표에는 {{site.data.keyword.cloud}} Virtual Private Cloud와의 인터넷 데이터 전송에 대한 가격이 요약되어 있습니다. VPC와 다른 클래식 IBM Cloud 서비스 간의 트래픽에는 비용이 발생하지 않는다는 점을 기억하십시오. 또한 Paygo 서비스의 경우에는 서비스 티어가 특정 VPC 인스턴스가 아니라 사용자의 계정에 바인드됩니다. 금액은 미국 달러로 표시됩니다.

IBM Cloud VPC 내에서 사용되는 [가상 서버 인스턴스](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) 및 [블록 스토리지](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing)에는 별도의 가격이 적용됩니다. 

## 인터넷 데이터 전송에 대한 무료 사용량
{: #free-allowances-for-internet-data-transfer}

| 데이터 전송 | 모든 IBM Cloud VPC 고객에 대한 비용 |
|---------------|------------------|
| 구역 내 | 무료 |
| 같은 지역의 구역 간 | 무료 |
| 공용 게이트웨이 사용 | 무료(PGW에서 사용하는 유동 IP에 대해서만 비용이 청구됨) |

## 유동 IP 가격
{: #floating-ip-pricing}

유동 IP는 예약 시부터 1달러(미국)/월의 비율로 비용이 청구됩니다. 이 비용은 해당 유동 IP가 VSI와 연관되지 않거나 사용 중이 아닌 경우에도 청구됩니다. 해당 유동 IP가 며칠밖에 예약되지 않은 경우에도 1달러의 월별 비용이 청구됩니다.

## 인터넷 데이터 전송에 대한 기본 PayGo 가격
{: #basic-paygo-pricing-for-internet-transfer}

| 데이터 전송 | 데이터의 양 | PayGo 가격 |
|-----------|-----------|------------------|
| 인터넷으로 송신 | 0 - 5GB | 무료 |
|  | 6 - 10,000GB | 0.087달러/GB |
|  | 10,001 - 50,000GB | 0.083달러/GB |
|  | 50,001 - 150,000GB | 0.07달러/GB |
|  | 150,001GB 이상 | 0.05달러/GB |


새 VPC를 작성하는 경우에는 초기 청구 비용이 콘솔 UI 또는 API에 표시되기까지 최대 1시간이 소요될 수 있습니다.
{: tip}

공용 게이트웨이 또는 유동 IP가 있는 경우에는 해당 시간 동안 송신 트래픽을 전송하지 않은 경우에도 어느 정도의 최소 송신 비용이 표시될 수 있습니다. 이러한 비용은 계정을 운영하는 데 필요한 ARP 트래픽에 대한 것입니다.
{: important}

## VPC용 LBaaS 가격
{: #lb-for-vpc-pricing}

VPC용 로드 밸런서 가격은 월별로 계산되는 다음 메트릭을 기반으로 합니다.
* 서비스 사용 시간
*  처리된 데이터 


| 지역 | LB 서비스 사용(시간당) | GB당 처리된 LB 데이터 |
|------------|--------------------------|-------------------------|
| 댈러스 | $0.025 | $0.008 |
| 프랑크푸르트 | $0.027 | $0.009 |
| 도쿄 | $0.028 | $0.009 |

가격은 다중 구역 지역별로 다릅니다.
{: note}

다음 차트는 댈러스 지역을 기준으로 미국 달러로 공용 로드 밸런싱에 월별 4500GB를 사용하는 고객의 예를 보여줍니다.

| 항목 | 사용량 | 요금 | 비용 |
|---------|--------|---------|---------|          
| LB 서비스 사용 | 720시간 | $0.025/시간 | 월별 $18 |
| 처리된 데이터 | 4500GB | $0.008/GB | 월별 $36 |

**표 1: 월별 비용 예제. 이 시나리오의 총 비용은 월별 $54(USD)입니다.**


## VPC용 VPN 가격
{: #vpn-for-vpc-pricing}

| 지역 | 시간당 연결(피어) | 시간당 인스턴스(게이트웨이) |
|------------|--------------------------|-------------------------|
| 댈러스 | $0.04 | $0.045 |
| 프랑크푸르트 | $0.044 | $0.0495 |
| 도쿄 | $0.0452 | $0.0585 |

VPNaaS 사용의 결과로 인터넷으로 데이터를 전송하는 경우 인터넷으로의 아웃바운드인 일반 데이터 전송으로 요금이 부과되며 VPC 레벨에서 청구됩니다.
{: note}
