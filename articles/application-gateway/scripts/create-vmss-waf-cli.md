---
title: Пример скрипта Azure CLI. Ограничение веб-трафика | Документация Майкрософт
description: Пример скрипта Azure CLI. Создание шлюза приложения с брандмауэром веб-приложения и масштабируемым набором виртуальных машин, использующим правила OWASP для ограничения трафика.
services: application-gateway
documentationcenter: networking
author: vhorne
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: e9d2a8a8d47128161c10e81fe70a950f1ed81e1e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2020
ms.locfileid: "76273383"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Ограничение веб-трафика с помощью Azure CLI

Этот скрипт создает шлюз приложений с брандмауэром веб-приложения, в котором используется масштабируемый набор виртуальных машин для внутренних серверов. Брандмауэр веб-приложения ограничивает веб-трафик на основе правил OWASP. Выполнив скрипт, вы можете проверить шлюз приложений с помощью его общедоступного IP-адреса.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Очистка развертывания 

Выполните следующую команду, чтобы удалить группу ресурсов, шлюз приложений и все связанные ресурсы.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Описание скрипта

Чтобы создать развертывание, скрипт использует следующие команды. Для каждого элемента в таблице приведены ссылки на документацию по команде.

| Get-Help | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Создает виртуальную сеть. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Создает подсеть в виртуальной сети. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Создает общедоступный IP-адрес для шлюза приложений. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Создание шлюза приложений. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Создает масштабируемый набор виртуальных машин. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Создание учетной записи хранения. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Создание учетной записи хранения. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show) | Получает общедоступный IP-адрес для шлюза приложений. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Другие примеры скриптов CLI для шлюза приложений см. в [документации по шлюзу приложений Azure](../cli-samples.md).
