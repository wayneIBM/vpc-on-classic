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

# IBM Cloud コンソールを使用した VPC の削除
{: #deleting-using-console}

IBM Cloud コンソールで VPC を削除する前に、VPC のパブリック・ゲートウェイ、サブネット、および接続されたリソースをすべて削除する必要があります。

コンソールを使用して VPC を削除するには、以下のようにします。

1. VPC 内のすべてのサブネットを検索します。ナビゲーション・ペインで**「VPC」**をクリックし、VPC を選択します。 
2. VPC の詳細ページで、**「この VPC のサブネット (Subnets in this VPC)」**リストに移動し、サブネットを選択してその詳細を表示します。
3. サブネットに接続されているすべてのリソースを削除します。ナビゲーション・ペインで、**「接続されているリソース (Attached resources)」**をクリックします。接続されているインスタンス、ロード・バランサー、または VPN ゲートウェイごとにリソースを選択して、その詳細ページに移動します。各リソースの詳細ページで、削除アイコンをクリックします。 

  リソースの状況は即時に**「削除中」**に変わりますが、削除操作が完了するまで最大 30 分かかる場合があります。接続されているすべてのリソースがコンソールに表示されなくなるまで、サブネットを削除することはできません。
  {: tip}

4. 接続されたすべてのリソースが削除されたら、サブネットの詳細ページに戻り、削除アイコンをクリックします。
5. すべてのサブネットを削除したら、VPC の詳細ページに戻り、削除アイコンをクリックします。VPC およびすべての公開ゲートウェイが削除されます。
