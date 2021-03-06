---
title: Шифрование безопасных ресурсов в службе автоматизации
description: Служба автоматизации Azure защищает защищенные ресурсы с помощью нескольких уровней шифрования. По умолчанию шифрование выполняется с помощью ключей, управляемых корпорацией Майкрософт. Клиенты могут настроить учетные записи службы автоматизации, чтобы использовать управляемые клиентом ключи для шифрования. В этой статье приводятся сведения о обоих режимах шифрования и о том, как можно переключаться между ними.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: e645be5ddd51a4fe7e7610e7f639407d5638f746
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2020
ms.locfileid: "75920932"
---
# <a name="secure-assets-in-azure-automation"></a>Защита ресурсов в службе автоматизации Azure

Безопасные средства в службе автоматизации Azure включают учетные данные, сертификаты, подключения и зашифрованные переменные. Эти ресурсы защищены в службе автоматизации Azure с использованием нескольких уровней шифрования. На основе ключа верхнего уровня, используемого для шифрования, существуют две модели шифрования:
-   Использование ключей, управляемых корпорацией Майкрософт
-   Использование управляемых клиентом ключей

## <a name="microsoft-managed-keys"></a>Ключи, управляемые корпорацией Майкрософт

По умолчанию учетная запись службы автоматизации Azure использует ключи, управляемые корпорацией Майкрософт.

Каждый защищенный ресурс шифруется и хранится в службе автоматизации Azure с помощью уникального ключа (ключа шифрования данных), который создается для каждой учетной записи службы автоматизации. Сами эти ключи шифруются и хранятся в службе автоматизации Azure с помощью еще одного уникального ключа, созданного для каждой учетной записи, которая называется ключом шифрования учетной записи (АЕК). Эти ключи шифрования учетной записи шифруются и хранятся в службе автоматизации Azure с помощью управляемых корпорацией Майкрософт ключей. 

## <a name="customer-managed-keys-with-key-vault-preview"></a>Ключи, управляемые клиентом, с Key Vault (Предварительная версия)

Вы можете управлять шифрованием защищенных ресурсов в службе автоматизации Azure на уровне учетной записи службы автоматизации с помощью собственных ключей. При указании ключа, управляемого клиентом, на уровне учетной записи службы автоматизации этот ключ используется для защиты и контроля доступа к ключу шифрования учетной записи службы автоматизации, который, в свою очередь, используется для шифрования и расшифровки всех защищенных ресурсов. Ключи, управляемые клиентом, обеспечивают большую гибкость при создании, повороте, отключении и отмене контроля доступа. Кроме того, можно выполнять аудит ключей шифрования, используемых для защиты безопасных ресурсов. 

Для хранения ключей, управляемых клиентом, необходимо использовать Azure Key Vault. Можно либо создать собственные ключи и сохранить их в хранилище ключей, либо использовать Azure Key Vault API для создания ключей.  Дополнительные сведения о хранилище ключей Azure см. в статье [Что такое хранилище ключей Azure?](../key-vault/key-vault-overview.md)

## <a name="enable-customer-managed-keys-for-an-automation-account"></a>Включение управляемых клиентом ключей для учетной записи службы автоматизации

Когда вы включаете шифрование с помощью управляемых клиентом ключей для учетной записи службы автоматизации, служба автоматизации Azure заключает ключ шифрования учетной записи в ключ, управляемый клиентом, в связанном хранилище ключей. Включение управляемых клиентом ключей не влияет на производительность, и учетная запись шифруется с новым ключом немедленно без задержки.

Новая учетная запись службы автоматизации всегда шифруется с помощью ключей, управляемых корпорацией Майкрософт. Невозможно включить управляемые клиентом ключи на момент создания учетной записи. Ключи, управляемые клиентом, хранятся в Azure Key Vault, а хранилище ключей должно быть подготовлено с политиками доступа, которые предоставляют ключевые разрешения для управляемого удостоверения, связанного с учетной записью службы автоматизации. Управляемое удостоверение доступно только после создания учетной записи хранения.

При изменении ключа, используемого для шифрования безопасного ресурса службы автоматизации Azure, путем включения или отключения ключей, управляемых клиентом, обновления ключа или указания другого ключа, шифрование ключа шифрования учетной записи изменяется, но защищенные активы в учетной записи службы автоматизации Azure не требуется повторное шифрование.

В следующих трех разделах описывается механизм включения управляемых клиентом ключей для учетной записи службы автоматизации. 

> [!NOTE] 
> Чтобы включить управляемые клиентом ключи, в настоящее время вам потребуется выполнить службы автоматизации Azure REST API вызовы с помощью API версии 2020-01-13-Preview.

### <a name="pre-requisites-for-using-customer-managed-keys-in-azure-automation"></a>Предварительные требования для использования управляемых клиентом ключей в службе автоматизации Azure

Перед включением ключей, управляемых клиентом, для учетной записи службы автоматизации необходимо убедиться, что выполнены следующие предварительные требования.

 - Ключ, управляемый клиентом, хранится в Azure Key Vault. 
 - Необходимо включить как **обратимое удаление** , так и **не очищать** свойства в хранилище ключей. Эти функции необходимы для восстановления ключей в случае случайного удаления.
 - С помощью шифрования службы автоматизации Azure поддерживаются только ключи RSA. Дополнительные сведения о ключах см. в разделе [сведения о ключах, секретах и сертификатах Azure Key Vault](../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).
- Учетная запись службы автоматизации и хранилище ключей могут находиться в разных подписках, но должны находиться в одном клиенте Azure Active Directory.

### <a name="assign-an-identity-to-the-automation-account"></a>Назначение удостоверения учетной записи службы автоматизации

Чтобы использовать управляемые клиентом ключи с учетной записью службы автоматизации, учетной записи службы автоматизации необходимо пройти проверку подлинности в keyvault, где хранятся ключи, управляемые клиентом. Служба автоматизации Azure использует управляемые удостоверения, назначенные системой, для проверки подлинности учетной записи с Key Vault. Дополнительные сведения об управляемых удостоверениях см. в статье [Что такое управляемые удостоверения для ресурсов Azure?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Настройте управляемое удостоверение, назначенное системой для учетной записи службы автоматизации, используя следующий вызов REST API

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```
Тело запроса
```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```    

В ответе возвращается удостоверение, назначенное системой для учетной записи службы автоматизации.

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 ..
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
..
}
```

### <a name="configure-the-key-vault-access-policy"></a>Настройка политики доступа Key Vault

После назначения управляемого удостоверения учетной записи службы автоматизации вы настраиваете доступ к Key Vault хранения ключей, управляемых клиентом. Службе автоматизации Azure требуются функции **Get**, **recover**, **wrapKey**, **UnwrapKey** на управляемых пользователем ключах.

Такую политику доступа можно задать с помощью следующего вызова REST API.

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```
Тело запроса

```json
{
  "properties": {
    "accessPolicies": [
      {
        "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "objectId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
        "permissions": {
          "keys": [
            "get",
            "recover",
            "wrapKey",
            "unwrapKey"
          ],
          "secrets": [],
          "certificates": []
        }
      }
    ]
  }
}
```

> [!NOTE] 
> Поля **tenantId** и **ObjectID** должны быть предоставлены со значениями **Identity. tenantId** и **Identity. principalId** соответственно из ответа управляемого удостоверения для учетной записи службы автоматизации.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Изменение конфигурации учетной записи службы автоматизации для использования управляемого клиентом ключа

Наконец, можно переключить учетную запись службы автоматизации с ключей, управляемых Microsft, на ключи, управляемые клиентом, используя следующий вызов REST API.

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```
Тело запроса

```json
 {
    "properties": {
      "encryption": {
        "keySource": "Microsoft.Keyvault",
        "keyvaultProperties": {
          "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
        }
      }
    }
  }
```
Пример ответа

```json
{
  "name": "automation-account-name",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
  ..
  "properties": {
    ..
    "encryption": {
      "keyvaultProperties": {
         "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
      },
      "keySource": "Microsoft.Keyvault"
    },
    ..
  }
}
```

## <a name="manage-customer-managed-keys-lifecycle"></a>Управление жизненным циклом ключей, управляемых клиентом

### <a name="rotate-customer-managed-keys"></a>Смена ключей, управляемых клиентом

Вы можете поворачивать ключ, управляемый клиентом, в Azure Key Vault в соответствии с политиками соответствия требованиям. При вращении ключа необходимо обновить учетную запись службы автоматизации, чтобы она использовала новый URI ключа. 

Вращение ключа не приводит к повторному шифрованию защищенных ресурсов в учетной записи службы автоматизации. От пользователя не требуется предпринимать никаких действий.

### <a name="revoke-access-to-customer-managed-keys"></a>Отозвать доступ к ключам, управляемым клиентом

Чтобы отозвать доступ к ключам, управляемым клиентом, используйте PowerShell или Azure CLI. Дополнительные сведения см. в разделе [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault/) или [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault). Отмена доступа обеспечивает доступ к защищенным ресурсам в учетной записи службы автоматизации, так как ключ шифрования недоступен в службе автоматизации Azure.

## <a name="next-steps"></a>Дальнейшие действия

- [Что такое хранилище ключей Azure?](../key-vault/key-vault-overview.md) 
- [Сертификация активов в службе автоматизации Azure](shared-resources/certificates.md)
- [Ресурсы учетных данных в службе автоматизации Azure](shared-resources/credentials.md)
- [Ресурсы переменных в службе автоматизации Azure](shared-resources/variables.md)
