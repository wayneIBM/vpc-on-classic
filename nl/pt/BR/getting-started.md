---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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
{:DomainName: data-hd-keyref="DomainName"}

# Tutorial Introdução
{: #getting-started}
[comment]: # (tópico da ajuda vinculado)


Para começar no {{site.data.keyword.cloud}} Virtual Private Cloud:

1. Crie uma Nuvem Privada Virtual.
2. Crie uma ou mais sub-redes na nuvem particular virtual em uma ou mais zonas.
3. Crie um gateway público (PGW) em uma sub-rede se você desejar que os recursos em sua sub-rede tenham acesso à Internet ou vice-versa.
4. Selecione os perfis de instâncias de servidor virtual (VSIs) que você gostaria de executar e instancie-os.
5. Reserve um endereço IP flutuante e associe-o a uma instância de servidor virtual se desejar atingi-la por meio da Internet.
5. Implemente seu serviço ou aplicativos entre as instâncias de servidor virtual.

## Pré-requisitos

 * **Permissões do usuário**: certifique-se de que seu usuário tenha permissões suficientes para criar e gerenciar recursos no VPC. Para obter uma lista de permissões necessárias, consulte [Concedendo permissões necessárias para usuários da VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

 * **Tenha sua chave SSH pronta**: você usará uma chave SSH para se conectar às suas instâncias do servidor virtual.

   * Procure por um arquivo chamado `id_rsa.pub` sob um diretório `.ssh` sob seu diretório inicial, por exemplo, `/Users/<USERNAME>/.ssh/id_rsa.pub`. O arquivo começa com `ssh-rsa` e termina com seu endereço de e-mail.

   * Ou gere uma chave SSH: se você não tiver uma chave SSH pública ou se tiver esquecido a senha de uma existente, gere uma nova executando o comando `ssh-keygen` e seguindo os prompts. Por exemplo, é possível gerar uma chave SSH em seu servidor Linux executando o comando `ssh-keygen -t rsa -C "user_ID"`. Esse comando gera dois arquivos. A chave pública gerada está no arquivo `<your key>.pub`.

## Usar a IU, a CLI ou a API de REST

É possível provisionar e gerenciar todos os seus recursos do VPC por meio da IU, CLI ou API de REST.

Se você for novo no IBM Cloud Virtual Private Cloud, escolha qualquer um dos links abaixo, que o guiará por meio do processo de criação do IBM Cloud VPC e seus recursos, desde o início até a conclusão. É possível escolher se você deve iniciar pela IU do Console, da linha de comandos (CLI) ou das APIs de REST.

* Para acesso por meio da interface com o usuário, efetue login no [IBM Cloud Console ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")]( https://{DomainName}/vpc){: new_window}. Para obter mais informações, consulte [Criando um VPC usando o console do IBM Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console).
* Para usar a interface da linha de comandos, siga o exemplo [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli).
* Para usuários mais avançados, é possível chamar as [APIs de REST](https://{DomainName}/apidocs/vpc-on-classic) diretamente. Siga o tutorial de [código de exemplo](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) para começar com as APIs de REST.

## Próximas etapas
Se você estiver pronto para se aprofundar, acesse diretamente a documentação detalhada sobre **Redes para o VPC**, **VSIs para o VPC** e **Block Storage for VPC**:

* [**Rede para Virtual Private Cloud**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Virtual Servers for Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage for Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
