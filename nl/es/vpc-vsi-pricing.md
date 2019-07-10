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

# Precios de {{site.data.keyword.vsi_is_short}} 
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (tema de ayuda enlazado)


{{site.data.keyword.vsi_is_full}} se ofrece en determinadas regiones con un máximo de 62 vCPU y 248 GB de RAM para que se ajuste a cualquier carga de trabajo. Solo se le factura una tarifa por hora, y se aplican más descuentos cuanto más tiempo se ejecuta la instancia. Los tiempos de uso de servidor virtual se calculan por segundo, tanto para el tiempo de uso como para el tiempo de suspensión de la instancia. Por ejemplo, si la instancia se ejecuta durante 45 minutos y 32 segundos, se le facturan 45 minutos y 32 segundos.
{:shortdesc}

## Uso sostenido
{: #sustained-usage}

Aunque las instancias se facturan según una tarifa por hora, cuanto más tiempo se esté ejecutando la instancia, más baja es la tarifa. A medida que transcurre el mes de facturación, recibirá los siguientes descuentos por hora.

| Tiempo transcurrido al mes       | Descuento en la facturación  | 
| ----------------------------- | ----------------- | 
| 0-20 %                         | tasa de facturación normal |                 
| 21-40 %                        | 5 %        |                  
| 41-60 %                        | 10 %       |                  
| 61-80 %                        | 15 %        |                  
| 81-100 %                       | 20 % |
{: caption="Tabla 1. Niveles de descuento" caption-side="top"}  

Estos niveles de descuento le proporcionan un ahorro del 10 % por mantener las instancias en ejecución mensualmente frente a cada hora. Este descuento solo se aplica a la tarifa horaria básica; no se aplica a los cargos de software, almacenamiento, red u otros.

<!-- As your workload demands change, you can always increase or decrease the size of your instance. If you resize to a larger instance size, the discounts reset and you pay the regular rate again. If you resize to a smaller instance size, the discounted rate does not reset. You continue to progress through the hourly discount tiers. -->

### Ejemplo de uso sostenido
{: #sustained-usage-example}

Supongamos que adquiere una instancia de la familia de servidores virtuales equilibrados, con 16 CPU y 64 GB de RAM, a un precio base de 0,795 dólares por hora. A medida que transcurre el mes, su tasa se reduce de la forma siguiente:

| Tiempo transcurrido al mes       | Descuento en la facturación  |  Tasa de ejemplo     |
| ----------------------------- | ----------------- | -------- |
| 0-20 %                         | tasa de facturación normal (0,795 $) | 116,07 $    |                
| 21-40 %                        | 5 %        |   110,27 $   |                 
| 41-60 %                        | 10 %       |    104,46 $  |            
| 61-80 %                        | 15 %        |    98,66 $    |                
| 81-100 %                       | 20 % |       92,86 $      |
{: caption="Tabla 2. Ejemplos de niveles de descuento" caption-side="top"}  

La factura total con este modelo, si se deja en ejecución durante todo el mes, es de 522,32 $. Los descuentos dan lugar a un ahorro mensual general del 10 %, si se compara con la tasa por hora.

## Precios de instancia base
{: #base-instance-prices}

Los precios de instancia base comienzan a partir de 0,087 $ por hora. Cuando se crea un servidor virtual, se le solicita que seleccione una familia de servidores virtuales y una configuración de perfil. Cuando realice la selección, la tarifa horaria asociada se muestra en la tabla. <!-- You can also use the Pricing Calculator to estimate your costs. --> 

## Sistemas operativos incluidos
{: #included-operating-systems}

Se incluyen los siguientes sistemas operativos sin cargo adicional:

* CentOS 7.más reciente
* Ubuntu 16.04 LTS, 18.04 (mínimo)
* Debian 8.más reciente, 9.más reciente (mínimo)

Hay sistemas operativos premium y otros complementos disponibles. Verá los precios reflejados en el Resumen de costes.

## Suspensión de la facturación
{: #suspend-billing}

Cuando se apaga una instancia, no se acumulan costes para determinados recursos de cálculo. La facturación se detiene automáticamente cuando se detiene la instancia. La característica de suspensión de facturación ayuda a reducir el coste y evita el tener que volver a crear una instancia cuando se necesitan de nuevo sus recursos.

En situaciones en las que desea incrementar o reducir la infraestructura en respuesta a las necesidades de la carga de trabajo, puede utilizar la característica de suspensión de la facturación como alternativa más rápida que crear y suprimir instancias.
{:tip}

### Detalles de facturación
{: #billing-details}

Es importante comprender qué costes se detienen y qué costes persisten cuando se apaga la instancia de servidor virtual.

La facturación se suspende solo cuando se apaga la instancia de servidor virtual a través de la consola de IBM Cloud, la CLI o la API. Si apaga el servidor virtual directamente mediante el SO, no se suspende la facturación correspondiente a esa instancia.
{:note}

Revise la tabla siguiente para obtener detalles sobre cómo afecta la suspensión de la facturación a los diversos cargos de recursos.

| Recurso                      | Fact. detenida   | Fact. persiste |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Actualizaciones ancho banda            |          X        |                  |
| Licencias de sistema operativo     |          X        |                  |
| IP flotantes, equilibradores de carga u otras ofertas de red conectadas |                   |         X        |
| Almacenamiento                       |                   |         X        |
{: caption="Tabla 1. Detalles de facturación de recursos" caption-side="top"}   

Los tiempos de uso se calculan por segundo, tanto para el tiempo de uso como para el tiempo de suspensión de la instancia de servidor virtual. Incluso si nunca inicia la característica de suspensión de facturación apagando la instancia, la facturación se calcula por segundo del ciclo de vida de la instancia. 
{:note}

#### Suspensión de la facturación y descuentos por uso sostenido
{: #suspend-billing-and-sustained-usage-discounts}

En el caso de descuentos por uso sostenido, se toma una imagen de la suspensión en el punto en que se ha apagado en el nivel de descuento. Es decir, un descuento de uso solo se aplica al momento en que la imagen está en uso, no al momento en que se ha suspendido la imagen.

#### Cargo por uso mínimo
{: #minimum-usage-charge}

Las instancias de servidor virtual tienen un cargo por uso mínimo al mes. Si el uso supera el 25 % durante el ciclo de facturación, se le factura el uso real. Si el uso es menor que el 25 % del tiempo en que ha existido durante el ciclo de facturación, se aplica el cargo mínimo del 25 %. 

Por ejemplo, supongamos que tiene una instancia en ejecución durante un ciclo de facturación completo (720 horas). De ese tiempo, la instancia ha estado suspendida durante 577 horas y ha estado en ejecución durante 143 horas. Se facturan para la instancia 180 horas (el mínimo de las horas disponibles en dicho periodo de facturación).  

Sin embargo, supongamos que tenía una instancia que se creó y se detuvo dentro de un ciclo de facturación y que duró 400 horas en total. De este tiempo, la instancia estuvo suspendida durante 120 horas y en ejecución durante 280 horas. Se facturaría para la instancia su uso de 280 horas.

#### Factura
{: #billing-invoice}

Cuando suspenda la facturación en un servidor virtual, verá algunos cambios en su factura. Los cargos relevantes ahora aparecen como detalles basados en el uso. Por ejemplo, es posible que vea las adiciones siguientes que reflejan las horas disponibles, las horas utilizadas y el número total de horas asignadas:

```
Uso de instancia de cálculo...
Uso de RAM...
Uso de sistema operativo...
```
{:screen}

### Detalles de recursos
{: #resource-details}

#### Almacenamiento
{: #suspend-billing-storage}

Si suspende la facturación en una instancia de servidor virtual, el almacenamiento asociado persiste, pero no puede acceder a los datos mientras la instancia de servidor virtual esté apagada. Cuando reanude la facturación en la instancia, podrá acceder a sus datos y comenzará la facturación normal.

#### Direcciones IP
{: #suspend-billing-ip-addresses}

Todas las configuraciones de red e IP (IP privadas del rango de subred) permanecen sin cambios mientras la instancia está suspendida.

Puede ver si el dispositivo está detenido en la página de detalles de la instancia. Para ver cuándo ha cambiado el estado, pulse **Actividad** en el panel de navegación. 

#### Limitaciones
{: #suspend-billing-limitations}

Los servidores virtuales suspendidos continuarán contando en la cuota de dispositivo a nivel de cuenta.
