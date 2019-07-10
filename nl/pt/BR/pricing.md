---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-29"

keywords: pricing, billing, data, instance, VSI, block, storage, paygo, transfer, floating, server, VPC, allowance, gateway, egress, minimal charges, ARP, traffic

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


# Precificação para VPC
{: #pricing-for-vpc}

A tabela resume a precificação para transferência de dados da Internet com o {{site.data.keyword.cloud}} Virtual Private Cloud. Lembre-se de que não há encargos para o tráfego entre o VPC e outros serviços Clássicos do IBM Cloud. Além disso, para serviços Pré-pagos, as camadas de serviço são ligadas à sua conta, não a uma instância específica do VPC. Os montantes são mostrados nos EUA dólares.

A precificação separada se aplica às [instâncias do servidor virtual](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) e ao [armazenamento de bloco](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) usados dentro do IBM Cloud VPC.

## Abonos grátis para transferência de dados da Internet
{: #free-allowances-for-internet-data-transfer}

| Transferência de dados |  Custo para todos os Clientes do IBM Cloud VPC |
|---------------|------------------|
| Dentro da zona | Grátis |
| Entre zonas na mesma região | Grátis |
| Uso de gateway público | Grátis (cobrado somente para o IP flutuante usado pelo PGW) |

## Precificação de IP flutuante
{: #floating-ip-pricing}

Um IP flutuante é cobrado com a taxa de $1 (EUA) por mês, iniciando quando ele é reservado. A taxa será cobrada mesmo se o IP flutuante não estiver associado a uma VSI ou não estiver em uso. O $1 para a taxa mensal será cobrado mesmo que o IP flutuante seja reservado por somente alguns dias.

## Precificação básica de Pré-pago para transferência de dados da Internet
{: #basic-paygo-pricing-for-internet-transfer}

| Transferência de dados | Quantidade de dados | Precificação PayGo |
|-----------|-----------|------------------|
| Egress para a Internet |  0 a 5 GB | Grátis |
|  | 6 a 10.000 GB | $0,087 por GB |
|  | 10.001 a 50.000 GB | $0,083 por GB |
|  | 50.001 a 150.000 GB | $0,07 por GB |
|  | 150.001 GB e mais | $0,05 por GB |


Ao criar um novo VPC, pode levar até uma hora para os encargos de faturamento iniciais aparecerem na API ou IU do Console.
{: tip}

Se você tiver um gateway público ou IP Flutuante, ainda poderá ver alguns encargos mínimos do egresso, mesmo que não tenha enviado nenhum tráfego de egresso durante esse tempo. Esses encargos são para o tráfego ARP, que é necessário para operar sua conta.
{: important}

## Precificação de LBaaS para VPC
{: #lb-for-vpc-pricing}

A precificação dos Load Balancers for VPC se baseia nas métricas a seguir, calculadas mensalmente:
* Horas de uso de serviço
* Dados processados


| Região | Uso do serviço LB (por hora) | Dados LB processados por gigabyte (GB) |
|------------|--------------------------|-------------------------|
| Dallas | US$ 0,025 | US$ 0,008 |
| Frankfurt | US$ 0,027 | US$ 0,009 |
| Tóquio | US$ 0,028 | US$ 0,009 |

Os preços variam por regiões de várias zonas.
{: note}

O gráfico a seguir mostra um exemplo para um cliente que usa 4.500 GB por mês para balanceamento de carga pública, em dólares dos EUA, baseados na região de Dallas.

| Item | Uso | Taxa | Custo |
|---------|--------|---------|---------|          
| Uso do serviço LB | 720 horas| US$ 0,025/hora | US$ 18 por mês |
| Dados processados | 4.500 GB  |  US$ 0,008/GB | US$ 36 por mês|

**Tabela 1: exemplo de custo mensal. O encargo total para este cenário é US$ 54 (USD) por mês.**


## Precificação do VPN for VPC
{: #vpn-for-vpc-pricing}

| Região | Conexão (Peer) por hora | Instância (Gateway) por hora |
|------------|--------------------------|-------------------------|
| Dallas | US$ 0,04 | US$ 0,045 |
| Frankfurt | US$ 0,044 | US$ 0,0495 |
| Tóquio | US$ 0,0452 | US$ 0,0585 |

A transferência de dados para a Internet como resultado do uso de VPNaaS é cobrada como transferência de dados regular, saída para a Internet, faturada no nível do VPC.
{: note}
