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

# Precios de Block Storage for VPC
{: #block-storage-pricing}

Los precios para {{site.data.keyword.block_storage_is_short}} se basan en la capacidad o el IOPS que se ha suministrado, en función del perfil de almacenamiento que haya elegido.

| Nivel de IOPS  | Tasa mensual publicada^1 |
|------------|--------------|
|  3 IOPS/GB |  0.13 USD/GB |
|  5 IOPS/GB |  0.25 USD/GB |
| 10 IOPS/GB |  0.58 USD/GB |
| Custom IOPS| 0.10 USD/GB + 0.07 USD/IOPS |

^1 Precio publicado al mes, [calculado cada hora](#how-are-charges-calculated).

## ¿Cómo se calculan los cargos?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} se calcula cada hora, basándose en el número de horas que el volumen de almacenamiento en bloques existe en la cuenta, hasta que se suprime el volumen o se llega al final de un ciclo de facturación, lo que venga primero. La facturación por horas se calcula de forma distinta para un nivel de IOPS predefinido que para IOPS personalizados. Vea los ejemplos siguientes.

### Cálculo de facturación de un volumen con el nivel IOPS de finalidad general
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

Suministra un volumen de 1000 GB con el nivel de finalidad general 3 IOPS/GB, utiliza el volumen durante 72 horas y después lo suprime. El precio total del volumen se factura por horas, como se indica a continuación:

((1000 GB x 0.13 USD/GB)/730 hrs) x 72 hrs = $12.82

### Cálculo de facturación para un volumen con IOPS personalizado
{: #billing-calculation-for-a-volume-with-custom-IOPS}

Suministra un volumen de 1000 GB con 2500 IOPS, utiliza el volumen durante 72 horas y después lo suprime. El precio total del volumen se factura por horas, como se indica a continuación:

(((1000 x 0.10 USD/GB) + (2500 x 0.07 USD)) / 730 hrs) x 72 hrs = $27.12
