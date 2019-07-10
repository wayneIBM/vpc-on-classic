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

# IBM Cloud CLI를 사용하여 VPC 작성
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (링크된 도움말 항목)

이 문서에서는 IBM Cloud CLI를 사용하여 {{site.data.keyword.cloud}} Virtual Private Cloud 리소스를 작성하는 방법을 보여줍니다.

## 전제조건:
{: #cli-pre-requisites}

1. [IBM Cloud CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview)를 설치하십시오.

2. IBM Cloud CLI에 `vpc-infrastructure` 플러그인을 설치하거나 업데이트하십시오. 

  설치하려는 경우에는 다음 명령을 실행하십시오.

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  업데이트하려는 경우에는 다음 명령을 실행하십시오.

  ```
  ibmcloud plugin update
  ```
  {: pre}

  설치된 플러그인 및 버전을 보려면 다음 명령을 실행하십시오.

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. 가상 서버 인스턴스(VSI)를 프로비저닝하는 데 필요한 공개 SSH 키를 생성하십시오.

공개 SSH 키를 이미 보유하고 있는 경우가 있습니다. 홈 디렉토리 아래의 ``.ssh`` 디렉토리에서 ``id_rsa.pub``라는 파일을 찾으십시오(예: ``/Users/<USERNAME>/.ssh/id_rsa.pub``). 이 파일은 ``ssh-rsa``로 시작하며 사용자의 이메일 주소로 끝납니다.

공개 SSH 키가 없거나 기존 키의 비밀번호를 잊어버린 경우에는 ``ssh-keygen`` 명령을 실행한 후 프롬프트에 따라 새 키를 생성하십시오.


## 1단계: IBM Cloud에 로그인
{: #step-1-log-in-to-ibm-cloud}

연합 계정이 있는 경우에는 다음 명령을 실행하십시오.
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

그렇지 않은 경우에는 다음 명령을 실행하십시오.

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

로그인 프로세스 동안 로그인 중에 지역을 선택하라는 프롬프트가 표시됩니다. VPC가 모든 지역에서 사용 가능하지 않을 수도 있습니다. 현재 사용 가능한 지역을 보려면 [다른 지역에 VPC 작성](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region)을 참조하십시오.
{: important}

## 2단계: 세대 대상 설정

다음 명령을 사용하여 세대 대상을 설정하십시오.

```
ibmcloud is target --gen 1
```
{: pre}


## 3단계: VPC 작성 및 VPC ID 저장
{: #step-3-create-a-vpc-and-save-the-vpc-id}

다음 명령을 사용하여 _helloworld-vpc_라는 VPC를 작성하십시오.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

다음과 같은 출력이 표시됩니다.

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

나중에 사용할 수 있도록 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## 4단계: 서브넷 작성 및 서브넷 ID 저장
{: #step-4-create-a-subnet-and-save-the-subnet-id}

서브넷의 위치로 `us-south-2` 구역을 선택하고 서브넷의 이름을 _helloworld-subnet_으로 지정하십시오.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

다음과 같은 출력이 표시됩니다.

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

나중에 사용할 수 있도록 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

서브넷을 처음 작성하면 서브넷의 상태가 `pending`인 것을 볼 수 있습니다. 진행하려면 먼저 서브넷이 `available` 상태로 변환되어야 하며, 여기에는 몇 초가 소요됩니다. 서브넷의 상태를 확인하려면 다음 명령을 실행하십시오.

```
ibmcloud is subnet $subnet
```
{: pre}

## 5단계: 공용 게이트웨이 연결
{: #step-5-attach-a-public-gateway}

공용 게이트웨이를 서브넷에 연결하여 모든 연결된 리소스가 공용 인터넷과 통신할 수 있도록 하십시오.

공용 게이트웨이를 작성하려면 다음 명령을 실행하십시오.

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

나중에 사용할 수 있도록 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다. 

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

공용 게이트웨이를 서브넷에 연결하려면 다음 명령을 실행하십시오.
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


VPC에서는 구역당 하나의 공용 게이트웨이만 허용되지만, 해당 공용 게이트웨이를 구역의 여러 서브넷에 연결할 수 있습니다. 구역의 공용 게이트웨이를 찾으려면 'ibmcloud is public-gateways` 명령을 실행하여 특정 VPC 및 and Zone 값을 찾으십시오.
{: tip}

다음과 같은 출력이 표시됩니다.

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

## 6단계: IBM 퍼블릭 클라우드에 SSH 키 작성
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

키를 사용하여 가상 서버 인스턴스를 프로비저닝하십시오. 동일한 키를 사용하여 여러 가상 서버 인스턴스를 프로비저닝할 수 있습니다.

처음에 VSI 작성의 일부로만 키를 추가할 수 있습니다. 나중에 키를 추가할 수 있는 도구를 없습니다.
{:tip}

IBM Cloud 계정에서 사용 가능한 키를 보려면 다음 명령을 실행하십시오.

```
ibmcloud is keys
```
{: pre}

키를 작성하려면 다음 명령을 실행하십시오. 경로를 `id_rsa.pub` 파일의 경로로 대체하십시오.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

다음과 같은 출력이 표시됩니다.

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

나중에 사용할 수 있도록 ID를 변수에 저장하십시오. 예를 들면 다음과 같습니다.

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## 7단계: 가상 서버 인스턴스의 프로파일 및 이미지 선택
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

사용 가능한 인스턴스 프로파일을 모두 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is instance-profiles
```
{: pre}

사용 가능한 이미지를 모두 나열하려면 다음 명령을 실행하십시오.

```
ibmcloud is images
```
{: pre}

인스턴스 프로파일 `bc1-2x8` 및 이미지 `ubuntu-16.04-amd64`를 선택하십시오. `ubuntu-16.04-amd64`의 이미지 ID를 얻으려면 다음 명령을 실행하십시오.

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## 8단계: 가상 서버 인스턴스 프로비저닝
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

처음에 VSI 작성의 일부로만 키를 추가할 수 있습니다. 나중에 키를 추가할 수 있는 도구를 없습니다.
{:tip}

다음과 같은 출력이 표시됩니다.

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

나중에 사용할 수 있도록 인스턴스의 ID를 변수에 저장하십시오. 

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

또한 나중에 사용할 수 있도록 기본 네트워크 인터페이스 `Primary Intf`의 ID를 변수에 저장하십시오.

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

인스턴스를 처음 작성하면 인스턴스의 상태가 `pending`인 것을 볼 수 있습니다. 진행하려면 먼저 인스턴스가 `running` 상태로 변환되어야 하며, 여기에는 몇 분이 소요됩니다. 인스턴스의 상태를 확인하려면 다음 명령을 실행하십시오.

```
ibmcloud is instance $vsi
```
{: pre}

## 9단계: 유동 IP 주소 작성
{: #step-9-create-a-floating-ip-address}

인터넷에서 가상 서버 인스턴스(VSI)에 로그인하려면 유동 IP 주소가 필요합니다.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

다음과 같은 출력이 표시됩니다.

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

나중에 사용할 수 있도록 유동 IP ID 및 `Address`를 변수에 저장하십시오.

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## 10단계: 기본 보안 그룹에 SSH에 대한 규칙 추가
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

VPC의 보안 그룹을 찾으십시오.

```
ibmcloud is vpc-sg $vpc
```
{: pre}

다음과 같은 출력이 표시됩니다.
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

나중에 사용할 수 있도록 ID를 변수에 저장하십시오.

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

이제 SSH를 허용하는 규칙을 작성하십시오.

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

다음과 같은 출력이 표시됩니다.

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

## 11단계: 블록 스토리지 데이터 볼륨 작성
{: #step-11-create-a-block-storage-data-volume}

이 명령을 실행하여 블록 스토리지 데이터 볼륨을 작성하십시오. 볼륨의 이름, 볼륨 프로파일 및 볼륨을 작성 중인 구역을 지정하십시오. 블록 스토리지 데이터 볼륨을 인스턴스에 연결하려면 인스턴스와 블록 스토리지 데이터 볼륨이 동일한 구역에 작성되어야 합니다.

볼륨 프로파일의 목록을 보려면 다음을 실행하십시오.

```
ibmcloud is volume-profiles
```
{: pre}

프로파일은 general-purpose(3IOPS/GB), 5iops-tier, 10-iops-tier 및 custom일 수 있습니다.
사용자가 선택한 볼륨 프로파일을 기반으로 하는 볼륨 용량 및 IOPS 범위에 대한 정보는 [Block Storage for VPC 정보](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)를 참조하십시오.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

다음과 같은 출력이 표시됩니다. 

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

나중에 사용할 수 있도록 볼륨의 ID를 변수에 저장하십시오. 

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

볼륨을 처음 작성하면 볼륨의 상태가 `pending`인 것을 볼 수 있습니다. 진행하려면 먼저 볼륨이 `available` 상태로 변환되어야 하며, 여기에는 몇 초가 걸립니다.

볼룸의 상태를 확인하려면 다음 명령을 실행하십시오.

```
ibmcloud is volume $vol
```
{: pre}

## 12단계: 블록 스토리지 데이터 볼륨을 인스턴스에 연결
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

작성한 변수를 사용하여 볼륨을 가상 서버 인스턴스에 연결하려면 다음 명령을 사용하십시오.

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## 13단계: 개인 SSH 키를 사용하여 가상 서버 인스턴스에 로그인
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

예를 들면, 다음 양식의 명령을 사용할 수 있습니다.

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

다음 예와 유사한 응답이 수신됩니다. 연결을 계속할지에 대한 프롬프트가 표시되면 `yes`를 입력하십시오.

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

## 14단계: Hello, World!
{: #step-14-hello-world}

터미널 창에서 다음 명령을 실행하십시오.

```
echo `hostname` says "Hello, World!"
```
{:pre}

다음 출력이 표시됩니다.

```
helloworld-vsi says Hello, World!
```
{:screen}

## 15단계: (선택사항) 블록 스토리지 데이터 볼륨 사용 시작
{: #step-15-optional-start-using-your-block-storage-data-volume}

블록 스토리지 볼륨을 파일 시스템으로 사용하려면 볼륨을 파티셔닝하고 형식화한 후 파일 시스템으로 마운트해야 합니다.

Linux에서 인스턴스의 모든 블록 스토리지 볼륨을 나열하려면 다음 명령을 실행하십시오.

```
lsblk
```
{:pre}

다음과 같은 출력이 표시됩니다.

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

볼륨 `xvdc`가 새 블록 스토리지 데이터 볼륨입니다.

다음 명령을 실행하여 볼륨을 파티셔닝하십시오. 시작하려면 새 파티션에 대해 `n` 명령을 사용하십시오.

```
fdisk /dev/xvdc
```
{:pre}

볼륨을 형식화하십시오.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

볼륨의 레이블을 "hellovol"로 지정하십시오.

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

디렉토리를 작성하고 볼륨을 파일 시스템으로 마운트하십시오.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

새 파일 시스템을 보려면 다음 명령을 실행하십시오.

```
df -k
```
{:pre}

다음과 같은 출력이 표시됩니다.

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

디렉토리를 새 파일 시스템으로 변경하고 파일을 작성하려면 다음과 같은 작업을 수행하십시오.

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## 15단계: (선택사항) 리소스 삭제
{: #step-15-delete-vpc-cli}

선택적으로 리소스를 삭제하십시오. 다른 리소스를 포함하는 리소스는 삭제할 수 없습니다. 예를 들어, 서브넷을 포함하는 Virtual Private Cloud는 삭제할 수 없으며, 가상 서버 인스턴스를 포함하는 서브넷은 삭제할 수 없습니다. 삭제 오퍼레이션 시, API는 빠르게 리턴될 수 있지만 리소스 삭제는 계속 진행 중일 수 있습니다. 삭제 요청을 발행한 후 상위 리소스를 삭제하려고 시도하기 전에 리소스가 삭제되었는지 확인하십시오. 세부사항은 [VPC 삭제](/docs/vpc-on-classic?topic=vpc-on-classic-deleting)를 참조하십시오.

### 유동 IP 해제

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### 가상 서버 인스턴스 삭제

```
ibmcloud is instance-delete $vsi
```
{: pre}

### 키 삭제

```
ibmcloud is key-delete $key
```
{: pre}

### 서브넷 삭제

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### 공용 게이트웨이 삭제

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### VPC 삭제

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### 볼륨 삭제(자동 삭제로 설정되지 않은 경우)

```
ibmcloud is volume-delete $vol
```
{: pre}

## 축하합니다!
{: #cli-congratulations}

IBM Cloud CLI를 사용하여 Virtual Private Cloud 인스턴스를 프로비저닝하고 여기에 연결했습니다. 더 많은 CLI 명령을 사용해 보려면 전체 참조를 살펴보십시오.

* [VPC에 대한 CLI 참조](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
