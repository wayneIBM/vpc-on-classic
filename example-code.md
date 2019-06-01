---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-30"


keywords: create, VPC, API, IAM, token, permissions, endpoint, region, zone, profile, status, subnet, gateway, floating IP, delete, resource, provision

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

# Creating a VPC using the REST APIs
{: #creating-a-vpc-using-the-rest-apis}

This document shows you how to create {{site.data.keyword.cloud}} Virtual Private Cloud resources using the REST APIs using `curl`. For code snippets calling the REST APIs using Go and Python, see this [example code](https://github.com/IBM-Cloud/vpc-api-samples).

## Prerequisites

1. Make sure you have a public SSH key, which will be used to connect to the virtual server instance.

   You may have a public SSH key already. Look for a file called `id_rsa.pub` under an `.ssh` directory under your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. The file starts with `ssh-rsa` and ends with your email address.

   If you do not have a public SSH key or if you forgot the password of an existing one, generate a new one by running the `ssh-keygen` command and following the prompts.
2.  Make sure you have an API key for your IBM Cloud account. If you don't have an API key, see [Creating an API key](/docs/iam?topic=iam-userapikey#create_user_key). You will need to store this API key in an environment variable in Step 1.

## Step 1: Store your API Key as a variable

Run the following command to store your API key in an environment variable:

```bash
apikey="<YOUR_API_KEY>"
```
{: pre}

## Step 2: Get an IBM Identity and Access Management (IAM) token

Refer to the [Getting an IBM Cloud IAM token by using an API key](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey) topic on how to get an IAM token or use the following example commands.

Run the following command to get and parse an IAM token using the utility `jq`. You can modify the command to use another parsing tool, or you can remove the last part of the command if you prefer to parse the token yourself, manually.

```bash
iam_token=`curl -k -X POST \
  --header "Content-Type: application/x-www-form-urlencoded" \
  --header "Accept: application/json" \
  --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
  --data-urlencode "apikey=$apikey" \
  "https://iam.cloud.ibm.com/identity/token"  |jq -r '(.token_type + " " + .access_token)'`
```
{: pre}

To view the IAM token, run `echo $iam_token`. The result should look like this:

```
Bearer <your token>
```

The Authorization header expects the IAM token to begin with `Bearer`. If the result you receive does not include `Bearer`, update the `iam_token` variable to include it. These examples assume that `Bearer` is included in the `iam_token`.

You must repeat the preceding step to refresh your IAM token every hour, because the token expires.
{: important}

## Step 3: Store the API endpoint and version parameter as a variable

The API endpoints are per region and follow the convention `https://<region>.iaas.cloud.ibm.com`. Endpoints by region are:

* US-SOUTH: `https://us-south.iaas.cloud.ibm.com`
* EU-DE: `https://eu-de.iaas.cloud.ibm.com`
* JP-TOK: `https://jp-tok.iaas.cloud.ibm.com`

Store the API endpoint in a variable so you can use it in API requests without having to type the full URL. For example, to store the us-south endpoint in a variable, run this command:

```bash
rias_endpoint="https://us-south.iaas.cloud.ibm.com"
 ```
{: pre}

To verify that this variable was saved, run `echo $rias_endpoint` and make sure the response is not empty.

Every API request must include the `version` parameter, in the format `YYYY-MM-DD`. Run the following command to store the version date in a variable so it can be reused in your session. For more information about setting the `version` parameter, see **Versioning** in the [Regional API for VPC](https://{DomainName}/apidocs/vpc-on-classic#versioning)

```bash
version="2019-05-31"
 ```
{: pre}

To verify that this variable was saved, run `echo $version` and make sure the response is not empty.

## Step 4: Run the GET regions API

The following command returns the regions available for VPC, in JSON format. At least one object should be returned.

A `version` and `generation` query parameter is required in each API request. For details, see the [Regional API for VPC](https://{DomainName}/apidocs/vpc-on-classic).
{: note}

```bash
curl -X GET "$rias_endpoint/v1/regions?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

If you encounter unexpected results, add the `--verbose` (debug) flag after the `curl` command to obtain detailed logging information. Also see commonly-encountered errors in our [troubleshooting page](/docs/vpc-on-classic?topic=vpc-on-classic-troubleshooting-your-ibm-cloud-vpc).
{: tip}


## Step 5: Run the GET zones API

The following command returns all the zones available for VPC in region `us-south`, in JSON format. At least three objects should be returned.

```bash
curl -X GET "$rias_endpoint/v1/regions/us-south/zones?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Update the region name in the parameter, `us-south` above, depending on the region endpoint you are using. A region endpoint only knows
about its own zones, so if you are using the endpoint in TOKYO, use `jp-tok`.
{: note}

## Step 6: Run the GET profiles API

The following command returns the profiles available for your instances, in JSON format. At least one object should be returned.

Add ` | json_pp ` after curl command to get readable JSON string
{: tip}


```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

If the API output is limited by pagination, set the limit higher than the `total_count`, for example:

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1&limit=50" \
     -H "Authorization: $iam_token"
```
{: pre}

## Step 7: Run the GET images API

The following command returns the images available for your instances, in JSON format. At least one object should be returned.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Step 8: Run the GET VPCs API

The following command returns any VPCs that have been created under your account, in JSON format.

```bash
curl -X GET "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Step 9: Create a virtual private cloud

Create an {{site.data.keyword.cloud_notm}} VPC called `my-vpc`.

```bash
curl -X POST "$rias_endpoint/v1/vpcs?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
      	"name": "my-vpc"
      }'
```
{: pre}

For the rest of the calls, you'll need to know the ID of the newly-created {{site.data.keyword.cloud_notm}} VPC. Store its ID in a variable. The command will look something like this: `vpc="35fb0489-7105-41b9-99de-033fae723006"`

```bash
vpc="<YOUR_VPC_ID>"
```
{: pre}

## Step 10: Create a subnet

Create a subnet in your {{site.data.keyword.cloud_notm}} VPC. The following example creates a VPC in the `us-south-2` zone.

The following example uses the [default address prefix](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets) for zone `us-south-2`. Defaults may have been changed for your account, see [How to Plan your VPC Addresses](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-vpc-addressing-plan-design).
{: note}

To get a list of address prefixes for your vpc, run the following command
`curl -X GET  "$rias_endpoint/v1/vpcs/$vpc/address_prefixes?version=$version&generation=1" -H "Authorization:$iam_token"`..
{: tip}

```bash
curl -X POST "$rias_endpoint/v1/subnets?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-subnet",
        "ipv4_cidr_block": "10.240.64.0/28",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Store the subnet ID in a variable.

```bash
subnet="<YOUR_SUBNET_ID>"
```
{: pre}

## Step 11: Check the status of your subnet

To provision resources in your subnet, the subnet must be in `available` status. Before you continue, query the subnet resource and make sure the status is `available`. If the status is `failed`, contact [Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) with the details. You can attempt to continue by trying to provision another subnet.

```bash
curl -X GET "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}


## Step 12: Create a public gateway

To allow virtual server instances in the subnet to have access to the public internet, add a public gateway (PGW) to the subnet.  

```bash
curl -X POST "$rias_endpoint/v1/public_gateways?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-gateway",
        "zone": { "name": "us-south-2" },
        "vpc": { "id": "'$vpc'" }
      }'
```
{: pre}

Store the public gateway ID in a variable.

```bash
gateway="<YOUR_GATEWAY_ID>"
```
{: pre}

In order to attach and use the public gateway, the public gateway must be in `available` status. Before you continue, query the public gateway resource and make sure the status is `available`. If the status is `failed`, contact [Support](/docs/vpc-on-classic?topic=vpc-on-classic-getting-help-and-support) with the details. You can attempt to continue by trying to provision another public gateway.

To check the status of the public gateway, run the following command:

```bash
curl -X GET "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

## Step 13: Attach the public gateway to the subnet

```bash
curl -X PUT "$rias_endpoint/v1/subnets/$subnet/public_gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "id": "'$gateway'"
      }'
```
{: pre}

## Step 14: Create an SSH key

Create a key with your public SSH key. This key is used when you create the virtual server instance. The key is also needed to log in to the virtual server instance.

Keys can be added when you first create the VPC in the UI or CLI. No tooling exists to add keys later.
{:tip}

```bash
curl -X POST "$rias_endpoint/v1/keys?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-key",
        "public_key": "ssh-rsa AAA....n",
        "type": "rsa"
      }'
```
{: pre}

Store the key ID in a variable.

```bash
key="<YOUR_KEY_ID>"
```
{: pre}

## Step 15: Choose a profile and image for your virtual server instance

Run the APIs to list all profiles and images available for your virtual server instance, and choose a combination.

```bash
curl -X GET "$rias_endpoint/v1/instance/profiles?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

To get additional details about the profiles, you can query the global catalog using the {{site.data.keyword.cloud_notm}} CLI command `ibmcloud catalog entry ID [--children] [--output TYPE] [--global]`. For example:

```
ibmcloud catalog entry bc1-2x8
```
{: pre}

The following command lists the images available.

```bash
curl -X GET "$rias_endpoint/v1/images?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Store the profile name and the image ID in variables, which you'll use to provision a virtual server instance.

```bash
profile_name="<CHOSEN_PROFILE_NAME>"
image_id="<CHOSEN_IMAGE_ID>"
```
{: pre}

## Step 16: Provision a virtual server instance

Provision a virtual server instance (VSI) in the newly-created subnet. Pass in your public SSH key so that you can log in after the instance is provisioned.

```bash
curl -X POST "$rias_endpoint/v1/instances?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "server-1",
        "type": "virtual",
        "zone": {
          "name": "us-south-2"
        },
        "vpc": {
          "id": "'$vpc'"
        },
        "primary_network_interface": {
          "subnet": {
            "id": "'$subnet'"
          }
        },
        "keys":[{"id": "'$key'"}],
        "flavor": {
          "name": "'$profile_name'"
         },
        "image": {
          "id": "'$image_id'"
         },
        "userdata": ""
      }'
```
{: pre}

Store the virtual server instance ID in a variable.

```bash
server="<YOUR_INSTANCE_ID>"
```
{: pre}

## Step 17: Check the status of your virtual server instance

Provisioning a virtual server instance can take up to a few minutes. Before you continue, query the status of the server and make sure it is `running`.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Store the primary network interface ID of the virtual server instance (returned in the previous API call) in a variable.  

```bash
network_interface="<YOUR_NETWORK_INTERFACE_ID>"
```
{: pre}

You can't get the primary network interface until you query the specific instance.
{: note}

## Step 18: Create a floating IP

To create a floating IP for the virtual server instance, use the instance's primary network interface as the target for the new floating IP address.

```bash
curl -X POST "$rias_endpoint/v1/floating_ips?version=$version&generation=1" \
  -H "Authorization:$iam_token" \
  -d '{
        "name": "my-server-floatingip",
        "target": {
            "id":"'$network_interface'"
        }
      }
'
```
{: pre}


Store the floating IP ID in a variable.

```bash
floating_ip="<YOUR_FLOATING_IP_ID>"
```
{: pre}

## Step 19: Add a rule to the default security group to allow SSH traffic

Configure the security group to define the inbound and outbound traffic that is allowed for the instance.

Find the security group for the VPC:

```bash
curl -X GET "$rias_endpoint/v1/vpcs/$vpc/default_security_group?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Store the security group ID in a variable.

```bash
sg=2d364f0a-a870-42c3-a554-000000981149
```
{: pre}

Now create a rule to allow inbound SSH traffic:

```bash
curl -X POST "$rias_endpoint/v1/security_groups/$sg/rules?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "direction": "inbound",
        "protocol": "tcp",
        "port_min": 22,
        "port_max": 22
      }'
```
{: pre}

## Step 20: SSH into your virtual server instance

To SSH to the server, use the `address` of the floating IP you created earlier. To get the `address` of the floating IP, run the following command:

```bash
curl -X GET "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
     -H "Authorization:$iam_token"
```
{: pre}

Use the `address` of the floating IP to connect to the virtual server instance with SSH:

```
ssh -i <private_key_file> root@<floating ip address>
```
{: pre}

SSH access into the virtual server may be prevented by security groups. If SSH access is required, you must use an [appropriate security group](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-using-security-groups).
{: note}

See [Connecting to your instance using Linux](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-connecting-to-your-linux-instance) for more information on how to connect to your instance.

## Step 21: Create and attach a block storage volume

Create a block storage volume with a request similar to this example and specify a volume name, zone, and profile.

To see a list of volume profiles, provide this request:

```bash
curl -X GET "$rias_endpoint/v1/volume/profiles?version=$version&generation=1" \
     -H "Authorization: $iam_token" \
```
{: pre}

Profiles can be general-purpose (3 IOPS/GB), 5iops-tier, 10iops-tier, and custom. See [About Block Storage for VPC](/docs/vpc-on-classic-block-storage?topic=vpc-on-classic-block-storage-block-storage-about#capacity-performance)
for information about volume capacity and IOPS ranges based on the volume profile you select.

You don't have to provide a volume name in the request, the system creates one by default.  This example specifies a volume name, `helloworld-vol`.  All volume names must be unique.

```bash
curl -X POST "$rias_endpoint/v1/volumes?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol",
        "iops": 1000,
        "capacity": 100,
        "zone": {
            "name": "us-south-2"
            },
        "profile": {
            "name": "custom"
            }
      }'
```
{: pre}

For the rest of the calls, you'll need to know the ID of the newly created volume. Save the ID of the volume as a variable, for example: `vol=640774d7-2adc-4609-add9-6dfd96167a8f`.

```bash
volume="<YOUR_VOLUME_ID>"
```
{: pre}

The status of the volume is `pending` when it first is created. Before you can proceed, the volume must move to `available` status, which takes a few minutes.
{: note}


To check the status of the volume, run the following command:

```bash
curl -X GET "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Create a volume attachment to attach the new volume to the virtual server instance. Use the instance ID variable that you created earlier in the request.  Use the volume ID, CRN of the volume, or URL to specify the volume in the data parameter.  This example uses the variable we created for the volume ID.

```bash
curl -X POST "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "helloworld-vol-attachment",
        "volume": {
            "id": "'$volume'"
            }
      }'
```
{: pre}

After creating the volume attachment, get the volume attachment ID.

```bash
curl -X GET "$rias_endpoint/v1/instances/$server/volume_attachments?version=$version&generation=1" \
     -H "Authorization: $iam_token"
```
{: pre}

Save the attachment ID as a variable.

```bash
attachment="<YOUR_VOLUME_ATTACHMENT_ID>"
```
{: pre}

Specify the volume attachment ID to attach the new volume to a virtual server instance.

```bash
curl -X PATCH "$rias_endpoint/v1/instances/$server/volume_attachments/$attachment?version=$version&generation=1" \
    -H "Authorization: $iam_token" \
    -d '{
	  "name": "helloworld-vsi-volattachment"
    }'
```
{: pre}


## Step 22 (Optional): Delete the Resources

Optionally delete the resources. A resource cannot be deleted if it contains other resources. For example, a virtual private cloud cannot be deleted if it contains subnets, and a subnet cannot be deleted if it contains virtual server instances. On a delete operation, the API may return quickly but the resource deletion might still be in progress. After issuing the delete request, make sure the resource has been deleted before attempting to delete the parent resource. See [Deleting a VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting) for more details.

### Delete the floating IP

```bash
curl -X DELETE "$rias_endpoint/v1/floating_ips/$floating_ip?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Delete the virtual server instance

```bash
curl -X DELETE "$rias_endpoint/v1/instances/$server?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Delete the key

```bash
curl -X DELETE "$rias_endpoint/v1/keys/$key?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Delete the subnet

```bash
curl -X DELETE "$rias_endpoint/v1/subnets/$subnet?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Delete the public gateway

```bash
curl -X DELETE "$rias_endpoint/v1/public_gateways/$gateway?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}


### Delete the VPC

```bash
curl -X DELETE "$rias_endpoint/v1/vpcs/$vpc?version=$version&generation=1" \
  -H "Authorization:$iam_token"
```
{: pre}

### Delete the volume

```bash
curl -X DELETE "$rias_endpoint/v1/volumes/$volume?version=$version&generation=1"  \
  -H "Authorization:$iam_token"
```
{: pre}

## Congratulations!

You've successfully provisioned a virtual private cloud instance using the REST APIs. To try out more API commands, explore the full reference:

* [Regional API for VPC](https://{DomainName}/apidocs/vpc-on-classic)
