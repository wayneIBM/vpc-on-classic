---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: troubleshoot, tips, limitations, debug, mode, error, bearer, token, API, CLI, endpoint, problem, reboot, 409, status, instance, reset, asynchronous

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
{:troubleshoot: .troubleshoot}

# IBM Cloud VPC 문제점 해결
{: #troubleshooting-your-ibm-cloud-vpc}

이 문서에서는 발생할 수 있는 일반적인 문제점을 다루며 몇 가지 유용한 팁을 제공합니다.

## CLI 사용 중 `TRACE` 모드 설정
{: #troubleshoot}

CLI를 사용할 때 `TRACE`(디버그) 모드를 설정하려면 CLI 명령 앞에 `IBMCLOUD_TRACE=true`를 설정하십시오.

예를 들면 다음과 같습니다.

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## API 사용 중 DEBUG 모드 또는 `verbose` 모드 사용
{: #troubleshoot}

예를 들면, `curl` 명령에 `--verbose`를 추가하고 문제점을 해결할 수 있도록 지원 팀에 `X-Request-Id:`를 전송할 수 있습니다. `--verbose` 플래그는 연결 문제가 발생한 경우 특히 유용합니다.

다른 선택사항은 지원 팀이 요청에 대한 응답의 헤더를 볼 수 있도록 `curl` 명령에 `-i` 플래그를 추가하는 것입니다. 이 플래그는 지원에 문의해야 하는 대부분의 상황에서 유용합니다.

## API 호출에는 Bearer 토큰이 필요함
{: #troubleshoot}

cURL 명령으로 API를 사용하는 경우에는 `$iam_token`에 포함된 항목에 따라 권한 부여 헤더에 "Bearer"를 포함시켜야 할 수 있습니다. 헤더가 단어 "Bearer"를 포함하는 경우에는 이를 다시 포함시키지 마십시오. 대부분의 예에서는 "Bearer"가 헤더에 포함되어 있다고 가정합니다.

## 일반적인 문제점
{: #troubleshoot}

발생할 수 있는 몇 가지 일반적인 문제점은 다음과 같습니다.

### 권한 부여되지 않음(401 또는 403 오류)
{: #troubleshoot}

사용자 계정에 VPC에 대한 권한이 부여되지 않았을 수 있습니다. 온보드된 계정을 사용하고 있는지 확인하십시오.

### 리소스를 작성할 수 없음
{: #troubleshoot}

VPC 또는 기타 리소스를 작성할 수 없는 경우에는 계정의 소유자가 사용자에게 올바른 [권한](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)을 부여했는지 확인하십시오.

### 인스턴스의 상태가 모두 Unknown임
{: #troubleshoot}

계정의 소유자가 사용자에게 인스턴스를 보고 관리하는 데 필요한 [권한](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)을 부여했는지 확인하십시오.

### API가 응답하지 않음
{: #troubleshoot}

API가 더 이상 JSON을 리턴하지 않는 경우에는 IAM 토큰이 만료되어 새로 고쳐야 하는 경우일 가능성이 높습니다. IBM Cloud에 다시 로그인하거나 `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`을 다시 실행하여 토큰을 새로 고치십시오..

### 오류: 인스턴스에 대한 조치를 호출할 때 409 충돌이 발생함
{: #troubleshoot}

인스턴스의 상태가 특정 조치와 충돌하는 경우에는 해당 조치를 호출할 수 없습니다. 예를 들어, 인스턴스의 상태가 `Stopped`인데 `reboot` 조치를 수행하려 한 경우에는 시스템이 409 오류를 리턴합니다.

| 상태        | 조치       | 충돌     |
| ----------- | ---------- | -------- |
| Running     | start      | 예       |
| Stopped     | start 외 모든 조치    | 예      |
| Not running | pause      | 예       |
| Not running | reboot     | 예       |
| Not paused  | resume     | 예       |
| Paused      | resume 외 모든 조치   | 예      |


### 인스턴스가 `instance-reboot` 요청에 응답하지 않음
{: #troubleshoot}

인스턴스가 `instance-reboot` 요청에 응답하지 않는 경우에는 `instance-reset` 요청을 시도해 볼 수 있습니다. `instance-reboot`는 소프트 요청으로, `instance-reset`은 하드 요청으로 생각할 수 있습니다. `instance-reboot` 요청은 인스턴스에 OS 다시 부팅 요청을 전송하지만, `instance-reset` 요청은 VSI 인스턴스의 하드 리셋을 수행합니다. 이 차이는 컴퓨터의 키보드에서 "ctrl-alt-delete"를 누르는 것과 리셋 또는 전원 단추를 누르는 것의 차이라고 보면 됩니다. `instance-reset` 요청은 `instance-reboot` 요청보다 완료하는 데 더 오랜 시간이 소요된다는 점을 알고 있는 것이 좋습니다.

### 리소스를 삭제할 수 없음
{: #troubleshoot}

특정 조작(VSI 작성 및 삭제, 서브넷 작성 및 삭제 등)은 API를 통해 비동기로 완료됩니다. 이로 인해, 삭제를 진행하기 전에는 삭제 여부를 확인하기 위해 삭제하는 리소스에 대한 폴링을 수행하는 것이 좋습니다.

비동기 조작으로 인해 리소스가 시스템에서 삭제되는 데는 몇 분 정도 소요될 수 있습니다. 삭제를 용이하게 하려면 다음 순서대로 작업을 수행하는 것이 좋습니다.

1. 인스턴스 삭제
2. 공용 게이트웨이 삭제
3. 서브넷 삭제
4. VPC 삭제

특정 정보는 [VPC 삭제 방법](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)의 지시사항을 참조하십시오.
