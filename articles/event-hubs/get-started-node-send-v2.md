---
title: Отправка и получение событий через Центры событий Azure с помощью Node.js (последняя версия)
description: В статье описано, как создать приложение Node.js, которое отправляет события или получает их из службы "Центры событий Azure" с помощью последнего пакета azure/event-hubs версии 5.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: b523e4a7b463564cbfeb407c91b7bb05317f8166
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906370"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-nodejs--azureevent-hubs-version-5"></a>Отправка событий или получение событий из концентраторов событий с помощью Node.js (azure/event-hubs версии 5)

Центры событий Azure — это платформа потоковой передачи больших данных и служба приема событий, принимающая и обрабатывающая миллионы событий в секунду. Центры событий могут обрабатывать и сохранять события, данные и телеметрию, созданные распределенным программным обеспечением и устройствами. Данные, отправляемые в концентратор событий, можно преобразовывать и сохранять с помощью любого поставщика аналитики в режиме реального времени, а также с помощью адаптеров пакетной обработки или хранения. Дополнительные сведения см. в статье [Центры событий Azure — платформа потоковой передачи больших данных и служба приема событий](event-hubs-about.md) и [Features and terminology in Azure Event Hubs](event-hubs-features.md) (Функции и терминология в Центрах событий Azure).

В этом кратком руководстве описано, как создать приложения Node.js, которые отправляют или получают события через концентратор событий.

> [!IMPORTANT]
> В рамках этого краткого руководства используется версия 5 пакета SDK Центров событий Azure для JavaScript. Сведения об использовании пакета SDK для JavaScript версии 2 см. [Краткое руководство. Отправка и получение событий через Центры событий Azure с помощью Node.js](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Предварительные требования

Для работы с данным руководством необходимо следующее:

- Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), прежде чем начать работу.  
- Node.js 8.x или более поздней версии. Скачайте последнюю версию [долгосрочной поддержки (LTS)](https://nodejs.org).  
- Visual Studio Code (рекомендовано) или любая другая интегрированная среда разработки (IDE).  
- Активное пространство имен Центров событий и концентратор событий. Чтобы создать их, выполните приведенные ниже действия. 

   1. Создайте на [портале Azure](https://portal.azure.com) пространство имен типа *Центров событий* и получите учетные данные для управления, необходимые приложению для взаимодействия с концентратором событий. 
   1. Чтобы создать пространство имен и концентратор событий, следуйте инструкциям в статье [Краткое руководство. Создание концентратора событий с помощью портала Azure](event-hubs-create.md).
   1. Продолжайте, выполнив инструкции в этом кратком руководстве. 
   1. Чтобы получить строку подключения для пространства имен концентратора событий, следуйте инструкциям в разделе [Get connection string from the portal](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) (Получение строки подключения на портале). Запишите строку подключения для дальнейшего использования в этом руководстве.

### <a name="install-the-npm-package"></a>Установка пакета npm
Чтобы установить [Пакет Node Package Manager (npm) для Центров событий](https://www.npmjs.com/package/@azure/event-hubs), откройте командную строку, в пути которой сохранен *npm*, перейдите в папку, в которой вы хотите сохранить примеры, и выполните эту команду.

```shell
npm install @azure/event-hubs
```

Для принимающей стороны необходимо установить еще два пакета. В этом кратком руководстве вы используете хранилище BLOB-объектов Azure для сохранения контрольных точек, чтобы программа не читала уже прочитанные события. Оно регулярно выполняет фиксацию метаданных для полученных сообщений в большом двоичном объекте. Благодаря такому подходу позже можно легко продолжить получать сообщения с того места, где вы остановились.

Выполните следующие команды:

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Отправка событий

В этом разделе вы создадите приложение Node.js, которое отправляет события в концентратор событий.

1. Откройте редактор, например [Visual Studio Code](https://code.visualstudio.com).
1. Создайте файл с именем *send.js* и вставьте в него следующий код.

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {

      // Create a producer client to send messages to the event hub.
      const producer = new EventHubProducerClient(connectionString, eventHubName);

      // Prepare a batch of three events.
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    

      // Send the batch to the event hub.
      await producer.sendBatch(batch);

      // Close the producer client.
      await producer.close();

      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
1. Используйте в коде реальные значения, чтобы заменить следующее:
    * `EVENT HUBS NAMESPACE CONNECTION STRING` 
    * `EVENT HUB NAME`
1. запустите `node send.js`, чтобы выполнить этот файл. Эта команда отправляет в концентратор событий пакет из трех событий.
1. Проверьте получение сообщения концентратором событий на портале Azure. В разделе**Метрики** переключитесь на представление **Сообщения**. Обновите страницу, чтобы обновить диаграмму. На отображение полученных сообщений может уйти несколько секунд.

    [![Проверка получения сообщения концентратором событий](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Полный исходный код, включая дополнительные информационные комментарии, можно найти на странице [GitHub sendEvents.js](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js).

Поздравляем! Вы отправили события в концентратор событий.


## <a name="receive-events"></a>Получение событий
В этом разделе вы получаете события из концентратора событий с помощью хранилища контрольных точек хранилища BLOB-объектов Azure в приложении Node.js. Оно регулярно создает контрольные точки метаданных для полученных сообщений в Azure Storage blob. Благодаря такому подходу позже можно легко продолжить получать сообщения с того места, где вы остановились.

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Создание учетной записи хранения Azure и контейнера больших двоичных объектов
Для создания учетной записи хранения Azure и контейнера больших двоичных объектов в ней, выполните действия в следующих ресурсах.

1. [Create an Azure Storage account](../storage/common/storage-account-create.md?tabs=azure-portal) (Создание учетной записи хранения Azure)  
2. [Создание контейнера](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
3. [Получение строки подключения к учетной записи хранения](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string).

Обязательно запишите строку подключения и имя контейнера для последующего использования в коде получения.

### <a name="write-code-to-receive-events"></a>Написание кода для получения событий

1. Откройте редактор, например [Visual Studio Code](https://code.visualstudio.com).
1. Создайте файл с именем *receive.js* и вставьте в него следующий код.

    ```javascript
    const { EventHubConsumerClient } = require("@azure/event-hubs");
    const { ContainerClient } = require("@azure/storage-blob");    
    const { BlobCheckpointStore } = require("@azure/eventhubs-checkpointstore-blob");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";    
    const eventHubName = "EVENT HUB NAME";
    const consumerGroup = "$Default"; // name of the default consumer group
    const storageConnectionString = "AZURE STORAGE CONNECTION STRING";
    const containerName = "BLOB CONTAINER NAME";

    async function main() {
      // Create a blob container client and a blob checkpoint store using the client.
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);

      // Create a consumer client for the event hub by specifying the checkpoint store.
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);

      // Subscribe to the events, and specify handlers for processing the events and errors.
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // Update the checkpoint.
            await context.updateCheckpoint(events[events.length - 1]);
          },

          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );

      // After 30 seconds, stop processing.
      await new Promise((resolve) => {
        setTimeout(async () => {
          await subscription.close();
          await consumerClient.close();
          resolve();
        }, 30000);
      });
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });    
    ```
1. Используйте в коде реальные значения, чтобы заменить следующие значения:
    - `EVENT HUBS NAMESPACE CONNECTION STRING`
    - `EVENT HUB NAME`
    - `AZURE STORAGE CONNECTION STRING`
    - `BLOB CONTAINER NAME`
1. В окне командной строки выполните команду `node receive.js`, которая запускает этот файл. Сообщения о полученных событиях должны отображаться в окне.

    > [!NOTE]
    > Полный исходный код, включая дополнительные информационные комментарии, можно найти на странице [GitHub receiveEventsUsingCheckpointStore](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js).

Поздравляем! Теперь вы получили события из вашего концентратора событий. Программа-получатель получит события из всех разделов группы потребителей по умолчанию в указанном центре событий.

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь с примерами в GitHub:

- [Примеры JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [Примеры TypeScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
