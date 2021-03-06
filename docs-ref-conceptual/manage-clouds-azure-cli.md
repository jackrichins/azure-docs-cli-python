---
title: Managing multiple clouds with Azure CLI 2.0
description: Create, log in, and manage multiple clouds with the Azure CLI 2.0.
author: sptramer
manager: carmonm
ms.author: sttramer
ms.date: 10/20/2017
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azure-cli
---

# Managing multiple clouds with Azure CLI 2.0

If you work across different regions or use [Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/), you may need to use more than one cloud. Microsoft provides clouds for compliance with regional laws, which are available for your use. This article shows you how to get information on clouds available to your account, change the current cloud, and register or unregister new clouds for use with Azure Stack.

## Listing clouds

You can list available clouds with the [az cloud list](/cli/azure/cloud#az-cloud-list) command. This tells you which cloud is currently active, what its current profile is, and information on regional suffixes and host names.

To get the active cloud and a list of all the available clouds:

```azurecli
az cloud list --output table
```

```output
IsActive    Name               Profile
----------  -----------------  ---------
True        AzureCloud         latest
            AzureChinaCloud    latest
            AzureUSGovernment  latest
            AzureGermanCloud   latest
```

The currently active cloud has `True` in the `IsActive` column. Only one cloud can be active at any time. To get more detailed information on a cloud, including the endpoints that it uses for Azure services, use the `cloud show` command:

```azurecli
az cloud show --name AzureChinaCloud --output json
```

```output
{
  "endpoints": {
    "activeDirectory": "https://login.chinacloudapi.cn",
    "activeDirectoryDataLakeResourceId": null,
    "activeDirectoryGraphResourceId": "https://graph.chinacloudapi.cn/",
    "activeDirectoryResourceId": "https://management.core.chinacloudapi.cn/",
    "batchResourceId": "https://batch.chinacloudapi.cn/",
    "gallery": "https://gallery.chinacloudapi.cn/",
    "management": "https://management.core.chinacloudapi.cn/",
    "resourceManager": "https://management.chinacloudapi.cn",
    "sqlManagement": "https://management.core.chinacloudapi.cn:8443/",
    "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json"
  },
  "isActive": false,
  "name": "AzureChinaCloud",
  "profile": "latest",
  "suffixes": {
    "azureDatalakeAnalyticsCatalogAndJobEndpoint": null,
    "azureDatalakeStoreFileSystemEndpoint": null,
    "keyvaultDns": ".vault.azure.cn",
    "sqlServerHostname": ".database.chinacloudapi.cn",
    "storageEndpoint": "core.chinacloudapi.cn"
  }
}
```

## Switching the active cloud

To switch the currently active cloud, run the [az cloud set](/cli/azure/cloud#az-cloud-set) command. This command takes one required argument, the name of the cloud.

```azurecli
az cloud set --name AzureChinaCloud
```

> [!IMPORTANT]
> If your authentication for the activated cloud has expired, you need to re-authenticate before performing any other CLI tasks. If this is your first time switching to the new cloud, you also need to set the active subscription.
> For instructions on authenticating, see [Log in with Azure CLI 2.0](authenticate-azure-cli.md). For information on subscription management, see [Manage Azure subscriptions with Azure CLI 2.0](manage-azure-subscriptions-azure-cli.md)

## Register a cloud

Register a new cloud if you have your own endpoints for Azure Stack. Creating a cloud is done with the [az cloud register](/cli/azure/cloud#az-cloud-register) command. This command requires a name and a set of capabilities with associated endpoints. To learn how to register a cloud for use with Azure Stack, see [Use API version profiles with Azure CLI 2.0 in Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-azurecli2#connect-to-azure-stack).

You do not need to register your own cloud for China, US Government, or German regions. These are managed by Microsoft and available by default.  For more information on all of the available endpoint settings, see the [documentation for `az cloud register`](/cli/azure/cloud#az-cloud-register).

Registering a cloud does not automatically switch to it. Use the `az cloud set` command to select the newly created cloud as described above.

## Update an existing cloud

If you have permissions, you can also update an existing cloud. Do this when you need to switch to a different Azure profile, add an endpoint, or change an endpoint.
You do this with the [az cloud update](/cli/azure/cloud#az-cloud-update) command, which takes the same arguments as `az cloud register`.

## Unregister a cloud

If you no longer require a registered cloud, it can be unregistered with the [az cloud unregister](/cli/azure/cloud#az-cloud-unregister) command:

```azurecli
az cloud unregister --name MyCloud
```
