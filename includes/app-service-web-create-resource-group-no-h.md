---
title: включение файла
description: включение файла
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185768"
---
[!INCLUDE [resource group intro text](resource-group.md)]

В Cloud Shell создайте группу ресурсов с помощью команды [`az group create`](/cli/azure/group?view=azure-cli-latest). В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *Западная Европа*. Чтобы просмотреть все поддерживаемые расположения для службы приложений уровня **Бесплатный**, выполните команду [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Обычно группа ресурсов и ресурсы создаются в ближайших регионах. 

По завершении команды в выходных данных JSON будут отображаться свойства группы ресурсов.