---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, control

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

# VPC 리소스에 대한 역할 기반 액세스 권한 지정
{: #assigning-role-based-access-to-vpc-resources}

계정 관리자는 개별 사용자의 _역할_에 따라 _리소스_에 대한 액세스를 제어하는 권한 부여 _정책_을 이용할 수 있습니다. 정책은 다음 대상의 권한 부여를 위해 적용될 수도 있습니다.

(1) _액세스 그룹_이라고 하는 사용자 그룹,

(2) 단일 _리소스_의 지정된 특성, 또는

(3) _리소스 그룹_이라고 하는 리소스 집합.

리소스에 대한 권한 부여 및 사용자에 대한 권한 부여는 서로 독립적으로 지정될 수 있습니다.

사용자, 사용자 액세스 그룹, 리소스 그룹 및 정책의 작성에 대한 자세한 정보는 [리소스 정보](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)를 참조하십시오.

## IAM 기반 액세스 제어
{: #iam-based-access-control}

일반적으로 {{site.data.keyword.cloud}} Virtual Private Cloud 권한 및 리소스 관리 방법은 IBM Cloud Identity and Access Management(IAM) 서비스에 맞춰 조정됩니다. IAM, 리소스 그룹, 액세스 그룹 전반에 대한 자세한 정보는 다음 IBM Cloud 문서를 참조하십시오.

* [IBM Cloud IAM](/docs/iam?topic=iam-getstarted)
* [리소스 그룹](/docs/overview?topic=overview-whatis-rgs)
* [액세스 그룹](/docs/overview?topic=overview-cloudaccess)

IBM Cloud IAM 서비스의 특정 기능은 IBM Cloud VPC에서 사용할 수 있도록 사용자 정의되었습니다. VPC에 적용되는 IAM 권한 부여 정책에 대해서는 [리소스 정보](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources) 문서에 더 자세히 설명되어 있습니다.

요약하면, IAM 기반 권한은 다음 항목에 따라 지정할 수 있습니다.

* 개별 사용자
* 액세스 그룹(사용자 그룹)
* 특정 유형의 리소스
* 리소스 그룹

## 사용자 권한 지정
{: #assigning-user-permissions}

사용자의 경우 액세스는 시스템 정의 IAM 역할을 지정하여 제어됩니다.

* 관리자
* 편집자
* 운영자
* 뷰어

다음 표는 각 사용자 역할과 해당 역할이 VPC에 대해 허용하는 제어 유형의 대응 관계를 보여줍니다.

| 역할 이름 | 허용되는 액세스 유형 |
|-----------|-------------------------|
| 뷰어 | VPC 보기, VPC 나열  |
| 편집자 | VPC 보기, VPC 나열, VPC 작성, VPC 삭제, VPC 업데이트 |
| 운영자  | VPC 보기, VPC 나열 |
| 관리자 | VPC 보기, VPC 나열, VPC 작성, VPC 삭제, VPC 업데이트, 다른 사용자에게 정책 지정 |


## 다음 단계
{: #permissions-next-steps}

사용자에게 특정 태스크에 대한 역할 기반 권한을 부여하는 데 대한 단계별 지시사항은 [VPC 리소스에 대한 사용자 권한 관리](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) 주제를 참조하십시오.
