---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: glossary, terminology, definition, access, floating, geography, image, region, zone, instance, VSI, LBaaS, VPN, VPC, NAT, profile, resource, security group, shares, storage, VNPaaS, volume, subnet, SSH, key, gateway, ACL

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

# Glosario de VPC
{: #vpc-glossary}

La terminología siguiente se utiliza normalmente y es específica de {{site.data.keyword.cloud}} Virtual Private Cloud. Consulte los [Términos del glosario para IBM Cloud](/docs/overview/glossary?topic=overview-glossary) para obtener más información sobre términos más amplios y sobre más términos.

## Lista de control de accesos
{: #access-control-list}

Una lista de control de accesos (ACL) proporciona controles de seguridad para el tráfico de entrada y de salida para una [subred](#subnet). Una ACL no tiene estado. Cada ACL consta de reglas, basadas en una _IP de origen_, un _puerto de origen_, una _IP de destino_, un _puerto de destino_ y un _protocolo_.

En IBM Cloud VPC, cada subred se crea con una ACL predeterminada, que permite el tráfico de entrada y salida, pero los clientes pueden crear ACL personalizadas. Solo una ACL está conectada a un subred en cualquier momento, aunque es posible conectar una ACL a varias subredes.

A veces también se denomina _ACL de red_ o NACL.

## IP flotante
{: #floating-ip}

IP flotante es un método para proporcionar acceso de entrada y de salida a Internet para recursos de VPC, como instancias, un equilibrador de carga o un túnel de VPN, mediante el uso de _Direcciones IP flotantes_ asignadas desde una agrupación.

Las direcciones IP flotantes son direcciones IP públicas que proporciona el sistema desde la agrupación preexistente. No puede traer sus propias direcciones IP públicas. Se puede acceder a las direcciones IP flotantes desde internet, y se pueden asociar a una instancia (por ejemplo, una VSI, un equilibrador de carga o una pasarela VPN) mediante una vNIC.

Puede reservar una dirección IP flotante de la agrupación de direcciones IP flotantes disponibles proporcionada por IBM, y puede asociarla o eliminar su asociación de cualquier instancia de la misma nube privada virtual. La IP flotante utiliza [NAT](#nat) de uno a uno para permitir que un servidor se comunique con Internet público y también con una subred privada dentro del entorno de la nube.

## Geografía
{: #geography}

En una VPC, la designación de geografía proporciona información sobre la región y la zona dentro de las cuales se despliega la VPC y sus recursos asociados.

## Imagen
{: #image}

La información necesaria para iniciar una instancia de servidor virtual, o _instancia_. Por lo general, es una imagen instantánea de un sistema operativo disponible a nivel comercial, utilizado para arrancar.

## Direccionador implícito
{: #implicit-router}

La conectividad de red inherente entre todas las subredes creadas dentro de una VPC.

## Instancia
{: #instance}

Un nombre abreviado para un servidor virtual, o VSI, que se ejecuta dentro de una VPC.

## LBaaS
{: #lbaas}

Load Balancer as a Service (LBaaS) proporciona capacidad para distribuir el tráfico en una asignación equilibrada entre instancias de una VPC.

## NAT
{: #nat}

Network Address Translation (NAT) es un método de direccionamiento que se describe en [Internet RFC 1631 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc1631){: new_window} de modo que se pueda utilizar una dirección IP para comunicar con otras direcciones IP, como las de una subred privada, principalmente mediante una tabla de búsqueda. NAT tiene dos tipos principales: NAT de uno a uno y [NAT entre muchos y uno ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://en.wikipedia.org/wiki/Network_address_translation){: new_window}. NAT fue concebido originalmente como un modo de extender la vida útil de las direcciones IP IPv4, pero actualmente se suele utilizar en entorno de nube
como forma de crear comunicación con subredes privadas.

## Perfil
{: #profile}

Un perfil es una combinación ampliamente utilizada de vCPU y RAM de la que se puede crear rápidamente una instancia para iniciar una instancia de servidor virtual (VSI). Se da soporte a tres familias de perfiles: equilibrados, de cálculo y de memoria.


## Pasarela pública
{: #public-gateway}

Una pasarela pública (PGW) permite el acceso solo de salida para que una subred (con todas las VSI conectadas a la subred) se conecte a Internet. Tenga en cuenta que las subredes son privadas de forma predeterminada; sin embargo, si lo desea puede crear una pasarela pública y conectar una subred a la misma. Una vez conectada la subred a la pasarela pública, todas las VSI de dicha subred se podrán conectar a Internet.

PGW utiliza NAT entre muchos y uno, lo que significa que miles de VSI con direcciones privadas utilizarán una dirección IP para comunicarse con internet público. La pasarela pública no permite que Internet inicie una conexión con dichas instancias.

## Región
{: #region}

Área geográfica en la que se despliega una VPC. Cada región contiene varias zonas, que representan dominios de error independientes. IBM Cloud VPC abarca barias zonas dentro de su región asignada.

## Recurso
{: #resource}

Un tipo de activo que forma parte de un despliegue de VPC y que puede incurrir en cargos de facturación, por ejemplo una VSI, una IP flotante o la propia VPC. Los recursos se pueden combinar en _grupos de recursos_ para facilitar la contabilidad cuando determinados recursos se utilizan siempre en combinación en una VPC.

## Grupo de seguridad
{: #security-group}

Un grupo de seguridad actúa como un cortafuegos virtual que controla el tráfico de entrada o de salida de uno o varios servidores (VSI). Un grupo de seguridad es una colección de reglas que especifican si se debe permitir el tráfico para una VSI asociada. Cuando un cliente inicia una VSI, puede asociar uno o varios grupos de seguridad a dicha VSI. Si se proporcionan los permisos adecuados, los clientes pueden modificar las reglas del grupo de seguridad.

## Comparticiones
{: #shares}

Una forma de proporcionar almacenamiento de archivos en una VPC.

## Subred
{: #subnet}

Una subred es un rango de direcciones IP, vinculado a una sola zona, que no puede abarcar varias zonas o regiones. Una subred puede abarcar la totalidad de una zona en una IBM Cloud VPC.

A efectos de IBM Cloud VPC, la característica importante de una subred es el hecho de que las subredes se pueden aislar unas de otras, así como interconectar de la forma habitual. El aislamiento de subredes se consigue mediante listas de control de accesos (ACL) de red que actúan como cortafuegos para controlar el flujo de paquetes de datos entre subredes. Paralelamente, los grupos de seguridad actúan como cortafuegos virtuales para controlar el flujo de entrada y salida de paquetes de datos de instancias de servidor virtual (VSI) individuales.

El aislamiento de subredes le permite crear un espacio privado dentro de la nube pública.

## Clave SSH
{: #ssh-key}

Credenciales de acceso criptográfico que se generan automáticamente y que controlan el acceso a las instancias de servidor virtual de una VPC.

## Volúmenes
{: #volumes}

Almacenamiento de datos de alto rendimiento montado en hipervisor para instancias de servidor virtual de una VPC.

## VPC
{: #vpc}

Red virtual vinculada a una cuenta. Ofrece un control preciso sobre la infraestructura virtual y la segmentación del tráfico de la red, junto con protección y capacidad para escalar de forma dinámica.

## VPN
{: #vpn}

Una red privada virtual (VPN) permite una conexión privada entre dos puntos finales, incluso cuando los datos se transfieren a través de una red pública. Los clientes pueden compartir datos como si estuvieran conectados a una red privada. Por lo general, una VPN se utiliza junto con métodos de seguridad como la autenticación y el cifrado para proporcionar la máxima seguridad de los datos y privacidad.

## Conexión VPN
{: #vpn-connection}

Una conexión privada entre dos puntos finales, que sigue siendo privada y que se puede cifrar incluso cuando los datos se transfieren a través de una red pública.

## VPNaaS
{: #vpnaas}

VPN as a Service (VPNaaS) permite configurar una VPN con puntos finales dentro y fuera de una VPC.

## Zona
{: #zone}

Un dominio independiente de tolerancia a errores. Una zona es una abstracción diseñada para aumentar la tolerancia a errores y reducir la latencia. Una zona garantiza las propiedades siguientes:

 * Ya que cada zona es un dominio independiente de tolerancia a errores, es extremadamente improbable que dos zonas de una región fallen simultáneamente.
 * El tráfico entre zonas de una región experimentarán menos de 2 ms de latencia.
