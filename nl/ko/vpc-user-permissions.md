---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

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

# VPC 리소스에 대한 사용자 권한 관리
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.cloud}} Virtual Private Cloud는 계정 관리자가 VPC 리소스에 대한 사용자의 액세스를 제어할 수 있도록 하는 역할 기반 액세스 제어를 사용합니다. 

IBM Cloud VPC가 역할 기반 액세스 제어를 사용하는 방법과 VPC가 IAM 역할 및 액세스 정책을 사용하는 방법에 대한 자세한 정보는 [VPC 리소스에 대한 역할 기반 액세스 권한 지정](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources)을 참조하십시오.

이 문서는 계정 관리자가 계정에 사용자를 추가하고 이들에게 [VPC 인프라 리소스](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)를 관리하는 데 필요한 권한을 부여하는 방법을 보여줍니다. 여기서는 VPC 관리자에 대한 두 가지 일반적인 시나리오를 다룹니다.

* **단순 액세스 시나리오:** 사용자가 VPC 인프라 리소스(Virtual Private Cloud 포함)를 작성하고 사용할 수 있도록 사용자에게 액세스 정책을 지정하는 방법을 보여줍니다. 

* **팀 액세스 시나리오:** 두 개별 팀이 각 팀에게 지정된 VPC 리소스를 작성하고 사용할 수 있도록 리소스 그룹 및 액세스 정책을 설정하는 방법을 보여줍니다.

VPC에 대한 IAM 액세스 정책의 변경사항은 적용되는 데 최대 10분이 소요될 수 있습니다.
{: note}

## 단순 액세스 시나리오
{: #simple-access-scenario}

이 시나리오에서는 개별 사용자를 설정하는 데 필요한 기본 단계를 다룹니다. 여기서는 두 가지 경우(계정에 새 사용자를 초대하는 경우 및 기존 사용자의 권한을 수정하는 경우)를 다룰 것입니다.

### VPC 리소스를 작성하거나 관리할 새 사용자 초대
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

IBM Cloud 사용자를 계정으로 초대한 후 이들이 VPC 리소스를 보고, 작성하고 업데이트할 수 있는 액세스 권한을 가질 수 있도록 `VPC Infrastructure`에 대한 액세스 권한을 부여하십시오. 이 절에서는 IAM 단계에 대한 간략한 개요를 제공합니다. 이 문서의 끝 부분에 있는 **관련 링크** 절의 링크를 통해 추가 정보를 얻을 수 있습니다.

IAM에서 VPC 서비스 및 리소스로 사용자를 초대하는 데 필요한 기본 단계는 다음과 같습니다.

1. IBM Cloud 콘솔에서 [IAM 사용자 UI ![사용자 외부 링크](../icons/launch-glyph.svg "사용자 외부 링크")](https://{DomainName}/iam#/users){: new_window}로 이동하십시오.
2. **사용자** 페이지에서 **사용자 초대**를 클릭하십시오.
3. **사용자 초대** 페이지의 **사용자** 섹션에서 **이메일 주소** 필드에 초대할 사용자의 이메일 주소를 입력하십시오.
4. **액세스** 섹션에서 **서비스**를 펼친 후 다음 태스크를 완료하십시오.
  * **액세스 권한 지정 대상** 목록에서 **리소스**를 선택하십시오.
  * **서비스** 목록에서 **VPC 인프라**를 선택하십시오. 
  * 사용자에게 지정할 플랫폼 액세스 역할을 선택하십시오. 이는 **관리자**, **편집자**, **운영자** 또는 **뷰어**가 될 수 있습니다.
  * **사용자 초대**를 클릭하십시오.

### 기존 사용자에게 VPC 리소스 관리 권한 부여
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

이 시나리오에서는 계정 내의 기존 사용자에게 VPC 리소스를 편집할 수 있는 권한을 부여하는 데 필요한 기본 단계를 다룹니다.

다음 단계에서는 두 가지 IAM 정책을 작성합니다. 사용자가 VPC 인프라 리소스를 작성하고 사용하려면 두 정책이 모두 필요합니다. 모든 리소스는 계정의 기본 리소스 그룹에 속해야 합니다.

1. IBM Cloud 콘솔에서 [IAM 사용자 UI ![사용자 외부 링크](../icons/launch-glyph.svg "사용자 외부 링크")](https://{DomainName}/iam#/users){: new_window}로 이동하십시오.
2. 권한을 부여할 사용자를 선택하십시오.
3. **액세스 정책** 탭에서 **액세스 권한 지정**을 선택하십시오.
4. **리소스 그룹 내 액세스 권한 지정**을 선택하십시오.
5. 계정의 기본 리소스 그룹을 선택하십시오.
6. **리소스 그룹에 대한 액세스 권한 지정** 옵션이 **뷰어**로 설정되어 있도록 하십시오.
7. 올바른 서비스를 지정하려면 **VPC 인프라**를 선택하십시오. 
8. **리소스 유형** 값이 **모든 리소스 유형**으로 설정되었는지 확인하십시오.
9. **편집자** 역할을 선택하십시오.
10. **지정**을 클릭하십시오.

이제 해당 사용자에게 계정의 기본 리소스 그룹에서 VPC 리소스를 작성하고 사용할 수 있는 권한이 부여되었습니다. 첫 번째 정책(1단계 - 6단계)은 사용자가 계정의 기본 리소스 그룹을 볼 수 있도록 합니다. 두 번째 정책(7단계 - 10단계)은 사용자에게 VPC 인프라 리소스에 대한 **편집자** 역할을 지정했지만 액세스는 계정의 기본 리소스 그룹에 있는 리소스로 제한됩니다.

{: tip}

## 사용자의 권한 보기
{: #viewing-your-user-s-permissions}

정책은 사용자의 **액세스 정책** 탭에서 볼 수 있습니다.

다음 CLI 명령을 사용하여, 정책 또는 액세스 그룹별로 사용자에게 지정된 리소스 그룹 권한을 유효성 검증할 수 있습니다.

**정책별로**
```
ibmcloud iam user-policies <username>
```
**액세스 그룹별로**
```
ibmcloud iam access-groups -u <username>
```

VPC에 대한 IAM 액세스 정책의 변경사항은 적용되는 데 최대 10분이 소요될 수 있습니다.
{: note}

## 팀 액세스 시나리오
{: #team-access-scenario}

이 시나리오에서는 다른 팀이 별도의 VPC 리소스에 액세스할 수 있도록 계정 관리자가 권한을 지정하는 방법에 대해 다룹니다. 이 예에서는 _리소스 그룹_을 사용하여 두 팀의 개별 리소스 액세스 권한을 설정합니다. 이 예의 목적상 리소스는 팀 간에 공유되지 않습니다.

이 예에서는 리소스 그룹 작성, 액세스 그룹 작성, 개별 VPC 리소스에 대한 액세스를 제공하기 위한 적절한 정책 지정 프로세스를 안내합니다.

두 별도 Virtual Private Cloud를 사용하는 서로 다른 두 프로젝트 팀을 설정한다고 가정하십시오. 여기서 사용자는 각 팀이 자기 팀의 VPC 리소스에 대해서만 액세스 권한을 갖도록 팀 구성원에게 액세스 권한을 지정합니다.

* 첫 번째 팀은 테스트 팀입니다. 이 팀에게는 `test_vpcs`라는 리소스 그룹의 VPC에 대한 팀 액세스 권한을 지정합니다.
* 두 번째 팀은 프로덕션 팀입니다. 이 팀에게는 `production_vpcs`라는 리소스 그룹의 VPC에 대한 액세스 권한이 지정됩니다.

이 전략은 임의의 수의 팀에 서로 별도의 VPC 리소스를 지정하는 데 사용할 수 있습니다. 그러나 모든 리소스는 동일한 계정 VPC 할당량을 공유합니다. 할당량 및 한계에 대한 자세한 정보는 [VPC 할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas)을 참조하십시오.
{: tip}

### 1단계: 리소스 그룹 작성
{: #step-1-create-resource-groups}

기본적으로 계정 관리자는 새 리소스 그룹을 작성할 수 있는 액세스 권한을 가지고 있습니다. 다른 사용자는 리소스 그룹을 작성할 수 있게 해 주는, **모든 계정 관리 서비스**에 대한 **편집자** 역할이 먼저 지정되어야 합니다.

첫 번째 태스크는 각 팀의 VPC 리소스를 포함할 리소스 그룹을 작성하는 것입니다.

1. `test_team`이라는 리소스 그룹을 작성하십시오.  
2. `production_team`이라는 리소스 그룹을 작성하십시오.  

리소스 그룹 작성 방법에 대한 자세한 정보는 [리소스 그룹 관리](/docs/resources?topic=resources-rgs)를 참조하십시오.

### 2단계: 액세스 그룹 작성
{: #step-2-create-access-groups}

리소스 액세스 권한은 개별 사용자 또는 사용자 그룹에 지정될 수 있습니다. 동일한 액세스 권한을 가진 사용자 그룹을 _액세스 그룹_이라고 합니다. 액세스 그룹은 특정 유형의 VPC 액세스 권한을 필요로 하는 팀 구성원의 각 그룹을 나타내며, 이 시나리오에서는 총 4개의 고유 액세스 그룹을 작성합니다.

**태스크:** 이름이 `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs`, `production_team_view_vpcs`인 네 개의 액세스 그룹을 작성하십시오.

액세스 그룹 작성에 대한 도움말은 [액세스 그룹 작성](/docs/iam?topic=iam-groups#create_ag)을 참조하십시오.

### 3단계: 방금 작성한 액세스 그룹에 IAM 정책 추가
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

`test_team` 액세스 그룹이 VPC 리소스를 관리(작성, 업데이트 및 삭제)할 수 있도록 필요한 VPC 액세스 정책을 추가하십시오.  

1. IBM Cloud 콘솔에서 [IAM 그룹 UI ![사용자 외부 링크](../icons/launch-glyph.svg "사용자 외부 링크")](https://{DomainName}/iam#/groups){: new_window}로 이동하십시오.
2. 원하는 액세스 그룹을 선택하십시오(`test_team_manage_vpcs`부터 시작).
3. **액세스 정책** 탭에서 **액세스 권한 지정**을 클릭하십시오.
4. **리소스 그룹 내 액세스 권한 지정**을 선택하십시오.
5. 원하는 리소스 그룹을 선택하십시오(`test_team`부터 시작).
6. **리소스 그룹에 대한 액세스 권한 지정**이 **뷰어**로 설정되어 있도록 하십시오.
7. **VPC 인프라** 서비스를 선택하십시오.
8. **리소스 유형**이 **모든 리소스 유형**으로 설정되어 있도록 하십시오.
9. **편집자** 역할을 선택하십시오.
10. **지정**을 선택하십시오.

이들 단계는 `test_team` 리소스 그룹에 대한 **뷰어** 액세스 권한도 지정합니다. `test_team` 리소스 그룹 내의 리소스를 업데이트, 작성 및 삭제하려면 뷰어 액세스가 필요합니다.

{: tip}

나머지 세 액세스 그룹에 대해 이전 단계를 반복하십시오. 이러한 태스크는 액세스 그룹과 리소스 그룹을 대응시키기 위해 수행해야 합니다.

* `test_team_view_vpcs` 액세스 그룹에 `test_team` 리소스 그룹 내부의 VPC 인프라 리소스에 대한 **뷰어** 역할을 지정하십시오. 
* `production_team_manage_vpcs` 액세스 그룹에 `production_team` 리소스 그룹 내부의 VPC 인프라 리소스에 대한 **편집자** 역할을 지정하십시오. 
* `production_team_view_vpcs` 액세스 그룹에 `production_team` 리소스 그룹 내부의 VPC 인프라 리소스에 대한 **뷰어** 역할을 지정하십시오. 

VPC 리소스에 대한 **편집자** 액세스 권한이 있는 사용자는 리소스를 볼 수도 있습니다. 구성원을 **편집자** 액세스 그룹과 **뷰어** 액세스 그룹에 동시에 추가할 필요는 없습니다.

#### 뷰어 액세스 권한 설정
{: #setting-up-viewer-access)

VPC 인프라 `Floating IP` 리소스는 계정의 기본 리소스 그룹에서 작성됩니다. 따라서 `Floating IP`를 관리해야 하는 사용자는 계정의 기본 리소스 그룹에 대한 **뷰어** 액세스 권한을 필요로 합니다.
{: tip}

**뷰어** 액세스 권한을 작성하는 방법은 다음과 같습니다.

1. IBM Cloud 콘솔에서 [IAM 그룹 UI ![사용자 외부 링크](../icons/launch-glyph.svg "사용자 외부 링크")](https://{DomainName}/iam#/groups){: new_window}로 이동하십시오.
2. 원하는 액세스 그룹을 선택하십시오(`test_team_manage_vpcs`부터 시작).
3. **액세스 정책** 탭에서 **액세스 권한 지정**을 클릭하십시오.
4. **계정 관리 서비스에 대한 액세스 권한 지정**을 선택하십시오.
7. 서비스 **모든 계정 관리 서비스**를 선택하십시오.
9. **뷰어** 역할을 선택하십시오.
10. **지정**을 선택하십시오.

이전 단계를 반복하여 `production_team_manage_vpcs` 액세스 그룹에 **모든 계정 관리 서비스**에 대한 **뷰어** 역할을 지정하십시오.

### 4단계: 사용자를 액세스 그룹에 추가
{: #step-4-add-users-to-the-access-groups}

이제 팀 구성원(사용자)을 적절한 액세스 그룹에 지정할 수 있습니다. 테스트 팀이 VPC를 관리할 수 있도록 하는 액세스 그룹에 관리 테스트 팀의 각 구성원을 추가하려면 다음 단계를 따르십시오.

1. IBM Cloud 콘솔에서 [IAM 그룹 UI ![사용자 외부 링크](../icons/launch-glyph.svg "사용자 외부 링크")](https://{DomainName}/iam#/groups){: new_window}로 이동하십시오.
2. 원하는 액세스 그룹을 선택하십시오(`test_team_manage_vpcs`부터 시작).
3. **사용자** 탭에서 **사용자 추가**를 클릭하십시오.
4. 액세스 그룹에 추가할 각 사용자를 선택하십시오.
5. **그룹에 추가**를 클릭하십시오.

이전 단계를 반복하여 다음 태스크를 수행하십시오.
* 원하는 사용자를 `test_team_view_vpcs` 액세스 그룹에 지정하십시오.
* 원하는 사용자를 `production_team_manage_vpcs` 액세스 그룹에 지정하십시오.
* 원하는 사용자를 `production_team_view_vpcs` 액세스 그룹에 지정하십시오.

사용자에게 관리 액세스 권한이 있는 경우에는 **뷰어** 액세스 권한을 지정할 필요가 없습니다.
{: tip}

## 다음 단계
{: #permissions-next-steps}

두 예제 팀이 VPC를 사용할 수 있도록 설정되었습니다. 이 시점에서, `test_team_manage_vpcs` 및 `production_team_manage_vpcs` 액세스 그룹의 구성원은 자신에게 할당된 리소스 그룹에서 VPC를 작성할 수 있습니다.


## 관련 링크
{: #permissions-related-links}

* [ID 및 액세스 관리](/docs/iam?topic=iam-getstarted)
* [사용자 및 액세스 관리](/docs/iam?topic=iam-iamuserinv)
* [IAM 개념](/docs/iam?topic=iam-iamoverview)
