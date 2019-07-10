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

# 管理 VPC 資源的使用者許可權
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.cloud}} Virtual Private Cloud 使用以角色為基礎的存取控制，讓帳戶管理者控制其使用者對 VPC 資源的存取。

如需 IBM Cloud VPC 如何使用以角色為基礎的存取控制以及 VPC 如何使用 IAM 角色及存取原則的相關資訊，請參閱[指派對 VPC 資源的角色存取](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources)。

本文件顯示帳戶管理者如何將其他使用者新增至帳戶，並提供他們正確的許可權來管理 [VPC 基礎架構資源](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources)。它涵蓋 VPC 管理者的兩個常見情境：

* **簡單存取情境：**說明如何為使用者指派存取原則，以便使用者可以建立和使用 VPC 基礎架構資源（包括 Virtual Private Cloud）。

* **團隊存取情境：**顯示如何設定資源群組及存取原則，讓兩個不同的團隊建立及使用指派給其團隊的 VPC 資源。

VPC 的 IAM 存取原則變更可能需要 10 分鐘才能生效。
{: note}

## 簡單存取情境
{: #simple-access-scenario}

此情境涵蓋設定個別使用者所需的基本步驟。我們將涵蓋兩種情況：邀請新使用者加入帳戶，以及修改現有使用者的許可權。

### 邀請新使用者建立或管理 VPC 資源
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

邀請 IBM Cloud 使用者加入您的帳戶，並為他們提供對 `VPC 基礎架構`的存取權，以便他們有權檢視、建立和更新 VPC 資源。本節提供 IAM 步驟的快速概觀。透過接近本文件尾端的**相關鏈結**小節中的鏈結，即可取得進一步資訊。

以下是 IAM 中邀請使用者加入 VPC 服務及資源所需的基本步驟：

1. 導覽至「IBM Cloud 主控台」中的 [IAM 使用者使用者介面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/iam#/users){: new_window}。
2. 在**使用者**頁面上，按一下**邀請使用者**。
3. 在**邀請使用者**頁面的**使用者**區段中，於**電子郵件位址**欄位中輸入您要邀請之使用者的電子郵件位址。
4. 在**存取**區段中，展開**服務**，然後完成下列作業：
  * 從**將存取權指派給**清單中，選取**資源**。
  * 從**服務**清單中，選取 **VPC 基礎架構**。
  * 選取您要指派給使用者的平台存取角色。它可以是**管理者**、**編輯者**、**操作員**或**檢視者**。
  * 按一下**邀請使用者**。

### 將 VPC 資源管理許可權提供給現有使用者
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

此情境涵蓋將 VPC 資源編輯許可權提供給您帳戶中現有使用者所需的基本步驟。

在下面的步驟中，您將建立兩個 IAM 原則。需要這兩個原則，使用者才能建立和使用 VPC 基礎架構資源。所有資源都必須位於帳戶的預設資源群組內。

1. 導覽至「IBM Cloud 主控台」中的 [IAM 使用者使用者介面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/iam#/users){: new_window}。
2. 選取您要啟用其授權的使用者。
3. 在**存取原則**標籤下，選取**指派存取權**。
4. 選取**在資源群組內指派存取權**。
5. 選取帳戶的預設資源群組。
6. 確定**指派對資源群組的存取權**選項保持設為**檢視者**。
7. 若要指派正確的服務，請選取 **VPC 基礎架構**。
8. 確定**資源類型**值設為**所有資源類型**。
9. 選取**編輯者**角色。
10. 按一下**指派**。

現在，使用者已獲授權可在帳戶的預設資源群組中建立及使用 VPC 資源。第一個原則（步驟 1-6）容許使用者檢視帳戶的預設資源群組。第二個原則（步驟 7-10）為使用者指派了對 VPC 基礎架構資源的**編輯者**角色，但僅限於存取帳戶的預設資源群組中的資源。

{: tip}

## 檢視使用者的許可權
{: #viewing-your-user-s-permissions}

您可以在使用者的**存取原則**標籤中檢視原則。

您可以使用下列 CLI 指令，透過原則或存取群組來驗證指派給您使用者的資源群組許可權：

**透過原則**
```
ibmcloud iam user-policies <username>
```
**透過存取群組**
```
ibmcloud iam access-groups -u <username>
```

VPC 的 IAM 存取原則變更可能需要 10 分鐘才能生效。
{: note}

## 團隊存取情境
{: #team-access-scenario}

此情境涵蓋帳戶管理者如何指派授權，讓不同的團隊可以存取個別的 VPC 資源。此範例使用_資源群組_ 為兩個團隊設定個別的資源存取權。基於此範例的目的，團隊之間不會共用資源。

此範例會引導您完成此處理程序：建立資源群組、建立存取群組，以及指派適當的原則，讓您的團隊可以存取個別的 VPC 資源。

假設您將兩個不同的專案團隊設定為使用兩個個別的 Virtual Private Cloud。您將存取權指派給團隊成員，讓每個團隊可以存取其團隊的 VPC 資源（僅限）。

* 您的第一個團隊是測試團隊。您決定對該團隊指派 `test_vpcs` 資源群組中 VPC 的存取權。
* 第二個團隊是您的正式作業團隊。它們將獲指派 `production_vpcs` 資源群組中 VPC 的存取權。

此策略可以用來將個別的 VPC 資源指派給任意數目的團隊。不過，所有資源都共用該帳戶的相同 VPC 配額。如需配額及限制的相關資訊，請參閱 [VPC 配額](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas)。
{: tip}

### 步驟 1：建立資源群組
{: #step-1-create-resource-groups}

依預設，帳戶管理者具有建立新資源群組的存取權。其他使用者必須先獲指派**所有帳戶管理服務**的**編輯者**角色，才能建立資源群組。

您的第一項作業是建立將包含每個團隊的 VPC 資源的資源群組。

1. 建立稱為 `test_team` 的資源群組。  
2. 建立稱為 `production_team` 的資源群組。  

如需如何建立資源群組的相關資訊，請參閱[管理資源群組](/docs/resources?topic=resources-rgs)。

### 步驟 2：建立存取群組
{: #step-2-create-access-groups}

可以將資源存取權指派給個別使用者或使用者群組。具有相同存取權的使用者群組稱為_存取群組_。在此情境下，您將建立一個存取群組，以代表需要特定類型之 VPC 存取權的每個團隊成員群組（總計有 4 個唯一存取群組）。

**作業：**建立四個具有下列名稱的存取群組：`test_team_manage_vpcs`、`test_team_view_vpcs`、`production_team_manage_vpcs`、`production_team_view_vpcs`。

如需建立存取群組的說明，請參閱[建立存取群組](/docs/iam?topic=iam-groups#create_ag)。

### 步驟 3：將 IAM 原則新增至您剛才建立的存取群組
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

新增必要的 VPC 存取原則，讓 `test_team` 存取群組可以管理（建立、更新及刪除）VPC 資源。  

1. 導覽至「IBM Cloud 主控台」中的 [IAM 群組使用者介面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/iam#/groups){: new_window}。
2. 選取所需的存取群組（開頭為：`test_team_manage_vpcs`）。
3. 在**存取原則**標籤下，按一下**指派存取權**。
4. 選取**在資源群組內指派存取權**。
5. 選取所需的資源群組（開頭為：`test_team`）
6. 確定**指派對資源群組的存取權**保持設為**檢視者**。
7. 對於服務，選取 **VPC 基礎架構**。
8. 確定**資源類型**保持設為**所有資源類型**。
9. 選取**編輯者**角色。
10. 選取**指派**。

這些步驟還會將**檢視者**存取權指派給 `test_team` 資源群組。需要檢視者存取權才能更新、建立和刪除 `test_team` 資源群組內的資源。

{: tip}

對於其餘三個存取群組，重複先前的步驟。您將完成下列作業，以比對存取群組與資源群組：

* 為 `test_team_view_vpcs` 存取群組指派對 `test_team` 資源群組中 VPC 基礎架構資源的**檢視者**角色。
* 為 `production_team_manage_vpcs` 存取群組指派對 `production_team` 資源群組中 VPC 基礎架構資源的**編輯者**角色。
* 為 `production_team_view_vpcs` 存取群組指派對 `production_team` 資源群組中 VPC 基礎架構資源的**檢視者**角色。

具有 VPC 資源**編輯者**存取權的使用者也可以檢視它們。不需要將成員新增至**編輯者**及**檢視者**存取群組。

#### 設定檢視者存取權
{: #setting-up-viewer-access)

VPC 基礎架構`浮動 IP` 資源在帳戶的預設資源群組中建立。因此，需要管理`浮動 IP` 的使用者需要帳戶的預設資源群組的**檢視者**存取權。
{: tip}

以下說明如何建立該**檢視者**存取權：

1. 導覽至「IBM Cloud 主控台」中的 [IAM 群組使用者介面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/iam#/groups){: new_window}。
2. 選取所需的存取群組（開頭為 `test_team_manage_vpcs`）。
3. 在**存取原則**標籤下，按一下**指派存取權**。
4. 選取**指派對帳戶管理服務的存取權**。
7. 選取服務**所有帳戶管理服務**。
9. 選取**檢視者**角色。
10. 選取**指派**。

重複先前的步驟，以將**所有帳戶管理服務**的**檢視者**角色指派給 `production_team_manage_vpcs` 存取群組。

### 步驟 4：將使用者新增至存取群組
{: #step-4-add-users-to-the-access-groups}

現在，您可以將團隊成員（使用者）指派給適當的存取群組。請遵循下列步驟，將測試團隊的每個成員新增至容許測試團隊 VPC 管理的存取群組：

1. 導覽至「IBM Cloud 主控台」中的 [IAM 群組使用者介面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/iam#/groups){: new_window}。
2. 選取所需的存取群組（開頭為：`test_team_manage_vpcs`）。
3. 在**使用者**標籤下，按一下**新增使用者**。
4. 選取您想要新增至存取群組的每位使用者。
5. 按一下**新增至群組**。

重複先前的步驟，以完成下列作業：
* 將所需的使用者指派給 `test_team_view_vpcs` 存取群組。
* 將所需的使用者指派給 `production_team_manage_vpcs` 存取群組。
* 將所需的使用者指派給 `production_team_view_vpcs` 存取群組。

如果使用者具有管理存取權，則不需要將 VPC 資源的**檢視者**存取權指派給他們。
{: tip}

## 後續步驟
{: #permissions-next-steps}

這兩個範例團隊現在設定為使用 VPC。此時，`test_team_manage_vpcs` 及 `production_team_manage_vpcs` 存取群組的成員可以在其獲指派的資源群組中建立 VPC。


## 相關鏈結
{: #permissions-related-links}

* [管理身分及存取](/docs/iam?topic=iam-getstarted)
* [管理使用者及存取](/docs/iam?topic=iam-iamuserinv)
* [何謂 IAM](/docs/iam?topic=iam-iamoverview)
