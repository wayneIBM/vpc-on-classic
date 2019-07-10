---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Criando um VPC usando as APIs de REST
{: #creating-a-vpc-using-the-rest-apis}

Este documento mostra como criar os recursos do {{site.data.keyword.cloud}} Virtual Private Cloud usando as APIs de REST com `curl`. Para fragmentos de código que chamam as APIs de REST usando o Go e o Python, consulte este [código de exemplo](https://github.com/IBM-Cloud/vpc-api-samples).

## Pré-requisitos

1. Certifique-se de que você tenha uma chave SSH pública, que será usada para se conectar à instância do servidor virtual.

   Talvez você já tenha uma chave SSH pública. Procure por um arquivo chamado `id_rsa.pub` sob um diretório `.ssh` sob seu diretório inicial, por exemplo, `/Users/<USERNAME>/.ssh/id_rsa.pub`. O arquivo começa com `ssh-rsa` e termina com seu endereço de e-mail.

   Se você não tiver uma chave SSH pública ou se tiver esquecido a senha de uma existente, gere uma nova executando o comando `ssh-keygen` e seguindo os prompts.
2.  Certifique-se de que você tenha uma chave de API para sua conta do IBM Cloud. Se você não tiver uma chave de API, consulte [Criando uma chave de API](/docs/iam?topic=iam-userapikey#create_user_key). Será necessário armazenar essa chave de API em uma variável de ambiente na Etapa 1.

## Etapa 1: armazenar a chave de API como uma variável

Execute o comando a seguir para armazenar a chave de API em uma variável de ambiente:

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## Etapa 2: obter um token do IBM Identity and Access Management (IAM)

Consulte o tópico [Obtendo um token do IBM Cloud IAM usando uma chave de API](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) sobre como obter um token do IAM ou usar os comandos de exemplo a seguir.

Execute o comando a seguir para obter e analisar um token do IAM usando o utilitário `jq`. É possível modificar o comando para usar outra ferramenta de análise ou remover a última parte do comando caso prefira analisar o token manualmente, sozinho.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

Para visualizar o token do IAM, execute `echo $iam_token`. O resultado deve ser semelhante a este:

```
Bearer <your token>
```

O cabeçalho de Autorização espera que o token do IAM comece com `Bearer`. Se o resultado que você receber não incluir `Bearer`, atualize a variável `iam_token` para incluí-lo. Esses exemplos supõem que `Bearer` esteja incluído no `iam_token`.

Deve-se repetir a etapa anterior para atualizar seu token do IAM a cada hora, pois o token expira.
{: important}

## Etapa 3: armazenar o terminal de API e o parâmetro de versão como uma variável

Os terminais de API são por região e seguem a convenção `https://<region>.iaas.cloud.ibm.com`. Os terminais por região são:

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

Armazene o terminal da API em uma variável para que seja possível usá-lo em solicitações de API sem ter que digitar a URL completa. Por exemplo, para armazenar o terminal us-south em uma variável, execute este comando:

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

Para verificar se essa variável foi salva, execute `echo $rias_endpoint` e certifique-se de que a resposta não esteja vazia.

Cada solicitação de API deve incluir o parâmetro `version`, no formato `YYYY-MM-DD`. Execute o comando a seguir para armazenar a data da versão em uma variável para que ela possa ser reutilizada em sua sessão. Para obter mais informações sobre como configurar o parâmetro `version`, consulte **Versão** na [Referência da API para VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

Para verificar se essa variável foi salva, execute `echo $version` e assegure-se de que a resposta não esteja vazia.

## Etapa 4: executar a API de regiões GET

O comando a seguir retorna as regiões disponíveis para VPC, no formato JSON. Pelo menos um objeto deve ser retornado.

Um parâmetro de consulta `version` e `generation` é necessário em cada solicitação de API. Para obter detalhes, consulte a [Referência de API para VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Se você encontrar resultados inesperados, inclua o sinalizador `--verbose` (debug) após o comando `curl` para obter informações de criação de log detalhadas. Consulte também os erros comumente encontrados em nossa [página de resolução de problemas](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Etapa 5: executar a API de zonas GET

O comando a seguir retorna todas as zonas disponíveis para VPC na região `us-south`, no formato JSON. Pelo menos três objetos devem ser retornados.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Atualize o nome da região no parâmetro, `us-south` acima, dependendo do terminal da região que você está usando. Um terminal da região só sabe sobre suas próprias zonas, portanto, se você estiver usando o terminal em TÓQUIO, use `jp-tok`.
{: note}

## Etapa 6: executar a API de perfis GET

O comando a seguir retorna os perfis disponíveis para suas instâncias, no formato JSON. Pelo menos um objeto deve ser retornado.

Inclua ` | json_pp` após o comando curl para obter a sequência JSON legível
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Se a saída da API for limitada por paginação, configure o limite maior que o `total_count`, por exemplo:

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etapa 7: executar a API de imagens GET

O comando a seguir retorna as imagens disponíveis para suas instâncias, no formato JSON. Pelo menos um objeto deve ser retornado.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etapa 8: executar a API GET VPCs

O comando a seguir retorna quaisquer VPCs que tenham sido criados sob sua conta, no formato JSON.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etapa 9: criar uma nuvem particular virtual

Crie um {{site.data.keyword.cloud_notm}} VPC chamado `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

Para o restante das chamadas, você precisará saber o ID do {{site.data.keyword.cloud_notm}} VPC recém-criado. Armazene seu ID em uma variável. O comando será semelhante a este: `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## Etapa 10: criar uma sub-rede

Crie uma sub-rede em seu {{site.data.keyword.cloud_notm}} VPC. O exemplo a seguir cria um VPC na zona `us-south-2`.

O exemplo a seguir usa o [prefixo de endereço padrão](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) para a zona `us-south-2`. Padrões podem ter sido mudados para sua conta, consulte [Como planejar seus endereços de VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

Para obter uma lista de prefixos de endereços para seu VPC, execute o comando a seguir `curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`..
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Armazene o ID da sub-rede em uma variável.

```bash
subnet= "< YOUR_SUBNET_ID>"
```
{: pre}

## Etapa 11: verificar o status de sua sub-rede

Para provisionar recursos em sua sub-rede, a sub-rede deve estar no status `available`. Antes de continuar, consulte o recurso de sub-rede e certifique-se de que o status seja `available`. Se o status for `failed`, entre em contato com o [Suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) com os detalhes. É possível tentar continuar experimentando provisionar outra sub-rede.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Etapa 12: criar um gateway público

Para permitir que as instâncias de servidor virtual na sub-rede tenham acesso à Internet pública, inclua um gateway público (PGW) na sub-rede.  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Armazene o ID do gateway público em uma variável.

```bash
gateway= "< YOUR_GATEWAY_ID>"
```
{: pre}

Para anexar e usar o gateway público, o gateway público deve estar no status `available`. Antes de continuar, consulte o recurso de gateway público e certifique-se de que o status seja `available`. Se o status for `failed`, entre em contato com o [Suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) com os detalhes. É possível continuar tentando provisionar outro gateway público.

Para verificar o status do gateway público, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Etapa 13: conectar o gateway público à sub-rede

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Etapa 14: criar uma chave SSH

Crie uma chave com sua chave SSH pública. Essa chave é usada quando você cria a instância do servidor virtual. A chave também é necessária para efetuar login na instância do servidor virtual.

As chaves podem ser incluídas quando você cria pela primeira vez o VPC na IU ou na CLI. Não existe nenhuma ferramenta para incluir chaves posteriormente.
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

Armazene o ID da chave em uma variável.

```bash
key= "< YOUR_KEY_ID>"
```
{: pre}

## Etapa 15: escolher um perfil e uma imagem para a instância do servidor virtual

Execute as APIs para listar todos os perfis e imagens disponíveis para sua instância de servidor virtual e escolha uma combinação.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Para obter detalhes adicionais sobre os perfis, é possível consultar o catálogo global usando o comando da CLI do {{site.data.keyword.cloud_notm}} `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`. Por exemplo:

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

O comando a seguir lista as imagens disponíveis.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Armazene o nome do perfil e o ID da imagem em variáveis, que você usará para provisionar uma instância do servidor virtual.

```bash
profile_name = "< CHOSEN_PROFILE_NAME>" image_id = "< CHOSEN_IMAGE_ID>"
```
{: pre}

## Etapa 16: provisionar uma instância do servidor virtual

Provisione uma instância do servidor virtual (VSI) na sub-rede recém-criada. Passe a sua chave SSH pública para que seja possível efetuar login depois que a instância for provisionada.

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-sul-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

Armazene o ID da instância do servidor virtual em uma variável.

```bash
server= "< YOUR_INSTANCE_ID>"
```
{: pre}

## Etapa 17: verificar o status de sua instância do servidor virtual

O provisionamento de uma instância de servidor virtual pode levar até alguns minutos. Antes de continuar, consulte o status do servidor e certifique-se de que ele seja `em execução`.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Armazene o ID da interface de rede principal da instância do servidor virtual (retornada na chamada de API anterior) em uma variável.  

```bash
network_interface = "< YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

Não é possível obter a interface de rede primária até que você consulte a instância específica.
{: note}

## Etapa 18: criar um IP flutuante

Para criar um IP flutuante para a instância do servidor virtual, use a interface de rede primária da instância como o destino para o novo endereço IP flutuante.

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Armazene o ID de IP flutuante em uma variável.

```bash
floating_ip = "< YOUR_FLOATING_IP_ID>"
```
{: pre}

## Etapa 19: incluir uma regra no grupo de segurança padrão para permitir o tráfego SSH

Configure o grupo de segurança para definir o tráfego de entrada e de saída que é permitido para a instância.

Localize o grupo de segurança para o VPC:

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Armazene o ID do grupo de segurança em uma variável.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Agora crie uma regra para permitir o tráfego SSH de entrada:

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Etapa 20: SSH para a instância do servidor virtual

Para SSH para o servidor, use o `endereço` do IP flutuante que você criou anteriormente. Para obter o `endereço` do IP flutuante, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Use o `endereço` do IP flutuante para se conectar à instância do servidor virtual com SSH:

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

O acesso SSH ao servidor virtual pode ser evitado por grupos de segurança. Se o acesso SSH é necessário, deve-se usar um [grupo de segurança apropriado](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).
{: note}

Veja [Conectando-se à sua instância usando o Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) para obter mais informações sobre como se conectar à sua instância.

## Etapa 21: criar e conectar um volume de armazenamento de bloco

Crie um volume de armazenamento de bloco com uma solicitação semelhante a este exemplo e especifique um nome de volume, uma zona e um perfil.

Para ver uma lista de perfis de volume, forneça esta solicitação:

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

Perfis podem ser de propósito geral (3 IOPS/GB), 5iops-tier, 10iops-tier e customizados. Consulte [Sobre o Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance) para obter informações sobre a capacidade do volume e os intervalos de IOPS com base no perfil de volume selecionado.

Você não precisa fornecer um nome de volume na solicitação, pois o sistema cria um por padrão. Este exemplo especifica um nome de volume, `helloworld-vol`. Todos os nomes de volumes devem ser exclusivos.

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-sul-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

Para o restante das chamadas, você precisará saber o ID do volume recém-criado. Salve o ID do volume como uma variável, por exemplo: `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

O status do volume é `pending` quando ele é criado pela primeira vez. Antes de continuar, o volume deve mover-se para o status `available`, que leva alguns minutos.
{: note}


Para verificar o status do volume, execute o comando a seguir:

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Crie um anexo de volume para anexar o novo volume à instância do servidor virtual. Use a variável de ID da instância que você criou anteriormente na solicitação. Use o ID do volume, CRN do volume ou a URL para especificar o volume no parâmetro de dados. Esse exemplo usa a variável que criamos para o ID do volume.

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

Depois de criar o anexo do volume, você obterá o ID do anexo do volume.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Salve o ID do anexo como uma variável.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

Especifique o ID do anexo do volume para anexar o novo volume a uma instância do servidor virtual.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Etapa 22 (opcional): excluir os recursos

Opcionalmente, exclua os recursos. Um recurso não poderá ser excluído se ele contiver outros recursos. Por exemplo, uma nuvem particular virtual não poderá ser excluída se ela contiver sub-redes, e uma sub-rede não poderá ser excluída se contiver instâncias do servidor virtual. Em uma operação de exclusão, a API pode retornar rapidamente, mas a exclusão de recurso ainda pode estar em andamento. Após emitir a solicitação de exclusão, certifique-se de que o recurso tenha sido excluído antes de tentar excluir o recurso pai. Consulte [Excluindo um VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obter mais detalhes.

### Excluir o IP flutuante

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Exclua a instância de servidor virtual

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Exclua a chave

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Excluir a sub-rede

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Exclua o gateway público

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### Exclua o VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Excluir o volume

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## Parabéns.

Você provisionou com êxito uma instância de nuvem particular virtual usando as APIs de REST. Para tentar mais comandos da API, explore a referência integral:

* [Referência de API para VPC](https://{DomainName}/apidocs/vpc-on-classic)
