---
title: Настройка ключей, управляемых клиентом, в Azure обозреватель данных с помощью шаблона Azure Resource Manager
description: В этой статье описывается, как настроить шифрование ключей, управляемых клиентом, на основе данных в Azure обозреватель данных с помощью шаблона Azure Resource Manager.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: d0f7085342f972f227fc549c423672296697d7de
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281263"
---
# <a name="configure-customer-managed-keys-using-the-azure-resource-manager-template"></a>Настройка ключей, управляемых клиентом, с помощью шаблона Azure Resource Manager

> [!div class="op_single_selector"]
> * [C#](customer-managed-keys-csharp.md)
> * [Шаблон Azure Resource Manager](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>Настройка шифрования с помощью управляемых клиентом ключей

В этом разделе вы настраиваете управляемые клиентом ключи с помощью шаблонов Azure Resource Manager. По умолчанию в Azure обозреватель данных Encryption используются ключи, управляемые корпорацией Майкрософт. На этом шаге настройте кластер Azure обозреватель данных для использования ключей, управляемых клиентом, и укажите ключ, связываемый с кластером.

Шаблон Azure Resource Manager можно развернуть с помощью портал Azure или с помощью PowerShell.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
      "metadata": {
        "description": "Name of the cluster to create"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "[parameters('clusterName')]",
      "type": "Microsoft.Kusto/clusters",
      "sku": {
        "name": "Standard_D13_v2",
        "tier": "Standard",
        "capacity": 2
      },
      "apiVersion": "2019-09-07",
      "location": "[parameters('location')]",
      "properties": {
        "keyVaultProperties": {
          "keyVaultUri": "<keyVaultUri>",
          "keyName": "<keyName>",
          "keyVersion": "<keyVersion"
        }
      }
    }
  ]
}
```

## <a name="update-the-key-version"></a>Обновление версии ключа

При создании новой версии ключа необходимо обновить кластер для использования новой версии. Сначала вызовите `Get-AzKeyVaultKey`, чтобы получить последнюю версию ключа. Затем обновите свойства хранилища ключей кластера, чтобы использовать новую версию ключа, как показано в разделе [Настройка шифрования с помощью управляемых клиентом ключей](#configure-encryption-with-customer-managed-keys).

## <a name="next-steps"></a>Дальнейшие действия

* [Защита кластеров Azure обозреватель данных в Azure](security.md)
* [Настройка управляемых удостоверений для кластера Azure обозреватель данных](managed-identities.md)
* [Защитите свой кластер в Azure обозреватель данных — портал Azure](manage-cluster-security.md) , включив шифрование неактивных компонентов.
* [Настройка ключей, управляемых клиентом, с помощьюC#](customer-managed-keys-csharp.md)

