---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-29"

keywords: vpc, vpc cli, create, VPC, CLI, resources, plugin, SSH, key, hello, world, provision, instance, subnet

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

# Creating a VPC using the IBM Cloud CLI
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (linked help topic)

This document shows you how to create {{site.data.keyword.cloud}} Virtual Private Cloud resources using the IBM Cloud CLI.

## Pre-requisites:
{: #cli-pre-requisites}

1. Install the [IBM Cloud CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli).

2. Install or update the `vpc-infrastructure` plugin to the IBM Cloud CLI.

  To install:

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  To update:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  To view installed plugins and versions:

  ```
  ibmcloud plugin list
  ```
  {: pre}

3. Generate a public SSH key to provision Virtual Server Instances (VSIs).

You may have a public SSH key already. Look for a file called ``id_rsa.pub`` under an ``.ssh`` directory under your home directory, for example, ``/Users/<USERNAME>/.ssh/id_rsa.pub``. The file starts with ``ssh-rsa`` and ends with your email address.

If you do not have a public SSH key or if you forgot the password of an existing one, generate a new one by running the ``ssh-keygen`` command and following the prompts.


## Step 1: Log in to IBM Cloud.
{: #step-1-log-in-to-ibm-cloud}

If you have a federated account:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

otherwise

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

During the login process, you will be prompted to choose a region during login. VPC may not be available in all regions. Refer to [Creating VPCs in different regions](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region) to see which regions are available today.
{: important}

## Step 2: Set generation target.

Use the following command to set generation target.

```
ibmcloud is target --gen 1
```
{: pre}


## Step 3: Create a VPC and save the VPC ID.
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Use the following command to create a VPC named _helloworld-vpc_.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

You should see output like this:

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

Save the ID in a variable so we can use it later, for example:

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## Step 4: Create a subnet and save the subnet ID.
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Let's pick the `us-south-2` zone for the subnet's location and call the subnet _helloworld-subnet_.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

You should see output like this:

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

Save the ID in a variable so we can use it later, for example:

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

Note that the status of the subnet is `pending` when it first is created. Before you can proceed, the subnet needs to move to `available` status, which takes a few seconds. To check the status of the subnet, run this command:

```
ibmcloud is subnet $subnet
```
{: pre}

## Step 5: Attach a public gateway.
{: #step-5-attach-a-public-gateway}

Attach a public gateway to the subnet to allow all attached resources to communicate with the public internet.

To create a public gateway, run the following command:

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Save the ID in a variable so you can use it later, for example:

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

To attach the public gateway to your subnet, run the following command:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone. To find the public gateway for a zone, run the 'ibmcloud is public-gateways` command and look for the particular VPC and Zone values.
{: tip}

You should see output like this:

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

## Step 6: Create an SSH Key in IBM Public Cloud.
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

You'll use the key to provision a virtual server instance. You can use the same key to provision multiple virtual server instances.

Keys only can be added initially as part of creating a VSI. No tooling exists to add keys later.
{:tip}

To see the available keys in your IBM Cloud account, run this command:

```
ibmcloud is keys
```
{: pre}

To create a key, run the following command. Substitute the path to your `id_rsa.pub` file.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

You should see output like this:

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

Save the ID in a variable so we can use it later, for example:

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## Step 7: Select a profile and image for the virtual server instance.
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

To list all available instance profiles, run the following command:

```
ibmcloud is instance-profiles
```
{: pre}

To list all available images, run the following command:

```
ibmcloud is images
```
{: pre}

Let's pick instance profile `bc1-2x8` and image `ubuntu-16.04-amd64`. To get the image ID of `ubuntu-16.04-amd64`, run the following command:

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Step 8: Provision a Virtual Server Instance.
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Keys only can be added initially as part of creating the VSI. No tooling exists to add keys later.
{:tip}

You should see output like this:

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

Save the ID of the instance in a variable so we can use it later:

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Also save the ID of the primary network interface `Primary Intf` in a variable so we can use it later:

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

Note that the status of the instance is `pending` when it first is created. Before you can proceed, the instance needs to move to `running` status, which takes a few minutes. To check the status of the instance, run this command:

```
ibmcloud is instance $vsi
```
{: pre}

## Step 9: Create a Floating IP address.
{: #step-9-create-a-floating-ip-address}

You need a Floating IP address so you can log in to the virtual server instance (VSI) from the internet.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

You should see output like this:

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

Save the ID of the Floating IP and the `Address` in a variable so we can use it later:

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Step 10: Add a rule to the default security group for SSH.
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Find the security group for the VPC:

```
ibmcloud is vpc-sg $vpc
```
{: pre}

You should see output like this:
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

Save the ID in a variable so we can use it later:

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create the rule to allow SSH:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

You should see output like this:

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

## Step 11: Create a block storage data volume.
{: #step-11-create-a-block-storage-data-volume}

Run this command to create a block storage data volume. Specify a name for your volume, volume profile, and the zone where you are creating the volume. To attach a block storage data volume to an instance, the instance and the block storage data volume must be created in the same zone.

To see a list of volume profiles, run:

```
ibmcloud is volume-profiles
```
{: pre}

Profiles can be general-purpose (3 IOPS/GB), 5iops-tier, 10-iops-tier, and custom.
See [About Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
for information about volume capacity and IOPS ranges based on the volume profile you select.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

You will see output like this:

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

Save the ID of the volume in a variable so we can use it later:

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Note that the status of the volume is `pending` when it first is created. Before you can proceed, the volume needs to move to `available` status, which takes a few minutes.

To check the status of the volume, run this command:

```
ibmcloud is volume $vol
```
{: pre}

## Step 12: Attach a block storage data volume to an instance.
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Use the following command to attach the volume to the virtual server instance, using the variables we created:

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Step 13: Log in to your virtual server instance using your private SSH key.
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

For example, you can use a command of this form:

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

You'll receive a response similar to the example that follows. When you're prompted to continue connecting, type `yes`.

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

## Step 14: Hello, World!
{: #step-14-hello-world}

Run the following command in the terminal window:

```
echo `hostname` says "Hello, World!"
```
{:pre}

You should see the following output:

```
helloworld-vsi says Hello, World!
```
{:screen}

## Step 15: (Optional) Start using your block storage data volume
{: #step-15-optional-start-using-your-block-storage-data-volume}

To use your block storage volume as a filesystem, you'll need to partition the volume, format the volume, and then mount it as a filesystem.

On Linux, run the following command to list all block storage volumes from your instance:

```
lsblk
```
{:pre}

You should see output like this:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

Volume `xvdc` is your new block storage data volume.

Run the following command to partition the volume. To begin, use command `n` for new partition.

```
fdisk /dev/xvdc
```
{:pre}

Format the volume.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Label the volume as "hellovol".

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Create the directory and mount the volume as a filesystem.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

To see your new filesystem, run the following command:

```
df -k
```
{:pre}

You should see output like this:

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

To change directory into your new filesystem and create a file, do something like this:

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Step 15: (Optional) Delete the Resources
{: #step-15-delete-vpc-cli}

Optionally delete the resources. A resource cannot be deleted if it contains other resources. For example, a virtual private cloud cannot be deleted if it contains subnets, and a subnet cannot be deleted if it contains virtual server instances. On a delete operation, the API may return quickly but the resource deletion might still be in progress. After issuing the delete request, make sure the resource has been deleted before attempting to delete the parent resource. See [Deleting a VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) for more details.

### Release the floating IP

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Delete the virtual server instance

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Delete the key

```
ibmcloud is key-delete $key
```
{: pre}

### Delete the subnet

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Delete the public gateway

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### Delete the VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Delete the volume (if not set to auto delete)

```
ibmcloud is volume-delete $vol
```
{: pre}

## Congratulations!
{: #cli-congratulations}

You've successfully provisioned and connected to your Virtual Private Cloud instance using the IBM Cloud CLI. To try out more CLI commands, explore the full reference:

* [CLI Reference for VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
