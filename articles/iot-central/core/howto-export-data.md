---
title: Экспорт данных IoT Central Azure | Документация Майкрософт
description: Экспорт данных из приложения IoT Central Azure в концентраторы событий Azure, служебную шину Azure и хранилище BLOB-объектов Azure
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: a3d60bf38c4a9dad13dacf8ba9798c4078c1df1a
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049694"
---
# <a name="export-your-azure-iot-central-data"></a>Экспорт данных IoT Central Azure



*Эта статья предназначена для администраторов.*

В этой статье описывается, как использовать функцию непрерывного экспорта данных в Azure IoT Central для экспорта данных в **концентраторы событий Azure**, **служебную шину Azure**или экземпляры **хранилища BLOB-объектов Azure** . Данные экспортируются в формате JSON и могут включать данные телеметрии, сведения об устройстве и сведения о шаблонах устройств. Используйте экспортированные данные для:

- Подробные сведения о теплом пути и аналитика. Этот параметр включает в себя запуск настраиваемых правил в Azure Stream Analytics, запуск пользовательских рабочих процессов в Azure Logic Apps или их передачу с помощью функций Azure для преобразования.
- Аналитика холодного пути, например моделей обучения в Машинное обучение Azure или долгосрочном анализе тенденций в Microsoft Power BI.

> [!Note]
> С момента включения экспорта непрерывных данных вы получаете только поступающие данные. Сейчас невозможно получить данные за то время, когда непрерывный экспорт данных был отключен. Чтобы сохранить больше исторических данных, включите экспорт непрерывных данных раньше.

## <a name="prerequisites"></a>предварительные требования

Необходимо быть администратором в IoT Central приложении или иметь разрешения на экспорт данных.

## <a name="set-up-export-destination"></a>Настройка места назначения экспорта

Назначение экспорта должно существовать до настройки непрерывного экспорта данных.

### <a name="create-event-hubs-namespace"></a>Создание пространства имен Центров событий

Если у вас нет пространства имен концентраторов событий для экспорта, выполните следующие действия.

1. Создайте [пространство имен Центров событий на портале Azure](https://ms.portal.azure.com/#create/Microsoft.EventHub). Дополнительные сведения см. в [документации по Центрам событий Azure](../../event-hubs/event-hubs-create.md).

2. Выберите подписку. Данные можно экспортировать в другие подписки, которые не находятся в той же подписке, что и приложение IoT Central. В этом случае вы подключаетесь, используя строку подключения.

3. Создайте концентратор событий в пространстве имен в Центрах событий. Перейдите к пространству имен и выберите **+ Концентратор событий** в верхней части, чтобы создать экземпляр концентратора событий.

### <a name="create-service-bus-namespace"></a>Создание пространства имен Служебной шины

Если у вас нет пространства имен служебной шины для экспорта, выполните следующие действия.

1. Создайте [новое пространство имен служебной шины в портал Azure](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5). Дополнительные сведения см. в [документации по Служебной шине Azure](../../service-bus-messaging/service-bus-create-namespace-portal.md).
2. Выберите подписку. Данные можно экспортировать в другие подписки, которые не находятся в той же подписке, что и приложение IoT Central. В этом случае вы подключаетесь, используя строку подключения.

3. Перейдите к пространству имен Служебной шины и выберите **+ Очередь** или **+ Раздел** в верхней части, чтобы создать очередь или раздел для экспорта.

Если в качестве назначения экспорта выбрана служебная шина, то очереди и разделы не должны включать сеансы или обнаружение повторяющихся записей. Если любая из этих функций включена, некоторые сообщения не будут поступать в очередь или раздел.

### <a name="create-storage-account"></a>Создание учетной записи хранения

Если у вас нет существующей учетной записи хранения Azure для экспорта, выполните следующие действия.

1. Создайте [учетную запись хранения на портале Azure](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Вы можете узнать больше о создании новых [учетных записей хранения BLOB-объектов Azure](https://aka.ms/blobdocscreatestorageaccount) или [учетных записей хранения Azure Data Lake Storage v2](../../storage/blobs/data-lake-storage-quickstart-create-account.md). Экспорт данных может записывать данные только в учетные записи хранения, поддерживающие блочные BLOB-объекты. Ниже приведен список известных совместимых типов учетных записей хранения. 

    |Уровень производительности|Тип учетной записи|
    |-|-|
    |Standard|общего назначения v2|
    |Standard|общего назначения v1|
    |Standard|Хранилище BLOB-объектов|
    |Premium|Блочное хранилище BLOB-объектов|

2. Создайте контейнер в учетной записи хранения. Войдите в свою учетную запись хранения. Выберите **Обзор BLOB-объектов** в разделе **Служба BLOB-объектов**. Выберите **+ Контейнер** в верхней части экрана, чтобы создать контейнер.

## <a name="set-up-continuous-data-export"></a>Настройка экспорта непрерывных данных

Теперь, когда у вас есть место назначения для экспорта данных, выполните следующие действия, чтобы настроить непрерывный экспорт данных.

1. Войдите в приложение IoT Central.

2. На левой панели выберите **Экспорт данных**.

    > [!Note]
    > Если вы не видите элемент Экспорт данных в левой области, у вас нет разрешений на настройку экспорта данных в приложении. Обратитесь к администратору, чтобы настроить экспорт данных.

3. Нажмите кнопку **+ создать** в правом верхнем углу. Выберите в качестве места назначения для экспорта один из **концентраторов событий Azure**, **служебную шину Azure**или **хранилище BLOB-объектов Azure** . Максимальное число экспортов на приложение равно пяти.

    ![Создание непрерывного экспорта данных](media/howto-export-data/new-export-definition.png)

4. В раскрывающемся списке выберите **пространство имен концентраторов событий**, **пространство имен служебной шины**, **пространство имен учетной записи хранения**или **введите строку подключения**.

    - Учетные записи хранения, пространства имен концентраторов событий и пространства имен служебной шины отображаются только в той же подписке, что и приложение IoT Central. Если вы хотите выполнить экспорт за пределы этой подписки, выберите **Enter a connection string** (Ввести строку подключения) и перейдите к шагу 5.
    - Для приложений, созданных с помощью бесплатного тарифного плана, единственным способом настройки непрерывного экспорта данных является строка подключения. Для приложений, иссвященных бесплатному тарифному плану, не назначена подписка Azure.

    ![Создать новый концентратор событий](media/howto-export-data/export-event-hub.png)

5. (Необязательно.) Если вы выбрали **Enter a connection string** (Ввести строку подключения), открывается новое окно, в которое нужно вставить строку подключения. Чтобы получить строку подключения, сделайте следующее:
    - Концентраторы событий или служебная шина, перейдите к пространству имен в портал Azure.
        - В разделе **Параметры**выберите **политики общего доступа** .
        - Выберите значение по умолчанию **RootManageSharedAccessKey** или создайте политику.
        - Скопируйте основную или дополнительную строку подключения.
    - Учетная запись хранения, перейдите к учетной записи хранения в портал Azure:
        - В разделе **Параметры**выберите **ключи доступа** .
        - Скопируйте строку подключения key1 или строку подключения Key2.

6. В раскрывающемся списке выберите концентратор событий, очередь, раздел или контейнер.

7. В разделе **данные для экспорта**выберите типы данных для экспорта, задав для параметра Тип значение **вкл**.

8. Чтобы включить непрерывный экспорт данных, убедитесь, что **включен** переключатель включить **.** Щелкните **Сохранить**.

9. Через несколько минут данные отобразятся в выбранном месте назначения.

## <a name="export-contents-and-format"></a>Экспорт содержимого и формата

Экспортированные данные телеметрии содержат все сообщения, отправленные в IoT Central, а не только сами значения телеметрии. Данные экспортированных устройств содержат изменения свойств и метаданных всех устройств, а экспортированные шаблоны устройств содержат изменения во всех шаблонах устройств.

Для концентраторов событий и служебной шины данные экспортируются практически в реальном времени. Данные находятся в свойстве body и представлены в формате JSON (примеры см. ниже).

Для хранилища BLOB-объектов данные экспортируются один раз в минуту, при этом каждый файл содержит пакет изменений с момента последнего экспортированного файла. Экспортированные данные помещаются в три папки в формате JSON. Пути по умолчанию в вашей учетной записи хранения:

- Данные телеметрии: _{Container}/{АПП-ИД}/телеметри/{ИИИИ}/{мм}/{дд}/{ХХ}/{мм}/{филенаме}_
- Устройства: _{Container}/{АПП-ИД}/девицес/{ИИИИ}/{мм}/{дд}/{ХХ}/{мм}/{филенаме}_
- Шаблоны устройств: _{Container}/{АПП-ИД}/девицетемплатес/{ИИИИ}/{мм}/{дд}/{ХХ}/{мм}/{филенаме}_

Вы можете просмотреть экспортированные файлы в портал Azure, перейдя к файлу и выбрав вкладку **Edit BLOB (изменить большой двоичный объект** ).


## <a name="telemetry"></a>Телеметрия

Для концентраторов событий и служебной шины новое сообщение быстро экспортируется после IoT Central получения сообщения от устройства, а каждое экспортированное сообщение содержит полное сообщение, отправленное устройством в свойстве body в формате JSON.

Для хранилища BLOB-объектов сообщения помещаются в пакет и экспортируются один раз в минуту. Экспортируемые файлы используют тот же формат, что и файлы сообщений, экспортированные [службой маршрутизации сообщений центра Интернета вещей](../../iot-hub/tutorial-routing.md) в хранилище BLOB-объектов. 

> [!NOTE]
> Для хранилища BLOB-объектов убедитесь, что устройства отправляют сообщения с `contentType: application/JSON` и `contentEncoding:utf-8` (или `utf-16`, `utf-32`). Пример см. в документации по центру [Интернета вещей](../../iot-hub/iot-hub-devguide-routing-query-syntax.md#message-routing-query-based-on-message-body) .

Устройство, отправившего данные телеметрии, представлено ИДЕНТИФИКАТОРом устройства (см. следующие разделы). Чтобы получить имена устройств, экспортируйте данные устройства и сопоставьте каждое сообщение с помощью **коннектиондевицеид** , который соответствует идентификатору **deviceId** сообщения устройства.

Это пример сообщения, полученного в концентраторе событий или в очереди или разделе служебной шины.

```json
{
  "body":{
    "temp":67.96099945281145,
    "humid":58.51139305465015,
    "pm25":36.91162432340187
  },
  "annotations":{
    "iothub-connection-device-id":"<deviceId>",
    "iothub-connection-auth-method":"{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id":"<generationId>",
    "iothub-enqueuedtime":1539381029965,
    "iothub-message-source":"Telemetry",
    "x-opt-sequence-number":25325,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539381030200
  },
  "sequenceNumber":25325,
  "enqueuedTimeUtc":"2018-10-12T21:50:30.200Z",
  "offset":"<offset>",
  "properties":{
    "content_type":"application/json",
    "content_encoding":"utf-8"
  }
}
```

Это пример записи, экспортированной в хранилище BLOB-объектов:

```json
{
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{

  },
  "SystemProperties":{
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

## <a name="devices"></a>Устройства

Каждое сообщение или запись в моментальном снимке представляет одно или несколько изменений устройства, свойства устройства и облака с момента последнего экспортированного сообщения. В том числе:

- идентификатор (`id`) устройства в IoT Central;
- имя (`displayName`) устройства;
- Идентификатор шаблона устройства в `instanceOf`
- Флаг `simulated`, значение true, если устройство является имитацией устройства
- Флаг `provisioned`, значение true, если устройство подготовлено
- Флаг `approved`, значение true, если устройство утверждено для отправки данных
- Значения свойств
- `properties`, включая значения свойств устройства и облака

Удаленные устройства не экспортируются. В настоящее время в сообщениях экспорта нет никаких индикаторов касательно удаленных устройств.

Для концентраторов событий и служебной шины сообщения, содержащие данные устройства, отправляются в концентратор событий или в очередь или раздел служебной шины практически в реальном времени, как показано в IoT Central. 

Для хранилища BLOB-объектов новый моментальный снимок, содержащий все изменения с момента последнего записанного, экспортируется один раз в минуту.

Это пример сообщения об устройствах и свойствах в концентраторе событий или в очереди или разделе служебной шины:

```json
{
  "body":{
    "id": "<device Id>",
    "etag": "<etag>",
    "displayName": "Sensor 1",
    "instanceOf": "<device template Id>",
    "simulated": false,
    "provisioned": true,
    "approved": true,
    "properties": {
        "sensorComponent": {
            "setTemp": "30",
            "fwVersion": "2.0.1",
            "status": { "first": "first", "second": "second" },
            "$metadata": {
                "setTemp": {
                    "desiredValue": "30",
                    "desiredVersion": 3,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 3
                },
                "fwVersion": { "ackVersion": 3 },
                "status": {
                    "desiredValue": {
                        "first": "first",
                        "second": "second"
                    },
                    "desiredVersion": 2,
                    "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                    "ackVersion": 2
                }
            },
            
        }
    },
    "installDate": { "installDate": "2020-02-01" }
},
  "annotations":{
    "iotcentral-message-source":"devices",
    "x-opt-partition-key":"<partitionKey>",
    "x-opt-sequence-number":39740,
    "x-opt-offset":"<offset>",
    "x-opt-enqueued-time":1539274959654
  },
  "partitionKey":"<partitionKey>",
  "sequenceNumber":39740,
  "enqueuedTimeUtc":"2020-02-01T18:14:49.3820326Z",
  "offset":"<offset>"
}
```

Это пример моментального снимка, содержащего устройства и свойства данных в хранилище BLOB-объектов. Экспортируемые файлы содержат одну строку для каждой записи.

```json
{
  "id": "<device Id>",
  "etag": "<etag>",
  "displayName": "Sensor 1",
  "instanceOf": "<device template Id>",
  "simulated": false,
  "provisioned": true,
  "approved": true,
  "properties": {
      "sensorComponent": {
          "setTemp": "30",
          "fwVersion": "2.0.1",
          "status": { "first": "first", "second": "second" },
          "$metadata": {
              "setTemp": {
                  "desiredValue": "30",
                  "desiredVersion": 3,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 3
              },
              "fwVersion": { "ackVersion": 3 },
              "status": {
                  "desiredValue": {
                      "first": "first",
                      "second": "second"
                  },
                  "desiredVersion": 2,
                  "desiredTimestamp": "2020-02-01T17:15:08.9284049Z",
                  "ackVersion": 2
              }
          },
          
      }
  },
  "installDate": { "installDate": "2020-02-01" }
}
```

## <a name="device-templates"></a>Шаблоны устройств

Каждое сообщение или запись моментального снимка представляет одно или несколько изменений шаблона опубликованного устройства с момента последнего экспортированного сообщения. Данные, отправляемые в каждое сообщение или запись, включают:

- `id` шаблона устройства, соответствующего `instanceOf` приведенного выше потока устройств
- версию (`displayName`) шаблона устройства;
- Устройство `capabilityModel` включая его `interfaces`, а также определения телеметрии, свойств и команд
- определения `cloudProperties`
- Переопределения и начальные значения, встроенные с `capabilityModel`

Удаленные шаблоны устройств не экспортируются. В настоящее время в сообщениях экспорта нет никаких индикаторов касательно удаленных шаблонов устройств.

Для концентраторов событий и служебной шины сообщения, содержащие данные шаблона устройства, отправляются в концентратор событий или в очередь или раздел служебной шины практически в реальном времени, как показано в IoT Central. 

Для хранилища BLOB-объектов новый моментальный снимок, содержащий все изменения с момента последнего записанного, экспортируется один раз в минуту.

Это пример сообщения о шаблонах устройств в концентраторе событий или в очереди или разделе служебной шины:

```json
{
  "body":{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "sensorComponent",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorConfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  },
    "annotations":{
      "iotcentral-message-source":"deviceTemplates",
      "x-opt-partition-key":"<partitionKey>",
      "x-opt-sequence-number":25315,
      "x-opt-offset":"<offset>",
      "x-opt-enqueued-time":1539274985085
    },
    "partitionKey":"<partitionKey>",
    "sequenceNumber":25315,
    "enqueuedTimeUtc":"2019-10-02T16:23:05.085Z",
    "offset":"<offset>"
  }
}
```

Это пример моментального снимка, содержащего устройства и свойства данных в хранилище BLOB-объектов. Экспортируемые файлы содержат одну строку для каждой записи.

```json
{
      "id": "<device template id>",
      "etag": "<etag>",
      "types": ["DeviceModel"],
      "displayName": "Sensor template",
      "capabilityModel": {
          "@id": "<capability model id>",
          "@type": ["CapabilityModel"],
          "contents": [],
          "implements": [
              {
                  "@id": "<component Id>",
                  "@type": ["InterfaceInstance"],
                  "name": "Sensor component",
                  "schema": {
                      "@id": "<interface Id>",
                      "@type": ["Interface"],
                      "displayName": "Sensor interface",
                      "contents": [
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry"],
                              "displayName": "Humidity",
                              "name": "humidity",
                              "schema": "double"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Telemetry", "SemanticType/Event"],
                              "displayName": "Error event",
                              "name": "error",
                              "schema": "integer"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Set temperature",
                              "name": "setTemp",
                              "writable": true,
                              "schema": "integer",
                              "unit": "Units/Temperature/fahrenheit",
                              "initialValue": "30"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Firmware version read only",
                              "name": "fwversion",
                              "schema": "string"
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Property"],
                              "displayName": "Display status",
                              "name": "status",
                              "writable": true,
                              "schema": {
                                  "@id": "urn:testInterface:status:obj:ka8iw8wka:1",
                                  "@type": ["Object"]
                              }
                          },
                          {
                              "@id": "<id>",
                              "@type": ["Command"],
                              "commandType": "synchronous",
                              "request": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Configuration",
                                  "name": "config",
                                  "schema": "string"
                              },
                              "response": {
                                  "@id": "<id>",
                                  "@type": ["SchemaField"],
                                  "displayName": "Response",
                                  "name": "response",
                                  "schema": "string"
                              },
                              "displayName": "Configure sensor",
                              "name": "sensorconfig"
                          }
                      ]
                  }
              }
          ],
          "displayName": "Sensor capability model"
      },
      "solutionModel": {
          "@id": "<id>",
          "@type": ["SolutionModel"],
          "cloudProperties": [
              {
                  "@id": "<id>",
                  "@type": ["CloudProperty"],
                  "displayName": "Install date",
                  "name": "installDate",
                  "schema": "dateTime",
                  "valueDetail": {
                      "@id": "<id>",
                      "@type": ["ValueDetail/DateTimeValueDetail"]
                  }
              }
          ]
      }
  }
```
## <a name="data-format-change-notice"></a>Уведомление об изменении формата данных

> [!Note]
> Это изменение не влияет на формат данных потока телеметрии. Затрагиваются только потоки данных для устройств и шаблонов устройств.

При наличии в предварительной версии приложения экспорта данных с включенными потоками *устройств* и *шаблонов устройств* необходимо будет обновить экспорт на **30 июня 2020**. Это относится к экспортам в хранилище BLOB-объектов Azure, концентраторам событий Azure и служебной шине Azure.

Начиная с 3 февраля 2020, все новые операции экспорта в приложениях с включенными устройствами и шаблонами устройств будут иметь формат данных, описанный выше. Все экспорты, созданные до этого, будут оставаться в старом формате до 30 июня 2020, после чего эти экспорты будут автоматически перенесены в новый формат данных. Новый формат данных соответствует [устройству](https://docs.microsoft.com/rest/api/iotcentral/devices/get), [свойству устройства](https://docs.microsoft.com/rest/api/iotcentral/devices/getproperties), [свойству облака устройства](https://docs.microsoft.com/rest/api/iotcentral/devices/getcloudproperties) и объектам [шаблона устройства](https://docs.microsoft.com/rest/api/iotcentral/devicetemplates/get) в общедоступном API IOT Central. 
 
Для **устройств**существенными различиями между старым форматом данных и новым форматом данных являются:
- `@id` для устройства удалено, `deviceId` переименовывается в `id` 
- добавлен флаг `provisioned` для описания состояния подготовки устройства.
- добавлен флаг `approved` для описания состояния утверждения устройства.
- `properties` включая свойства устройства и облака, сопоставляет сущности в общедоступном API

Для **шаблонов устройств**существенными различиями между старым форматом данных и новым форматом данных являются:

- `@id` для шаблона устройства переименовывается в `id`
- `@type` для шаблона устройства переименовывается в `types`, а теперь является массивом

### <a name="devices-format-deprecated-as-of-3-february-2020"></a>Устройства (формат не рекомендуется к 3 февраля 2020)
```json
{
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{
    "$cloudProperties":{
        "Color":"blue"
    },
    "EnvironmentalSensor":{
      "thsensormodel":{
        "reported":{
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{
        "reported":{
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{
      "totalStorage":{
        "reported":{
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{
        "reported":{
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates-format-deprecated-as-of-3-february-2020"></a>Шаблоны устройств (формат не рекомендуется к 3 февраля 2020)
```json
{
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{
                  "@value":"50"
                }
              },
              "visualizationDetail":{
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      {
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            {
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[
      {
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```
## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как экспортировать данные в концентраторы событий Azure, служебную шину Azure и хранилище BLOB-объектов Azure, перейдите к следующему шагу:

> [!div class="nextstepaction"]
> [Создание веб-перехватчиков](./howto-create-webhooks.md)
