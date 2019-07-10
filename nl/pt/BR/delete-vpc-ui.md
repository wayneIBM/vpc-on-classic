---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, api, rias, ui, console

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

# Excluindo um VPC usando o console do IBM Cloud
{: #deleting-using-console}

Antes de poder excluir um VPC no console do IBM Cloud, deve-se excluir todos os gateways públicos, sub-redes e recursos conectados do VPC.

Para excluir um VPC usando o console:

1. localize todas as sub-redes no VPC. Clique em **VPCs** na área de janela de navegação e selecione seu VPC. 
2. Na página Detalhes de VPC, acesse a lista **Sub-redes neste VPC** e selecione uma sub-rede para visualizar seus detalhes.
3. Exclua todos os recursos que estiverem anexados à sub-rede. Na área de janela de navegação, clique em **Recursos anexados**. Para cada instância conectada, balanceador de carga ou gateway VPN, selecione o recurso para acessar sua página de detalhes. Na página de detalhes de cada recurso, clique no ícone de exclusão. 

  Embora o status do recurso seja imediatamente mudado para **Excluindo**, pode levar até 30 minutos para que a operação de exclusão seja concluída. A sub-rede não pode ser excluída até que todos os recursos conectados não sejam mais exibidos no console.
  {: tip}

4. Depois de excluir todos os recursos anexados, volte para a página de detalhes da sub-rede e clique no ícone excluir. 
5. Depois de excluir todas as sub-redes, volte para a página de detalhes do VPC e clique no ícone excluir. O VPC e todos os gateways públicos serão excluídos.
