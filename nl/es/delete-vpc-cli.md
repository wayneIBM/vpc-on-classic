---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, cli, rias, infrastructure

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
{:DomainName: data-hd-keyref="DomainName"}

# Supresión de una VPC utilizando la CLI de IBM Cloud
{: #deleting-using-cli}

En este tema se proporcionan ejemplos de cómo suprimir recursos de {{site.data.keyword.cloud}} Virtual Private Cloud, para cada recurso de VPC, en el orden sugerido. 

A continuación se muestran algunos hechos clave para recordar la supresión:

* No se puede suprimir una VPC hasta que esté vacía. 
* Se deben suprimir satisfactoriamente todos los recursos contenidos para poder suprimir el recurso padre. 
* Si el recurso está pendiente de una supresión, muestra el estado `deleting` al visualizarlo en las consultas de lista. 
* La mayoría de las solicitudes de supresión son _asíncronas_, lo que significa que el recurso puede mostrar el estado **deleting** en las consultas de lista cuando todavía no se ha suprimido completamente. 
* Debe sondear para asegurarse de que el recurso hijo ya no se encuentra en la vista de lista antes de intentar suprimir el recurso padre. 

Si el estado del recurso cambia de `deleting` a `failed`, puede volver a intentar suprimir el recurso. Si no puede suprimir un recurso en estado `failed`, [Póngase en contacto con el servicio de soporte](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support).
{: tip}

## Paso 1: Busque todas las subredes dentro de la VPC que desea suprimir
{: #deleting-find-subnets-cli}

Para poder suprimir una VPC, antes se debe suprimir cada una de sus subredes. Obtenga el ID de la VPC que desea suprimir ejecutando el mandato siguiente y mirando el valor de `ID`:

```
ibmcloud is vpcs
```
{: pre}

Guarde el ID de la VPC en una variable para poder utilizarlo más adelante; por ejemplo:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Para obtener la lista de todas las subredes de la cuenta, ejecute el mandato siguiente:

```
ibmcloud is subnets
```
{: pre}

Mire el valor de `VPC` para determinar las subredes que hay que suprimir. Guarde el ID de la subred en una variable para poder utilizarlo más adelante; por ejemplo:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Si la VPC que desea suprimir tiene varias subredes, repita estos pasos para suprimir cada una de las subredes.
{: important}

## Paso 2: Suprima cada subred de la VPC
{: #deleting-subnet-resources-cli}

### Suprima todas las pasarelas de VPN de la subred, si las hay

Para listar todas las pasarelas de VPN de la cuenta, ejecute el mandato siguiente:

```
ibmcloud is vpn-gateways
```
{: pre}

Para cada pasarela de VPN de la subred que desea suprimir, ejecute el mandato siguiente, donde `$vpnid` es el ID de la pasarela VPN.

```
ibmcloud is vpn-gateway-delete $vpnid
```

El estado de la pasarela VPN debe cambiar a `deleting` inmediatamente, pero aún aparecerá en el resultado de una consulta de lista. La supresión de una pasarela VPN puede tardar hasta 30 minutos.
{: note}

Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima la pasarela VPN. Sin embargo, la subred no se puede suprimir hasta que la pasarela VPN y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

### Suprima todos los equilibradores de carga de la subred, si los hay
{: #deleting-lbs-cli}

Para listar todos los equilibradores de carga de la cuenta, ejecute el mandato siguiente:

```
ibmcloud is load-balancers
```
{: pre}

Para cada equilibrador de carga de la subred que desea suprimir, ejecute el mandato siguiente, donde `$lbid` es el ID del equilibrador de carga.

```
ibmcloud is load-balancer-delete $lbid
```
{: pre}

El estado del equilibrador de carga debe cambiar a `deleting` inmediatamente, pero aún se mostrará en un resultado de consulta de lista. La supresión de un equilibrador de carga puede tardar hasta 30 minutos.
{: note}

Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima el equilibrador de carga. Sin embargo, la subred no se puede suprimir hasta que el equilibrador de carga y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

### Suprima todas las interfaces de red de la subred, si las hay
{: #deleting-nics-cli}

Una instancia puede tener varias interfaces de red, y esas interfaces de red pueden pertenecer a varias subredes de la VPC. Para poder suprimir una subred, se debe suprimir primero cualquier interfaz de red que haya en la subred. 

La única forma de suprimir una interfaz de red de una subred es suprimir la instancia.
{: note}

Para listar todas las instancias de la cuenta, ejecute el mandato siguiente:

```
ibmcloud is instances
```
{: pre}

Si va a suprimir todas las instancias de la VPC, puede ejecutar el mandato siguiente para cada instancia de la VPC, donde `$vsi` es el ID de la instancia que desea suprimir. Cuando se suprime la instancia, todas las interfaces de red de otras subredes, si las hay, se suprimen automáticamente.

```
ibmcloud is instance-delete $vsi
```
{: pre}

El estado de la instancia debe cambiar a `deleting` inmediatamente, pero aún aparecerá en el resultado de una consulta de lista. La supresión de una instancia puede tardar hasta 30 minutos.
{: note}

Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima la instancia. Sin embargo, la subred no se puede suprimir hasta que la instancia y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

Para listar todas las interfaces de red de una instancia, ejecute el mandato siguiente:

```
ibmcloud is instance-network-interfaces $vsi
```
{: pre}

Si existe una interfaz de red secundaria en la subred que está intentando suprimir, debe suprimirla. Una interfaz de red no se puede suprimir de la instancia sin suprimir la instancia.

### Suprima la subred
{: #deleting-subnet-cli}

Una vez que se hayan suprimido todos los recursos de la subred, que significa que no se devuelven en el resultado de una consulta de lista, ejecute el mandato siguiente para suprimir la subred:

```
ibmcloud is subnet-delete $subnet
```
{: pre}

El estado de la subred debe cambiar a `deleting` inmediatamente, pero pueden pasar unos minutos hasta que la subred se suprima realmente y ya no aparezca en los resultados de la consulta de lista.

## Paso 3: Suprima todas las pasarelas públicas de la VPC, si las hay
{: #deleting-pgs-cli}

Para listar todas las pasarelas públicas de la cuenta, ejecute el mandato siguiente:

```
ibmcloud is public-gateways
```
{: pre}

Para cada pasarela pública de la VPC que desee suprimir, ejecute el mandato siguiente para suprimir la pasarela pública, donde `$gateway` es el ID de pasarela pública.

```
ibmcloud is public-gateway-delete $gateway
```
{: pre}


## Paso 4: Suprima la VPC
{: #deleting-single-vpc-cli}

Una vez que se han suprimido todas las subredes, pasarelas de VPN, equilibradores de carga, pasarelas públicas de la VPC, ejecute el mandato siguiente para suprimir la VPC, donde `$vpc` es el ID de la VPC que se va a suprimir.

```
ibmcloud is vpc-delete $vpc
```
{: pre}

El estado de la VPC debe cambiar a `deleting` inmediatamente, pero pueden pasar unos minutos hasta que la VPC se suprima realmente y ya no aparezca en los resultados de la consulta de lista.
