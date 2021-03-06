---
title: Создание учетной записи, поддерживающей управляемые клиентом ключи для таблиц и очередей
titleSuffix: Azure Storage
description: Узнайте, как создать учетную запись хранения, которая поддерживает настройку ключей, управляемых клиентом, для таблиц и очередей. Используйте Azure CLI или шаблон Azure Resource Manager, чтобы создать учетную запись хранения, которая использует ключ шифрования учетной записи для шифрования службы хранилища Azure. Затем можно настроить ключи, управляемые клиентом, для учетной записи.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/05/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 09558a8d1e4e2dc68cefd2c870f54e008d10b97b
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2020
ms.locfileid: "77083554"
---
# <a name="create-an-account-that-supports-customer-managed-keys-for-tables-and-queues"></a>Создание учетной записи, поддерживающей управляемые клиентом ключи для таблиц и очередей

Служба хранилища Azure шифрует все данные в неактивных учетных записях хранения. По умолчанию хранилище очередей и хранилище таблиц используют ключ, относящийся к службе и управляемый корпорацией Майкрософт. Вы также можете использовать управляемые клиентом ключи для шифрования данных очереди или таблицы. Чтобы использовать управляемые клиентом ключи с очередями и таблицами, сначала необходимо создать учетную запись хранения, которая использует ключ шифрования, заданный в области действия учетной записи, а не в службе. После создания учетной записи, использующей ключ шифрования учетной записи для данных очередей и таблиц, можно настроить ключи, управляемые клиентом, с помощью Azure Key Vault для этой учетной записи хранения.

В этой статье описывается, как создать учетную запись хранения, которая зависит от ключа, относящегося к учетной записи. При первом создании учетной записи Корпорация Майкрософт использует ключ учетной записи для шифрования данных в учетной записи, и корпорация Майкрософт управляет ключом. Впоследствии можно настроить управляемые клиентом ключи для учетной записи, чтобы воспользоваться преимуществами этих преимуществ, включая возможность предоставления собственных ключей, обновления ключа, смены ключей и отзыва элементов управления доступом.

## <a name="about-the-feature"></a>О функции

Чтобы создать учетную запись хранения, которая использует ключ шифрования учетной записи для хранилища очередей и таблиц, сначала необходимо зарегистрироваться, чтобы использовать эту функцию в Azure. В связи с ограниченной емкостью имейте в виду, что до утверждения запросов на доступ может пройти несколько месяцев.

Вы можете создать учетную запись хранения, которая использует ключ шифрования учетной записи для хранилища очередей и таблиц в следующих регионах:

- Восточная часть США
- Центрально-южная часть США
- западная часть США 2  

### <a name="register-to-use-the-account-encryption-key"></a>Зарегистрируйтесь, чтобы использовать ключ шифрования учетной записи

Чтобы зарегистрироваться для использования ключа шифрования учетной записи с хранилищем очередей или таблиц, используйте PowerShell или Azure CLI.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Чтобы зарегистрироваться в PowerShell, вызовите команду [Get-азпровидерфеатуре](/powershell/module/az.resources/get-azproviderfeature) .

```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForQueues
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForTables
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы зарегистрироваться в Azure CLI, вызовите команду [AZ Feature Register](/cli/azure/feature#az-feature-register) .

```azurecli
az feature register --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForQueues
az feature register --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForTables
```

# <a name="templatetabtemplate"></a>[Шаблон](#tab/template)

Недоступно

---

### <a name="check-the-status-of-your-registration"></a>Проверка состояния регистрации

Чтобы проверить состояние регистрации для хранилища очередей или таблиц, используйте PowerShell или Azure CLI.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Чтобы проверить состояние регистрации с помощью PowerShell, вызовите команду [Get-азпровидерфеатуре](/powershell/module/az.resources/get-azproviderfeature) .

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForQueues
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForTables
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы проверить состояние регистрации с помощью Azure CLI, вызовите команду [AZ Feature](/cli/azure/feature#az-feature-show) .

```azurecli
az feature show --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForQueues
az feature show --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForTables
```

# <a name="templatetabtemplate"></a>[Шаблон](#tab/template)

Недоступно

---

### <a name="re-register-the-azure-storage-resource-provider"></a>Повторная регистрация поставщика ресурсов службы хранилища Azure

После утверждения регистрации необходимо повторно зарегистрировать поставщик ресурсов службы хранилища Azure. Используйте PowerShell или Azure CLI для повторной регистрации поставщика ресурсов.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Чтобы повторно зарегистрировать поставщик ресурсов с помощью PowerShell, вызовите команду [Register-азресаурцепровидер](/powershell/module/az.resources/register-azresourceprovider) .

```powershell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Storage'
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы повторно зарегистрировать поставщик ресурсов с Azure CLI, вызовите команду [AZ Provider Register](/cli/azure/provider#az-provider-register) .

```azurecli
az provider register --namespace 'Microsoft.Storage'
```

# <a name="templatetabtemplate"></a>[Шаблон](#tab/template)

Недоступно

---

## <a name="create-an-account-that-uses-the-account-encryption-key"></a>Создание учетной записи, использующей ключ шифрования учетной записи

Необходимо настроить новую учетную запись хранения, чтобы использовать ключ шифрования учетной записи для очередей и таблиц во время создания учетной записи хранения. Область действия ключа шифрования не может быть изменена после создания учетной записи.

Учетная запись хранения должна иметь тип общего назначения v2. Вы можете создать учетную запись хранения и настроить ее для использования ключа шифрования учетной записи с помощью Azure CLI или шаблона Azure Resource Manager.

> [!NOTE]
> При необходимости можно настроить шифрование данных с помощью ключа шифрования учетной записи при создании учетной записи хранения. Хранилище BLOB-объектов и служба файлов Azure всегда используют ключ шифрования учетной записи для шифрования данных.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Чтобы использовать PowerShell для создания учетной записи хранения, которая использует ключ шифрования учетной записи, убедитесь, что установлен модуль Azure PowerShell версии 3.4.0 или более поздней. Дополнительные сведения см. [в разделе Установка модуля Azure PowerShell](/powershell/azure/install-az-ps).

Затем создайте учетную запись хранения общего назначения версии 2, вызвав команду [New-азсторажеаккаунт](/powershell/module/az.storage/new-azstorageaccount) с соответствующими параметрами:

- Включите параметр `-EncryptionKeyTypeForQueue` и присвойте ему значение `Account`, чтобы использовать ключ шифрования учетной записи для шифрования данных в хранилище очередей.
- Включите параметр `-EncryptionKeyTypeForTable` и задайте для его значения значение `Account`, чтобы использовать ключ шифрования учетной записи для шифрования данных в табличном хранилище.

В следующем примере показано, как создать учетную запись хранения общего назначения версии 2, настроенную для геоизбыточного хранилища с доступом на чтение (RA-GRS) и использующего ключ шифрования учетной записи для шифрования данных как в очереди, так и в хранилище таблиц. Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями:

```powershell
New-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage-account> `
    -Location <location> `
    -SkuName "Standard_RAGRS" `
    -Kind StorageV2 `
    -EncryptionKeyTypeForTable Account `
    -EncryptionKeyTypeForQueue Account
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы использовать Azure CLI для создания учетной записи хранения, которая использует ключ шифрования учетной записи, убедитесь, что установлена версия Azure CLI 2.0.80 или более поздняя. Дополнительные сведения см. в статье [Установка Azure CLI](/cli/azure/install-azure-cli).

Затем создайте учетную запись хранения общего назначения версии 2, вызвав команду [AZ Storage Account Create](/cli/azure/storage/account#az-storage-account-create) с соответствующими параметрами:

- Включите параметр `--encryption-key-type-for-queue` и присвойте ему значение `Account`, чтобы использовать ключ шифрования учетной записи для шифрования данных в хранилище очередей.
- Включите параметр `--encryption-key-type-for-table` и задайте для его значения значение `Account`, чтобы использовать ключ шифрования учетной записи для шифрования данных в табличном хранилище.

В следующем примере показано, как создать учетную запись хранения общего назначения версии 2, настроенную для геоизбыточного хранилища с доступом на чтение (RA-GRS) и использующего ключ шифрования учетной записи для шифрования данных как в очереди, так и в хранилище таблиц. Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_RAGRS \
    --kind StorageV2 \
    --encryption-key-type-for-table Account \
    --encryption-key-type-for-queue Account
```

# <a name="templatetabtemplate"></a>[Шаблон](#tab/template)

В следующем примере JSON создается учетная запись хранения общего назначения версии 2, настроенная для геоизбыточного хранилища с доступом на чтение (RA-GRS), которая использует ключ шифрования учетной записи для шифрования данных как в очереди, так и в хранилище таблиц. Не забудьте заменить значения заполнителей в угловых скобках собственными значениями:

```json
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2019-06-01",
        "name": "[parameters('<storage-account>')]",
        "location": "[parameters('<location>')]",
        "dependsOn": [],
        "tags": {},
        "sku": {
            "name": "[parameters('Standard_RAGRS')]"
        },
        "kind": "[parameters('StorageV2')]",
        "properties": {
            "accessTier": "[parameters('<accessTier>')]",
            "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]",
            "largeFileSharesState": "[parameters('<largeFileSharesState>')]",
            "encryption": {
                "services": {
                    "queue": {
                        "keyType": "Account"
                    },
                    "table": {
                        "keyType": "Account"
                    }
                },
                "keySource": "Microsoft.Storage"
            }
        }
    }
],
```

---

После создания учетной записи, которая использует ключ шифрования учетной записи, ознакомьтесь с одной из следующих статей, чтобы настроить ключи, управляемые клиентом, с помощью Azure Key Vault.

- [Настройка ключей, управляемых клиентом, с помощью Azure Key Vault портал Azure](storage-encryption-keys-portal.md)
- [Настройка ключей, управляемых клиентом, с Azure Key Vault с помощью PowerShell](storage-encryption-keys-powershell.md)
- [Настройка ключей, управляемых клиентом, с помощью Azure Key Vault Azure CLI](storage-encryption-keys-cli.md)

## <a name="verify-the-account-encryption-key"></a>Проверка ключа шифрования учетной записи

Чтобы убедиться, что служба в учетной записи хранения использует ключ шифрования учетной записи, вызовите команду Azure CLI [AZ Storage Account](/cli/azure/storage/account#az-storage-account-show) . Эта команда возвращает набор свойств учетной записи хранения и их значений. Найдите поле `keyType` для каждой службы в свойстве encryption и убедитесь, что оно имеет значение `Account`.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Чтобы убедиться, что служба в учетной записи хранения использует ключ шифрования учетной записи, вызовите команду [Get-азсторажеаккаунт](/powershell/module/az.storage/get-azstorageaccount) . Эта команда возвращает набор свойств учетной записи хранения и их значений. Найдите поле `KeyType` для каждой службы в свойстве `Encryption` и убедитесь, что оно имеет значение `Account`.

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
$account.Encryption.Services.Queue
$account.Encryption.Services.Table
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Чтобы убедиться, что служба в учетной записи хранения использует ключ шифрования учетной записи, вызовите команду [AZ Storage Account](/cli/azure/storage/account#az-storage-account-show) . Эта команда возвращает набор свойств учетной записи хранения и их значений. Найдите поле `keyType` для каждой службы в свойстве encryption и убедитесь, что оно имеет значение `Account`.

```azurecli
az storage account show /
    --name <storage-account> /
    --resource-group <resource-group>
```

# <a name="templatetabtemplate"></a>[Шаблон](#tab/template)

Недоступно

---

## <a name="next-steps"></a>Дальнейшие действия

- [Шифрование неактивных данных в службе хранилища Azure](storage-service-encryption.md) 
- [Что такое Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
