---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Gerenciando permissões de usuário para recursos do VPC
{: #managing-user-permissions-for-vpc-resources}

O {{site.data.keyword.cloud}} Virtual Private Cloud usa o controle de acesso baseado em função que permite que os administradores de conta controlem o acesso de seus usuários a recursos do VPC.

Para obter mais informações sobre como o IBM Cloud VPC usa o controle de acesso baseado em função e como o VPC faz uso das políticas de função e acesso do IAM, veja [Designando o acesso baseado em função a recursos do VPC](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources).

Este documento mostra como o administrador da conta pode incluir usuários adicionais na conta e conceder a eles as permissões corretas para gerenciar os [recursos de infraestrutura do VPC](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources). Ele abrange dois cenários comuns para um administrador do VPC:

* **Cenário de acesso simples:** mostra como designar uma política de acesso a um usuário, para que o usuário possa criar e usar recursos de infraestrutura do VPC (incluindo Virtual Private Clouds).

* **Cenário de acesso de equipe:** mostra como configurar grupos de recursos e políticas de acesso que permitem que duas equipes separadas criem e usem os recursos do VPC designados à equipe deles.

As mudanças nas políticas de acesso do IAM para VPC podem levar até 10 minutos para entrar em vigor.
{: note}

## Cenário de acesso simples
{: #simple-access-scenario}

Este cenário abrange as etapas básicas necessárias para configurar um usuário individual. Cobriremos dois casos, convidando um novo usuário para a conta e modificando as permissões de um usuário existente.

### Convidando um novo usuário para criar ou gerenciar recursos do VPC
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

Convide um usuário do IBM Cloud para sua conta e conceda a eles acesso a `VPC Infrastructure` para que eles possam ter acesso para visualizar, criar e atualizar recursos do VPC. Esta seção fornece uma visão geral rápida das etapas do IAM. Informações adicionais estão disponíveis por meio dos links na seção **Links relacionados**, próximo ao final deste documento.

Aqui estão as etapas básicas no IAM necessárias para convidar usuários para serviços e recursos do VPC:

1. Navegue para a [IU de usuários do IAM ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam#/users){: new_window} no IBM Cloud Console.
2. Na página **Usuários**, clique em **Convidar usuários**.
3. Na página **Convidar usuários**, na seção **Usuários**, insira os endereços de e-mail dos usuários que você deseja convidar no campo **Endereço de e-mail**.
4. Na seção **Acesso**, expanda **Serviços** e, em seguida, conclua as tarefas a seguir:
  * Selecione **Recurso** na lista **Designar acesso a**.
  * Selecione **Infraestrutura do VPC** na lista **Serviços**.
  * Selecione a função de acesso à plataforma que você deseja designar aos usuários. Ela pode ser **Administrador**, **Editor**, **Operador** ou **Visualizador**.
  * Clique em  ** Convidar usuários **.

### Fornecendo uma permissão de usuário existente para gerenciar os recursos do VPC
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

Este cenário cobre as etapas básicas necessárias para conceder a um usuário existente em sua conta a permissão para editar recursos do VPC.

Nas etapas a seguir, você criará duas políticas do IAM. Ambas as políticas são necessárias antes que seu usuário possa criar e usar recursos de infraestrutura do VPC. Todos os recursos devem residir dentro do grupo de recursos padrão da conta.

1. Navegue para a [IU de usuários do IAM ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam#/users){: new_window} no IBM Cloud Console.
2. Selecione o usuário cuja autorização você está ativando.
3. Na guia **Políticas de acesso**, selecione **Designar acesso**.
4. Selecione **Designar acesso em um grupo de recursos**.
5. Selecione o grupo de recursos padrão da conta.
6. Certifique-se de que a opção **Designar acesso a um grupo de recursos** permaneça configurada como **Visualizador**.
7. Para designar o serviço correto, selecione **Infraestrutura do VPC**.
8. Certifique-se de que o valor **Tipo de recurso** esteja configurado como **Todos os tipos de recurso**.
9. Selecione a função  ** Editor ** .
10. Clique em  ** Designar **.

O usuário está agora autorizado a criar e usar recursos do VPC no grupo de recursos padrão da conta. A primeira política (etapas 1 a 6) permite que o usuário visualize o grupo de recursos padrão da conta. A segunda política (etapas 7 a 10) designou o usuário à função de **Editor** para recursos de infraestrutura do VPC, mas o acesso é limitado aos recursos no grupo de recursos padrão da conta.

{: tip}

## Visualizando as permissões de seu usuário
{: #viewing-your-user-s-permissions}

As políticas podem ser visualizadas na guia **Políticas de acesso** do usuário.

É possível usar os comandos da CLI a seguir para validar as permissões do grupo de recursos designadas a seu usuário, por política ou por grupo de acesso:

** Por política **
```
ibmcloud iam user-policies <username>
```
**Por grupo de acesso**
```
ibmcloud iam access-groups -u <username>
```

As mudanças nas políticas de acesso do IAM para VPC podem levar até 10 minutos para entrar em vigor.
{: note}

## Cenário de acesso da
{: #team-access-scenario}

Este cenário cobre como um administrador de conta pode designar autorização para que diferentes equipes tenham acesso a recursos do VPC separados. O exemplo usa _grupos de recursos_ para configurar o acesso de recurso separado para duas equipes. Para os propósitos deste exemplo, os recursos não são compartilhados entre as equipes.

O exemplo o conduz pelo processo de criação de grupos de recursos, a criação de grupos de acesso e a designação de políticas apropriadas para fornecer às suas equipes o acesso a recursos do VPC separados.

Imagine que você está configurando duas equipes do projeto diferentes para usar duas Virtual Private Clouds separadas. Você designará acesso a membros da equipe para que cada equipe tenha acesso aos recursos do VPC de sua equipe (somente).

* Sua primeira equipe é uma equipe de teste. Você decidiu designar a essa equipe o acesso aos VPCs em um grupo de recursos denominado `test_vpcs`.
* A segunda equipe é sua equipe de produção. Ela terá acesso designado aos VPCs em um grupo de recursos denominado `production_vpcs`.

Essa estratégia pode ser usada para designar recursos do VPC separados a qualquer número de equipes. No entanto, todos os recursos compartilham as mesmas cotas do VPC para a conta. Para obter mais informações sobre cotas e limites, veja [Cotas do VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas).
{: tip}

### Etapa 1: criar grupos de recursos
{: #step-1-create-resource-groups}

Por padrão, os administradores de conta têm acesso para criar novos grupos de recursos. Outros usuários devem primeiro ser designados à função de **Editor** para **Todos os serviços de gerenciamento de conta**, que permite que eles criem grupos de recursos.

Sua primeira tarefa é criar grupos de recursos que conterão cada um dos recursos do VPC de suas equipes.

1. Crie um grupo de recursos chamado  ` test_team `.  
2. Crie um grupo de recursos chamado  ` production_team `.  

Para obter mais informações sobre como criar grupos de recursos, veja [Gerenciando grupos de recursos](/docs/resources?topic=resources-rgs).

### Etapa 2: criar grupos de acesso
{: #step-2-create-access-groups}

O acesso ao recurso pode ser designado a usuários individuais ou a grupos de usuários. Os grupos de usuários com as mesmas permissões de acesso são chamados de _grupos de acesso_. Neste cenário, você criará um grupo de acesso para representar cada agrupamento de membros da equipe que requerem um tipo específico de acesso do VPC, um total de 4 grupos de acesso exclusivos.

**Tarefa:** crie quatro grupos de acesso com os nomes a seguir: `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs`, `production_team_view_vpcs`.

Para obter ajuda sobre como criar grupos de acesso, veja [Criar grupos de acesso](/docs/iam?topic=iam-groups#create_ag).

### Etapa 3: incluir políticas do IAM nos grupos de acesso que você acabou de criar
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

Inclua as políticas de acesso do VPC necessárias para que o grupo de acesso `test_team` possa gerenciar (criar, atualizar e excluir) recursos do VPC.  

1. Navegue para a [IU de grupo do IAM ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam#/groups){: new_window} no IBM Cloud Console.
2. Selecione o grupo de acesso desejado (inicie com: `test_team_manage_vpcs`).
3. Na guia **Políticas de acesso**, clique em **Designar acesso**.
4. Selecione **Designar acesso em um grupo de recursos**.
5. Selecione o grupo de recursos desejado (inicie com: `test_team`)
6. Certifique-se de que **Designar acesso a um grupo de recursos** permaneça configurado como **Visualizador**.
7. Selecione o serviço **Infraestrutura do VPC**.
8. Certifique-se de que **Tipo de recurso** permaneça configurado como **Todos os tipos de recurso**.
9. Selecione a função  ** Editor ** .
10. Selecione **Designar**.

Essas etapas também designam o acesso de **Visualizador** ao grupo de recursos `test_team`. O acesso do visualizador é necessário para atualizar, criar e excluir recursos dentro do grupo de recursos `test_team`.

{: tip}

Repita as etapas anteriores para os três grupos de acesso restantes. Essas tarefas serão realizadas para correspondência de grupos de acesso com grupos de recursos:

* Designe a função **Visualizador** do grupo de acesso `test_team_view_vpcs` para recursos de infraestrutura do VPC dentro do grupo de recursos `test_team`.
* Designe a função **Editor** do grupo de acesso `production_team_manage_vpcs` aos recursos de infraestrutura do VPC dentro do grupo de recursos `production_team`.
* Designe a função **Visualizador** do grupo de acesso `production_team_view_vpcs` aos recursos de infraestrutura do VPC dentro do grupo de recursos `production_team`.

Os usuários com acesso de **Editor** para recursos do VPC também podem visualizá-los. Não é necessário incluir membros nos grupos de acesso **Editor** e **Visualizador**.

#### Configurando o acesso ao Visualizador
{: #setting-up-viewer-access)

Os recursos de `IP flutuante` da infraestrutura do VPC são criados no grupo de recursos padrão da conta. Portanto, os usuários que precisam gerenciar `Floating IPs` precisam de acesso de **Visualizador** ao grupo de recursos padrão da conta.
{: tip}

Aqui está como criar esse acesso de **Visualizador**:

1. Navegue para a [IU de grupo do IAM ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam#/groups){: new_window} no IBM Cloud Console.
2. Selecione o grupo de acesso desejado (inicie com `test_team_manage_vpcs`).
3. Na guia **Políticas de acesso**, clique em **Designar acesso**.
4. Selecione **Designar acesso aos serviços de gerenciamento de conta**.
7. Selecione o serviço  ** Todos os serviços de gerenciamento de conta **.
9. Selecione a função  ** Visualizador ** .
10. Selecione **Designar**.

Repita as etapas anteriores para designar ao grupo de acesso `production_team_manage_vpcs` a função de **Visualizador** para **Todos os serviços de gerenciamento de conta**.

### Etapa 4: incluir usuários nos grupos de acesso
{: #step-4-add-users-to-the-access-groups}

Agora é possível designar membros da equipe (usuários) aos grupos de acesso apropriados. Siga estas etapas para incluir cada membro da equipe de teste no grupo de acesso que permite o gerenciamento de VPC da equipe de teste:

1. Navegue para a [IU de grupo do IAM ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam#/groups){: new_window} no IBM Cloud Console.
2. Selecione o grupo de acesso desejado (inicie com: `test_team_manage_vpcs`).
3. Na guia **Usuários**, clique em **Incluir usuários**.
4. Selecione cada usuário que você deseja incluir no grupo de acesso.
5. Clique em  ** Incluir no grupo **.

Repita as etapas anteriores para realizar estas tarefas:
* Designe os usuários desejados ao grupo de acesso `test_team_view_vpcs`.
* Designe os usuários desejados ao grupo de acesso `production_team_manage_vpcs`.
* Designe os usuários desejados ao grupo de acesso `production_team_view_vpcs`.

Não é necessário designar aos usuários o acesso de **Visualizador** para os recursos do VPC se eles tiverem acesso de gerenciamento.
{: tip}

## Próximas etapas
{: #permissions-next-steps}

As duas equipes de exemplo agora estão configuradas para usar VPCs. Neste momento, os membros dos grupos de acesso `test_team_manage_vpcs` e `production_team_manage_vpcs` podem criar VPCs em seus grupos de recursos designados.


## Links relacionados
{: #permissions-related-links}

* [ Gerenciando a identidade e o acesso ](/docs/iam?topic=iam-getstarted)
* [ Gerenciando usuários e o acesso ](/docs/iam?topic=iam-iamuserinv)
* [ O que é IAM ](/docs/iam?topic=iam-iamoverview)
