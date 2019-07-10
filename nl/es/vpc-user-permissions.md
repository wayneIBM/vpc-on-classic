---

copyright:
  years: 2017, 2018, 2019

lastupdated: "2019-05-17"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

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

# Gestión de permisos de usuario para recursos de VPC
{: #managing-user-permissions-for-vpc-resources}

La nube privada virtual de {{site.data.keyword.cloud}} utiliza un control de acceso basado en roles que permite a los administradores de cuentas controlar el acceso de sus usuarios a los recursos de la VPC.

Para obtener más información sobre cómo IBM Cloud VPC utiliza el control de acceso basado en roles y sobre cómo VPC utiliza el rol de IAM y las políticas de acceso, consulte [Asignación de acceso basado en roles a recursos de VPC](/docs/vpc-on-classic?topic=vpc-on-classic-assigning-role-based-access-to-vpc-resources).

En este documento se muestra cómo el administrador de la cuenta puede añadir usuarios adicionales a la cuenta y otorgarles los permisos correctos para gestionar los [recursos de la infraestructura de VPC](/docs/vpc-on-classic?topic=vpc-on-classic-about-vpc-infrastructure-resources). Abarca dos casos de ejemplo comunes para un administrador de VPC:

* **Caso de ejemplo de acceso simple:** muestra cómo asignar a un usuario una política de acceso para que pueda crear y utilizar recursos de infraestructura de VPC (incluyendo nubes privadas virtuales).

* **Caso de ejemplo de acceso por equipo:** muestra cómo configurar grupos de recursos y políticas de acceso que permitan que dos equipos creen y utilicen los recursos de VPC que tienen asignados.

Los cambios en las políticas de acceso de IAM para VPC pueden tardar hasta 10 minutos en entrar en vigor.
{: note}

## Caso de ejemplo de acceso simple
{: #simple-access-scenario}

Este caso de ejemplo cubre los pasos básicos necesarios para configurar un usuario individual. Se describen dos casos: invitación a un nuevo usuario a la cuenta y modificación de los permisos de un usuario existente.

### Invitación a un nuevo usuario a crear o gestionar recursos de VPC
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

Invite a un usuario de IBM Cloud a su cuenta y otórguele acceso a la `Infraestructura de VPC` para que pueda tener acceso para ver, crear y actualizar recursos de VPC. En esta sección se proporciona una breve visión general de los pasos de IAM. Encontrará más en los enlaces de la sección **Enlaces relacionados** hacia el final de este documento.

Estos son los pasos básicos de IAM necesarios para invitar a los usuarios a los servicios y recursos de VPC:

1. Vaya a la [interfaz de usuario de usuarios de IAM ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam#/users){: new_window} en la consola de IBM Cloud.
2. En la página **Usuarios**, pulse **Invitar a usuarios**.
3. En la página **Invitar a usuarios**, en la sección **Usuarios**, especifique las direcciones de correo electrónico de los usuarios que desea invitar en el campo **Dirección de correo electrónico**.
4. En la sección **Acceso**, expanda **Servicios** y, a continuación, complete las tareas siguientes:
  * Seleccione **Recurso** en la lista **Asignar acceso a**.
  * Seleccione **Infraestructura de VPC** en la lista **Servicios**.
  * Seleccione el rol de acceso de la plataforma que desea asignar a los usuarios. Puede ser **Administrador**, **Editor**, **Operador** o **Visor**.
  * Pulse **Invitar a usuarios**.

### Cómo otorgar a un usuario existente permiso para gestionar recursos de VPC
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

En este caso de ejemplo se muestran los pasos básicos necesarios para otorgar a un usuario existente en su cuenta permiso para editar recursos de VPC.

En los pasos siguientes, creará dos políticas de IAM. Se necesitan ambas políticas para que un usuario pueda crear y utilizar recursos de infraestructura de VPC. Todos los recursos deben residir dentro del grupo de recursos predeterminado de la cuenta.

1. Vaya a la [interfaz de usuario de usuarios de IAM ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam#/users){: new_window} en la consola de IBM Cloud.
2. Seleccione el usuario cuya autorización va a habilitar.
3. En el separador **Políticas de acceso**, seleccione **Asignar acceso**.
4. Seleccione **Asignar acceso dentro de un grupo de recursos**.
5. Seleccione el grupo de recursos predeterminado de la cuenta.
6. Asegúrese de que la opción **Asignar acceso a un grupo de recursos** sigue teniendo el valor **Visor**.
7. Para asignar el servicio correcto, seleccione **Infraestructura de VPC**.
8. Asegúrese de que el valor de **Tipo de recurso** esté establecido en **Todos los tipos de recursos**.
9. Seleccione el rol **Editor**.
10. Pulse **Asignar**.

Ahora el usuario tiene autorización para crear y utilizar recursos de VPC en el grupo de recursos predeterminado de la cuenta. La primera política (pasos 1-6) permite al usuario ver el grupo de recursos predeterminado de cuenta de la cuenta. La segunda política (pasos 7-10) asigna al usuario el rol **Editor** para los recursos de infraestructura de VPC, pero el acceso se limita a los recursos del grupo de recursos predeterminado de la cuenta.

{: tip}

## Visualización de los permisos de su usuario
{: #viewing-your-user-s-permissions}

Las políticas se pueden ver en el separador **Políticas de acceso** del usuario.

Puede utilizar los siguientes mandatos de CLI para validar los permisos de grupo de recursos asignados a su usuario, por política o por grupo de acceso:

**Por política**
```
ibmcloud iam user-policies <username>
```
**Por grupo de acceso**
```
ibmcloud iam access-groups -u <username>
```

Los cambios en las políticas de acceso de IAM para VPC pueden tardar hasta 10 minutos en entrar en vigor.
{: note}

## Caso de ejemplo de acceso por equipo
{: #team-access-scenario}

En este caso de ejemplo se describe cómo un administrador de la cuenta puede asignar autorización para que distintos equipos tengan acceso a distintos recursos de VPC. En el ejemplo se utilizan _grupos de recursos_ para configurar distinto acceso a recursos para dos equipos. A los efectos de este ejemplo, los recursos no se comparten entre los equipos.

El ejemplo le guía por el proceso de creación de grupos de recursos, de creación de grupos de acceso y de asignación de las políticas adecuadas para proporcionar a los equipos acceso a distintos recursos de VPC.

Supongamos que va a configurar dos equipos de proyecto diferentes que utilizarán dos redes privadas virtuales distintas. Asignará acceso a los miembros del equipo de modo que cada equipo tenga acceso a los recursos de VPC de su equipo (únicamente).

* Tu primer equipo es un equipo de prueba. Ha decidido asignar a este equipo acceso a las VPC de un grupo de recursos denominado `test_vpcs`.
* El segundo equipo es su equipo de producción. Se les asignará acceso a las VPC de un grupo de recursos denominado `production_vpcs`.

Esta estrategia se puede utilizar para asignar distintos recursos de VPC a tantos equipos como desee. Sin embargo, todos los recursos comparten las mismas cuotas de VPC para la cuenta. Para obtener más información sobre cuotas y límites, consulte [Cuotas de VPC](/docs/vpc-on-classic?topic=vpc-on-classic-quotas#quotas).
{: tip}

### Paso 1: Crear grupos de recursos
{: #step-1-create-resource-groups}

De forma predeterminada, los administradores de cuentas tienen acceso para crear nuevos grupos de recursos. A los demás usuarios primero se les debe asignar el rol de **Editor** sobre **Todos los servicios de gestión de cuentas**, lo que les permite crear grupos de recursos.

La primera tarea consiste en crear grupos de recursos que contendrán cada uno de los recursos de VPC de los equipos.

1. Cree un grupo de recursos denominado `test_team`.  
2. Cree un grupo de recursos denominado `production_team`.  

Para obtener más información sobre cómo crear grupos de recursos, consulte [Gestión de grupos de recursos](/docs/resources?topic=resources-rgs).

### Paso 2: Crear grupos de acceso
{: #step-2-create-access-groups}

El acceso a los recursos se puede asignar a usuarios individuales o a grupos de usuarios. Los grupos de usuarios con los mismos permisos de acceso se denominan _grupos de acceso_. En este caso de ejemplo, creará un grupo de acceso que representará cada agrupación de miembros del equipo que necesiten un tipo específico de acceso de VPC; en total creará 4 grupos de acceso exclusivos.

**Tarea:** crear cuatro grupos de acceso con los siguientes nombres: `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs`, `production_team_view_vpcs`.

Para obtener ayuda para la creación de grupos de acceso, consulte el apartado sobre [Creación de grupos de acceso](/docs/iam?topic=iam-groups#create_ag).

### Paso 3: Añadir políticas de IAM a los grupos de acceso que acaba de crear
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

Añada las políticas de acceso de VPC necesarias para que el grupo de acceso `test_team` pueda gestionar (crear, actualizar y suprimir) recursos de VPC.  

1. Vaya a la [interfaz de usuario de grupos de IAM ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam#/groups){: new_window} en la consola de IBM Cloud.
2. Seleccione el grupo de acceso que desee (empiece por `test_team_manage_vpcs`).
3. En el separador **Políticas de acceso**, pulse **Asignar acceso**.
4. Seleccione **Asignar acceso dentro de un grupo de recursos**.
5. Seleccione el grupo de recursos que desee (empiece por `test_team`).
6. Asegúrese de que **Asignar acceso a un grupo de recursos** sigue teniendo el valor **Visor**.
7. Seleccione el servicio **Infraestructura de VPC**.
8. Asegúrese de que el **Tipo de recurso** sigue siendo **Todos los tipos de recursos**.
9. Seleccione el rol **Editor**.
10. Seleccione **Asignar**.

Estos pasos también asignan el acceso de **Visor** al grupo de recursos `test_team`. Se requiere acceso de visor para actualizar, crear y suprimir recursos dentro del grupo de recursos `test_team`.

{: tip}

Repite los pasos anteriores para los otros tres grupos de acceso. Para realizar estas tareas, debe combinar grupos de acceso con grupos de recursos:

* Asigne al grupo de acceso `test_team_view_vpcs` el rol de **Visor** sobre los recursos de infraestructura de VPC del grupo de recursos `test_team`.
* Asigne al grupo de acceso `production_team_manage_vpcs` el rol de **Editor** sobre los recursos de infraestructura de VPC del grupo de recursos `production_team`.
* Asigne al grupo de acceso `production_team_view_vpcs` el rol de **Visor** sobre los recursos de infraestructura de VPC del grupo de recursos `production_team`.

Los usuarios con acceso de **Editor** sobre los recursos de VPC también pueden visualizarlos. No es necesario añadir miembros a los grupos de acceso **Editor** Y **Visor**.

#### Configuración del acceso de visor
{: #setting-up-viewer-access)

Los recursos de `Floating IP` de infraestructura de VPC se crean en el grupo de recursos predeterminado de cuenta de la cuenta. Por lo tanto, los usuarios que tienen que gestionar `IP flotantes` necesitan acceso de **Visor** sobre el grupo de recursos predeterminado de la cuenta.
{: tip}

Este es el modo de crear el acceso de **Visor**:

1. Vaya a la [interfaz de usuario de grupos de IAM ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam#/groups){: new_window} en la consola de IBM Cloud.
2. Seleccione el grupo de acceso que desee (empiece por `test_team_manage_vpcs`).
3. En el separador **Políticas de acceso**, pulse **Asignar acceso**.
4. Seleccione **Asignar acceso a servicios de gestión de cuentas**.
7. Seleccione el servicio **Todos los servicios de gestión de cuentas**.
9. Seleccione el rol **Visor**.
10. Seleccione **Asignar**.

Repita los pasos anteriores para asignar al grupo de acceso `production_team_manage_vpcs` el rol de **Visor** sobre **Todos los servicios de gestión de cuentas**.

### Paso 4: Añadir usuarios a los grupos de acceso
{: #step-4-add-users-to-the-access-groups}

Ahora puede asignar miembros del equipo (usuarios) a los grupos de acceso adecuados. Siga estos pasos para añadir cada miembro del equipo de prueba al grupo de acceso que permite la gestión de VPC del equipo de prueba:

1. Vaya a la [interfaz de usuario de grupos de IAM ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/iam#/groups){: new_window} en la consola de IBM Cloud.
2. Seleccione el grupo de acceso que desee (empiece por `test_team_manage_vpcs`).
3. En la página **Usuarios**, pulse **Añadir usuarios**.
4. Seleccione cada usuario que desee añadir al grupo de acceso.
5. Pulse **Añadir a grupo**.

Repite los pasos anteriores para realizar estas tareas:
* Asignar los usuarios que desee al grupo de acceso `test_team_view_vpcs`.
* Asignar los usuarios que desee al grupo de acceso `production_team_manage_vpcs`.
* Asignar los usuarios que desee al grupo de acceso `production_team_view_vpcs`.

No es necesario asignar a los usuarios el acceso de **Visor** sobre los recursos de VPC si tienen acceso de gestión.
{: tip}

## Siguientes pasos
{: #permissions-next-steps}

Ahora los dos equipos de ejemplo están configurados para utilizar las VPC. En este punto, los miembros de los grupos de acceso `test_team_manage_vpcs`
y `production_team_manage_vpcs` pueden crear VPC en sus grupos de recursos asignados.


## Enlaces relacionados
{: #permissions-related-links}

* [Gestión de identidad y acceso](/docs/iam?topic=iam-getstarted)
* [Gestión de usuarios y acceso](/docs/iam?topic=iam-iamuserinv)
* [¿Qué es IAM?](/docs/iam?topic=iam-iamoverview)
