---
title: Событие завершения изменения размера пула пакетной службы Azure
description: Справочник по событию завершения изменения размера пула пакетной службы. См. Пример пула, размер которого увеличился и успешно завершен.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: e2c66471ad9fe8d917d1ffddceb6e01c339d62dd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022228"
---
# <a name="pool-resize-complete-event"></a>Событие завершения изменения размера пула

 Это событие создается при завершении или сбое операции изменения размера пула.

 Следующий пример показывает текст для события завершения изменения размера пула, когда размер пула успешно увеличивается.

```
{
    "id": "myPool",
    "nodeDeallocationOption": "invalid",
        "currentDedicatedNodes": 10,
        "targetDedicatedNodes": 10,
    "currentLowPriorityNodes": 5,
        "targetLowPriorityNodes": 5,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "resultCode": "Success",
    "resultMessage": "The operation succeeded"
}
```

|Элемент|Тип|Примечания|
|-------------|----------|-----------|
|`id`|String|Идентификатор пула.|
|`nodeDeallocationOption`|String|Указывает, когда можно удалить узлы из пула, если его размер уменьшается.<br /><br /> Возможны следующие значения:<br /><br /> **requeue** — завершает выполняющиеся задачи и повторно ставит их в очередь. Задачи будут запущены повторно при включении задания. Узлы удаляются сразу после завершения задач.<br /><br /> **terminate** — завершает выполняющиеся задачи. Задачи не будут запущены повторно. Узлы удаляются сразу после завершения задач.<br /><br /> **taskcompletion** — разрешает завершить текущие выполняющиеся задачи. Во время ожидания новые задачи не планируются. Узлы удаляются после выполнения всех задач.<br /><br /> **Retaineddata** — разрешает завершить текущие выполняющиеся задачи и затем ожидает, пока истекут сроки хранения данных для всех задач. Во время ожидания новые задачи не планируются. Узлы удаляются после истечения сроков хранения для всех задач.<br /><br /> По умолчанию используется значение requeue.<br /><br /> Если размер пула увеличивается, устанавливается значение **invalid**.|
|`currentDedicatedNodes`|Int32|Число выделенных кластерных узлов, которые в настоящее время назначены пулу.|
|`targetDedicatedNodes`|Int32|Число выделенных единиц вычислений, запрашиваемых для пула.|
|`currentLowPriorityNodes`|Int32|Число узлов вычислений с низким приоритетом, которые в настоящее время назначены пулу.|
|`targetLowPriorityNodes`|Int32|Количество расчетных узлов с низким приоритетом, запрошенных для пула.|
|`enableAutoScale`|Bool|Указывает, корректируется ли размер пула автоматически с течением времени.|
|`isAutoPool`|Bool|Определяет, создан ли пул с помощью механизма AutoPool задания.|
|`startTime`|Дата и время|Время, когда было начато изменение размера пула.|
|`endTime`|Дата и время|Время, когда изменение размера пула было завершено.|
|`resultCode`|String|Результат изменения размера.|
|`resultMessage`|String| Подробное сообщение о результате.<br /><br /> Если размер успешно изменен, сообщается, что операция выполнена.|
