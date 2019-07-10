---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: create, configure, permissions, ACL, virtual, server, instance, subnet, block, storage, volume, security, group, images, Windows, Linux, example, monitoring, VPN, load balancer, IKE, IPsec

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

# Criando um VPC usando o console do  {{site.data.keyword.cloud_notm}}
{: #creating-a-vpc-using-the-ibm-cloud-console}
[comment]: # (tópico da ajuda vinculado)

Este documento mostra como criar e configurar um {{site.data.keyword.cloud}} Virtual Private Cloud usando o console do {{site.data.keyword.cloud_notm}}.

Para criar e configurar seu Virtual Private Cloud (VPC) e outros recursos anexados, execute as etapas nas seções a seguir, nesta ordem:

1. Crie um VPC e uma sub-rede para definir a rede. Ao criar sua sub-rede, anexe um gateway público para permitir que todos os recursos na sub-rede se comuniquem com a Internet pública.
1. Configure uma lista de controle de acesso (ACL) para limitar o tráfego de entrada e de saída da sub-rede. Por padrão, todo o tráfego é permitido.
1. Crie uma instância de servidor virtual.
1. Crie um volume de armazenamento de bloco e anexe-o a uma instância.
1. Configure um grupo de segurança para definir o tráfego de entrada e de saída que é permitido para a instância.
1. Reserve e associe um endereço IP flutuante para permitir que sua instância seja atingível por meio da Internet.
1. Crie um balanceador de carga para distribuir solicitações em múltiplas instâncias.
1. Crie uma rede privada virtual (VPN) para que seu VPC possa se conectar de forma segura à outra rede privada, como a sua rede no local ou outro VPC.

## Antes de Começar
{: #before}
Certifique-se de que você tenha permissões suficientes para criar e gerenciar recursos em seu VPC. Para obter mais informações, veja [Gerenciando permissões de usuário para recursos do VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Gere uma chave SSH, que será usada para se conectar à instância de servidor virtual. Por exemplo, gere uma chave SSH em seu servidor Linux, executando o comando `ssh-keygen -t rsa -C "user_ID"`. Esse comando gera dois arquivos. A chave pública gerada está no arquivo `<your key>.pub`.

Se você planeja criar um balanceador de carga e usar HTTPs para o listener, um certificado SSL é necessário. É possível gerenciar certificados com o [IBM Certificate Manager ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/catalog/services/certificate-manager){: new_window}. Deve-se também criar uma autorização para permitir que a instância do balanceador de carga acesse a instância do Certificate Manager que contém o certificado SSL. É possível criar uma autorização por meio de [Autorizações de identidade e acesso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/iam/#/authorizations){: new_window}. Para a origem, selecione **Infraestrutura do VPC** como o serviço de Origem, **Load Balancer for VPC** como o Tipo de recurso e **Todas as instâncias de recurso** para a instância de recurso de Origem. Selecione **Certificate Manager** como o serviço de Destino e designe **Gravador** para a função de acesso ao serviço. Configure a instância de serviço de Destino como **Todas as instâncias** ou como sua instância específica do Certificate Manager. Para obter mais informações, veja [Usando Load Balancers no IBM Cloud VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).

## Criando um VPC e uma Sub-rede

Para criar um VPC e uma sub-rede:

1. Abra o [console do {{site.data.keyword.cloud_notm}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}
1. Clique em **Ícone de menu ![Ícone de menu](../../icons/icon_hamburger.svg) > Infraestrutura do VPC > Rede > VPCs** e clique em **Nova nuvem particular virtual**.
1. Insira um nome para o VPC, como `my-vpc`.
1. Selecione um grupo de recursos para o VPC e todos os seus recursos anexados. Os grupos de recursos permitem organizar os recursos da conta para propósitos de controle de acesso e faturamento. Para obter mais informações, consulte [Melhores práticas para organizar recursos em um grupo de recursos](/docs/resources?topic=resources-bp_resourcegroups).
1. _Opcional:_ insira as tags para ajudar a organizar e localizar seus recursos. É possível incluir mais tags posteriormente. Para obter mais informações, consulte [Trabalhando com tags](/docs/resources?topic=resources-tag).
1. Selecione ou crie a ACL padrão para novas sub-redes nesse VPC. Neste tutorial, vamos criar uma nova ACL padrão. Nós vamos configurar regras para a ACL posteriormente.
1. Selecione se o grupo de segurança padrão permite o tráfego de entrada de SSH e de ping para instâncias de servidor virtual nesse VPC. Nós vamos configurar mais regras para o grupo de segurança padrão posteriormente.
1. Insira um nome para a nova sub-rede em seu VPC, como `my-subnet`.
1. Selecione um local para a sub-rede. O local consiste em uma região e uma zona.

    A região selecionada é usada como a região do VPC. Todos os recursos adicionais que você criar nesse VPC serão criados na região selecionada.
    {: tip}

1. Insira um intervalo de IP para a sub-rede na notação CIDR, por exemplo: `10.240.0.0/24`. Na maioria dos casos, é possível usar o intervalo de IP padrão. Se você desejar especificar um intervalo de IP customizado, será possível usar a calculadora de intervalo de IP para selecionar um prefixo de endereço diferente ou mudar o número de endereços.
1. Selecione uma ACL para a sub-rede. Selecione **Usar padrão do VPC** para usar a ACL padrão criada para esse VPC.
1. Anexe um gateway público à sub-rede para permitir que todos os recursos anexados se comuniquem com a Internet pública.  

    Também é possível anexar o gateway público depois de criar a sub-rede.
    {: tip}

1. Clique em **Criar nuvem particular virtual**.
1. Para criar outra sub-rede nesse VPC, clique na guia **Sub-redes** e clique em **Nova sub-rede**. Quando definir a sub-rede, certifique-se de selecionar `my_vpc` no campo **Nuvem particular virtual**.

## Configurando a ACL

É possível configurar a ACL para limitar o tráfego de entrada e de saída para a sub-rede. Por padrão, todo o tráfego é permitido.

Cada sub-rede pode ser anexada a somente uma ACL. No entanto, cada ACL pode ser anexada a múltiplas sub-redes.

Para configurar a ACL:

1. Na área de janela de navegação, clique em **Rede > Sub-redes**.
1. Clique na sub-rede que você criou.
1. Na área **Detalhes da sub-rede**, clique no nome da ACL.
1. Clique em **Incluir regra** para configurar regras de entrada e saída que definem qual tráfego é permitido dentro ou fora da sub-rede. Para cada regra, especifique as informações a seguir:  
   * Especifique a prioridade da regra. As regras com números inferiores são avaliadas primeiro e substituem as regras com números mais altos. Por exemplo, se uma regra com prioridade 2 permitir tráfego HTTP e uma regra com prioridade 5 negar todo o tráfego, o tráfego HTTP ainda será permitido.  
   * Selecione se deseja permitir ou negar o tráfego especificado.  
   * Especifique um bloco CIDR para indicar o intervalo de IP ao qual a regra se aplica.
   * Selecione os protocolos e portas aos quais a regra se aplica.
1. Ao concluir a criação de regras, clique na trilha de navegação **Todas as listas de controle de acesso** na parte superior da página.

### Exemplo de ACL

Por exemplo, é possível configurar regras de entrada que fazem o seguinte:

 1. Permitir tráfego HTTP da Internet
 1. Permitir todo o tráfego de entrada da sub-rede 10.10.20.0/24
 1. Negar todos os outros tráfego de entrada  

Em seguida, configure regras de saída que fazem o seguinte:

1. Permitir tráfego HTTP para a Internet
1. Permitir todo o tráfego de saída para a sub-rede 10.10.20.0/24
1. Negar todos os outros tráfegos de saída.  

![Mostra as regras de entrada e de saída de amostra](images/acl-rules.png)

## Criando uma instância de servidor virtual

Para criar uma instância de servidor virtual na sub-rede recém-criada:

1. Clique em **Calcular > Instância de servidor virtual** na área de janela de navegação e clique em **Nova instância**.
1. Insira um nome para a instância, tal como `my-instance`.
1. Selecione o VPC que você criou.
1. No campo **Local**, selecione a zona na qual criar a instância.
1. Selecione uma imagem (isto é, sistema operacional e versão), como Ubuntu Linux 16.04.
1. Para configurar o tamanho da instância, selecione um dos perfis populares ou clique em **Todos os perfis** para escolher uma combinação de núcleo e RAM diferente que seja mais apropriada para sua carga de trabalho.
1. Selecione uma chave SSH existente ou inclua uma nova chave SSH que será usada para acessar a instância de servidor virtual. Para incluir uma chave SSH, clique em **Nova chave** e nomeie a chave. Depois de inserir seu valor de chave pública gerada anteriormente, clique em **Incluir chave SSH**.

As chaves só podem ser incluídas inicialmente como parte da criação da VSI. Não existe nenhuma ferramenta para incluir chaves posteriormente.
{:tip}

1. _Opcional:_ insira os dados do usuário para executar tarefas de configuração comuns quando sua instância for iniciada. Por exemplo, é possível especificar diretivas cloud-init ou shell scripts para imagens do Linux. Para obter mais informações, consulte  [ Dados do usuário ](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-user-data).
1. Observe o volume de inicialização. Na liberação atual, 100 GB são atribuídos para o volume de inicialização. A *Exclusão automática* é ativada para o volume; ela será excluída automaticamente se a instância for excluída.
1. Na área **Volume de armazenamento de bloco conectado**, é possível clicar em **Novo volume de armazenamento de bloco** para anexar um volume de armazenamento de bloco à sua instância. Neste tutorial, criaremos um volume de armazenamento de bloco e o anexaremos à instância posteriormente.
1. Na área **Interfaces de rede**, é possível editar a interface de rede e mudar seu nome. Se você tiver mais de uma sub-rede na zona selecionada e VPC, será possível anexar uma sub-rede diferente à interface. Se você desejar que a instância exista em múltiplas sub-redes, será possível criar mais interfaces.

   Também é possível selecionar quais grupos de segurança anexar a cada interface. Por padrão, o grupo de segurança padrão do VPC está anexado. O grupo de segurança padrão permite o tráfego de entrada de SSH e de ping, todo o tráfego de saída e todo o tráfego entre as instâncias no grupo. Todo outro tráfego é bloqueado; é possível configurar regras para permitir mais tráfego. Se você editar posteriormente as regras do grupo de segurança padrão, essas regras atualizadas se aplicarão a todas as instâncias atuais e futuras no grupo.

1. Clique em  ** Criar instância de servidor virtual **. O status da instância inicia como *Pendente*, muda para *Interrompida* e, em seguida, para *Ligada*. Talvez seja necessário atualizar a página para ver a mudança no status.

## Criando e conectando um volume de armazenamento de bloco

É possível criar um volume de armazenamento de bloco e anexá-lo à sua instância do servidor virtual.

Para criar e anexar um volume de armazenamento de bloco:

1. Na área de janela de navegação, clique em **Armazenamento > Volumes de armazenamento de bloco**.
1. Na página Volumes de armazenamento de bloco para VPC, clique em **Novo volume** e especifique as informações a seguir.
  * **Nome**: insira um nome para o volume de armazenamento de bloco, tal como `data-volume-1`.  
  * **Grupo de recursos**: selecione um grupo de recursos para o volume de armazenamento de bloco. Os grupos de recursos permitem organizar os recursos da conta para propósitos de controle de acesso e faturamento. Para obter mais informações, consulte [Melhores práticas para organizar recursos em um grupo de recursos](/docs/resources?topic=resources-bp_resourcegroups).
  * **Tags**: _Opcional:_ insira tags para ajudar a organizar e localizar seus recursos. É possível incluir mais tags posteriormente. Para obter mais informações, consulte [Trabalhando com tags](/docs/resources?topic=resources-tag).
  * **Local**: selecione um local para o volume de armazenamento de bloco. O local consiste em uma região e uma zona, por exemplo, US South 1.
  * **Tamanho**: especifique o tamanho do volume entre 10 GBs e 2.000 GBs.
  * **IOPs**: selecione uma das Camadas IOPs ou clique em Customizado para inserir um valor de IOPs com base no tamanho do volume.
  * **Criptografia**: aceite a criptografia gerenciada por Provedor padrão ou selecione Cliente gerenciado e use sua própria chave de criptografia. Esta etapa requer o provisionamento de uma instância de serviço Key Protect e a criação ou a importação de uma chave raiz. Para obter mais informações, consulte [Criando volumes de armazenamento de bloco com criptografia gerenciada pelo cliente](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption#data-vol-encryption-ui).
1. Clique em **Criar volume**.
1. Na lista de volumes de armazenamento de bloco, localize o volume que você criou. Quando o status for Disponível, clique em "..." e selecione **Anexar à instância**.
1. Selecione a instância para a qual deseja anexar o volume e clique em **Anexar**.

## Configurando o grupo de segurança para a instância

É possível configurar o grupo de segurança para definir o tráfego de entrada e de saída que é permitido para a instância. Por exemplo, depois de configurar regras de ACL para a sub-rede com base nas políticas de segurança de sua empresa, é possível restringir ainda mais o tráfego para instâncias específicas, dependendo de suas cargas de trabalho.

Para configurar o grupo de segurança:

1. Na página Instâncias do servidor virtual, clique em sua instância para visualizar seus detalhes.
1. Na seção **Interfaces de rede**, clique no grupo de segurança.
1. Clique em **Incluir regra** para configurar regras de entrada e de saída que definem qual tipo de tráfego é permitido para e da instância. Para cada regra, especifique as informações a seguir:  
   * Especifique um bloco CIDR ou um endereço IP para o tráfego permitido. Como alternativa, é possível especificar um grupo de segurança no mesmo VPC para permitir o tráfego para ou de todas as instâncias anexadas ao grupo de segurança selecionado.   
   * Selecione os protocolos e portas aos quais a regra se aplica.    

   ** Dicas: **  
  * Todas as regras são avaliadas, independentemente da ordem em que elas são incluídas.
  * As regras são stateful, o que significa que o tráfego de retorno em resposta ao tráfego permitido é autorizado automaticamente. Por exemplo, uma regra que permite o tráfego TCP de entrada na porta 80 também permite a reaplicação do tráfego TCP de saída na porta 80 de volta para o host de origem, sem a necessidade de uma regra adicional.
1. _Opcional:_ se você desejar anexar esse grupo de segurança a outras instâncias, clique em **Interfaces anexadas** na área de janela de navegação e selecione interfaces adicionais.
1. Quando você concluir a criação de regras, clique na trilha de navegação **Todos os grupos de segurança** na parte superior da página.

### Exemplo de grupo de segurança  

Por exemplo, é possível configurar regras de entrada que fazem o seguinte:

 * Permitir todo o tráfego SSH (porta TCP 22)
 * Permitir todo o tráfego de ping (ICMP tipo 8)

Em seguida, configure regras de saída que permitam todo o tráfego TCP:

![Mostra as regras de entrada e de saída de amostra](images/sg-example-ui.png)

## Reservando um endereço IP flutuante

Reserve e associe um endereço IP flutuante para permitir que sua instância seja atingível por meio da Internet.  

Sua instância deve estar em execução antes que seja possível associar um endereço IP flutuante. Pode levar alguns minutos para que a instância esteja funcionando.
{: tip}

Para reservar e associar um endereço IP flutuante:

1. Na página Instâncias do servidor virtual, clique em sua instância para visualizar seus detalhes.
1. Na seção **Interfaces de rede**, clique em **Reservar** para a interface que você deseja associar a um endereço IP flutuante.

Se você desejar posteriormente redesignar esse endereço IP flutuante a outra instância na mesma zona, localize o endereço IP flutuante na página **Rede > IPs flutuantes**, clique em seu menu overflow (**...**) e clique em **Desassociar**. Em seguida, clique em **Associar** para selecionar a instância e a interface de rede que você deseja associar ao endereço IP flutuante.
{: tip}

## Conectando-se à sua instância
Usando o endereço IP flutuante que você criou, execute ping de sua instância para certificar-se de que ela esteja funcionando:

```
ping <public-ip-address>
```
{:pre}


### Conectando-se às imagens do Linux

Como você criou sua instância com uma chave SSH pública, agora é possível se conectar a ela diretamente usando sua chave privada:


```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

Veja [Conectando-se à sua instância usando o Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) para obter mais informações sobre como se conectar à sua instância.

### Conectando-se às imagens do Windows
Para se conectar a uma imagem do Windows, efetue login usando sua senha decriptografada. Para obter a senha decriptografada, copie a senha criptografada da página de detalhes da instância e execute o comando a seguir:

```
# Decodificar a senha criptografada
cat ~/examplepwd | base64 --decode > ~/examplepwd64
# Decriptografar a senha criptografada usando a chave privada RSA
openssl pkeyutl -in ~/examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
-pkeyopt rsa_mgf1_md:sha256
```
{:codeblock}


Veja [Conectando-se à sua instância do Windows](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-windows-instance) para obter mais informações sobre como se conectar à sua instância.



## Monitorando sua instância

Para obter um log de atividades que mostra quando a instância foi iniciada, interrompida ou reinicializada, clique em **Atividade** na área de janela de navegação.

## Criando um balanceador de carga
É possível criar um balanceador de carga para distribuir o tráfego de entrada entre múltiplas instâncias.

Para criar um balanceador de carga:
1. Na área de janela de navegação, clique em **Rede > Balanceadores de carga**.
1. Na página Balanceadores de carga, clique em **Novo balanceador de carga** e especifique as informações a seguir.
    * **Nome**: insira um nome para o balanceador de carga, como `my-load-balancer`.
    * ** Nuvem privada virtual **: selecione seu VPC.
    * **Grupo de recursos**: selecione um grupo de recursos para o balanceador de carga.
    * **Tipo**: selecione o tipo de balanceador de carga, Público ou Privado.
    * **Região**: indica a região na qual o balanceador de carga será criado; isto é, a região selecionada para seu VPC.
    * **Sub-redes**: selecione as sub-redes nas quais criar seu balanceador de carga. Para maximizar a disponibilidade de seu aplicativo, selecione sub-redes em zonas diferentes.
1. Clique em **Novo conjunto** e especifique as informações a seguir para criar um conjunto de back-end. É possível criar um ou mais conjuntos.
    * **Nome**: insira um nome para o conjunto, tal como `my-pool`.
    * **Protocolo**: selecione o protocolo para suas instâncias de back-end por trás do balanceador de carga. O protocolo do conjunto deve corresponder ao protocolo de seu listener associado. Por exemplo, se um protocolo HTTPS ou HTTP for selecionado para o listener, o protocolo do conjunto deverá ser HTTP. Da mesma forma, se o protocolo listener for TCP, o protocolo do conjunto de back-end deverá ser TCP.
    * **Método**: selecione como você deseja que o balanceador de carga distribua o tráfego entre as instâncias de back-end:
      * **Round-robin:** encaminhe solicitações para cada instância sucessivamente. Todas as instâncias recebem aproximadamente um número igual de conexões do cliente.
      * **Round-robin ponderado:** encaminhar solicitações para cada instância em proporção a seu peso designado. Por exemplo, se você tiver as instâncias A, B e C e seus pesos forem configurados como 60, 60 e 30, as instâncias A e B receberão um número igual de conexões e a instância C receberá metade do número de conexões.
      * **Conexões mínimas:** encaminhe solicitações para a instância com o número mínimo de conexões no momento atual.  
    * **Permanência de sessão**: selecione se todas as solicitações durante a sessão de um usuário são enviadas para a mesma instância.  
    * **Verificações de funcionamento**: configure como o balanceador de carga verifica o funcionamento das instâncias.
      * **Protocolo de funcionamento**: o protocolo usado pelo balanceador de carga para enviar mensagens de verificação de funcionamento para as instâncias de back-end.
      * **Caminho de funcionamento**: o caminho de funcionamento será aplicável somente se HTTP for selecionado como o protocolo de verificação de funcionamento. O caminho de funcionamento especifica a URL usada pelo balanceador de carga para enviar as solicitações de verificação de funcionamento de HTTP para as instâncias de back-end. Por padrão, as verificações de funcionamento são enviadas para o caminho raiz (`/`).
      * **Intervalo**: intervalo em segundos entre duas tentativas de verificação de funcionamento consecutivas. Por padrão, as verificações de funcionamento são enviadas a cada cinco segundos.
      * **Tempo limite**: quantia máxima de tempo que o sistema aguarda por uma resposta de uma solicitação de verificação de funcionamento. Por padrão, o balanceador de carga aguarda dois segundos para uma resposta.
      * **Máximo de novas tentativas**: o número máximo de tentativas de verificação de funcionamento adicionais que o balanceador de carga faz antes de declarar uma instância de back-end não funcional. Por padrão, uma instância não é mais considerada funcional após duas verificações de funcionamento com falha.

        Embora o balanceador de carga pare de enviar conexões para instâncias não funcionais, o balanceador de carga continuará monitorando o funcionamento dessas instâncias e continuará seu uso se elas forem localizadas como funcionais novamente, passando com êxito duas tentativas de verificação de funcionamento consecutivas.

      Se as instâncias de back-end não estiverem funcionais e você achar que seu aplicativo está sendo executado sem problemas, verifique os valores de protocolo de funcionamento e de caminho de funcionamento. Verifique também quaisquer grupos de segurança anexados às instâncias para assegurar que as regras permitam o tráfego entre o balanceador de carga e as instâncias.
      {: tip}

1. Clique em  ** Criar **.
1. Próximo à entrada para o novo conjunto, clique em **Anexar** na coluna **Instâncias** para incluir uma instância no conjunto. Clique em **Incluir** para incluir mais instâncias no conjunto. Especifique as informações a seguir para cada instância:
   * Selecione uma ou mais sub-redes nas quais selecionará uma instância.
   * Selecione uma instância. Se uma instância tiver múltiplas interfaces, certifique-se de selecionar o endereço IP correto.
   * Especifique a porta na qual o tráfego é enviado para a instância.
   * Se o seu conjunto usar o método **Round-robin ponderado**, designe um peso para cada instância.  

      A designação do peso '0' a uma instância significa que nenhuma nova conexão será encaminhada para essa instância, mas qualquer tráfego existente continuará a fluir enquanto a conexão atual estiver ativa. Usar um peso de '0' pode ajudar a derrubar uma instância graciosamente e removê-la da rotação de serviço.
      {: tip}

1. Clique em **Novo listener** e especifique as informações a seguir para criar um listener. É possível criar um ou mais listeners.
   * **Protocolo**: o protocolo a ser usado para o recebimento de solicitações recebidas.
   * **Porta**: a porta de recebimento na qual as solicitações são recebidas. O intervalo de portas de 56500 a 56520 é reservado para propósitos de gerenciamento e não pode ser usado.
   * **Conjunto de back-end**: o conjunto de back-end para o qual esse listener encaminha o tráfego.
   * **Máximo de conexões** (opcional): número máximo de conexões simultâneas que o listener permite.
   * **Certificado SSL **: se HTTPS é o protocolo selecionado para esse listener, deve-se selecionar um certificado SSL. Certifique-se de que o balanceador de carga esteja autorizado a acessar o certificado SSL. Para obter instruções, consulte  [ Antes de iniciar ](#before).
1. Clique em  ** Criar **.
1. Depois de concluir a criação de conjuntos e listeners, clique em **Criar balanceador de carga**.
1. Para visualizar os detalhes de um balanceador de carga existente, clique no nome de seu balanceador de carga na página **Balanceadores de carga**.

## Criando uma VPN
É possível criar uma rede privada virtual (VPN) para que seu VPC possa se conectar de forma segura à outra rede privada, como uma rede no local ou outro VPC.

Para visualizar um exemplo de código, veja [Usando a VPN com seu VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
{: tip}

Para criar uma VPN:
1. Na área de janela de navegação, clique em **Rede > VPNs**.
1. Na página VPN, clique em **Novo gateway VPN** e especifique as informações a seguir:
    * **Nome**: insira um nome para o gateway VPN em sua nuvem particular virtual, como `my-vpn-gateway`.
    * ** Nuvem privada virtual **: selecione seu VPC.
    * **Grupo de recursos**: selecione um grupo de recursos para a VPN.
    * **Sub-rede**: selecione a sub-rede na qual criar o gateway VPN.
1. Na seção **Nova conexão VPN**, defina uma conexão entre esse gateway e uma rede fora de seu VPC especificando as informações a seguir.
    * **Nome de conexão**: insira um nome para a conexão, como `my-connection`.
    * **Endereço do gateway de peer**: especifique o endereço IP do gateway VPN para a rede fora de seu VPC.
    * **Chave pré-compartilhada**: especifique a chave de autenticação do gateway VPN para a rede fora de seu VPC.
    * **Sub-redes locais**: especifique uma ou mais sub-redes no VPC que você deseja conectar por meio do túnel VPN.
    * **Sub-redes de peer**: especifique uma ou mais sub-redes na outra rede que você deseja conectar por meio do túnel VPN.
1. Para configurar como o gateway de nuvem envia mensagens para verificar se o gateway de peer está ativo, especifique as informações a seguir na seção **Detecção de peer inativo**.
    * **Ação de detecção de peer inativo**: a ação a ser tomada quando um gateway de peer para de responder. Por exemplo, selecione **Reiniciar** se você desejar que o gateway renegocie imediatamente a conexão.
    * **Intervalo**: com que frequência verificar se o gateway de peer está ativo. Por padrão, as mensagens são enviadas a cada 30 segundos.
    * **Tempo limite**: quanto tempo esperar por uma resposta do gateway de peer. Por padrão, um gateway de peer não será mais considerado ativo se uma resposta não for recebida dentro de 150 segundos.
1. Especifique os parâmetros de segurança IKE e IPsec para a conexão.
    * Selecione **Automático** se você desejar que o gateway de nuvem tente estabelecer automaticamente a conexão.
    * Selecione ou crie políticas customizadas se você precisar aplicar requisitos de segurança específicos ou o gateway VPN para a outra rede não suportar as propostas de segurança que são tentadas pela negociação automática.

  **Importante**: os parâmetros de segurança IKE e IPsec que você especifica para a conexão devem ser os mesmos parâmetros que são configurados no gateway para a rede fora de seu VPC.

## Parabéns.

Você criou e configurou com êxito um VPC e uma sub-rede, uma ACL, uma instância de servidor virtual, um volume de armazenamento de bloco, um grupo de segurança, um endereço IP flutuante, um balanceador de carga e uma VPN. É possível continuar a desenvolver seu VPC incluindo mais instâncias, sub-redes e outros recursos.
