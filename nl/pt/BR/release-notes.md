---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}

# Notas sobre a liberação
{: #release-notes}

Use estas notas sobre a liberação para saber mais sobre as mudanças mais recentes para o {{site.data.keyword.cloud}} Virtual Private Cloud.
{:shortdesc}

## 14 de junho de 2019
{: #june-14-2019}

**Atualizações na IU do IBM Cloud VPC**

- **Chaves SSH e grupos de recursos.**
    * Os grupos de recursos são mostrados na visualização de lista de chaves SSH.
    * Os grupos de recursos são selecionáveis no modal de provisionamento de chave SSH.
- **Perfis e largura de banda.** O limite da largura da banda designado a cada perfil é mostrado em **Perfis populares** e nas páginas de exibição **Todos os perfis**.
- **Grupos de recursos.** A página de provisionamento de VSI mostra a lista suspensa do grupo de recursos.

**Atualizações no Block Storage for VPC**
- O **comprimento do nome do volume** foi aumentado para 63 caracteres alfanuméricos.
- Os **códigos de erro do volume** a seguir foram renomeados e as mensagens foram atualizadas ou excluídas:
    * `volume_encryption_key_account_id_mismatch` é renomeado para `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` é renomeado para `volume_crn_cname_mismatch`
    * `volume_encryption_key_region_not_found` foi removido

**Atualizações para o SDK**

- **O provedor terraform v0.17.1** foi liberado. Faça download do [binário mais recente](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1).
- A **Imagem do Docker** também foi atualizada com o provedor Terraform mais recente. É possível extrair a imagem mais recente do Docker usando este comando: `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Lembre-se de configurar a `generation` como um argumento do provedor ou exporte `IC_GENERATION= 1` para que seu código funcione com a versão atualmente liberada do VPC na infraestrutura clássica. Por padrão, o valor é configurado como 2 (VPC, chegando em breve, agora em Beta).
- Os argumentos do provedor (`bluemix_api_key` e `bluemix_timeout`) foram descontinuados, portanto, um aviso poderá ser lançado quando você executar o plano Terraform ou aplicar.

## 7 de junho de 2019
{: #june-07-2019}

- **O IBM Cloud VPC agora está em disponibilidade geral.** Todas as contas [Pré-pagas](/docs/account?topic=account-accounts) podem provisionar recursos do VPC. Efetue login no [IBM Cloud Console](https://{DomainName}/vpc/overview) e inicie hoje!

- **Políticas de autorização individuais em instâncias, chaves, volumes e grupos de segurança podem agora ser configurados.** Aprenda quais funções são necessárias para gerenciar esses recursos em [Autorizações de recursos necessárias para chamadas de API e CLI](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Os usuários de acesso antecipado não são afetados com as políticas existentes.

- **O parâmetro de velocidade da porta para uma interface de rede virtual foi removido da Interface com o usuário, da CLI e da API.**

- **O suporte multiregion para monitoramento de métricas agora está disponível na Interface com o usuário.**


## 31 de maio de 2019
{: #may-31-2019}

**Nova versão de API disponível.** A versão da API do VPC on Classic foi atualizada a partir de `2019-01-01` para `2019-05-31`. Para obter diretrizes e boas práticas, consulte [Versão](https://{DomainName}/apidocs/vpc-on-classic#versioning) na API do Virtual Private Cloud on Classic.

## 24 de maio de 2019
{: #may-24-2019}

- **As instâncias do Balanceador de Carga ao excluir status agora são mostradas na visualização de lista na Interface com o usuário.** Quando você solicita a exclusão de um balanceador de carga na IU, a visualização de lista continua a mostrar o balanceador de carga até que o processo de exclusão seja concluído.

- **Os recursos do Balanceador de Carga da Camada 7 estão disponíveis na Interface com o usuário.** Agora é possível configurar os recursos do balanceador de carga da Camada 7 na Interface com o usuário.

- **A visualização do Balanceador de Carga e as políticas de edição estão disponíveis agora na Interface com o usuário.** Uma nova função, visualizar e editar as políticas do balanceador de carga, está disponível na interface com o usuário.

Para obter mais informações, consulte a [Documentação do Balanceador de Carga](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10 de maio de 2019
{: #may-10-2019}


- **Os nomes de perfil foram mudados**. Os nomes dos perfis da instância foram mudados. Os comandos para as instâncias de provisão precisam ser atualizados para usar os novos nomes de perfil. Leia mais [sobre perfis aqui](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **A precificação mensal estimada do IP flutuante está disponível na Interface com o usuário**. À medida que você reserva um endereço IP flutuante, agora você pode ver uma linha mostrando o preço mensal estimado para esse IP flutuante.

- **O suporte ao Hyper Protect está disponível**.

- **O nome e a zona de sub-rede aparecem como links que o levarão à sub-rede associada na Interface com o usuário**. As sub-redes agora estão listadas por nome em vez de ID de sub-rede, e o nome atua como um link para a página de detalhes de sub-rede da sub-rede associada.

- **O estimador de resumo de custo está disponível na Interface com o usuário**.

- **A pesquisa de conjuntos e listeners do Balanceador de Carga é refletida na Interface com o usuário**.

    * Na página de conjuntos do balanceador de carga, você agora vê um contador que mostra a última atualização, abaixo da tabela de recursos.
    * O intervalo de pesquisa padrão é de 30 segundos.
    * Quatro (4) estados diferentes podem ser exibidos: `newProvision`, `active`, `failed` e `all other states`.
        * Para uma nova provisão: o intervalo é de 15 segundos. Os conjuntos e listeners vão para o estado ativo imediatamente.
        * Para ativo: a lista de recursos e o cabeçalho são atualizados a cada 60 segundos.
        * Para falha: a pesquisa está interrompida.
        * Para todos os outros estados: a pesquisa continua em intervalos de 30 segundos.
    * Se você excluir um listener, será possível acionar o status do cabeçalho para mostrar `Updating (Actions Unavailable)`.
    * Além disso, observe que as informações do cabeçalho são atualizadas quando ocorre uma atualização.
    * Essas mesmas mudanças se aplicam à pesquisa de listeners.

- **A contagem de membros do Balanceador de Carga é mostrada na página Membros e detalhes na Interface com o usuário, enquanto a pesquisa de membros está ocorrendo**.

    * Na seção de status de funcionamento, você vê a contagem de membros enquanto os "Detalhes da instância de busca" estão próximos a ele.
    * Na coluna Instâncias na tabela de recursos, você vê a contagem de membros enquanto os "Detalhes da instância de busca" estão próximos a ele.
    * Depois de carregado, se você tiver membros inválidos, você verá o ícone de cuidado amarelo.

- **A página de detalhes do VPC na Interface com o usuário mostra todas as sub-redes conectadas**.
