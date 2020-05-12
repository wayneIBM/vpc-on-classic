---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-17"

keywords: 

subcollection: vpc-on-classic

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .aria-labeledby="caption"}
{:download: .download}

# VPC CLI quick reference 
{: #quick-reference-to-cli-commands-for-resources}

Use this quick reference to get going with the {{site.data.keyword.cloud}} CLI for Virtual Private Cloud.

* To log in
```
ibmcloud login [-sso]
```

* To get information about VPCs
```
ibmcloud is vpcs
```

   Use the resource's ID to view specific details on the resource.
   {: tip}

* To view VPC details
```
ibmcloud is vpc [vpc_id]
```

* To get information about subnets
```
ibmcloud is subnets
```

* To get subnet details
```
ibmcloud is subnet [subnet_id]
```

* To get information about instances
```
ibmcloud is instances
```

* To view details about instances
```
ibmcloud is instance [instance_id]
```

* To get information about floating IPs
```
ibmcloud is floating-ips
```

* To view floating IP details
```
ibmcloud is floating-ip [floating-ips_id]
```

* To get information about your regions and endpoints
```
ibmcloud is regions
```

* To get information about zones
```
ibmcloud is zones [region_name]
```

* To get information about all block storage volumes
```
ibmcloud is volumes
```

* To view details about a block storage volume
```
ibmcloud is volume [volume_id]
```
