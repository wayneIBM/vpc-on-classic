---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# 릴리스 정보
{: #release-notes}

다음 릴리스 정보를 사용하여 {{site.data.keyword.cloud}} Virtual Private Cloud의 최신 변경사항에 대해 알아보십시오.
{:shortdesc}

## 2019년 6월 14일
{: #june-14-2019}

**IBM Cloud VPC UI에 대한 업데이트**

- **SSH 키 및 리소스 그룹.**
    * 리소스 그룹이 SSH 키 목록 보기에 표시됩니다.
    * SSH 키 프로비저닝 모달에서 리소스 그룹을 선택할 수 있습니다.
- **프로파일 및 대역폭.** 각 프로파일에 지정된 대역폭 상한이 **인기 있는 프로파일** 및 **모든 프로파일** 디스플레이 페이지에 표시됩니다.
- **리소스 그룹.** VSI 프로비저닝 페이지에 리소스 그룹 드롭 다운이 표시됩니다.

**Block Storage for VPC에 대한 업데이트**
- **볼륨 이름 길이**가 63자의 영숫자 문자로 증가되었습니다.
- 다음 **볼륨 오류 코드**의 이름이 변경되었거나 메시지가 업데이트되거나 삭제되었습니다.
    * `volume_encryption_key_account_id_mismatch`의 이름이 `volume_crn_account_id_mismatch`로 변경되었습니다.
    * `volume_encryption_key_cname_mismatch`의 이름이 `volume_crn_cname_mismatch`로 변경되었습니다.
    * `volume_encryption_key_region_not_found`가 제거되었습니다.

**SDK에 대한 업데이트**

- **Terraform 제공자 v0.17.1**이 릴리스되었습니다. [최신 바이너리](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1)를 다운로드하십시오.
- 또한 **Docker 이미지**가 최신 Terraform 제공자로 업데이트되었습니다. `docker pull ibmterraform/terraform-provider-ibm-docker:latest` 명령을 사용하여 최신 Docker 이미지를 가져올 수 있습니다.
- 코드가 현재 릴리스된 버전의 VPC on Classic 인프라에서 작동하도록 `generation`을 제공자 인수로 설정하거나 `IC_GENERATION= 1`을 내보내야 합니다. 기본적으로 이 값은 2(현재 베타이고 곧 출시 예정인 VPC)로 설정됩니다.
- 제공자 인수(`bluemix_api_key` 및 `bluemix_timeout`)가 더 이상 사용되지 않으므로 Terraform 플랜을 실행하거나 적용할 때 경고가 발생할 수 있습니다.

## 2019년 6월 7일
{: #june-07-2019}

- **IBM Cloud VPC가 이제 GA(Generally Available)되었습니다.** 모든 [종량과금제 계정](/docs/account?topic=account-accounts)이 VPC 리소스를 프로비저닝할 수 있습니다.
지금 [IBM Cloud 콘솔](https://{DomainName}/vpc/overview)에 로그인하여 시작하십시오!

- **이제 인스턴스, 키, 볼륨 및 보안 그룹에 대한 개별 권한 부여 정책을 설정할 수 있습니다.** [API 및 CLI 호출에 필요한 리소스 권한](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls)에서 이러한 리소스를 관리하는 데 필요한 역할을 알아보십시오. 초기 액세스 사용자는 기존 정책에 영향을 받지 않습니다.

- **가상 네트워크 인터페이스에 대한 포트 속도 매개변수가 사용자 인터페이스, CLI 및 API에서 제거되었습니다.**

- **이제 모니터링 메트릭에 대한 다중 지역 지원이 사용자 인터페이스에서 사용 가능합니다.**


## 2019년 5월 31일
{: #may-31-2019}

**새 API 버전이 사용 가능합니다.** VPC on Classic API 버전이 `2019-01-01`에서 `2019-05-31`로 업데이트되었습니다. 가이드라인 및 우수 사례는 Virtual Private Cloud on Classic API의 [버전화](https://{DomainName}/apidocs/vpc-on-classic#versioning)를 참조하십시오.

## 2019년 5월 24일
{: #may-24-2019}

- **삭제 중 상태의 로드 밸런서 인스턴스가 이제 사용자 인터페이스의 목록 보기에 표시됩니다.** UI에서 로드 밸런서를 삭제하려고 요청하면 삭제 프로세스가 완료될 때까지 목록 보기에 로드 밸런서가 계속 표시됩니다.

- **사용자 인터페이스에서 계층 7 로드 밸런서 기능을 사용할 수 있습니다.** 이제 사용자 인터페이스에서 계층 7 로드 밸런서 기능을 구성할 수 있습니다.

- **이제 사용자 인터페이스에서 로드 밸런서 보기 및 편집 정책을 사용할 수 있습니다.** 로드 밸런서 정책을 보고 편집하기 위한 새 기능을 사용자 인터페이스에서 사용할 수 있습니다.

자세한 정보는 [로드 밸런서 문서](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc)를 참조하십시오.


## 2019년 5월 10일
{: #may-10-2019}


- **프로파일 이름이 변경되었습니다**. 인스턴스 프로파일 이름이 변경되었습니다. 인스턴스를 프로비저닝하는 명령이 새 프로파일 이름을 사용하도록 업데이트되어야 합니다. [여기](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles)에서 프로파일에 대해 자세히 알아보십시오.

- **예상 월별 유동 IP 가격을 사용자 인터페이스에서 볼 수 있습니다**. 유동 IP 주소를 예약할 때 이제 해당 유동 IP에 대한 예상 월별 가격을 표시하는 행을 볼 수 있습니다.

- **Hyper Protect 지원이 사용 가능합니다**.

- **이름 및 구역이 사용자 인터페이스에서 연관된 서브넷으로 이동하는 링크로 표시됩니다**. 이제 서브넷은 서브넷 ID가 아닌 이름별로 나열되며 이름은 연관된 서브넷의 서브넷 세부사항 페이지에 대한 링크로 작동합니다.

- **비용 요약 추정기를 사용자 인터페이스에서 사용할 수 있습니다**.

- **로드 밸런서 풀 및 리스너에 대한 폴링이 사용자 인터페이스에 반영됩니다**.

    * 이제 로드 밸런서 풀 페이지의 리소스 테이블 아래에서 마지막 업데이트를 표시하는 카운터를 볼 수 있습니다.
    * 기본 폴링 간격은 30초입니다.
    * 네 가지 다른 상태 `newProvision`, `active`, `failed` 및 `all other states`가 표시될 수 있습니다.
        * 새 프로비저닝: 간격은 15초입니다. 풀 및 리스너가 즉시 활성 상태로 이동합니다.
        * 활성: 리소스 목록 및 헤더가 60초마다 업데이트됩니다.
        * 실패: 폴링이 중지됩니다.
        * 기타 모든 상태: 폴링이 30초 간격으로 계속됩니다.
    * 리스너를 삭제하는 경우 `Updating (Actions Unavailable)`을 표시하도록 헤더 상태를 트리거할 수 있습니다.
    * 또한 업데이트가 발생할 때 헤더 정보가 업데이트된다는 점에 유의하십시오.
    * 리스너 폴링에도 이러한 동일한 변경사항이 적용됩니다.

- **멤버 폴링이 발생하는 동안 로드 밸런서 멤버 수가 사용자 인터페이스의 멤버 및 세부사항 페이지에 표시됩니다**.

    * 상태 섹션에서 "인스턴스 세부사항 페치 중"이 옆에 있는 동안 멤버 수가 표시됩니다.
    * 리소스 테이블의 인스턴스 열에서 "인스턴스 세부사항 페치 중"이 옆에 있는 동안 멤버 수가 표시됩니다.
    * 로드된 후 올바르지 않은 멤버가 있는 경우 노란색 주의 아이콘이 표시됩니다.

- **사용자 인터페이스의 VPC 세부사항 페이지에 연결된 서브넷이 모두 표시됩니다**.
