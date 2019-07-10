---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# {{site.data.keyword.vsi_is_short}} 가격 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (링크된 도움말 항목)


{{site.data.keyword.vsi_is_full}}는 어떤 워크로드도 처리할 수 있도록, 최대 62개의 vCPU 및 248GB의 RAM을 선별된 지역에서 제공합니다. 청구는 시간당 요금으로만 이뤄지며 인스턴스 실행 시간이 길어질수록 할인이 적용됩니다. 가상 서버 사용 시간은 인스턴스 사용 시간 및 일시중단 시간 모두 초 단위로 계산됩니다. 예를 들어, 인스턴스가 45분 32초동안 실행된 경우에는 45분 32초에 대해 청구됩니다.
{:shortdesc}

## 지속 사용
{: #sustained-usage}

인스턴스는 시간당 요금으로 청구되지만, 인스턴스 실행 시간이 길어질수록 요금이 더 저렴해집니다. 청구 대상 월에서 시간이 지남에 따라 다음 시간당 할인이 적용됩니다.

| 1개월 내 경과 시간       | 청구 할인  | 
| ----------------------------- | ----------------- | 
| 0-20%                         | 일반 소매 요금 |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="표 1. 계층식 할인" caption-side="top"}  

이러한 할인 계층은 인스턴스를 월별로 계속 실행하는 경우 시간별 계산에 비해 10% 할인을 제공합니다. 이 할인은 기본 시간당 요금에만 적용되며 소프트웨어, 스토리지, 네트워크 또는 기타 비용에는 적용되지 않습니다.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### 지속 사용 예
{: #sustained-usage-example}

Balanced Virtual Server 제품군의 인스턴스(16개 CPU 및 64GB RAM)를 시간당 0.795달러의 기본 가격으로 구매했다고 가정해 보십시오. 해당 월에서 시간이 지남에 따라 요금은 다음과 같이 내려갑니다.

| 1개월 내 경과 시간       | 청구 할인  |  요금 예     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | 일반 소매 가격(0.795달러) | $116.07    |                
| 21-40%                        | 5%        |   $110.27   |                 
| 41-60%                        | 10%       |    $104.46  |            
| 61-80%                        | 15%        |    $98.66    |                
| 81-100%                       | 20% |       $92.86      |
{: caption="표 2. 계층식 할인 예" caption-side="top"}  

1개월 내내 실행되도록 둔 경우 이 모델의 총 요금은 522.32달러입니다. 할인을 통한 총 월별 절감 비율은 시간별 요금과 비교했을 때 10%입니다.

## 기본 인스턴스 가격
{: #base-instance-prices}

기본 인스턴스 가격은 시간당 0.087달러부터 시작합니다. Virtual Server를 작성할 때는 Virtual Server 제품군 및 프로파일 구성을 선택하라는 프롬프트가 표시됩니다. 선택하면 연관된 시간별 요금이 테이블에 표시됩니다. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## 포함된 운영 체제
{: #included-operating-systems}

다음 운영 체제는 무료로 포함되어 있습니다.

* CentOS 7.x(최신)
* Ubuntu 16.04 LTS, 18.04 이상
* Debian 8.x(최신), 9.x(최신) 이상

다른 프리미엄 운영 체제 및 추가 기능도 사용할 수 있습니다. 비용 요약에서 이러한 항목이 반영된 가격을 볼 수 있습니다.

## 청구 일시중단
{: #suspend-billing}

인스턴스의 전원을 끄면 특정 컴퓨팅 리소스에 대한 비용이 발생하지 않습니다. 인스턴스를 중지하면 청구가 자동으로 중지됩니다. 청구 일시중단 기능은 비용을 절감할 수 있도록 도와주며 해당 리소스가 다시 필요할 때 이를 다시 작성할 필요가 없게 해 줍니다.

워크로드로 인한 필요성에 대응하기 위해 인프라를 확장하거나 축소하고자 하는 상황에서는 인스턴스를 작성하고 삭제하는 데 대한 더 빠른 대안으로서 청구 일시중단 기능을 사용할 수 있습니다.
{:tip}

### 청구 세부사항
{: #billing-details}

사용자는 가상 서버 인스턴스의 전원이 꺼졌을 때 비용 발생이 중단되는 리소스와 계속되는 리소스가 무엇인지 알아야 합니다.

청구는 IBM Cloud 콘솔, CLI 또는 API를 통해 가상 서버 인스턴스의 전원이 꺼진 경우에만 일시중단됩니다. OS를 통해 가상 서버 인스턴스의 전원을 직접 끄는 경우에는 해당 인스턴스에 대한 청구가 일시중단되지 않습니다.
{:note}

청구 일시중단이 리소스 비용에 주는 영향에 대한 세부사항은 다음 표를 검토하십시오.

| 리소스                      | 청구 중지   | 청구 지속 |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| 대역폭 업그레이드            |          X        |                  |
| 운영 체제 라이센스     |          X        |                  |
| 유동 IP, 로드 밸런서, 기타 연결된 네트워킹 오퍼링 |                   |         X        |
| 스토리지                       |                   |         X        |
{: caption="표 1. 리소스 청구 세부사항" caption-side="top"}   

사용 시간은 가상 서버 인스턴스 사용 시간 및 일시중단 시간 모두 초 단위로 계산됩니다. 인스턴스의 전원을 꺼 청구 일시중단 기능을 시작한 경우가 한 번도 없는 경우에도, 청구는 인스턴스 라이프사이클의 초 단위로 계산됩니다. 
{:note}

#### 청구 일시중단 및 지속 사용 할인
{: #suspend-billing-and-sustained-usage-discounts}

지속 사용 할인의 경우, 일시중단된 이미지는 이전에 중단되었던 부분부터 할인 계층을 다시 시작합니다. 즉, 사용 할인은 이미지 사용 시간에 대해서만 적용되며 이미지가 일시중단된 시간에는 적용되지 않습니다.

#### 최소 사용 비용
{: #minimum-usage-charge}

가상 서버 인스턴스에는 월당 최소 사용 비용이 있습니다. 사용량이 25%보다 크면 실제 사용량에 대해 청구됩니다. 사용량이 청구 주기의 25% 미만인 경우에는 최소 비용인 25%가 적용됩니다. 

예를 들어, 전체 청구 주기(720시간) 동안 실행된 인스턴스가 있다고 가정해 보십시오. 이 시간 중에, 인스턴스가 577시간 동안 일시중단되고 143시간 동안 실행되었습니다. 이 인스턴스에 대해서는 180시간에 대한 비용이 청구됩니다(해당 청구 기간의 최소 사용 시간).  

반면, 작성되어 총 400시간의 청구 주기 동안 유지된 후 중지된 인스턴스가 있다고 가정해 보십시오. 이 시간 중에, 인스턴스가 120시간 동안 일시중단되고 280시간 동안 실행되었습니다. 이 인스턴스에 대해서는 사용 시간인 280시간에 대한 비용이 청구됩니다.

#### 청구서
{: #billing-invoice}

Virtual Server에 대한 청구를 일시중단하면 청구서에서 몇 가지 변경사항을 볼 수 있습니다. 이제 관련 비용이 사용 기반 세부사항으로 표시됩니다. 예를 들면 사용 가능한 시간, 사용된 시간 및 총 청구 시간을 반영하는 다음 추가 항목을 볼 수 있습니다.

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### 리소스 세부사항
{: #resource-details}

#### 스토리지
{: #suspend-billing-storage}

가상 서버 인스턴스에 대한 청구를 일시중단하는 경우, 연관된 스토리지는 유지되지만 가상 서버 인스턴스의 전원이 꺼진 상태에서는 해당 데이터에 액세스할 수 없습니다. 인스턴스에 대한 청구를 재개하고 나면 데이터에 액세스할 수 있으며 정상 청구가 다시 시작됩니다.

#### IP 주소
{: #suspend-billing-ip-addresses}

인스턴스가 일시중단된 동안 모든 네트워크 구성 및 IP(서브넷 범위의 사설 IP)는 유지됩니다.

인스턴스 세부사항 페이지에서 자신의 디바이스가 중지되었는지 볼 수 있습니다. 상태가 언제 변경되었는지 보려면 탐색 분할창에서 **활동**을 클릭하십시오. 

#### 제한사항
{: #suspend-billing-limitations}

일시중단된 Virtual Server는 계정 전체 디바이스 할당량에 대해 계속 계산됩니다.
