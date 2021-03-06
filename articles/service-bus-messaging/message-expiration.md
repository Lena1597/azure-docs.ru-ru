---
title: Служебная шина Azure — срок действия сообщения
description: В этой статье объясняется срок действия сообщений служебной шины Azure и время их жизни. После такого крайнего срока сообщение больше не будет доставлено.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: e86c92fa1cfb13929d5617502224f479709efdd3
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2020
ms.locfileid: "76756340"
---
# <a name="message-expiration-time-to-live"></a>Срок действия сообщения (срок жизни)

Полезные данные в сообщении либо команда или запрос, которые сообщение передает получателю, почти всегда имеют срок действия, который тем или иным образом определяется на уровне приложения. По прошествии данного крайнего срока содержимое уже не доставляется или не выполняется запрошенная операция.

Для среды разработки и тестовой среды, в которых очереди и разделы часто используются в контексте частичного запуска приложений или частей приложений, желательно также выполнять автоматическую сборку мусора для удаления потерянных тестовых сообщений, чтобы следующий тест можно было выполнить "с чистого листа".

Истечением срока действия любого отдельного сообщения можно управлять, задав системное свойство [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive), которое указывает относительный срок. Срок действия истекает мгновенно, когда сообщение помещается в очередь в сущности. В этот момент свойство [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) принимает значение [ **(EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**)](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive). Параметр срока жизни (TTL) для сообщения через посредника не применяется, если клиенты не ожидают активного прослушивания.

После того, как момент **ExpiresAtUtc** пройдет, сообщения станут недоступными для получения. Срок действия не влияет на сообщения, доставка которых в настоящий момент заблокирована. Эти сообщения по-прежнему обрабатываются обычным образом. Если срок действия блокировки истекает или сообщение отбрасывается, его срок действия вступает в силу немедленно.

Во время блокировки сообщения приложение может посчитать, что его срок действия истек. Будет ли приложение обрабатывать это сообщение или отбросит его, определяется разработчиком.

## <a name="entity-level-expiration"></a>Срок действия уровня сущности

Для всех сообщений, отправленных в очередь или раздел, применяется срок действия по умолчанию, который задается на уровне сущности с помощью свойства [defaultMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues). Его можно также указать на портале во время создания и затем изменить. Срок действия по умолчанию используется для всех сообщений, отправленных в сущность, для которой значение [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) не задано явным образом. Срок действия по умолчанию задает максимальное значение для **TimeToLive**. Для сообщений, значение **TimeToLive** которых превышает срок действия по умолчанию, автоматически применяется значение **defaultMessageTimeToLive**, прежде чем они попадают в очередь.

> [!NOTE]
> Значение [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) по умолчанию для сообщения с посредником — [TimeSpan. Max](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue) , если не указано иное.
>
> Для сущностей обмена сообщениями (очередей и разделов) время истечения срока действия по умолчанию равно также [TimeSpan. Max](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue) для уровней "Стандартный" и "Премиум" служебной шины.  Для уровня "базовый" срок действия по умолчанию составляет 14 дней.

Просроченные сообщения при необходимости можно переместить в [очередь недоставленных сообщений](service-bus-dead-letter-queues.md), задав свойство [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) или установив соответствующий флажок на портале. Если этот параметр оставить отключенным, то просроченные сообщения будут удаляться. Просроченные сообщения, перемещенные в очередь недоставленных сообщений, можно отличить от других недоставленных сообщений, оценивая свойство [DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq), которое брокер сохраняет в разделе свойств пользователя. В данном случае оно имеет значение [TTLExpiredException](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq).

В упомянутом выше случае, в котором сообщение защищено от истечения срока действия благодаря блокировке, если для сущности установлен соответствующий флаг, то это сообщение автоматически перемещается в очередь недоставленных сообщений после того, как блокировка будет снята или истечет срок ее действия. Однако оно не перемещается в эту очередь, если сообщение успешно согласовано, после чего предполагается, что приложение успешно его обработало, несмотря на то, что номинально его срок действия истек.

Сочетание [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) и автоматического (и транзакционного) перемещения в очередь недоставленных сообщений по истечении срока действия — ценный инструмент, позволяющий гарантировать, что задание с крайним сроком, переданное обработчику или группе обработчиков, будет получено для обработки к этому крайнему сроку.

Для примера рассмотрим веб-сайт, который должен надежно выполнять задания в серверной части с ограниченными возможностями масштабирования и который иногда подвержен скачкам трафика или должен быть независимым от доступности этой серверной части. В обычном случае обработчик на стороне сервера, предназначенный для отправленных данных пользователя, передает информацию в очередь и затем получает ответ, подтверждающий успешную обработку транзакций в очереди ответов. Если происходит скачок трафика и внутренний обработчик не может обработать свои элементы невыполненной работы вовремя, то просроченные задания возвращаются в очередь недоставленных сообщений. Текущий пользователь может быть оповещен о том, что запрошенная операция займет немного больше времени, чем обычно, и что затем запрос можно будет поместить в другую очередь для обработки. В итоге результат обработки отправляется пользователю по электронной почте. 


## <a name="temporary-entities"></a>Временные сущности

Очереди, разделы и подписки служебной шины могут создаваться как временные сущности, которые автоматически удаляются, если не используются в течение указанного периода времени.
 
Автоматическая очистка удобна в сценариях разработки и тестирования, в которых создаются динамически сущности, которые не очищаются после использования из-за прерывания цикла тестирования или отладки. Ее также удобно использовать, когда приложение создает динамические сущности, такие как очередь ответов, для получения ответов процессом веб-сервера или в другим объектом с относительно небольшим временем существования. После того, как экземпляр объекта исчезает, надежно очищать эти сущности сложно.

Эта функция включается с помощью свойства [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues). Для этого свойства задается значение времени, в течение которого сущность должна быть неактивной (не использоваться) и по истечении которого она автоматически удаляется. Минимальное значение свойства — 5.
 
Свойство **autoDeleteOnIdle** должно быть установлено посредством операции Azure Resource Manager или с помощью интерфейсов API [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) клиента .NET Framework. Его нельзя настроить на портале.

## <a name="idleness"></a>Неактивность

Ниже описаны условия, которые определяют неактивность сущностей (очередей, разделов и подписок).

- Очереди
    - Нет отправок.  
    - Нет получений.  
    - Нет обновлений очереди.  
    - Нет запланированных сообщений.  
    - Нет просмотров или обзоров. 
- Разделы  
    - Нет отправок.  
    - Нет обновлений раздела.  
    - Нет запланированных сообщений. 
- Подписки
    - Нет получений.  
    - Нет обновлений подписки.  
    - В подписке нет новых добавленных правил.  
    - Нет просмотров или обзоров.  
 


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об обмене сообщениями через служебную шину см. в следующих статьях:

* [Очереди, разделы и подписки служебной шины](service-bus-queues-topics-subscriptions.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)
* [Как использовать разделы и подписки служебной шины](service-bus-dotnet-how-to-use-topics-subscriptions.md)
