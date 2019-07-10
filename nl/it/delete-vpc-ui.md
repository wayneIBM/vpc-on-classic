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

# Eliminazione di un VPC utilizzando la console IBM Cloud
{: #deleting-using-console}

Prima di poter eliminare un VPC nella console IBM Cloud, devi eliminare tutti i gateway pubblici, tutte le sottoreti e tutte le risorse collegate del VPC. 

Per eliminare un VPC utilizzando la console:

1. Trova tutte le sottoreti nel VPC.  Fai clic su **VPCs** nel riquadro di navigazione e seleziona il tuo VPC. 
2. Nella pagina dei dettagli del VPC, vai all'elenco **Subnets in this VPC** e seleziona una sottorete per visualizzarne i dettagli.
3. Elimina tutte le risorse collegate alla sottorete. Nel riquadro di navigazione, fai clic su **Attached resources**. Per ogni istanza, programma di bilanciamento del carico o gateway VPN collegato, seleziona la risorsa per andare alla relativa pagina dei dettagli. Nella pagina dei dettagli per ogni risorsa, fai clic sull'icona di eliminazione. 

  Sebbene lo stato della risorsa cambi immediatamente in **Deleting**, l'operazione di eliminazione può richiedere fino a 30 minuti. La sottorete non può essere eliminata finché tutte le risorse collegate non vengono più visualizzate nella console.
  {: tip}

4. Dopo che tutte le risorse collegate sono state eliminate, torna alla pagina dei dettagli della sottorete e fai clic sull'icona di eliminazione.
5. Dopo aver eliminato tutte le sottoreti, torna alla pagina dei dettagli del VPC e fai clic sull'icona di eliminazione. Il VPC e tutti i gateway pubblici vengono eliminati.
