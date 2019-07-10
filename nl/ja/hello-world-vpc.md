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

# IBM Cloud CLI を使用した VPC の作成
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (リンクされたヘルプ・トピック)

この資料では、IBM Cloud CLI を使用して {{site.data.keyword.cloud}} 仮想プライベート・クラウドのリソースを作成する方法について説明します。

## 前提条件:
{: #cli-pre-requisites}

1. [IBM Cloud CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview) をインストールします。

2. `vpc-infrastructure` プラグインを IBM Cloud CLI にインストールまたは更新します。

  インストールするには、次のようにします。

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  更新するには、次のようにします。

  ```
  ibmcloud plugin update
  ```
  {: pre}

  インストールされているプラグインとバージョンを表示するには、以下のようにします。

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. 公開 SSH 鍵を生成して、仮想サーバー・インスタンス (VSI) をプロビジョンします。

公開 SSH 鍵が既に存在している可能性があります。 ホーム・ディレクトリーの ``.ssh`` ディレクトリー内で ``id_rsa.pub`` というファイル (例: ``/Users/<USERNAME>/.ssh/id_rsa.pub``) を探します。 このファイルは ``ssh-rsa`` で始まり、E メール・アドレスで終わります。

公開 SSH 鍵がない場合、または既存の SSH 鍵のパスワードを忘れた場合は、``ssh-keygen`` コマンドを実行し、プロンプトの指示に従って新しい鍵を生成します。


## ステップ 1: IBM Cloud にログインする
{: #step-1-log-in-to-ibm-cloud}

統合アカウントの場合:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

統合アカウントでない場合:

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

ログイン・プロセス中のログイン時に、地域を選択するようプロンプトが出されます。地域によっては、VPC を利用できない場合があります。現在利用可能な地域については、[さまざまな地域での VPC の作成](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)を参照してください。
{: important}

## ステップ 2: 生成ターゲットを設定する

次のコマンドを使用して、生成ターゲットを設定します。

```
ibmcloud is target --gen 1
```
{: pre}


## ステップ 3: VPC を作成して VPC ID を保存する
{: #step-3-create-a-vpc-and-save-the-vpc-id}

次のコマンドを使用して、_helloworld-vpc_ という名前の VPC を作成します。

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

次のような出力が表示されます。

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

後で利用できるように、ID を変数に保存します。次に例を示します。

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## ステップ 4: サブネットを作成してサブネット ID を保存する
{: #step-4-create-a-subnet-and-save-the-subnet-id}

サブネットの場所として `us-south-2` ゾーンを選択し、サブネットの名前を _helloworld-subnet_ とします。

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

次のような出力が表示されます。

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

後で利用できるように、ID を変数に保存します。次に例を示します。

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

サブネットを初めて作成する場合、サブネットの状況が `pending` であることに注意してください。 続行する前に、サブネットの状況が `available` になる必要があります。これには数秒かかります。 サブネットの状況を確認するには、次のコマンドを実行します。

```
ibmcloud is subnet $subnet
```
{: pre}

## ステップ 5: パブリック・ゲートウェイを接続する
{: #step-5-attach-a-public-gateway}

パブリック・ゲートウェイをサブネットに接続して、すべての接続リソースがパブリック・インターネットと通信できるようにします。

パブリック・ゲートウェイを作成するには、次のコマンドを実行します。

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

後で利用できるように、ID を変数に保存します。次に例を示します。

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

パブリック・ゲートウェイをサブネットに接続するには、次のコマンドを実行します。
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


1 つの VPC 内でゾーンあたり 1 つのパブリック・ゲートウェイのみが許可されていますが、そのパブリック・ゲートウェイをゾーン内の複数のサブネットに接続できます。ゾーンのパブリック・ゲートウェイを見つけるには、'ibmcloud is public-gateways` コマンドを実行し、VPC とゾーンの特定の値を探してください。
{: tip}

次のような出力が表示されます。

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

## ステップ 6: IBM Public Cloud で SSH 鍵を作成する
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

この鍵は、仮想サーバー・インスタンスをプロビジョンする際に使用します。 同じ鍵を使用して、複数の仮想サーバー・インスタンスをプロビジョンできます。

鍵は最初に VSI の作成の一環としてのみ追加できます。後で鍵を追加する方法はありません。
{:tip}

IBM Cloud アカウントで使用可能な鍵を確認するには、次のコマンドを実行します。

```
ibmcloud is keys
```
{: pre}

鍵を作成するには、次のコマンドを実行します。 ご使用の `id_rsa.pub` ファイルのパスに置き換えてください。

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

次のような出力が表示されます。

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

後で利用できるように、ID を変数に保存します。次に例を示します。

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## ステップ 7: 仮想サーバー・インスタンスのプロファイルとイメージを選択する
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

使用可能なインスタンス・プロファイルをすべてリストするには、次のコマンドを実行します。

```
ibmcloud is instance-profiles
```
{: pre}

使用可能なイメージをすべてリストするには、次のコマンドを実行します。

```
ibmcloud is images
```
{: pre}

インスタンス・プロファイル `bc1-2x8` とイメージ `ubuntu-16.04-amd64` を選択します。`ubuntu-16.04-amd64` のイメージ ID を取得するには、次のコマンドを実行します。

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## ステップ 8: 仮想サーバー・インスタンスをプロビジョンする
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

鍵は最初に VSI の作成の一環としてのみ追加できます。後で鍵を追加する方法はありません。
{:tip}

次のような出力が表示されます。

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

後で使用できるように、インスタンスの ID を変数に保存します。

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

また、後で利用できるように、1 次ネットワーク・インターフェース `Primary Intf` の ID も変数に保存します。

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

インスタンスを初めて作成する場合、インスタンスの状況が `pending` であることに注意してください。 続行する前に、インスタンスの状況が `running` になる必要があります。これには数分かかります。 インスタンスの状況を確認するには、次のコマンドを実行します。

```
ibmcloud is instance $vsi
```
{: pre}

## ステップ 9: 浮動 IP アドレスを作成する
{: #step-9-create-a-floating-ip-address}

浮動 IP アドレスは、インターネットから仮想サーバー・インスタンス (VSI) にログインする際に必要です。

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

次のような出力が表示されます。

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

後で使用できるように、浮動 IP の ID と `Address` を変数に保存します。

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## ステップ 10: SSH のデフォルト・セキュリティー・グループにルールを追加する
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

VPC のセキュリティー・グループを見つけます。

```
ibmcloud is vpc-sg $vpc
```
{: pre}

次のような出力が表示されます。
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

後で使用できるように、ID を変数に保存します。

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

ここで、SSH を許可するルールを作成します。

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

次のような出力が表示されます。

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

## ステップ 11: ブロック・ストレージのデータ・ボリュームを作成する
{: #step-11-create-a-block-storage-data-volume}

このコマンドを実行して、ブロック・ストレージのデータ・ボリュームを作成します。ボリュームの名前、ボリューム・プロファイル、およびボリュームを作成するゾーンを指定します。ブロック・ストレージ・データ・ボリュームをインスタンスに接続するには、インスタンスとブロック・ストレージ・データ・ボリュームを同じゾーンに作成する必要があります。

ボリューム・プロファイルのリストを表示するには、以下を実行します。

```
ibmcloud is volume-profiles
```
{: pre}

プロファイルには、general-purpose (3 IOPS/GB)、5iops-tier、10-iops-tier、custom を指定できます。
選択したボリューム・プロファイルに基づくボリューム容量と IOPS 範囲については、[VPC のブロック・ストレージについて](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)を参照してください。
  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

次のような出力が表示されます。

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

後で使用できるように、ボリュームの ID を変数に保存します。

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

ボリュームを初めて作成する場合、ボリュームの状況は `pending` であることに注意してください。続行する前に、ボリュームの状況が `available` になる必要があります。これには数分かかります。

ボリュームの状況を確認するには、次のコマンドを実行します。

```
ibmcloud is volume $vol
```
{: pre}

## ステップ 12: ブロック・ストレージ・データ・ボリュームをインスタンスに接続する
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

次のコマンドを使用し、ボリュームを仮想サーバー・インスタンスに接続します。その際、作成した変数を使用します。

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## ステップ 13: SSH 秘密鍵を使用して仮想サーバー・インスタンスにログインする
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

例えば、次の形式のコマンドを使用できます。

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

次の例のような応答を受け取ります。 接続の継続を求めるプロンプトが出されたら、`yes` と入力します。

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

## ステップ 14: Hello, World!
{: #step-14-hello-world}

端末ウィンドウで次のコマンドを実行します。

```
echo `hostname` says "Hello, World!"
```
{:pre}

次のような出力が表示されます。

```
helloworld-vsi says Hello, World!
```
{:screen}

## ステップ 15: (オプション) ブロック・ストレージ・データ・ボリュームの使用を開始する
{: #step-15-optional-start-using-your-block-storage-data-volume}

ブロック・ストレージ・ボリュームをファイル・システムとして使用するには、ボリュームをパーティション化し、ボリュームをフォーマットしてから、ファイル・システムとしてマウントする必要があります。

Linux では、次のコマンドを実行して、インスタンスからすべてのブロック・ストレージ・ボリュームをリストします。

```
lsblk
```
{:pre}

次のような出力が表示されます。

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

ボリューム `xvdc` は、新規ブロック・ストレージ・データ・ボリュームです。

次のコマンドを実行して、ボリュームをパーティション化します。まず、新規パーティションに対してコマンド `n` を使用します。

```
fdisk /dev/xvdc
```
{:pre}

ボリュームをフォーマットします。

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

ボリュームに "hellovol" というラベルを設定します。

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

ディレクトリーを作成し、ボリュームをファイル・システムとしてマウントします。

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

新規ファイル・システムを表示するには、次のコマンドを実行します。

```
df -k
```
{:pre}

次のような出力が表示されます。

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

ディレクトリーを新規ファイル・システムに変更してファイルを作成するには、次のようにします。

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## ステップ 15 (オプション) リソースを削除する
{: #step-15-delete-vpc-cli}

オプションで、リソースを削除します。他のリソースが含まれているリソースを削除することはできません。 例えば、サブネットが含まれている仮想プライベート・クラウドを削除することはできず、仮想サーバー・インスタンスが含まれているサブネットを削除することはできません。削除操作を行うと、API は素早く復帰する可能性がありますが、リソースの削除自体はまだ進行中である可能性があります。削除要求を出した後、親リソースの削除を試行する前に、リソースが削除されていることを確認してください。詳しくは、[VPC の削除](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)を参照してください。

### 浮動 IP の解放

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### 仮想サーバー・インスタンスの削除

```
ibmcloud is instance-delete $vsi
```
{: pre}

### 鍵の削除

```
ibmcloud is key-delete $key
```
{: pre}

### サブネットの削除

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### パブリック・ゲートウェイの削除

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### VPC の削除

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### ボリュームの削除 (自動削除に設定していない場合)

```
ibmcloud is volume-delete $vol
```
{: pre}

## これで完了です。
{: #cli-congratulations}

IBM Cloud CLI を使用して、仮想プライベート・クラウド・インスタンスを正常にプロビジョンして接続しました。 他の CLI コマンドを試すには、以下の詳しいリファレンスを参照してください。

* [VPC の CLI リファレンス](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
