---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Block Storage for VPC 가격
{: #block-storage-pricing}

{{site.data.keyword.block_storage_is_short}} 가격은 선택한 스토리지 프로파일에 따라 프로비저닝된 IOPS 또는 용량을 기반으로 합니다.

| IOPS 티어 | 공개된 월별 요금^1 |
|------------|--------------|
|  3IOPS/GB | 0.13USD/GB |
|  5IOPS/GB | 0.25USD/GB |
| 10IOPS/GB | 0.58USD/GB |
| 사용자 정의 IOPS | 0.10USD/GB + 0.07USD/IOPS |

^1 [시간별로 계산되는](#how-are-charges-calculated) 공개된 월별 가격입니다.

## 비용은 어떻게 계산됩니까?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}}는 볼륨이 삭제될 때 또는 청구 주기 종료까지(둘 중 먼저 다가오는 시간) 블록 스토리지 파일 볼륨이 계정에 존재하는 시간을 기반으로 시간별로 계산됩니다. 시간별 청구는 사용자 정의 IOPS와 다르게 사전 정의된 IOPS 티어에 대해 계산됩니다. 다음 예를 참조하십시오.

### 범용 IOPS 티어를 사용하여 볼륨에 대한 청구 계산
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

범용 3IOPS/GB 티어로 1000GB 볼륨을 프로비저닝한 후 삭제하기 전에 72시간 동안 볼륨을 사용합니다. 총 볼륨 가격은 다음과 같이 시간별로 청구됩니다.

((1000GB x 0.13USD/GB)/730시간) x 72시간 = $12.82

### 사용자 정의 IOPS를 사용하여 볼륨에 대한 청구 계산
{: #billing-calculation-for-a-volume-with-custom-IOPS}

2500IOPS로 1000GB 볼륨을 프로비저닝한 후 삭제하기 전에 72시간 동안 볼륨을 사용합니다. 총 볼륨 가격은 다음과 같이 시간별로 청구됩니다.

(((1000 x 0.10USD/GB) + (2500 x 0.07USD)) / 730시간) x 72시간 = $27.12
