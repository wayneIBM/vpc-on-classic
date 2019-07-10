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

# Creazione di un VPC utilizzando la CLI IBM Cloud
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (argomento della guida collegato)

Questo documento ti mostra come creare le risorse {{site.data.keyword.cloud}} Virtual Private Cloud utilizzando la CLI IBM Cloud.

## Prerequisiti:
{: #cli-pre-requisites}

1. Installa la CLI [IBM Cloud](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview).

2. Installa o aggiorna il plugin `vpc-infrastructure` nella CLI IBM Cloud.

  Per eseguire l'installazione:

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  Per eseguire l'aggiornamento:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  Per visualizzare i plugin e le versioni installate:

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. Genera una chiave SSH pubblica per eseguire il provisioning delle istanze del server virtuale (VSI).

Potresti avere già una chiave SSH pubblica. Ricerca un file denominato ``id_rsa.pub`` in una directory ``.ssh`` all'interno della tua directory home, ad esempio ``/Users/<USERNAME>/.ssh/id_rsa.pub``. Il file inizia con ``ssh-rsa`` e termina con il tuo indirizzo email.

Se non hai una chiave SSH pubblica o se hai dimenticato la password di una chiave esistente, generane una nuova eseguendo il comando ``ssh-keygen`` e seguendo le istruzioni.


## Passo 1: accedi a IBM Cloud.
{: #step-1-log-in-to-ibm-cloud}

Se hai un account federato:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

altrimenti

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

Durante il processo di accesso, ti verrà richiesto di scegliere una regione. VPC potrebbe non essere disponibile in tutte le regioni. Fai riferimento a [Creazione di VPC in zone diverse](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region) per vedere quali regioni sono disponibili attualmente.
{: important}

## Passo 2: imposta la destinazione di generazione.

Utilizza il seguente comando per impostare la destinazione di generazione.

```
ibmcloud is target --gen 1
```
{: pre}


## Passo 3: crea un VPC e salva l'ID VPC.
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Utilizza il seguente comando per creare un VPC denominato _helloworld-vpc_.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

Dovresti vedere un output simile a questo:

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

Salva l'ID in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## Passo 4: crea una sottorete e salva l'ID sottorete.
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Scegli la zona `us-south-2` per l'ubicazione della sottorete e chiama la sottorete _helloworld-subnet_.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

Dovresti vedere un output simile a questo:

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

Salva l'ID in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

Nota che lo stato della sottorete è `pending` appena viene creata. Prima di poter procedere, la sottorete deve passare allo stato `available`, il che richiede alcuni secondi. Per controllare lo stato della sottorete, esegui questo comando:

```
ibmcloud is subnet $subnet
```
{: pre}

## Passo 5: collega un gateway pubblico.
{: #step-5-attach-a-public-gateway}

Collega un gateway pubblico alla sottorete per consentire a tutte le risorse collegate di comunicare con l'Internet pubblico.

Per creare un gateway pubblico, immetti il seguente comando:

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Salva l'ID in una variabile in modo da poterla utilizzare in seguito, ad esempio:

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

Per collegare il gateway pubblico alla tua sottorete, immetti il seguente comando:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


In un VPC, è consentito un solo gateway pubblico per ogni zona, ma tale gateway pubblico può essere collegato a più sottoreti nella zona. Per trovare il gateway pubblico per una zona, esegui il comando 'ibmcloud is public-gateways` e cerca gli specifici valori di VPC e Zone.
{: tip}

Dovresti vedere un output simile a questo:

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

## Passo 6: crea una chiave SSH nel cloud pubblico di IBM.
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

Utilizzerai la chiave per eseguire il provisioning di un'istanza del server virtuale. Puoi utilizzare la stessa chiave per eseguire il provisioning di più istanze del server virtuale.

Le chiavi possono essere aggiunte solo inizialmente come parte della creazione di una VSI. Non esistono strumenti per aggiungere le chiavi in un secondo momento.
{:tip}

Per visualizzare le chiavi disponibili nel tuo account IBM Cloud, esegui questo comando:

```
ibmcloud is keys
```
{: pre}

Per creare una chiave, immetti il seguente comando. Sostituisci il percorso al tuo file `id_rsa.pub`.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

Dovresti vedere un output simile a questo:

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

Salva l'ID in una variabile in modo che possiamo utilizzarla in seguito, ad esempio:

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## Passo 7: seleziona un profilo e un'immagine per l'istanza del server virtuale.
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

Per elencare tutti i profili di istanza disponibili, immetti il seguente comando:

```
ibmcloud is instance-profiles
```
{: pre}

Per elencare tutte le immagini disponibili, immetti il seguente comando:

```
ibmcloud is images
```
{: pre}

Seleziona il profilo di istanza `bc1-2x8` e l'immagine `ubuntu-16.04-amd64`. Per ottenere l'ID dell'immagine di `ubuntu-16.04-amd64`, immetti il seguente comando:

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Passo 8: esegui il provisioning di un'istanza del server virtuale.
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Le chiavi possono essere aggiunte solo inizialmente come parte della creazione della VSI. Non esistono strumenti per aggiungere le chiavi in un secondo momento.
{:tip}

Dovresti vedere un output simile a questo:

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

Salva l'ID dell'istanza in una variabile in modo che possiamo utilizzarla in seguito:

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Salva anche l'ID dell'interfaccia di rete primaria `Primary Intf` in una variabile in modo da poterla utilizzare in seguito:

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

Nota che lo stato dell'istanza è `pending` appena viene creata. Prima di poter procedere, l'istanza deve passare allo stato `running`, il che richiede alcuni minuti. Per controllare lo stato dell'istanza, esegui questo comando:

```
ibmcloud is instance $vsi
```
{: pre}

## Passo 9: crea un indirizzo IP mobile.
{: #step-9-create-a-floating-ip-address}

Hai bisogno di un indirizzo IP mobile per poter accedere all'istanza del server virtuale (VSI) da Internet.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

Dovresti vedere un output simile a questo:

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

Salva l'ID dell'IP mobile e l'`Address` in una variabile in modo che possiamo utilizzarla in seguito:

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Passo 10: aggiungi una regola al gruppo di sicurezza predefinito per SSH.
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Trova il gruppo di sicurezza per il VPC:

```
ibmcloud is vpc-sg $vpc
```
{: pre}

Dovresti vedere un output simile a questo:
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

Salva l'ID in una variabile in modo che possiamo utilizzarla in seguito:

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Ora crea la regola per consentire SSH:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Dovresti vedere un output simile a questo:

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

## Passo 11: crea un volume di dati di archiviazione blocchi.
{: #step-11-create-a-block-storage-data-volume}

Esegui questo comando per creare un volume di dati di archiviazione blocchi. Specifica un nome per il tuo volume, il profilo del volume e la zona in cui stai creando il volume. Per collegare un volume di dati di archiviazione blocchi a un'istanza, l'istanza e il volume di dati di archiviazione blocchi devono essere creati nella stessa zona.

Per visualizzare un elenco di profili del volume, esegui:

```
ibmcloud is volume-profiles
```
{: pre}

I profili possono essere di utilizzo generico (3 IOPS/GB), di livello 5 iops, di livello 10 iops e personalizzati.
Vedi [Informazioni su Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
per informazioni sulla capacità e sugli intervalli IOPS del volume in base al profilo del volume che hai selezionato.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

Vedrai un output simile a questo:

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

Salva l'ID del volume in una variabile in modo che possiamo utilizzarla in seguito:

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Nota che lo stato del volume è `pending` quando viene creato per la prima volta. Prima di poter procedere, il volume deve passare allo stato `available`, il che richiede alcuni minuti.

Per controllare lo stato del volume, esegui questo comando:

```
ibmcloud is volume $vol
```
{: pre}

## Passo 12: collega un volume di dati di archiviazione blocchi a un'istanza.
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Utilizza il seguente comando per collegare il volume all'istanza del server virtuale, utilizzando le variabili che abbiamo creato:

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Passo 13: accedi alla tua istanza del server virtuale utilizzando la tua chiave SSH privata.
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

Ad esempio, puoi utilizzare un comando di questo formato:

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

Riceverai una risposta simile al seguente esempio. Quando ti viene richiesto di continuare con la connessione, digita `yes`.

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

## Passo 14: Hello, World!
{: #step-14-hello-world}

Immetti il seguente comando nella finestra del terminale:

```
echo `hostname` says "Hello, World!"
```
{:pre}

Dovresti vedere il seguente output:

```
helloworld-vsi says Hello, World!
```
{:screen}

## Passo 15: (facoltativo) inizia a utilizzare il tuo volume di dati di archiviazione blocchi
{: #step-15-optional-start-using-your-block-storage-data-volume}

Per utilizzare il volume di archiviazione blocchi come file system, dovrai partizionare il volume, formattare il volume e quindi montarlo come file system.

Su Linux, immetti il seguente comando per elencare tutti i volumi di archiviazione blocchi dalla tua istanza:

```
lsblk
```
{:pre}

Dovresti vedere un output simile a questo:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

Il volume `xvdc` è il tuo nuovo volume di dati di archiviazione blocchi.

Immetti il seguente comando per partizionare il volume. Per iniziare, utilizza il comando `n` per la nuova partizione.

```
fdisk /dev/xvdc
```
{:pre}

Formatta il volume.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Etichetta il volume come "hellovol".

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Crea la directory e monta il volume come file system.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

Per visualizzare il tuo nuovo file system, immetti il seguente comando:

```
df -k
```
{:pre}

Dovresti vedere un output simile a questo:

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

Per modificare la directory nel tuo nuovo file system e creare un file, esegui un comando simile a questo:

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Passo 15: (facoltativo) elimina le risorse
{: #step-15-delete-vpc-cli}

Facoltativamente, elimina le risorse. Non è possibile eliminare una risorsa se contiene altre risorse. Ad esempio, non è possibile eliminare un VPC (Virtual Private Cloud) se contiene delle sottoreti e non è possibile eliminare una sottorete se contiene istanze del server virtuale. In un'operazione di eliminazione, l'API può essere restituita rapidamente ma l'eliminazione della risorsa potrebbe ancora essere in corso. Dopo aver emesso la richiesta di eliminazione, assicurati che la risorsa sia stata eliminata prima di tentare di eliminare la risorsa principale. Per ulteriori dettagli, vedi [Eliminazione di un VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).

### Rilascia l'IP mobile

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Elimina l'istanza del server virtuale

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Elimina la chiave

```
ibmcloud is key-delete $key
```
{: pre}

### Elimina la sottorete

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Elimina il gateway pubblico

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### Elimina il VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Elimina il volume (se non impostato sull'eliminazione automatica)

```
ibmcloud is volume-delete $vol
```
{: pre}

## Congratulazioni!
{: #cli-congratulations}

Hai eseguito correttamente il provisioning e la connessione alla tua istanza Virtual Private Cloud tramite la CLI IBM Cloud. Per provare ulteriori comandi della CLI, esplora il riferimento completo:

* [Riferimento CLI per VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
