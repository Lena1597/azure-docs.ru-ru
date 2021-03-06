---
title: Копирование и преобразование данных в Azure Cosmos DB (API SQL)
description: Узнайте, как копировать данные в Azure Cosmos DB (API SQL) и обратно, а затем преобразовывать данные в Azure Cosmos DB (API SQL) с помощью фабрики данных.
services: data-factory, cosmosdb
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/11/2019
ms.openlocfilehash: 6e9e1d54599ab88092638762ccd7974e44c82cbf
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025815"
---
# <a name="copy-and-transform-data-in-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Копирование и преобразование данных в Azure Cosmos DB (API SQL) с помощью фабрики данных Azure

> [!div class="op_single_selector" title1="Выберите используемую версию службы "Фабрика данных":"]
> * [Версия 1](v1/data-factory-azure-documentdb-connector.md)
> * [Текущая версия](connector-azure-cosmos-db.md)

В этой статье описано, как использовать действие копирования в Фабрике данных Azure, чтобы копировать данные в Azure Cosmos DB (API SQL) и оттуда, а также использовать Поток данных для преобразования данных в Azure Cosmos DB (API SQL). Дополнительные сведения о Фабрике данных Azure см. во [вводной статье](introduction.md).

>[!NOTE]
>Этот соединитель поддерживает только Cosmos DB API SQL. Сведения о MongoDB см. в статье [Копирование данных в API службы Azure Cosmos DB для MongoDB или из него с помощью Фабрики данных Azure](connector-azure-cosmos-db-mongodb-api.md). Другие типы API сейчас не поддерживаются.

## <a name="supported-capabilities"></a>Поддерживаемые возможности

Этот соединитель Azure Cosmos DB (API SQL) поддерживается для следующих действий:

- [Действие копирования](copy-activity-overview.md) с [поддерживаемой матрицей источника и приемника](copy-activity-overview.md)
- [Поток данных сопоставления](concepts-data-flow-overview.md)
- [Действие поиска](control-flow-lookup-activity.md)

Для действия копирования этот соединитель Azure Cosmos DB (API SQL) поддерживает:

- Копирование данных из службы Azure Cosmos DB и в нее с помощью [API SQL](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Запись данных в Azure Cosmos DB с помощью операции **INSERT** или **UPSERT**.
- Импорт и экспорт документов JSON "как есть" либо копирование данных из набора табличных данных или в него. К примерам можно отнести базу данных SQL и CSV-файл. Сведения о копировании документов "как есть" в или из JSON-файлов или в другую Azure Cosmos DB коллекции см. в разделе [Импорт и экспорт документов JSON](#import-and-export-json-documents).

Фабрика данных интегрируется с [библиотекой массового исполнителя Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started), чтобы обеспечить оптимальную производительность при операциях записи в Azure Cosmos DB.

> [!TIP]
> [Видео о переносе данных](https://youtu.be/5-SRNiC_qOU) поможет вам выполнить копирование данных из хранилища BLOB-объектов Azure в Azure Cosmos DB. Кроме того, в видео приведены общие рекомендации по настройке производительности приема данных в Azure Cosmos DB.

## <a name="get-started"></a>Начать

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Следующие разделы содержат сведения о свойствах, которые можно использовать для определения сущностей фабрики данных, характерных для Azure Cosmos DB (SQL API).

## <a name="linked-service-properties"></a>Свойства связанной службы

Для связанной службы Azure Cosmos DB (SQL API) поддерживаются следующие свойства.

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Для свойства **type** необходимо задать значение: **CosmosDb**. | Да |
| connectionString |Укажите сведения, необходимые для подключения к базе данных Azure Cosmos DB.<br />**Примечание**. Необходимо указать сведения о базе данных в строке подключения, как показано в приведенных ниже примерах. <br/> Вы можете также поместить ключ учетной записи в Azure Key Vault и извлечь конфигурацию `accountKey` из строки подключения. Ознакомьтесь с приведенными ниже примерами и подробными сведениями в статье [Хранение учетных данных в Azure Key Vault](store-credentials-in-key-vault.md). |Да |
| connectVia | [Среда выполнения интеграции](concepts-integration-runtime.md), используемая для подключения к хранилищу данных. Вы можете использовать Azure Integration Runtime или локальную среду IR (если хранилище данных расположено в частной сети). Если это свойство не задано, используется Azure Integration Runtime по умолчанию. |Нет |

**Пример**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Пример: ключ учетной записи хранения в Azure Key Vault**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;Database=<Database>",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных

Полный список разделов и свойств, используемых для определения наборов данных, приведен в статье [Наборы данных и связанные службы в фабрике данных Azure](concepts-datasets-linked-services.md).

Для набора данных Azure Cosmos DB (SQL API) поддерживаются следующие свойства: 

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство **Type** набора данных должно иметь значение **космосдбсклапиколлектион**. |Да |
| collectionName |Имя коллекции документов Azure Cosmos DB. |Да |

Если используется тип данных "DocumentDbCollection", он по-прежнему поддерживается как есть для обратной совместимости операций копирования и поиска, но не поддерживается для потока данных. Рекомендуется использовать новую модель.

**Пример**

```json
{
    "name": "CosmosDbSQLAPIDataset",
    "properties": {
        "type": "CosmosDbSqlApiCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [],
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования

Этот раздел содержит список свойств, поддерживаемых источником и приемником Azure Cosmos DB (SQL API). Полный список разделов и свойств, доступных для определения действий, см. в статье, посвященной [конвейерам и действиям в Фабрике данных Azure](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-sql-api-as-source"></a>Azure Cosmos DB (SQL API) как источник

Чтобы скопировать данные из Azure Cosmos DB (SQL API), в действии копирования в качестве типа **source** задайте значение **DocumentDbCollectionSource**. 

В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство **Type** источника действия копирования должно иметь значение **космосдбсклаписаурце**. |Да |
| query |Укажите запрос Azure Cosmos DB для чтения данных.<br/><br/>Пример:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Нет <br/><br/>Если не указано, то выполняется инструкция SQL `select <columns defined in structure> from mycollection`. |
| преферредрегионс | Предпочтительный список регионов для подключения при получении данных из Cosmos DB. | Нет |
| pageSize | Число документов на страницу результата запроса. Значение по умолчанию — "-1", что означает использование динамического размера страницы на стороне службы до 1000. | Нет |

Если используется источник типов "DocumentDbCollectionSource", он по-прежнему поддерживается как есть для обратной совместимости. Рекомендуется использовать новую модель, которая пересылается, чтобы обеспечить более широкие возможности копирования данных из Cosmos DB.

**Пример**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB SQL API input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "CosmosDbSqlApiSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"",
                "preferredRegions": [
                    "East US"
                ]
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

При копировании данных из Cosmos DB, если не требуется [экспортировать документы JSON "как есть](#import-and-export-json-documents)", рекомендуется указать сопоставление в действии копирования. Фабрика данных учитывает сопоставление, указанное в действии. Если строка не содержит значения для столбца, для значения столбца предоставляется значение null. Если сопоставление не указано, фабрика данных определяет схему с помощью первой строки данных. Если первая строка не содержит полную схему, некоторые столбцы будут отсутствовать в результате операции действия.

### <a name="azure-cosmos-db-sql-api-as-sink"></a>Azure Cosmos DB (SQL API) как приемник

Чтобы скопировать данные в Azure Cosmos DB (SQL API), в действии копирования задайте для типа **sink** значение **DocumentDbCollectionSink**. 

В разделе **source** действия копирования поддерживаются следующие свойства:

| Свойство | Description | Обязательно для заполнения |
|:--- |:--- |:--- |
| type | Свойство **Type** приемника действия копирования должно иметь значение **космосдбсклаписинк**. |Да |
| writeBehavior |Описывает способ записи данных в Azure Cosmos DB. Допустимые значения: **insert** и **upsert**.<br/><br/>Поведение **upsert** — замена документа, если документ с таким идентификатором уже существует. В противном выполняется вставка документа.<br /><br />**Примечание**. Фабрика данных автоматически создает идентификатор для документа, если идентификатор не указан в исходном документе или с помощью сопоставления столбцов. Это означает, что для правильной работы **upsert** у документа должен быть идентификатор. |Нет<br />(По умолчанию используется **insert**.) |
| writeBatchSize | Фабрика данных использует [библиотеку массового исполнителя Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) для записи данных в Azure Cosmos DB. Свойство **writeBatchSize** определяет размер документов, предоставляемых ADF в библиотеку. Вы можете увеличить значение **writeBatchSize** для повышения производительности или уменьшить значение, если документ большого размера. См. рекомендации ниже. |Нет<br />(Значение по умолчанию — **10 000**.) |
| дисаблеметриксколлектион | Фабрика данных собирает такие метрики, как Cosmos DB RUs для оптимизации производительности и рекомендаций по копированию. Если вы отвечаете за такое поведение, укажите `true`, чтобы отключить его. | Нет (значение по умолчанию — `false`) |

>[!TIP]
>Чтобы импортировать документы JSON "как есть", см. раздел [Импорт или экспорт документов JSON](#import-and-export-json-documents) . сведения о копировании из табличных данных см. в разделе [Миграция из реляционной базы данных в Cosmos DB](#migrate-from-relational-database-to-cosmos-db).

>[!TIP]
>В Cosmos DB размер одного запроса не должен превышать 2 МБ. Формула выглядит так: размер запроса = размера одного документа * размер пакета для записи. Если появится ошибка **Запрос слишком большой**, **уменьшите значение `writeBatchSize`** в конфигурации приемника действия копирования.

Если используется источник типов "DocumentDbCollectionSink", он по-прежнему поддерживается как есть для обратной совместимости. Рекомендуется использовать новую модель, которая пересылается, чтобы обеспечить более широкие возможности копирования данных из Cosmos DB.

**Пример**

```json
"activities":[
    {
        "name": "CopyToCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "CosmosDbSqlApiSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

### <a name="schema-mapping"></a>Сопоставление схем

Чтобы скопировать данные из Azure Cosmos DB в табличный приемник или обратно, см. [сопоставление схемы](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="mapping-data-flow-properties"></a>Сопоставление свойств потока данных

При преобразовании данных в потоке сопоставления данные можно считывать и записывать в коллекции Cosmos DB. Дополнительные сведения см. в разделе Преобразование [исходного кода](data-flow-source.md) и [приемника](data-flow-sink.md) при сопоставлении потоков данных.

### <a name="source-transformation"></a>Преобразование источника

Параметры, относящиеся к Azure Cosmos DB, доступны на вкладке **Параметры источника** преобразования «источник». 

**Включить системные столбцы:** Если значение true, ```id```, ```_ts```и другие системные столбцы будут включаться в метаданные потока данных из CosmosDB. При обновлении коллекций важно включить этот параметр, чтобы можно было взять существующий идентификатор строки.

**Размер страницы:** Число документов на страницу результата запроса. Значение по умолчанию — "-1", которое использует динамическую страницу службы до 1000.

**Пропускная способность:** Задайте необязательное значение для числа RUs, которое вы хотите применить к коллекции CosmosDB для каждого выполнения потока данных во время операции чтения. Минимум — 400.

**Предпочтительные регионы:** Выберите Предпочтительные регионы чтения для этого процесса.

#### <a name="json-settings"></a>Параметры JSON

**Один документ:** Выберите этот параметр, если ADF обрабатывает весь файл как отдельный документ JSON.

**Имена столбцов, не заключенные в кавычки:** Выберите этот параметр, если имена столбцов в JSON не заключены в кавычки.

**Содержит комментарии:** Используйте этот вариант, если документы JSON содержат комментарии в данных.

**Одинарные кавычки:** Этот параметр следует выбрать, если столбцы и значения в документе заключены в кавычки с одинарными кавычками.

**Обратная косая черта:** Если для экранирования символов в JSON используется обратная косая черта, выберите этот параметр.

### <a name="sink-transformation"></a>Преобразование приемника

Параметры, относящиеся к Azure Cosmos DB, доступны на вкладке **Параметры** преобразования приемника.

**Метод обновления:** Определяет, какие операции разрешены в назначении базы данных. По умолчанию разрешены только вставки. Для обновления, Upsert или удаления строк необходимо преобразование «изменение строки» для добавления тегов в строки для этих действий. Для обновлений, операции Upsert и DELETE необходимо задать ключевой столбец или столбцы, чтобы определить, какая строка будет изменена.

**Действие сбора:** Определяет, следует ли повторно создавать целевую коллекцию перед записью.
* Нет: никакие действия в коллекцию не выполняются.
* Повторное создание: коллекция будет удалена и создана повторно

**Размер пакета**: определяет, сколько строк записывается в каждом контейнере. Более крупные размеры пакетов улучшают сжатие и оптимизацию памяти, но при кэшировании данных возникает риск нехватки памяти.

**Ключ секции:** Введите строку, представляющую ключ секции для коллекции. Например, ```/movies/title```.

**Пропускная способность:** Задайте необязательное значение для числа RUs, которое вы хотите применить к коллекции CosmosDB для каждого выполнения этого потока данных. Минимум — 400.

**Бюджет пропускной способности записи:** Целое число, представляющее количество, которое необходимо выделить для задания массового приема Spark. Это число выходит за пределы общей пропускной способности, выделенной для коллекции.

## <a name="lookup-activity-properties"></a>Свойства действия поиска

Чтобы получить сведения о свойствах, проверьте [действие поиска](control-flow-lookup-activity.md).

## <a name="import-and-export-json-documents"></a>Импорт и экспорт документов JSON

Соединитель Azure Cosmos DB (SQL API) помогает легко:

* копировать документы между двумя коллекциями Azure Cosmos DB "как есть".
* импортировать документы JSON из различных источников в Azure Cosmos DB, включая хранилище BLOB-объектов Azure, Azure Data Lake Store или другие файловые хранилища, поддерживаемые Фабрикой данных Azure;
* экспортировать документы JSON из коллекции Azure Cosmos DB в различные файловые хранилища;

Чтоб обеспечить копирование без использования схем, сделайте следующее.

* При использовании инструмента "Копирование данных" установите флажок **Export as-is to JSON files or DocumentDB collection** (Экспортировать "как есть" в JSON-файлы или коллекцию Cosmos DB).
* При использовании создания действия выберите формат JSON с соответствующим хранилищем файлов для источника или приемника.

## <a name="migrate-from-relational-database-to-cosmos-db"></a>Миграция из реляционной базы данных в Cosmos DB

При переходе с реляционной базы данных, например SQL Server на Azure Cosmos DB, действие копирования может легко сопоставлять табличные данные из источника для спрямления документов JSON в Cosmos DB. В некоторых случаях может потребоваться переконструировать модель данных, чтобы оптимизировать ее для NoSQL вариантов использования в соответствии с [моделированием данных в Azure Cosmos DB](../cosmos-db/modeling-data.md), например, для денормализации данных путем встраивания всех связанных вложенных элементов в один документ JSON. В этом случае обратитесь к [этой записи блога](https://medium.com/@ArsenVlad/denormalizing-via-embedding-when-copying-data-from-sql-to-cosmos-db-649a649ae0fb) с пошаговым руководством по его выполнению с помощью действия копирования в фабрике данных Azure.

## <a name="next-steps"></a>Дальнейшие действия

В таблице [Поддерживаемые хранилища данных](copy-activity-overview.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования в Фабрике данных Azure.
