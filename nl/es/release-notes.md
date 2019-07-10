---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: release notes, changes, updates, vpc, profile, hyper protect, estimator, load balancer

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

# Notas del release
{: #release-notes}

Utilice estas notas de release para obtener información sobre los últimos cambios en {{site.data.keyword.cloud}} Virtual Private Cloud.
{:shortdesc}

## 14 de junio de 2019
{: #june-14-2019}

**Actualizaciones en la interfaz de usuario de IBM Cloud VPC**

- **Claves SSH y grupos de recursos.**
    * Los grupos de recursos se muestran en la vista de lista de claves SSH.
    * Los grupos de recursos se pueden seleccionar en el modal de suministro de claves SSH.
- **Perfiles y ancho de banda.** El límite de ancho de banda asignado a cada perfil se muestra en las páginas de visualización **Perfiles populares** y **Todos los perfiles**.
- **Grupos de recursos.** La página de suministro de VSI muestra la lista desplegable de grupos de recursos.

**Actualizaciones para Block Storage for VPC**
- La **longitud de nombre de volumen** se ha aumentado a 63 caracteres alfanuméricos.
- Se han renombrado los siguientes **códigos de error de volumen** y se han actualizado o suprimido los mensajes:
    * `volume_encryption_key_account_id_mismatch` se ha renombrado a `volume_crn_account_id_mismatch`
    * `volume_encryption_key_cname_mismatch` se ha renombrado a `volume_crn_cname_mismatch`
    * `volume_encryption_key_region_not_found` se ha eliminado

**Actualizaciones en SDK**

- **Proveedor de Terraform v0.17.1** se ha publicado. Descargue el [binario más reciente](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v0.17.1).
- **Imagen de Docker** también se ha actualizado con el proveedor de Terraform más reciente. Puede obtener la imagen de Docker más reciente utilizando este mandato:  `docker pull ibmterraform/terraform-provider-ibm-docker:latest`
- Recuerde establecer `generation` como argumento de proveedor o exportar `IC_GENERATION= 1` para que el código funcione con la versión publicada actualmente de VPC en la infraestructura clásica. De forma predeterminada, el valor se establece en 2 (VPC, próximamente, ahora en Beta).
- Los argumentos de proveedor (`bluemix_api_key` y `bluemix_timeout`) han quedado en desuso, por lo que puede ser que se emita un aviso si ejecuta el plan Terraform o lo solicita.

## 7 de junio de 2019
{: #june-07-2019}

- **IBM Cloud VPC está ahora disponible para el público en general.** Todas las [cuentas de Pago según uso](/docs/account?topic=account-accounts) pueden suministrar recursos de VPC.
Inicie sesión en la [Consola de IBM Cloud](https://{DomainName}/vpc/overview) y empiece hoy mismo.

- **Ahora se pueden establecer políticas de autorización individuales en instancias, claves, volúmenes y grupos de seguridad.** Conozca los roles necesarios para gestionar estos recursos en [Autorizaciones de recursos necesarias para llamadas de API y CLI](/docs/vpc-on-classic?topic=vpc-on-classic-resource-authorizations-required-for-api-and-cli-calls). Los usuarios de acceso previo no se ven afectados por las políticas existentes.

- **El parámetro de velocidad de puerto de una interfaz de red virtual se ha eliminado de la interfaz de usuario, la CLI y la API.**

- **Ahora dispone de soporte para varias regiones para métricas de supervisión en la interfaz de usuario.**


## 31 de mayo de 2019
{: #may-31-2019}

**Nueva versión de API disponible.** La versión de la API de VPC on Classic se ha actualizado de `2019-01-01` a `2019-05-31`. Para ver las directrices y las mejores prácticas, consulte [Control de versiones](https://{DomainName}/apidocs/vpc-on-classic#versioning) en la API de Virtual Private Cloud on Classic.

## 24 de mayo de 2019
{: #may-24-2019}

- **Las instancias de equilibrador de carga en estado 'deleting' ahora aparecen en la vista de lista de la interfaz de usuario.** Cuando se solicita suprimir un equilibrador de carga en la interfaz de usuario, la vista de lista continúa mostrando el equilibrador de carga hasta que se completa el proceso de supresión.

- **Las características de equilibrador de carga de Capa 7 están disponibles en la interfaz de usuario.** Ahor apuede configurar las características de equilibrador de carga de Capa 7 en la interfaz de usuario.

- **Las políticas de visualización y edición de equilibrador de carga están ahora disponibles en la interfaz de usuario.** Hay una nueva función para ver y editar las políticas del equilibrador de carga disponible en la interfaz de usuario.

Para obtener más información, consulte la [Documentación del equilibrador de carga](/docs/infrastructure/vpc-on-classic-network?topic=vpc-on-classic-network---using-load-balancers-in-ibm-cloud-vpc).


## 10 de mayo de 2019
{: #may-10-2019}


- **Los nombres de perfil han cambiado**. Los nombres de perfil de instancia han cambiado. Los mandatos para suministrar instancias deben actualizarse para utilizar los nuevos nombres de perfil. Puede obtener más información [sobre perfiles aquí](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-profiles).

- **Precio mensual estimado de IP flotante está ahora disponible en la interfaz de usuario**. Cuando reserva una dirección IP flotante, ahora puede ver una línea que indica el precio mensual estimado de esa IP flotante.

- **El soporte de Hyper Protect está disponible**.

- **El nombre de subred y la zona aparecen en forma de enlaces que le llevan a la subred asociada en la interfaz de usuario**. Las subredes ahora aparecen listadas por nombre en lugar de por ID de subred y el nombre actúa como un enlace a la página de detalles de subred de la subred asociada.

- **El estimador de resumen de costes está disponible en la interfaz de usuario**.

- **El sondeo a las agrupaciones de equilibrador de carga y a los escuchas se refleja en la interfaz de usuario**.

    * En la página Agrupaciones de equilibrador de carga, ahora verá un contador que muestra la última actualización, debajo de la tabla de recursos.
    * El intervalo de sondeo predeterminado es de 30 segundos.
    * Se pueden visualizar cuatro (4) estados diferentes: `newProvision`, `active`, `failed` y `all other states`.
        * Para un nuevo suministro: el intervalo es de 15 segundos. Las agrupaciones y los escuchas se ponen directamente en estado 'active'.
        * Para 'active': la lista de recursos y la cabecera se actualizan cada 60 segundos.
        * Para 'failed': se detiene el sondeo.
        * Para 'all other states': el sondeo continúa en intervalos de 30 segundos.
    * Si suprime un escucha, puede desencadenar el estado de la cabecera para que muestre `Updating (Actions Unavailable)`.
    * Tenga en cuenta también que la información de cabecera se actualiza cuando se produce una actualización.
    * Estos mismos cambios se aplican a los sondeos para los escuchas.

- **En la página de miembros y de detalles de la interfaz de usuario se muestra el número de miembros de equilibrador de carga, mientras se está realizando el sondeo a los miembros**.

    * En la sección Estado, verá el número de miembros, mientras que "Captando detalles de instancia" aparece al lado.
    * En la columna Instancias, en la tabla de recursos, verá el número de miembros, mientras que "Captando detalles de instancia" aparece al lado.
    * Una vez cargado, si tiene miembros no válidos, verá el icono de precaución amarillo.

- **La página Detalles de VPC de la interfaz de usuario muestra todas las subredes conectadas**.
