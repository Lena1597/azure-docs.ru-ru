---
title: Руководство по устранению неполадок в концентраторах событий Azure | Документация Майкрософт
description: В этой статье приводится список исключений обмена сообщениями Центров событий и предлагаемые действия.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: de5b95bd10bf72f60ba5d63c4b3a799556fcce33
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309784"
---
# <a name="troubleshooting-guide-for-azure-event-hubs"></a>Руководство по устранению неполадок для концентраторов событий Azure
В этой статье приведены некоторые исключения .NET, создаваемые концентраторами событий .NET Framework API, а также другие советы по устранению неполадок. 

## <a name="event-hubs-messaging-exceptions---net"></a>Исключения обмена сообщениями концентраторов событий .NET
В этом разделе перечислены исключения .NET, созданные .NET Framework API. 

### <a name="exception-categories"></a>Категории исключений

Интерфейсы API .NET концентраторов событий создают исключения, которые можно разделить на следующие категории, а также связанные действия, которые можно предпринять, чтобы попытаться исправить их.

1. Ошибка в коде пользователя: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Общее действие: прежде чем продолжить, попытайтесь исправить код.
2. Ошибка настройки или конфигурации: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Общее действие: проверьте конфигурацию и измените ее при необходимости.
3. Временные исключения: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Общее действие: повторите операцию или уведомите пользователей.
4. Другие исключения: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Общее действие: связано с типом исключения; см. таблицу в следующем разделе. 

### <a name="exception-types"></a>Типы исключений
В следующей таблице перечислены типы исключений обмена сообщениями и их причины, а также рекомендуемые действия, которые можно выполнять.

| Тип исключения | Описание, причина, примеры | Рекомендуемое действие | Примечание к автоматическому или немедленному повтору |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Сервер не ответил на запрошенную операцию в течение указанного времени, который управляется [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). Возможно, сервер выполнил запрошенную операцию. Это исключение может произойти из-за сетевой или другой задержки инфраструктуры. |Проверьте состояние системы для обеспечения согласованности и при необходимости повторите попытку.<br /> Ознакомьтесь с разделом [TimeoutException](#timeoutexception). | Повторная попытка может помочь в некоторых случаях. Добавьте в код логику повторных попыток. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Запрошенная пользовательская операция не разрешена в пределах сервера или службы. Подробные сведения см. в сообщении об исключении. Например, операция [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) создаст это исключение, если сообщение было получено в режиме [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode). | Проверьте код и обратитесь к документации. Убедитесь, что запрошенная операция допустима. | Повторная попытка не поможет. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Предпринята попытка вызвать операцию с объектом, который уже закрыт, прерван или удален. Иногда это означает, что внешняя транзакция уже удалена. | Проверьте код и убедитесь, что он не вызывает операции с удаленным объектом. | Повторная попытка не поможет. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | Объекту [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) не удалось получить маркер, токен является недопустимым, или маркер не содержит утверждений, необходимых для выполнения операции. | Убедитесь, что поставщик маркеров создан с использованием правильных значений. Проверьте конфигурацию службы контроля доступа. | Повторная попытка может помочь в некоторых случаях. Добавьте в код логику повторных попыток. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Для этого метода предоставлен один или несколько недопустимых аргументов. Универсальный код ресурса (URI), переданный в [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) или [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory), содержит сегменты пути. В [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) или [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) передана недопустимая схема универсального кода ресурса (URI). Значение свойства превышает 32 КБ. | Проверьте вызывающий код и убедитесь, что аргументы заданы правильно. | Повторная попытка не поможет. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | Сущность, связанная с операцией, не существует или была удалена. | Убедитесь, что сущность существует. | Повторная попытка не поможет. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | Клиент не может установить соединение с концентратором событий. |Убедитесь, что имя узла указано правильно и узел доступен. | Повторная попытка может помочь при наличии периодических сбоев подключения. |
| [ServerBusyException Microsoft. ServiceBus. Messaging](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | В настоящее время служба не может обработать запрос. | Клиент может некоторое время подождать, после чего следует повторить операцию. <br /> Ознакомьтесь с разделом [ServerBusyException](#serverbusyexception). | Клиент может повторить операцию через определенный промежуток времени. Если в результате повтора возникает другое исключение, проверьте поведение повтора этого исключения. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Универсальное исключение обмена сообщениями, которое может быть вызвано в следующих случаях: предпринята попытка создать [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) с использованием имени или пути, которые относятся к другому типу сущности (например, разделу). Предпринята попытка отправить сообщение размером свыше 1 МБ. Во время обработки запроса возникла ошибка сервера или службы. Подробные сведения см. в сообщении об исключении. Как правило, это временное исключение. | Проверьте код и убедитесь, что для текста сообщения применяются только сериализуемые объекты (или используйте настраиваемый сериализатор). Обратитесь к документации для получения сведений о поддерживаемых типах значений свойств и используйте только поддерживаемые типы. Проверьте свойство [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) . Если оно имеет значение **true**, можно повторить операцию. | Алгоритм поведения не определен и может не помочь. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Предпринята попытка создать сущность с именем, которое уже используется другой сущностью в этом пространстве имен службы. | Удалите существующую сущность или выберите другое имя для сущности, которую нужно создать. | Повторная попытка не поможет. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | Сущность обмена сообщениями достигла максимально допустимого размера. Это исключение может произойти, если уже было открыто максимальное число получателей (равное 5) на группу потребителей. | Создайте в сущности пространство, получая сообщения из сущности или ее подочередей. <br /> Ознакомьтесь с разделом [QuotaExceededException](#quotaexceededexception). | Повторная попытка может помочь, если сообщения были удалены. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Выполнен запрос на операцию среды выполнения с отключенной сущностью. |Активируйте сущность. | Повторная попытка может помочь, если сущность была активирована. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Полезная нагрузка сообщения превышает ограничение в 1 МБ. Ограничение в 1 МБ предназначено для всего сообщения, которое может включать системные свойства и любые дополнительные издержки .NET. | Сократите размер полезных данных сообщения, а затем повторите операцию. |Повторная попытка не поможет. |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) указывает на превышение квоты для конкретного объекта.

Это исключение может произойти, если уже было открыто максимальное число получателей (5) на группу потребителей.

#### <a name="event-hubs"></a>Концентраторы событий
Центры событий имеют ограничение в 20 групп потребителей на один концентратор событий. При попытке создать больше групп появляется исключение [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

### <a name="timeoutexception"></a>TimeoutException
[TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) указывает, что запущенная пользователем операция занимает больше времени, чем время ожидания операции. 

Для Центров событий время ожидания указывается как часть строки подключения или с помощью [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). Само сообщение об ошибке может отличаться, но оно всегда содержит значение времени ожидания, указанное для текущей операции. 

#### <a name="common-causes"></a>Основные причины
Существует две основные причины этой ошибки: неправильная конфигурация или временная ошибка службы.

1. **Неправильная конфигурация.** Время ожидания операции может быть слишком коротким для состояния операции. Время ожидания операции в пакете SDK клиента по умолчанию составляет 60 секунд. Проверьте, не задано ли в вашем коде слишком маленькое значение. Состояние использования сети и ЦП может повлиять на время, затрачиваемое на выполнение определенной операции, поэтому время ожидания операции не должно быть небольшим.
2. **Временная ошибка службы.** Иногда обработка запросов в службе Центров событий выполняется с задержками, например в периоды интенсивной загрузки сети. В таких случаях можно настроить повторную попытку выполнения операции через некоторое время до ее успешного завершения. Если же операцию по-прежнему не удается выполнить после нескольких попыток, посетите [сайт состояния служб Azure](https://azure.microsoft.com/status/), чтобы получить сведения об известных простоях служб.

### <a name="serverbusyexception"></a>ServerBusyException

[Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) или [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) указывают на то, что сервер перегружен. Для этого исключения есть два соответствующих кода ошибки.

#### <a name="error-code-50002"></a>Код ошибки 50002

Эта ошибка может возникать по одной из двух причин:

1. Нагрузка равномерно распределяется по всем секциям в концентраторе событий, а одна секция — к локальному ограничению единицы пропускной способности.
    
    Способ устранения: пересмотреть стратегию распределения нагрузки по секциям или попробовать воспользоваться [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient).

2. Пространство имен концентраторов событий не имеет достаточного количества единиц пропускной способности (вы можете проверить экран **метрик** в окне пространства имен концентраторов событий в [портал Azure](https://portal.azure.com) для подтверждения). На портале отображается агрегированная (1 минута) информация, но мы измеряем пропускную способность в реальном времени, так что это лишь оценка.

    Способ устранения: увеличить число единиц пропускной способности для пространства имен. Эту операцию можно выполнить на портале в окне **Масштаб** экрана пространства имен решения "Центры событий". Можно также использовать [автоматическое расширение](event-hubs-auto-inflate.md).

#### <a name="error-code-50001"></a>Код ошибки 50001

Эта ошибка не должна возникать часто. Она происходит, когда контейнеру, выполняющему код для пространства имен, не хватает ресурсов ЦП — всего за пару секунд до запуска подсистемы балансировки нагрузки Центров событий.

#### <a name="limit-on-calls-to-the-getruntimeinformation-method"></a>Ограничить вызовы метода GetRuntimeInformation
Концентраторы событий Azure поддерживают до 50 вызовов в секунду в Жетрунтимеинфо в секунду. При достижении предела может появиться исключение, аналогичное следующему:

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```

## <a name="connectivity-certificate-or-timeout-issues"></a>Проблемы с подключением, сертификатом или временем ожидания
Следующие шаги могут помочь при устранении неполадок с подключением, сертификатами и временем ожидания для всех служб в каталоге *. servicebus.windows.net. 

- Перейдите к `https://<yournamespacename>.servicebus.windows.net/`или [wget](https://www.gnu.org/software/wget/) . Он помогает проверять наличие IP-фильтрации, виртуальной сети или цепочки сертификатов (чаще всего при использовании пакета SDK для Java).

    Пример успешного сообщения:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Пример сообщения об ошибке сбоя:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Выполните следующую команду, чтобы проверить, заблокирован ли порт в брандмауэре. Используются порты 443 (HTTPS), 5671 (AMQP) и 9093 (Kafka). В зависимости от используемой библиотеки также используются другие порты. Ниже приведен пример команды, которая проверяет, заблокирован ли порт 5671.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    В Linux

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- При наличии периодических проблем с подключением выполните следующую команду, чтобы проверить наличие пропущенных пакетов. Эта команда попытается установить 25 разных TCP-подключений каждые 1 секунду со службой. После этого можно проверить, сколько из них прошло успешное выполнение и завершилось сбоем, а также увидеть задержку подключения TCP. Вы можете скачать средство `psping` [отсюда.](/sysinternals/downloads/psping)

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    Аналогичные команды можно использовать, если вы используете другие средства, такие как `tnc`, `ping`и т. д. 
- Найдите трассировку сети, если предыдущие шаги не помогают и не анализируют их с помощью таких средств, как [Wireshark](https://www.wireshark.org/). При необходимости обратитесь в [Служба поддержки Майкрософт](https://support.microsoft.com/) . 

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Центрах событий см. в следующих источниках:

* [Общие сведения о Центрах событий](event-hubs-what-is-event-hubs.md)
* [Создание концентратора событий](event-hubs-create.md)
* [Часто задаваемые вопросы о Центрах событий](event-hubs-faq.md)
