---
title: Рекомендации по хранению для функций Azure
description: Узнайте о требованиях к хранению в службе "функции Azure" и о шифровании хранимых данных.
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: f094996ca44ec36d46330e54eac56b28794ef22e
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190300"
---
# <a name="storage-considerations-for-azure-functions"></a>Рекомендации по хранению для функций Azure

При создании экземпляра приложения-функции для функций Azure требуется учетная запись хранения Azure. Приложение функции может использовать следующие службы хранилища:


|Служба хранилища  | Использование функций  |
|---------|---------|
| [Хранилище BLOB-объектов Azure](../storage/blobs/storage-blobs-introduction.md)     | Поддерживать состояние привязок и функциональные ключи.  <br/>Также используется [центрами задач в устойчивые функции](durable/durable-functions-task-hubs.md). |
| [Файлы Azure](../storage/files/storage-files-introduction.md)  | Общая папка, используемая для хранения и запуска кода приложения-функции в [плане потребления](functions-scale.md#consumption-plan). |
| [хранилище очередей Azure](../storage/queues/storage-queues-introduction.md);     | Используется [центрами задач в устойчивые функции](durable/durable-functions-task-hubs.md).   |
| [Хранилище таблиц Azure](../storage/tables/table-storage-overview.md)  |  Используется [центрами задач в устойчивые функции](durable/durable-functions-task-hubs.md).       |

> [!IMPORTANT]
> При использовании плана потребления файлы кода и конфигурации привязок приложения-функции хранятся в хранилище файлов Azure в основной учетной записи хранения. При удалении основной учетной записи хранения это содержимое удаляется без возможности восстановления.

## <a name="storage-account-requirements"></a>Требования к учетной записи хранения

При создании приложения-функции необходимо создать или связать с учетной записью хранения Azure общего назначения, которая поддерживает хранилище BLOB-объектов, очередей и таблиц. Это связано с тем, что функции используют службу хранилища Azure для таких операций, как Управление триггерами и ведение журнала выполнения функций. Некоторые учетные записи хранения не поддерживают очереди и таблицы. Эти учетные записи включают учетные записи хранения только для больших двоичных объектов, хранилище Azure класса Premium и учетные записи хранения общего назначения с репликацией ZRS. При создании приложения-функции эти неподдерживаемые учетные записи отфильтровываются из колонки учетной записи хранения.

Дополнительные сведения о типах учетных записей хранения см. в разделе [Введение в службы хранилища Azure](../storage/common/storage-introduction.md#azure-storage-services). 

Хотя вы можете использовать существующую учетную запись хранения в приложении функции, необходимо убедиться, что она соответствует этим требованиям. Учетные записи хранения, созданные как часть процесса создания приложения функции, гарантированно удовлетворяют этим требованиям к учетной записи хранения.  

## <a name="storage-account-guidance"></a>Руководство по учетной записи хранения

Для работы каждого приложения-функции нужна учетная запись хранения. Если эта учетная запись удалена, приложение-функция не будет выполняться. Сведения об устранении проблем, связанных с хранилищем, см. [в разделе Устранение неполадок, связанных с хранилищем](functions-recover-storage-account.md). Следующие дополнительные рекомендации относятся к учетной записи хранения, используемой приложениями функций.

### <a name="storage-account-connection-setting"></a>Параметр подключения учетной записи хранения

Подключение к учетной записи хранения сохраняется в [параметре приложения AzureWebJobsStorage](./functions-app-settings.md#azurewebjobsstorage). 

При повторном создании ключей хранилища необходимо обновить строку подключения к учетной записи хранения. [Дополнительные сведения об управлении ключами хранилища](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)см. здесь.

### <a name="shared-storage-accounts"></a>Общие учетные записи хранения

Несколько приложений функций могут совместно использовать одну и ту же учетную запись хранения без каких бы то ни было проблем. Например, в Visual Studio можно разрабатывать несколько приложений с помощью эмулятора хранения Azure. В этом случае эмулятор действует как единая учетная запись хранения. Учетную запись хранения, используемую приложением функции, также можно использовать для хранения данных приложения. Однако этот подход не всегда хорош в рабочей среде.

### <a name="optimize-storage-performance"></a>Оптимизация производительности хранилища

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>Шифрование данных хранилища

Служба хранилища Azure шифрует все данные в неактивных учетных записях хранения. Дополнительные сведения см. [в статье шифрование службы хранилища Azure для неактивных данных](../storage/common/storage-service-encryption.md).

По умолчанию данные шифруются с помощью ключей, управляемых корпорацией Майкрософт. Для дополнительного управления ключами шифрования можно предоставить ключи, управляемые клиентом, для шифрования больших двоичных объектов и данных файлов. Эти ключи должны присутствовать в Azure Key Vault, чтобы функции могли получить доступ к учетной записи хранения. Дополнительные сведения см. в статье о [настройке ключей, управляемых клиентом, с помощью Azure Key Vault с помощью портал Azure](../storage/common/storage-encryption-keys-portal.md).  

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о параметрах размещения функций Azure.

> [!div class="nextstepaction"]
> [Масштабирование и размещение Функций Azure](functions-scale.md)


