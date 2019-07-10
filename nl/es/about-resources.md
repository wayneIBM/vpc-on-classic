---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-06-04"

keywords: resource, policies, authorization, resource type, resource groups, roles, load balancer, VPN, operator, editor, viewer, admin

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
{:DomainName: data-hd-keyref="DomainName"}

# Acerca de los recursos de la infraestructura de VPC
{: #about-vpc-infrastructure-resources}

El término _recursos_ hace referencia a las partes de componente de un despliegue de nube privada virtual de {{site.data.keyword.cloud}}. Cada VPC puede contener recursos de los tipos siguientes, que se pueden agrupar según sea necesario:

* **vpc**
* **instancia**
* **clave**
* **imagen**
* **subred**
* **volumen**
* **listas de control de acceso de red (acl)**
* **grupo de seguridad**
* **IP flotante**
* **vnic**
* **pasarela**
* **equilibrador de carga**
* **vpn**

Algunos recursos de la infraestructura de VPC heredan sus políticas de autorización, que rigen su uso, de la VPC padre, pero otros se gestionan por separado.

Actualmente, los tipos de recurso `instance`, `key`, `security-group`, `volume`, `loadbalancer` y `vpn` mantienen sus propias políticas de autorización. Más adelante en este documento encontrará información más detallada sobre las autorizaciones de recurso para estos recursos.
{: note}

## Políticas de recursos
{: #resource-policies}

Por lo general, el acceso a los recursos de una VPC se controla mediante _políticas_. Si desea personalizar el acceso a los recursos para su VPC, puede crear políticas personalizadas. Una política de recursos aplicarse a:

* todos los recursos
* todos los recursos de un tipo
* todos los recursos de un grupo
* un recurso individual

## Recursos y grupos de recursos
{: #resources-and-resource-groups}

Un _grupo de recursos_ es una colección de recursos, por ejemplo una VPC entera, o una única subred y su ACL, que están asociadas con el fin de definir la autorización y el uso. Podría considerar que un grupo de recursos es una colección de la infraestructura que puede utilizar un proyecto, un departamento o un equipo.

Puede utilizar la CLI de búsqueda para visualizar las etiquetas conectadas a un recurso. Con los filtros adecuados, sólo puede visualizar sólo las instancias que le importan, como por ejemplo `ibmcloud resource search name:` o `ibmcloud resource search 'service_name:is AND type:vpc'`.
{: tip}

Las grandes empresas tal vez deseen dividir una VPC en grupos de recursos. Es posible que las empresas pequeñas no necesiten dividir su VPC en grupos de recursos, ya que todo su equipo usaría toda la VPC. Si está familiarizado con _OpenStack_, el concepto de grupo de recursos se parece al concepto de _Proyecto_ de _OpenStack Keystone_.

SOLO se puede asignar un recurso a un grupo de recursos al crear la VPC. Los recursos no pueden cambiar de grupo de recursos una vez creados.
{: .note}

Los grupos de recursos se han diseñado principalmente para las organizaciones grandes. Para la mayoría de las empresas y los equipos, la fuga de recursos constituye un problema de la VPC. Esta regla general puede cambiar si se pueden asignar VSI (instancias) individuales a un grupo de recursos y usuarios individuales a un recurso de VSI (instancia).

Al configurar la VPC de IBM Cloud, si desea utilizar grupos de recursos, se recomienda haber planificado cómo se desea asignar los recursos y los usuarios de la organización a cada grupo de recursos.
{: tip}

## Especificaciones para VPC
{: #specifics-for-vpc}

Algunos recursos de la infraestructura de VPC heredan sus políticas de autorización, que rigen su uso, de la cuenta o de la VPC padre, mientras que otros tienen políticas individuales.

### Autorización de recursos de VPC por tipo de recurso
{: #vpc-resource-authorization-by-resource-type}

En la tabla se resume la forma en que se autorizan los recursos de VPC de forma predeterminada.

| Tipo de recurso | Dónde obtiene su autorización predeterminada |
|-----------|------------|
| VPC | Comprobación de autorización cuando se crea |
| Equilibrador de carga | Comprobación de autorización cuando se crea |
| VPN | Comprobación de autorización cuando se crea |
| Subred | VPC padre |
| Pasarela pública | VPC padre |
| Instancia | Comprobación de autorización cuando se crea |
| Grupo de seguridad | Comprobación de autorización cuando se crea |
| Clave | Comprobación de autorización cuando se crea |
| Imagen | Cuenta |
| IP flotante | Cuenta |
| ACL de red | Cuenta|
| Volumen | Comprobación de autorización cuando se crea |

Las IP flotantes y las ACL de red se pueden crear en el ámbito de nivel de cuenta, si no se ha asignado. Sin embargo, en cuanto se asigna una IP flotante a una instancia o se asigna una ACL a una subred, estos recursos pasan a estar sujetos al ámbito de autorización de la VPC.
{: note}

### Cobertura de roles y acciones autorizadas sobre recursos de VPC
{: #vpc-coverage-of-roles-and-authorized-actions-on-resources}

En un caso de ejemplo con 2 VPC, aquí encontrará algunos ejemplos de lo que sucede cuando cualquier usuario con determinados roles intenta realizar determinadas acciones:

* Usuario _admin_, con permiso de **Administrador** sobre todos los recursos
  * Crea `vpc1`: correcto

* Usuario _editor_, con permiso de **Editor** sobre todos los recursos
  * Crea `vpc2`: correcto
  * Actualiza `vpc2`: correcto
  * Suprime `vpc2`: correcto

* Usuario _operator_, con permiso de **Operador** sobre todos los recursos
  * Crea `vpc`: denegado
  * Actualiza `vpc1`: denegado
  * Suprime `vpc1`: denegado

* Usuario _viewer_, con permiso de **Visor** sobre todos los recursos
  * Lista VPC: consulta [vpc1]
  * Lee `vpc1`: correcto

* Usuario _noRole_, sin política
  * Lista VPC: consulta []
  * Lee `vpc1`: denegado

### Tabla de resumen de la cobertura de VPC
{: #vpc-coverage-summary-table}

Para cualquier política que otorga acceso por rol (Administrador, Editor, Operador, Visor), se otorgan las acciones siguientes (X=denegado, o=correcto)

 rol:      | ninguno | Visor | Operador | Editor | Administrador
-----------:|------|--------|-------|--------|-------
Crear      | X    | X      | X     | o      | o
Listar        | X    | o      | X     | o      | o
Leer        | X    | o      | X     | o      | o
Actualizar      | X    | X      | X     | o      | o
Suprimir      | X    | X      | X     | o      | o


## Autorización de recursos de VPN para VPC
{: #resource-authorizations-of-vpn-for-vpc}

La autorización de recursos de **VPN para VPC** se configura por separado de los otros tipos de autorización de recursos de VPC, pero de manera similar.

Las políticas de VPN para VPC controlan el acceso a los recursos de VPN, especialmente a las pasarelas de VPN. Para acceder a los recursos hijo de una pasarela VPN, como conexiones VPN y CIDR locales o de igual, debe tener acceso de **Lectura** (o superior) a la pasarela VPN padre. Las políticas de recursos sólo se pueden utilizar con las conexiones VPN que también tienen acceso de **Lectura** o superior para la pasarela VPN padre. Una política de recursos en VPN puede aplicarse a:

* todos los recursos de VPN (pasarelas de VPN, conexiones VPN, políticas de IKE e IPsec)
* todos los recursos VPN de un grupo de recursos
* una pasarela de VPN individual

El uso de VPN también se factura por separado.
{: note}

### Cobertura de roles y acciones autorizadas sobre recursos de VPN para VPC
{: #vpn-for-vpc-coverage-of-roles-and-authorized-actions-on-resources}

Se da soporte a los mismos roles de usuario que en la autorización de recursos de VPC, pero con distintas acciones habilitadas para cada rol.

Estos son algunos ejemplos de lo que sucede cuando usuarios con determinados roles intentan realizar ciertas acciones relacionadas con VPN:

* Usuario _admin_, con permiso de **Administrador** sobre todos los recursos
  * Crea, actualiza, suprime, lee, lista pasarelas de VPN: correcto
  * Crea, actualiza, suprime, lee, lista conexiones VPN: correcto
  * Crea, actualiza, suprime, lee, lista CIDR iguales y locales de conexión de VPN: correcto
  * Crea, actualiza, suprime, lee, lista políticas de IKE: correcto
  * Crea, actualiza, suprime, lee, lista políticas de IPSec: correcto

* Usuario _editor_, con permiso de **Editor** sobre todos los recursos
  * Igual que los del administrador

* Usuario _operator_, con permiso de **Operador** sobre todos los recursos
  * Crea, actualiza, suprime pasarelas de VPN: denegado
  * Crea, actualiza, suprime conexiones VPN: denegado
  * Crea, actualiza, suprime CIDR iguales y locales de conexión de VPN: denegado
  * Crea, actualiza, suprime políticas de IKE: denegado
  * Crea, actualiza, suprime políticas de IPSec: denegado
  * Lee, lista pasarelas de VPN: correcto - ve datos reales
  * Lee, lista conexiones VPN: correcto - ve datos reales
  * Lee, lista CIDR iguales y locales de conexión de VPN: correcto - ve datos reales
  * Lee, lista políticas de IKE: correcto - ve datos reales
  * Lee, lista políticas de IPSec: correcto - ve datos reales

* Usuario _viewer_, con permiso de **Visor** sobre todos los recursos
  * Igual que los del operador

* Usuario _noRole_, sin política
  * Lee, lista pasarelas de VPN: denegado - la lista muestra datos vacíos
  * Lee, lista conexiones VPN: denegado
  * Lee, lista CIDR iguales y locales de conexión de VPN: denegado
  * Lee, lista políticas de IKE: denegado - la lista muestra datos vacíos
  * Lee, lista políticas de IPSec: denegado - la lista muestra datos vacíos

### Tabla de resumen de la cobertura de VPN para VPC
{: #vpn-for-vpc-coverage-summary-table}

Puede ver la correlación de roles de acción en VPN para VPC en la tabla siguiente (X=denegado, o=correcto):

rol:       | ninguno | Visor | Operador | Editor | Administrador
-----------:|------|--------|-------|--------|-------
Crear      | X    | X      | X     | o      | o
Listar        | X    | o      | o     | o      | o
Leer        | X    | o      | o     | o      | o
Actualizar      | X    | X      | X     | o      | o
Suprimir      | X    | X      | X     | o      | o

## Autorización de recursos de equilibrador de carga para VPC
{: #resource-authorization-for-load-balancer-for-vpc}

La autorización de recursos para del **equilibrador de carga para VPC** se configura por separado de las otras autorizaciones de recursos de la VPC, pero de manera similar.

El uso del equilibrador de carga también se factura por separado.
{: note}

### Cobertura de roles y acciones autorizadas sobre recursos del equilibrador de carga
{: #load-balancer-coverage-of-roles-and-authorized-actions-on-resources}

Estos son algunos ejemplos de lo que sucede cuando usuarios con determinados roles intentan realizar ciertas acciones relacionadas con el equilibrador de carga de VPC:

* Usuario _admin_, con permiso de **Administrador** sobre todos los recursos
  * Crea, actualiza, suprime, lee, lista equilibradores de carga: correcto
  * Crea, actualiza, suprime, lee, lista escuchas: correcto
  * Crea, actualiza, suprime, lee, lista agrupaciones: correcto
  * Crea, actualiza, suprime, lee, lista miembros: correcto
  * Crea, actualiza, suprime, lee, lista estadísticas de equilibrador de carga: correcto

* Usuario _editor_, con permiso de **Editor** sobre todos los recursos
  * Igual que los del administrador

* Usuario _operator_, con permiso de **Operador** sobre todos los recursos
  * Crea, actualiza, suprime, lee, lista equilibradores de carga: denegado
  * Crea, actualiza, suprime, lee, lista escuchas: denegado
  * Crea, actualiza, suprime, lee, lista agrupaciones: denegado
  * Crea, actualiza, suprime, lee, lista miembros: denegado
  * Crea, actualiza, suprime, lee, lista estadísticas de equilibrador de carga: denegado

* Usuario _viewer_, con permiso de **Visor** sobre todos los recursos
  * Lee, lista equilibradores de carga: correcto
  * Lee, lista escuchas: correcto
  * Lee, lista agrupaciones: correcto
  * Lee, lista miembros: correcto
  * Lee estadísticas de equilibrador de carga: correcto

* Usuario _noRole_, sin política
  * Lee, lista equilibradores de carga: denegado - la lista muestra datos vacíos
  * Lee, lista escuchas: denegado
  * Lee, lista agrupaciones: denegado
  * Lee, lista miembros: denegado
  * Lee estadísticas de equilibrador de carga: denegado

### Tabla de resumen de la cobertura del equilibrador de carga
{: #load-balancer-coverage-summary-table}

Puede ver la correlación de roles de acción del equilibrador de carga para VPC en la tabla siguiente (X=denegado, o=correcto):

rol:       | ninguno | Visor | Operador | Editor | Administrador
-----------:|------|--------|-------|--------|-------
Crear      | X    | X      | X     | o      | o
Listar        | X    | o      | X     | o      | o
Leer        | X    | o      | X     | o      | o
Actualizar      | X    | X      | X     | o      | o
Suprimir      | X    | X      | X     | o      | o


## Autorización de recursos de Servidor virtual para VPC
{: #planning-virtual-servers-for-vpc-permissions}

Configure la autorización de recursos para **Servidor Virtual para VPC** aparte de otras autorizaciones de recursos de la VPC. 

El uso de servidores virtuales para VPC se factura por separado.
{: note}

* Como administrador, puede definir roles y llevar a cabo las acciones disponibles en {{site.data.keyword.vsi_is_short}}.
* Como editor, puede modificar el estado y crear o suprimir subrecursos.
* Como operador, puede realizar acciones que no cambien el estado de los recursos.
* Como visor, puede realizar acciones que no cambien el estado de los recursos.

### Tabla de resumen de la cobertura de servidor virtual
{: #vsi-coverage-summary-table}

Puede ver la correlación de roles de acción del servidor virtual para VPC en la tabla siguiente (X=denegado, o=correcto):

rol:       | ninguno | Visor | Operador | Editor | Administrador
-----------:|------|--------|-------|--------|-------
Crear      | X    | X      | X     | o      | o
Listar        | X    | o      | o     | o      | o
Leer        | X    | o      | o     | o      | o
Actualizar      | X    | X      | X     | o      | o
Suprimir      | X    | X      | X     | o      | o

Cuando crea una instancia, debe tener también acceso de Operador a los recursos de la VPC y del grupo de seguridad, si están especificados. Los recursos de subred y de IP flotante heredan los permisos de la VPC asociada.  
{: tip} 
