---
title: Отправка и получение событий через Центры событий Azure с помощью .NET (последняя версия)
description: В этой статье объясняется, как создать приложение .NET Core, которое отправляет события в Центры событий Azure или получает из них события с помощью последней версии пакета Azure.Messaging.EventHubs.
services: event-hubs
documentationcenter: na
author: spelluru
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: c8c6e2741eeeadf2afc0c027da8f9cf957c29c95
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023248"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs---net-core-azuremessagingeventhubs"></a>Отправка и получение событий с помощью .NET Core в Центрах событий Azure (Azure.Messaging.EventHubs) 
Центры событий — это служба, которая обрабатывает большие объемы данных телеметрии о событиях, поступающих от подключенных устройств и приложений. Собираемые в Центрах событий данные можно сохранять в кластере хранилища или сразу обрабатывать. Например, можно преобразовать данные событий с помощью поставщика данных аналитики в режиме реального времени. Эта возможность сбора и обработки большого объема сведений о событиях — ключевой компонент в современных архитектурах приложений, включая "Интернет вещей". Подробный обзор Центров событий см. в статьях [Что такое Центры событий Azure?](event-hubs-about.md) и [Обзор функций Центров событий](event-hubs-features.md).

В этом руководстве демонстрируется, как отправлять и получать события через концентратор событий с применением пакета SDK Центров событий для .NET Core. 

> [!IMPORTANT]
> В рамках этого краткого руководства применяется библиотека **Azure.Messaging.EventHubs**. Краткое руководство с примерами для старой библиотеки **Microsoft.Azure.EventHubs** вы найдете [здесь](event-hubs-dotnet-standard-getstarted-send.md). 

## <a name="prerequisites"></a>Предварительные требования

- **Подписка Microsoft Azure.** Чтобы использовать службы Azure, в том числе Центры событий Azure, потребуется действующая подписка.  Если у вас еще нет учетной записи Azure, [зарегистрируйтесь для работы с бесплатной пробной версией](https://azure.microsoft.com/free/) или [активируйте преимущества для подписчиков MSDN при создании учетной записи](https://azure.microsoft.com).
- **Microsoft Visual Studio 2019**. Клиентская библиотека для Центров событий Azure использует новые возможности, реализованные в C# версии 8.0.  Вы можете применять эту библиотеку и с более старыми версиями C#, но тогда некоторые функции будут недоступны.  Чтобы включить эти возможности, настройте [нацеливание на .NET Core 3.0](/dotnet/standard/frameworks#how-to-specify-target-frameworks) или [укажите требуемую версию языка](/dotnet/csharp/language-reference/configure-language-version#override-a-default) (не ниже 8.0). Если вы используете Visual Studio, учитывайте, что версии старше Visual Studio 2019 несовместимы со средствами, необходимыми для сборки проектов C# 8.0. Скачать Visual Studio 2019, в том числе выпуск Community, можно [здесь](https://visualstudio.microsoft.com/vs/).
- **Создайте пространство имен Центров событий и концентратор событий**. Первым шагом является использование [портала Azure](https://portal.azure.com) для создания пространства имен типа Центров событий и получение учетных данных управления, необходимых приложению для взаимодействия с концентратором событий. Чтобы создать пространство имен и концентратор событий, выполните инструкции из [этой статьи](event-hubs-create.md). Получите **строку подключения для пространства имен Центров событий**, следуя инструкциям из статьи [Получение строки подключения на портале](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Строка подключения понадобится нам позже.

## <a name="send-events"></a>Отправка событий 
В этом разделе показано, как создать консольное приложение .NET Core, которое отправляет события в концентратор событий. 

### <a name="create-a-console-application"></a>Создание консольного приложение

1. Запустите Visual Studio 2019. 
1. Выберите **Создать новый проект**. 
1. В диалоговом окне **Создать проект** выполните следующие действия: Если это диалоговое окно не отображается, щелкните в меню пункт **Файл**, затем последовательно выберите **Новый** и **Проект**. 
    1. Выберите язык программирования **C#** .
    1. Для типа приложения выберите значение **Консоль**. 
    1. Затем выберите в списке результатов **Console App (.NET Core)** (Консольное приложение (.NET Core)). 
    1. Затем нажмите кнопку **Далее**. 

        ![Диалоговое окно "Новый проект"](./media/getstarted-dotnet-standard-send-v2/new-send-project.png)    
1. Введите имя проекта **EventHubsSender** и имя решения **EventHubsQuickStart**, затем щелкните **OK** для создания проекта. 

    ![C# > Консольное приложение](./media/getstarted-dotnet-standard-send-v2/project-solution-names.png)

### <a name="add-the-event-hubs-nuget-package"></a>Добавление пакета NuGet для Центров событий

1. Выберите в меню элементы **Инструменты** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. 
1. Выполните следующую команду, чтобы установить пакет NuGet **Azure.Messaging.EventHubs**:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```


### <a name="write-code-to-send-messages-to-the-event-hub"></a>Написание кода для отправки сообщений в концентратор событий

1. Добавьте следующие операторы `using` в начало файла **Program.cs**.

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Producer;
    ```

2. Добавьте в класс `Program` константы для значений строки подключения к Центрам событий и имени концентратора событий. Замените заполнители в скобках соответствующими значениями, которые вы получили при создании концентратора событий. Убедитесь, что `{Event Hubs namespace connection string}` — это строка подключения на уровне пространства имен, а не концентратора событий. 

    ```csharp
    private const string connectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
    private const string eventHubName = "<EVENT HUB NAME>";
    ```

3. Замените метод `Main` приведенным ниже методом `async Main`. Дополнительные сведения см. в комментариях к коду. 

    ```csharp
        static async Task Main()
        {
            // Create a producer client that you can use to send events to an event hub
            await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
            {
                // Create a batch of events 
                using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

                // Add events to the batch. An event is a represented by a collection of bytes and metadata. 
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First event")));
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second event")));
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Third event")));

                // Use the producer client to send the batch of events to the event hub
                await producerClient.SendAsync(eventBatch);
                Console.WriteLine("A batch of 3 events has been published.");
            }
        }
    ```
5. Выполните сборку проекта и убедитесь, что она прошла без ошибок.
6. Выполните программу и дождитесь подтверждающего сообщения. 
7. Вы можете проверить получение сообщения концентратором событий на портале Azure. Переключитесь на представление **Сообщения** в разделе **Метрики**. Обновите страницу, чтобы обновить диаграмму. На отображение полученных сообщений может уйти несколько секунд. 

    [![Проверка получения сообщения концентратором событий](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Полный исходный код с более подробными комментариями можно найти в [этом файле в GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/samples/Sample03_PublishAnEventBatch.cs).

## <a name="receive-events"></a>Получение событий
В этом разделе показано, как создать консольное приложение .NET Core для получения сообщений из концентратора событий с помощью обработчика событий. Обработчик событий упрощает прием событий от концентраторов событий, позволяя управлять постоянными контрольными точками и принимать сообщения от этих концентраторов событий в параллельном режиме. Обработчик событий связывается с определенным концентратором событий и группой потребителей. Он получает события от нескольких платформ концентратора событий и передает их делегату обработчика для применения к ним предоставленного вами кода. 


### <a name="create-an-azure-storage-and-a-blob-container"></a>Создание службы хранилища Azure и контейнера больших двоичных объектов
В рамках этого краткого руководства вы настроите Службу хранилища Azure в качестве хранилища контрольных точек. Выполните указанные ниже действия, чтобы создать учетную запись хранения Azure. 

1. [Создайте учетную запись хранения Azure](/azure/storage/common/storage-account-create?tabs=azure-portal)
2. [Создание контейнера больших двоичных объектов](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Получение строки подключения к учетной записи хранения](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string).

    Запишите имя контейнера и строку подключения. Их вы примените далее в коде получения данных. 


### <a name="create-a-project-for-the-receiver"></a>Создание проекта для получателя

1. В окне обозревателя решений щелкните правой кнопкой мыши решение **EventHubQuickStart**, выберите элемент **Добавить** и действие **Создать проект**. 
1. Выберите **Консольное приложение (.NET Core)** , а затем щелкните **Далее**. 
1. Введите строку **EventHubsReceiver** в качестве **имени проекта** и щелкните **Создать**. 

### <a name="add-the-event-hubs-nuget-package"></a>Добавление пакета NuGet для Центров событий

1. Выберите в меню элементы **Инструменты** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. 
1. Выполните следующую команду, чтобы установить пакет NuGet **Azure.Messaging.EventHubs**:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```
1. Выполните следующую команду, чтобы установить пакет NuGet **Azure.Messaging.EventHubs.Processor**:

    ```cmd
    Install-Package Azure.Messaging.EventHubs.Processor
    ```    

### <a name="update-the-main-method"></a>Обновление метода Main 

1. Добавьте следующие инструкции `using` в начало файла **Program.cs**.

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Storage.Blobs;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Consumer;
    using Azure.Messaging.EventHubs.Processor;
    ```
1. Добавьте в класс `Program` константы для значений строки подключения к Центрам событий и имени концентратора событий. Замените заполнители в скобках соответствующими значениями, которые вы получили при создании концентратора событий. Замените заполнители в скобках соответствующими значениями, которые вы получили при создании концентратора событий, и именем учетной записи хранения (основная учетная запись хранения в разделе ключей доступа). Убедитесь, что `{Event Hubs namespace connection string}` — это строка подключения на уровне пространства имен, а не концентратора событий.

    ```csharp
        private const string ehubNamespaceConnectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
        private const string eventHubName = "<EVENT HUB NAME>";
        private const string blobStorageConnectionString = "<AZURE STORAGE CONNECTION STRING>";
        private const string blobContainerName = "<BLOB CONTAINER NAME>";
    ```
3. Замените метод `Main` приведенным ниже методом `async Main`. Дополнительные сведения см. в комментариях к коду. 

    ```csharp
        static async Task Main()
        {
            // Read from the default consumer group: $Default
            string consumerGroup = EventHubConsumerClient.DefaultConsumerGroupName;

            // Create a blob container client that the event processor will use 
            BlobContainerClient storageClient = new BlobContainerClient(blobStorageConnectionString, blobContainerName);

            // Create an event processor client to process events in the event hub
            EventProcessorClient processor = new EventProcessorClient(storageClient, consumerGroup, ehubNamespaceConnectionString, eventHubName);

            // Register handlers for processing events and handling errors
            processor.ProcessEventAsync += ProcessEventHandler;
            processor.ProcessErrorAsync += ProcessErrorHandler;

            // Start the processing
            await processor.StartProcessingAsync();

            // Wait for 10 seconds for the events to be processed
            await Task.Delay(TimeSpan.FromSeconds(10));

            // Stop the processing
            await processor.StopProcessingAsync();
        }    
    ```
1. Теперь добавьте в этот класс указанные ниже методы обработки событий и ошибок. 

    ```csharp
        static Task ProcessEventHandler(ProcessEventArgs eventArgs)
        { 
            // Write the body of the event to the console window
            Console.WriteLine("\tRecevied event: {0}", Encoding.UTF8.GetString(eventArgs.Data.Body.ToArray())); 
            return Task.CompletedTask; 
        }

        static Task ProcessErrorHandler(ProcessErrorEventArgs eventArgs)
        {
            // Write details about the error to the console window
            Console.WriteLine($"\tPartition '{ eventArgs.PartitionId}': an unhandled exception was encountered. This was not expected to happen.");
            Console.WriteLine(eventArgs.Exception.Message);
            return Task.CompletedTask;
        }    
    ```
1. Выполните сборку проекта и убедитесь, что она прошла без ошибок.

    > [!NOTE]
    > Полный исходный код с более подробными комментариями приводится в [этом файле в GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample01_HelloWorld.cs).
6. Запустите приложение получателя. 
1. Отобразится сообщение о том, что событие успешно получено. 

    ![Событие получено](./media/getstarted-dotnet-standard-send-v2/event-received.png)

    Здесь отображаются те же три события, которые вы ранее отправили в концентратор событий, выполняя программу отправителя. 


## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь с примерами в GitHub. 

- [Azure Event Hubs samples](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs/samples) (Примеры Центров событий Azure)
- [Примеры обработчика событий в GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples)
