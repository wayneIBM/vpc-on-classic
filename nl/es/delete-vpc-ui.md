---

copyright:
  years: 2019
lastupdated: "2019-05-17"


keywords: vpc, delete, resources, api, rias, ui, console

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

# Supresión de una VPC utilizando la consola de IBM Cloud
{: #deleting-using-console}

Para poder suprimir una VPC en la consola de IBM Cloud, antes debe suprimir todas las pasarelas públicas, las subredes y los recursos conectados a la VPC.

Para suprimir una VPC utilizando la consola:

1. Busque todas las subredes de la VPC.  Pulse **VPC** en el panel de navegación y seleccione su VPC. 
2. En la página de detalles de VPC, vaya a la lista **Subredes en esta VPC** y seleccione una subred para ver sus detalles.
3. Suprima los recursos que estén conectados a la subred. En el panel de navegación, pulse **Recursos conectados**. Para cada instancia conectada, equilibrador de carga o pasarela de VPN, seleccione el recurso para ir a su página de detalles. En la página de detalles de cada recurso, pulse el icono Suprimir. 

  Aunque el estado del recurso cambia inmediatamente a **Suprimiendo**, la operación de supresión puede tardar hasta 30 minutos en completarse. La subred no se puede suprimir hasta que todos los recursos conectados ya no se muestren en la consola.
  {: tip}

4. Una vez que todos los recursos conectados se hayan suprimido, vuelva a la página de detalles de la subred y pulse el icono Suprimir.
5. Después de suprimir todas las subredes, vuelva a la página de detalles de la VPC y pulse el icono Suprimir. Se suprimen la VPC y las pasarelas públicas que haya.
