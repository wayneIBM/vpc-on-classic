---



copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: pricing, billing, data, instance, VSI, VPC, vCPU, RAM, workload, discount, usage, minimum, invoice, delay, limitation, operating system, suspend

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Precificação para  {{site.data.keyword.vsi_is_short}} 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (tópico da ajuda vinculado)


O {{site.data.keyword.vsi_is_full}} é oferecido em regiões selecionadas com até 62 vCPU e 248 GB de RAM para ajustar qualquer necessidade de carga de trabalho. Você é faturado somente com uma taxa horária, com descontos aplicados quanto mais tempo sua instância está em execução. Os tempos de uso do servidor virtual são calculados por segundo, para o tempo de uso e o tempo de suspensão de sua instância. Por exemplo, se sua instância for executada por 45 minutos e 32 segundos, você será cobrado pelos 45 minutos e 32 segundos.
{:shortdesc}

## Uso Suspenso
{: #sustained-usage}

Embora as instâncias sejam cobradas com uma taxa horária, quanto mais tempo sua instância estiver em execução, menos caro a taxa será. À medida que o mês de faturamento progride, você recebe os descontos por hora a seguir.

| Tempo decorrido em um mês       | Faturamento de f  | 
| ----------------------------- | ----------------- | 
| 0-20%                         | taxa de varejo regular |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="Tabela 1. Descontos em camadas" caption-side="top"}  

Essas camadas descontadas fornecem uma economia de 10% para manter as instâncias em execução mensalmente versus por hora. Esse desconto se aplica somente às taxas horárias de base; ele não se aplica a software, armazenamento, rede ou outros encargos.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### Exemplo de Uso Suspenso
{: #sustained-usage-example}

Digamos que você compre uma instância da família de servidores virtuais balanceados, com 16 CPUs e 64 GB de RAM, a um preço base de $0,795 por hora. À medida que o mês avança, sua taxa diminui como a seguir:

| Tempo decorrido em um mês       | Faturamento de f  |  Taxa de exemplo     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | taxa de varejo regular ($.795) | $116.07    |                
| 21-40%                        | 5%        |   U$ 1.10.27   |                 
| 41-60%                        | 10%       |    U$ 104,46  |            
| 61-80%                        | 15%        |    U$ 98,66    |                
| 81-100%                       | 20% |       $92,86      |
{: caption="Tabela 2. Exemplo de descontos em camadas" caption-side="top"}  

Sua conta total, se deixada em execução para o mês inteiro, com esse modelo é $522,32. Os descontos resultam em uma economia geral mensal de 10%, em comparação com a taxa horária.

## Preços de instância de base
{: #base-instance-prices}

Os preços de instância de base iniciam em $0,087 por hora. Ao criar um servidor virtual, você é solicitado a selecionar uma família de servidores virtuais e selecionar uma configuração de perfil. Quando você faz sua seleção, a taxa horária associada é exibida na tabela. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## Sistemas operacionais incluídos
{: #included-operating-systems}

Os sistemas operacionais a seguir são incluídos sem encargo:

* CentOS 7.latest
* Ubuntu 16.04 LTS, 18.04 (mínimo)
* Debian 8.latest, 9.latest (mínimo)

Há sistemas operacionais premium e outros complementos disponíveis. Você verá a precificação refletida em seu Resumo de custo.

## Suspender faturamento
{: #suspend-billing}

Quando você desliga uma instância, você não acumula custos para determinados recursos de cálculo. O faturamento parará automaticamente quando você parar a instância. O recurso de suspensão de faturamento ajuda a reduzir o custo e evita que você tenha que recriar uma instância quando precisar de seus recursos novamente.

Em situações em que você deseja escalar sua infraestrutura para cima e para baixo em resposta às necessidades de carga de trabalho, é possível usar o recurso de suspensão de faturamento como uma alternativa mais rápida para criar e excluir instâncias.
{:tip}

### Detalhes do Faturamento
{: #billing-details}

É importante entender quais custos param de acumular e quais custos persistem quando a instância de servidor virtual é desligada.

O faturamento é suspenso somente quando você desliga a instância de servidor virtual por meio do console do IBM Cloud, da CLI ou da API. Se você desligar a instância de servidor virtual diretamente por meio do S.O., o faturamento não será suspenso para essa instância.
{:note}

Revise a tabela a seguir para obter detalhes sobre como a suspensão de faturamento afeta vários encargos de recurso.

| Recurso                      | Faturamento interrompido   | O faturamento persiste |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Upgrades de Largura da B            |          X        |                  |
| Licenças do sistema operacional     |          X        |                  |
| IPs flutuantes, balanceadores de carga ou outras ofertas de rede anexadas |                   |         X        |
| Armazenamento                       |                   |         X        |
{: caption="Tabela 1. Detalhes de faturamento de recurso" caption-side="top"}   

Os tempos de uso são calculados por segundo, para o tempo de uso e o tempo de suspensão de sua instância de servidor virtual. Mesmo que você nunca inicie o recurso de suspensão de faturamento desligando sua instância, o faturamento é calculado por segundo do ciclo de vida da instância. 
{:note}

#### Suspender o faturamento e os descontos de uso
{: #suspend-billing-and-sustained-usage-discounts}

Para descontos contínuos de uso, uma imagem suspensa recomeça de onde parou na camada de desconto. Em outras palavras, um desconto de uso se aplica somente ao momento em que a imagem está em uso, não ao momento em que a imagem foi suspensa.

#### Encargo mínimo de uso
{: #minimum-usage-charge}

As instâncias de servidor virtual têm um encargo mínimo de uso por mês. Se o uso for maior que 25% no ciclo de faturamento, você será faturado para o uso real. Se o uso for menor que 25% do tempo que ele existiu no ciclo de faturamento, o encargo mínimo de 25% se aplicará. 

Por exemplo, vamos dizer que você tenha uma instância em execução por um ciclo de faturamento inteiro (720 horas). Desse tempo, a instância foi suspensa por 577 horas e esteve em execução por 143 horas. A instância é cobrada por 180 horas (o mínimo de horas disponíveis nesse período de faturamento).  

No entanto, vamos dizer que você tinha uma instância que foi criada e interrompida em um ciclo de faturamento que durou por 400 horas no total. Desse tempo, a instância foi suspensa por 120 horas e executada por 280 horas. A instância seria cobrada por seu uso de 280 horas.

#### Fatura de fatur
{: #billing-invoice}

Ao suspender o faturamento em um servidor virtual, você verá algumas mudanças em sua fatura de faturamento. Os encargos relevantes agora aparecem como detalhes baseados em uso. Por exemplo, é possível ver as seguintes adições que refletem horas disponíveis, horas usadas e o número total de horas cobradas:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

### Detalhes do recurso
{: #resource-details}

#### Armazenamento
{: #suspend-billing-storage}

Quando você suspende o faturamento em uma instância de servidor virtual, o armazenamento associado persiste, mas não é possível acessar dados enquanto a instância de servidor virtual está desligada. Ao retomar o faturamento na instância, será possível acessar seus dados e o faturamento normal começará novamente.

#### Endereços IP
{: #suspend-billing-ip-addresses}

Todas as configurações de rede e IPs (IPs privados do intervalo de sub-rede) permanecem inalterados enquanto a instância está suspensa.

É possível visualizar se seu dispositivo foi interrompido na página de detalhes da instância. Para ver quando o status mudou, clique em **Atividade** na área de janela de navegação. 

#### Limitações
{: #suspend-billing-limitations}

Os servidores virtuais suspensos continuarão a contar com relação à sua cota de dispositivo em toda a conta.
