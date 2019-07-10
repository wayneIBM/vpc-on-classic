---

copyright:
  years: 2019
lastupdated: "2019-05-29"


keywords: vpc, delete, resources

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

# Supresión de una VPC
{: #deleting}

Un recurso de {{site.data.keyword.cloud}} Virtual Private Cloud no se puede suprimir si contiene otros recursos o si está conectado a otro recurso. Este documento describe las relaciones entre los recursos de VPC y proporciona información sobre las mejores prácticas para suprimir una VPC.

En la tabla siguiente se resumen los tipos de recursos de VPC y las relaciones que se deben tener en cuenta al suprimir esos recursos.

| Recurso | Antes de suprimirlo... | Después de suprimirlo... |
| ---------------- | ----------------------------------------- | --------------------------- |
| VPC | Se deben suprimir todas las subredes y las pasarelas públicas de la VPC | Todos los grupos de seguridad y prefijos de direcciones se suprimen automáticamente |
| Subred | Se deben suprimir todas las instancias y las interfaces de red de la subred; se deben suprimir todas las pasarelas de VPN y los equilibradores de carga de la subred | Se desconectará cualquier pasarela pública que sirva a la subred; se desconectará cualquier ACL de red asociada a la subred |
| Instancia | ---- | Todas las interfaces de red se suprimen automáticamente; el volumen de arranque se suprime con la instancia; la supresión del volumen secundario depende del parámetro `delete_volume_on_instance_delete` ; cualquier IP flotante conectada a una de las interfaces de red se desconecta automáticamente |
| Interfaz de red | ---- | Se libera cualquier IP flotante conectada a la interfaz de red |
| Clave | ---- | Después de suprimir una clave, ya no se puede utilizar para suministrar una instancia nueva ni para realizar una recarga de sistema operativo en una instancia existente. No obstante, la clave sigue estando disponible en cualquier instancia que haya suministrado con la misma, y puede seguir utilizándola para iniciar sesión.  |
| Imagen | ---- | La imagen no se puede utilizar para suministrar una nueva instancia, pero las instancias existentes con la imagen no se ven afectadas. |
| Volumen | El volumen se debe desconectar de todas las instancias | ---- |
| ACL de red | La ACL de red se debe desconectar de todas las subredes; las ACL de red predeterminadas de una VPC no se pueden suprimir  | ---- |
| Grupo de seguridad | El grupo de seguridad se debe desconectar de todas las interfaces de red; el grupo de seguridad predeterminado de una VPC no se puede suprimir. | ---- |
| IP flotante | La IP flotante se debe desconectar de cualquier interfaz de red | ---- |
| Pasarela pública | La pasarela pública se debe desconectar de todas las subredes |  ---- |
| Equilibrador de carga | ---- | ---- |
| Pasarela de VPN | ---- | Debido a la capacidad de compartir políticas de IKE e IPsec entre las pasarelas, éstas no se suprimirán cuando se suprima una pasarela de VPN. Las políticas de IKE e IPsec se deben eliminar manualmente. |


## Los recursos de VPC no se pueden suprimir si están en un estado transitorio
{: #deleting-status}

Los recursos de VPC no se pueden suprimir cuando están en un estado transitorio, como por ejemplo `pending`. Todos los recursos deben estar en un estado "final" como, por ejemplo, `available` o `failed`, para poder suprimirlos. Debe esperar a que el recurso esté en estado `available` o `failed` antes de intentar suprimirlo. [Póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si un recurso no muestra un estado `available` o `failed` en un plazo de 30 minutos.

Cuando se emite una solicitud de supresión de un recurso, el recurso entra en el estado transitorio `deleting`. Espere a que el recurso desaparezca de la lista antes de intentar suprimir el recurso padre o conectado. En algunos casos, la operación de supresión puede fallar, y el recurso entra en estado `failed` al cabo de unos minutos. Póngase en contacto con el servicio de soporte si un recurso no desaparece de la lista o muestra el estado `failed` en un plazo de 30 minutos desde que realiza la solicitud de supresión. Una vez que el recurso está en estado `failed`, puede intentar suprimirlo de nuevo. [Póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) si no puede suprimir un recurso en estado `failed`.

## Siga este orden al suprimir una VPC
{: #deleting-order}

Una VPC contiene subredes y pasarelas públicas. Una pasarela pública se puede conectar a una o más subredes en la VPC. Dentro de una subred de una VPC hay un equilibrador de carga y una pasarela de VPN gateway. Una subred también puede contener instancias de servidor virtual (VSI), y una VSI puede tener varias interfaces de red en subredes diferentes dentro de la VPC. Para poder suprimir una subred, antes se debe suprimir las pasarelas de VPN, los equilibradores de carga y las interfaces de red que contenga. Una VPC contiene también grupos de seguridad, pero estos se suprimen automáticamente cuando se suprime la VPC. Los prefijos de dirección son atributos de una VPC, y se suprimen automáticamente cuando se suprime la VPC.

Siga este orden al suprimir una VPC:

1.  Suprima todas las pasarelas de VPN que se hayan suministrado en cualquier subred de la VPC.
2.  Desconecte todos los equilibradores de carga que se hayan suministrado en cualquier subred de la VPC.
3.  Suprima todas las instancias que tengan interfaces de red en cualquier subred de la VPC. Cualquier IP flotante asociada con una interfaz de red se desconecta automáticamente de la interfaz de red cuando se suprime la instancia.
4.  Suprima todas las subredes de la VPC. Cualquier pasarela pública conectada a una subred se desconecta automáticamente de la subred cuando se suprime la subred.
5.  Suprima todas las pasarelas públicas de la VPC.
6.  Una vez que la VPC no tiene subredes ni pasarelas públicas, ya se puede suprimir. Todos los prefijos de direcciones de la VPC se suprimen automáticamente cuando se suprime la VPC.

## Relaciones entre los recursos de una VPC
{: #deleting-relationships}

Algunos recursos de VPC están contenidos en otros recursos de VPC (como "hijos"), pero otros recursos de VPC están contenidos a nivel de cuenta, fuera de una VPC específica. Todos los recursos hijo se deben suprimir para poder suprimir el recurso padre. Para los recursos de VPC a nivel de cuenta, se deben suprimir todos los enlaces a recursos existentes para poder suprimir el recurso de cuenta.

En algunos casos, la supresión de un recurso de VPC elimina automáticamente el enlace a los recursos existentes. En las tablas siguientes se describe el comportamiento esperado.

### VPC
{: #deleting-details}

Las VPC contienen subredes y pasarelas públicas. En una subred se suministran equilibradores de carga, pasarelas de VPN e instancias. Para poder suprimir una VPC, antes se deben suprimir todas las subredes y las pasarelas públicas de la VPC. En otras palabras, antes se tienen que suprimir todas las pasarelas públicas, los equilibradores de carga, las pasarelas de VPN y las instancias de la VPC.

Una VPC también contiene prefijos de direcciones y grupos de seguridad, pero estos recursos se suprimen automáticamente cuando se suprime la VPC.

Una VPC debe tener un grupo de seguridad predeterminado y una ACL de red predeterminada. Si crea una VPC sin especificar un grupo de seguridad y una ACL de red predeterminados, se creará automáticamente un grupo de seguridad y una ACL de red predeterminados para la VPC. El grupo de seguridad y la ACL de red predeterminados no se pueden suprimir. La ACL de red predeterminada se suprime automáticamente cuando se suprime la VPC. Todos los grupos de seguridad de la VPC se suprimen automáticamente cuando se suprime la VPC.

| En la VPC | Puede contener | Puede conectarse a | ¿Tiene estado? | Se suprime automáticamente cuando se suprime la VPC | Se desconecta automáticamente cuando se suprime |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Subred | instancias, equilibradores de carga, pasarelas de VPN | pasarela pública, ACL de red | Sí| No | Sí|
| Pasarela pública| --- | subredes, IP flotantes | Sí | No | No para las subredes, Sí para las IP flotantes |
| Grupos de seguridad | ---  | instancias (NIC), VPC de forma predeterminada | No| Sí | No |

### Subred
{: #deleting-subnet}

Se deben suprimir todos los recursos de una subred para poder suprimir la subred. Las subredes contienen las interfaces de red de la instancia, los equilibradores de carga y las pasarelas de VPN. Se debe suprimir la interfaz de red asociada a la subred para poder suprimir la subred, junto con los equilibradores de carga o la pasarela de VPN que se suministre en la subred.

Una instancia puede tener varias interfaces de red, y esas interfaces de red pueden pertenecer a varias subredes de la VPC. Para poder suprimir una subred, se debe suprimir primero cualquier interfaz de red que haya en la subred. La interfaz de red primaria de la instancia no se puede suprimir ni mover a otra subred, por lo que se deben suprimir todas las instancias con una interfaz de red primaria en la subred para poder suprimir la subred.


| En la subred | Puede contener | Puede conectarse a | ¿Tiene estado? | Se suprime automáticamente cuando se suprime la subred | Se desconecta automáticamente cuando se suprime |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Instancia (interfaz de red) | varias interfaces de red | conexiones de volumen, grupos de seguridad | Sí| No  | Sí|
| VPN | --- | ---| Sí | No  | --- |
| Equilibrador de carga | ---  | --- | Sí | No | ---  |

### Instancia
{: #deleting-instance}

No hay requisitos previos necesarios para suprimir una instancia. Cuando se suprime la instancia, se suprimen automáticamente todas sus interfaces de red. Se suprimen el volumen de arranque de la instancia y todas sus conexiones de volumen. Todas las IP flotantes conectadas a cualquiera de sus interfaces de red se liberan automáticamente. Cualquier volumen de almacenamiento en bloques conectado a la instancia se suprime automáticamente, si se ha creado el volumen con el distintivo `delete_volume_on_instance_delete` establecido en true; de lo contrario, la instancia se desconecta del volumen, pero el volumen permanece. Si algún grupo de seguridad está conectado a alguna de las interfaces de red de la instancia, también se desconectan automáticamente cuando se suprimen las interfaces de red.

| En la instancia | Puede contener | Puede conectarse a | ¿Tiene estado? | Se suprime automáticamente cuando se suprime la instancia | Se desconecta automáticamente cuando se suprime |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Interfaz de red | --- | subredes, IP flotante, grupos de seguridad | No | Sí | Sí |

### Equilibrador de carga
{: #deleting-lb}

No hay requisitos previos necesarios para suprimir un equilibrador de carga. Cuando se suprime el equilibrador de carga, todos los escuchas, las agrupaciones y los miembros de las agrupaciones que forman parte del equilibrador de carga se suprimen automáticamente.

La supresión de un equilibrador de carga puede tardar hasta 30 minutos. La solicitud de supresión cambia de forma inmediata el estado de suministro del equilibrador de carga a `deleting`. Sin embargo, no se suprime realmente hasta que el equilibrador de carga desaparece de la consulta de lista.
{: important}

### VPN
{: #deleting-vpn}

No hay requisitos previos necesarios para suprimir una pasarela de VPN. Cuando se suprime la pasarela VPN, sus conexiones asociadas también se suprimen automáticamente. Las políticas de IKE y las políticas de IPSec no se suprimen al suprimir una pasarela de VPN.

La supresión de una pasarela de VPN puede tardar hasta 30 minutos. La solicitud de supresión cambia de forma inmediata el estado de suministro de la pasarela VPN a `deleting`. Sin embargo, no se suprime realmente hasta que la pasarela VPN desaparece de la consulta de lista.
{: important}

### IP flotante
{: #deleting-floating-ip}

Existe una IP flotante fuera de una VPC, a nivel de cuenta. Si la IP flotante está enlazada a una pasarela o una instancia pública, se debe suprimir (liberar) el enlace para poder suprimir la IP flotante.

Si suprime un recurso al que está enlazado la IP flotante, por ejemplo, si suprime la interfaz de red de una instancia, la IP flotante se libera automáticamente.

### Volumen
{: #deleting-volume}

Pueden existir dos tipos de volúmenes en una VPC: un volumen de arranque de almacenamiento en bloques y un volumen de datos de almacenamiento en bloques. Estos volúmenes se suprimen de forma diferente.

**Supresión de volúmenes de arranque**

Los volúmenes de arranque existen dentro de una instancia, y no se consideran entidades independientes de dicha instancia. Cuando se crea una instancia, también se crea un volumen de arranque y se conecta a la instancia. El volumen de arranque se suprime automáticamente cuando se suprime la instancia. No se puede suprimir el volumen de arranque sin suprimir la instancia.

**Supresión de volúmenes de datos**

Los volúmenes de datos de almacenamiento de bloques se pueden suministrar y gestionar aparte de sus instancias de servidor virtual asociadas. Puede adjuntar un volumen de datos como almacenamiento secundario a una instancia de servidor virtual. No se puede suprimir un volumen de datos que esté conectado a una instancia (es decir, si el volumen está "activo"). Primero se debe desconectar el volumen de la instancia y entonces se puede suprimir el volumen.

Un volumen de datos tiene un atributo (o distintivo) denominado `delete_volume_on_instance_delete` en la API y `Auto Delete` en la CLI y la IU. Si este distintivo está establecido en `true` (`Enabled` en la IU), cuando se suprime la instancia que tiene el volumen conectado, el volumen se desconecta y se suprime automáticamente. Si el distintivo del volumen está establecido en `false` (`Disabled` en la IU), la instancia se desconecta del volumen, pero el volumen no se suprime cuando se suprime la instancia con la que está conectado. El volumen se puede conectar a otra instancia.

Un volumen de almacenamiento en bloques se puede conectar a un solo servidor virtual a la vez.
{: tip}

### Grupos de seguridad
{: #deleting-secgroup}

No se puede suprimir un grupo de seguridad si lo está utilizando una interfaz de red, o si es el grupo de seguridad predeterminado de una VPC. Antes de suprimir el grupo de seguridad, elimine todas las interfaces de red de dicho grupo de seguridad y asegúrese de que el grupo de seguridad no se esté utilizando como grupo de seguridad predeterminado de la VPC.

Al suprimir una VPC, se suprimirán automáticamente todos los grupos de seguridad de esa VPC.

### ACL de red
{: #deleting-netacl}

No se puede suprimir una ACL de red si la está utilizando una subred, o si es la ACL de red predeterminada de una VPC. Antes de suprimir la ACL de red, desconecte la ACL de red de todas las subredes y asegúrese de que la ACL de red no se esté utilizando como la ACL de red predeterminada de una VPC.

Cuando se crea una VPC, ésta necesita una ACL de red predeterminada. Si no se especifica una ACL de red existente como predeterminada, cuando se crea la VPC, se crea una nueva ACL de red y se establece como predeterminada. Esta ACL de red predeterminada se suprime automáticamente cuando se suprime la VPC, si la ACL no se está utilizando en ningún otro lugar.

A diferencia de los grupos de seguridad, las ACL de red se pueden asignar a varias VPC. Por lo tanto, al suprimir una VPC no se suprimen las ACL de red.
{: note}

## Siguientes pasos
{: #deleting-nextsteps}

En los temas siguientes se proporcionan más ejemplos sobre cómo suprimir recursos de VPC, utilizando la API de la consola de IBM Cloud, la interfaz de línea de mandatos o la API.

-   [Supresión de recursos de VPC utilizando la interfaz de usuario](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-console)
-   [Supresión de recursos de VPC utilizando la CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli)
-   [Supresión de recursos de VPC utilizando la API](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-api)
