---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, storage, boot, block, volume, name, naming, best practices

subcollection: vpc-on-classic


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}

# Creación y gestión del almacenamiento en bloques en VPC
{: #creating-and-managing-storage-in-vpc}

{{site.data.keyword.cloud}} Virtual Private Cloud proporciona un volumen de almacenamiento de arranque y permite volúmenes adicionales de almacenamiento en bloques, que son un almacenamiento de datos de alto rendimiento montado en hipervisores para las instancias de servidor virtual (VSI). La infraestructura de VPC proporciona un escalado rápido de almacenamiento en varias regiones y zonas, con un rendimiento y una seguridad extra.

## Resumen de características
{: #summary-of-characteristics}

Cuando se suministra una instancia de {{site.data.keyword.vsi_is_full}}, se crea automáticamente un volumen de almacenamiento en bloques IOPS (3 IOPS/GB) de finalidad general de 100 GB, como un volumen de arranque primario, conectado a la VSI. 
Durante el suministro, puede cambiar el nombre del volumen de arranque y el tamaño del volumen.
{:shortdesc}

* Puede crear volúmenes de almacenamiento en bloques siempre que suministre una instancia de servidor virtual en una red de VPC.  
* Puede crear nuevos volúmenes, independientemente del ciclo de vida de la VSI, y después conectar esos volúmenes a una VSI.
* Puede crear volúmenes de datos secundarios, conectados a la VSI, automáticamente. 
* Los volúmenes de datos persisten cuando se desconectan de la VSI. Por lo tanto, más tarde puede conectar un volumen a una nueva instancia. 
* Puede especificar si los volúmenes de datos se suprimen automáticamente cuando se suprime una VSI.  
* Los volúmenes de arranque se suprimen cuando se suprime su VSI.
* De forma predeterminada, los volúmenes de arranque y de datos se cifran con el cifrado gestionado por IBM. 
* Puede cifrar los volúmenes utilizando sus propias claves de cifrado, al suministrar una VSI o al crear un volumen autónomo.


## Volúmenes de almacenamiento en bloques
{: #block-storage-volumes}

Dispone de información más completa sobre cómo trabajar con volúmenes de almacenamiento en bloques y con VPC en la [documentación de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started).

Para obtener información general sobre volúmenes almacenamiento en bloques para VPC, consulte [Acerca de Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about). 

Para empezar a crear volúmenes independientemente del suministro de VSI, consulte [Cómo empezar con {{site.data.keyword.block_storage_is_short}}](/docs/infrastructure/block-storage-is?topicid=block-storage-is-block-storage-getting-started).



## Prácticas recomendadas para crear y asignar un nombre a los volúmenes de almacenamiento en bloques de VPC:
{: #best-practices-for-creating-and-naming-your-vpc-block-storage-volumes}

* Determine los requisitos de almacenamiento antes de suministrar un volumen. Permita una capacidad adecuada y un rendimiento de IOPS.
* Decida si un perfil IOPS predefinido satisface mejor sus necesidades de capacidad y de rendimiento, o si es mejor especificar valores personalizados.
* Decida si desea crear volúmenes de datos secundarios durante el suministro de instancias de servidor virtual. Estos volúmenes se conectan automáticamente a la instancia.
* Decida si desea que el volumen conectado a una VSI se suprima automáticamente cuando suprima la VSI.
* Al cifrar los volúmenes con su propia clave de cifrado, asegúrese de que la clave es válida, que tiene autorización para utilizarla y que se puede utilizar en la región actual.
* Asegúrese de asignar un nombre a TODOS los volúmenes en el momento del suministro, incluido el volumen de arranque.
* Asegúrese de que asignar un nombre a TODAS las conexiones de volumen en el momento de realizar la conexión.
* Cada volumen debe tener un nombre distinto en una región dentro de una cuenta.

Puede volver a utilizar el nombre de un volumen después de que se suprima dicho volumen. Sin embargo, tenga en cuenta que el hecho de volver a utilizar un nombre de volumen podría provocar confusiones a efectos de facturación.
{:note}
