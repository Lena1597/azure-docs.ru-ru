---
title: Отправка и получение событий через Центры событий Azure с помощью Java (последних версий)
description: В статье описано, как создать приложение Java, которое отправляет события или получает их из службы "Центры событий Azure" с помощью пакета сообщений azure-eventhubs последней версии.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: d9d22374868f3befd659918c532f339d49ba1642
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77032052"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Использование Java для отправки и получения событий в Центрах событий Azure (azure-messaging-eventhubs)
В этом кратком руководстве описано, как создавать приложения Java для отправки и получения событий через Центры событий Azure. 

Центры событий Azure — это платформа потоковой передачи больших данных и служба приема событий, принимающая и обрабатывающая миллионы событий в секунду. Центры событий могут обрабатывать и сохранять события, данные и телеметрию, созданные распределенным программным обеспечением и устройствами. Данные, отправляемые в концентратор событий, можно преобразовывать и сохранять с помощью любого поставщика аналитики в реальном времени, а также с помощью адаптеров пакетной обработки или хранения. Подробный обзор Центров событий см. в статьях [Что такое Центры событий Azure?](event-hubs-about.md) и [Обзор функций Центров событий](event-hubs-features.md).

> [!IMPORTANT]
> В этом кратком руководстве используется новый пакет **azure-messaging-eventhubs**. Краткое руководство, в котором используются старые пакеты **azure-eventhubs** и **azure-eventhubs-eph**, см. в [этой статье](event-hubs-java-get-started-send.md). 


## <a name="prerequisites"></a>Предварительные требования

Для работы с данным руководством вам потребуется:

- Активная учетная запись Azure. Если у вас еще нет подписки Azure, создайте [бесплатная учетная запись](https://azure.microsoft.com/free/), прежде чем начинать работу.
- Среда разработки Java. В этом руководстве используется среда [Eclipse](https://www.eclipse.org/). Необходим комплект SDK Java (JDK) версии 8 или более поздней. 
- **Создайте пространство имен Центров событий и концентратор событий**. Первым шагом является использование [портала Azure](https://portal.azure.com) для создания пространства имен типа Центров событий и получение учетных данных управления, необходимых приложению для взаимодействия с концентратором событий. Чтобы создать пространство имен и концентратор событий, выполните инструкции из [этой статьи](event-hubs-create.md). Затем получите значение ключа доступа для концентратора событий, следуя инструкциям в приведенных ниже статьях. [Получение строки подключения на портале](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Используйте ключ доступа в коде, который вы напишете далее в рамках этого руководства. Имя ключа по умолчанию: **RootManageSharedAccessKey**.

## <a name="send-events"></a>Отправка событий 
Из этого раздела вы узнаете, как создать приложение Java, которое отправляет события в концентратор событий. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Добавление ссылки на библиотеку Центров событий Azure

Клиентскую библиотеку Java для Центров событий можно использовать в проектах Maven из [центрального репозитория Maven](https://search.maven.org/search?q=a:azure-messaging-eventhubs). Чтобы сослаться на эту библиотеку, используйте следующее объявление зависимостей в файле проекта Maven:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Написание кода для отправки сообщений в концентратор событий

Следующий пример сначала создает новый проект Maven для приложения консоли или оболочки в избранной среде разработки Java. Добавьте класс с именем `SimpleSend` и добавьте в него следующий код.

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
    public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>Строка подключения и концентратор событий
Этот код использует строку подключения к пространству имен Центров событий и имя концентратора событий для создания клиента Центров событий. 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>Создание клиента Центров событий 
Этот код создает клиентский объект производителя, который используется для получения или отправки событий в концентратор событий. 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducer();
```

### <a name="prepare-a-batch-of-events"></a>Подготовка пакета событий
Этот код подготавливает пакет событий. 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>Отправка пакета событий в концентратор событий
Этот код отправляет пакет событий, подготовленных на предыдущем шаге, в концентратор событий. Следующий код блокирует операцию отправки. 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>Закрыть и очистить
Этот код закрывает производитель. 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>Завершение кода для отправки событий
Ниже приведен полный код для отправки событий в концентратор событий. 

```java
import com.azure.messaging.eventhubs.*;

public class Sender {
    public static void main(String[] args) {
        final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        final String eventHubName = "EVENT HUB NAME";

        // create a producer using the namespace connection string and event hub name
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducer();

        // prepare a batch of events to send to the event hub    
        EventDataBatch batch = producer.createBatch();
        batch.tryAdd(new EventData("First event"));
        batch.tryAdd(new EventData("Second event"));
        batch.tryAdd(new EventData("Third event"));
        batch.tryAdd(new EventData("Fourth event"));
        batch.tryAdd(new EventData("Fifth event"));

        // send the batch of events to the event hub
        producer.send(batch);

        // close the producer
        producer.close();
    }
}
```

Скомпилируйте программу и убедитесь в отсутствии ошибок. Эта программа будет запущена после запуска программы получателя. 

## <a name="receive-events"></a>Получение событий
Код в этом руководстве основан на [шаблоне EventProcessorClient в GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java). Изучите его, чтобы получить полное представление о рабочем приложении.

### <a name="create-a-java-project"></a>Создание проекта Java

Клиентская библиотека Java для Центров событий доступна для использования в проектах из [центрального репозитория Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22). Ссылаться на нее можно, используя следующее объявление зависимости в файле проекта Maven: 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. Создайте новый класс с именем `Receiver`с помощью следующего кода: Замените значения заполнителей значениями, которые использовались при создании концентратора событий и учетной записи хранения:
   
   ```java
    import com.azure.messaging.eventhubs.*;
    import com.azure.messaging.eventhubs.models.ErrorContext;
    import com.azure.messaging.eventhubs.models.EventContext;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;

    public class Receiver {

        private static final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        private static final String eventHubName = "EVENT HUB NAME";
    
        public static void main(String[] args) throws Exception {

            // function to process events
            Consumer<EventContext> processEvent = eventContext  -> {
                System.out.print("Received event: ");
                // print the body of the event
                System.out.println(eventContext.getEventData().getBodyAsString());
                eventContext.updateCheckpoint();
            };

            // function to process errors
            Consumer<ErrorContext> processError = errorContext -> {
                // print the error message
                System.out.println(errorContext.getThrowable().getMessage());
            };

            EventProcessorBuilder eventProcessorBuilder = new EventProcessorBuilder()
                .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                .connectionString(connectionString, eventHubName)
                .processEvent(processEvent)
                .processError(processError)
                .checkpointStore(new InMemoryCheckpointStore());
        
            EventProcessorClient eventProcessorClient = eventProcessorClientBuilder.buildEventProcessorClient();
            System.out.println("Starting event processor");
            eventProcessorClient.start();

            System.out.println("Press enter to stop.");
            System.in.read();

            System.out.println("Stopping event processor");
            eventProcessor.stop();
            System.out.println("Event processor stopped.");
    
            System.out.println("Exiting process");
        }
    }
    ```
    
2. Скачайте файл **InMemoryCheckpointStore.java** из [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) и добавьте его в проект. 
3. Скомпилируйте программу и убедитесь в отсутствии ошибок. 

## <a name="run-the-applications"></a>Запуск приложений
1. Сначала запустите приложение **получателя**.
1. Затем запустите приложение **отправителя**. 
1. В окне **получатель** приложения убедитесь, что отображаются события, опубликованные приложением отправителя.
1. Нажмите **ENTER** в окне приложения получателя, чтобы прервать работу приложения. 

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь с [примерами пакета SDK для Java на сайте GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)

