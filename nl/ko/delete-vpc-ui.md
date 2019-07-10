---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, api, rias, ui, console

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# IBM Cloud 콘솔을 사용하여 VPC 삭제
{: #deleting-using-console}

IBM Cloud 콘솔에서 VPC를 삭제하기 전에 먼저 VPC의 공용 게이트웨이, 서브넷 및 연결된 리소스를 모두 삭제해야 합니다.

콘솔을 사용하여 VPC를 삭제하려면 다음 작업을 수행하십시오.

1. VPC의 모든 서브넷을 찾으십시오. 탐색 분할창에서 **VPC**를 클릭하고 VPC를 선택하십시오. 
2. VPC 세부사항 페이지에서 **이 VPC의 서브넷** 목록으로 이동하고 세부사항을 확인할 서브넷을 선택하십시오.
3. 서브넷에 연결된 모든 리소스를 삭제하십시오. 탐색 분할창에서 **연결된 리소스**를 클릭하십시오. 각각의 연결된 인스턴스, 로드 밸런서 또는 VPN 게이트웨이에 대해 세부사항 페이지로 이동할 리소스를 선택하십시오. 각 리소스의 세부사항 페이지에서 삭제 아이콘을 클릭하십시오. 

  리소스의 상태가 즉시 **삭제 중**으로 변경되지만 삭제 오퍼레이션을 완료하는 데 최대 30분이 걸릴 수 있습니다. 연결된 모든 리소스가 더 이상 콘솔에 표시되지 않을 때까지 서브넷을 삭제할 수 없습니다.
  {: tip}

4. 연결된 모든 리소스가 삭제된 후 서브넷의 세부사항 페이지로 돌아가서 삭제 아이콘을 클릭하십시오.
5. 모든 서브넷을 삭제한 후 VPC의 세부사항 페이지로 돌아가서 삭제 아이콘을 클릭하십시오. VPC 및 모든 공용 게이트웨이가 삭제됩니다.
