---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-17"

keywords: troubleshoot, tips, limitations, debug, mode, error, bearer, token, API, CLI, endpoint, problem, reboot, 409, status, instance, reset, asynchronous

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
{:troubleshoot: .troubleshoot}

# Troubleshooting your IBM Cloud VPC
{: #troubleshooting-your-ibm-cloud-vpc}

This document covers common difficulties you might encounter, and offers some helpful tips.

## Turning on `TRACE` mode when using the CLI
{: #troubleshoot}

To turn on `TRACE` (debug) mode when using the CLI, set `IBMCLOUD_TRACE=true` before the CLI command.

For example:

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Using DEBUG mode or `verbose` mode when using the API
{: #troubleshoot}

For example, you can add `--verbose` to your `curl` command and send us the `X-Request-Id:` value so we can troubleshoot the problem. The `--verbose` flag is particularly helpful if you are experiencing connectivity problems.

Another option is to add the `-i` flag to your `curl` command so that the support team can see the headers in the response of your request. This flag is helpful in most situations when you need to contact support.

## API calls require a Bearer token
{: #troubleshoot}

When using the API with a cURL command, you may need to include "Bearer" in the Authorization header, depending on what is in the `$iam_token`. If it includes the word "Bearer" you don't include it again in the header. Most of our examples assume that "Bearer" is included in the header.

## Common problems
{: #troubleshoot}

Here are a few difficulties you might encounter.

### Not authorized (401 or 403 error)
{: #troubleshoot}

Your account may not be authorized for VPC. Make sure you are using an account that has been onboarded.

### Cannot create resources
{: #troubleshoot}

If you cannot create a VPC or other resources, make sure the owner of the account has granted you the correct [permissions](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

### Instances are all in Unknown status
{: #troubleshoot}

Make sure the owner of the account has granted you the correct [permissions](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) to view and manage instances.

### No response from API
{: #troubleshoot}

If the API is no longer returning any JSON, it is likely your IAM token has expired and needs to be refreshed. Log in to IBM Cloud again or refresh your token by running `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.

### Error: 409 conflict when invoking an action on an instance
{: #troubleshoot}

You can't invoke certain instance actions if the status of your instance is in conflict with the action. For example, if your instance status is `stopped`, and you try to run a `reboot` action, the system returns a 409 error.

| status      | action     | conflict |
| ----------- | ---------- | -------- |
| Running     | start      | yes      |
| Stopped     | any action but start  | yes      |
| Not running | pause      | yes      |
| Not running | reboot     | yes      |
| Not paused  | resume     | yes      |
| Paused      | any action but resume | yes      |


### Instance not responding to `instance-reboot` request
{: #troubleshoot}

If your instance is not responding to an `instance-reboot` request, you can try an `instance-reset` request. You can think of `instance-reboot` as a soft request and `instance-reset` as a hard request. The `instance-reboot` request sends an OS-reboot request to the instance, but an `instance-reset` request performs a hard reset of the VSI instance. You could think of the difference as typing "ctrl-alt-delete" on your computer's keyboard versus hitting the reset or power button. It is good to remember that the `instance-reset` request takes longer to complete than the `instance-reboot` request.

### Cannot delete resources
{: #troubleshoot}

Certain operations--creating and deleting VSIs, and creating and deleting subnets, for example--are completed asynchronously through the API. Because of this fact, it is recommended to poll the resources you're deleting, to check for deletion before proceeding.

It can take several minutes for resources to be deleted from the system, due to these asynchronous operations. To facilitate deletion, the best practice is to do things in this order:

1. Delete your instances
2. Delete your public gateways
3. Delete your subnets
4. Delete your VPCs

For specific information, refer to the instructions on [how to delete a VPC](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).
