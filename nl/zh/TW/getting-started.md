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

# 入門指導教學
{: #getting-started}
[comment]: # (鏈結的說明主題)


若要開始使用 {{site.data.keyword.cloud}} Virtual Private Cloud，請執行下列動作：

1. 建立 Virtual Private Cloud。
2. 在一個以上區域的 Virtual Private Cloud 中建立一個以上的子網路。
3. 如果您希望子網路上的資源可以存取網際網路，請在子網路上建立公用閘道 (PGW)，反之亦然。
4. 選取您要執行的虛擬伺服器實例 (VSI) 的設定檔，並將它們實例化。
5. 保留浮動 IP 位址，如果您要從網際網路連接虛擬伺服器實例，請建立浮動 IP 位址與虛擬伺服器實例的關聯。
5. 將服務或應用程式部署至虛擬伺服器實例。

## 必要條件

 * **使用者許可權**：確保您的使用者具有足夠的許可權可在 VPC 中建立及管理資源。如需必要許可權的清單，請參閱[授與 VPC 使用者所需的許可權](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources)。

 * **備妥 SSH 金鑰**：您將使用 SSH 金鑰來連接至虛擬伺服器實例。

   * 在起始目錄的 `.ssh` 目錄下尋找稱為 `id_rsa.pub` 的檔案，例如，`/Users/<USERNAME>/.ssh/id_rsa.pub`。檔案的開頭為 `ssh-rsa`，結尾為電子郵件位址。

   * 或者，「產生 SSH 金鑰」：如果您沒有公開 SSH 金鑰，或忘記現有 SSH 金鑰的密碼，請執行 `ssh-keygen` 指令並遵循提示來產生新的 SSH 金鑰。例如，您可以透過執行 `ssh-keygen -t rsa -C "user_ID"` 指令，在 Linux 伺服器上產生 SSH 金鑰。該指令會產生兩個檔案。產生的公開金鑰位於 `<your key>.pub` 檔案中。

## 使用使用者介面、CLI 或 REST API

您可以透過使用者介面、CLI 或 REST API 來佈建及管理所有 VPC 資源。

如果您不熟悉 IBM Cloud Virtual Private Cloud，請一律選擇下面的任何鏈結，以引導您完成建立 IBM Cloud VPC 及其資源的處理程序。您可以選擇是要從主控台使用者介面、指令行 (CLI) 還是從 REST API 開始。

* 若要透過使用者介面存取，請登入 [IBM Cloud 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")]( https://{DomainName}/vpc){: new_window}。如需相關資訊，請參閱[使用 IBM Cloud 主控台建立 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console)。
* 若要使用指令行介面，請遵循 [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli) 範例。
* 若為更高階的使用者，您可以直接呼叫 [REST API](https://{DomainName}/apidocs/vpc-on-classic)。請遵循[程式碼範例](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis)指導教學，開始使用 REST API。

## 後續步驟
如果您準備好深入探索，請直接移至有關 **VPC 網路連線功能**、**VSI for VPC** 和 **Block Storage for VPC** 的詳細文件：

* [**Virtual Private Cloud 網路連線功能**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Virtual Servers for Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage for Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
