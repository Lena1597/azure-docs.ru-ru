---
title: включение файла
description: включение файла
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: dc7c86ff1df48f9ce96769098f7aab76d33c8822
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68481557"
---
Центры событий обеспечивают потоковую передачу сообщений так, чтобы каждый секционированный потребитель считывал только определенное подмножество (секцию) потока сообщений. Этот шаблон обеспечивает горизонтальное масштабирование для обработки событий и предоставляет другие функции, ориентированные на поток, которые недоступны для очередей и разделов.

Секция — это упорядоченная последовательность событий, хранящаяся в концентраторе событий. По мере поступления новых событий они добавляются в конец этой последовательности. Секцию можно рассматривать как «журнал фиксации».

![Концентраторы событий](./media/event-hubs-partitions/partition.png)

Центры событий сохраняют данные в течение указанного времени хранения, которое определяется для всех секций в концентраторе событий. События удаляются по истечении срока действия; невозможно явно удалить их. Так как секции независимы и содержат собственные последовательности данных, они часто расширяются с разной скоростью.

![Концентраторы событий](./media/event-hubs-partitions/multiple-partitions.png)

Количество секций определяется во время создания. Рабочий диапазон — от 2 до 32. Так как число секций неизменно, вам следует продумать масштаб заранее. Секции — это способ организации данных, который соотносится со степенью параллелизма подчиненных элементов, требуемой для работы потребляющих приложений. Поэтому количество секций в концентраторе событий непосредственно связано с предполагаемым числом параллельных модулей чтения. Но вы можете увеличить число секций, превысив максимальное ограничение (32 секции). Для этого обратитесь к группе разработчиков Центров событий.

Вы можете задать для него максимально возможное значение, которое равно 32 во время создания. Помните, что наличие нескольких секций приведет к тому, что события отправляются в несколько секций без изменения порядка, если только отправители не настроили отправку в один раздел из 32, оставив оставшиеся 31 секции избыточными. В первом случае придется считывать события по всем 32 секциям. В последнем случае нет очевидных дополнительных затрат, помимо дополнительной настройки, которую необходимо внести на узле обработчика событий.

Хотя секции можно идентифицировать и отправлять напрямую, это не рекомендуется делать. Вместо этого можно использовать конструкции более высокого уровня, представленные в разделе [издателей событий](../articles/event-hubs/event-hubs-features.md#event-publishers) . 

Секции заполнены последовательностями данных событий, которые содержат текст события, контейнер определяемых пользователем свойств, а также метаданные события, включая сведения о смещении в секции и ее номер в последовательности потока.

Для достижения оптимального масштаба рекомендуется сбалансировать единицы пропускной способности 1:1 и секции. Один раздел имеет гарантированный входной и выходной трафик до одной единицы пропускной способности. Хотя вы можете достичь более высокой пропускной способности секции, производительность не гарантируется. Поэтому настоятельно рекомендуется, чтобы количество секций в концентраторе событий не превышало число единиц пропускной способности.

Учитывая общую пропускную способность, которую вы планируете использовать, вы узнаете необходимое количество единиц пропускной способности и минимальное количество секций, но сколько разделов должно быть? Выберите количество секций на основе подчиненного параллелизма, который требуется достичь, а также будущих потребностей в пропускной способности. Плата за количество разделов в концентраторе событий не взимается.

Дополнительные сведения о секциях и компромиссе между доступностью и надежностью см. в статье [Руководство по программированию Центров событий](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) и [Доступность и согласованность в Центрах событий](../articles/event-hubs/event-hubs-availability-and-consistency.md).
