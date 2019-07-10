---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: permissions, infrastructure, VPC, SSH, CLI, API, console, classic

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

# Guía de aprendizaje de iniciación
{: #getting-started}
[comment]: # (tema de ayuda enlazado)


Para empezar a trabajar con {{site.data.keyword.cloud}} Virtual Private Cloud:

1. Cree una nube privada virtual.
2. Cree una o varias subredes en la nube privada virtual en una o varias zonas.
3. Cree una pasarela pública (PGW) en una subred si desea que los recursos de la subred tengan acceso a Internet o viceversa.
4. Seleccione los perfiles de las instancias de servidor virtual (VSI) que desee ejecutar y cree una instancia de los mismos.
5. Reserve una dirección IP flotante y asóciela con una instancia de servidor virtual si desea acceder a ella desde Internet.
5. Despliegue el servicio o las aplicaciones en las instancias de servidor virtual.

## Requisitos previos

 * **Permisos de usuario**: Asegúrese de que el usuario tiene permisos suficientes para crear y gestionar recursos en la VPC. Para obtener una lista de los permisos necesarios, consulte [Cómo otorgar los permisos necesarios para usuarios de VPC](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

 * **Tenga la clave SSH preparada**: Utilizará una clave SSH para conectarse a las instancias del servidor virtual.

   * Busque un archivo denominado `id_rsa.pub` en un directorio `.ssh` del directorio de inicio; por ejemplo, `/Users/<USERNAME>/.ssh/id_rsa.pub`. El archivo empieza por `ssh-rsa` y termina con su dirección de correo electrónico.

   * O genere una clave SSH: Si no tiene una clave SSH pública o si ha olvidado la contraseña de una clave existente, genere una nueva ejecutando el mandato `ssh-keygen` y siguiendo la solicitud siguiente. Por ejemplo, puede generar una clave SSH en el servidor Linux ejecutando el mandato `ssh-keygen -t rsa -C "ID_usuario"`. Este mandato genera dos archivos. La clave pública generada se encuentra en el archivo `<your key>.pub`.

## Utilice la IU, la CLI o la API REST

Puede suministrar y gestionar todos sus recursos de VPC mediante la IU, la CLI o la API REST.

Si es un nuevo usuario de IBM Cloud Virtual Private Cloud, elija cualquiera de los enlaces siguientes, que le guiarán por el proceso de creación de su IBM Cloud VPC y sus recursos, de principio a fin. Puede elegir si desea comenzar desde la interfaz de usuario de la consola, desde la línea de mandatos (CLI) o desde las API REST.

* Para acceder a través de la interfaz de usuario, inicie una sesión en la [consola de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")]( https://{DomainName}/vpc){: new_window}. Para obtener más información, consulte [Creación de una VPC utilizando la consola de IBM Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-console).
* Para utilizar la interfaz de línea de mandatos, siga el ejemplo [Hello World](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-ibm-cloud-cli).
* Los usuarios más avanzados pueden llamar directamente a las [API REST](https://{DomainName}/apidocs/vpc-on-classic). Siga la guía de aprendizaje del [código de ejemplo](/docs/vpc-on-classic?topic=vpc-on-classic-creating-a-vpc-using-the-rest-apis) para empezar a utilizar las API REST.

## Siguientes pasos
Si está preparado para empezar a trabajar, vaya directamente a la documentación detallada sobre **Redes para VPC**, **VSI para VPC** y **Block Storage for VPC**:

* [**Redes para Virtual Private Cloud**](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-getting-started)
* [**Servidores virtuales para Virtual Private Cloud**](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-getting-started)
* [**Block Storage for Virtual Private Cloud**](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-getting-started)
