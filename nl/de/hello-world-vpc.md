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

# VPC über die IBM Cloud-CLI erstellen
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (Verknüpfter Hilfeabschnitt)

In diesem Dokument erfahren Sie, wie Sie {{site.data.keyword.cloud}} Virtual Private Cloud-Ressourcen mithilfe der IBM Cloud-CLI erstellen.

## Voraussetzungen:
{: #cli-pre-requisites}

1. Installieren Sie die [IBM Cloud-CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview).

2. Installieren oder aktualisieren Sie das Plug-in `vpc-infrastructure` für die IBM Cloud-CLI.

  Führen Sie zum Installieren den folgenden Befehl aus:

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  Führen Sie zum Aktualisieren den folgenden Befehl aus:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  Führen Sie zum Anzeigen installierter Plug-ins und Versionen den folgenden Befehl aus:

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. Generieren Sie einen öffentlichen SSH-Schlüssel zum Bereitstellen virtueller Serverinstanzen (Virtual Server Instances, VSIs).

Möglicherweise haben Sie bereits einen öffentlichen SSH-Schlüssel. Suchen Sie nach einer Datei mit dem Namen ``id_rsa.pub`` in einem ``.ssh``-Verzeichnis unter Ihrem Ausgangsverzeichnis, z. B. ``/Users/<USERNAME>/.ssh/id_rsa.pub``. Die Datei beginnt mit ``ssh-rsa`` und endet mit Ihrer E-Mail-Adresse.

Wenn Sie keinen öffentlichen SSH-Schlüssel oder das Kennwort eines vorhandenen Schlüssels vergessen haben, generieren Sie einen neuen, indem Sie den Befehl ``ssh-keygen`` ausführen und den Eingabeaufforderungen folgen.


## Schritt 1: Bei IBM Cloud anmelden
{: #step-1-log-in-to-ibm-cloud}

Wenn Sie über ein föderiertes Konto verfügen, setzen Sie den folgenden Befehl ab:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

Andernfalls verwenden Sie den folgenden Befehl:

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

Während des Anmeldeprozesses werden Sie aufgefordert, eine Region auszuwählen. VPC ist möglicherweise nicht in allen Regionen verfügbar. Wenn Sie erfahren möchten, in welchen Regionen VPC verfügbar ist, überprüfen Sie die Informationen in [VPCs in verschiedenen Regionen erstellen](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region).
{: important}

## Schritt 2: Generierungsziel festlegen

Verwenden Sie den folgenden Befehl, um das Generierungsziel zu definieren.

```
ibmcloud is target --gen 1
```
{: pre}


## Schritt 3: VPC erstellen und VPC-ID speichern
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Verwenden Sie den folgenden Befehl, um eine VPC mit dem Namen _helloworld-vpc_ zu erstellen.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676
Name      helloworld-vpc
Default   no
Created   1 second ago
Status    available
```
{:screen}

Speichern Sie die ID in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## Schritt 4: Teilnetz erstellen und Teilnetz-ID speichern
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Wählen Sie die Zone `us-south-2` für die Position des Teilnetzes aus und rufen Sie das Teilnetz _helloworld-subnet_ auf.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

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

Speichern Sie die ID in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

Beachten Sie, dass der Status des Teilnetzes bei der erstmaligen Erstellung `pending` (anstehend) lautet. Bevor Sie fortfahren können, muss das Teilnetz in den Status `available` (verfügbar) versetzt werden. Dies kann einige Sekunden dauern. Führen Sie den folgenden Befehl aus, um den Status des Teilnetzes zu überprüfen:

```
ibmcloud is subnet $subnet
```
{: pre}

## Schritt 5: Öffentliches Gateway zuordnen
{: #step-5-attach-a-public-gateway}

Hängen Sie ein öffentliches Gateway an das Teilnetz an, damit alle zugeordneten Ressourcen mit dem öffentlichen Internet kommunizieren können.

Führen Sie den folgenden Befehl aus, um ein öffentliches Gateway zu erstellen: 

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Speichern Sie die ID in einer Variablen so, dass sie später verwendet werden kann; Beispiel:

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

Führen Sie den folgenden Befehl aus, um das öffentliche Gateway dem Teilnetz zuzuordnen:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


Es ist zwar nur ein öffentliches Gateway pro Zone in einer VPC zulässig, dieses öffentliche Gateway kann jedoch mehreren Teilnetzen in der Zone zugeordnet werden. Wenn Sie das öffentliche Gateway für eine Zone suchen, führen Sie den Befehl 'ibmcloud is public-gateways' aus und überprüfen die konkreten Werte für die VPC und die Zone.
{: tip}

Die Ausgabe sollte wie folgt aussehen:

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

## Schritt 6: SSH-Schlüssel in IBM Public Cloud erstellen
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

Sie verwenden den Schlüssel zum Bereitstellen einer virtuellen Serverinstanz. Sie können denselben Schlüssel verwenden, um mehrere virtuelle Serverinstanzen bereitzustellen.

Schlüssel können nur am Anfang im Rahmen der Erstellung einer VSI hinzugefügt werden. Später stehen keine Tools zum Hinzufügen von Schlüsseln zur Verfügung.
{:tip}

Führen Sie den folgenden Befehl aus, um die verfügbaren Schlüssel in Ihrem IBM Cloud-Konto anzuzeigen:

```
ibmcloud is keys
```
{: pre}

Führen Sie den folgenden Befehl aus, um einen Schlüssel zu erstellen. Ersetzen Sie den Pfad zu der Datei `id_rsa.pub`.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

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

Speichern Sie die ID in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## Schritt 7: Profil und Image für virtuelle Serverinstanz auswählen
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

Führen Sie den folgenden Befehl aus, um alle verfügbaren Instanzprofile aufzulisten:

```
ibmcloud is instance-profiles
```
{: pre}

Führen Sie den folgenden Befehl aus, um alle verfügbaren Images aufzulisten:

```
ibmcloud is images
```
{: pre}

Wählen Sie das Instanzprofil `bc1-2x8` und das Image `ubuntu-16.04-amd64` aus. Führen Sie den folgenden Befehl aus, um die Image-ID von `ubuntu-16.04-amd64` abzurufen:

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Schritt 8: Virtuelle Serverinstanz bereitstellen
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Schlüssel können nur am Anfang im Rahmen der Erstellung der VSI hinzugefügt werden. Später stehen keine Tools zum Hinzufügen von Schlüsseln zur Verfügung.
{:tip}

Die Ausgabe sollte wie folgt aussehen:

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

Speichern Sie die Instanz-ID in einer Variablen so, dass sie später verwendet werden kann:

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Speichern Sie auch die ID der primären Netzschnittstelle `Primary Intf` in einer Variablen so, dass sie später verwendet werden kann. Beispiel:

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

Beachten Sie, dass der Status der Instanz bei der erstmaligen Erstellung `pending` (anstehend) lautet. Bevor Sie fortfahren können, muss die Instanz in den Status `running` (aktiv) versetzt werden, was einige Minuten in Anspruch nimmt. Führen Sie den folgenden Befehl aus, um den Status der Instanz zu überprüfen:

```
ibmcloud is instance $vsi
```
{: pre}

## Schritt 9: Variable IP-Adresse erstellen
{: #step-9-create-a-floating-ip-address}

Sie benötigen eine variable IP-Adresse, damit Sie sich über das Internet bei der virtuellen Serverinstanz (VSI) anmelden können.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

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

Speichern Sie die ID der variablen IP-Adresse und den Wert für `Address` in einer Variablen, damit Sie diese Angaben später wieder verwenden können:

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Schritt 10: Regel zur Standardsicherheitsgruppe für SSH hinzufügen
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Suchen Sie die Sicherheitsgruppe für die VPC:

```
ibmcloud is vpc-sg $vpc
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:
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

Speichern Sie die ID in einer Variablen so, dass sie später verwendet werden kann:

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Erstellen Sie nun die Regel, um SSH zu ermöglichen:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

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

## Schritt 11: Datenträger für Blockspeicherdaten erstellen
{: #step-11-create-a-block-storage-data-volume}

Führen Sie den folgenden Befehl aus, um einen Datenträger für Blockspeicherdaten zu erstellen. Geben Sie einen Namen für den Datenträger, das Datenträgerprofil und die Zone an, in der der Datenträger erstellt wird. Damit ein Datenträger für Blockspeicherdaten einer Instanz zugeordnet werden kann, müssen die Instanz und der Datenträger für die Blockspeicherdaten in derselben Zone erstellt werden.

Führen Sie die folgenden Befehle aus, um eine Liste der Datenträgerprofile anzuzeigen:

```
ibmcloud is volume-profiles
```
{: pre}

Zur Verfügung stehen Universalprofile (3 IOPS/GB), Profile mit 5 IOPS-Schichten (5iops-tier), Profile mit 10 IOPS-Schichten (10-iops-tier) und angepasste Profile. Informationen zur Datenträgerkapazität und den IOPS-Bereichen abhängig vom jeweils ausgewählten Datenträgerprofil finden Sie unter [Informationen zu Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance).  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

Die Ausgabe sollte wie folgt aussehen:

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

Speichern Sie die ID des Datenträgers in einer Variablen so, dass sie später verwendet werden kann:

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Beachten Sie, dass der Status des Datenträgers bei der erstmaligen Erstellung `pending` (anstehend) lautet. Damit Sie fortfahren können, muss der Datenträger in den Status `available` (verfügbar) wechseln; dies kann einige Minuten dauern.


Führen Sie den folgenden Befehl aus, um den Status des Datenträgers zu überprüfen:

```
ibmcloud is volume $vol
```
{: pre}

## Schritt 12: Einer Instanz einen Datenträger für Blockspeicherdaten zuordnen
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Verwenden Sie den folgenden Befehl, um den Datenträger mithilfe der erstellten Variablen der virtuellen Serverinstanz zuzuordnen:

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Schritt 13: Mit privatem SSH-Schlüssel an virtueller Serverinstanz anmelden
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

Sie können z. B. einen Befehl in diesem Format verwenden:

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

Sie erhalten eine ähnliche Antwort wie das folgende Beispiel. Wenn Sie aufgefordert werden, die Verbindung fortzusetzen, geben Sie `yes` ein.

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

## Schritt 14: Hello, World!
{: #step-14-hello-world}

Führen Sie den folgenden Befehl im Terminalfenster aus:

```
echo `hostname` says "Hello, World!"
```
{:pre}

Sie sollten die folgende Ausgabe sehen:

```
helloworld-vsi says Hello, World!
```
{:screen}

## Schritt 15: (Optional) Einstieg in die Verwendung des Datenträgers für Blockspeicherdaten
{: #step-15-optional-start-using-your-block-storage-data-volume}

Damit Sie den Blockspeicherdatenträger als Dateisystem verwenden können, müssen Sie den Datenträger partitionieren, formatieren und danach einem Dateisystem zuordnen.

Führen Sie unter Linux den folgenden Befehl aus, um alle Blockspeicherdatenträger für die Instanz aufzulisten:

```
lsblk
```
{:pre}

Die Ausgabe sollte wie folgt aussehen:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

Der Datenträger `xvdc` ist der neue Datenträger für die Blockspeicherdaten.

Führen Sie den folgenden Befehl aus, um den Datenträger zu partitionieren. Verwenden Sie zum Starten den Befehl `n` für die neue Partition.

```
fdisk /dev/xvdc
```
{:pre}

Formatieren Sie den Datenträger.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Legen Sie für den Datenträger die Bezeichnung 'hellovol' fest.

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Erstellen Sie das Verzeichnis und hängen Sie den Datenträger als Dateisystem an. 

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

Führen Sie den folgenden Befehl aus, um das neue Dateisystem anzuzeigen:

```
df -k
```
{:pre}

Die Ausgabe sollte wie folgt aussehen:

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

Gehen Sie wie folgt vor, um das Verzeichnis im neuen Dateisystem zu wechseln und eine Datei zu erstellen: 

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Schritt 15: (Optional) Ressourcen löschen
{: #step-15-delete-vpc-cli}

Löschen Sie optional die Ressourcen. Eine Ressource kann nicht gelöscht werden, wenn sie andere Ressourcen enthält. Beispiel: Eine VPC kann nicht gelöscht werden, wenn sie Teilnetze enthält, und ein Teilnetz kann nicht gelöscht werden, wenn es virtuelle Serverinstanzen enthält. Es kann vorkommen, dass ein Löschvorgang von der API schnell als abgeschlossen betrachtet wird, tatsächlich jedoch noch ausgeführt wird. Stellen Sie nach dem Absetzen der Löschanforderung sicher, dass die Ressource gelöscht wurde, bevor Sie versuchen, die übergeordnete Ressource zu löschen. Weitere Informationen hierzu finden Sie im Abschnitt [VPC löschen](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

### Variable IP-Adresse freigeben

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Instanz des virtuellen Servers löschen

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Schlüssel löschen

```
ibmcloud is key-delete $key
```
{: pre}

### Teilnetz löschen

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Öffentliches Gateway löschen

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### VPC löschen

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Datenträger löschen (wenn nicht automatisches Löschen eingestellt ist)

```
ibmcloud is volume-delete $vol
```
{: pre}

## Herzlichen Glückwunsch!
{: #cli-congratulations}

Sie haben Ihre VPC-Instanz mithilfe der IBM Cloud-CLI erfolgreich bereitgestellt und eine entsprechende Verbindung hergestellt. Wenn Sie weitere CLI-Befehle testen möchten, sollten Sie sich die vollständige Referenz ansehen:

* [CLI-Referenz für VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
