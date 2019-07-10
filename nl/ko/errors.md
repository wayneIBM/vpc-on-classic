---

copyright:

  years: 2018, 2019
lastupdated: "2019-06-11"

keywords: error, message, API, limitations, rias, support

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# IBM Cloud Virtual Private Cloud API 오류 메시지
{: #rias-error-messages}

{{site.data.keyword.cloud}} Virtual Private Cloud API로부터 오류 메시지를 수신하는 경우, 메시지 ID를 사용하여 문제를 해결하는 방법에 대한 자세한 정보를 찾을 수 있습니다.
{:shortdesc}

## account_type_invalid
**메시지**: Virtual Private Cloud를 프로비저닝하려면 종량과금제 계정이 있어야 합니다.

VPC를 프로비저닝하려면 사용자 계정이 종량과금제 플랜을 사용 중이어야 합니다. [계정 업그레이드](/docs/account?topic=account-accounts#upgrade-lite-account)에서 계정 업그레이드에 대한 세부사항을 찾을 수 있습니다.

## acl_in_use
**메시지**: 네트워크 ACL이 리소스에 연결되어 있어 삭제할 수 없습니다.

네트워크 ACL이 서브넷 또는 VPC에 연결되어 있어 삭제할 수 없습니다. 

서브넷이 네트워크 ACL을 사용 중인지 확인하려면 `GET /v1/subnets?version=2019-05-31&generation=1` API를 사용하십시오. 동등한 CLI 명령: `ibmcloud is subnets`. 서브넷에서 사용되는 네트워크 ACL을 업데이트하려면 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API 또는 `ibmcloud is subnet-update` CLI를 사용하십시오.

VPC가 네트워크 ACL을 사용 중인지 확인하려면 `GET /v1/vpcs?version=2019-05-31&generation=1` API를 사용하십시오. 동등한 CLI 명령: `ibmcloud is vpcs`. VPC에서 사용되는 기본 네트워크 ACL을 업데이트하거나 삭제할 수 없습니다. VPC가 삭제되면 기본 네트워크 ACL이 자동으로 삭제됩니다. 

## address_prefix_conflict
**메시지**: 이 CIDR의 주소 접두부가 사용 중입니다.

요청된 주소 접두부에 대한 CIDR이 기존 주소 접두부와 충돌합니다.

VPC에 대한 주소 접두부의 목록을 보려면 `GET /vpcs/{vpc-id}/address_prefixes` API를 사용하고 요청된 CIDR 주소가 다른 주소 접두부의 `cidr` 필드에서 사용 중이지 않은지 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**메시지**: 사용 중인 주소 접두부는 삭제할 수 없습니다.

하나 이상의 서브넷이 해당 주소 접두부를 사용 중입니다.  주소 접두부를 사용 중인 서브넷을 판별하려면 `GET /v1/subnets?version=2019-05-31&generation=1` API 명령을 사용하고 `ipv4_cidr_block` 필드를 확인하십시오. 동등한 CLI 명령은 `ibmcloud is subnets`이며 `Subnet CIDR` 열을 확인하십시오. 주소 접두부를 삭제하기 전에 먼저 주소 접두부를 사용하여 모든 서브넷을 삭제해야 합니다.

## backend_service_unavailable
**메시지**: 백엔드 서비스가 사용 불가능합니다.

VPC에서 사용되는 백엔드 클라우드 서비스가 응답하는 데 실패했습니다. VPC에서는 다음과 같은 여러 IBM Cloud 서비스를 사용합니다.

- [ID 및 자산 관리](/docs/iam?topic=iam-iamoverview)(IAM)
- [글로벌 카탈로그](https://{DomainName}/catalog)
- 리소스 제어기
- 리소스 관리자

IBM Cloud 서비스의 [상태](https://{DomainName}/status)를 확인하고 몇 분 후에 다시 시도할 수 있습니다. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## bad_field
**메시지**: 올바른 인스턴스 UUID를 제공해야 합니다.

요청에 제공된 값 중 하나가 올바르지 않습니다. 올바르지 않은 매개변수에 대한 단서는 리턴된 오류의 `target` 값을 참조하십시오. 일부 경우에는 요청의 UUID 또는 볼륨을 찾을 수 없습니다. 올바른 값을 제공하고 다시 시도하십시오.

추가 도움말은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## bad_request
**메시지**: 제공된 정보가 올바르지 않거나, 형식이 잘못되었거나, 필수 필드가 누락되었습니다.

요청이 예상한 상태가 아닙니다. 요청을 형식화하는 데 도움을 받으려면 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 사용하십시오.

스펙을 따르고 있지만 계속 오류가 발생하는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## classic_access_vpc_conflict_duplicate_res
**메시지**: 클래스 액세스를 사용하는 VPC는 지역별로 하나만 작성할 수 있습니다.

클래스 액세스를 사용하는 VPC는 지역별로 하나만 작성할 수 있습니다. 클래식 액세스를 사용하는 VPC를 나열하려면 `ibmcloud is vpcs --classic-access`를 실행하십시오. 클래식 액세스를 사용하는 다른 VPC를 작성하려면 먼저 클래식 액세스를 사용하는 기존 VPC를 삭제해야 합니다.

## classic_access_vpc_account_not_VRF_enabled
**메시지**: 클래식 액세스 VPC를 작성하려면 계정에 VRF가 사용되어야 합니다.

링크된 클래식 계정에는 VRF가 사용되지 않습니다. 클래식 액세스 VPC에서는 링크된 클래식 계정에 VRF가 사용되어야 합니다. 계정에 VRF를 사용하도록 하려면 [VRF on IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)를 참조하십시오.

## default_address_prefix_not_found
**메시지**: 기본 주소 접두부를 찾을 수 없습니다.

기본 주소 접두부를 찾을 수 없는 경우 이 오류 메시지가 표시될 수 있습니다.

## duplicate_error
**메시지**: 제공된 입력이 이미 있습니다.

지정된 리소스가 이미 있습니다. 작성하려는 리소스에 다른 이름을 사용하십시오. 예를 들어, 새 VPC를 작성할 때 `ibmcloud is vpcs`를 실행하여 기존 VPC를 나열할 수 있습니다. 충돌하지 않는 이름을 선택하십시오.

## floating_ip_in_use
**메시지**: 해당 유동 IP가 사용 중입니다.

이 유동 IP는 가상 서버 인터페이스 또는 공용 게이트웨이와 연관되어 있습니다. 

유동 IP가 사용되는 위치를 확인하려면 `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` API를 사용하고 "target" 값을 확인하십시오. 동등한 CLI 명령: `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_many
**메시지**: PVC에서는 구역당 하나의 공용 게이트웨이만 보유할 수 있습니다.

VPC에서는 구역당 하나의 공용 게이트웨이만 허용되지만, 이 공용 게이트웨이는 구역의 여러 서브넷에 연결될 수 있습니다. 구역의 공용 게이트웨이를 알아보려면 GET public_gateways API를 실행하여 "vpc" 및 "zone" 값을 보십시오. CLI를 사용하고 있는 경우에는 `ibmcloud is public-gateways` 명령을 실행하여 "VPC" 및 "Zone" 값을 볼 수 있습니다.

공용 게이트웨이의 ID를 사용하여 이를 서브넷에 연결하십시오. [API 예](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-)의 예를 참조하십시오. CLI를 사용하고 있는 경우에는 서브넷 업데이트 명령을 사용하여 이를 공용 게이트웨이에 연결할 수 있습니다(예: `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`).

## health_monitor_invalid_delay
**메시지**: 지정된 상태 모니터 지연 <health_monitor_delay>가 올바르지 않습니다.

`health monitor delay` 매개변수에 대해 2 - 60 사이의 값을 제공하십시오.

## health_monitor_invalid_max_retries
**메시지**: 상태 모니터 최대 재시도 수 <health_monitor_max_retries>가 올바르지 않습니다.

`health max retries`에 대해 1 - 10 사이의 값을 제공하십시오.

## health_monitor_invalid_timeout
**메시지**: 지정된 상태 모니터 제한시간 <health_monitor_timeout>이 올바르지 않습니다. 

`health timeout`에 대해 1 - 59 사이의 값을 제공하십시오. 선택한 값이 `health monitor delay` 값보다 작아야 합니다.

## health_monitor_invalid_url_path
**메시지**: 지정된 상태 URL 경로 <health_monitor_url>이 올바르지 않습니다.

올바른 상태 모니터 URL을 제공하십시오. URL에는 영숫자 문자, 하이픈 및 마침표가 포함될 수 있습니다. 공백 또는 백슬래시는 허용되지 않습니다. URL 경로의 최대 길이는 255자입니다.

## health_monitor_invalid_protocol
**메시지**: 지정된 상태 모니터 프로토콜 <health_monitor_protocol>이 올바르지 않습니다.

올바른 상태 모니터 프로토콜 값을 제공하십시오. 허용 가능한 값은 `http` 및 `tcp`입니다.

## health_monitor_missing_protocol
**메시지**: 상태 모니터 프로토콜이 누락되었습니다.

상태 모니터 프로토콜은 필수 필드입니다. 요청에 상태 모니터 프로토콜을 지정하십시오. 허용 가능한 값은 `http` 및 `tcp`입니다.

## health_monitor_missing_url_path
**메시지**: 상태 모니터 URL 경로가 누락되었습니다. 이는 HTTP 유형 상태 모니터에 필요합니다.

HTTP 프로토콜을 사용하는 상태 모니터의 경우 상태 모니터 URL 경로가 필수 필드입니다. 올바른 URL 경로를 제공하십시오.

## health_monitor_timeout_delay_conflict
**메시지**: 상태 모니터 제한시간 <health_monitor_timeout>은 지연 <health_monitor_delay> 미만이어야 합니다.

`health monitor delay` 매개변수의 값이 `health monitor timeout` 값보다 커야 합니다.

## http_request_size_exceeded
**메시지**: HTTP 요청이 너무 큽니다.

이 문제점은 요청에서 전송한 페이로드에 너무 많은 문자가 있는 경우 발생합니다. 더 작은 페이로드로 다시 시도하십시오. 예를 들면, 하나의 요청에서 모든 작업을 수행하려 하는 대신, 하나의 요청에서 최소한의 리소스를 작성한 후 몇 개의 후속 요청에서 해당 리소스에 증분식으로 상태를 추가해 보십시오.

추가 도움말은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## iam_failure
**메시지**: 없음

IAM 서비스에서 인증 또는 권한을 확인하는 중에 장애가 발생했습니다. IAM 서비스 중단은 일시적일 수 있습니다. 몇 분 후에 요청을 재시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ike_policies_quota_exceeded
**메시지**: 사용자 계정이 할당량에 도달했으므로 IKE 정책을 작성할 수 없습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

현재 IKE 정책을 보려면 `GET /ike_policies` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**메시지**: 이름 `<ike_policy_name>`이 이미 IKE 정책 `<ike_policy_id>`에서 사용 중입니다.

다른 IKE 정책 이름을 제공하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ike_policy_in_use
**메시지**: IKE 정책 `<ike_policy_id>`가 하나 이상의 연결에서 사용 중입니다.

하나 이상의 연결에서 사용 중인 IKE 정책은 삭제할 수 없습니다.

IKE 정책을 사용 중인 연결을 보려면 `GET /ike_policies/<ike_policy_id>/connections` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**메시지**: 이름 `<ike_policy_name>`은 올바른 IKE 정책 이름이 아닙니다.

올바른 IKE 정책 이름은 문자로 시작하며 그 뒤로 문자, 숫자, 밑줄 또는 하이픈이 따릅니다.

## ike_policy_not_created
**메시지**: IKE 정책 `<ike_policy_id>`를 작성하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ike_policy_not_deleted
**메시지**: IKE 정책 `<ike_policy_id>`를 삭제하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ike_policy_not_found
**메시지**: IKE 정책 `<ike_policy_id>`를 찾지 못했습니다.

존재하지 않는 IKE 정책을 참조했습니다. 요청을 검토하여 적절한 IKE 정책 ID를 지정했는지 확인하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ike_policy_not_updated
**메시지**: IKE 정책 `<ike_policy_id>`를 업데이트하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## instance_delete_conflict
**메시지**: 현재 상태에서 인스턴스를 삭제할 수 없습니다.

`deleting`, `pending`, `starting`, `stopping` 또는 `restarting` 상태인 경우 인스턴스를 삭제할 수 없습니다. 인스턴스가 `running` 또는 `stopped` 상태여야 합니다. 상태가 변경되지 않으면 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## instance_invalid_hostname
**메시지**: 호스트 이름이 올바르지 않으므로 인스턴스를 작성할 수 없습니다.

인스턴스 호스트 이름은 다음 요구사항을 충족해야 합니다.
* 호스트 이름에는 영숫자 및 대시만 포함되어야 합니다.
* 대문자는 허용되지 않습니다.
* 선행 및 후행 대시는 허용되지 않습니다.
* 선행 숫자는 허용되지 않습니다.
* 연속 대시는 허용되지 않습니다.
* Windows의 경우 최대 길이는 15자입니다.
* 기타 운영 체제의 경우 최대 길이는 40자입니다.

## instance_invalid_port_speed
**메시지**: 지정된 네트워크 인터페이스 포트 속도가 올바르지 않으므로 인스턴스를 작성할 수 없습니다.

네트워크 인터페이스 포트 속도는 `100`Mbps 또는 `1000`Mbps여야 합니다.

## instance_name_exists
**메시지**: 동일한 인스턴스 이름이 이미 있습니다.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## instance_sec_volume_over_quota
**메시지**: 볼륨 연결의 수가 할당량을 초과하므로 인스턴스를 작성할 수 없습니다.

리소스별 VPC 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window} 페이지에 지정되어 있습니다. 

## instance_too_many_keys
**메시지**: 요청에 다중 키가 포함되어 있으므로 Windows 인스턴스를 작성할 수 없습니다.

Windows 인스턴스는 하나의 키만 지원합니다.

## insufficient_space_for_subnet
**메시지**: 주소 접두부에 서브넷을 위한 공간이 충분하지 않습니다.

요청된 수의 주소를 할당할 수 없으므로 서브넷을 작성할 수 없습니다. 더 작은 주소 수 또는 더 큰 CIDR을 사용하여 다시 시도하십시오.

## internal_error
**메시지**: 내부 오류가 발생했습니다.

예상치 못한 오류가 발생했습니다. 이 문제점은 일시적일 수 있습니다. 몇 분 후에 요청을 다시 시도하십시오. 이 오류가 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## internal_server_error
**메시지**: 없음

예상치 못한 오류가 발생했습니다. 이 문제점은 일시적일 수 있습니다. 몇 분 후에 요청을 다시 시도하십시오. 이 오류가 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## internal_solution
**메시지**: 관리자에게 문의하십시오.

예상치 못한 오류가 발생했습니다. 이 문제점은 일시적일 수 있습니다. 몇 분 후에 요청을 다시 시도하십시오. 이 오류가 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## invalid_generation_parameter
**메시지**: generation 조회 매개변수가 1로 설정되어야 합니다.

5/31/2019 이후 버전의 경우 VPC on Classic API 요청을 허용하려면 `generation` 조회 매개변수가 1로 설정되어야 합니다.

## invalid_id_format
**메시지**: ID 형식이 잘못되었습니다. 형식이 올바른지 확인하십시오.

제공한 ID에 잘못된 형식의 데이터가 포함되어 있지 않은지 확인하십시오.

페이지 매김 요청을 전송할 때 잘못된 형식의 시작 조회를 제공하는 경우 이 오류 메시지가 수신될 수 있습니다. 예를 들어, `GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1`에는 두 개의 `?`가 포함되어 있습니다. 조회를 수정하고 다시 시도하십시오.

## invalid_route
**메시지**: 요청된 라우트가 존재하지 않습니다.

사용자가 제공한 API URL에 대해 요청된 라우트가 존재하지 않습니다. API 엔드포인트를 호출하기 위해 지정한 URL이 올바른지 확인하십시오.

## invalid_request_field
**메시지**: 요청에 제공된 필드가 올바르지 않습니다.

예를 들어, 서브넷에서 사용되는 네트워크 ACL을 업데이트하려면 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` API를 사용하십시오.
"networkacl"이 올바른 필드가 아니므로 `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1 -d '{ "networkacl":{ "id": “{network_acl_id}” } }’` 요청이 올바르지 않습니다.

허용 가능한 값은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## invalid_state
**메시지**: 현재 리소스 상태에서 지원되지 않는 리소스에 대한 조치가 요청되었습니다.

리소스가 인스턴스인 경우 다음과 같습니다.

1. 인스턴스에 대한 다시 부팅 오퍼레이션이 이미 진행 중일 수 있습니다. 인스턴스의 상태에 따라 [허용되는 조치](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance)를 참조하십시오.

2. 인스턴스 상태가 `pending`인 동안에는 다음 조치를 수행할 수 없습니다.
  * 인스턴스 삭제
  * 볼륨을 인스턴스에 연결
  * 볼륨을 인스턴스에서 분리
  * 인스턴스 업데이트

나중에 다시 조치를 시도하십시오. 리소스 상태가 변경되지 않는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

리소스가 이미지인 경우 다음과 같습니다.

인스턴스 상태가 `deleting`인 동안에는 다음 조치를 수행할 수 없습니다.
  * 이미지 패치
  * 이미지 삭제

요청을 다시 작성하지 마십시오. 이미지 삭제는 자체 시간 내에 완료됩니다. 삭제가 완료되면 해당 이미지에 대한 모든 요청이 대신 `not_found` 오류와 함께 실패합니다.

## invalid_version
**메시지**: `version` 매개변수가 올바르지 않습니다. 이는 `YYYY-MM-DD` 양식으로 되어 있어야 합니다.

버전은 _YYYY-MM-DD_ 형식을 준수해야 합니다. 1월 1일과 같은 한자리 숫자의 월 또는 날짜의 경우 버전은 `2019-01-01`과 같아야 합니다. version 매개변수는 URL에 있어야 합니다. version 매개변수에 지정된 날짜는 2019-01-01 이후이지만 현재 날짜 이전이어야 합니다. 

## invalid_version_range
**메시지**: `version` 값은 미래 날짜 또는 `2019-01-01` 이전 날짜로 설정될 수 없습니다.

version 매개변수에 지정된 날짜는 2019-01-01 이후이지만 현재 날짜 이전이어야 합니다. 

## invalid_zone
**메시지**: 요청하는 리소스가 동일한 구역에 있는지 확인하십시오.

zone-1의 공용 게이트웨이를 zone-2의 서브넷에 연결하려는 중에 이 메시지가 표시될 수 있습니다.

## ipsec_policies_quota_exceeded
**메시지**: 사용자 계정이 할당량에 도달했으므로 IPsec 정책을 작성할 수 없습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

현재 IPsec 정책을 보려면 `GET /ipsec_policies` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**메시지**: 이름 `<ipsec_policy_name>`이 이미 IPsec 정책 `<ipsec_policy_id>`에서 사용 중입니다.

다른 IPsec 정책 이름을 제공하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ipsec_policy_in_use
**메시지**: IPsec 정책 `<ipsec_policy_id>`가 하나 이상의 연결에서 사용 중입니다.

하나 이상의 연결에서 사용 중인 IPsec 정책은 삭제할 수 없습니다.

IPsec 정책을 사용 중인 연결을 보려면 `GET /ipsec_policies/<ipsec_policy_id>/connections` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**메시지**: 이름 `<ipsec_policy_name>`은 올바른 IPsec 정책 이름이 아닙니다.

올바른 IPsec 정책 이름은 문자로 시작하며 그 뒤로 문자, 숫자, 밑줄 또는 하이픈이 따릅니다.

## ipsec_policy_not_created
**메시지**: IPsec 정책 `<ipsec_policy_id>`를 작성하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ipsec_policy_not_deleted
**메시지**: IPsec 정책 `<ipsec_policy_id>`를 삭제하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ipsec_policy_not_found
**메시지**: IPsec 정책 `<ipsec_policy_id>`를 찾지 못했습니다.

존재하지 않는 IPsec 정책을 참조했습니다. 요청을 검토하고 올바른 IPsec 정책 ID를 지정하십시오. 정책이 있는지 확신할 수 없는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## ipsec_policy_not_updated
**메시지**: IPsec 정책 `<ipsec_policy_id>`를 업데이트하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## key_content_exists
**메시지**: 동일한 키 컨텐츠가 이미 있습니다.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## key_name_exists
**메시지**: 동일한 키 이름이 이미 있습니다.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## key_invalid_name
**메시지**: 이름이 올바르지 않으므로 키를 작성할 수 없습니다.

키 이름은 다음 요구사항을 충족해야 합니다.
* 영문자([A-Za-z])로 시작해야 합니다.
* 영숫자 문자, 대시 또는 밑줄([-A-Za-z0-9_])만 포함해야 합니다.
* 길이는 65자를 초과하지 않아야 합니다.

## key_invalid_type
**메시지**: 유형이 올바르지 않으므로 키를 작성할 수 없습니다.

지원되는 유일한 키 유형은 `rsa`입니다.

## key_parse_failure
**메시지**: 공개 키 값이 올바르지 않으므로 키를 작성할 수 없습니다.

요청에 제공된 공개 키가 올바르지 않습니다. 값을 확인하거나 공개 키를 재생성한 후 다시 시도하십시오.

Linux 운영 체제에서는 `ssh-keygen -l -v -f <key_file>` 명령을 실행하여 공개 키를 확인할 수 있습니다.

## listener_certificate_not_found
**메시지**: CRN이 `<listener_certificate_crn>`인 인증서 인스턴스를 찾을 수 없거나 인증서 인스턴스에 액세스할 권한이 없습니다.

기존 인증서 인스턴스 CRN을 제공하거나, 제공된 인증서 인스턴스에 대한 액세스 권한을 부여하도록 계정 관리자에게 요청하십시오.

## listener_duplicate_port
**메시지**: 포트 `<listener_port>`를 다른 리스너 인스턴스에서 사용 중입니다. 다른 포트를 선택하십시오.

다른 포트를 선택하십시오.

## listener_invalid_certificate
**메시지**: HTTPS 리스너에 대한 인증서 인스턴스의 CRN이 올바르지 않습니다.

올바른 인증서 인스턴스 CRN을 제공하십시오.

## listener_invalid_connection_limit
**메시지**: 리스너 연결 한계 `<listener_connection_limit>`가 올바르지 않습니다.

올바른 연결 한계를 제공하십시오. 연결 한계는 1 - 15000 사이의 정수여야 합니다. 

## listener_invalid_port
**메시지**: 리스너 포트 `<listener_port>`가 올바르지 않습니다.

다른 포트를 선택하십시오.

## listener_invalid_protocol
**메시지**: 리스너 프로토콜 `<listener_protocol>`이 올바르지 않습니다.

올바른 리스너 프로토콜을 제공하십시오. 리스너 프로토콜에 대해 허용 가능한 값은 `http`, `https` 및 `tcp`입니다.

## listener_missing_certificate_crn
**메시지**: `https` 리스너에 대한 인증서 인스턴스의 CRN이 누락되었습니다.

인증서 인스턴스의 CRN은 필수 필드입니다. 

## listener_missing_port
**메시지**: 리스너 포트가 누락되었습니다.

리스너 포트는 필수 필드입니다.

## listener_missing_protocol
**메시지**: 리스너 프로토콜이 누락되었습니다.

리스너 프로토콜은 필수 필드입니다. 요청에 리스너 프로토콜을 제공하십시오. 허용 가능한 값은 `http`, `https` 및 `tcp`입니다.

## listener_not_found
**메시지**: ID가 `<listener_id>`인 리스너를 찾을 수 없습니다.

기존 리스너 ID를 제공하십시오.

## listener_over_quota
**메시지**: 리스너를 작성할 수 없습니다. 로드 밸런서 리소스의 리스너 할당량이 최대 한계에 도달했습니다.

리소스별 할당량은 [VPC의 할당량 및 한계](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}에 지정되어 있습니다.

## listener_pool_protocols_conflict
**메시지**: 리스너 프로토콜(`<listener_protocol>`) 및 풀 프로토콜(`<pool_protocol>`)이 충돌합니다.

`https` 또는 `http` 프로토콜을 사용하는 리스너는 `http` 프로토콜을 사용하는 풀하고만 연관될 수 있습니다. 마찬가지로, `tcp` 리스너는 `tcp` 풀하고만 연관될 수 있습니다.

## listener_reserved_port_conflict
**메시지**: 리스너 포트 `<listener_port>`가 예약된 포트 중 하나입니다. 56500 - 56520 포트 범위는 관리 용도로 예약되어 있습니다. 다른 포트를 선택하십시오.

다른 포트를 선택하십시오.

## load_balancer_delete_conflict
**메시지**: 상태가 다음 중 하나이므로 ID가 <load_balancer_id>인 로드 밸런서를 삭제할 수 없습니다.  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

ACTIVE 상태일 때 로드 밸런서를 삭제하십시오.

## load_balancer_duplicate_name
**메시지**: 이름 `<load_balancer_name>`을 다른 로드 밸런서 인스턴스에서 사용 중입니다. 다른 이름을 선택하십시오.

로드 밸런서 인스턴스에 대해 다른 이름을 제공하십시오. 로드 밸런서 이름은 VPC 및 계정 내에서 고유해야 합니다.

VPC용 Load Balancer가 클라우드 로드 밸런서와 충돌하지 않습니다. 이러한 서비스에 대한 DNS 도메인 이름이 서로 다르고 VPC용 Load Balancer의 이름이 DNS 이름에 포함되지 않기 때문입니다.
{: note}

## load_balancer_insufficient_ips
**메시지**: ID가 <subnet_id>인 서브넷에 사용 가능한 IPv4 주소가 충분하지 않습니다.

요청에서 로드 밸런서를 작성할 사용 가능한 IPv4 주소가 있는 올바른 서브넷을 제공하십시오.

## load_balancer_invalid_description
**메시지**: 로드 밸런서 설명 `<load_balancer_description>`이 올바르지 않습니다.

로드 밸런서 설명은 255자를 초과할 수 없습니다.

## load_balancer_invalid_is_public
**메시지**: is_public `<load_balancer_is_public>`의 값이 올바르지 않습니다.

로드 밸런서 매개변수 `is_public`의 값은 _true_ 또는 _false_여야 합니다.

## load_balancer_invalid_name
**메시지**: 이름 `<load_balancer_name>`이 올바르지 않습니다.

이름은 비어 있을 수 없습니다. 올바른 로드 밸런서 이름은 문자로 시작하며 그 뒤로 문자, 숫자 또는 밑줄이 따릅니다. 이름의 길이는 40자를 초과할 수 없습니다.

## load_balancer_invalid_subnet
**메시지**: ID가 <subnet_id>인 서브넷이 올바르지 않습니다. 'available' 상태의 기존 서브넷을 사용해야 합니다.

로드 밸런서 작성 요청을 입력할 때 `available` 상태의 올바른 서브넷을 제공하십시오.

## load_balancer_missing_is_public
**메시지**: 'is_public' 필드가 누락되었습니다.

`is_public` 필드는 필수입니다. `is_public` 필드의 값을 제공하십시오.

## load_balancer_missing_name
**메시지**: 로드 밸런서 이름이 누락되었습니다.

로드 밸런서 `name`은 필수 필드입니다. 로드 밸런서의 이름을 제공하십시오.

## load_balancer_missing_subnets
**메시지**: 로드 밸런서 서브넷이 누락되었습니다.

`subnets` 필드는 필수입니다. 로드 밸런서 작성 요청에서 로드 밸런서를 작성할 서브넷에 대한 정보를 제공하십시오.

## load_balancer_missing_subnet_id
**메시지**: 서브넷 ID가 누락되었습니다.

**서브넷 ID**는 필수 필드입니다. 명령을 사용하여 서브넷 ID를 제공하십시오.

## load_balancer_not_found
**메시지**: ID가 `<load_balancer_id>`인 로드 밸런서를 찾을 수 없습니다.

기존 로드 밸런서의 ID를 제공하십시오.

## load_balancer_over_quota
**메시지**: 로드 밸런서를 작성할 수 없습니다. 계정의 로드 밸런서 인스턴스 할당량이 최대 한계에 도달했습니다.

기존 로드 밸런서를 삭제하거나 지원에 문의하여 계정의 로드 밸런서 할당량을 늘리십시오.

리소스별 할당량은 [VPC의 할당량 및 한계](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}에 지정되어 있습니다.

## load_balancer_subnet_not_found
**메시지**: ID가 <subnet_id>인 서브넷을 찾을 수 없습니다.

요청에서 로드 밸런서를 작성할 기존 서브넷의 ID를 제공해야 합니다.

## load_balancer_unchanged_update
**메시지**: ID가 `<load_balancer_id>`인 로드 밸런서에 적용할 업데이트가 없습니다.

사용자가 지정한 ID로 로드 밸런서 인스턴스를 업데이트하기 위한 정보가 제공되지 않았습니다.

## load_balancer_update_conflict
**메시지**: 상태가 다음 중 하나이므로 ID가 <load_balancer_id>인 로드 밸런서를 업데이트할 수 없습니다.
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

ACTIVE 상태일 때 로드 밸런서를 업데이트하십시오.

## member_duplicate_address_and_port
**메시지**: 주소가 <member_address>이고 포트가 <member_port>인 멤버가 풀에 이미 있습니다.

요청에 고유 멤버 주소 및 포트 조합을 제공하십시오.

## member_invalid_address
**메시지**: 지정된 멤버 IP 주소 <member_ip>가 올바르지 않습니다.

요청에서 멤버에 대해 올바른 IPv4 CIDR 주소를 제공하십시오.

## member_invalid_port
**메시지**: 지정된 멤버 포트 `<member_port>`가 올바르지 않습니다.

올바른 포트 번호를 제공하십시오. 올바른 포트 번호는 1 - 65535 범위에 속합니다.

## member_invalid_weight
**메시지**: 지정된 멤버 가중치 `<member_weight>`가 올바르지 않습니다.

올바른 멤버 가중치를 제공하십시오. 값은 0 - 100 사이의 정수여야 합니다.

## member_ip_not_found
**메시지**: IP 주소가 <member_IP>인 멤버가 로드 밸런서의 지역 및 VPC에 있는 서브넷에 속해 있지 않습니다.

요청에 로드 밸런서와 동일한 지역 및 VPC의 서브넷에 속하는 멤버의 주소를 제공하십시오.

## member_missing_address
**메시지**: 멤버 주소가 누락되었습니다.

멤버 주소는 필수 필드입니다. 올바른 멤버 주소 값을 제공하십시오.

## member_missing_protocol_port
**메시지**: 멤버 포트가 누락되었습니다.

멤버 포트는 필수 필드입니다. 올바른 멤버 포트 값을 제공하십시오.

## member_not_found
**메시지**: ID가 <member_id>인 멤버를 찾을 수 없습니다.

기존 멤버 ID를 제공하십시오.

## member_over_quota
**메시지**: 멤버를 작성할 수 없습니다. 풀의 멤버 인스턴스 할당량이 최대 한계에 도달했습니다.

리소스별 할당량은 [VPC의 할당량 및 한계](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}에 지정되어 있습니다.

## missing_generation_parameter
**메시지**: generation 조회 매개변수는 필수입니다.

5/31/2019 이후 버전의 경우 VPC on Classic API 요청에 `generation` 조회 매개변수가 필요합니다.

## missing_ims_account_id
**메시지**: 없음

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## missing_version
**메시지**: `version` 매개변수는 필수이며, `YYYY-MM-DD` 양식으로 되어 있어야 합니다.

version 매개변수는 모든 API 요청에 대해 필수입니다. 버전은 _YYYY-MM-DD_ 형식을 준수해야 합니다. 1월 1일과 같은 한자리 숫자의 월 또는 날짜의 경우 버전은 `2019-01-01`과 같아야 합니다. version 매개변수에 지정된 날짜는 2019-01-01 이후이지만 현재 날짜 이전이어야 합니다. version 매개변수를 제공하는 방법은 이러한 [API 예제](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)를 참조하십시오.

## network_conflict
**메시지**: CIDR이 VPC의 기존 주소 접두부와 충돌합니다.

동일한 IP 공간의 기존 네트워크 CIDR과 충돌하는 네트워크 CIDR을 제공한 경우 이 메시지가 표시될 수 있습니다. 

VPC에 대한 주소 접두부의 기존 CIDR을 찾으려면 API 명령 `GET /v1/vpcs/<vpc-id>/address_prefixes`를 실행하고 응답의 `cidr` 값을 확인하십시오. `cidr` 값을 확인하여 제공한 CIDR을 다른 주소 접두부에서 사용 중이지 않은지 확인하십시오.

CLI를 사용 중인 경우 `ibmcloud is vpc-addrs <vpc-id>` 명령을 실행하고 `CIDR Block` 값이 충돌하는지 확인하십시오.

서브넷의 기존 CIDR을 찾으려면 API 명령 `GET /v1/subnets`를 실행하여 VPC의 모든 서브넷을 나열하십시오. 그런 다음 VPC의 각 서브넷에 대해 API 명령 `GET /v1/subnets/<subnet-id>`를 실행하고 응답에서 `ipv4_cidr_block` 값을 확인하십시오. `ipv4_cidr_block` 값을 확인하여 제공한 CIDR을 다른 주소 접두부에서 사용 중이지 않은지 확인하십시오.

CLI를 사용 중인 경우 `ibmcloud is subnets` 명령을 실행하여 VPC의 모든 서브넷을 나열하십시오. 그런 다음 VPC의 각 서브넷에 대해 `ibmcloud is subnet <subnet-id>` 명령을 실행하고 CLI 출력에서 `IPv4 CIDR` 값이 충돌하는지 확인하십시오.

## not_authorized
**메시지**: 요청이 권한 부여되지 않았습니다.

IAM 토큰이 누락되었거나 만료된 경우 이 오류가 표시될 수 있습니다. 토큰을 생성하는 방법에 대한 지시사항은 [REST API를 사용하여 VPC 작성](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)을 참조하십시오. 토큰이 만료되지 않은 경우에는 사용 중인 계정에 이 기능을 사용할 수 있는 권한이 부여되었고 [올바른 권한](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)이 있는지 확인하십시오.

올바른 권한이 있지만 이 오류가 계속 발생하는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## not_found
**메시지**: 요청하는 리소스가 존재하는지 확인하십시오.

존재하지 않거나 액세스 권한이 없는 리소스를 참조했습니다. 요청을 검토하여 적절한 ID 및 참조를 지정했는지 확인하십시오.

추가 도움말은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## not_implemented
**메시지**: 없음

해당 메소드가 구현되지 않았습니다. 요청을 검사하여 [올바른 API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 호출 중인지 확인하십시오. 이 API가 구현될 것으로 예상하는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## not_in_address_prefix
**메시지**: 제공된 CIDR이 제공된 구역 내의 어느 주소 접두부와도 맞지 않습니다.

이 오류는 사용자가 해당 구역의 어느 주소 접두부에도 속하지 않는 CIDR로 서브넷을 작성하려 하는 경우 발생합니다.

`GET /vpcs/{vpc_id}/address_prefixes`를 실행하여 VPC의 주소 접두부 목록을 얻으십시오. CLI를 사용 중인 경우 `ibmcloud is vpc-address-prefixes`를 실행하여 VPC의 모든 주소 접두부를 나열할 수 있습니다. 응답의 `cidr` 및 `zone` 값을 보고 서브넷의 `cidr`이 서브넷을 작성하려는 구역의 주소 접두부의 `cidr`인지 확인하십시오. 

## over_quota
**메시지**: 이 요청을 수행하면 특정 리소스 유형에 대한 할당량이 초과됩니다.

리소스별 할당량은 [VPC의 할당량 및 한계](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}에 지정되어 있습니다.

## password_not_ready
**메시지**: 없음

비밀번호를 검색하려면 인스턴스가 실행 중이어야 합니다. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## pool_delete_conflict
**메시지**: 풀이 하나 이상의 리스너와 여전히 연관되어 있어 삭제할 수 없습니다.

풀이 모든 리스너에서 연관 해제되었는지 확인하고 다시 시도하십시오.

## pool_duplicate_name
**메시지**: 풀 이름 `<pool_name>`을 이미 다른 풀에서 사용하고 있습니다. 다른 이름을 선택하십시오.

다른 풀 이름을 선택하십시오.

## pool_invalid_name
**Message**: 이름 <pool_name>이 올바르지 않습니다.

올바른 풀 이름을 제공하십시오. 올바른 로드 밸런서 이름은 문자로 시작하며 그 뒤로 문자, 숫자, 하이픈이 따릅니다. 이름의 길이는 40자를 초과할 수 없습니다.

## pool_invalid_session_persistence
**메시지**: 풀 세션 지속성 값이 올바르지 않습니다.

올바른 세션 지속성 값을 제공하십시오. 허용 가능한 값은 `SOURCE_IP`입니다.

## pool_missing_algorithm
**메시지**: 로드 밸런싱 알고리즘이 누락되었습니다.

로드 밸런싱 알고리즘은 필수 필드입니다. 요청에 로드 밸런싱 알고리즘을 제공하십시오. 허용 가능한 값은 `round_robin`, `weighted_round_robin` 및 `least_connections`입니다.

## pool_missing_health_monitor
**메시지**: 풀 상태 모니터가 누락되었습니다.

풀 상태 모니터는 필수 필드입니다.

## pool_missing_members
**메시지**: 풀 멤버가 누락되었습니다.

풀 멤버를 제공하십시오.

## pool_missing_name
**메시지**: 풀 이름이 누락되었습니다.

풀 이름은 필수 필드입니다.

## pool_missing_protocol
**메시지**: 풀 프로토콜이 누락되었습니다.

풀 프로토콜은 필수 필드입니다. 요청에 풀 프로토콜을 제공하십시오. 허용 가능한 값은 `http` 및 `tcp`입니다.

## pool_not_found
**메시지**: ID가 `<pool_id>`인 풀을 찾을 수 없습니다.

기존 풀 ID를 제공하십시오.

## pool_over_quota
**메시지**: 풀을 작성할 수 없습니다. 로드 밸런서 리소스의 풀 할당량이 최대 한계에 도달했습니다.

리소스별 할당량은 [VPC의 할당량 및 한계](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}에 지정되어 있습니다.

## public_gateway_in_use
**메시지**: 사용 중인 공용 게이트웨이는 삭제할 수 없습니다.

현재 해당 공용 게이트웨이가 하나 이상의 서브넷에 연결되어 있습니다. 공용 게이트웨이를 삭제하려면 먼저 이를 모든 서브넷으로부터 분리해야 합니다.

공용 게이트웨이를 사용 중인 서브넷을 보려면 `GET /v1/subnets?version=2019-05-31&generation=1` API를 사용하십시오. 동등한 CLI 명령: `ibmcloud is subnets`.  공용 게이트웨이를 서브넷에서 분리하려면 API 명령 `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` 또는 CLI 명령 `ibmcloud is subnet-public-gateway-detach`를 사용하십시오.

## rate_limit_exceeded
**메시지**: 짧은 시간 내에 요청이 너무 많습니다.

이 오류 메시지는 지정된 시간 간격 내에 너무 많은 요청을 수신하는 경우 리턴됩니다. 잠시 기다린 후 다시 시도하십시오.

## security_group_active_transactions
**메시지**: 인터페이스는 인스턴스가 활성 상태로 표시될 때까지 연결하거나 분리할 수 없습니다.

인스턴스의 네트워크 인터페이스를 보안 그룹에 연결하려면 해당 인스턴스가 활성 상태여야 합니다. 인스턴스의 상태를 확인하려면 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 또는 `ibmcloud is instance`를 사용하십시오. 상태가 `running`이 되면 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## security_group_already_attached
**메시지**: 인터페이스가 이미 보안 그룹에 연결되어 있습니다.

인터페이스가 이미 보안 그룹에 연결되어 있습니다. 연결된 인터페이스를 보려면 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group-network-interfaces`를 사용하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.


## security_group_exists
**메시지**: 해당 보안 그룹이 이미 있습니다.

보안 그룹 이름은 VPC 내에서 고유해야 합니다. 해당 이름의 보안 그룹이 대상 VPC에 이미 있습니다. 현재 보안 그룹을 보려면 `GET /v1/security_groups?version=2019-05-31&generation=1` 또는 `ibmcloud is security-groups`를 사용하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic) 또는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.


## security_group_interfaces_attached
**메시지**: 인터페이스가 연결되어 있으면 보안 그룹을 삭제할 수 없습니다.


보안 그룹을 삭제하기 전에 모든 인터페이스를 분리하십시오. 인터페이스를 분리하려면 `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group-network-interface-remove`를 사용하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic) 또는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.


## security_group_interfaces_per_sg_exceeded
**메시지**: 보안 그룹당 인터페이스 수에 대한 한계를 초과했습니다.

다른 인터페이스를 연결하면 보안 그룹당 인터페이스 수에 대한 한계를 초과하게 됩니다. 리소스별 할당량은 [VPC의 할당량 및 한계](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}에 지정되어 있습니다. 자신의 전략을 평가하고 유사한 규칙을 사용하는 다른 보안 그룹을 작성하는 것을 고려하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic) 또는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.


## security_group_last_security_group_is_default
**메시지**: 기본 보안 그룹이 연결된 유일한 보안 그룹인 경우에는 이를 제거할 수 없습니다.

네트워크 인터페이스는 하나 이상의 보안 그룹에 연결되어야 합니다.
이는 보안 그룹이 지정되지 않은 경우 VPC의 기본 보안 그룹에 연결됩니다.
다른 보안 그룹에 인터페이스를 연결한 후 이를 기본 보안 그룹에서 분리하십시오.
기본 보안 그룹에 대한 정보는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## security_group_limit_exceeded
**메시지**: 보안 그룹 한계를 초과했습니다.

새 보안 그룹을 작성하려 시도했으나 사용자가 현재 계정 할당량에 도달한 상태입니다. 리소스별 할당량은 [VPC의 할당량 및 한계](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}에 지정되어 있습니다. 인스턴스를 보안 그룹에 지정하는 데 대한 자신의 전략을 평가하십시오. 보통은 여러 인스턴스를 동일한 보안 그룹에 지정하여 전체 보안 그룹의 수를 줄일 수 있습니다. 이 전략을 사용하면 계정 할당량 밑으로 보안 그룹 수를 줄일 수 있습니다. 대부분 큰 조직에 해당되지만, 드물게 할당량을 확장해야 하는 경우가 있습니다. 이러한 경우에는 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하여 이것이 가능한지 확인하십시오.

## security_group_network_interface_not_active
**메시지**: 인터페이스가 활성 상태가 아니므로 연결할 수 없습니다.

보안 그룹에 연결하려면 네트워크 인터페이스가 활성 상태여야 합니다. 인터페이스의 상태를 보려면 `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` 또는 `ibmcloud is instance-network-interface`를 사용하십시오. 상태가 'available'이 되면 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## security_group_not_attached
**메시지**: 인터페이스가 연결되지 않았습니다.

인터페이스가 보안 그룹에 연결되지 않았습니다. 연결된 인터페이스를 보려면 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group-network-interfaces`를 사용하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic) 또는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.


## security_group_not_in_vpc
**메시지**: 인터페이스와 보안 그룹이 서로 다른 VPC에 있습니다.

네트워크 인터페이스를 보안 그룹에 연결하려면 인스턴스가 보안 그룹과 동일한 VPC에 있어야 합니다. 보안 그룹 세부사항 및 VPC를 보려면 `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group`을 사용하십시오. 인스턴스 세부사항 및 VPC를 보려면 `GET /v1/instances/{id}?version=2019-05-31&generation=1` 또는 `ibmcloud is instance`를 사용하십시오.

## security_group_order_bindings
**메시지**: 보류 중인 인터페이스 주문이 있으면 보안 그룹을 삭제할 수 없습니다.

네트워크 인터페이스에 지정된 보안 그룹을 사용하여 인스턴스가 작성된 경우 해당 보안 그룹을 삭제하려면 해당 인스턴스 및 네트워크 인터페이스가 활성 상태여야 합니다. 연결된 네트워크 인터페이스 및 해당 상태를 보려면 `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group-network-interfaces`를 사용하십시오. 인터페이스의 상태가 'available'이 되면 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## security_group_port_max_less_than_port_min
**메시지**: TCP/UDP 최대 포트는 최소 포트보다 작을 수 없습니다.

최대 포트 값은 최소 포트 값보다 작을 수 없습니다. 최소 포트 값보다 큰 최대 포트 값을 지정하십시오.

## security_group_port_range_both_or_neither
**메시지**: 포트 범위를 설정 취소하거나, 'tcp' 및 'udp'에 대해 최소 및 최대 포트를 모두 설정해야 합니다.

'tcp' 또는 'udp' 프로토콜을 사용하는 규칙은 설정되지 않은 포트 범위(규칙을 모든 트래픽에 적용하기 위해)를 갖거나 포트 범위를 지정할 수 있습니다. 트래픽을 하나의 포트로 제한하려는 경우에는 최소 및 최대 포트를 모두 해당 포트 값으로 설정하십시오. 예를 들어, 다음 명령은 포트 22에서 SSH를 허용하는 규칙을 작성합니다. `ibmcloud is security-group-rule-add <id> inbound tcp --port-min 22 --port-max 22`

보안 그룹 구성에 대한 자세한 정보는 [API 문서](https://{DomainName}/apidocs/vpc-on-classic)를 참조하십시오.


## security_group_port_range_invalid_protocol
**메시지**: 포트 범위가 'icmp' 프로토콜을 사용하여 지정되었습니다. 포트 범위는 'tcp' 또는 'udp' 프로토콜에만 유효합니다.

'icmp' 프로토콜을 사용하는 규칙은 유형 및 코드를 갖습니다. 'tcp' 또는 'udp' 프로토콜을 사용하는 규칙은 포트 범위를 갖습니다. 보안 그룹 구성에 대한 자세한 정보는 [API 문서](https://{DomainName}/apidocs/vpc-on-classic)를 참조하십시오.

## security_group_remote_group_not_in_vpc
**메시지**: 원격 그룹이 이 보안 그룹과 동일한 VPC에 있지 않습니다.

보안 그룹 규칙은 원격 보안 그룹을 참조하여 두 그룹의 연결된 인터페이스 간에 트래픽을 허용할 수 있습니다. 원격 보안 그룹은 동일한 VPC에 있어야 합니다. 현재 보안 그룹 및 해당 VPC를 보려면 `GET /v1/security_groups?2019-01-01` 또는 `ibmcloud is security-groups`를 사용하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.


## security_group_remoting_rules
**메시지**: 원격 그룹이 연결되어 있으면 보안 그룹을 삭제할 수 없습니다.

보안 그룹이 원격 보안 그룹을 참조하는 하나 이상의 규칙을 포함하고 있습니다. 규칙을 보려면 `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` 또는 `ibmcloud is security-group-rules`를 사용하십시오. 'remote' 필드는 임의의 원격 보안 그룹을 지정합니다. 원격 규칙을 삭제하거나 편집한 후 다시 시도하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic) 또는 [보안 그룹 사용 문서](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}를 참조하십시오.

## security_group_remoting_rules_per_sg_exceeded
**메시지**: 보안 그룹당 원격 규칙 수에 대한 한계를 초과했습니다.

원격 보안 그룹을 참조하는 다른 규칙을 추가하면 보안 그룹당 원격 규칙 수에 대한 한계를 초과하게 됩니다. 리소스별 할당량은 [VPC의 할당량 및 한계](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}에 지정되어 있습니다.

## security_group_rules_per_sg_exceeded
**메시지**: 보안 그룹당 규칙 수에 대한 한계를 초과했습니다.

규칙을 추가하면 보안 그룹당 규칙 수에 대한 한계를 초과하게 됩니다. 리소스별 할당량은 [VPC의 할당량 및 한계](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}에 지정되어 있습니다. 자신의 전략을 평가하고 다른 보안 그룹을 작성하는 것을 고려하십시오. 

## security_group_sgs_per_interface_exceeded
**메시지**: 인터페이스당 보안 그룹 수에 대한 한계를 초과했습니다.

이 인터페이스를 다른 보안 그룹에 연결하면 인터페이스당 보안 그룹 수에 대한 한계를 초과하게 됩니다. 리소스별 할당량은 [VPC의 할당량 및 한계](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}에 지정되어 있습니다. 자신의 전략을 평가하고 규칙을 기존 보안 그룹에 추가하는 것을 고려하십시오. 


## security_group_type_code_invalid_protocol
**메시지**: 'icmp' 유형/코드가 지정되었으나 요청된 프로토콜이 'icmp'가 아닙니다. 프로토콜을 'icmp'로 설정하거나 포트 범위를 지정하십시오.

'icmp' 프로토콜을 사용하는 규칙만 유형 및 코드를 갖습니다. 'tcp' 또는 'udp' 프로토콜을 사용하는 규칙은 포트 범위를 갖습니다. 보안 그룹 구성에 대한 자세한 정보는 [API 문서](https://{DomainName}/apidocs/vpc-on-classic)를 참조하십시오.

## security_group_vpc_default
**메시지**: VPC의 기본 보안 그룹은 삭제할 수 없습니다.

기본 보안 그룹은 삭제할 수 없습니다. 기본 보안 그룹에 대한 정보는 [기본 보안 그룹 업데이트](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group)에 대한 안내서를 참조하십시오.

## service_manager_service_failure
**메시지**: 없음

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_conflict
**메시지**: CIDR이 VPC의 기존 서브넷과 충돌합니다.

`GET /subnets` API를 실행하여 VPC의 모든 서브넷을 검색하십시오. `ipv4_cidr_block`의 값을 확인하여 사용자가 제공한 CIDR을 다른 서브넷에서 사용 중이지 않은지 확인하십시오.

CLI를 사용하고 있는 경우에는 `ibmcloud is subnets`를 실행하고 "Subnet CIDR" 값에서 충돌 여부를 확인하십시오.

추가 도움말은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_not_empty
**메시지**: 비어 있을 때까지 서브넷을 삭제할 수 없습니다. 서브넷의 리소스를 삭제하고 재시도하십시오.

서브넷 삭제 요청이 있었지만 해당 서브넷에 여전히 리소스가 있습니다. 서브넷은 인스턴스, VPN, 로드 밸런서 또는 공용 게이트웨이를 포함하고 있을 수 있습니다. 서브넷을 삭제하려면 먼저 서브넷의 리소스를 삭제해야 합니다.

일부 상황에서는 콘솔이 0개의 VSI 및 0개의 로드 밸런서를 표시하는 경우에도 이 오류가 발생할 수 있습니다. 삭제는 비동기식이며 내부 상태가 변경되는 데 몇 분이 걸릴 수 있습니다. 몇 분 후에 서브넷 삭제를 다시 시도하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_not_empty_pgway_exists
**메시지**: 서브넷이 공용 게이트웨이에 연결되어 있으면 서브넷을 삭제할 수 없습니다. 공용 게이트웨이를 분리하고 재시도하십시오.

서브넷 삭제 요청이 있었지만 해당 서브넷에 여전히 공용 게이트웨이가 연결되어 있습니다. 서브넷을 삭제하려면 먼저 공용 게이트웨이를 삭제하거나 분리해야 합니다.

CLI를 사용하고 있는 경우에는 `ibmcloud is public-gateways`를 실행하여 공용 게이트웨이를 나열하고 `ibmcloud is subnet-public-gateway-detach`를 실행하여 서브넷에서 공용 게이트웨이를 분리할 수 있습니다.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_not_empty_ipaddr_exists
**메시지**: 서브넷이 IP 주소를 포함하고 있으면 서브넷을 삭제할 수 없습니다. 해당 IP 주소와 연관된 모든 서버 인스턴스를 삭제하고 재시도하십시오.

서브넷 삭제 요청이 있었지만 해당 서브넷에 여전히 IP 주소가 포함되어 있습니다. 서브넷을 삭제하려면 먼저 IP 주소와 연관된 서버 인스턴스를 삭제해야 합니다.

CLI를 사용 중인 경우 `ibmcloud is instances`를 실행하여 서버 인스턴스를 나열할 수 있습니다. "Address" 값을 확인하여 해당 IP 주소를 판별하고 "Status"를 확인하여 해당 상태를 판별하십시오. 이미 "deleting" 상태가 아닌 서버 인스턴스를 삭제하려면 `ibmcloud is instance-delete`를 실행하십시오.

일부 상황에서는 콘솔이 0개의 VSI를 표시하는 경우에도 이 오류가 발생할 수 있습니다. 삭제는 비동기식이며 내부 상태가 변경되는 데 몇 분이 걸릴 수 있습니다. 몇 분 후에 서브넷 삭제를 다시 시도하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_not_empty_loadbalancer_exists
**메시지**: 서브넷이 로드 밸런서를 포함하고 있으면 서브넷을 삭제할 수 없습니다. 로드 밸런서를 삭제하고 재시도하십시오.

서브넷 삭제 요청이 있었지만 해당 서브넷에 여전히 로드 밸런서가 포함되어 있습니다. 서브넷을 삭제하기 전에 먼저 로드 밸런서를 삭제해야 합니다.

CLI를 사용 중인 경우 `ibmcloud is load-balancers`를 실행하여 로드 밸런서를 나열할 수 있습니다. "Subnets" 값을 확인하여 해당 서브넷을 판별하고 "Status"를 해당 상태를 판별하십시오. 이미 "deleting" 상태가 로드 밸런서를 삭제하려면 `ibmcloud is load-balancer-delete`를 실행하십시오.

일부 상황에서는 콘솔이 0개의 로드 밸런서를 표시하는 경우에도 이 오류가 발생할 수 있습니다. 삭제는 비동기식이며 내부 상태가 변경되는 데 몇 분이 걸릴 수 있습니다. 몇 분 후에 서브넷 삭제를 다시 시도하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_not_empty_vpn_gway_exists
**메시지**: 서브넷이 VPN 게이트웨이를 포함하고 있으면 서브넷을 삭제할 수 없습니다. VPN 게이트웨이를 삭제하고 재시도하십시오. 

서브넷 삭제 요청이 있었지만 해당 서브넷에 여전히 VPN 게이트웨이가 연결되어 있습니다. 서브넷을 삭제하려면 먼저 이 VPN 게이트웨이를 삭제해야 합니다.

CLI를 사용 중인 경우 `ibmcloud is vpn-gateways`를 실행하여 VPN 게이트웨이를 나열할 수 있습니다. "Subnets" 값을 확인하여 해당 서브넷을 판별하고 "Status"를 확인하여 해당 상태를 판별하십시오. 이미 상태가 "deleting"이 아닌 VPN 게이트웨이를 삭제하려면 `ibmcloud is vpn-gateway-delete`를 실행하십시오.

일부 상황에서는 콘솔이 0개의 VPN 게이트웨이를 표시하는 경우에도 이 오류가 발생할 수 있습니다. 삭제는 비동기식이며 내부 상태가 변경되는 데 몇 분이 걸릴 수 있습니다. 몇 분 후에 서브넷 삭제를 다시 시도하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## subnet_unknown_state
**메시지**: 서브넷 `<subnet_id>`의 상태가 요청된 오퍼레이션에 대해 올바르지 않습니다.

서브넷을 작동시키려면 서브넷이 `available` 상태여야 합니다. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## target_in_use
**메시지**: 대상에 유동 IP가 이미 연결되어 있습니다.

서버의 네트워크 인터페이스에 유동 IP 주소를 연결하라는 요청이 있었지만 네트워크 인터페이스에 유동 IP가 이미 연결되어 있습니다.

현재 네트워크 인터페이스에 연결된 유동 IP 주소를 찾으려면 API 명령 `GET /v1/floating_ips?version=2019-05-31&generation=1`을 실행하고 `target.id` 필드에서 네트워크 인터페이스 ID를 찾으십시오.  

CLI를 사용 중인 경우 `ibmcloud is floating-ips` 명령을 실행하고 `Target` 값을 확인하십시오. CLI가 네트워크 인터페이스 ID를 첫 번째 대시("-") 문자에서 자른다는 점에 유의하십시오. 예를 들어, ID가 `abdfcb29-b3c5-4e4a-b7a0-cf0300154699`인 서버의 기본 네트워크 인터페이스는 CLI 출력에서 `primary(abdfcb29-.)`로 표시됩니다.

## token_invalid
**메시지**: 서비스 토큰이 만료되었거나 올바르지 않습니다.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## token_missing
**메시지**: 서비스 토큰이 비어 있거나 요청에 포함되지 않았습니다.

토큰을 다시 작성하고 다시 시도하십시오.

## validation_enum
**메시지**: 제공된 값이 올바른 옵션이 아닙니다.

제공된 필드 중 하나에 가능한 값의 열거에 속해야 하는 값이 있습니다.

네트워크 ACL 규칙의 경우에는 다음과 같이 방향을 지정할 수 있습니다.

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum:
[ inbound, outbound ]
```

예를 들면, `northbound`는 열거 `[ inbound, outbound ]`의 올바른 옵션이 아니므로 다음 값은 올바르지 않습니다.

```json
{
    "direction":"northbound"
}
```

허용 가능한 값은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window} 또는 [CLI 참조](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window}를 참조하십시오.

## validation_failure
**메시지**: 제공된 JSON이 예상 구조와 일치하지 않습니다.

이 문제를 해결하려면 요청의 컨텐츠가 올바른 JSON이며 요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 따르는지 확인하십시오.

## validation_invalid_cidr
**메시지**: 값이 올바른 CIDR이 아닙니다.

이 값은 호스트 부분이 0인 올바른 내부 CIDR 블록이어야 합니다.

특정 IP 주소 범위는 예약되어 있습니다. 예약된 IP 범위에 대한 자세한 정보는 [지역 및 서브넷과 함께 VPC 사용](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}에 대한 개요에 있습니다.

## validation_invalid_ipv4_cidr
**메시지**: 값이 올바른 IPv4 CIDR이 아닙니다.

이 값은 호스트 부분이 0인 IPv4 내부 CIDR 블록이어야 합니다.

특정 IP 주소 범위는 예약되어 있습니다. 예약된 IP 범위에 대한 자세한 정보는 [지역 및 서브넷과 함께 VPC 사용](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}에 대한 개요에 있습니다.

## validation_invalid_ipv6_cidr
**메시지**: 값이 올바른 IPv6 CIDR이 아닙니다.

IPv6은 현재 지원되지 않습니다. IPv4 주소를 사용하십시오.

## validation_invalid_address
**메시지**: 값이 올바른 주소가 아닙니다.

올바른 IP 주소여야 합니다.

개별적으로 예약된 IP 주소의 목록은 [지역 및 서브넷](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) 문서에 제공되어 있습니다.

## validation_invalid_ipv4_address
**메시지**: 값이 올바른 IPv4 주소가 아닙니다.

올바른 IPv4 주소를 제공하십시오.

## validation_invalid_ipv6_address
**메시지**: 값이 올바른 IPv6 주소가 아닙니다.

올바른 IPv6 주소를 제공하십시오. IPv6은 현재 지원되지 않으므로 IPv4 주소를 사용하십시오.

## validation_invalid_field_type
**메시지**: 값 유형이 필드 유형과 일치하지 않습니다.

이 문제를 해결하려면 요청의 컨텐츠가 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 따르는지 확인하십시오.

## validation_max_value
**메시지**: 매개변수에 대해 제공된 값이 허용되는 값보다 큽니다.

[스펙](https://{DomainName}/apidocs/vpc-on-classic){: new_window}에 지정된 최대값을 충족하는 더 작은 값을 제공하십시오.

## validation_min_value
**메시지**: 매개변수에 대해 제공된 값이 허용되는 값보다 작습니다.

`total_ipv4_address_count`가 8보다 작은 서브넷을 작성하려고 시도하면 이 오류가 발생할 수 있습니다.

[스펙](https://{DomainName}/apidocs/vpc-on-classic){: new_window}에 지정된 최소값을 충족하는 더 큰 값을 제공하십시오.

## validation_not_null
**메시지**: 제공된 필드는 널이어야 합니다.

해당 값이 널인지 확인하십시오. [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

올바르지 않은 예는 다음과 같습니다.

```json
{
    "name": null
}
```

## validation_only_one
**메시지**: 값이 하위 스키마 중 하나와 일치해야 합니다.

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 

## validation_discriminator_forbidden
**메시지**: 식별자 필드가 이 하위 구조를 금지합니다.

예를 들어 다음과 같이 지정할 경우,
```
{
protocol: icmp
port_min: 5
port_max: 100
}
```

프로토콜은 `icmp`이며 _port_min_은 `tcp` 필드이므로 오류가 발생합니다.
1. `icmp` 규칙이 누락된 경우 validation_discriminator_required
2. `icmp`가 지정된 상태에서 `tcp` 필드가 있는 경우 validation_discriminator_forbidden

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_internal_error
**메시지**: 없음

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_invalid_name
**메시지**: 값이 올바른 이름이 아닙니다.

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_invalid_ref
**메시지**: 값이 올바른 HREF가 아닙니다.

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_non_empty_uuid
**메시지**: 없음

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_required_field
**메시지**: 필수 필드가 누락되었습니다.

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## validation_unique_failed
**메시지**: 제공된 필드가 고유해야 합니다.

이미 사용 중인 이름을 지정했을 수 있습니다. 다른 값을 사용해 보십시오.

요청이 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 준수하는지 확인하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## volume_attachment_delete_invalid_request
**메시지**: 볼륨 연결을 삭제할 수 없습니다.

이 오퍼레이션은 허용되지 않습니다. 부트 볼륨이 인스턴스에 필요하므로 부트 볼륨의 볼륨 연결을 삭제할 수 없습니다.

## volume_attachment_update_invalid_request
**메시지**: 요청이 올바르지 않으므로 볼륨 연결을 업데이트할 수 없습니다.

다음과 같은 이유 중 하나로 볼륨 연결을 업데이트할 수 없습니다.

* 부트 볼륨의 볼륨 연결 이름을 변경할 수 없습니다.
* 부트 볼륨의 볼륨 연결 `delete_volume_on_instance_delete` 필드를 `true`로 설정할 수 없습니다.

## volume_capacity_max
**메시지**: 허용되는 최대 볼륨 용량은 2000GB입니다.

사용자가 지정한 볼륨 용량이 최대 볼륨 용량을 초과합니다. 10GG - 2000GB 사이의 값을 지정하십시오. 사용자 정의 프로파일에 허용되는 용량 범위는 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 문서를 참조하십시오.

## volume_capacity_missing
**메시지**: 요청된 볼륨 용량 값이 요청에서 누락되었습니다.

볼륨을 작성할 때 이 프로파일 정의를 기반으로 볼륨 용량을 지정해야 합니다. 자세한 정보는 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 문서를 참조하십시오

## volume_capacity_zero_or_negative
**메시지**: 볼륨 용량이 0보다 커야 합니다.

볼륨을 작성할 때 요청에 지정된 용량 값은 10GB - 2000GB 사이의 양수여야 합니다. 지원되는 볼륨 용량 값은 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 문서를 참조하십시오.

## volume_crn_account_id_mismatch
**메시지**: 요청에 지정된 CRN이 현재 사용자의 계정에 속해 있지 않습니다.

Key Protect 루트 키 CRN이 IAM 권한 계정 ID와 일치하지 않습니다. IAM 계정에 대해 다른 Key Protect 루트 키 CRN을 지정하십시오. 자세한 정보는 [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) 문서를 참조하십시오.

## volume_crn_cname_mismatch
**메시지**: 요청에 지정된 CRN을 현재 환경에 사용할 수 없습니다.

요청에 지정한 CRN을 현재 환경에서 사용할 수 없습니다. 환경에 적합한 CRN을 제공하십시오.

## volume_encryption_key_crn_invalid
**메시지**: 요청에 지정된 볼륨 암호화 키 CRN이 올바르지 않습니다.

제공한 Key Protect CRN이 올바르지 않습니다. Cloud Identity and Access Management 서비스에서 루트 키의 CRN을 얻으십시오. 자세한 정보는 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption) 문서를 참조하십시오

## volume_encryption_key_not_active
**메시지**: 요청에 지정된 볼륨 암호화 키가 활성 상태가 아닙니다.

기존 키를 활성화하거나 새 키를 제공하십시오. 사용자 고유의 키를 활성화할 수 없는 경우 시스템 관리자 또는 [고객 지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 문의하십시오.

## volume_encryption_key_not_authorized
**메시지**: 제공된 볼륨 암호화 키에 액세스할 수 있는 권한이 없습니다.

액세스 권한이 있는 다른 암호화 키를 제공하거나 시스템 관리자에게 문의하여 현재 키에 대한 액세스 권한을 얻으십시오.

## volume_encryption_key_not_implemented
볼륨 암호화 키 기능은 지원되지 않습니다.

사용자의 암호화 키를 사용하여 볼륨을 작성할 수 없습니다. 이 버전은 제공자 관리 암호화를 사용하는 볼륨만 지원합니다. 사용자 고유의 암호화 키를 사용하려면 고객 관리 암호화를 지원하는 VPC용 블록 스토리지 버전을 사용하십시오. 자세한 정보는 [고객 지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 문의하십시오.

## volume_id_invalid
**메시지**: 요청 매개변수에 지정된 볼륨 ID가 올바르지 않습니다.

지정한 볼륨 ID의 형식이 올바르지 않습니다. 볼륨 ID를 올바르게 입력했는지 확인한 후 다시 시도하십시오. 볼륨 ID를 확신할 수 없는 경우 CLI에서 `ibmcloud is volumes`를 지정하거나 API에서 `GET volumes`를 지정하고 볼륨 목록을 검색하십시오.

## volume_id_missing
**메시지**: 볼륨 ID가 요청 매개변수에서 누락되었습니다.

특정 볼륨을 가져올 때 요청 매개변수에 볼륨 ID를 제공해야 합니다.

## volume_id_not_found
**메시지**: 볼륨을 찾을 수 없습니다. 볼륨 ID `<volume_id>`가 존재하지 않습니다. 여기서 `<volume_id>`는 제공된 볼륨 ID입니다.

지정된 볼륨 ID가 존재하지 않습니다. 올바른 볼륨 ID를 제공하십시오.

## volume_iops_zero_or_negative
**메시지**: 볼륨 IOPS는 0보다 커야 합니다.

볼륨을 작성할 때 요청에 지정된 IOPS 값은 양수여야 합니다. 지원되는 IOPS 값은 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 문서를 참조하십시오.

## volume_name_duplicate
**메시지**: 볼륨 이름이 중복되었습니다. 요청에 제공된 볼륨 이름 `<volume_name>`이 이미 있습니다. 여기서 `<volume_name>`은 제공된 볼륨 이름입니다.

요청에 지정된 볼륨 이름이 이미 있습니다. 현재 사용 중이지 않은 볼륨 이름을 제공하십시오.

## volume_not_available
**메시지**: 볼륨을 사용할 수 없습니다. 사용 가능 상태의 볼륨만 수정할 수 있습니다. 현재 볼륨 `<volume_id>` 상태가 `<volume_status>`입니다. 여기서 `<volume_id>`는 요청에 제공된 볼륨 ID이며 `<volume_status>`는 현재 볼륨 상태입니다.

`available` 상태인 경우에만 볼륨을 수정할 수 있습니다. 볼륨을 수정하기 전에 볼륨이 사용 가능한지 확인하십시오.

## volume_not_deletable
**메시지**: 삭제에 실패했습니다.

`available` 또는 `failed` 상태인 경우에만 볼륨을 삭제할 수 있습니다. 볼륨이 삭제될 수 있는 올바른 상태인지 확인하십시오.

## volume_not_found
**메시지**: 지정된 ID의 볼륨을 찾을 수 없습니다.

입력한 볼륨 ID가 올바른지 확인하고 다시 시도하십시오. 볼륨 목록을 보려면 CLI에서 `ibmcloud is volumes`를 지정하거나 API에서 `GET volumes`를 지정하십시오.

## volume_not_implemented
**메시지**: 요청된 오퍼레이션이 이 릴리스에서 지원되지 않습니다.

자세한 정보는 [고객 지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 문의하십시오.

## volume_profile_capacity_iops_invalid
**메시지**: 요청에 지정된 볼륨 프로파일이 제공된 용량 및/또는 IOPS에 대해 유효하지 않습니다.

요청에 지정한 볼륨 용량 및/또는 IOPS 값이 볼륨 프로파일에서 허용되지 않습니다. 사용자 정의 프로파일에 대한 올바른 최소 및 최대 용량과 IOPS 값은 [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) 문서를 참조하십시오.

## volume_profile_iops_invalid
**메시지**: 요청에 지정된 볼륨 프로파일이 사용자 정의 IOPS를 허용할 수 없습니다.

볼륨 프로파일이 사용자 정의 IOPS 값을 허용하지 않습니다. IOPS 값을 지정할 필요가 없는 계층화된 IOPS 프로파일을 지정했을 수 있습니다. 특정 IOPS 값을 제공하려면 사용자 정의 IOPS 프로파일을 사용하십시오.

## volume_profile_name_missing
**메시지**: 요청된 볼륨 프로파일 이름이 요청에서 누락되었습니다.

볼륨 프로파일 이름이 볼륨을 작성할 때 요청 본문에서 누락되었거나 볼륨을 가져올 때 요청 매개변수에서 누락되었습니다. 요청에 볼륨 프로파일 이름을 제공하고 다시 시도하십시오. 볼륨 프로파일 이름을 모르는 경우 CLI에서 `ibmcloud is volume-profiles`를 지정하거나 API 요청에서 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`를 사용하여 목록을 검색하십시오.

## volume_profile_not_found
**메시지**: 지정된 이름의 볼륨 프로파일을 찾을 수 없습니다.

해당 이름의 볼륨 프로파일을 찾을 수 없습니다. 올바른 볼륨 프로파일 이름을 제공했는지 확인하십시오. 볼륨 프로파일 이름을 모르는 경우 CLI에서 `ibmcloud is volume-profiles`를 지정하거나 API 요청에서 `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD`를 사용하여 목록을 검색하십시오.

## volume_quota_reached
**메시지**: 볼륨 할당량에 도달했습니다.

현재 계정에 대한 할당량이 부족합니다. 일부 볼륨을 삭제하거나 [고객 지원](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)에 문의하여 할당량을 늘리십시오. [Virtual Private Cloud 인프라에 대한 할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas) 관련 정보는 문서를 참조하십시오.

## volume_resource_group_id_invalid
**메시지**: 요청에 지정된 리소스 그룹 ID가 올바르지 않습니다.

요청에 지정한 리소스 그룹 ID가 올바르지 않습니다. 리소스 그룹 ID가 올바른지 확인하십시오. CLI에서 `ibmcloud is volume VOLUME_ID` 명령을 사용하십시오. 결과 정보에는 리소스 그룹 및 리소스 그룹 ID가 포함됩니다. 자세한 정보는 [VPC용 IBM Cloud CLI 참조](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)를 참조하십시오.

## volume_resource_group_not_authorized
**메시지**: 현재 조치에 지정된 리소스 그룹에 대한 권한이 부여되지 않았습니다.

지정된 리소스 그룹에서 볼륨을 나열하거나 작성할 수 있는 권한이 부여되지 않았습니다. 권한이 있는 다른 리소스 그룹을 사용하거나 관리자에게 리소스 그룹에 대한 나열 및 작성 권한을 요청하십시오.

## volume_resource_group_not_found
**메시지**: 지정된 ID의 리소스 그룹을 작성할 수 없습니다.

잘못 입력했거나 존재하지 않으므로 리소스 그룹 ID를 찾을 수 없습니다. 리소스 그룹 ID가 올바른지 확인하십시오. CLI에서 `ibmcloud is volume VOLUME_ID` 명령을 사용하십시오. 결과 정보에는 리소스 그룹 및 리소스 그룹 ID가 포함됩니다. 자세한 정보는 [VPC용 IBM Cloud CLI 참조](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage)를 참조하십시오.

## volume_start_not_found
**메시지**: ID가 페이지 시작 매개변수로 지정된 볼륨을 찾을 수 없습니다.

목록 볼륨 호출의 시작 매개변수에 지정된 볼륨 ID를 찾을 수 없습니다. 볼륨 ID가 올바르고 볼륨에 대한 액세스 권한이 있는지 확인하십시오.

## volume_status_not_available
**메시지**: 지정된 ID의 볼륨을 사용할 수 없습니다.

볼륨이 사용 가능 상태가 아닙니다. 해당 상태가 보류 상태일 수 있습니다. 볼륨이 사용 가능하게 될 때까지 대기한 후 다시 시도하십시오. 볼륨을 삭제 중인 경우 다른 볼륨을 사용하십시오. 볼륨의 상태를 확인하려면 CLI에서 `ibmcloud is volume VOLUME_ID`를 지정하거나 API 요청에서 `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD`를 사용하십시오.

## volume_still_attached
**메시지**: 볼륨이 여전히 인스턴스에 연결되어 있습니다.

여전히 VSI에 연결된 볼륨은 삭제할 수 없습니다. 볼륨의 자동 삭제를 설정한 경우 VSI가 삭제되고 볼륨이 분리 및 삭제될 때까지 대기하십시오. 자동 삭제를 설정하지 않은 경우 수동으로 볼륨을 분리하십시오.

## volume_template_invalid
**메시지**: 올바르지 않은 볼륨 템플리트가 제공되었습니다.

볼륨을 작성하기 위해 올바르지 않은 볼륨 템플리트를 제공했습니다. 요청 본문에 올바른 매개변수를 제공하는지 확인하려면 [VPC API 참조](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## volume_update_invalid_request
**메시지**: 볼륨 업데이트 요청이 올바르지 않습니다.

사용자가 지정한 업데이트 요청 본문 특성이 올바르지 않습니다. 정보 및 올바른 API 요청 구문은 [VPC API 참조](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## vpc_not_empty
**메시지**: VPC가 비어 있지 않아 삭제할 수 없습니다.

VPC를 삭제하려면 먼저 VPC의 모든 리소스를 삭제해야 합니다. VPC에는 인스턴스, 서브넷, 공용 게이트웨이, 로드 밸런서 및 VPN 게이트웨이가 포함되어 있으며 이러한 리소스 중 일부에도 리소스가 포함되어 있습니다. 세부사항은 [VPC 삭제 방법](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) 문서를 참조하십시오.

이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpc_resource_separation
**메시지**: 리소스가 서로 다른 VPC에 있습니다. 

리소스는 동일한 VPC에 있어야 합니다. 예를 들어, 공용 게이트웨이를 서브넷에 연결하려고 시도하는 경우 공용 게이트웨이가 서브넷과 동일한 VPC에 있어야 합니다.

공용 게이트웨이를 포함하는 VPC를 판별하려면 `GET /public_gateways/{id}` 명령을 실행하고 "vpc" 값을 확인하십시오. 동등한 CLI 명령은 `ibmcloud is public-gateway <id>`입니다.

## vpn_connection_cidr_duplicated
**메시지**: `<cidr_type>` CIDR 블록에 중복된 CIDR 블록 `<cidr_blocks>`가 있습니다.

연결을 작성할 때 중복된 CIDR 블록이 제공되었습니다. CIDR 블록이 고유한지 확인하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_cidr_not_created
**메시지**: CIDR 블록을 VPN 연결 `<vpn_connection_id>`에 추가할 수 없습니다.

스펙에 지정된 요구사항을 충족하는 올바른 CIDR을 제공하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_cidr_not_deleted
**메시지**: CIDR 블록을 VPN 연결 `<vpn_connection_id>`에서 삭제할 수 없습니다.

연결에 있는 올바른 CIDR을 제공하십시오. 연결에는 하나 이상의 로컬 및 피어 CIDR이 있어야 합니다.

연결의 CIDR 블록을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `local_cidrs` 및 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_found
**메시지**: CIDR 블록을 VPN 연결 `<vpn_connection_id>`에서 찾을 수 없습니다.

연결에 있는 올바른 CIDR을 제공하십시오. 

연결의 CIDR 블록을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `local_cidrs` 및 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**메시지**: VPN 연결 `<vpn_connection_id>`에서 CIDR 블록을 업데이트하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_cidr_not_valid
**메시지**: CIDR 블록 `<cidr_block>`이 올바른 주소를 나타내지 않습니다.

이 값은 호스트 비트가 설정되지 않은 올바른 내부 CIDR이어야 합니다. 

연결의 CIDR 블록을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `local_cidrs` 및 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_overlap
**메시지**: CIDR 블록 `<cidr_block_1>`이 `<cidr_block_2>`와 겹칩니다. 두 피어 CIDR 블록은 동일한 VPC에서 겹칠 수 없으며 두 로컬 CIDR 블록은 동일한 연결에서 겹칠 수 없습니다.

스펙에 지정된 요구사항을 충족하는 올바른 CIDR을 제공하십시오.

연결의 CIDR 블록을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `local_cidrs` 및 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**메시지**: 이름 `<vpn_connection_name>`이 이미 VPN 연결 `<vpn_connection_id>`에서 사용 중입니다.

다른 연결 이름을 제공하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_invalid_name
**메시지**: 이름 `<vpn_connection_name>`은 올바른 VPN 연결 이름이 아닙니다.

올바른 연결 이름은 문자로 시작하며 그 뒤로 문자, 숫자, 밑줄 또는 하이픈이 따릅니다.

## vpn_connection_invalid_psk_format
**메시지**: 사전 공유 키(PSK) `<psk>`의 형식이 올바르지 않습니다.

올바른 PSK는 6 - 128자 길이로 다음 문자만을 포함해야 합니다: 문자, 숫자, `-`, `+`, `&`, `!`, `@`, `#`, `$`, `%`, `^`, `*`, `(`, `)`, `,`, `.`, `:`, `_`

## vpn_connection_local_cidrs_required
**메시지**: VPN 연결을 작성할 때는 하나 이상의 로컬 CIDR 블록이 필요합니다.

스펙에 지정된 요구사항을 충족하는 올바른 로컬 CIDR을 제공하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## vpn_connection_local_subnets_quota_exceeded
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`에 대한 VPN 연결 전체의 로컬 서브넷이 할당량에 도달했습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

VPN 연결 전체의 현재 로컬 서브넷을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections` API를 사용하고 각 연결에 대한 `local_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**메시지**: VPN 연결에 대한 로컬 서브넷이 할당량에 도달했습니다.

리소스별 할당량은 [할당량](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

VPN 연결에 대한 현재 로컬 서브넷을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `local_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**메시지**: VPN 연결 `<vpn_connection_id>`를 작성하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_not_deleted
**메시지**: VPN 연결 `<vpn_connection_id>`를 삭제하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_not_found
**메시지**: VPN 연결 `<vpn_connection_id>`를 찾지 못했습니다.

연결 ID가 올바른지 확인하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_not_updated
**메시지**: VPN 연결 `<vpn_connection_id>`를 업데이트하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_pair_cidrs_duplicated
**메시지**: 동일한 피어 게이트웨이를 사용하는 하나 이상의 로컬 CIDR 및 피어 CIDR 쌍이 중복되었습니다.

이 연결과 일치하는 로컬 CIDR 및 피어 CIDR이 있는 다른 연결이 이 게이트웨이에 있습니다. 올바른 로컬/피어 CIDR 세트를 제공하십시오.

## vpn_connection_peer_cidrs_required
**메시지**: VPN 연결을 작성할 때는 하나 이상의 피어 CIDR 블록이 필요합니다.

스펙에 지정된 요구사항을 충족하는 올바른 피어 CIDR을 제공하십시오.

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## vpn_connection_peer_subnets_quota_exceeded
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`에 대한 VPN 연결 전체의 피어 서브넷이 할당량에 도달했습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

VPN 연결 전체의 현재 피어 서브넷을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections` API를 사용하고 각 연결에 대한 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**메시지**: VPN 연결에 대한 피어 서브넷이 할당량에 도달했습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

VPN 연결에 대한 현재 피어 서브넷을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` API를 사용하고 `peer_cidrs` 필드를 확인하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**메시지**: 피어 CIDR 블록 `<peer_cidr>`에 대한 정적 라우트를 추가하는 데 실패했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_static_route_not_deleted
**메시지**: 피어 CIDR 블록 `<peer_cidr>`에 대한 정적 라우트를 제거하는 데 실패했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_connection_update_cidrs_not_permitted
**메시지**: 연결을 업데이트 중일 때는 CIDR 블록을 변경할 수 없습니다. CIDR 블록을 작성하거나 삭제할 때 올바른 API를 사용하십시오.

스펙에 지정된 요구사항을 충족하는 올바른 요청 값을 제공하십시오. 

이 문제점을 해결하는 데 대한 추가 지시사항은 [API 문서](https://{DomainName}/apidocs/vpc-on-classic){: new_window}를 참조하십시오.

## vpn_connections_quota_exceeded
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`가 할당량에 도달했으므로 VPN 연결을 작성할 수 없습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

VPN 게이트웨이에 대한 현재 VPN 연결을 보려면 `GET /vpn_gateways/<vpn_gateway_id>/connections` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**메시지**: 이름 `<vpn_gateway_name>`이 이미 VPN 게이트웨이 `<vpn_gateway_id>`에서 사용 중입니다.

다른 VPN 게이트웨이 이름을 제공하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_initialization_error
**메시지**: VPN 게이트웨이를 초기화하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_invalid_name
**메시지**: 이름 `<vpn_gateway_name>`은 올바른 VPN 게이트웨이 이름이 아닙니다.

올바른 VPN 게이트웨이 이름은 문자로 시작하며 그 뒤로 문자, 숫자, 밑줄 또는 하이픈이 따릅니다.

## vpn_gateway_invalid_state
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`의 상태가 요청된 오퍼레이션에 대해 올바르지 않습니다.

VPN 게이트웨이를 작동시키려면 게이트웨이가 `available` 상태여야 합니다. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_ip_create_error
**메시지**: VPN 게이트웨이의 IP 주소를 작성하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_missing_subnet_id
**메시지**: 지정된 VPN 게이트웨이 템플리트에 대한 서브넷 ID를 찾지 못했습니다.

**서브넷 ID**는 필수 필드입니다. 명령을 사용하여 서브넷 ID를 제공하십시오.

## vpn_gateway_missing_name
**메시지**: 지정된 VPN 게이트웨이 템플리트에 대한 이름을 찾지 못했습니다.

**이름**은 필수 필드입니다. 명령을 사용하여 이름을 제공하십시오.

## vpn_gateway_not_created
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`를 작성하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_not_deleted
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`를 삭제하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_not_found
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`를 찾지 못했습니다.

VPN 게이트웨이 ID가 올바른지 확인하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_not_updated
**메시지**: VPN 게이트웨이 `<vpn_gateway_id>`를 업데이트하지 못했습니다.

몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_subnet_not_found
**메시지**: 지정된 VPN 게이트웨이에 대한 서브넷 `<subnet_id>`를 찾지 못했습니다.

존재하지 않는 서브넷을 참조했습니다. 요청을 검토하여 적절한 서브넷 ID를 지정했는지 확인하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateway_subnet_status_error
**메시지**: 서브넷 상태로 인해 VPN 게이트웨이를 작성하지 못했습니다.

`available` 상태의 서브넷을 제공하십시오. 몇 분 후에 다시 시도하십시오. 이 문제점이 지속되는 경우 [지원에 문의](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support)하십시오.

## vpn_gateways_quota_exceeded
**메시지**: 사용자 계정 및/또는 지역이 할당량에 도달했으므로 VPN 게이트웨이를 작성할 수 없습니다.

리소스별 할당량은 [할당량](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window} 페이지에 지정되어 있습니다. 

현재 VPN 게이트웨이를 보려면 `GET /vpn_gateways` API를 사용하십시오.
동등한 CLI 명령: `ibmcloud is vpn-gateways`

## zone_inconsistency
**메시지**: 리소스는 동일한 구역에 속해 있어야 합니다.

* 볼륨을 연결할 때 볼륨과 인스턴스가 동일한 구역에 있는지 확인하십시오.
* 유동 IP를 연관시킬 때 유동 IP와 서브넷이 동일한 구역에 있는지 확인하십시오.
