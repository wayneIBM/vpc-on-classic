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

# 使用 IBM Cloud CLI 创建 VPC
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (链接的帮助主题)

本文档说明了如何使用 IBM Cloud CLI 来创建 {{site.data.keyword.cloud}} Virtual Private Cloud 资源。

## 先决条件：
{: #cli-pre-requisites}

1. 安装 [IBM Cloud CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview)。

2. 将 `vpc-infrastructure` 插件安装到 IBM Cloud CLI 或更新该插件。

  安装该插件：

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  更新该插件：

  ```
  ibmcloud plugin update
  ```
  {: pre}

  查看安装的插件和版本：

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. 生成公用 SSH 密钥以供应虚拟服务器实例 (VSI)。

您可能已有公用 SSH 密钥。在主目录的 ``.ssh`` 目录下查找名为 ``id_rsa.pub`` 的文件，例如 ``/Users/<USERNAME>/.ssh/id_rsa.pub``。该文件以 ``ssh-rsa`` 开头，以您的电子邮件地址结尾。

如果您没有公用 SSH 密钥，或者如果忘记了现有 SSH 密钥的密码，请通过运行 ``ssh-keygen`` 命令并遵循提示来生成新密钥。


## 步骤 1：登录到 IBM Cloud。
{: #step-1-log-in-to-ibm-cloud}

如果您有联合帐户，请运行以下命令：
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

否则，请运行以下命令：

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

在登录过程中，系统会提示您选择区域。VPC 不一定在所有区域都可用。请参阅[在不同区域中创建 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)，以查看目前可用的区域。
{: important}

## 步骤 2：设置生成目标。

使用以下命令来设置生成目标。

```
ibmcloud is target --gen 1
```
{: pre}


## 步骤 3：创建 VPC 并保存 VPC 标识。
{: #step-3-create-a-vpc-and-save-the-vpc-id}

使用以下命令来创建名为 _helloworld-vpc_ 的 VPC。

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

您应该会看到输出类似于以下内容：

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

将该标识保存在变量中，以便稍后可以使用，例如：

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## 步骤 4：创建子网并保存子网标识。
{: #step-4-create-a-subnet-and-save-the-subnet-id}

假设选取 `us-south-2` 专区作为子网位置，然后将子网命名为 _helloworld-subnet_。

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

您应该会看到输出类似于以下内容：

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

将该标识保存在变量中，以便稍后可以使用，例如：

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

请注意，子网创建之初，其状态为 `pending`。子网的状态需要移至 `available` 后（这需要几秒钟时间），您才能继续操作。要检查子网的状态，请运行以下命令：

```
ibmcloud is subnet $subnet
```
{: pre}

## 步骤 5：连接公共网关。
{: #step-5-attach-a-public-gateway}

将公共网关连接到子网，以允许所有连接的资源与公用因特网进行通信。

要创建公共网关，请运行以下命令：

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

将该标识保存在变量中，以便稍后可以使用，例如：

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

要将公共网关连接到子网，请运行以下命令：
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


在 VPC 中，每个专区只允许有一个公共网关，但该公共网关可以连接到所在专区中的多个子网。要查找专区的公共网关，请运行 `ibmcloud is public-gateways` 命令，然后查找特定的“VPC”和“Zone”值。
{: tip}

您应该会看到输出类似于以下内容：

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

## 步骤 6：在 IBM Public Cloud 中创建 SSH 密钥。
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

您将使用此密钥来供应虚拟服务器实例。可以使用同一密钥来供应多个虚拟服务器实例。

只能在创建 VSI 的过程中初始添加密钥。以后没有任何工具可用于添加密钥。
{:tip}

要查看 IBM Cloud 帐户中的可用密钥，请运行以下命令：

```
ibmcloud is keys
```
{: pre}

要创建密钥，请运行以下命令。将路径替换为您的 `id_rsa.pub` 文件。

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

您应该会看到输出类似于以下内容：

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

将该标识保存在变量中，以便稍后可以使用，例如：

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## 步骤 7：为虚拟服务器实例选择概要文件和映像。
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

要列出所有可用的实例概要文件，请运行以下命令：

```
ibmcloud is instance-profiles
```
{: pre}

要列出所有可用映像，请运行以下命令：

```
ibmcloud is images
```
{: pre}

假设选取实例概要文件 `bc1-2x8` 和映像 `ubuntu-16.04-amd64`。要获取 `ubuntu-16.04-amd64` 的映像标识，请运行以下命令：

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## 步骤 8：供应虚拟服务器实例。
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

只能在创建 VSI 的过程中初始添加密钥。以后没有任何工具可用于添加密钥。
{:tip}

您应该会看到输出类似于以下内容：

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

将实例标识保存在变量中，以便稍后可以使用：

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

此外，将主网络接口 `Primary Intf` 的标识保存在变量中，以便稍后可以使用：

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

请注意，实例创建之初，其状态为 `pending`。实例的状态需要移至 `running` 后（这需要几秒钟时间），您才能继续操作。要检查实例的状态，请运行以下命令：

```
ibmcloud is instance $vsi
```
{: pre}

## 步骤 9：创建浮动 IP 地址。
{: #step-9-create-a-floating-ip-address}

您需要一个浮动 IP 地址，以便可以从因特网登录到虚拟服务器实例 (VSI)。

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

您应该会看到输出类似于以下内容：

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

将浮动 IP 地址的标识和`地址`保存在变量中，以便稍后可以使用：

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## 步骤 10：向缺省安全组添加用于 SSH 的规则。
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

查找 VPC 的安全组：

```
ibmcloud is vpc-sg $vpc
```
{: pre}

您应该会看到输出类似于以下内容：
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

将该标识保存在变量中，以便稍后可以使用：

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

现在，创建允许 SSH 的规则：

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

您应该会看到输出类似于以下内容：

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

## 步骤 11：创建块存储器数据卷。
{: #step-11-create-a-block-storage-data-volume}

运行此命令以创建块存储器数据卷。指定卷的名称、卷概要文件以及要在其中创建卷的专区。要将块存储器数据卷连接到实例，实例和块存储器数据卷必须在同一专区中进行创建。

要查看卷概要文件的列表，请运行以下命令：

```
ibmcloud is volume-profiles
```
{: pre}

概要文件可以是 general-purpose (3 IOPS/GB)、5iops-tier、10iops-tier 和 custom。有关基于所选卷概要文件的卷容量和 IOPS 范围的信息，请参阅[关于 Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)。  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

您将看到输出类似于以下内容：

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

将卷标识保存在变量中，以便稍后可以使用：

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

请注意，卷创建之初，其状态为 `pending`。卷需要移至 `available` 状态（这需要几分钟时间）后，您才能继续操作。

要检查卷的状态，请运行以下命令：

```
ibmcloud is volume $vol
```
{: pre}

## 步骤 12：将块存储器数据卷连接到实例。
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

使用以下命令通过已创建的变量，将卷连接到虚拟服务器实例：

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## 步骤 13：使用专用 SSH 密钥登录到虚拟服务器实例。
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

例如，可以使用以下格式的命令：

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

您将收到类似于以下示例的响应。系统提示您继续连接时，请输入 `yes`。

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

## 步骤 14：Hello, World!
{: #step-14-hello-world}

在终端窗口中运行以下命令：

```
echo `hostname` says "Hello, World!"
```
{:pre}

您应该会看到以下输出：

```
helloworld-vsi says Hello, World!
```
{:screen}

## 步骤 15：（可选）开始使用块存储器数据卷
{: #step-15-optional-start-using-your-block-storage-data-volume}

要将块存储卷用作文件系统，您需要对卷分区，格式化卷，然后将其作为文件系统进行安装。

在 Linux 上，运行以下命令来列出实例中的所有块存储卷：

```
lsblk
```
{:pre}

您应该会看到输出类似于以下内容：

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

卷 `xvdc` 是新的块存储器数据卷。

运行以下命令来对卷分区。首先，对新分区使用 `n` 命令。

```
fdisk /dev/xvdc
```
{:pre}

对卷进行格式化。

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

将卷标记为“hellovol”。

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

创建目录，然后将卷作为文件系统进行安装。

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

要查看新的文件系统，请运行以下命令：

```
df -k
```
{:pre}

您应该会看到输出类似于以下内容：

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

要将目录切换到新的文件系统并创建文件，请执行类似下面的命令：

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## 步骤 15：（可选）删除资源
{: #step-15-delete-vpc-cli}

（可选）删除资源。如果某个资源包含其他资源，那么无法删除该资源。例如，如果虚拟私有云包含子网，那么无法删除该虚拟私有云；如果子网包含虚拟服务器实例，那么无法删除该子网。执行删除操作时，API 可能会快速返回，但资源删除操作可能仍在进行中。发出删除请求后，请确保资源已删除，然后再尝试删除父资源。有关更多详细信息，请参阅[删除 VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)。

### 释放浮动 IP

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### 删除虚拟服务器实例

```
ibmcloud is instance-delete $vsi
```
{: pre}

### 删除密钥

```
ibmcloud is key-delete $key
```
{: pre}

### 删除子网

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### 删除公共网关

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### 删除 VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### 删除卷（如果未设置为自动删除）

```
ibmcloud is volume-delete $vol
```
{: pre}

## 祝贺您！
{: #cli-congratulations}

您已使用 IBM Cloud CLI 成功供应并连接到虚拟私有云实例。要试用更多 CLI 命令，请探索完整的参考：

* [VPC 的 CLI 参考](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
