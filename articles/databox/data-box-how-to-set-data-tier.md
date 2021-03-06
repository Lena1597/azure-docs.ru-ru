---
title: Использование Azure Data Box, Azure Data Box активное для отправки данных на "Горячий", "холодное", архивировать уровня BLOB-объекта | Документация Microsoft в данных
description: Описывает, как использовать Azure Data Box или большой поле данных Azure для отправки данных в уровень хранилища BLOB-объектов соответствующего блока, таких как "Горячий", "холодное" или архивировать
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 05/24/2019
ms.author: alkohli
ms.openlocfilehash: ea208c395e2ef69ce8f28052351643e963cceb05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427875"
---
# <a name="use-azure-data-box-or-azure-data-box-heavy-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Использование Azure Data Box или тяжелой поле данных Azure, для отправки данных в соответствующий уровень BLOB-объектов хранилища Azure

Служба Azure Data Box перемещает большие объемы данных в Azure, отправляя вам специальное устройство хранения. Заполните устройство данными и верните его. Данные из Data Box отправляются на уровень хранилища по умолчанию, связанный с учетной записью хранения. Затем вы можете переместить данные на другой уровень хранилища.

В этой статье описывается, как отправленные на Data Box данные можно переместить на горячий, холодный или архивный уровень большого двоичного объекта. Эта статья относится к Azure Data Box и Azure Data Box большой.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Выбор правильного уровня хранилища для данных

В службе хранилища Azure данные могут храниться наиболее экономически эффективным способом на трех разных уровнях — горячем, холодном или архивном. Горячий уровень хранилища оптимизирован для хранения часто используемых данных. Стоимость хранения данных на горячем уровне хранилища выше, чем на холодном и архивном уровнях. Но на этом уровне доступ к данным обходится дешевле.

Холодный уровень хранилища предназначен для редко используемых данных, которые должны храниться не менее 30 дней. Стоимость холодного уровня хранилища ниже горячего, однако плата за доступ к данным высокая, если сравнивать с горячим уровнем.

Архивный уровень Azure используется в автономном режиме. Он обеспечивает наименьшие затраты на хранение, но требует наибольших затрат на доступ. Этот уровень предназначен для данных, которые остаются в архивном хранилище в течение как минимум 180 дней. Дополнительные сведения о каждом из этих уровней и модели ценообразования см. в статье [о сравнении уровней хранилища](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

Данные из поле или поле данных в большой отправки к уровню хранилища, связанный с учетной записью хранения. При создании учетной записи хранения можно установить уровень доступа "горячий" или "холодный". В зависимости от шаблона доступа рабочей нагрузки и стоимости, можно переместить эти данные с уровня по умолчанию на другой уровень хранилища.

Данные хранилища объектов можно распределять в учетной записи хранилища BLOB-объектов либо в учетной записи общего назначения версии 2 (GPv2). Учетные записи общего назначения версии 1 (GPv1) не поддерживают распределение по уровням. Чтобы выбрать правильный уровень хранилища для своих данных, ознакомьтесь с вопросами, описанными в статье [Хранилище BLOB-объектов Azure: горячий, холодный, архивный уровни хранилища и уровень "Премиум" (предварительная версия)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Установка уровня большого двоичного объекта по умолчанию

Уровень большого двоичного объекта по умолчанию указывается при создании учетной записи хранения на портале Azure. Когда вы выберите тип хранилища как GPv2 или хранилище BLOB-объектов, можно указать атрибут уровня доступа. По умолчанию выбирается горячий уровень.

Уровни не может быть указан, если вы пытаетесь создать учетную запись, при упорядочивании данных поле или поле данных в большой. После создания учетной записи вы можете изменить ее на портале, чтобы установить уровень доступа по умолчанию.

Кроме того, вы можете сначала создать учетную запись хранения с указанным атрибутом уровня доступа. При создании заказа Data Box или большой поле данных выберите существующую учетную запись хранения. Дополнительные сведения по установке уровня хранения большого двоичного объекта во время создания учетной записи хранения см. в статье [Создание учетной записи хранения](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Перемещение данных на уровень, отличный от уровня по умолчанию

После загрузки данных из устройство Data Box на уровень по умолчанию можно переместить данные на уровне не по умолчанию. Существует два способа перемещения этих данных на уровень, отличный от уровня по умолчанию.

- **Управление жизненным циклом хранилища BLOB-объектов Azure**. Вы можете использовать основанный на политике подход для автоматического размещения данных на уровнях или удаления их по завершении жизненного цикла. Дополнительные сведения см. в статье [Управление жизненным циклом хранилища BLOB-объектов Azure (предварительная версия)](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Создание сценариев**. Вы можете выполнить сценарий через Azure PowerShell, чтобы включить распределение BLOB-объектов по уровням. Вы можете вызвать операцию `SetBlobTier`, чтобы задавать уровень большого двоичного объекта.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Использование Azure PowerShell для установки уровня большого двоичного объекта

Следующие шаги описывают, как можно задать архивный уровень хранилища для большого двоичного объекта с помощью сценария Azure PowerShell.

1. Откройте сеанс Windows PowerShell с повышенными привилегиями. Убедитесь, что у вас установлена PowerShell 5.0 или более поздней версии. Тип:

   `$PSVersionTable.PSVersion`     

2. Войдите в Azure PowerShell. 

   `Login-AzAccount`  

3. Определите переменные для учетной записи хранения, ключа доступа, контейнера и контекста хранилища.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Получите все большие двоичные объекты в контейнере.

    `$blobs = Get-AzStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Настроите архивный уровень для всех больших двоичных объектов в контейнере.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Результат выполнения команды показан ниже:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Если вам нужно, чтобы данные архивировались во время принятия, задайте горячий как уровень по умолчанию для учетной записи. Если уровень по умолчанию — "холодный" и данные немедленно перемещаются на архивный уровень, тогда штраф за досрочное удаление составляет 30 дней.

## <a name="next-steps"></a>Дальнейшие действия

-  Узнайте, как разрешать [распространенные сценарии распределения данных по уровням с помощью правил политики жизненного цикла](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples).

