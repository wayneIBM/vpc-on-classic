---
copyright:
  years: 2018-2019
lastupdated: "2019-06-11"

keywords: resource, storage, connection, COS, object, endpoints, cross-region, regional, datacenter

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

# VPC에서 IBM Cloud Object Storage에 연결
{: #connecting-to-ibm-cloud-object-storage-from-a-vpc}

이 문서에서는 Virtual Private Cloud에서 {{site.data.keyword.cloud}} Object Storage에 연결하는 방법을 보여줍니다. 엔드포인트 정보는 특히 클래식 인프라 환경의 {{site.data.keyword.cloud_notm}} Virtual Private Cloud에 적용됩니다.
{: shortdesc}


## IBM Cloud Object Storage(COS) 개념
{: #what-is-ibm-cloud-object-storage-cos}

{{site.data.keyword.cloud}} Object Storage(COS)는 비정형 데이터를 저장하는 웹 범위 플랫폼입니다. 이는 신뢰성, 보안, 가용성과 복제가 필요 없는 재해 복구 기능을 제공합니다.

Information stored with {{site.data.keyword.cloud_notm}} Object Storage에 저장되는 정보는 암호화되어 여러 지리적 위치에 분산됩니다. 이는 S3 API를 구현하여 액세스할 수 있습니다. 이 서비스는 IBM Cloud Object Storage 시스템에서 제공하는 분산 스토리지 기술을 사용합니다. 

IBM Cloud Object Storage는 복원성을 위해 세 가지 유형의 구성(**교차 지역**, **지역** 및 **단일 데이터 센터**)으로 사용할 수 있습니다. 
* **교차 지역** 서비스는 단일 지역을 사용하는 경우보다 더 높은 내구성 및 가용성을 제공하지만 대기 시간이 약간 더 깁니다. 이 서비스는 현재 미국, E.U. 및 A.P. 영역에서 사용할 수 있습니다. 
* **지역** 서비스는 이와 반대입니다. 단일 지역에 있는 여러 가용성 구역에 오브젝트를 분배합니다. 미국, E.U. 및 A.P. 지역에서 사용할 수 있습니다. 지정된 지역 또는 구역이 사용 불가능한 경우에도 오브젝트 저장소는 제한 없이 계속 작동합니다.
**단일 데이터 센터** 서비스는 동일한 실제 위치 내의 여러 시스템에 오브젝트를 분배합니다. 정기적으로 이 문서에서 사용 가능한 지역을 확인하십시오.

### VPC와 함께 사용할 수 있는 COS 직접 엔드포인트
{: #cos-direct-endpoints-for-use-with-vpc}

REST API 요청을 전송하려면 대상 엔드포인트 또는 URL을 설정해야 하며 각 COS 스토리지 위치에 고유한 URL 세트가 있어야 합니다. 직접 엔드포인트는 사용자의 서버에 {{site.data.keyword.cloud_notm}} 서비스에 대한 고속의 직접 연결을 제공하므로 부가 대역폭 비용이 발생하지 않습니다. COS 직접 엔드포인트를 사용하면 VPC가 전 세계 어디나 원하는 위치에 있는 COS 버킷에 연결할 수 있습니다.  

VPC에서 이 페이지에 나열된 모든 COS 엔드포인트로의 트래픽에는 비용이 부과되지 않습니다.
{: note}

* VPC 클라이언트도 공용 엔드포인트를 통해 COS 버킷에 액세스할 수 있습니다.
* VPC 클라이언트는 직접 엔드포인트가 아니라 공용 엔드포인트를 통해서만 [COS 구성 API](https://{DomainName}/apidocs/cos/cos-configuration)에 액세스할 수 있습니다. COS 구성 API는 버킷 메타데이터를 보고 버킷에 대한 일부 COS 기능을 구성하는 데 사용될 수 있습니다.
* 방화벽이 사용되는 경우 VPC 클라이언트가 COS 버킷에 액세스할 수 없습니다.

## VPC에서 IBM Cloud Object Storage(COS)에 연결하는 방법
{: #how-to-connect-to-ibm-cloud-object-storage-cos-from-a-vpc}

1. [카탈로그 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog/services/cloud-object-storage){: new_window}에서 COS를 프로비저닝하십시오.
2. 다음 절에 나열된 지역 중 하나에서 COS 버킷을 작성하십시오. 
3. 엔드포인트를 사용하여 COS 버킷과 통신하십시오.

### 지역 엔드포인트
{: #regional-endpoints}

지역 엔드포인트에서 작성된 버킷은 세 개의 데이터 센터에 데이터를 분배하며, 이는 대도시권에 분산됩니다. 가용성에 영향을 미치지 않고 이러한 데이터 센터 중 하나가 가동 중단되거나 파기될 수 있습니다. 

| **지역** | **엔드포인트** |
|------------|-------------------------------|
| 미국 남부 | `s3.direct.us-south.cloud-object-storage.appdomain.cloud`|
| 미국 동부 | `s3.direct.us-east.cloud-object-storage.appdomain.cloud`|
| E.U. 영국 | `s3.direct.eu-gb.cloud-object-storage.appdomain.cloud`|
| E.U. 독일 | `s3.direct.eu-de.cloud-object-storage.appdomain.cloud`|
| A.P. 오스트레일리아 | `s3.direct.au-syd.cloud-object-storage.appdomain.cloud`
| A.P. 일본 | `s3.direct.jp-tok.cloud-object-storage.appdomain.cloud` |


### 교차 지역 엔드포인트
{: #cross-region-endpoints}

교차 지역 엔드포인트에서 작성된 버킷은 세 개의 지역에 데이터를 분배합니다. 가용성에 영향을 미치지 않고 이러한 지역 중 하나가 가동 중단되거나 파기될 수 있습니다. BGP(Border Gateway Protocol) 라우팅을 사용하여 요청이 가장 가까운 지역의 데이터 센터로 라우팅됩니다.

가동 중단이 발생하면 자동으로 요청이 활성 지역으로 다시 라우팅됩니다. 자체 장애 복구 로직을 작성하려는 고급 사용자는 요청을 특정 지역의 엔드포인트에 전송하고 BGP 라우팅을 무시하여 이를 수행할 수 있습니다.

| **미국 교차 지역** | **엔드포인트** |
|------------|-------------------------------|
| 미국 교차 지역 | `s3.direct.us.cloud-object-storage.appdomain.cloud` |
| 댈러스 액세스 지점 | `s3.direct.dal.us.cloud-object-storage.appdomain.cloud` |
| 산호세 액세스 지점 | `s3.direct.sjc.us.cloud-object-storage.appdomain.cloud` |

| **E.U. 교차 지역** | **엔드포인트** |
|------------|-------------------------------|
| E.U. 교차 지역 | `s3.direct.eu.cloud-object-storage.appdomain.cloud` |
| 암스테르담 액세스 지점 | `s3.direct.ams.eu.cloud-object-storage.appdomain.cloud` |
| 프랑크푸르트 액세스 지점 | `s3.direct.fra.eu.cloud-object-storage.appdomain.cloud` |
| 밀라노 액세스 지점 | `s3.direct.mil.eu.cloud-object-storage.appdomain.cloud` |

| **아시아 태평양 교차 지역** | **엔드포인트** |
|------------|-------------------------------|
| A.P. 교차 지역 | `s3.direct.ap.cloud-object-storage.appdomain.cloud` |
| 도쿄 액세스 지점 | `s3.direct.tok.ap.cloud-object-storage.appdomain.cloud` |
| 서울 액세스 지점 | `s3.direct.seo.ap.cloud-object-storage.appdomain.cloud` |
| 홍콩 액세스 지점 | `s3.direct.hkg.ap.cloud-object-storage.appdomain.cloud` |


 ### 단일 데이터 센터 엔드포인트
 {: #single-datacenter-endpoints}

단일 데이터 센터는 IAM 또는 Key Protect와 같은 IBM Cloud 서비스와 동일한 위치에 배치되지 않으며 사이트 가동 중단 또는 파기 시 복원성을 제공하지 않습니다. 

네트워크 장애로 인해 파티션이 발생하여 데이터 센터가 IAM에 액세스하기 위해 핵심 IBM Cloud 지역에 연결할 수 없는 경우 시간이 경과된(stale) 상태가 될 수 있는 캐시에서 인증 및 권한 부여 정보를 읽습니다. 이 조치로 인해 최대 24시간 동안 새로 작성되거나 변경된 IAM 정책이 적용되지 않을 수 있습니다.{:important}

| **지역** | **엔드포인트** |
|------------|-------------------------------|
| 네덜란드 암스테르담 | `s3.direct.ams03.cloud-object-storage.appdomain.cloud` |
| 인도 첸나이 | `s3.direct.che01.cloud-object-storage.appdomain.cloud` |
|홍콩 | `s3.direct.hkg02.cloud-object-storage.appdomain.cloud` |
| 오스트레일리아 멜버른 | `s3.direct.mel01.cloud-object-storage.appdomain.cloud` |
| 멕시코 멕시코시티 | `s3.direct.mex01.cloud-object-storage.appdomain.cloud` |
| 이탈리아 밀라노 | `s3.direct.mil01.cloud-object-storage.appdomain.cloud` |
| 캐나다 몬트리올 | `s3.direct.mon01.cloud-object-storage.appdomain.cloud` |
| 노르웨이 오슬로 | `s3.direct.osl01.cloud-object-storage.appdomain.cloud` |
| 미국 산호세 | `s3.direct.sjc04.cloud-object-storage.appdomain.cloud` |
| 브라질 상파울루 | `s3.direct.sao01.cloud-object-storage.appdomain.cloud` |
| 대한민국 서울 | `s3.direct.seo01.cloud-object-storage.appdomain.cloud` |
| 캐나다 토론토 | `s3.direct.tor01.cloud-object-storage.appdomain.cloud` |
