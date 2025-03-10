---
author: probableprime
ms.service: azure-communication-services
ms.topic: include
ms.date: 06/30/2021
ms.author: rifox
---

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/dotnet/).
- Install [Azure CLI](/cli/azure/install-azure-cli-windows?tabs=azure-cli) 

Note that if you're planning on using phone numbers, you can't use the free trial account. Check that your subscription meets all the [requirements](../../concepts/telephony-sms/plan-solution.md) if you plan to purchase phone numbers before creating your resource. 

## Create Azure Communication Services resource

To create an Azure Communication Services resource, [sign in to Azure CLI](/cli/azure/authenticate-azure-cli). You can do this through the terminal using the ```az login``` command and providing your credentials. Run the following command to create the resource:

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

If you would like to select a specific subscription you can also specify the ```--subscription``` flag and provide the subscription ID.
```
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup> --subscription "<subscriptionID>"
```

You can configure your Communication Services resource with the following options:

* The resource group
* The name of the Communication Services resource
* The geography the resource will be associated with

In the next step, you can assign tags to the resource. Tags can be used to organize your Azure resources. See the [resource tagging documentation](../../../azure-resource-manager/management/tag-resources.md) for more information about tags.

## Manage your Communication Services resource

To add tags to your Communication Services resource, run the following commands. You can target a specific subscription as well.

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal1" --resource-group "<resourceGroup>"

az communication update --name "<communicationName>" --tags newTag="newVal2" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"
```

For information on additional commands, see [az communication](/cli/azure/communication).
