---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: create, configure, permissions, ACL, virtual, server, instance, subnet, block, storage, volume, security, group, images, Windows, Linux, example, monitoring, VPN, load balancer, IKE, IPsec

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
{:DomainName: data-hd-keyref="DomainName"}

# Creación de una VPC mediante la consola de {{site.data.keyword.cloud_notm}}
{: #creating-a-vpc-using-the-ibm-cloud-console}
[comment]: # (tema de ayuda enlazado)

En este documento se muestra cómo crear y configurar una nube privada virtual de {{site.data.keyword.cloud}} utilizando la consola de {{site.data.keyword.cloud_notm}}.

Para crear y configurar su nube privada virtual (VPC) y otros recursos conectados, siga los pasos de las secciones siguientes, en este orden:

1. Cree una VPC y una subred para definir la red. Cuando cree la subred, conecte una pasarela pública para permitir que todos los recursos de la subred se comuniquen con la internet pública.
1. Configure una lista de control de accesos (ACL) para limitar el tráfico de entrada y de salida de la subred. De forma predeterminada, se permite todo el tráfico.
1. Cree una instancia de servidor virtual.
1. Cree un volumen de almacenamiento en bloques y adjunte el archivo a una instancia.
1. Configure un grupo de seguridad para definir el tráfico de entrada y de salida que está permitido para la instancia.
1. Reserve y asocie una dirección IP flotante para que se pueda acceder a su instancia desde Internet.
1. Cree un equilibrador de carga para distribuir las solicitudes entre varias instancias.
1. Cree una red privada virtual (VPN) para que la VPC pueda conectarse de forma segura a otra red privada, como por ejemplo la red local u otra VPC.

## Antes de empezar
{: #before}
Asegúrese de tener permisos suficientes para crear y gestionar recursos en la VPC. Para obtener más información, consulte [Gestión de permisos de usuario para recursos de VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

Genere una clave SSH, que se utilizará para conectar con la instancia de servidor virtual. Por ejemplo, genere una clave SSH en el servidor Linux ejecutando el mandato `ssh-keygen -t rsa -C "ID_usuario"`. Este mandato genera dos archivos. La clave pública generada se encuentra en el archivo `<your key>.pub`.

Si tiene previsto crear un equilibrador de carga y utilizar HTTPs para el escucha, se necesita un certificado SSL. Puede gestionar los certificados con [IBM Certificate Manager ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/certificate-manager){: new_window}. También debe crear una autorización para permitir que la instancia del equilibrador de carga acceda a la instancia del gestor de certificados que contiene el certificado SSL. Puede crear una autorización a través de [Autorizaciones de gestión de identidad y acceso ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam/#/authorizations){: new_window}. Para el origen, seleccione **Infraestructura de VPC** como servicio de origen, **Equilibrador de carga para VPC** como tipo de recurso y **Todas las instancias de recursos** para la instancia del recurso de origen. Seleccione **Gestor de certificados** como servicio de destino y asigne **Escritor** como rol de acceso a servicio. Establezca la instancia del servicio de destino en **Todas las instancias** o en la instancia específica del gestor de certificados. Para obtener más información, consulte [Utilización de equilibradores de carga en IBM Cloud VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).

## Creación de una VPC y una subred

Para crear una VPC y una subred:

1. Abra [la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}
1. Pulse el **icono de Menú ![Icono de menú](../../icons/icon_hamburger.svg) > Infraestructura de VPC > Red > VPC** y pulse **Nueva nube privada virtual**.
1. Especifique un nombre para la VPC, como por ejemplo `my-vpc`.
1. Seleccione un grupo de recursos para la VPC y todos sus recursos adjuntos. Los grupos de recursos le permiten organizar los recursos de la cuenta para facilitar el control de accesos y la facturación. Para obtener más información, consulte [Prácticas recomendadas para organizar los recursos en un grupo de recursos](/docs/resources?topic=resources-bp_resourcegroups).
1. _Opcional:_ especifique etiquetas que le ayuden a organizar y a encontrar recursos. Puede añadir más etiquetas posteriormente. Para obtener más información, consulte [Cómo trabajar con etiquetas](/docs/resources?topic=resources-tag).
1. Seleccione o cree la ACL predeterminada para las nuevas subredes de esta VPC. En esta guía de aprendizaje, vamos a crear una nueva ACL predeterminada. Más tarde configuraremos reglas para la ACL.
1. Seleccione si el grupo de seguridad predeterminado permite el tráfico SSH y ping de entrada en las instancias de servidor virtual de esta VPC. Luego configuraremos más reglas para el grupo de seguridad predeterminado.
1. Especifique un nombre para la nueva subred de la VPC, como por ejemplo `my-subnet`.
1. Seleccione una ubicación para la subred. La ubicación consta de una región y una zona.

    La región que selecciona se utiliza como la región de la VPC. Todos los recursos adicionales que cree en esta VPC se crearán en la región seleccionada.
    {: tip}

1. Especifique un rango de IP para la subred en la notación CIDR, por ejemplo: `10.240.0.0/24`. En la mayoría de los casos, puede utilizar el rango de IP predeterminado. Si desea especificar un rango de IP personalizado, puede utilizar el calculador de rangos de IP para seleccionar otro prefijo de dirección o para cambiar el número de direcciones.
1. Seleccione una ACL para la subred. Seleccione **Utilizar valor predeterminado de VPC** para utilizar la ACL predeterminada que se ha creado para esta VPC.
1. Conecte una pasarela pública a la subred para permitir que todos los recursos conectados se comuniquen con la internet pública.  

    También puede conectar la pasarela pública después de crear la subred.
    {: tip}

1. Pulse **Crear nube privada virtual**.
1. Para crear otra subred en esta VPC, pulse el separador **Subredes** y pulse **Nueva subred**. Cuando defina la subred, asegúrese de seleccionar `my_vpc` en el campo **Nube privada virtual**.

## Configuración de la ACL

Puede configurar la ACL para limitar el tráfico de entrada y de salida a la subred. De forma predeterminada, se permite todo el tráfico.

Solo se puede adjuntar una ACL a cada subred. Sin embargo, cada ACL se puede conectar a varias subredes.

Para configurar la ACL:

1. En el panel de navegación, pulse **Red > Subredes**.
1. Pulse la subred que ha creado.
1. En el área **Detalles de subred**, pulse el nombre de la ACL.
1. Pulse **Añadir regla** para configurar las reglas de entrada y de salida que definen el tráfico de entrada y de salida que se permite en la subred. Para cada regla, especifique la información siguiente:  
   * Especifique la prioridad de la regla. Las reglas con números más bajos se evalúan en primer lugar y prevalecen sobre las reglas con números más altos. Por ejemplo, si una regla con prioridad 2 permite el tráfico HTTP y una regla con prioridad 5 deniega todo el tráfico, se permite el tráfico HTTP.  
   * Seleccione si desea permitir o denegar el tráfico especificado.  
   * Especifique un bloque CIDR para indicar el rango de IP al que se aplica la regla.
   * Seleccione los protocolos y puertos a los que se aplica la regla.
1. Cuando termine de crear reglas, pulse el rastro de navegación **Todas las listas de control de accesos** de la parte superior de la página.

### ACL de ejemplo

Por ejemplo, puede configurar reglas de entrada que hagan lo siguiente:

 1. Permitir tráfico HTTP de Internet
 1. Permitir todo el tráfico de entrada de la subred 10.10.20.0/24
 1. Denegar el resto del tráfico de entrada  

A continuación, configure reglas de salida que hagan lo siguiente:

1. Permitir tráfico HTTP a Internet
1. Permitir todo el tráfico de salida en la subred 10.10.20.0/24
1. Denegar el resto del tráfico de salida  

![Muestra las reglas de entrada y de salida de ejemplo](images/acl-rules.png)

## Creación de una instancia de servidor virtual

Para crear una instancia de servidor virtual en la subred recién creada:

1. Pulse **Cálculo > Instancia de servidor virtual** en el panel de navegación y pulse **Nueva instancia**.
1. Especifique un nombre para la instancia, como por ejemplo `my-instance`.
1. Seleccione la VPC que ha creado.
1. En el campo **Ubicación**, seleccione la zona en la que va a crear la instancia.
1. Seleccione una imagen (es decir, el sistema operativo y la versión), como Ubuntu Linux 16.04.
1. Para establecer el tamaño de la instancia, seleccione uno de los perfiles más utilizados o pulse **Todos los perfiles**
para elegir otra combinación de núcleo y RAM que resulte más adecuada para la carga de trabajo.
1. Seleccione una clave SSH existente o añada una nueva clave SSH que se utilizará para acceder a la instancia de servidor virtual. Para añadir una clave SSH, pulse **Nueva clave** y asigne un nombre a la clave. Después de especificar el valor de clave pública generado anteriormente, pulse **Añadir clave SSH**.

Las claves sólo se pueden añadir inicialmente como parte de la creación de la VSI. No existe ninguna herramienta para añadir claves posteriormente.
{:tip}

1. _Opcional:_ especifique los datos de usuario para ejecutar tareas de configuración comunes cuando se inicie la instancia. Por ejemplo, puede especificar directivas cloud-init o scripts shell para imágenes Linux. Para obtener más información, consulte [Datos de usuario](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-user-data).
1. Anote el volumen de arranque. En el release actual, se asignan 100 GB para el volumen de arranque. *Supresión automática* está habilitado para el volumen; se suprimirá automáticamente si se suprime la instancia.
1. En el área **Volumen de almacenamiento en bloques conectado**, puede pulsar **Nuevo volumen de almacenamiento en bloqu** para conectar un volumen de almacenamiento en bloques a la instancia. En esta guía de aprendizaje, crearemos un volumen de almacenamiento en bloques y lo conectaremos a la instancia más adelante.
1. En el área **Interfaces de red**, puede editar la interfaz de red y cambiarle el nombre. Si tiene más de una subred en la zona seleccionada y en la VPC, puede adjuntar otra subred a la interfaz. Si desea que la instancia exista en varias subredes, puede crear más interfaces.

   También puede seleccionar qué grupos de seguridad se adjuntan a cada interfaz. De forma predeterminada, se adjunta el grupo de seguridad predeterminado de VPC. El grupo de seguridad predeterminado permite el tráfico SSH y ping de entrada, todo el tráfico de salida y todo el tráfico entre instancias del grupo. El resto del tráfico se bloquea; puede configurar reglas para permitir más tráfico. Si posteriormente edita las reglas del grupo de seguridad predeterminado, estas reglas actualizadas se aplicarán a todas las instancias actuales y futuras del grupo.

1. Pulse **Crear instancia de servidor virtual**. El estado de la instancia se inicia como *Pendiente*, cambia a *Detenido* y luego a *Encendido*. Es posible que tenga que renovar la página para ver el cambio de estado.

## Creación y conexión de un volumen de almacenamiento en bloques

Puede crear un volumen de almacenamiento en bloques y adjuntarlo a la instancia de servidor virtual.

Para crear y adjuntar un volumen de almacenamiento en bloques:

1. En el panel de navegación, pulse **Almacenamiento > Volúmenes de almacenamiento en bloques**.
1. En la página Volúmenes de almacenamiento en bloques para VPC, pulse **Nuevo volumen** y especifique la siguiente información.
  * **Nombre**: especifique un nombre para el volumen de almacenamiento en bloques, como por ejemplo `data-volume-1`.  
  * **Grupo de recursos**: seleccione un grupo de recursos para el volumen de almacenamiento en bloques. Los grupos de recursos le permiten organizar los recursos de la cuenta para facilitar el control de accesos y la facturación. Para obtener más información, consulte [Prácticas recomendadas para organizar los recursos en un grupo de recursos](/docs/resources?topic=resources-bp_resourcegroups).
  * **Etiquetas**: _Opcional:_ especifique etiquetas que le ayuden a organizar y a encontrar recursos. Puede añadir más etiquetas posteriormente. Para obtener más información, consulte [Cómo trabajar con etiquetas](/docs/resources?topic=resources-tag).
  * **Ubicación**: seleccione una ubicación para el volumen de almacenamiento en bloques. La ubicación se compone de una región y una zona, por ejemplo, US South 1.
  * **Tamaño**: especifique el tamaño del volumen, entre 10 y 2000 GB.
  * **IOPs**: seleccione uno de los Niveles de IOPs o pulse Personalizado para especificar un valor de IOPs basado en el tamaño del volumen.
  * **Cifrado**: Acepte el cifrado Gestionado por el proveedor predeterminado o seleccione Gestionado por el cliente y utilice su propia clave de cifrado. Este paso requiere suministrar una instancia de servicio y crear o importar una clave raíz. Para obtener más información, consulte [Creación de volúmenes de almacenamiento en bloques con cifrado gestionado por el cliente](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-encryption#data-vol-encryption-ui).
1. Pulse **Crear volumen**.
1. En la lista de volúmenes de almacenamiento en bloques, busque el volumen que ha creado. Cuando el estado sea Available, haga clic en "...". y seleccione **Conectar a instancia**.
1. Seleccione la instancia a la que desea conectar el volumen y haga clic en **Conectar**.

## Configuración del grupo de seguridad para la instancia

Puede configurar el grupo de seguridad para definir el tráfico de entrada y de salida que se permite para la instancia. Por ejemplo, después de configurar las reglas de ACL para la subred en función de las políticas de seguridad de la empresa, puede restringir aún más el tráfico para instancias específicas en función de sus cargas de trabajo.

Para configurar el grupo de seguridad:

1. En la página Instancias de servidor virtual, pulse la instancia para ver sus detalles.
1. En la sección **Interfaces de red**, pulse el grupo de seguridad.
1. Pulse **Añadir regla** para configurar las reglas de entrada y salida que definen el tipo de tráfico de entrada y salida que se permite se permite en la instancia. Para cada regla, especifique la información siguiente:  
   * Especifique un bloque CIDR o una dirección IP para el tráfico permitido. De forma alternativa, puede especificar un grupo de seguridad en la misma VPC para permitir el tráfico de entradas o de salida de todas las instancias conectadas al grupo de seguridad seleccionado.   
   * Seleccione los protocolos y puertos a los que se aplica la regla.    

   **Consejos:**  
  * Se evalúan todas las reglas, independientemente del orden en el que se añadan.
  * Las reglas son con estado, lo que significa que el tráfico de retorno en respuesta al tráfico permitido se autoriza automáticamente. Por ejemplo, una regla que permite tráfico TCP de entrada en el puerto 80 también permite responder al tráfico TCP de salida en el puerto 80 al host de origen, sin la necesidad de usar reglas adicionales.
1. _Opcional:_ si desea adjuntar este grupo de seguridad a otras instancias, pulse **Interfaces conectadas** en el panel de navegación y seleccione interfaces adicionales.
1. Cuando haya terminado de crear reglas, pulse el rastro de navegación **Todos los grupos de seguridad** en la parte superior de la página.

### Ejemplo de grupo de seguridad  

Por ejemplo, puede configurar reglas de entrada que hagan lo siguiente:

 * Permitir todo el tráfico SSH (puerto TCP 22)
 * Permitir todo el tráfico de ping (puerto ICMP 8)

A continuación, configure las reglas de salida que permitan todo el tráfico TCP.

![Muestra las reglas de entrada y de salida de ejemplo](images/sg-example-ui.png)

## Reserva de una dirección IP flotante

Reserve y asocie una dirección IP flotante para que se pueda acceder a su instancia desde Internet.  

La instancia debe estar en ejecución para poder asociar una dirección IP flotante. La instancia puede tardar unos minutos en estar en funcionamiento.
{: tip}

Para reservar y asociar una dirección IP flotante:

1. En la página Instancias de servidor virtual, pulse la instancia para ver sus detalles.
1. En la sección **Interfaces de red**, pulse **Reservar** para la interfaz que desea asociar a una dirección IP flotante.

Si más adelante desea volver a asignar esta dirección IP flotante a otra instancia de la misma zona, busque la dirección IP flotante en la página **Red > IP flotantes**, pulse el menú de desbordamiento (**...**) y pulse **No asociar**. A continuación, pulse **Asociar** para seleccionar la instancia y la interfaz de red que desea asociar a la dirección IP flotante.
{: tip}

## Conexión con la instancia
Utilización de la dirección IP flotante que ha creado, ejecute ping sobre la instancia para asegurarse de que está en funcionamiento:

```
ping <public-ip-address>
```
{:pre}


### Conectar con imágenes de Linux

Puesto que ha creado la instancia con una clave SSH pública, ahora puede conectarse directamente con la misma utilizando la clave privada:


```
ssh -i <path-to-private-key-file> root@<public-ip-address>
```
{:pre}

Consulte [Conexión con la instancia mediante Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance)
para obtener más información sobre cómo conectarse a su instancia.

### Conectar con imágenes de Windows
Para conectarse a una imagen de Windows, inicie la sesión utilizando su contraseña descifrada. Para obtener la contraseña descifrada, copie la contraseña cifrada de la página de detalles de la instancia y ejecute el siguiente mandato:

```
# Decode the encrypted password
cat ~/examplepwd | base64 --decode > ~/examplepwd64
# Decrypt the decoded password using the RSA private key
openssl pkeyutl -in ~/examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
-pkeyopt rsa_mgf1_md:sha256
```
{:codeblock}


Consulte [Conexión a la instancia de Windows](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-windows-instance) para obtener más información sobre cómo conectarse a la instancia.



## Supervisión de la instancia

Para ver un registro de actividad que muestra cuándo se ha iniciado, detenido o reiniciado la instancia, pulse **Actividad** en el panel de navegación.

## Creación de un equilibrador de carga
Puede crear un equilibrador de carga para distribuir el tráfico de entrada entre varias instancias.

Para crear un equilibrador de carga:
1. En el panel de navegación, pulse **Red > Equilibradores de carga**.
1. En la página Equilibradores de carga, pulse **Nuevo equilibrador de carga** y especifique la siguiente información.
    * **Nombre**: especifique un nombre para el equilibrador de carga, como por ejemplo `my-load-balancer`.
    * **Nube privada virtual**: seleccione la VPC.
    * **Grupo de recursos**: seleccione un grupo de recursos para el equilibrador de carga.
    * **Tipo**: seleccione el tipo de equilibrador de carga: Público o Privado.
    * **Región**: indica la región en la que se va a crear el equilibrador de carga; es decir, la región seleccionada para la VPC.
    * **Subredes**: seleccione las subredes en las que desea crear el equilibrador de carga. Para maximizar la disponibilidad de la aplicación, seleccione subredes de diferentes zonas.
1. Pulse **Nueva agrupación** y especifique la información siguiente para crear una agrupación de programas de fondo. Puede crear una o varias agrupaciones.
    * **Nombre**: especifique un nombre para la agrupación, como por ejemplo `my-pool`.
    * **Protocolo**: seleccione el protocolo para las instancias de programa de fondo que hay tras el equilibrador de carga. El protocolo de la agrupación debe coincidir con el protocolo del escucha asociado. Por ejemplo, si se selecciona un protocolo HTTPS o HTTP para el escucha, el protocolo de la agrupación debe ser HTTP. Paralelamente, si el protocolo de escucha es TCP, el protocolo de la agrupación de programas de fondo debe ser TCP.
    * **Método**: seleccione cómo desea que el equilibrador de carga distribuya el tráfico entre las instancias de fondo:
      * **Round robin:** las solicitudes se envían a cada instancia en turnos. Todas las instancias reciben aproximadamente un mismo número de conexiones de cliente.
      * **Round robin ponderado:** las solicitudes se envían a cada instancia en proporción a su ponderación asignada. Por ejemplo, si tiene las instancias A, B y C, y sus ponderaciones se han establecido en 60, 60 y 30, las instancias A y B reciben el mismo igual de conexiones y la instancia C recibe la mitad de conexiones.
      * **Conexiones mínimas:** las solicitudes se reenvían a la instancia que en ese momento tiene el menor número de conexiones.  
    * **Retención de sesiones**: seleccione si todas las solicitudes durante una sesión de usuario se envían a la misma instancia.  
    * **Comprobaciones de estado**: configure el modo en que el equilibrador de carga comprueba el estado de las instancias.
      * **Protocolo de estado**: el protocolo que utiliza el equilibrador de carga para enviar mensajes de comprobación de estado a las instancias de programa de fondo.
      * **Vía de acceso de estado**: la vía de acceso de estado solo se aplica si se selecciona HTTP como protocolo de comprobación de estado. La vía de acceso de estado especifica el URL que utiliza el equilibrador de carga para enviar las solicitudes de comprobación de estado HTTP a las instancias de programa de fondo. De forma predeterminada, las comprobaciones de estado se envían a la vía de acceso raíz (`/`).
      * **Intervalo**: el intervalo en segundos entre dos intentos consecutivos de comprobación de estado. De forma predeterminada, se envían comprobaciones de estado cada cinco segundos.
      * **Tiempo de espera**: el intervalo máximo de tiempo que el sistema espera una respuesta a la solicitud de comprobación de estado. De forma predeterminada, el equilibrador de carga espera dos segundos para obtener una respuesta.
      * **Número máximo de reintentos**: número máximo de intentos de comprobación de estado adicionales que realiza el equilibrador de carga antes de declarar que una instancia de programa de fondo no está en buen estado. De forma predeterminada, se deja de considerar que una instancia está en buen estado después de dos comprobaciones de estado fallidas.

        Aunque el equilibrador de carga deja de enviar conexiones a las instancias que no están en buen estado, el equilibrador de carga sigue supervisando el estado de estas instancias y reanuda su uso si se detecta que vuelven a estar en buen estado después de pasar correctamente dos intentos de comprobación de estado consecutivos.

      Si las instancias de programa de fondo no están en buen estado y cree que la aplicación se está ejecutando correctamente, vuelva a comprobar los valores de protocolo de estado y de vía de acceso de estado. Compruebe también los grupos de seguridad conectados a las instancias para asegurarse de que las reglas permiten el tráfico entre el equilibrador de carga y las instancias.
      {: tip}

1. Pulse **Crear**.
1. Junto a la entrada correspondiente a la nueva agrupación, pulse **Conectar** en la columna **Instancias** para añadir una instancia a la agrupación. Pulse **Añadir** para añadir más instancias a la agrupación. Especifique la información siguiente para cada instancia:
   * Seleccione una o varias subredes desde las que seleccionar una instancia.
   * Seleccione una instancia. Si una instancia tiene varias interfaces, asegúrese de seleccionar la dirección IP correcta.
   * Especifique el puerto en el que se envía el tráfico a la instancia.
   * Si la agrupación utiliza el método **Round robin ponderado**, asigne una ponderación a cada instancia.  

      Si se asigna la ponderación '0' a una instancia, significa que no se reenviarán nuevas conexiones a dicha instancia, pero el tráfico existente seguirá fluyendo mientras la conexión actual esté activa. El hecho de utilizar la ponderación '0' puede ayudar a desactivar fácilmente una instancia y a eliminarla de la rotación del servicio.
      {: tip}

1. Pulse **Nuevo escucha** y especifique la información siguiente para crear un escucha. Puede crear uno o varios escuchas.
   * **Protocolo**: el protocolo que se debe utilizar para recibir las solicitudes entrantes.
   * **Puerto**: el puerto de escucha en el que se reciben las solicitudes. El rango de puertos 56500 a 56520 está reservado para fines de gestión y no se puede utilizar.
   * **Agrupación de fondo**: la agrupación de fondo a la que este escucha reenvía el tráfico.
   * **Número máximo de conexiones** (opcional): número máximo de conexiones simultáneas que permite el escucha.
   * **Certificado SSL**: si el protocolo seleccionado para este escucha es HTTPS, debe seleccionar un certificado SSL. Asegúrese de que el equilibrador de carga tiene autorización para acceder al certificado SSL. Para ver instrucciones, consulte
[Antes de empezar](#before).
1. Pulse **Crear**.
1. Cuando termine de crear agrupaciones y escuchas, pulse **Crear equilibrador de carga**.
1. Para ver los detalles de un equilibrador de carga existente, pulse el nombre del equilibrador de carga en la página **Equilibradores de carga**.

## Creación de una VPN
Puede crear una red privada virtual (VPN) para que la VPC pueda conectarse de forma segura a otra red privada, como por ejemplo una red local u otra VPC.

Para ver un ejemplo de código, consulte [Utilización de VPN con la VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
{: tip}

Para crear una VPN:
1. En el panel de navegación, pulse **Red VPN > VPN**.
1. En la página VPN, pulse **Nueva pasarela VPN** y especifique la información siguiente:
    * **Nombre**: especifique un nombre para la pasarela VPN en la nube privada virtual, como por ejemplo `my-vpn-gateway`.
    * **Nube privada virtual**: seleccione la VPC.
    * **Grupo de recursos**: seleccione un grupo de recursos para la VPN.
    * **Subred**: seleccione la subred en la que se creará la pasarela VPN.
1. En la sección **Nueva conexión VPN**, defina una conexión entre esta pasarela y una red externa a la VPC especificando la información siguiente.
    * **Nombre de conexión**: escriba un nombre para la conexión, como por ejemplo `my-connection`.
    * **Dirección de pasarela igual**: especifique la dirección IP de la pasarela VPN para la red externa a la VPC.
    * **Clave precompartida**: especifique la clave de autenticación de la pasarela VPN para la red externa a la VPC.
    * **Subredes locales**: especifique una o varias subredes de la VPC con las que desea conectar a través del túnel VPN.
    * **Subredes iguales**: especifique una o varias subredes de la otra red con las que desea conectar a través del túnel VPN.
1. Para configurar la forma en que la pasarela de la nube envía mensajes para comprobar que la pasarela igual está activa, especifique la información siguiente en la sección **Detección de igual inactivo**.
    * **Acción de detección de igual inactivo**: la acción que se debe llevar a cabo si una pasarela igual deja de responder. Por ejemplo, seleccione **Reiniciar** si desea que la pasarela renegocie inmediatamente la conexión.
    * **Intervalo**: la frecuencia con la que se comprueba que la pasarela igual está activa. De forma predeterminada, se envían mensajes cada 30 segundos.
    * **Tiempo de espera excedido**: el tiempo que se debe esperar una respuesta de la pasarela igual. De forma predeterminada, una pasarela igual se deja de considerar activa si no se recibe una respuesta en un plazo de 150 segundos.
1. Especifique los parámetros de seguridad de IKE e IPsec para la conexión.
    * Seleccione **Automático** si desea que la pasarela de nube intente establecer automáticamente la conexión.
    * Seleccione o cree políticas personalizadas si tiene que imponer requisitos de seguridad específicos o si la pasarela VPN de la otra red no da soporte a las propuestas de seguridad que se intentan mediante la negociación automática.

  **Importante**: los parámetros de seguridad de IKE e IPsec que especifique para la conexión deben coincidir con los parámetros que se han establecido en la pasarela para la red externa a la VPC.

## ¡Enhorabuena!

Ha creado y configurado correctamente una VPC y una subred, una ACL, una instancia de servidor virtual, un volumen de almacenamiento en bloques, un grupo de seguridad, una dirección IP flotante, un equilibrador de carga y una VPN. Puede seguir desarrollando la VPC añadiendo más instancias, subredes y otros recursos.
