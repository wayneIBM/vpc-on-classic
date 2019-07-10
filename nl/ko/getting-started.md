---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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

# 시작하기 튜토리얼
{: #getting-started}
[comment]: # (링크된 도움말 항목)


{{site.data.keyword.cloud}} Virtual Private Cloud를 시작하려면 다음을 수행하십시오.

1. Virtual Private Cloud를 작성하십시오.
2. 한 구역 이상의 Virtual Private Cloud에서 서브넷을 하나 이상 작성하십시오.
3. 서브넷에 있는 리소스와 인터넷 간의 연결을 가능하게 하려면 서브넷에 공용 게이트웨이(PGW)를 작성하십시오.
4. 실행하려는 가상 서버 인스턴스(VSI)의 프로파일을 선택하고 이를 인스턴스화하십시오.
5. 유동 IP 주소를 예약하고, 인터넷에서 가상 서버 인스턴스에 접근할 수 있도록 하려면 해당 주소를 가상 서버 인스턴스와 연관시키십시오.
5. 가상 서버 인스턴스에 서비스 또는 애플리케이션을 배치하십시오.

## 전제조건

 * **사용자 권한**: 사용자에게 VPC에서 리소스를 작성하고 관리할 수 있는 권한이 있는지 확인하십시오. 필수 권한의 목록은 [VPC 사용자에게 필요한 권한 부여](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)를 참조하십시오.

 * **SSH 키 준비**: 사용자는 가상 서버 인스턴스에 연결하는 데 SSH 키를 사용합니다.

   * 홈 디렉토리 아래의 `.ssh` 디렉토리에서 `id_rsa.pub`라는 파일을 찾으십시오(예: `/Users/<USERNAME>/.ssh/id_rsa.pub`). 이 파일은 `ssh-rsa`로 시작하며 사용자의 이메일 주소로 끝납니다.

   * 또는 SSH 키 생성: 공개 SSH 키가 없거나 기존 키의 비밀번호를 잊어버린 경우에는 `ssh-keygen` 명령을 실행한 후 프롬프트에 따라 새 키를 생성하십시오. 예를 들면, 명령 `ssh-keygen -t rsa -C "user_ID"`를 실행하여 Linux 서버에서 SSH 키를 생성할 수 있습니다. 이 명령은 두 개의 파일을 생성합니다. 생성된 공개 키는 `<your key>.pub` 파일에 있습니다.

## UI, CLI 또는 REST API 사용

모든 VPC 리소스는 UI, CLI 또는 REST API를 통해 프로비저닝하고 관리할 수 있습니다.

IBM Cloud Virtual Private Cloud를 처음 사용하는 경우에는 IBM Cloud VPC 및 해당 리소스를 작성하는 프로세스를 처음부터 끝까지 안내하는 아래 링크 중 하나를 선택하십시오. 콘솔 UI, 명령행(CLI) 또는 REST API에서 시작할지 선택할 수 있습니다.

* 사용자 인터페이스를 통한 액세스를 위해서는 [IBM Cloud 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")]( https://{DomainName}/vpc){: new_window}에 로그인하십시오. 자세한 정보는 [IBM Cloud 콘솔을 사용하여 VPC 작성](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console)을 참조하십시오.
* 명령행 인터페이스를 사용하려면 [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli) 예제를 따르십시오.
* 고급 사용자는 [REST API](https://{DomainName}/apidocs/vpc-on-classic)를 직접 호출할 수 있습니다. [코드 예](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) 튜토리얼에 따라 REST API 작업을 시작하십시오.

## 다음 단계
작업을 시작할 준비가 되었으면 **VPC용 네트워킹**, **VPC용 VSI** 및 **VPC용 블록 스토리지**에 대한 상세 문서로 바로 이동하십시오.

* [**Virtual Private Cloud용 네트워킹**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Virtual Private Cloud용 Virtual Server**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Virtual Private Cloud용 Block Storage**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
