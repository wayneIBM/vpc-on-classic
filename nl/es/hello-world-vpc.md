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

# Creación de una VPC mediante la CLI de IBM Cloud
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (tema de ayuda enlazado)

En este documento se muestra cómo crear recursos de {{site.data.keyword.cloud}} Virtual Private Cloud mediante la CLI de IBM Cloud.

## Requisitos previos:
{: #cli-pre-requisites}

1. Instale la [CLI de IBM Cloud](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview).

2. Instale o actualice el plugin `vpc-infrastructure` en la CLI de IBM Cloud.

  Para instalar:

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  Para actualizar:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  Para ver los plugins instalados y las versiones:

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. Genere una clave SSH pública para suministrar instancias de servidor virtual (VSI).

Es posible que ya tenga una clave SSH pública. Busque un archivo denominado ``id_rsa.pub`` en un directorio ``.ssh`` del directorio de inicio; por ejemplo, ``/Users/<USERNAME>/.ssh/id_rsa.pub``. El archivo empieza por ``ssh-rsa`` y termina con su dirección de correo electrónico.

Si no tiene una clave SSH pública o si ha olvidado la contraseña de una clave existente, genere una nueva ejecutando el mandato ``ssh-keygen`` y siguiendo la solicitud siguiente.


## Paso 1: Inicie sesión en IBM Cloud.
{: #step-1-log-in-to-ibm-cloud}

Si tiene una cuenta federada:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

de lo contrario

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

Durante el proceso de inicio de sesión, se le solicitará que elija una región al iniciar sesión. Es posible que la VPC no esté disponible en todas las regiones. Consulte [Creación de VPC en distintas regiones](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region) para ver las regiones que hay disponibles a día de hoy.
{: important}

## Paso 2: Establezca el destino de generación.

Utilice el mandato siguiente para establecer el destino de generación.

```
ibmcloud is target --gen 1
```
{: pre}


## Paso 3: Cree una VPC y guarde el ID de VPC.
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Utilice el mandato siguiente para crear una VPC denominada _helloworld-vpc_.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

Debería ver una salida como la siguiente:

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676
Name      helloworld-vpc
Default   no
Created   1 second ago
Status    available   
```
{:screen}

Guarde el ID en una variable para poder utilizarlo más adelante; por ejemplo:

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676"
```
{: pre}

## Paso 4: Cree una subred y guarde el ID de subred.
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Vamos a seleccionar la zona `us-south-2` para la ubicación de la subred y llamar a la subred _helloworld-subnet_.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

Debería ver una salida como la siguiente:

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

Guarde el ID en una variable para poder utilizarlo más adelante; por ejemplo:

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14"
```
{: pre}

Tenga en cuenta que el estado de la subred es `pendiente` cuando se crea por primera vez. Antes de poder continuar, la subred debe pasar al estado `available`, proceso que tarda unos segundos. Para comprobar el estado de la subred, ejecute el mandato siguiente:

```
ibmcloud is subnet $subnet
```
{: pre}

## Paso 5: Conecte una pasarela pública.
{: #step-5-attach-a-public-gateway}

Conecte una pasarela pública a la subred para permitir que todos los recursos conectados se comuniquen con la internet pública.

Para crear una pasarela pública, ejecute el mandato siguiente:

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Guarde el ID en una variable para poder utilizarlo más adelante; por ejemplo:

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

Para conectar la pasarela pública a la subred, ejecute el mandato siguiente:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


Solo se permite una pasarela pública por zona en una VPC, pero esa pasarela pública se puede conectar a varias subredes de la zona. Para encontrar la pasarela pública para una zona, ejecute el mandato 'ibmcloud is public-gateways` y busque los valores de VPC y Zona en particular.
{: tip}

Debería ver una salida como la siguiente:

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

## Paso 6: Cree una clave SSH en la nube pública de IBM.
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

Utilizará la clave para suministrar una instancia de servidor virtual. Puede utilizar la misma clave para suministrar varias instancias de servidor virtual.

Las claves sólo se pueden añadir inicialmente como parte de la creación de una VSI. No existe ninguna herramienta para añadir claves posteriormente.
{:tip}

Para ver las claves disponibles en la cuenta de IBM Cloud, ejecute este mandato:

```
ibmcloud is keys
```
{: pre}

Para crear una clave, ejecute el mandato siguiente. Sustituya la vía de acceso en el archivo `id_rsa.pub`.

```
ibmcloud is key-create helloworld-key @$HOME/.ssh/id_rsa.pub
```
{: pre}

Debería ver una salida como la siguiente:

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

Guarde el ID en una variable para poder utilizarlo más adelante; por ejemplo:

```
key="859b4e97-7540-4337-9c64-384792b85653"
```

## Paso 7: Seleccione un perfil y una imagen para la instancia de servidor virtual.
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

Para ver una lista de todos los perfiles de instancia disponibles, ejecute el siguiente mandato:

```
ibmcloud is instance-profiles
```
{: pre}

Para listar todas las imágenes disponibles, ejecute el siguiente mandato:

```
ibmcloud is images
```
{: pre}

Escojamos el perfil de instancia `bc1-2x8` y la imagen `ubuntu-16.04-amd64`. Para obtener el ID de imagen de `ubuntu-16.04-amd64`, ejecute el mandato siguiente:

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Paso 8: Proporcione una instancia de servidor virtual.
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

Las claves sólo se pueden añadir inicialmente como parte de la creación de la VSI. No existe ninguna herramienta para añadir claves posteriormente.
{:tip}

Debería ver una salida como la siguiente:

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

Guarde el ID de la instancia en una variable para poder utilizarlo más adelante:

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Guarde también el ID de la interfaz de red primaria `Primary Intf` en una variable para poder utilizarlo posteriormente:

```
nic="2e850924-b5d7-4386-a778-03556d5850c1"
```
{:pre}

Tenga en cuenta que el estado de la instancia es `pendiente` cuando se crea por primera vez. Antes de poder continuar, la instancia debe pasar al estado `running`, proceso que tarda unos minutos. Para comprobar el estado de la instancia, ejecute el mandato siguiente:

```
ibmcloud is instance $vsi
```
{: pre}

## Paso 9: Cree una dirección IP flotante.
{: #step-9-create-a-floating-ip-address}

Necesita una dirección IP flotante para poder iniciar sesión en la instancia de servidor virtual (VSI) desde Internet.

```
ibmcloud is floating-ip-reserve helloworld-fip --nic-id $nic
```
{: pre}

Debería ver una salida como la siguiente:

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

Guarde el ID de la IP flotante y la `dirección` en una variable para poder utilizarlos más adelante:

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Paso 10: Añada una regla al grupo de seguridad predeterminado para SSH.
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Busque el grupo de seguridad para la VPC:

```
ibmcloud is vpc-sg $vpc
```
{: pre}

Debería ver una salida como la siguiente:
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

Guarde el ID en una variable para poder utilizarlo más adelante:

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Ahora cree la regla para permitir el SSH:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Debería ver una salida como la siguiente:

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

## Paso 11: Cree un volumen de datos de almacenamiento en bloques.
{: #step-11-create-a-block-storage-data-volume}

Ejecute este mandato para crear un volumen de datos de almacenamiento en bloques. Especifique un nombre para el volumen, el perfil de volumen y la zona donde va a crear el volumen. Para adjuntar un volumen de datos de almacenamiento en bloques a una instancia, la instancia y el volumen de datos de almacenamiento en bloques se deben crear en la misma zona.

Para ver una lista de perfiles de volúmenes, ejecute:

```
ibmcloud is volume-profiles
```
{: pre}

Los perfiles pueden ser generales (3 IOPS/GB), 5iops-tier, 10-iops-tier o personalizados.
Consulte [Acerca de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
para obtener información sobre la capacidad de volumen y los rangos de IOPS según el perfil de volumen que haya seleccionado.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

Verá una salida como la siguiente:

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

Guarde el ID del volumen en una variable para poder utilizarlo más adelante:

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Tenga en cuenta que el estado del volumen es `pendiente` cuando se crea por primera vez. Para poder continuar, el volumen debe pasar al estado `available`, proceso que tarda unos minutos.

Para comprobar el estado del volumen, ejecute el mandato siguiente:

```
ibmcloud is volume $vol
```
{: pre}

## Paso 12: Adjunte un volumen de datos de almacenamiento en bloques a una instancia.
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Utilice el mandato siguiente para adjuntar el volumen a la instancia de servidor virtual, utilizando las variables que hemos creado:

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Paso 13: Inicie sesión en la instancia de servidor virtual utilizando la clave SSH privada.
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

Por ejemplo, puede utilizar un mandato con este formato:

```
ssh -i $HOME/.ssh/id_rsa root@$address
```
{:pre}

Recibirá una respuesta similar a la del ejemplo siguiente. Cuando se le solicite continuar conectándose, escriba `yes`.

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

## Paso 14: Hello, World!
{: #step-14-hello-world}

Ejecute el mandato siguiente en la ventana de terminal:

```
echo `hostname` says "Hello, World!"
```
{:pre}

Verá la salida siguiente:

```
helloworld-vsi says Hello, World!
```
{:screen}

## Paso 15: (opcional) Empiece a utilizar el volumen de datos de almacenamiento en bloques
{: #step-15-optional-start-using-your-block-storage-data-volume}

Para utilizar el volumen de almacenamiento en bloques como sistema de ficheros, tendrá que particionar el volumen, formatearlo y luego montarlo como un sistema de ficheros.

En Linux, ejecute el mandato siguiente para listar todos los volúmenes de almacenamiento en bloques de la instancia:

```
lsblk
```
{:pre}

Debería ver una salida como la siguiente:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

El volumen `xvdc` es el nuevo volumen de datos de almacenamiento en bloques.

Ejecute el mandato siguiente para particionar el volumen. Para empezar, utilice el mandato `n` para hacer una nueva partición.

```
fdisk /dev/xvdc
```
{:pre}

Dé formato al volumen.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Etiquete el volumen como "hellovol".

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Cree el directorio y monte el volumen como un sistema de archivos.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

Para ver el nuevo sistema de archivos, ejecute el mandato siguiente:

```
df -k
```
{:pre}

Debería ver una salida como la siguiente:

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

Para cambiar el directorio en el nuevo sistema de archivos y crear un archivo, haga algo como esto:

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Paso 15: (opcional) Suprima los recursos
{: #step-15-delete-vpc-cli}

Opcionalmente, suprima los recursos. No se puede suprimir un recurso si contiene otros recursos. Por ejemplo, una nube privada virtual no se puede suprimir si contiene subredes, y una subred no se puede suprimir si contiene instancias del servidor virtual. En una operación de supresión, la API puede retornar rápidamente, pero es posible que la supresión de recursos siga en curso. Después de emitir la solicitud de supresión, asegúrese de que el recurso se ha suprimido antes de intentar suprimir el recurso padre. Consulte [Supresión de una VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obtener más detalles.

### Libere la IP flotante

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Suprima la instancia de servidor virtual

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Suprima la clave

```
ibmcloud is key-delete $key
```
{: pre}

### Suprima la subred

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Suprima la pasarela pública

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### Suprima la VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Suprima el volumen (si no está establecido en supresión automática)

```
ibmcloud is volume-delete $vol
```
{: pre}

## ¡Enhorabuena!
{: #cli-congratulations}

Ha suministrado y conectado correctamente con la instancia de la nube privada virtual mediante la CLI de IBM Cloud. Para probar más mandatos de CLI, consulte el siguiente enlace:

* [Referencia de CLI para VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
