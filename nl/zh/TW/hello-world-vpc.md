---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-29"

keywords: create, VPC, CLI, resources, plugin, SSH, key, hello, world, provision, instance, subnet

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 使用 IBM Cloud CLI 建立 VPC
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (鏈結的說明主題)

本文件說明如何使用 IBM Cloud CLI 來建立 {{site.data.keyword.cloud}} Virtual Private Cloud 資源。

## 必要條件：
{: #cli-pre-requisites}

1. 安裝 [IBM Cloud CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview)。

2. 將 `vpc-infrastructure` 外掛程式安裝到 IBM Cloud CLI 或更新該外掛程式。

  若要安裝，請執行下列指令：

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  若要更新，請執行下列指令：

  ```
  ibmcloud plugin update
  ```
  {: pre}

  若要檢視已安裝的外掛程式及版本，請執行下列指令：

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. 產生用來佈建「虛擬伺服器實例 (VSI)」的公開 SSH 金鑰。

您可能已有公開 SSH 金鑰。在起始目錄的 ``.ssh`` 目錄下尋找稱為 ``id_rsa.pub`` 的檔案，例如，``/Users/<USERNAME>/.ssh/id_rsa.pub``。檔案的開頭為 ``ssh-rsa``，結尾為電子郵件位址。

如果您沒有公開 SSH 金鑰，或忘記現有 SSH 金鑰的密碼，請執行 ``ssh-keygen`` 指令並遵循提示來產生新的 SSH 金鑰。


## 步驟 1：登入 IBM Cloud。
{: #step-1-log-in-to-ibm-cloud}

如果您具有聯合帳戶，請執行下列指令：
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

否則，請執行下列指令：

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

在登入程序中，系統會提示您選擇地區。VPC 不一定在所有地區都可用。請參閱[在不同地區中建立 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)，以查看目前可用的地區。
{: important}

## 步驟 2：設定世代目標。

使用下列指令來設定世代目標。

```
ibmcloud is target --gen 1
```
{: pre}


## 步驟 3：建立 VPC 並儲存 VPC ID。
{: #step-3-create-a-vpc-and-save-the-vpc-id}

使用下列指令，以建立名為 _helloworld-vpc_ 的 VPC。

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

您應該會看到如下的輸出：

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

將 ID 儲存在變數中，以供稍後使用，例如：

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## 步驟 4：建立子網路並儲存子網路 ID。
{: #step-4-create-a-subnet-and-save-the-subnet-id}

請挑選子網路位置的 `us-south-2` 區域，並將子網路稱為 _helloworld-subnet_。

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

您應該會看到如下的輸出：

```
Creating Subnet helloworld-subnet in resource group Default under account <Account Name> as user <User>...

ID               50ba0da9-279a-4791-b7cb-cd3d7b2bc14d   
Name             helloworld-subnet   
IPv*             ipv4   
IPv4 CIDR        10.240.64.0/29  
IPv6 CIDR        -   
Addr available   3   
Addr Total       8   
Gen              -   
ACL              allow-all-network-acl-ba9e785a-3e10-418a-811c-56cfe5669676(e9c2790b-cee2-465a-8539-d8cd90d33621)   
Gateway          -   
Created          now   
Status           pending   
Zone             us-south-2   
VPC              helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)   
```
{:screen}

將 ID 儲存在變數中，以供稍後使用，例如：

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

請注意，第一次建立子網路時，其狀態為 `pending`。在您繼續之前，子網路需要變成 `available` 狀態，這需要幾秒鐘。若要檢查子網路的狀態，請執行以下指令：

```
ibmcloud is subnet $subnet
```
{: pre}

## 步驟 5：連接公用閘道。
{: #step-5-attach-a-public-gateway}

將公用閘道連接至子網路，以容許所有連接的資源與公用網際網路通訊。

若要建立公用閘道，請執行下列指令：

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

將該 ID 儲存在變數中，以便稍後可以使用，例如：

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

若要將公用閘道連接到子網路，請執行下列指令：
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


在 VPC 中，每個區域只容許有一個公用閘道，但該公用閘道可以連接到所在區域中的多個子網路。若要尋找區域的公用閘道，請執行 `ibmcloud is public-gateways` 指令，然後尋找特定的 "VPC" 和 "Zone" 值。
{: tip}

您應該會看到如下的輸出：

```
Updating Subnet f174ca4d-cb44-4e4d-a0f3-afce151f4198 under account Account as user ...

ID               f174ca4d-cb44-4e4d-a0f3-afce151f4198   
Name             helloworld-subnet   
IPv*             ipv4   
IPv4 CIDR        10.240.64.0/29   
IPv6 CIDR        -   
Addr available   3   
Addr Total       8   
ACL              allow-all-network-acl-6dcd8188-f1e2-48ca-b872-d04cf54479c1(d589e5dc-3b95-4984-9191-174eed7d1f98)   
Gateway          my-gateway(446c0c63-f0b1-4043-b30d-644f55fde391)   
Created          41 minutes ago   
Status           available   
Zone             us-south-2   
VPC              helloworld-vpc(6dcd8188-f1e2-48ca-b872-d04cf54479c1)
```
{: screen}

## 步驟 6：在 IBM Public Cloud 中建立 SSH 金鑰。
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

您將使用金鑰來佈建虛擬伺服器實例。您可以使用相同的金鑰來佈建多個虛擬伺服器實例。

只能在一開始建立 VSI 的過程中新增金鑰。沒有任何工具可供之後再新增金鑰。
{:tip}

若要查看 IBM Cloud 帳戶中的可用金鑰，請執行此指令：

```
ibmcloud is keys
```
{: pre}

若要建立金鑰，請執行下列指令。替換 `id_rsa.pub` 檔案的路徑。

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

您應該會看到如下的輸出：

```
Creating key helloworld-key in resource group Default under account <Account Name> as user <User>...

ID               859b4e97-7540-4337-9c64-384792b85653   
Name             helloworld-key   
Type             rsa   
Length           2048   
FingerPrint      SHA256:hkcAOGB5z7QXqZLHd0kGqhj735qrfMjZLH9PxS/42vA   
Key              ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9i2joQ8eiFVdJ7pOlC6h5+IoBL6wFFygkk9Na3gV8Bi52dv44YOAjSJ2oduguHEtLp5r4eh4+5jiEBkFYkHNUhE0MxlcVZABTYWePXx4QnlmGr99xyOfi6DAHhSRQiSBhmhjcGjbADavDuIgoyKpVXbU9O1If3P0miNpaouaZmr+d68OHt4yPvNnztlluV3JBISJgqJ7pzg6wFF0SrjqtEYKBd8oxwogHu+rmRgT7IF09oSiKpKZRF0VfeLFz+REh9RuKa4Jh63aa2PRIgDKq6HO7MEdfOtGzCzoqqlFKgpl+VgGyT0b5BjQEnWv13cwx02bv5QCwma/GeAOpW0CD user@email.com   

Created          now   
```
{:screen}

將 ID 儲存在變數中，以供稍後使用，例如：

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## 步驟 7：為虛擬伺服器實例選取設定檔和映像檔。
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

若要列出所有可用的實例設定檔，請執行下列指令：

```
ibmcloud is instance-profiles
```
{: pre}

若要列出所有可用的映像檔，請執行下列指令：

```
ibmcloud is images
```
{: pre}

假設選取實例設定檔 `bc1-2x8` 和映像檔 `ubuntu-16.04-amd64`。若要取得 `ubuntu-16.04-amd64` 的映像檔 ID，請執行下列指令：

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## 步驟 8：佈建虛擬伺服器實例。
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

金鑰只能在一開始建立 VSI 時新增。沒有任何工具可供之後再新增金鑰。
{:tip}

您應該會看到如下的輸出：

```
Creating instance helloworld-vsi in resource group Default under account <Account Name> as user <User>...

ID                4562c5c0-9cf7-4406-bc90-ab4baea91057   
Name              helloworld-vsi   
Gen                  
Profile           bc1-2x8   
CPU Arch          amd64   
CPU Cores         2   
CPU Frequency     2000   
Memory            8   
Primary Intf      primary(2e850924-b5d7-4386-a778-03556d5850c1)   
Primary Address   10.240.64.4  
Image             ubuntu-16.04-amd64(7eb4e35b-4257-56f8-d7da-326d85452591)   
Status            pending   
Created           5 seconds ago   
VPC               helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)   
Zone              us-south-2   
```
{:screen}

將實例 ID 儲存在變數中，以便稍後可以使用：

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

此外，將主要網路介面 `Primary Intf` 的 ID 儲存在變數中，以便稍後可以使用：

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

請注意，第一次建立實例時，其狀態為 `pending`。在您繼續之前，實例需要變成 `running` 狀態，這需要幾分鐘。若要檢查實例的狀態，請執行以下指令：

```
ibmcloud is instance $vsi
```
{: pre}

## 步驟 9：建立浮動 IP 位址。
{: #step-9-create-a-floating-ip-address}

您需要有「浮動 IP 位址」，才能從網際網路登入虛擬伺服器實例 (VSI)。

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

您應該會看到如下的輸出：

```
Creating floating ip helloworld-fip in resource group Default under account <Account Name> as user <User>...

ID               b9d1cc1f-67db-40e3-81de-9228465170a5   
Address          169.61.181.53   
Name             helloworld-fip   
Target           primary(2e850924-.)   
Target Type      intf   
Target IP        10.0.1.5   
Created          now   
Status           available   
Zone             us-south-2   
```
{:screen}

將浮動 IP 位址的 ID 和`位址`儲存在變數中，以便稍後可以使用：

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## 步驟 10：對預設安全群組新增用於 SSH 的規則。
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

尋找 VPC 的安全群組：

```
ibmcloud is vpc-sg $vpc
```
{: pre}

您應該會看到如下的輸出：
```
Getting default security group of vpc ba9e785a-3e10-418a-811c-56cfe5669676 under account <Account Name> as user <User>...

ID               2d364f0a-a870-42c3-a554-000000981149
Name             errand-drastic-imperial-retail-unlocked-jester
Created          1 week ago
VPC              helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)
Resource Group   -
Tags             -

Rules
ID                                     Direction   IPv*   Protocol                  Remote
b597cff2-38e8-4e6e-999d-000002031345   inbound     ipv4   all                       errand-drastic-imperial-retail-unlocked-jester(2d364f0a-.)
b597cff2-38e8-4e6e-999d-000002031527   outbound    ipv4   all                       -

```
{:screen}

將 ID 儲存在變數中，以供稍後使用：

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

現在，請建立規則來容許 SSH：

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

您應該會看到如下的輸出：

```
Creating rule for security group 2d364f0a-a870-42c3-a554-000000981149 under account <Account Name> as user <User>...

ID          b597cff2-38e8-4e6e-999d-000001949921
Direction   inbound
IPv*        ipv4
Protocol    tcp
Min Port    22
Max Port    22
Remote      -
```
{:screen}

## 步驟 11：建立區塊儲存空間資料磁區。
{: #step-11-create-a-block-storage-data-volume}

執行此指令以建立區塊儲存空間資料磁區。指定磁區的名稱、磁區設定檔以及要在其中建立磁區的區域。若要將區塊儲存空間資料磁區連接到實例，實例和區塊儲存空間資料磁區必須在相同區域中建立。

若要查看磁區設定檔的清單，請執行以下指令：

```
ibmcloud is volume-profiles
```
{: pre}

設定檔可以是 general-purpose (3 IOPS/GB)、5iops-tier、10iops-tier 和 custom。如需根據您所選磁區設定檔的磁區容量及 IOPS 範圍相關資訊，請參閱[關於 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)。  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

您應該會看到如下的輸出：

```
Creating volume helloworld-vol in resource group Default under account <Account Name> as user <User>...

ID                                      933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    helloworld-vol
Capacity                                100
IOPS                                    1000
Auto delete                             false
Encryption                              provider managed
Profile                                 custom
Status                                  pending
Created                                 2019-02-15 10:09:28
Zone                                    us-south-2
Volume Attachment Instance Reference    none
```
{:screen}

將磁區 ID 儲存在變數中，以便稍後可以使用：

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

請注意，首次建立磁區時，其狀態為 `pending`。磁區需要變成 `available` 狀態（這需要幾分鐘時間）後，您才能繼續操作。

若要檢查磁區的狀態，請執行以下指令：

```
ibmcloud is volume $vol
```
{: pre}

## 步驟 12：將區塊儲存空間資料磁區連接到實例。
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

使用下列指令透過已建立的變數，將磁區連接到虛擬伺服器實例：

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## 步驟 13：使用專用 SSH 金鑰登入到虛擬伺服器實例。
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

例如，您可以使用下列形式的指令：

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

您會收到類似下列範例的回應。系統提示您繼續連接時，請鍵入 `yes`。

```
The authenticity of host '169.61.181.53 (169.61.181.53)' can't be established.
ECDSA key fingerprint is SHA256:9MczXIwJq892DYwu0sZpQZOXORdvNXeP1aFioZpQDsM.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '169.61.181.53' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.4.0-133-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@helloworld-vsi:~#
```
{:screen}

## 步驟 14：Hello, World!
{: #step-14-hello-world}

在終端機視窗中，執行下列指令：

```
echo `hostname` says "Hello, World!"
```
{:pre}

您應該會看到下列輸出：

```
helloworld-vsi says Hello, World!
```
{:screen}

## 步驟 15：（選用）開始使用區塊儲存空間資料磁區
{: #step-15-optional-start-using-your-block-storage-data-volume}

若要將區塊儲存空間磁區用作檔案系統，您需要分割磁區，格式化磁區，然後將其作為檔案系統進行裝載。

在 Linux 上，執行下列指令來列出實例中的所有區塊儲存空間磁區：

```
lsblk
```
{:pre}

您應該會看到如下的輸出：

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

磁區 `xvdc` 是新的區塊儲存空間資料磁區。

執行下列指令來分割磁區。首先，對新分割區使用 `n` 指令。

```
fdisk /dev/xvdc
```
{:pre}

格式化磁區。

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

將磁區標記為 "hellovol" 。

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

建立目錄，然後將磁區作為檔案系統進行裝載。

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

若要查看新的檔案系統，請執行下列指令：

```
df -k
```
{:pre}

您應該會看到如下的輸出：

```
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4075344       0   4075344   0% /dev
tmpfs             816936    8844    808092   2% /run
/dev/xvda2     101330012 1261048 100052580   2% /
tmpfs            4084664       0   4084664   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4084664       0   4084664   0% /sys/fs/cgroup
/dev/xvda1        245679   64360    168212  28% /boot
tmpfs             817040       0    817040   0% /run/user/0
/dev/xvdc      103081248   61176  97777192   1% /hellovol
```
{:screen}

若要將目錄切換到新的檔案系統並建立檔案，請執行如下的指令：

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## 步驟 15：（選用）刪除資源
{: #step-15-delete-vpc-cli}

選擇性地刪除資源。如果資源包含其他資源，則無法予以刪除。例如，如果虛擬專用雲端包含子網路，則無法予以刪除，而且，如果子網路包含虛擬伺服器實例，則無法予以刪除。進行刪除作業時，API 可能很快即會返回，但可能仍在進行資源刪除。發出刪除要求之後，請確定已刪除資源，然後才嘗試刪除母項資源。如需詳細資料，請參閱[刪除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)。

### 釋放浮動 IP

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### 刪除虛擬伺服器實例

```
ibmcloud is instance-delete $vsi
```
{: pre}

### 刪除金鑰

```
ibmcloud is key-delete $key
```
{: pre}

### 刪除子網路

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### 刪除公用閘道

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### 刪除 VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### 刪除磁區（如果未設定為自動刪除）

```
ibmcloud is volume-delete $vol
```
{: pre}

## 恭喜！
{: #cli-congratulations}

您已使用 IBM Cloud CLI 順利佈建及連接至 Virtual Private Cloud 實例。若要嘗試其他 CLI 指令，請探索完整參考資料：

* [VPC 的 CLI 參考資料](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
