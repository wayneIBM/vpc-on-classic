---

copyright:
  years: 2019
lastupdated: "2019-05-21"

keywords: storage, capacity, billing, volume

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:new_window: target="_blank"}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Precificação para Block Storage for VPC
{: #block-storage-pricing}

A precificação para o {{site.data.keyword.block_storage_is_short}} é baseada na capacidade ou no IOPS que foi provisionado, dependendo do perfil de armazenamento escolhido.

| Camada de IOPS  | Taxa mensal publicada^1 |
|------------|--------------|
|  3 IOPS/GB |  US$ 0,13/GB |
|  5 IOPS/GB |  US$ 0,25/GB |
| 10 IOPS/GB |  US$ 0,58/GB |
| IOPS customizado | US$ 0,10/GB + US$ 0,07/IOPS |

^1 Preço publicado por mês, [calculado por hora](#how-are-charges-calculated).

## Como os encargos são calculados?
{: #how-are-charges-calculated}

O {{site.data.keyword.block_storage_is_short}} é calculado de hora em hora, com base no número total de horas que o volume de armazenamento de bloco existe na conta, até que o volume seja excluído ou o final de um ciclo de faturamento, o que ocorrer primeiro. O faturamento por hora é calculado de forma diferente para uma camada IOPS predefinida do que para IOPS customizado. Consulte os exemplos a seguir.

### Cálculo de faturamento para um volume com a camada IOPS de propósito geral
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

Você provisiona um volume de 1000-GB com a camada 3 IOPS/GB de propósito geral e, em seguida, usa o volume por 72 horas antes de excluí-lo. O preço total para o volume é faturado por hora, conforme a seguir:

((1.000 GB x US$ 0,13/GB)/730 horas) x 72 horas = US$ 12,82

### Cálculo de faturamento para um volume com IOPS customizado
{: #billing-calculation-for-a-volume-with-custom-IOPS}

Você provisiona um volume de 1.000 GB com 2.500 IOPS e, em seguida, usa o volume por 72 horas antes de excluí-lo. O preço total para o volume é faturado por hora, conforme a seguir:

(((1.000 x US$ 0,10/GB) + (2.500 x US$ 0,07))/730 horas) x 72 horas = US$ 27,12
