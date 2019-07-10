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


# Precios de VPC
{: #pricing-for-vpc}

En esta tabla se resumen los precios de la transferencia de datos de Internet con la nube privada virtual de {{site.data.keyword.cloud}}. Recuerde que el tráfico entre la VPC y otros servicios clásicos de IBM Cloud no se factura. Además, para los servicios PayGo, los niveles de servicio están vinculados a su cuenta, no a una instancia específica de VPC. Los importes se muestran en dólares de EE. UU.

Se aplican distintos precios a las [instancias de servidor virtual](/docs/vpc-on-classic?topic=vpc-on-classic-pricing-for-virtual-servers-for-vpc) y al [almacenamiento en bloques](/docs/vpc-on-classic?topic=vpc-on-classic-block-storage-pricing) que se utilizan en la VPC de IBM Cloud.

## Concesiones gratuitas para la transferencia de datos en Internet
{: #free-allowances-for-internet-data-transfer}

| Transferencia de datos |  Coste para todos los clientes de IBM Cloud VPC |
|---------------|------------------|
| Dentro de la zona | Gratuito |
| Entre zonas de la misma región | Gratuito |
| Uso de pasarela pública | Gratuito (solo se factura la IP flotante que utiliza PGW) |

## Precios de IP flotante
{: #floating-ip-pricing}

La tasa de facturación de una IP flotante es de 1 $ (EE. UU.) al mes, empezando en el momento en que se reserva. La tarifa se cargará incluso si la IP flotante no está asociada a una VSI o si no se utiliza. El dólar de la tarifa mensual se carga incluso si la IP flotante se reserva solo unos días.

## Precios básicos de PayGo para la transferencia de datos en Internet
{: #basic-paygo-pricing-for-internet-transfer}

| Transferencia de datos | Cantidad de datos | Precios de PayGo |
|-----------|-----------|------------------|
| Salida a Internet |  0 a 5 GB | Gratuito |
|  | 6 a 10.000 GB | 0,087 $ por GB |
|  | 10.001 a 50.000 GB | 0,083 $ por GB |
|  | 50.001 a 150.000 GB | 0,07 $ por GB |
|  | 150.001 GB y superiores | 0,05 $ por GB |


Cuando crea una nueva VPC, los cargos de facturación iniciales pueden tardar hasta una hora en aparecer en la interfaz de usuario de la consola o en la API.
{: tip}

Si tiene una pasarela pública o una IP flotante, es posible que siga viendo algunos cargos mínimos de salida, aunque no haya enviado ningún tráfico de salida durante ese tiempo. Estos cargos corresponden al tráfico de ARP, necesario para trabajar con su cuenta.
{: important}

## Precios de LBaaS para VPC
{: #lb-for-vpc-pricing}

Los precios de los equilibradores de carga para VPC se basan en las métricas siguientes, calculadas mensualmente:
* Horas de uso de servicio
* Datos procesados


| Región | Uso de servicio de LB | Datos procesados de LB por GigaByte (GB) |
|------------|--------------------------|-------------------------|
| Dallas | 0,025 $ | 0,008 $ |
| Frankfurt | 0,027 $ | 0,009 $ |
| Tokio | 0,028 $ | 0,009 $ |

Los precios varían según la región multizona.
{: note}

El gráfico siguiente muestra un ejemplo para un cliente que utiliza 4500 GB al mes para el equilibrio de carga pública, en dólares de EE.UU., con sede en la región de Dallas.

| Elemento | Uso | Tarifa | Coste |
|---------|--------|---------|---------|          
| Uso de servicio LB| 720 horas| 0,025 $/hora | 18 $ al mes |
| Datos procesados | 4500 GB  |  0,008 $/GB | 36 $ al mes|

**Tabla 1: Ejemplo de coste mensual. El cargo total para este caso de ejemplo es de 54 $(USD) al mes.**


## Precios de VPN para VPC
{: #vpn-for-vpc-pricing}

| Región | Conexión (homólogo) por hora | Instancia (pasarela) por hora |
|------------|--------------------------|-------------------------|
| Dallas | 0,04 $ | 0,045 $ |
| Frankfurt | 0,044 $ | 0,0495 $ |
| Tokio | 0,0452 $ | 0,0585 $ |

La transferencia de datos a internet como resultado de utilizar VPNaaS se cobra como una transferencia de datos normal, con salida a internet, facturada a nivel de la VPC.
{: note}
