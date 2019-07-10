---

copyright:

  years: 2018, 2019
lastupdated: "2019-06-11"

keywords: error, message, API, limitations, rias, support

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}

# Mensagens de erro da API do IBM Cloud Virtual Private Cloud
{: #rias-error-messages}

Quando você recebe uma mensagem de erro das APIs do {{site.data.keyword.cloud}} Virtual Private Cloud, é possível usar o ID de mensagem para localizar mais informações sobre como resolver o problema.
{:shortdesc}

## account_type_invalid
**Mensagem**: deve-se ter uma conta Pré-paga para provisão de um Virtual Private Cloud.

Sua conta deve estar em um plano Pré-pago para provisão de VPCs. É possível localizar mais detalhes sobre como fazer upgrade da sua conta aqui: [Fazer upgrade da sua conta](/docs/account?topic=account-accounts#upgrade-lite-account)

## acl_in_use
**Mensagem**: a ACL de rede não pode ser excluída porque ela está anexada a recursos.

A ACL de rede não pode ser excluída porque ela está conectada a uma sub-rede ou VPC.

Para ver se uma sub-rede está usando a ACL de rede, use a API `GET /v1/subnets?version=2019-05-31&generation=1`. Comando da CLI equivalente: `ibmcloud is subnets`. Para atualizar a ACL de rede usada por uma sub-rede, use a API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’` ou a CLI `ibmcloud is subnet-update`.

Para ver se um VPC está usando a ACL de rede, use a API `GET /v1/vpcs?version=2019-05-31&generation=1`. Comando da CLI equivalente: `ibmcloud is vpcs`. Não é possível atualizar ou excluir a ACL de rede padrão usada por um VPC. A ACL de rede padrão é excluída automaticamente quando o VPC é excluído.

## address_prefix_conflict
**Mensagem**: o prefixo de endereço com esse CIDR está em uso.

O CIDR para o prefixo de endereço solicitado está em conflito com um prefixo de endereço existente.

Para visualizar a lista de prefixos de endereço de um VPC, use a API `GET /vpcs/{vpc-id}/address_prefixes` e verifique se o endereço CIDR solicitado não está em uso pelo campo `cidr` de outro prefixo de endereço. Comando da CLI equivalente: `ibmcloud is vpc-address-prefix`

## address_prefix_in_use
**Mensagem**: não é possível excluir um prefixo de endereço em uso.

Uma ou mais sub-redes estão usando o prefixo de endereço.  Para determinar quais sub-redes estão usando o prefixo de endereço, use o comando de API `GET /v1/subnets?version=2019-05-31&generation=1` e verifique o campo `ipv4_cidr_block`. Comando da CLI equivalente: `ibmcloud is subnets` e verifique a coluna `CIDR de sub-rede`. Deve-se excluir todas as sub-redes usando o prefixo de endereço antes que o prefixo de endereço possa ser excluído.

## backend_service_indisponível
** Mensagem **: o serviço de backend está indisponível.

Um serviço de nuvem de back-end que é usado pelo VPC falhou ao responder. O VPC usa vários serviços do IBM Cloud, como:

- [Identity and Asset Management](/docs/iam?topic=iam-iamoverview) (IAM)
- [Global Catalog](https://{DomainName}/catalog)
- Resource Controller
- Resource Manager

É possível verificar o [status](https://{DomainName}/status) dos serviços do IBM Cloud e tentar novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## Bad_field
**Mensagem**: o UUID de instância correto deve ser fornecido

Um dos valores fornecidos na solicitação está incorreto. Verifique o valor `target` no erro retornado para obter pistas sobre qual parâmetro estava incorreto. Em alguns casos, o UUID ou o volume na solicitação não pode ser localizado. Forneça um valor válido e tente novamente.

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter ajuda adicional. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## Bad_request
**Mensagem**: as informações fornecidas eram inválidas, estavam malformadas ou um campo obrigatório estava ausente.

A solicitação não era o que esperávamos. Use a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para ajudar a formatar a solicitação.

Se você estiver seguindo a especificação, mas ainda obtiver o erro, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## classic_access_vpc_conflict_duplicate_res
**Mensagem**: apenas um VPC com Acesso Clássico pode ser criada por região.

Apenas um VPC com Acesso Clássico pode ser criada por região. Para listar o VPC com Acesso Clássico, execute `ibmcloud is vpcs -- classic-access`. O VPC existente com Acesso Clássico deve ser excluído antes que outro com Acesso Clássico possa ser criado.

## classic_access_vpc_account_not_VRF_enabled
**Mensagem**: a conta deve ser ativada para VRF para criar um VPC de acesso clássico.

A conta clássica vinculada não é ativada para VRF. Um VPC de acesso clássico requer que a conta clássica vinculada seja ativada para VRF. Para ativar sua conta para VRF, consulte o [VRF no IBM Cloud](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).

## default_address_prefix_not_localizado
** Mensagem **: prefixo de endereço padrão não localizado.

Você poderá ver essa mensagem de erro quando o prefixo de endereço padrão não for localizado.

## duplicate_error
** Mensagem **: a entrada fornecida já existe.

O recurso especificado já existe. Tente usar um nome diferente para o recurso que você deseja criar. Por exemplo, ao criar um novo VPC, é possível listar VPCs existentes executando `ibmcloud is vpcs`. Escolha um nome que não entre em conflito.

## floating_ip_in_use
**Mensagem**: o IP flutuante está em uso.

Esse IP flutuante está associado a uma instância de servidor virtual ou gateway público.

Para ver onde o IP flutuante é usado, consulte a API `GET /v1/floating_ips/e6e4850d-123e-43a9-a224-ea10a287770e?version=2019-03-12` e olhe os valores "target". Comando da CLI equivalente: `ibmcloud is floating-ip FLOATINGIP_ID`.

## gateway_too_muitos
**Mensagem**: é possível ter somente um gateway público por zona em um VPC.

Somente um gateway público por zona é permitido em um VPC, mas o gateway público pode ser anexado a múltiplas sub-redes na zona. Para localizar o gateway público para uma zona, execute a API GET public_gateways e veja os valores "vpc" e "zone". Se estiver usando a CLI, será possível executar o comando `ibmcloud is public-gateways` e ver os valores "VPC" e "Zone".

Use o ID do gateway público para anexá-lo a uma sub-rede, veja um exemplo em nossos [Exemplos de API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis#step-13-attach-the-public-gateway-to-the-subnet-). Se estiver usando a CLI, será possível usar o comando de atualização de sub-rede para anexá-la a um gateway público, por exemplo, `ibmcloud is subnet-update SUBNET_ID --public-gateway-id PUBLIC_GATEWAY_ID`.

## health_monitor_invalid_delay
**Mensagem**: o atraso do monitor de funcionamento especificado <health_monitor_delay> é inválido

Forneça um valor entre 2 e 60 para o parâmetro `health monitor delay`.

## health_monitor_invalid_max_retries
**Mensagem**: o máximo de novas tentativas do monitor de funcionamento especificado <health_monitor_max_retries> é inválido

Forneça um valor entre 1 e 10 para `health max retries`.

## health_monitor_invalid_timeout
**Mensagem**: o tempo limite do monitor de funcionamento especificado <health_monitor_timeout> é inválido.

Forneça um valor entre 1 e 59 para `health timeout`. O valor selecionado deve ser menor que o valor de `health monitor delay`.

## health_monitor_invalid_url_path
**Mensagem**: o caminho da URL de funcionamento especificado <health_monitor_url> é inválido.

Forneça uma URL válida para o monitor de funcionamento. A URL pode conter caracteres alfanuméricos, hifens e pontos. Nenhum espaço ou barra invertida é permitido. O comprimento máximo do caminho da URL é de 255 caracteres.

## health_monitor_invalid_protocol
**Mensagem**: o protocolo do monitor de funcionamento especificado <health_monitor_protocol> é inválido.

Forneça um valor válido para o protocolo do monitor de funcionamento. Os valores aceitáveis são `http` e `tcp`.

## health_monitor_missing_protocol
**Mensagem**: o protocolo do monitor de funcionamento está ausente

O protocolo do monitor de funcionamento é um campo obrigatório. Em sua solicitação, especifique o protocolo do monitor de funcionamento. Os valores aceitáveis são `http` e `tcp`.

## health_monitor_missing_url_path
**Mensagem**: o caminho da URL do monitor de funcionamento está ausente. Ele é necessário para um monitor de funcionamento do tipo HTTP.

Para um monitor de funcionamento que usa o protocolo HTTP, o caminho da URL do monitor de funcionamento é um campo obrigatório. Forneça um caminho de URL válido.

## health_monitor_timeout_delay_conflict
**Mensagem**: o tempo limite do Monitor de funcionamento <health_monitor_timeout> não deve ser maior que ou igual ao atraso <health_monitor_delay>.

O valor do parâmetro `health monitor delay` deve ser maior que o valor do `health monitor timeout`.

## http_request_size_exceeded
**Mensagem**: a solicitação de HTTP é muito grande.

Esse problema ocorre quando a carga útil que você enviou em sua solicitação tem muitos caracteres. Tente novamente com uma carga útil menor. Por exemplo, em vez de tentar fazer tudo em uma única solicitação, tente criar um recurso mínimo em uma solicitação e, em seguida, anexar o estado a ele incrementalmente em várias solicitações subsequentes.

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter ajuda adicional. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## iam_failure
** Mensagem **: Nenhuma

Ocorreu uma falha no serviço do IAM, ao verificar a autenticação ou a autorização. A indisponibilidade do serviço do IAM pode ser temporária. Tente novamente a solicitação após alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policies_quota_exceeded
**Mensagem**: a política IKE não pode ser criada porque sua conta atingiu a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar as políticas IKE atuais, use a API `GET /ike_policies`.
Comando da CLI equivalente: `ibmcloud is ike-policies`

## ike_policy_duplicate_name
**Mensagem**: o nome `<ike_policy_name>` está em uso já pela política IKE `<ike_policy_id>`.

Forneça um nome de política IKE diferente. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_in_use
**Mensagem**: a política IKE `<ike_policy_id>` está em uso por uma ou mais conexões.

Uma política IKE não poderá ser excluída se ela estiver em uso por uma ou mais conexões.

Para ver quais conexões estão usando a política IKE, use a API `GET /ike_policies/<ike_policy_id>/connections`.
Comando da CLI equivalente: `ibmcloud is ike-policy-connections IKE_POLICY_ID`

## ike_policy_invalid_name
**Mensagem**: o nome `<ike_policy_name>` não é um nome de política IKE válido.

Um nome de política IKE válido inicia com uma letra, seguida por letras, dígitos, sublinhados ou hifens.

## ike_policy_not_created
**Mensagem**: a política IKE `<ike_policy_id>` não pôde ser criada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_deleted
**Mensagem**: a política IKE `<ike_policy_id>` não pôde ser excluída.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_localizado
**Mensagem**: a política IKE `<ike_policy_id>` não pôde ser localizada.

Você referenciou uma política IKE que não existe. Revise sua solicitação para assegurar que você tenha especificado o ID de política IKE adequado. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ike_policy_not_updated
**Mensagem**: a política IKE `<ike_policy_id>` não pôde ser atualizada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_delete_conflict
**Mensagem**: a instância não pode ser excluída no status atual.

Não será possível excluir uma instância quando ela estiver no status `deleting`, `pending`, `starting`, `stopping` ou `restarting`. A instância deve estar no status `running` ou `stopped`. Se o status não mudar, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_invalid_hostname
**Mensagem**: a instância não pode ser criada porque o nome do host não é válido.

O nome do host da instância deve atender aos requisitos a seguir:
* O nome do host deve conter apenas caracteres alfanuméricos e traços
* Caracteres maiúsculos não são permitidos
* Traços à esquerda e à direita não são permitidos
* Números à esquerda não são permitidos
* Traços consecutivos não são permitidos
* O comprimento máximo é de 15 caracteres para Windows
* O comprimento máximo é de 40 caracteres para outros sistemas operacionais

## instance_invalid_port_speed
**Mensagem**: a instância não pode ser criada porque a velocidade da porta da interface de rede especificada não é válida.

A velocidade da porta da interface de rede deve ser `100` ou `1000` Mbps.

## instance_name_exists
**Mensagem**: o mesmo nome de instância já existe.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## instance_sec_volume_over_quota
**Mensagem**: a instância não pode ser criada porque o número de anexos do volume excederia a cota.

As cotas de VPC por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas){: new_window}.

## instance_too_many_keys
**Mensagem**: a instância do Windows não pode ser criada porque a solicitação contém várias chaves.

As instâncias do Windows suportam apenas uma chave.

## insufficient_space_for_subnet
**Mensagem**: espaço insuficiente para sub-rede no prefixo de endereço.

A sub-rede não pode ser criada porque o número de endereços solicitados não pode ser alocado. Tente novamente usando uma contagem de endereços menor ou um CIDR maior.

## internal_error
** Mensagem **: ocorreu um erro interno.

Ocorreu um erro inesperado. Esse problema pode ser temporário. Tente a solicitação novamente em alguns minutos. Se esse erro persistir, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_server_error
** Mensagem **: Nenhuma

Ocorreu um erro inesperado. Esse problema pode ser temporário. Tente a solicitação novamente em alguns minutos. Se esse erro persistir, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## internal_solution
** Mensagem **: entre em contato com seu administrador.

Ocorreu um erro inesperado. Esse problema pode ser temporário. Tente a solicitação novamente em alguns minutos. Se esse erro persistir, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## invalid_generation_parameter
**Mensagem**: o parâmetro de consulta de geração deve ser configurado como 1.

Para versões a partir de 31/05/2019, o parâmetro de consulta `generation` deve ser configurado como 1 para permitir solicitações de API do VPC on Classic.

## invalid_id_format
** Mensagem **: Formato de ID inválido. Assegure-se de que o formato esteja correto.

Certifique-se de que o ID fornecido não contenha dados malformados.

É possível obter essa mensagem de erro caso você forneça uma consulta de início malformada ao fazer uma solicitação de paginação. Por exemplo,`GET /v1/network_acls?start=23fbba08-ceb3-4cbe-a951-84ff20a06069?version=2019-05-31&generation=1` contém dois `?`. Corrija a consulta e tente novamente.

## invalid_route
**Mensagem**: a rota solicitada não existe.

A rota solicitada na URL da API que você forneceu não existe. Verifique se a URL que você especificou para chamar o terminal de API está correta.

## invalid_request_field
**Mensagem**: um campo fornecido na solicitação não é válido.

Por exemplo, para atualizar a ACL de rede usada por uma sub-rede, use a API `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "network_acl":{ "id": “{network_acl_id}” } }’`. A solicitação a seguir seria inválida porque “networkacl” não é um campo válido, `PATCH /v1/subnets/{subnet_id}?version=2019-05-31&generation=1  -d '{ "networkacl":{ "id": “{network_acl_id}” } }’`

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter os valores aceitáveis.

## invalid_state
**Mensagem**: uma ação foi solicitada em um recurso que não é suportado no status atual do recurso.

Se o recurso for uma instância:

1. Uma operação de reinicialização pode já estar em andamento para a instância. Consulte as [ações permitidas](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc#error-409-conflict-when-invoking-an-action-on-an-instance), dependendo do status da instância.

2. Enquanto o status da instância estiver `pending`, não será possível executar as ações a seguir:
  * excluir a instância
  * anexar um volume à instância
  * separar um volume da instância
  * atualizar a instância

Tente a ação novamente mais tarde. Se o status do recurso não for mudado, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

Se o recurso for uma imagem:

Enquanto o status da instância for `deleting`, não será possível executar as ações a seguir:
  * corrigir a imagem
  * excluir a imagem

Não faça a solicitação novamente. A exclusão da imagem será concluída em seu próprio horário. Depois que a exclusão for concluída, todas as solicitações nessa imagem falharão com o erro `not_found` no lugar.

## invalid_version
**Mensagem**: o parâmetro `version` é inválido, ele deve estar no formato `YYYY-MM-DD`.

A versão deve obedecer ao formato _YYYY-MM-DD_. Para meses ou datas de dígito único, como 1º de janeiro, a versão deve ser semelhante a `2019-01-01`. O parâmetro de versão deve estar presente na URL. A data fornecida no parâmetro version deve ser posterior a 2019-01-01, mas antes da data atual.

## invalid_version_range
**Mensagem**: o valor de `version` não pode ser configurado em uma data futura nem antes de `2019-01-01`.

A data fornecida no parâmetro version deve ser posterior a 2019-01-01, mas antes da data atual.

## invalid_zone
**Mensagem**: verifique se os recursos que você está solicitando estão na mesma zona.

É possível ver essa mensagem ao tentar conectar um gateway público em zone-1 a uma sub-rede em zone-2.

## ipsec_policies_quota_exceeded
**Mensagem**: a política IPsec não pode ser criada porque sua conta atingiu a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar políticas IPsec atuais, use a API `GET /ipsec_policies`.
Comando da CLI equivalente: `ibmcloud is ipsec-policies`

## ipsec_policy_duplicate_name
**Mensagem**: o nome `<ipsec_policy_name>` está em uso já pela política IPsec `<ipsec_policy_id>`.

Forneça um nome de política IPsec diferente. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_in_use
**Mensagem**: a política IPsec `<ipsec_policy_id>` está em uso por uma ou mais conexões.

Uma política IPsec não poderá ser excluída se ela estiver em uso por uma ou mais conexões.

Para ver quais conexões estão usando a política IPsec, use a API `GET /ipsec_policies/<ipsec_policy_id>/connections`. Comando da CLI equivalente: `ibmcloud is ipsec-policy-connections IPSEC_POLICY_ID`

## ipsec_policy_invalid_name
**Mensagem**: o nome `<ipsec_policy_name>` não é um nome de política IPsec válido.

Um nome de política IPsec válido inicia com uma letra, seguida por letras, dígitos, sublinhados ou hifens.

## ipsec_policy_not_created
**Mensagem**: a política IPsec `<ipsec_policy_id>` não pôde ser criada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_deleted
**Mensagem**: a política IPsec `<ipsec_policy_id>` não pôde ser excluída.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_localizado
**Mensagem**: a política IPsec `<ipsec_policy_id>` não pôde ser localizada.

Você referenciou uma política IPsec que não existe. Revise sua solicitação e especifique um ID de política IPsec válido. Se você tiver certeza de que a política existe, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## ipsec_policy_not_updated
**Mensagem**: a política IPsec `<ipsec_policy_id>` não pôde ser atualizada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_content_exists
**Mensagem**: o mesmo conteúdo de chave já existe.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_name_exists
**Mensagem**: o mesmo nome de chave já existe.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## key_invalid_name
**Mensagem**: a chave não pode ser criada porque o nome não é válido.

O nome da chave deve atender aos requisitos a seguir:
* ele deve começar com o caractere alfabético, [A-Za-z]
* ele deve conter apenas caracteres alfanuméricos, traços ou sublinhados, [-A-Za-z0-9_]
* o comprimento não deve exceder 65 caracteres

## key_invalid_type
**Mensagem**: a chave não pode ser criada porque o tipo não é válido.

O único tipo de chave suportado é `rsa`.

## key_parse_failure
**Mensagem**: a chave não pode ser criada porque o valor da chave pública não é válido.

A chave pública fornecida na solicitação não é válida. Verifique o valor ou gere novamente a chave pública e tente novamente.

Nos sistemas operacionais Linux, é possível executar o comando a seguir para validar a chave pública `ssh-keygen -l -v -f <key_file>`.

## listener_certificate_not_localizado
**Mensagem**: a instância de certificado com CRN `<listener_certificate_crn>` não pode ser localizada ou não há permissão para acessar a instância de certificado.

Forneça um CRN de instância de certificado existente ou peça ao administrador da conta para conceder a você permissões de acesso para a instância de certificado fornecida.

## listener_duplicate_port
**Mensagem**: a porta `<listener_port>` é usada por outra instância do listener. Escolha uma porta diferente.

Escolha uma porta diferente.

## listener_invalid_certificate
**Mensagem**: o CRN de instância de certificado para o listener HTTPS é inválido.

Forneça um CRN da instância de certificado válida.

## listener_invalid_connection_limit
**Mensagem**: o limite de conexão do listener `<listener_connection_limit>` é inválido.

Forneça um limite de conexão válido. O limite de conexão deve ser um número inteiro entre 1 e 15000.

## listener_invalid_port
**Mensagem**: a porta do listener `<listener_port>` é inválida.

Escolha uma porta diferente.

## listener_invalid_protocol
**Mensagem**: o protocolo do listener `<listener_protocol>` é inválido.

Forneça um protocolo de listener válido. Os valores aceitáveis para o protocolo do listener são `http`, `https` e `tcp`.

## listener_missing_certificate_crn
**Mensagem**: o CRN da instância de certificado para o listener `https` está ausente.

O CRN da instância de certificado é um campo obrigatório.

## listener_missing_port
** Mensagem **: A porta do atendente está ausente.

A porta do atendente é um campo obrigatório.

## listener_missing_protocol
** Mensagem **: o protocolo Listener está ausente.

O protocolo Listener é um campo obrigatório. Forneça o protocolo do listener em sua solicitação. Os valores aceitáveis são `http`, `https` e `tcp`.

## listener_not_localizado
**Mensagem**: o listener com o ID `<listener_id>` não pode ser localizado.

Forneça um ID do listener existente.

## listener_over_quota
** Mensagem **: o Listener não pode ser criado. A cota de listeners para o recurso de balanceador de carga atingiu o limite máximo.

As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## listener_pool_protocols_conflict
**Mensagem**: o protocolo do listener (`<listener_protocol>`) e o protocolo de conjunto (`<pool_protocol>`) estão em conflito.

Um listener com o protocolo `https` ou `http` pode ser associado somente a um conjunto com o protocolo `http`. Da mesma forma, um listener `tcp` pode ser associado somente a um conjunto `tcp`.

## listener_reserved_port_conflict
**Mensagem**: a porta do listener `<listener_port>` é uma das portas reservadas. O intervalo de portas de 56500 a 56520 é reservado para propósitos de gerenciamento. Escolha uma porta diferente.

Escolha uma porta diferente.

## load_balancer_delete_conflict
**Mensagem**: o balanceador de carga com o ID <load_balancer_id> não pode ser excluído porque seu status é um destes:  
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Exclua o balanceador de carga quando ele estiver no status ACTIVE.

## load_balancer_duplicate_name
**Mensagem**: o nome `<load_balancer_name>` é usado por outra instância do balanceador de carga. Escolha um nome diferente.

Forneça um nome diferente para a instância do balanceador de carga. O nome do balanceador de carga deve ser exclusivo dentro do VPC e da conta.

O Load Balancer for VPC não estará em conflito com seu Cloud Load Balancer, pois os nomes de domínio DNS são diferentes para esses serviços e o nome do Load Balancer for VPC não está incluído no nome DNS.
{: note}

## load_balancer_insufficient_ips
**Mensagem**: a sub-rede com ID(s) <subnet_id> tem endereços IPv4 insuficientes disponíveis.

Em sua solicitação, forneça uma sub-rede válida com endereços IPv4 disponíveis, em que você deseja criar o balanceador de carga.

## load_balancer_invalid_description
**Mensagem**: a descrição do balanceador de carga `<load_balancer_description>` não é válida.

Uma descrição do balanceador de carga não pode exceder 255 caracteres.

## load_balancer_invalid_is_public
**Mensagem**: o valor de is_public `<load_balancer_is_public>` não é válido.

O valor do parâmetro do balanceador de carga `is_public` deve ser _true_ ou _false_.

## load_balancer_invalid_name
**Mensagem**: o nome `<load_balancer_name>` é inválido.

Os nomes podem não estar vazios. Um nome do balanceador de carga válido é iniciado com uma letra, seguida por letras, dígitos ou sublinhados. O comprimento do nome não pode exceder 40 caracteres.

## load_balancer_invalid_subnet
**Mensagem**: a sub-rede com o ID <subnet_id> não é válida. Certifique-se de usar uma sub-rede existente com o status 'available'.

Forneça sub-redes válidas que estão no status `available` ao inserir sua solicitação para criar um balanceador de carga.

## load_balancer_missing_is_public
** Mensagem **: o campo 'is_public ' está ausente.

O campo `is_public` é obrigatório. Forneça um valor para o campo `is_public`.

## load_balancer_missing_name
** Mensagem **: o nome do balanceador de carga está ausente.

O balanceador de carga `name` é um campo obrigatório. Forneça um nome para o balanceador de carga.

## load_balancer_missing_subnets
** Mensagem **: as sub-redes do balanceador de carga estão ausentes.

O campo `subnets` é obrigatório. Em sua solicitação para criar um balanceador de carga, forneça informações sobre as sub-redes nas quais você deseja criar o balanceador de carga.

## load_balancer_missing_subnet_id
**Mensagem**: o ID da sub-rede está ausente.

O **ID da sub-rede** é um campo obrigatório. Forneça um ID de sub-rede com seu comando.

## load_balancer_not_localizado
**Mensagem**: o balanceador de carga com o ID `<load_balancer_id>` não pode ser localizado.

Forneça o ID de um balanceador de carga existente.

## load_balancer_over_quota
** Mensagem **: o balanceador de carga não pode ser criado. A cota de instâncias do balanceador de carga sob sua conta atingiu o limite máximo.

Exclua um balanceador de carga existente ou entre em contato com o suporte para aumentar a cota do balanceador de carga em sua conta.

As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## load_balancer_subnet_not_found
**Mensagem**: a sub-rede com o ID <subnet_id> não pode ser localizada.

Em sua solicitação, deve-se fornecer IDs de sub-redes existentes nas quais deseja criar o balanceador de carga.

## load_balancer_unchanged_update
**Mensagem**: não há nada para atualizar o balanceador de carga com o ID `<load_balancer_id>`.

Nenhuma informação foi fornecida para atualizar uma instância do balanceador de carga com o ID que você especificou.

## load_balancer_update_conflict
**Mensagem**: o balanceador de carga com o ID <load_balancer_id> não pode ser atualizado porque seu status é um destes:
* UPDATE_PENDING
* CREATE_PENDING
* DELETE_PENDING
* MAINTENANCE_PENDING

Atualize o balanceador de carga quando ele estiver no status ACTIVE.

## member_duplicate_address_and_port
**Mensagem**: o membro com o endereço <member_address> e a porta <member_port> já existe no conjunto.

Forneça um endereço de membro exclusivo e uma combinação de porta em sua solicitação.

## member_invalid_address
**Mensagem**: o endereço IP do membro especificado <member_ip> é inválido.

Em sua solicitação, forneça um endereço IPv4 CIDR válido para o membro.

## member_invalid_port
**Mensagem**: a porta do membro especificada `<member_port>` é inválida.

Forneça um número de porta válido. Um número de porta válido está no intervalo de 1 a 65535.

## member_invalid_weight
**Mensagem**: o peso do membro especificado `<member_weight>` é inválido.

Forneça um peso de membro válido. O valor deve ser um número inteiro entre 0 e 100.

## member_ip_not_found
**Mensagem**: o membro com o endereço IP <member_IP> não pertence a nenhuma sub-rede na Região e no VPC do balanceador de carga.

Em sua solicitação, forneça o endereço de um membro que pertence a uma sub-rede na mesma Região e o VPC como o balanceador de carga.

## member_missing_address
** Mensagem **: o endereço do membro está ausente.

O endereço do membro é um campo obrigatório. Forneça um valor para o endereço do membro.

## member_missing_protocol_port
** Mensagem **: a porta do membro está ausente.

A porta do membro é um campo obrigatório. Forneça um valor para a porta do membro.

## member_not_found
**Mensagem**: o membro com o ID <member_id> não pode ser localizado.

Forneça um ID de membro existente.

## member_over_quota
** Mensagem **: o membro não pode ser criado. A cota de instâncias do membro sob o conjunto atingiu o limite máximo.

As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## missing_generation_parameter
**Mensagem**: o parâmetro de consulta de geração é necessário.

Para versões a partir de 31/05/2019, o parâmetro de consulta `generation` é necessário para solicitações de API do VPC on Classic.

## missing_ims_account_id
** Mensagem **: Nenhuma

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## missing_version
**Mensagem**: o parâmetro `version` é necessário e deve estar no formato `YYYY-MM-DD`.

Um parâmetro de versão é necessário para todas as solicitações de API. A versão deve obedecer ao formato _YYYY-MM-DD_. Para meses ou datas de dígito único, como 1º de janeiro, a versão deve ser semelhante a `2019-01-01`. A data fornecida no parâmetro version deve ser posterior a 2019-01-01, mas antes da data atual. Confira estes [exemplos de API](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) para saber como fornecer o parâmetro de versão.

## network_conflict
**Mensagem**: o CIDR está em conflito com o Prefixo de endereço existente no VPC.

Você poderá ver essa mensagem se você forneceu um CIDR de rede que entra em conflito com um CIDR de rede existente no mesmo espaço de IP.

Para localizar os CIDRs existentes em Prefixos de endereço para o VPC, execute o comando de API `GET /v1/vpcs/<vpc-id>/address_prefixes` e veja os valores de `cidr` da resposta. Verifique o valor de `cidr` para se certificar de que o CIDR que você forneceu não esteja em uso por outros Prefixos de endereço.

Se você estiver usando a CLI, execute o comando `ibmcloud is vpc-addrs <vpc-id>` e verifique o valor do `CIDR Block` para conflitos.

Para localizar os CIDRs existentes em sub-redes, execute o comando de API `GET /v1/subnets` para listar todas as sub-redes para o VPC. Em seguida, execute o comando da API `GET /v1/subnets/<subnet-id>` para cada sub-rede no VPC e verifique o valor de `ipv4_cidr_block` na resposta. Verifique o valor de `ipv4_cidr_block` para se certificar de que o CIDR que você forneceu não esteja em uso por outros Prefixos de endereço.

Se você estiver usando a CLI, execute o comando `ibmcloud is subnets` para listar todas as sub-redes para o VPC. Em seguida, execute o comando `ibmcloud is subnet <subnet-id>` para cada sub-rede no VPC e verifique se há conflitos com o valor de `IPv4 CIDR` na saída da CLI.

## not_authorized
** Mensagem **: A solicitação não está autorizada.

É possível ver que esse erro é se o token do IAM está ausente ou expirado. Para obter instruções sobre como gerar um token, consulte [Criando um VPC usando as APIs de REST](/docs/infrastructure/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis). Se o token não estiver expirado, certifique-se de que a conta que você está usando esteja autorizada a usar essa função e que tenha as [permissões corretas](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Se você tiver a autorização correta, mas ainda estiver obtendo esse erro, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_localizado
**Mensagem**: verifique se o recurso que você está solicitando existe.

Você referenciou um recurso que não existe ou para o qual não tem acesso. Revise sua solicitação para assegurar que você tenha especificado os IDs e referências adequados.

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter ajuda adicional. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## not_implementada
** Mensagem **: Nenhuma

O método não está implementado. Verifique sua solicitação para se certificar que você está chamando uma [API válida](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. [Entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) se você espera que essa API seja implementada.

## not_in_address_prefix
**Mensagem**: o CIDR fornecido não se ajusta a nenhum dos prefixos de endereço na zona fornecida.

Esse erro ocorrerá se um usuário estiver tentando criar uma sub-rede com um CIDR que não caia em nenhum prefixo de endereço para a zona especificada.

Execute `GET /vpcs/{vpc_id}/address_prefixes` para obter a lista de prefixos de endereços para o VPC. Se estiver usando a CLI, será possível executar `ibmcloud is vpc-address-prefixes` para listar todos os prefixos de endereço para a seu VPC. Consulte os valores de `cidr` e `zone` da resposta e certifique-se de que o `cidr` da sub-rede seja um subconjunto do `cidr` do prefixo de endereço para a zona que você está tentando criá-lo.

## over_quota
**Mensagem**: a solicitação excederia a cota para um tipo de recurso.

As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## password_not_ready
** Mensagem **: Nenhuma

Sua instância deve estar em execução antes que seja possível recuperar a senha. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## pool_delete_conflict
**Mensagem**: o conjunto não pode ser excluído porque ele ainda está associado a um ou mais listeners.

Certifique-se de que o conjunto esteja desassociado de quaisquer listeners e tente novamente.

## pool_duplicate_name
**Mensagem**: o nome do conjunto `<pool_name>` já está sendo usado por outro conjunto. Escolha um nome diferente.

Escolha um nome de conjunto diferente.

## pool_invalid_name
**Mensagem**: o nome <pool_name> é inválido.

Forneça um nome de conjunto válido. Um nome do balanceador de carga válido é iniciado com uma letra seguida por letras, dígitos, hifens. O comprimento do nome não pode exceder 40 caracteres.

## pool_invalid_session_persistence
**Mensagem**: o valor de persistência de sessão do conjunto não é válido.

Forneça um valor de persistência de sessão válido. O valor aceitável é `SOURCE_IP`.

## pool_missing_algorithm
** Mensagem **: o algoritmo de balanceamento de carga está ausente.

O algoritmo de balanceamento de carga é um campo obrigatório. Forneça o algoritmo de balanceamento de carga em sua solicitação. Os valores aceitáveis são `round_robin`, `weighted_round_robin` e `least_connections`.

## pool_missing_health_monitor
** Mensagem **: o monitor de funcionamento do conjunto está ausente.

O monitor de funcionamento do conjunto é um campo obrigatório.

## pool_missing_members
**Mensagem**: os membros do conjunto estão ausentes.

Forneça membros do conjunto.

## pool_missing_name
**Mensagem**: o nome do conjunto está ausente.

O nome do conjunto é um campo obrigatório.

## pool_missing_protocol
** Mensagem **: O protocolo do conjunto está ausente.

O protocolo do conjunto é um campo obrigatório. Forneça o protocolo do conjunto em sua solicitação. Os valores aceitáveis são `http` e `tcp`.

## pool_not_localizado
**Mensagem**: o conjunto com o ID `<pool_id>` não pode ser localizado.

Forneça um ID do conjunto existente.

## pool_over_quota
** Mensagem **: o conjunto não pode ser criado. A cota de conjuntos para o recurso de balanceador de carga atingiu o limite máximo.

As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/infrastructure/vpc/?topic=vpc-quotas){: new_window}.

## public_gateway_in_use
**Mensagem**: não é possível excluir um gateway público quando ele está em uso.

O gateway público está atualmente anexado a uma ou mais sub-redes. Deve-se remover o gateway público de todas as sub-redes antes de poder excluí-lo.

Para ver qual sub-rede está usando o gateway público, use a API `GET /v1/subnets?version=2019-05-31&generation=1`. Comando da CLI equivalente: `ibmcloud is subnets`. Para separar o gateway público da sub-rede, use o comando da API `DELETE /v1/subnets/{subnet_id}/public_gateway?version=2019-05-31&generation=1` ou o comando da CLI `ibmcloud is subnet-public-gateway-detach`.

## rate_limit_exceeded
**Mensagem**: muitas solicitações dentro de um curto tempo.

Essa mensagem de erro será retornada se muitas solicitações forem recebidas dentro de um intervalo de tempo especificado. Espere um pouco e tente novamente.

## security_group_active_transactions
**Mensagem**: a interface não pode ser anexada ou removida até que a instância apareça no estado Ativo.

É necessário que a instância esteja ativa antes que suas interfaces de rede possam ser anexadas a um grupo de segurança. Use `GET /v1/instances/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance` para verificar o status da instância. Quando o status for `running`, tente novamente. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_already_attached
**Mensagem**: a interface já está anexada ao grupo de segurança.

A interface já está anexada ao grupo de segurança. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` para visualizar as interfaces vinculadas.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_exists
** Mensagem **: o grupo de segurança já existe.

Os nomes dos grupos de segurança devem ser exclusivos em um VPC. Um grupo de segurança com esse nome já existe no VPC de destino. Use `GET /v1/security_groups?version=2019-05-31&generation=1` ou `ibmcloud is security-groups` para visualizar os grupos de segurança atuais.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic) ou o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_attached
**Mensagem**: não é possível excluir o grupo de segurança enquanto as interfaces são anexadas.


Remova todas as interfaces antes de excluir o grupo de segurança. Use `DELETE /v1/security_groups/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interface-remove` para separar uma interface.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic) ou o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_interfaces_per_sg_exceeded
**Mensagem**: limite excedido de interfaces por grupo de segurança.

A anexação de outra interface ao grupo de segurança excederia o limite de interfaces por grupo de segurança. As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Avalie sua estratégia e considere criar outro grupo de segurança com regras semelhantes.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic) ou o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_last_security_group_is_default
**Mensagem**: o grupo de segurança padrão não pode ser removido quando ele é o único grupo de segurança anexado.

Uma interface de rede deve ser anexada a pelo menos um grupo de segurança.
Ela será anexada ao grupo de segurança padrão do VPC, se um não for especificado.
Anexe a interface a um grupo de segurança diferente e, em seguida, tente novamente removê-la do grupo de segurança padrão.
Para obter informações sobre o grupo de segurança padrão, consulte o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_limit_exceeded
** Mensagem **: limite do grupo de segurança excedido.

Você tentou criar um novo grupo de segurança, mas está atualmente em sua cota de conta. As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Avalie sua estratégia para designar instâncias a grupos de segurança. Muitas vezes, é possível reduzir o número geral de grupos de segurança designando múltiplas instâncias ao mesmo grupo de segurança. Essa estratégia reduzirá o número de grupos de segurança e fará você ficar abaixo sua cota de conta. Em casos raros, geralmente para grandes organizações, há a necessidade de expandir a cota. Nesse caso, [entre em contato com o suporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para consultar se isso é possível.

## security_group_network_interface_not_active
**Mensagem**: a interface não pode ser anexada porque ela não está ativa.

As interfaces de rede devem estar ativas antes de anexar a grupos de segurança. Use `GET /v1/instances/{id}/network_interfaces/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance-network-interface` para visualizar o status de uma interface. Quando o status for 'available', tente novamente. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_not_attached
** Mensagem **: A interface não está conectada.

A interface não está anexada ao grupo de segurança. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` para visualizar as interfaces vinculadas.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic) ou o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-security-groups-using-the-cli){: new_window}.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).


## security_group_not_in_vpc
**Mensagem**: a interface e o grupo de segurança estão em VPCs diferentes.

Para anexar uma interface de rede a um grupo de segurança, a instância deve estar no mesmo VPC que o grupo de segurança. Use `GET /v1/security_groups/{id}?version=2019-05-31&generation=1` ou `ibmcloud is security-group` para visualizar detalhes dos grupos de segurança e o VPC. Use `GET /v1/instances/{id}?version=2019-05-31&generation=1` ou `ibmcloud is instance` para visualizar os detalhes da instância e o VPC.

## security_group_order_bindings
**Mensagem**: não é possível excluir o grupo de segurança enquanto ele tem ordens de instância pendentes.

Se uma instância foi criada com um ou mais grupos de segurança especificados nas interfaces de rede, a instância e as interfaces de rede deverão estar ativas antes que o grupo de segurança possa ser excluído. Use `GET /v1/security_groups/{id}/network_interfaces?version=2019-05-31&generation=1` ou `ibmcloud is security-group-network-interfaces` para visualizar as interfaces da rede conectadas e seus status. Quando o status das interfaces for 'available', tente novamente. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## security_group_port_max_less_than_port_min
**Mensagem**: a porta máxima do TCP/UDP não pode ser menor que a porta mínima.

O valor máximo de porta não pode ser menor que o valor mínimo de porta. Especifique um valor máximo de porta que seja maior que o valor mínimo de porta.

## security_group_port_range_both_or_neither
**Mensage **: o intervalo de portas deve ser desconfigurado ou uma porta mínima e uma máxima devem ser configuradas para 'tcp' e 'udp'.

As regras com um protocolo 'tcp' ou 'udp' podem ter um intervalo de portas desconfigurado (para aplicar a regra a todo o tráfego) ou elas podem especificar um intervalo de portas. Para restringir o tráfego para uma única porta, configure as portas mínima e máxima para o valor de porta. Por exemplo, o comando a seguir criaria uma regra para permitir SSH na porta 22: `ibmcloud is security-group-rule-add <id> inbound tcp -- port-min 22 --port-max 22`.

Para obter informações adicionais sobre configurações de regra do grupo de segurança, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic).


## security_group_port_range_invalid_protocol
**Mensagem**: um intervalo de portas foi especificado com um protocolo 'icmp'. Um intervalo de portas é válido somente para um protocolo 'tcp' ou 'udp'.

As regras com um protocolo 'icmp' têm um tipo e um código. As regras com os protocolos 'tcp' ou 'udp' têm um intervalo de portas. Para obter informações adicionais sobre configurações de regra do grupo de segurança, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_remote_group_not_in_vpc
**Mensagem**: o grupo remoto não está no mesmo VPC que este grupo de segurança.

As regras do grupo de segurança podem se referir a um grupo de segurança remoto, permitindo o tráfego entre as interfaces anexadas dos dois grupos. Os grupos de segurança remotos devem estar no mesmo VPC. Use `GET /v1/security_groups?2019-01-01` ou `ibmcloud is security-groups` para visualizar os grupos de segurança atuais e seus VPCs.

Para obter instruções adicionais para corrigir esse problema, consulte o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.


## security_group_remoting_rules
**Mensagem**: não é possível excluir o grupo de segurança enquanto regras remotas são anexadas.

O grupo de segurança contém uma ou mais regras que referenciam um grupo de segurança remoto. Use `GET /v1/security_groups/{id}/rules?version=2019-05-31&generation=1` ou `ibmcloud is security-group-rules` para visualizar as regras. O campo 'remoto' especificará quaisquer grupos de segurança remotos. Tente novamente após excluir ou editar a regra de acesso remoto.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic) ou o [documento Usando grupos de segurança](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups){: new_window}.

## security_group_remoting_rules_per_sg_exceeded
**Mensagem**: limite excedido de regras remotas por grupo de segurança.

A inclusão de outra regra que se refere a um grupo de segurança remoto excederia o limite de regras remotas por grupo de segurança. As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}.

## security_group_rules_per_sg_exceeded
**Mensagem **: limite excedido de regras por grupo de segurança.

A inclusão de uma regra excederia o limite de regras por grupo de segurança. As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Avalie sua estratégia e considere criar outro grupo de segurança.

## security_group_sgs_per_interface_exceeded
**Mensagem**: limite excedido de grupos de segurança por interface.

Anexar essa interface a outro grupo de segurança excederia o limite de grupos de segurança por interface. As cotas por recurso são especificadas em [Cotas e limites para o VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#security-groups-quotas){: new_window}. Avalie sua estratégia e considere incluir regras em um grupo de segurança existente.


## security_group_type_code_invalid_protocol
**Mensagem**: um tipo/código de 'icmp' foi fornecido, mas o protocolo solicitado não era 'icmp'. Configure o protocolo como 'icmp' ou especifique um intervalo de portas.

Somente regras com um protocolo 'icmp' têm um tipo e um código. As regras com os protocolos 'tcp' ou 'udp' têm um intervalo de portas. Para obter informações adicionais sobre configurações de regra do grupo de segurança, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic).

## security_group_vpc_default
**Mensagem**: não é possível excluir o grupo de segurança padrão para um VPC.

O grupo de segurança padrão não pode ser excluído. Para obter informações sobre o grupo de segurança padrão, consulte o guia em [Atualizando o grupo de segurança padrão](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-updating-the-default-security-group).

## service_manager_service_failure
** Mensagem **: Nenhuma

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_conflict
**Mensagem**: o CIDR entra em conflito com a Sub-rede existente no VPC.

Execute a API `GET /subnets` para recuperar todas as sub-redes no VPC. Verifique o valor de `ipv4_cidr_block` para se certificar de que o CIDR que você forneceu não esteja em uso por outras sub-redes.

Se estiver usando a CLI, será possível executar `ibmcloud is subnets` e consultar o valor "Subnet CIDR" quanto a conflitos.

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter ajuda adicional. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty
**Mensagem**: não é possível excluir a sub-rede até que ela esteja vazia. Exclua todos os recursos na sub-rede e tente novamente.

Houve uma solicitação para excluir uma sub-rede, mas a sub-rede ainda contém recursos. As sub-redes podem conter instâncias, VPNs, balanceadores de carga ou gateways públicos. Deve-se excluir seus recursos na sub-rede antes de poder excluir a sub-rede.

Em algumas situações, esse erro pode ocorrer mesmo quando o console mostra 0 VSI e 0 balanceador de carga. A exclusão é assíncrona e pode demorar alguns minutos para que o status interno mude. Tente a exclusão da sub-rede novamente em alguns minutos.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_pgway_exists
**Mensagem**: não é possível excluir a sub-rede enquanto ela está anexada a um gateway público. Separe o gateway público e tente novamente.

Houve uma solicitação para excluir uma sub-rede, mas a sub-rede ainda tem um gateway público anexado a ela. Deve-se excluir ou separar o gateway público antes de poder excluir a sub-rede.

Se estiver usando a CLI, será possível executar `ibmcloud is public-gateways` para listar os gateways públicos e `ibmcloud is sub-net-public-gateway-detach` para remover um gateway público de uma sub-rede.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_ipaddr_exists
**Mensagem**: não é possível excluir a sub-rede enquanto ela contém endereços IP. Exclua qualquer instância do servidor associada ao endereço IP e tente novamente.

Houve uma solicitação para excluir uma sub-rede, mas a sub-rede ainda contém endereços IP. Deve-se excluir a instância do servidor associada ao endereço IP antes de poder excluir a sub-rede.

Se estiver usando a CLI, será possível executar `ibmcloud is instances` para listar as instâncias do servidor. Observe o valor "Address" para determinar seu Endereço IP e "Status" para determinar seu status. Execute `ibmcloud is instance-delete` para excluir uma instância do servidor que ainda não tenha um status de "deleting".

Em algumas situações, esse erro poderá ocorrer mesmo quando o console mostrar 0 VSI. A exclusão é assíncrona e pode demorar alguns minutos para que o status interno mude. Tente a exclusão da sub-rede novamente em alguns minutos.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_loadbalancer_exists
**Mensagem**: não é possível excluir a sub-rede enquanto ela contém um balanceador de carga. Exclua o balanceador de carga e tente novamente.

Houve uma solicitação para excluir uma sub-rede, mas a sub-rede ainda contém um balanceador de carga. Deve-se excluir o balanceador de carga antes de poder excluir a sub-rede.

Se estiver usando a CLI, será possível executar `ibmcloud is load-balancers` para listar os balanceadores de carga. Consulte o valor "Subnets" para determinar sua sub-rede e "Status" para determinar seu status. Execute `ibmcloud is load-balancer-delete` para excluir um balanceador de carga que ainda não tenha um status de "deleting".

Em algumas situações, esse erro pode ocorrer mesmo quando o console mostra 0 balanceador de carga. A exclusão é assíncrona e pode demorar alguns minutos para que o status interno mude. Tente a exclusão da sub-rede novamente em alguns minutos.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_not_empty_vpn_gway_exists
**Mensagem**: não é possível excluir a sub-rede enquanto ela contém um gateway VPN. Exclua o gateway de VPN e tente novamente.

Houve uma solicitação para excluir uma sub-rede, mas a sub-rede ainda tem um gateway VPN anexado a ela. Deve-se excluir o gateway de VPN antes de poder excluir a sub-rede.

Se estiver usando a CLI, será possível executar `ibmcloud is vpn-gateways` para listar os gateways de VPN. Consulte o valor "Subnets" para determinar sua sub-rede e "Status" para determinar seu status. Execute `ibmcloud is vpn-gateway-delete` para excluir um gateway de VPN que ainda não tenha um status de "deleting".

Em algumas situações, esse erro pode ocorrer mesmo quando o console mostra 0 gateway de VPN. A exclusão é assíncrona e pode demorar alguns minutos para que o status interno mude. Tente a exclusão da sub-rede novamente em alguns minutos.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## subnet_unknown_state
**Mensagem**: a sub-rede `<subnet_id>` está em um estado inválido para a operação solicitada.

A sub-rede deve estar no status `available` antes de poder operá-la. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## target_in_use
**Mensagem**: o destino já tem IP flutuante anexado.

Houve uma solicitação para anexar um endereço IP flutuante à interface de rede do servidor, mas a interface de rede já possui um IP flutuante anexado a ela.

Para localizar o endereço IP flutuante atualmente anexado a uma interface de rede, execute o comando de API `GET /v1/floating_ips?version=2019-05-31&generation=1` e procure o ID da interface de rede no campo `target.id`.  

Se você estiver usando a CLI, execute o comando `ibmcloud is floating-ips` e consulte o valor `Target`. Esteja ciente de que a CLI trunca o ID da interface de rede no primeiro caractere traço ("-"). Por exemplo, a interface de rede primária de um servidor com um ID `abdfcb29-b3c5-4e4a-b7a0-cf0300154699` é exibida como `primary(abdfcb29-.)` na saída da CLI.

## token_invalid
**Mensagem**: o token de serviço estava expirado ou era inválido.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## token_missing
**Mensagem **: o token de serviço estava vazio ou não existia na solicitação.

Recrie um token e tente novamente.

## validation_enum
**Mensagem**: o valor fornecido não era uma opção válida.

Um dos campos fornecidos tem um valor que deve vir de uma enumeração de valores possíveis.

Para regras de ACL de rede, é possível especificar uma direção:

```
direction*	string
Whether the traffic to be matched is inbound or outbound

Enum: [ inbound, outbound ]
```

Por exemplo, o valor a seguir seria inválido, porque `northbound` não é uma opção válida na enumeração `[ inbound, outbound ]`.

```json
{
    "direction": "northbound"
}
```

Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} ou a [referência da CLI](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference){: new_window} para obter os valores aceitáveis.

## validation_failure
**Mensagem**: o JSON fornecido não correspondeu à estrutura esperada.

Para corrigir esse problema, certifique-se de que o conteúdo de sua solicitação seja JSON válido e que sua solicitação se adéque à [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_invalid_cidr
**Mensagem**: o valor não é um CIDR válido.

O valor deve ser um bloco CIDR interno válido com uma parte de 0 host.

Determinados intervalos de endereços IP são reservados. Mais informações sobre os intervalos de IP reservados estão disponíveis na visão geral de [Usando seu VPC com regiões e sub-redes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv4_cidr
**Mensagem**: o valor não é um CIDR IPv4 válido.

Deve ser um bloco CIDR interno IPv4 com uma parte de 0 host.

Determinados intervalos de endereços IP são reservados. Mais informações sobre os intervalos de IP reservados estão disponíveis na visão geral de [Usando seu VPC com regiões e sub-redes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets){: new_window}.

## validation_invalid_ipv6_cidr
**Mensagem**: o valor não é um CIDR IPv6 válido.

Atualmente, o IPv6 não é suportado. Use um endereço IPv4.

## validation_invalid_address
**Mensagem**: o valor não é um endereço válido.

Deve ser um endereço IP válido.

Uma lista de endereços IP reservados individualmente é fornecida no documento [Regiões e sub-redes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets).

## validation_invalid_ipv4_address
**Mensagem**: o valor não é um endereço IPv4 válido.

Forneça um endereço IPv4 válido.

## validation_invalid_ipv6_address
**Mensagem**: o valor não é um endereço IPv6 válido.

Forneça um endereço IPv6 válido. Atualmente, o IPv6 não é suportado; use um endereço IPv4.

## validation_invalid_field_type
**Mensagem**: o tipo de valor não corresponde ao tipo de campo.

Para corrigir esse problema, certifique-se de que o conteúdo de sua solicitação se adéque à [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para o terminal que você está chamando.

## validation_max_value
**Mensagem**: um valor fornecido para um parâmetro é maior que o permitido.

Forneça um valor menor que atenda ao máximo conforme fornecido pela [especificação](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_min_value
**Mensagem**: um valor fornecido para um parâmetro é menor que o permitido.

É possível que você obtenha esse erro caso tente criar uma sub-rede com `total_ipv4_address_count` menor que 8.

Forneça um valor maior que atenda ao mínimo conforme fornecido pela [especificação](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_not_null
**Mensagem**: o campo fornecido deve ser nulo.

Certifique-se de que o valor seja nulo. Consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

Aqui está um exemplo inválido:

```json
{
    "name": null }
```

## validation_only_one
**Mensagem**: o valor deve corresponder a um dos subesquemas.

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## validation_discriminator_UNK
**Mensagem**: o campo discriminador proíbe essa subestrutura.

Por exemplo, se você especificar
```
{
protocolo: icmp port_min: 5 port_max: 100
}
```

O protocolo é `icmp` e _port_min_ é um campo `tcp`, portanto, você obterá um erro.
1. validation_discriminator_required para regras de  ` icmp `  ausentes
2. validation_discriminator_forbidden para campos `tcp` com `icmp` especificado

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_internal_error
** Mensagem **: Nenhuma

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_name
**Mensagem**: o valor não é um nome válido.

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_invalid_ref
**Mensagem**: o valor não é um HREF válido.

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_non_empty_uuid
** Mensagem **: Nenhuma

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_required_field
** Mensagem **: Ausente um campo obrigatório.

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## validation_unique_failed
**Mensagem**: o campo fornecido deve ser exclusivo.

Você pode ter especificado um nome que já esteja em uso. Tente um valor diferente.

Certifique-se de que sua solicitação esteja em conformidade com a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_attachment_delete_invalid_request
**Mensagem**: o anexo do volume não pode ser excluído.

Esta operação não é permitida. O anexo do volume de inicialização não pode ser excluído porque o volume de inicialização é requerido pela instância.

## volume_attachment_update_invalid_request
**Mensagem**: o anexo do volume não pôde ser atualizado porque a solicitação é inválida.

O anexo do volume não pôde ser atualizado por qualquer um destes motivos:

* O anexo do volume de inicialização não pode ser renomeado
* O campo do anexo do volume de inicialização `delete_volume_on_instance_delete` não pode ser configurado como `true`

## volume_capacity_max
**Mensagem**: a capacidade de volume máxima permitida é 2.000 GB.

A capacidade de volume especificada excede a capacidade máxima de volume. Especifique um valor entre 10 e 2.000 GB. Consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para obter intervalos de capacidade aceitos para um perfil customizado.

## volume_capacity_missing
**Mensagem**: o valor da capacidade do volume necessário está ausente na solicitação.

Ao criar um volume, deve-se especificar uma capacidade de volume com base nessa definição de perfil. Para obter mais informações, consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about).

## volume_capacity_zero_or_negative
**Mensagem**: a capacidade do volume deve ser maior que zero.

Ao criar um volume, o valor da capacidade especificado na solicitação deve ser um número positivo entre 10 GB e 2.000 GB. Consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para obter os valores de capacidade de volume suportados.

## volume_crn_account_id_mismatch
**Mensagem**: o CRN especificado na solicitação não pertence à conta do usuário atual.

O CRN da chave raiz do Key Protect não corresponde ao ID da conta de autorização do IAM. Especifique um CRN de chave raiz do Key Protect diferente para sua conta do IAM. Consulte a documentação [Key Protect](/docs/services/key-protect?topic=key-protect-getting-started-tutorial) para obter mais informações.

## volume_crn_cname_mismatch
**Mensagem**: o CRN especificado na solicitação não pode ser usado para o ambiente atual.

O CRN que você especificou na solicitação não pode ser usado no ambiente atual. Forneça um CRN aplicável ao seu ambiente.

## volume_encryption_key_crn_invalid
**Mensagem**: o CRN da chave de criptografia do volume especificado na solicitação não é válido.

O CRN de Key Protect que você forneceu não é válido. Obtenha o CRN da chave raiz no serviço Cloud Identity and Access Management. Para obter mais informações, consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption).

## volume_encryption_key_not_active
**Mensagem**: a chave de criptografia de volume especificada na solicitação não está ativa.

Ative a chave existente ou forneça uma nova chave. Se não for possível ativar sua própria chave, entre em contato com o administrador do sistema ou com o [suporte ao cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## volume_encryption_key_not_authorized
**Mensagem**: você não está autorizado a acessar a chave de criptografia de volume fornecida.

Forneça outra chave de criptografia para a qual você tenha acesso ou entre em contato com o administrador do sistema para obter acesso à chave atual.

## volume_encryption_key_not_implemented
O recurso de chave de criptografia do volume não é suportado.

Um volume não pôde ser criado usando sua chave de criptografia. Esta versão suporta apenas volumes com criptografia gerenciada pelo provedor. Para usar sua própria chave de criptografia, use uma versão do Block Storage for VPC que suporte a criptografia gerenciada pelo cliente. Entre em contato com o [suporte ao cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para obter mais informações.

## volume_id_invalid
**Mensagem**: o ID do volume especificado no parâmetro de solicitação não é válido.

O ID do volume especificado não está no formato correto. Verifique se você digitou corretamente o ID do volume e tente novamente. Se você não tiver certeza do ID do volume, especifique `ibmcloud is volumes` na CLI ou `GET volumes` na API e procure na lista de volumes.

## volume_id_missing
**Mensagem**: o ID do volume está ausente no parâmetro de solicitação.

Deve-se fornecer o ID do volume no parâmetro de solicitação ao obter um volume específico.

## volume_id_not_found
**Mensagem**: volume não localizado. O ID do volume `<volume_id>` não existe, em que `<volume_id>` é o ID do volume fornecido.

O ID do volume especificado não existe. Forneça um ID de volume válido.

## volume_iops_zero_or_negative
**Mensagem**: o IOPS do volume deve ser maior que zero.

Ao criar um volume, o valor de IOPS especificado na solicitação deve ser um número positivo. Consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para obter os valores de IOPS suportados.

## volume_name_duplicate
**Mensagem**: o nome do volume é uma duplicata. O nome do volume `<volume_name>` fornecido na solicitação já existe, em que `<volume_name>` é o nome do volume fornecido.

O nome do volume especificado na solicitação já existe. Forneça um nome de volume que não esteja atualmente em uso.

## volume_not_available
**Mensagem**: o Volume não está disponível. O volume pode ser modificado somente no status disponível. O status atual do `<volume_id>` de volume é `<volume_status>`, em que `<volume_id>` é o ID do volume fornecido na solicitação e `<volume_status>` é o status do volume atual.

Um volume pode ser modificado somente quando ele está em um status `available`. Certifique-se de que o volume esteja disponível antes de modificá-lo.

## volume_not_deletable
**Mensagem**: a exclusão falhou.

O volume poderá ser excluído apenas se estiver no status `available` ou `failed`. Certifique-se de que o volume esteja em um status válido para ser excluído.

## volume_not_found
**Mensagem**: um volume com o ID especificado não foi localizado.

Verifique se o ID do volume digitado está correto e tente novamente. Para obter uma lista de volumes, especifique `ibmcloud is volumes` na CLI ou `GET volumes` na API.

## volume_not_implemented
**Mensagem**: a operação solicitada não é suportada nesta liberação.

Entre em contato com o [suporte ao cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para obter mais informações.

## volume_profile_capacity_iops_invalid
**Mensagem**: o perfil de volume especificado na solicitação não é válido para a capacidade fornecida e/ou IOPS.

A capacidade do volume e/ou o valor de IOPS que você especificou na solicitação não são permitidos pelo perfil de volume. Consulte a documentação do [IBM Cloud Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about) para obter a capacidade mínima e máxima válida e os valores IOPS para um perfil customizado.

## volume_profile_iops_invalid
**Mensagem**: o perfil de volume especificado na solicitação não pode aceitar IOPS customizado.

Seu perfil de volume não aceita um valor de IOPS Customizado. É possível que você tenha especificado um perfil IOPS em Camada, o que não requer que você especifique um valor de IOPS. Se você desejar fornecer um valor de IOPS específico, use o perfil IOPS Customizado.

## volume_profile_name_missing
**Mensagem**: o nome do perfil de volume necessário está ausente na solicitação.

O nome do perfil do volume está ausente no corpo da solicitação ao criar um volume ou no parâmetro de solicitação ao obter um volume. Forneça um nome de perfil de volume em sua solicitação e tente novamente. Se você não souber o nome do perfil do volume, especifique `ibmcloud is volume-profiles` na CLI ou use `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` na solicitação da API e procure na lista.

## volume_profile_not_found
**Mensagem**: um perfil de volume com o nome especificado não foi localizado.

Um perfil de volume com esse nome não pôde ser localizado. Verifique se você forneceu o nome do perfil de volume correto. Se você não souber o nome do perfil do volume, especifique `ibmcloud is volume-profiles` na CLI ou use `GET $rias_endpoint/v1/volume/profiles/?version=YYYY-MM-DD` na solicitação da API e procure na lista.

## volume_quota_reached
**Mensagem**: a cota do volume foi atingida.

Você está fora da cota para a conta atual. Tente excluir alguns volumes ou entre em contato com o [suporte ao cliente](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) para aumentar sua cota. Consulte a documentação para obter informações sobre [cotas para a infraestrutura do Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-quotas).

## volume_resource_group_id_invalid
**Mensagem**: o ID do grupo de recursos especificado na solicitação não é válido.

O ID do grupo de recursos que você especificou na solicitação não é válido. Verifique o ID do grupo de recursos correto. Na CLI, use o comando `ibmcloud is volume VOLUME_ID`. As informações resultantes incluirão o grupo de recursos e o ID do grupo de recursos. Consulte a [CLI do IBM Cloud para referência do VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage) para obter mais informações.

## volume_resource_group_not_authorized
**Mensagem**: a ação atual não está autorizada no grupo de recursos especificado.

Você não está autorizado a listar nem a criar volumes no grupo de recursos especificado. Use um grupo de recursos diferente para o qual você tem permissão ou solicite a permissão de seu administrador para a lista e crie privilégios no grupo de recursos.

## volume_resource_group_not_found
**Mensagem**: um grupo de recursos com o ID especificado não pôde ser localizado.

O ID do grupo de recursos não pôde ser localizado porque você pode tê-lo digitado incorretamente ou ele não existe. Verifique o ID do grupo de recursos correto. Na CLI, use o comando `ibmcloud is volume VOLUME_ID`. As informações resultantes incluirão o grupo de recursos e o ID do grupo de recursos. Consulte a [CLI do IBM Cloud para referência do VPC](/docs/vpc-on-classic?topic=vpc-infrastructure-cli-plugin-vpc-reference#storage) para obter mais informações.

## volume_start_not_found
**Mensagem**: um volume com o ID especificado como o parâmetro de início da página não foi localizado.

O ID do volume especificado no parâmetro de início da chamada de volume de lista não pôde ser localizado. Verifique se o ID do volume está correto e se você tem acesso ao volume.

## volume_status_not_available
**Mensagem**: o volume com o ID especificado não está disponível.

O volume não está em um estado Available; seu status pode estar em um estado Pending. Aguarde o volume se tornar disponível e tente novamente. Se o volume estiver sendo excluído, use um volume diferente. Para verificar o status do volume, especifique `ibmcloud is volume VOLUME_ID` na CLI ou use `GET $rias_endpoint/v1/volumes/$volume_id?version=YYYY-MM-DD` na solicitação da API.

## volume_still_attached
**Mensagem**: o volume ainda está anexado a uma instância.

Não é possível excluir um volume que ainda esteja anexado a uma VSI. Se você configurar a exclusão automática para o volume, aguarde que a VSI seja excluída e o volume será separado e excluído. Se você não configurou a exclusão automática, separe o volume manualmente.

## volume_template_invalid
**Mensagem**: modelo de volume inválido fornecido.

Você forneceu um modelo de volume inválido para criar um volume. Consulte a [Referência da API do VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para verificar se você está fornecendo os parâmetros corretos no corpo da solicitação.

## volume_update_invalid_request
**Mensagem**: a solicitação de atualização de volume não é válida.

A propriedade do corpo da solicitação de atualização que você especificou não é válida. Consulte a [Referência de API do VPC](https://{DomainName}/apidocs/vpc-on-classic){: new_window} para obter informações e a sintaxe de solicitação da API correta.

## vpc_not_empty
**Mensagem**: o VPC não pode ser excluído porque ele não está vazio.

Todos os recursos no VPC devem ser excluídos antes que o VPC possa ser excluído. Um VPC contém instâncias, sub-redes, gateways públicos, balanceadores de carga e gateways de VPN e alguns desses recursos contêm recursos também. Consulte a documentação [Como excluir um VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) para obter detalhes.

Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpc_resource_separation
**Mensagem**: os recursos estão em VPCs diferentes.

Os recursos devem estar no mesmo VPC. Por exemplo, se você estiver tentando anexar um gateway público a uma sub-rede, o gateway público deverá estar no mesmo VPC que a sub-rede.

Para determinar qual VPC contém o gateway público, execute o comando `GET /public_gateways/{id}` e olhe para o valor "vpc". O comando da CLI equivalente é `ibmcloud is public-gateway <id>`.

## vpn_connection_cidr_duplicated
**Mensagem**: os blocos CIDR do `<cidr_type>` têm blocos CIDR duplicados `<cidr_blocks>`.

Os Blocos duplicados CIDR foram fornecidos ao criar a conexão. Assegure-se de que os blocos CIDR sejam exclusivos.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se o problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_UNK
**Mensagem**: um bloco CIDR não pôde ser incluído na conexão de VPN `<vpn_connection_id>`.

Forneça um CIDR válido que atenda aos requisitos fornecidos pela especificação.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_deleted
**Mensagem**: um bloco CIDR não pôde ser excluído da conexão de VPN `<vpn_connection_id>`.

Forneça um CIDR válido que exista na conexão. Uma conexão deve ter pelo menos um CIDR local e peer.

Para visualizar os blocos CIDR da conexão, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique os campos `local_cidrs` e `peer_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_localizado
**Mensagem**: o bloco CIDR não foi localizado na conexão de VPN `<vpn_connection_id>`.

Forneça um CIDR válido que esteja na conexão.

Para visualizar os blocos CIDR da conexão, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique os campos `local_cidrs` e `peer_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_not_updated
**Mensagem**: um bloco CIDR não pôde ser atualizado na conexão de VPN `<vpn_connection_id>`.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_cidr_not_valid
**Mensagem**: o bloco CIDR `<cidr_block>` não representa um endereço válido.

O valor deve ser um CIDR interno válido sem nenhum conjunto de bits do host.

Para visualizar os blocos CIDR da conexão, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique os campos `local_cidrs` e `peer_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_cidr_sobreposição
**Mensagem**: o bloco CIDR `<cidr_block_1>` se sobrepõe com o `<cidr_block_2>`. Dois blocos CIDR de peer não podem se sobrepor no mesmo VPC e dois blocos CIDR locais não podem se sobrepor na mesma conexão.

Forneça um CIDR válido que atenda aos requisitos fornecidos pela especificação.

Para visualizar os blocos CIDR da conexão, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique os campos `local_cidrs` e `peer_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_duplicate_name
**Mensagem**: o nome `<vpn_connection_name>` já está em uso pela conexão de VPN `<vpn_connection_id>`.

Forneça um nome de conexão diferente. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_invalid_name
**Mensagem**: o nome `<vpn_connection_name>` não é um nome de conexão de VPN válido.

Um nome de conexão válido inicia com uma letra, seguida por letras, dígitos, sublinhados ou hifens.

## vpn_connection_invalid_psk_format
**Mensagem**: a chave pré-compartilhada (PSK) `<psk>` não está no formato válido.

Um PSK válido deve conter somente de 6 a 128 caracteres, que são letras, dígitos, `-`, `+`, `&`, `!`, `@`, `#`, `$`, `%`,`^`,`*`,`(`,`)`,`,`,`.`,`:`,`_`.

## vpn_connection_local_cidrs_required
**Mensagem**: pelo menos um bloco CIDR local é necessário ao criar uma conexão VPN.

Forneça um CIDR local válido que atenda aos requisitos fornecidos pela especificação.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_local_subnets_quota_exceeded
**Mensagem**: as sub-redes locais nas conexões de VPN para o gateway de VPN `<vpn_gateway_id>` atingiram a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar as sub-redes locais atuais em conexões VPN, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections` e verifique o campo `local_cidrs` para cada conexão.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_local_subnets_quota_exceeded_for_connection
**Mensagem**: as sub-redes locais para a conexão de VPN atingiram a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/infrastructure/vp?topic=vpc-quotas#vpn-quotas){: new_window}.

Para visualizar as sub-redes locais atuais para uma conexão VPN, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique o campo `local_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_not_created
**Mensagem**: a conexão de VPN `<vpn_connection_id>` não pôde ser criada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_deleted
**Mensagem**: a conexão de VPN `<vpn_connection_id>` não pôde ser excluída.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_localizado
**Mensagem**: a conexão de VPN `<vpn_connection_id>` não pôde ser localizada.

Verifique se o ID de conexão está correto. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_not_updated
**Mensagem**: a conexão de VPN `<vpn_connection_id>` não pôde ser atualizada.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_pair_cidrs_duplicated
**Mensagem**: pelo menos um par de CIDR local e CIDR de mesmo nível com o mesmo gateway de peer foi duplicado.

Outra conexão que possui um CIDR local correspondente e um CIDR de peer para este existe nesse gateway. Forneça um conjunto válido de CIDRs local/peer.

## vpn_connection_peer_cidrs_required
**Mensagem**: pelo menos um bloco CIDR de peer é necessário ao criar uma conexão VPN.

Forneça um CIDR de peer válido que atenda aos requisitos fornecidos pela especificação.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connection_peer_subnets_quota_exceeded
**Mensagem**: sub-redes de peer através de conexões de VPN para o gateway de VPN `<vpn_gateway_id>` atingiram a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar as sub-redes de peer atuais em conexões VPN, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections` e verifique o campo `peer_cidrs` para cada conexão.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_connection_peer_subnets_quota_exceeded_for_connection
**Mensagem**: as sub-redes de peer para a conexão de VPN atingiram a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar as sub-redes de peer atuais para uma conexão VPN, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections/<vpn_connection_id>` e verifique o campo `peer_cidrs`.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connection VPN_GATEWAY_ID CONNECTION_ID`

## vpn_connection_static_route_not_created
**Mensagem**: falha ao incluir uma rota estática para o bloco CIDR de peer `<peer_cidr>`.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_static_route_not_deleted
**Mensagem**: falha ao remover uma rota estática para o bloco CIDR de peer `<peer_cidr>`.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_connection_update_cidrs_not_permitido
**Mensagem**: os blocos CIDR não podem ser mudados ao atualizar uma conexão. Use a API correta ao criar ou excluir blocos CIDR.

Forneça um valor de solicitação válido que atenda aos requisitos fornecidos pela especificação.

Para obter instruções adicionais para corrigir esse problema, consulte a [documentação da API](https://{DomainName}/apidocs/vpc-on-classic){: new_window}.

## vpn_connections_quota_exceeded
**Mensagem**: a conexão de VPN não pode ser criada porque o gateway de VPN `<vpn_gateway_id>` atingiu a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar as conexões VPN atuais para um gateway VPN, use a API `GET /vpn_gateways/<vpn_gateway_id>/connections `.
Comando equivalente da CLI: `ibmcloud is vpn-gateway-connections VPN_GATEWAY_ID`

## vpn_gateway_duplicate_name
**Mensagem**: o nome `<vpn_gateway_name>` já está em uso pelo gateway de VPN `<vpn_gateway_id>`.

Forneça um nome de gateway de VPN diferente. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_initialization_error
**Mensagem**: o gateway VPN não pôde ser inicializado.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_invalid_name
**Mensagem**: o nome `<vpn_gateway_name>` não é um nome de gateway de VPN válido.

Um nome de gateway VPN válido inicia com uma letra, seguida por letras, dígitos, sublinhados ou hifens.

## vpn_gateway_invalid_state
**Mensagem**: o gateway de VPN `<vpn_gateway_id>` está em um estado inválido para a operação solicitada.

O Gateway VPN deve estar no status `available` antes de poder operá-lo. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_ip_create_error
**Mensagem**: não é possível criar um endereço IP para o gateway VPN.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_missing_subnet_id
**Mensagem**: não foi possível localizar um identificador de sub-rede para o modelo de gateway de VPN fornecido.

O **ID da sub-rede** é um campo obrigatório. Forneça um ID de sub-rede com seu comando.

## vpn_gateway_missing_name
**Mensagem**: não foi possível localizar um nome para o modelo de gateway de VPN fornecido.

O **nome** é um campo obrigatório. Forneça um nome com seu comando.

## vpn_gateway_not_created
**Mensagem**: o gateway de VPN `<vpn_gateway_id>` não pôde ser criado.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_deleted
**Mensagem**: o gateway de VPN `<vpn_gateway_id>` não pôde ser excluído.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_localizado
**Mensagem**: o gateway de VPN `<vpn_gateway_id>` não pôde ser localizado.

Verifique se o ID do gateway VPN está correto. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_not_updated
**Mensagem**: o gateway de VPN `<vpn_gateway_id>` não pôde ser atualizado.

Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_not_localizado
**Mensagem**: não foi possível localizar a sub-rede `<subnet_id>` para o gateway de VPN fornecido.

Você referenciou uma sub-rede que não existe. Revise sua solicitação para assegurar que você tenha especificado o ID de sub-rede adequado. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateway_subnet_status_error
**Mensagem**: o gateway VPN não pôde ser criado devido ao status da sub-rede.

Forneça uma sub-rede que esteja no status `available`. Tente novamente em alguns minutos. Se esse problema persistir,  [ entre em contato com o suporte ](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).

## vpn_gateways_quota_exceeded
**Mensagem**: o Gateway de VPN não pode ser criado porque sua conta e/ou a região atingiu a cota.

As cotas por recurso são especificadas na página [Cotas](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#vpn-quotas){: new_window}.

Para visualizar os gateways VPN atuais, use a API `GET /vpn_gateways`.
Comando da CLI equivalente: `ibmcloud is vpn-gateways`

## zone_inconsistency
**Mensagem**: os recursos devem pertencer à mesma zona.

* Quando você anexar um volume, certifique-se de que o volume e a instância estejam na mesma zona.
* Quando você associar um IP flutuante, certifique-se de que o IP flutuante e a sub-rede estejam na mesma zona.
