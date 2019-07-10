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

# 使用 IBM Cloud 主控台刪除 VPC
{: #deleting-using-console}

在 IBM Cloud 主控台中，必須先刪除 VPC 的所有公用閘道、子網路和連接的資源，然後才能刪除 VPC。

若要使用主控台刪除 VPC，請執行下列動作：

1. 尋找 VPC 中的所有子網路。按一下導覽窗格中的 **VPC**，然後選取 VPC。 
2. 在 VPC 詳細資料頁面上，移至**此 VPC 中的子網路**清單，然後選取子網路以檢視其詳細資料。
3. 刪除連接到子網路的所有資源。在導覽窗格中，按一下**連接的資源**。對於每個連接的實例、負載平衡器或 VPN 閘道，選取相應資源以移至其詳細資料頁面。在每個資源的詳細資料頁面上，按一下「刪除」圖示。 

  雖然資源的狀態會立即變更為 **deleting**，但完成刪除作業可能需要最長 30 分鐘時間。直到控制台中不再顯示所有連接的資源後，才能刪除子網路。
  {: tip}

4. 刪除所有連接的資源後，請回到子網路的詳細資料頁面，然後按一下「刪除」圖示。
5. 刪除所有子網路後，請回到 VPC 的詳細資料頁面，然後按一下「刪除」圖示。這將刪除 VPC 及其所有公用閘道。
