---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# VPC에서의 블록 스토리지 작성 및 관리
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud는 부트 스토리지 볼륨을 제공하며 가상 서버 인스턴스(VSI)에 적합한 하이퍼바이저가 마운트된 고성능 데이터 스토리지인 추가 블록 스토리지 볼륨을 허용합니다. VPC 인프라는 추가 성능 및 보안으로 다중 지역 및 구역에서 빠른 스토리지 스케일링을 제공합니다.

## 특성 요약
{: #summary-of-characteristics}

{{site.data.keyword.vsi_is_full}} 인스턴스를 프로비저닝할 때 100GB의 범용 IOPS(3IOPS/GB) 블록 스토리지 볼륨이 자동으로 기본 부트 볼륨으로 작성되어 VSI에 연결됩니다.
프로비저닝 중에 부트 볼륨의 이름을 바꾸고 볼륨 크기를 변경할 수 있습니다.
{:shortdesc}

* VPC 네트워크에서 가상 서버 인스턴스를 프로비저닝할 때마다 블록 스토리지 볼륨을 작성할 수 있습니다.  
* VSI 라이프사이클과 독립적으로 새 볼륨을 작성하고 이러한 볼륨을 VSI에 연결할 수 있습니다.
* 자동으로 VSI에 연결되는 보조 데이터 볼륨을 작성할 수 있습니다. 
* 데이터 볼륨이 VSI에서 분리되는 경우에도 유지됩니다. 따라서 나중에 볼륨을 새 인스턴스에 연결할 수 있습니다. 
* VSI를 삭제할 때 데이터 볼륨이 자동으로 삭제되는지 여부를 지정할 수 있습니다.  
* 부트 볼륨은 해당 VSI를 삭제할 때 삭제됩니다.
* 기본적으로 부트 및 데이터 볼륨은 IBM 관리 암호화로 암호화됩니다. 
* 고유 암호화 키를 사용하거나 VSI 프로비저닝 중 또는 독립형 볼륨 작성 시 볼륨을 암호화할 수 있습니다.


## 블록 스토리지 볼륨
{: #block-storage-volumes}

블록 스토리지 볼륨 및 VPC 관련 작업에 대한 완전한 정보는 [Block Storage for VPC 문서](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)에 있습니다.

VPC용 블록 스토리지 볼륨에 대한 개요 정보는 [Block Storage for VPC 정보](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about)를 참조하십시오. 

VSI 프로비저닝과 별도로 볼륨 작성을 시작하려면 [{{site.data.keyword.block_storage_is_short}} 시작하기](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started)를 참조하십시오.



## VPC 블록 스토리지 볼륨 작성 및 이름 지정에 대한 우수 사례:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* 볼륨을 프로비저닝하기 전에 스토리지 요구사항을 판별하십시오. 적절한 용량 및 IOPS 성능을 고려하십시오.
* 사전 정의된 IOPS 프로파일이 용량 및 성능 요구사항을 가장 잘 충족하는지 또는 사용자 정의 설정을 지정하는 것이 더 나은지를 결정하십시오.
* 가상 서버 인스턴스 프로비저닝 중에 보조 데이터 볼륨을 작성할지 여부를 결정하십시오. 이러한 볼륨은 자동으로 인스턴스에 연결됩니다.
* VSI를 삭제할 때 VSI에 연결된 볼륨이 자동으로 삭제되도록 할지 여부를 결정하십시오.
* 고유 암호화 키로 볼륨을 암호화하는 경우 키가 올바른지, 키를 사용할 수 있는 권한이 있는지, 현재 지역에서 사용될 수 있는지 확인하십시오.
* 부트 볼륨을 포함하여 모든 볼륨은 프로비저닝 시에 이름을 지정해야 합니다.
* 연결 시 모든 볼륨 연결에 이름을 지정하는지 확인하십시오.
* 각 볼륨에는 계정 및 지역 내에서 고유한 이름이 있어야 합니다.

볼륨이 삭제된 후에는 해당 볼륨의 이름을 다시 사용할 수 있습니다. 그러나 볼륨 이름을 다시 사용하면 청구와 관련하여 혼동이 발생할 수 있습니다.
{:note}
