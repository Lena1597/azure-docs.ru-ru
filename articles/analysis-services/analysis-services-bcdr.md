---
title: Высокая доступность служб Azure Analysis Services | Документы Майкрософт
description: В этой статье описывается, как Azure Analysis Services обеспечивает высокий уровень доступности во время прерывания работы службы.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2e750dce804ea93f3d3068ffd36bc7a73a50906a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73573359"
---
# <a name="analysis-services-high-availability"></a>Высокая доступность служб Analysis Services

Эта статья описывает обеспечение высокой доступности для серверов служб Azure Analysis Services. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Обеспечение высокой доступности во время перерывов в обслуживании

В редких случаях возможен сбой центра обработки данных Azure. Он вызывает нарушение работы организации, которое может длиться от считанных минут до нескольких часов. Высокая доступность чаще всего достигается за счет избыточности сервера. При работе со службами Azure Analysis Services избыточности можно добиться путем создания дополнительных серверов-получателей в одном или нескольких регионах. При создании избыточных серверов синхронизировать их данные и метаданные с сервером в регионе, где произошло отключение, можно несколькими способами:

* Разверните модели на избыточных серверах в других регионах. При таком методе для обеспечения общей синхронизации требуется параллельная обработка данных как на сервере-источнике, так и на избыточных серверах.

* [Выполните резервное копирование](analysis-services-backup.md) баз данных с сервера-источника и восстановите их на избыточных серверах. Например, можно настроить регулярную и автоматическую архивацию в службу хранилища Azure с восстановлением на избыточных серверах в других регионах. 

В любом случае при отключении сервера-источника нужно изменить строки подключения в затронутых клиентах, чтобы подключиться к серверу в другом региональном центре обработки данных. Такое изменение следует рассматривать как крайнюю меру на случай катастрофического сбоя в региональном центре обработки данных. Вероятнее всего, центр с вашим сервером-источником возобновит работу до того, как вы внесете изменения для всех клиентов. 

Чтобы избежать необходимости изменять строки подключения в затронутых клиентах, можно создать [псевдоним](analysis-services-server-alias.md) для сервера-источника. Если сервер-источник выходит из строя, можно изменить псевдоним, чтобы он указывал на запасной сервер в другом регионе. Это изменение псевдонима можно автоматизировать путем написания кода для проверки работоспособности конечной точки на сервере-источнике. При сбое проверки работоспособности та же конечная точка будет направлять на запасной сервер в другом регионе. 

## <a name="related-information"></a>Связанные сведения

[Архивация и восстановление](analysis-services-backup.md)   
[Управление службами Azure Analysis Services](analysis-services-manage.md)   
[Псевдонимы сервера](analysis-services-server-alias.md) 

