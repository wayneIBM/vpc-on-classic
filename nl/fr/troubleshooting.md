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

# Traitement des incidents liés à votre cloud privé virtuel IBM Cloud
{: #troubleshooting-your-ibm-cloud-vpc}

Ce document présente les difficultés fréquentes que vous risquez de rencontrer et propose des conseils utiles.

## Activation du mode `TRACE` dans l'interface de ligne de commande
{: #troubleshoot}

Pour activer le mode (débogage) `TRACE` dans l'interface de ligne de commande, définissez `IBMCLOUD_TRACE=true` avant la commande d'interface de ligne de commande.

Exemple :

 ```
IBMCLOUD_TRACE=true ibmcloud is pubgws
```

## Utilisation du mode DEBUG ou `verbose` lors de l'utilisation de l'API
{: #troubleshoot}

Par exemple, vous pouvez ajouter `--verbose` à votre commande `curl` et nous envoyer la valeur `X-Request-Id:` pour que nous puissions traiter le problème. L'indicateur `--verbose` est particulièrement utile si vous rencontrez des problèmes de connectivité.

Vous pouvez également ajouter l'indicateur `-i` à votre commande `curl` pour que l'équipe de support puisse voir les en-têtes dans la réponse de votre demande. Cet indicateur est utile dans la plupart des cas où vous avez besoin de contacter le support.

## Les appels d'API nécessitent un jeton Bearer
{: #troubleshoot}

Lors de l'utilisation de l'API avec une commande cURL, il peut être nécessaire d'inclure "Bearer" dans l'en-tête d'autorisation, selon les données qui se trouvent dans le jeton `$iam_token`. Si ce dernier inclut le mot "Bearer", il n'est pas nécessaire de l'inclure à nouveau dans l'en-tête. Dans la plupart de nos exemples, on suppose que "Bearer" est inclus dans l'en-tête.

## Problèmes courants
{: #troubleshoot}

Voici quelques difficultés que vous pouvez rencontrer.

### Non autorisé (erreur 401 ou 403)
{: #troubleshoot}

Il se peut que votre compte ne soit pas autorisé pour VPC. Assurez-vous d'utiliser un compte qui a été intégré.

### Impossible de créer des ressources
{: #troubleshoot}

Si vous ne parvenez pas à créer un cloud privé virtuel ou d'autres ressources, assurez-vous que le propriétaire du compte vous a accordé les [droits](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) appropriés.

### Toutes les instances ont le statut Unknown
{: #troubleshoot}

Assurez-vous que le propriétaire du compte vous a accordé les [droits](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) appropriés permettant d'afficher et gérer des instances.

### Aucune réponse de l'API
{: #troubleshoot}

Si l'API ne renvoie plus de code JSON, il se peut que votre jeton IAM ait expiré et qu'il doive être actualisé. Reconnectez-vous à IBM Cloud ou actualisez votre jeton en exécutant `iam_token=$(ibmcloud iam oauth-tokens | awk '/IAM/{ print $4; }')`.

### Erreur : 409 Conflit lors de l'appel d'une action ou d'une instance
{: #troubleshoot}

Vous ne pouvez pas appeler certaines actions d'instance si le statut de votre instance est en conflit avec l'action. Par exemple, si le statut de votre instance est Stopped (Arrêté) et que vous essayez d'exécuter l'action `reboot`, le système renvoie une erreur 409.

| statut      | action     | conflit  |
| ----------- | ---------- | -------- |
| Running     | démarrer   | oui      |
| Stopped     | toute action sauf démarrer  | oui      |
| Not running | suspendre  | oui      |
| Not running | réamorcer  | oui      |
| Not paused  | reprendre  | oui      |
| Paused      | toute action sauf reprendre | oui      |


### L'instance ne répond pas à la demande `instance-reboot`
{: #troubleshoot}

Si votre instance ne répond pas à une demande `instance-reboot`, vous pouvez essayer d'exécuter une demande `instance-reset`. Considérez `instance-reboot` comme une demande à chaud et `instance-reset` comme une demande à froid. La demande `instance-reboot` envoie une demande de réamorçage du système d'exploitation à l'instance, mais une demande `instance-reset` procède à une réinitialisation à froid de l'instance de serveur virtuel. Vous pouvez comparer la différence entre ces opérations à la différence entre la saisie de "ctrl-alt-suppr" sur le clavier de votre ordinateur et l'utilisation de la touche de réinitialisation ou du bouton d'alimentation. Gardez à l'esprit que l'exécution de la demande `instance-reset` est plus longue que celle de la demande `instance-reboot`.

### Impossible de supprimer des ressources
{: #troubleshoot}

Certaines opérations, comme par exemple la création et la suppression d'instances de serveur virtuel et la création et la suppression de sous-réseaux, sont effectuées de façon asynchrone via l'API. Ainsi, il est recommandé d'interroger les ressources que vous supprimez afin de vérifier qu'elles ont été supprimées avant de continuer.

La suppression des ressources sur le système peut prendre plusieurs minutes en raison de ces opérations asynchrones. Pour faciliter la suppression, il est recommandé d'effectuer les opérations dans l'ordre suivant :

1. Supprimez vos instances
2. Supprimez vos passerelles publiques
3. Supprimez vos sous-réseaux
4. Supprimez vos clouds privés virtuels

Pour des informations spécifiques, consultez les instructions sur la [suppression d'un cloud privé virtuel](/docs/vpc-on-classic?topic=vpc-on-classic-deleting).
