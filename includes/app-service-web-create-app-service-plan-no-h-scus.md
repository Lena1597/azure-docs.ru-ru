---
title: включение файла
description: включение файла
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
ms.date: 09/18/2018
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: 89a97196f14332ec1812dc3e8a16905af1aa4407
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185773"
---
В Cloud Shell создайте план службы приложений с помощью команды [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create).

В следующем примере создается план службы приложений с именем `myAppServicePlan` и ценовой категорией **Бесплатный**.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

После создания плана службы приложений в Azure CLI отображается информация следующего вида:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
