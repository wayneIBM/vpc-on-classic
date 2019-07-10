---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# 在 VPC 中建立和管理區塊儲存空間
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud 提供開機儲存空間磁區，並容許使用其他區塊儲存空間磁區，這些磁區是針對虛擬伺服器實例 (VSI) 透過 Hypervisor 裝載的高效能資料儲存空間。透過 VPC 基礎架構，可跨多個地區和區域快速調整儲存空間，並可額外提高效能和安全。

## 性質摘要
{: #summary-of-characteristics}

佈建 {{site.data.keyword.vsi_is_full}} 實例時，會自動建立一個 100 GB 的通用 IOPS (3 IOPS/GB) 區塊儲存空間磁區作為主要開機磁區，並連接到該 VSI。在佈建期間，可以重新命名開機磁區並變更磁區大小。
{:shortdesc}

* 每當在 VPC 網路中佈建虛擬伺服器實例時，都可以建立區塊儲存空間磁區。  
* 可以獨立於 VSI 生命週期建立新磁區，並在以後將這些磁區連接到 VSI。
* 可以建立次要資料磁區，這些磁區會自動連接到 VSI。 
* 資料磁區從 VSI 分離後，會持續保存。因此，您以後可以將磁區連接到新實例。 
* 可以指定刪除 VSI 時是否自動刪除資料磁區。  
* 刪除開機磁區的 VSI 時，會刪除這些開機磁區。
* 依預設，開機磁區和資料磁區使用 IBM 管理的加密進行加密。 
* 在 VSI 佈建期間或建立獨立磁區時，您可以使用自己的加密金鑰對磁區進行加密。


## 區塊儲存空間磁區
{: #block-storage-volumes}

如需使用區塊儲存空間磁區和 VPC 的更多完整資訊，請參閱 [Block Storage for VPC 文件](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)。

如需 VPC 的區塊儲存空間磁區的概觀資訊，請參閱[關於 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about)。 

若要開始獨立於 VSI 佈建來建立磁區，請參閱 [{{site.data.keyword.block_storage_is_short}} 開始使用](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started)。



## 建立和命名 VPC 區塊儲存空間磁區的最佳作法：
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* 在佈建磁區之前確定儲存空間需求。容許提供足夠的容量和 IOPS 效能。
* 決定預先定義的 IOPS 設定檔是否最符合您的容量和效能需求，或者是否指定自訂設定更好。
* 決定是否要在虛擬伺服器實例佈建期間建立次要資料磁區。這些磁區會自動連接到實例。
* 決定是否要在刪除 VSI 時自動刪除連接到該 VSI 的磁區。
* 使用您自己的加密金鑰來加密磁區時，請確保金鑰有效，您有權使用該金鑰，並且可以在現行地區中使用該金鑰。
* 確定您在佈建時為所有磁區命名（包括開機磁區）。
* 確保在連接時對所有磁區連接命名。
* 在帳戶的地區內，每個磁區都必須具有一個不同的名稱。

刪除磁區之後，即可重新使用磁區的名稱。不過，請注意，重複使用磁區名稱可能會導致計費的混淆。
{:note}
