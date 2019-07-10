---

copyright:
  years: 2019
lastupdated: "2019-05-07"


keywords: vpc, delete, resources, api, rias

subcollection: vpc

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

# Supresión de una VPC utilizando las API REST
{: #deleting-using-api}

La supresión de una {{site.data.keyword.cloud}} Virtual Private Cloud utilizando las API REST sigue los mismos pasos generales en el proceso de
[deleting](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) que la supresión utilizando la [CLI](/docs/vpc-on-classic?topic=vpc-on-classic-deleting-using-cli).

Éstos son los principales pasos del proceso:

1. Busque todas las subredes de la VPC que desea suprimir.
2. Suprima cada red en la VPC, lo que significa:
    - Suprima todas las pasarelas de VPN de la subred.
    - Suprima todos los equilibradores de carga de la subred.
    - Suprima todas las interfaces de red (instancias) de la subred.
    - Suprima la subred.
3. Suprima todas las pasarelas públicas de la VPC.
4. Suprima la VPC.

En las secciones siguientes se proporcionan algunas llamadas de API de ejemplo, utilizando `curl`, que se puede ejecutar para suprimir una VPC.

## Paso 1: Busque todas las subredes de la VPC que desea suprimir
{: #deleting-find-subnets-api}

Para poder suprimir una VPC, antes se debe suprimir cada una de sus subredes. Obtenga el ID de la VPC que desea suprimir ejecutando el mandato siguiente y mirando el valor de `id`:

Añada ` | json_pp ` o ` | jq ` después del mandato curl para obtener una serie JSON legible.
{: tip}

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Guarde el ID de la VPC en una variable para poder utilizarlo más adelante; por ejemplo:

```
vpc="3524fef5-da35-4622-bf9a-31614964649d"
```
{: pre}

Para obtener la lista de todas las subredes de la cuenta, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/subnets?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Mire el valor de `vpc` para determinar las subredes que hay que suprimir. Guarde el ID de la subred en una variable para su uso posterior, por ejemplo:

```
subnet="d31ce469-9b0f-44e1-87ce-0fc77d8b0e53"
```
{: pre}

Si la VPC que desea suprimir tiene varias subredes, repita los pasos anteriores para suprimir cada una de las subredes.
{: important}

## Paso 2: Suprima cada subred de la VPC
{: #deleting-subnet-resources-api}

### Suprima todas las pasarelas de VPN de la subred, si las hay

Para listar todas las pasarelas de VPN de la cuenta, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/vpn_gateways?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Para cada pasarela de VPN de la subred que desea suprimir, ejecute el mandato siguiente, donde `$vpnid` es el ID de la pasarela VPN.

```bash
curl -X DELETE "$rias_endpoint/v1/vpn_gateways/$vpnid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

El estado de la pasarela VPN debe cambiar a `deleting` inmediatamente, pero aún se devolverá al realizar una consulta de lista. La supresión de una pasarela VPN puede tardar hasta 30 minutos. Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima la pasarela VPN, pero la subred no se puede suprimir hasta que la pasarela VPN y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

### Suprima todos los equilibradores de carga de la subred, si los hay
{: #deleting-lbs-api}

Para listar todos los equilibradores de carga de la cuenta, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/load_balancers?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Para cada equilibrador de carga de la subred que desea suprimir, ejecute el mandato siguiente, donde `$lbid` es el ID del equilibrador de carga.

```bash
curl -X DELETE "$rias_endpoint/v1/load_balancers/$lbid?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

El estado del equilibrador de carga debe cambiar a `deleting` inmediatamente, pero aún se devolverá al realizar una consulta de lista. La supresión de un equilibrador de carga puede tardar hasta 30 minutos. Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima el equilibrador de carga, pero la subred no se puede suprimir hasta que el equilibrador de carga y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

### Suprima todas las interfaces de red de la subred, si las hay
{: #deleting-nics-api}

Una instancia puede tener varias interfaces de red, y esas interfaces de red pueden pertenecer a varias subredes de la VPC. Para poder suprimir una subred, se debe suprimir primero cualquier interfaz de red que tenga la subred. La única forma de suprimir una interfaz de red de una subred es suprimir la instancia.

Para listar todas las instancias de la cuenta, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/instances?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Si va a suprimir todas las instancias de la VPC, puede ejecutar el mandato siguiente para cada instancia de la VPC, donde `$vsi` es el ID de la instancia que desea suprimir. Cuando se suprime la instancia, todas las interfaces de red de otras subredes, si las hay, se suprimen automáticamente.

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$vsi?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

El estado de la instancia debe cambiar a `deleting` inmediatamente, pero todavía se puede visualizar en los resultados de una consulta de lista. La supresión de una instancia puede tardar hasta 30 minutos. Puede solicitar que se supriman otros recursos de la subred en paralelo mientras espera a que se suprima la instancia, pero la subred no se puede suprimir hasta que la instancia y todos los demás recursos de la subred ya no aparezcan en las consultas de lista.

Para listar todas las interfaces de red de una instancia, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/instances/$vsi/network_interfaces?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Si existe una interfaz de red secundaria en la subred que está intentando suprimir, tiene que suprimirla. Una interfaz de red no se puede suprimir de la instancia sin suprimir la instancia.

### Suprima la subred
{: #deleting-subnet-api}

Una vez que se hayan suprimido todos los recursos de la subred y no se devuelven al ejecutar una consulta de lista, ejecute el mandato siguiente para suprimir la subred:

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

El estado de la subred debe cambiar a `deleting` inmediatamente, pero puede tardar unos minutos en suprimirse, hasta que ya no se devuelva en las consultas de lista.

## Paso 3: Suprima todas las pasarelas públicas de la VPC, si las hay
{: #deleting-pgs-api}

Para listar todas las pasarelas públicas de la cuenta, ejecute el mandato siguiente:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

Para cada pasarela pública de la VPC que desee suprimir, ejecute el mandato siguiente para suprimir la pasarela pública, donde `$gateway` es el ID de pasarela pública.

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}


## Paso 4: Suprima la VPC
{: #deleting-single-vpc-api}

Una vez que se han suprimido todas las subredes, pasarelas de VPN, equilibradores de carga, pasarelas públicas de la VPC, ejecute el mandato siguiente para suprimir la VPC, donde `$vpc` es el ID de la VPC que se va a suprimir.

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \     
     -H "Authorization:$iam_token"
```
{: pre}

El estado de la VPC debe cambiar a `deleting` inmediatamente, pero puede tardar unos minutos en suprimirse, hasta que deje de aparecer en las consultas de lista.
