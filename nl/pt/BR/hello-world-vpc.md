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

# Criando um VPC usando a CLI do IBM Cloud
{: #creating-a-vpc-using-the-ibm-cloud-cli}
[comment]: # (tópico da ajuda vinculado)

Este documento mostra como criar os recursos do {{site.data.keyword.cloud}} Virtual Private Cloud usando a CLI do IBM Cloud.

## Pré-requisitos:
{: #cli-pre-requisites}

1. Instale a  [ CLI do IBM Cloud ](/docs/cli?topic=cloud-cli-ibmcloud-cli#overview).

2. Instale ou atualize o plug-in `vpc-infrastructure` para a CLI do IBM Cloud.

  Para instalar:

  ```
  ibmcloud plugin install vpc-infrastructure
  ```
  {: pre}

  Para atualizar:

  ```
  ibmcloud plugin update
  ```
  {: pre}

  Para visualizar plug-ins e versões instalados:

  ```
  ibmcloud plugin list
  ```
  {: pre}


3. Gere uma chave SSH pública para provisionar Instâncias de Servidor Virtual (VSIs).

Talvez você já tenha uma chave SSH pública. Procure por um arquivo chamado ``id_rsa.pub`` sob um diretório ``.ssh`` sob seu diretório inicial, por exemplo, ``/Users/<USERNAME>/.ssh/id_rsa.pub``. O arquivo começa com ``ssh-rsa`` e termina com seu endereço de e-mail.

Se você não tiver uma chave SSH pública ou se tiver esquecido a senha de uma existente, gere uma nova executando o comando ``ssh-keygen`` e seguindo os prompts.


## Etapa 1: Efetuar login no IBM Cloud.
{: #step-1-log-in-to-ibm-cloud}

Se você tiver uma conta federada:
```
ibmcloud login -a cloud.ibm.com -sso
```
{: pre}

otherwise

```
ibmcloud login -a cloud.ibm.com
```
{: pre}

Durante o processo de login, será solicitado que você escolha uma região durante o login. O VPC pode não estar disponível em todas as regiões. Consulte [Criando VPCs em diferentes regiões](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-in-a-different-region) para ver quais regiões estão disponíveis hoje.
{: important}

## Etapa 2: configurar destino de geração.

Use o comando a seguir para configurar o destino de geração.

```
ibmcloud is target --gen 1
```
{: pre}


## Etapa 3: criar um VPC e salve o ID do VPC.
{: #step-3-create-a-vpc-and-save-the-vpc-id}

Use o comando a seguir para criar um VPC denominada _helloworld-vpc_.

```
ibmcloud is vpc-create helloworld-vpc
```
{: pre}

Você deve ver a saída como esta:

```
Creating VPC helloworld-vpc in resource group Default under account <Account Name> as user <User>...

ID        ba9e785a-3e10-418a-811c-56cfe5669676   
Name      helloworld-vpc   
Default   no   
Created   1 second ago   
Status    available   
```
{:screen}

Salve o ID em uma variável para que possamos utilizá-lo posteriormente, por exemplo:

```
vpc="ba9e785a-3e10-418a-811c-56cfe5669676 "
```
{: pre}

## Etapa 4: criar uma sub-rede e salve o ID da sub-rede.
{: #step-4-create-a-subnet-and-save-the-subnet-id}

Vamos escolher a zona `us-south-2` para o local da sub-rede e chamar a sub-rede _helloworld-subnet_.

```
ibmcloud is subnet-create helloworld-subnet $vpc us-south-2 --ipv4-address-count 8
```
{: pre}

Você deve ver a saída como esta:

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

Salve o ID em uma variável para que possamos utilizá-lo posteriormente, por exemplo:

```
subnet="50ba0da9-279a-4791-b7cb-cd3d7b2bc14 "
```
{: pre}

Observe que o status da sub-rede é `pending` quando ela é criada pela primeira vez. Antes de continuar, a sub-rede precisa ser movida para o status `available`, o que leva alguns segundos. Para verificar o status da sub-rede, execute este comando:

```
ibmcloud é sub-rede $subnet
```
{: pre}

## Etapa 5: anexar um gateway público.
{: #step-5-attach-a-public-gateway}

Anexe um gateway público à sub-rede para permitir que todos os recursos anexados se comuniquem com a Internet pública.

Para criar um gateway público, execute o comando a seguir:

```
ibmcloud is public-gateway-create my-gateway $vpc us-south-2
```
{: pre}

Salve o ID em uma variável para que seja possível usá-lo posteriormente, por exemplo:

```
gateway="f94a91c7-95db-42f2-9949-93a7e8fb63fb"
```
{: pre}

Para conectar o gateway público à sua sub-rede, execute o comando a seguir:
```
ibmcloud is subnet-update $subnet --public-gateway-id $gateway
```
{: pre}


Apenas um gateway público por zona é permitido em um VPC, mas esse gateway público pode ser conectado a diversas sub-redes na zona. Para localizar o gateway público para uma zona, execute o comando 'ibmcloud is public-gateways` e procure os valores de VPC e Zona específicos.
{: tip}

Você deve ver a saída como esta:

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

## Etapa 6: criar uma chave SSH no IBM Public Cloud.
{: #step-6-create-an-ssh-key-in-ibm-public-cloud}

Você usará a chave para fornecer uma instância de servidor virtual. É possível usar a mesma chave para provisionar múltiplas instâncias de servidor virtual.

As chaves só podem ser incluídas inicialmente como parte da criação de uma VSI. Não existe nenhuma ferramenta para incluir chaves posteriormente.
{:tip}

Para ver as chaves disponíveis em sua conta do IBM Cloud, execute este comando:

```
ibmcloud são chaves
```
{: pre}

Para criar uma chave, execute o comando a seguir. Substitua o caminho para o arquivo `id_rsa.pub`.

```
ibmcloud is key-create helloworld-key @ $HOME/ .ssh/id_rsa.pub
```
{: pre}

Você deve ver a saída como esta:

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

Salve o ID em uma variável para que possamos utilizá-lo posteriormente, por exemplo:

```
key=" 859b4e97-7540-4337-9c64-384792b85653 "
```

## Etapa 7: selecionar um perfil e uma imagem para a instância de servidor virtual.
{: #step-7-select-a-profile-and-image-for-the-virtual-server-instance}

Para listar todos os perfis de instância disponíveis, execute o comando a seguir:

```
ibmcloud é instance-profiles
```
{: pre}

Para listar todas as imagens disponíveis, execute o comando a seguir:

```
ibmcloud são imagens
```
{: pre}

Vamos escolher o perfil da instância `bc1-2x8` e a imagem `ubuntu-16.04-amd64`. Para obter o ID da imagem de `ubuntu-16.04-amd64`, execute o comando a seguir:

```
image=$(ibmcloud is images | grep "ubuntu-16.04-amd64" | cut -d" " -f1)
```
{: pre}

## Etapa 8: provisionar uma instância de servidor virtual.
{: #step-8-provision-a-virtual-server-instance}

```
ibmcloud is instance-create helloworld-vsi $vpc us-south-2 bc1-2x8 $subnet --image-id $image --key-ids $key
```
{: pre}

As chaves só podem ser incluídas inicialmente como parte da criação da VSI. Não existe nenhuma ferramenta para incluir chaves posteriormente.
{:tip}

Você deve ver a saída como esta:

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

Salve o ID da instância em uma variável para que possamos usá-lo posteriormente:

```
vsi=4562c5c0-9cf7-4406-bc90-ab4baea91057
{: pre}
```

Além disso, salve o ID da interface de rede primária `Primary Intf` em uma variável para que possamos usá-la posteriormente:

```
nico="2e850924-b5d7-4386-a778-03556d5850c1 "
```
{:pre}

Observe que o status da instância é `pending` quando ela é criada pela primeira vez. Antes de ser possível continuar, a instância precisa ser movida para o status `running`, o que leva alguns minutos. Para verificar o status da instância, execute este comando:

```
ibmcloud is instance $vsi
```
{: pre}

## Etapa 9: criar um endereço IP flutuante.
{: #step-9-create-a-floating-ip-address}

Você precisa de um endereço IP Flutuante para que seja possível efetuar login na instância de servidor virtual (VSI) por meio da Internet.

```
ibmcloud is floating-ip-reserve helloworld-fip -- nic-id $nic
```
{: pre}

Você deve ver a saída como esta:

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

Salve o ID do IP flutuante e o `Address` em uma variável para que possamos usá-lo posteriormente:

```
floating_ip="b9d1cc1f-67db-40e3-81de-9228465170a5"
address=169.61.181.53
```
{: pre}

## Etapa 10: incluir uma regra no grupo de segurança padrão para SSH.
{: #step-10-add-a-rule-to-the-default-security-grop-for-ssh}

Localize o grupo de segurança para o VPC:

```
ibmcloud é vpc-sg $vpc
```
{: pre}

Você deve ver a saída como esta:
```
Getting default security group of vpc ba9e785a-3e10-418a-811c-56cfe5669676 under account <Account Name> as user <User>...

ID               2d364f0a-a870-42c3-a554-000000981149
Name             errand-drastic-imperial-retail-unlocked-jester
Created          1 week ago
VPC              helloworld-vpc(ba9e785a-3e10-418a-811c-56cfe5669676)
Grupo de Recursos - Tags -

Rules
ID                                     Direction   IPv*   Protocol                  Remote
b597cff2-38e8-4e6e-999d-000002031345   inbound     ipv4   all                       errand-drastic-imperial-retail-unlocked-jester(2d364f0a-.)
b597cff2-38e8-4e6e-999d-000002031527   outbound    ipv4   all                       -

```
{:screen}

Salve o ID em uma variável para que possa ser usado posteriormente:

```
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Agora crie a regra para permitir a SSH:

```
ibmcloud is sg-rulec $sg inbound tcp --port-min=22 --port-max=22
```
{: pre}

Você deve ver a saída como esta:

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

## Etapa 11: criar um volume de dados de armazenamento de bloco.
{: #step-11-create-a-block-storage-data-volume}

Execute este comando para criar um volume de dados de armazenamento de bloco. Especifique um nome para seu volume, perfil de volume e a zona em que você está criando o volume. Para anexar um volume de dados de armazenamento de bloco a uma instância, a instância e o volume de dados de armazenamento de bloco devem ser criados na mesma zona.

Para ver uma lista de perfis de volume, execute:

```
ibmcloud is volume-profiles
```
{: pre}

Os perfis podem ser de propósito geral (3 IOPS/GB), 5iops-tier, 10-iops-tier e customizado.
Consulte [Sobre o Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance) para obter informações sobre a capacidade do volume e os intervalos de IOPS com base no perfil de volume selecionado.  

```
ibmcloud is volume-create helloworld-vol custom us-south-2 --iops 1000
```
{: pre}

Você verá uma saída como esta:

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

Salve o ID do volume em uma variável para que possamos usá-lo posteriormente:

```
vol=933c8781-f7f5-4a8f-8a2d-3bfc711788ee
```
{: pre}

Observe que o status do volume está `pending` quando ele é criado pela primeira vez. Antes de continuar, o volume precisa mover-se para o status `available`, que leva alguns minutos.

Para verificar o status do volume, execute este comando:

```
ibmcloud is volume $vol
```
{: pre}

## Etapa 12: conectar um volume de dados de armazenamento de bloco a uma instância.
{: #step-12-attach-a-block-storage-data-volume-to-an-instance}

Use o comando a seguir para conectar o volume à instância do servidor virtual, usando as variáveis que criamos:

```
ibmcloud is instance-volume-attachment-add helloworld-vol-attachment $vsi $vol --auto-delete true
```
{: pre}

## Etapa 13: efetuar login em sua instância do servidor virtual usando sua chave SSH privada.
{: #step-13-log-in-to-your-virtual-server-instance-using-your-private-ssh-key}

Por exemplo, é possível usar um comando deste formulário:

```
ssh -i $HOME/ .ssh/id_rsa root @ $address
```
{:pre}

Você receberá uma resposta semelhante ao exemplo a seguir. Quando for solicitado que continue se conectando, digite `yes`.

```
The authenticity of host '169.61.181.53 (169.61.181.53)' can't be established.
A impressão digital da chave ECDSA é SHA256:9MczXIwJq892DYwu0sZpQZOXORdvNXeP1aFioZpQDsM.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '169.61.181.53' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.4.0-133-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 pacotes podem ser atualizados.
0 atualizações são atualizações de segurança.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@helloworld-vsi: ~ #
```
{:screen}

## Etapa 14: Hello, World!
{: #step-14-hello-world}

Execute o comando a seguir na janela do terminal:

```
echo ` hostname ` diz "Hello, World!"
```
{:pre}

Você deve ver a saída a seguir:

```
helloworld-vsi diz Hello, World!
```
{:screen}

## Etapa 15: (opcional) começar a usar seu volume de dados de armazenamento de bloco
{: #step-15-optional-start-using-your-block-storage-data-volume}

Para usar seu volume de armazenamento de bloco como um sistema de arquivos, você precisará particionar o volume, formatar o volume e, em seguida, montá-lo como um sistema de arquivos.

No Linux, execute o comando a seguir para listar todos os volumes de armazenamento de bloco de sua instância:

```
lsblk
```
{:pre}

Você deve ver a saída como esta:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdc    202:32   0  100G  0 disk
xvdp    202:240  0   64M  0 disk
```
{:screen}

O volume `xvdc` é seu novo volume de dados de armazenamento de bloco.

Execute o comando a seguir para particionar o volume. Para começar, use o comando `n` para a nova partição.

```
fdisk /dev/xvdc
```
{:pre}

Formate o volume.

```
/sbin/mkfs -t ext3 /dev/xvdc
```
{:pre}

Rotule o volume como "hellovol".

```
/sbin/e2label /dev/xvdc /hellovol
```
{:pre}

Crie o diretório e monte o volume como um sistema de arquivos.

```
mkdir /hellovol
mount /dev/xvdc /hellovol
```
{: codeblock}

Para ver seu novo sistema de arquivos, execute o comando a seguir:

```
df -k
```
{:pre}

Você deve ver a saída como esta:

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

Para mudar o diretório para o seu novo sistema de arquivos e criar um arquivo, faça algo semelhante a:

```
cd /hellovol
touch hellovolume
```
{:codeblock}

## Etapa 15: (opcional) excluir os recursos
{: #step-15-delete-vpc-cli}

Opcionalmente, exclua os recursos. Um recurso não poderá ser excluído se ele contiver outros recursos. Por exemplo, uma nuvem particular virtual não poderá ser excluída se ela contiver sub-redes, e uma sub-rede não poderá ser excluída se contiver instâncias do servidor virtual. Em uma operação de exclusão, a API pode retornar rapidamente, mas a exclusão de recurso ainda pode estar em andamento. Após emitir a solicitação de exclusão, certifique-se de que o recurso tenha sido excluído antes de tentar excluir o recurso pai. Consulte [Excluindo um VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obter mais detalhes.

### Liberar o IP flutuante

```
ibmcloud is floating-ip-release $floating_ip
```
{: pre}

### Exclua a instância de servidor virtual

```
ibmcloud is instance-delete $vsi
```
{: pre}

### Exclua a chave

```
ibmcloud is key-delete $key
```
{: pre}

### Excluir a sub-rede

```
ibmcloud is subnet-delete $subnet
```
{: pre}

### Exclua o gateway público

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


### Exclua o VPC

```
ibmcloud is vpc-delete $vpc
```
{: pre}

### Excluir o volume (se não configurado para exclusão automática)

```
ibmcloud is volume-delete $vol
```
{: pre}

## Parabéns.
{: #cli-congratulations}

Você provisionou e conectou-se com êxito à sua instância do Virtual Private Cloud usando a CLI do IBM Cloud. Para tentar mais comandos da CLI, explore a referência integral:

* [Referência de CLI para VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference)
