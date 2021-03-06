---
title: Использование PowerShell для настройки ключей, управляемых клиентом
titleSuffix: Azure Storage
description: Узнайте, как использовать PowerShell для настройки ключей, управляемых клиентом, для шифрования службы хранилища Azure. Ключи, управляемые клиентом, позволяют создавать, поворачивать, отключать и отзывать элементы управления доступом.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/03/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 77324dff7e3f34574f36aa3bb775aed6a945a3bd
ms.sourcegitcommit: 2c59a05cb3975bede8134bc23e27db5e1f4eaa45
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2020
ms.locfileid: "75665280"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-powershell"></a>Настройка ключей, управляемых клиентом, с Azure Key Vault с помощью PowerShell

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

В этой статье показано, как настроить Azure Key Vault с управляемыми клиентом ключами с помощью PowerShell. Сведения о создании хранилища ключей с помощью Azure CLI см. в разделе [Краткое руководство. Настройка и извлечение секрета из Azure Key Vault с помощью PowerShell](../../key-vault/quick-create-powershell.md).

## <a name="assign-an-identity-to-the-storage-account"></a>Назначение удостоверения учетной записи хранения

Чтобы включить управляемые клиентом ключи для учетной записи хранения, сначала назначьте назначенное системой управляемое удостоверение учетной записи хранения. Вы будете использовать это управляемое удостоверение, чтобы предоставить учетной записи хранения разрешения на доступ к хранилищу ключей.

Чтобы назначить управляемое удостоверение с помощью PowerShell, вызовите [Set-азсторажеаккаунт](/powershell/module/az.storage/set-azstorageaccount). Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Дополнительные сведения о настройке управляемых удостоверений, назначенных системой с помощью PowerShell, см. [в статье Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Создание нового хранилища ключей

Чтобы создать новое хранилище ключей с помощью PowerShell, вызовите [New-азкэйваулт](/powershell/module/az.keyvault/new-azkeyvault). Хранилище ключей, используемое для хранения управляемых клиентом ключей для шифрования службы хранилища Azure, должно включать два параметра защиты ключей: **обратимое удаление** и **не очищать**.

Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

Сведения о том, как включить **обратимое удаление** и **не выполнять очистку** в существующем хранилище ключей с помощью PowerShell, см. в разделах **Включение обратимого удаления** и **Включение защиты от вирусов** в статье [Использование обратимого удаления с помощью PowerShell](../../key-vault/key-vault-soft-delete-powershell.md).

## <a name="configure-the-key-vault-access-policy"></a>Настройка политики доступа к хранилищу ключей

Затем настройте политику доступа для хранилища ключей, чтобы у учетной записи хранения были разрешения на доступ к ней. На этом шаге вы будете использовать управляемое удостоверение, назначенное ранее учетной записи хранения.

Чтобы задать политику доступа для хранилища ключей, вызовите [Set-азкэйваултакцессполици](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy). Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями и использовать переменные, определенные в предыдущих примерах.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Создание ключа

Затем создайте новый ключ в хранилище ключей. Чтобы создать новый ключ, вызовите [Add-азкэйваулткэй](/powershell/module/az.keyvault/add-azkeyvaultkey). Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями и использовать переменные, определенные в предыдущих примерах.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Настройка шифрования с помощью управляемых клиентом ключей

По умолчанию шифрование службы хранилища Azure использует ключи, управляемые корпорацией Майкрософт. На этом шаге настройте учетную запись хранения Azure для использования ключей, управляемых клиентом, и укажите ключ, связываемый с учетной записью хранения.

Вызовите [Set-азсторажеаккаунт](/powershell/module/az.storage/set-azstorageaccount) , чтобы обновить параметры шифрования учетной записи хранения, как показано в следующем примере. Включите параметр **-кэйваултенкриптион** , чтобы включить управляемые клиентом ключи для учетной записи хранения. Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями и использовать переменные, определенные в предыдущих примерах.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Обновление версии ключа

При создании новой версии ключа необходимо обновить учетную запись хранения, чтобы она использовала новую версию. Сначала вызовите [Get-азкэйваулткэй](/powershell/module/az.keyvault/get-azkeyvaultkey) , чтобы получить последнюю версию ключа. Затем вызовите [Set-азсторажеаккаунт](/powershell/module/az.storage/set-azstorageaccount) , чтобы обновить параметры шифрования учетной записи хранения для использования новой версии ключа, как показано в предыдущем разделе.

## <a name="use-a-different-key"></a>Использовать другой ключ

Чтобы изменить ключ, используемый для шифрования службы хранилища Azure, вызовите [Set-азсторажеаккаунт](/powershell/module/az.storage/set-azstorageaccount) , как показано в разделе [Настройка шифрования с помощью управляемых клиентом ключей](#configure-encryption-with-customer-managed-keys) и предоставление нового имени и версии ключа. Если новый ключ находится в другом хранилище ключей, также обновите URI хранилища ключей.

## <a name="disable-customer-managed-keys"></a>Отключение ключей, управляемых клиентом

При отключении управляемых пользователем ключей учетная запись хранения шифруется с помощью ключей, управляемых корпорацией Майкрософт. Чтобы отключить управляемые клиентом ключи, вызовите [Set-азсторажеаккаунт](/powershell/module/az.storage/set-azstorageaccount) с параметром `-StorageEncryption`, как показано в следующем примере. Не забудьте заменить значения заполнителей в квадратных скобках собственными значениями и использовать переменные, определенные в предыдущих примерах.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -StorageEncryption  
```

## <a name="next-steps"></a>Дальнейшие действия

- [Шифрование неактивных данных в службе хранилища Azure](storage-service-encryption.md)
- [Что такое Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
