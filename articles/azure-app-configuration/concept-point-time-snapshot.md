---
title: Моментальный снимок точки конфигурации приложения Azure
description: Общие сведения о функционировании моментального снимка на определенный момент времени в службе "Конфигурация приложений Azure"
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.openlocfilehash: 4a352ba913b6ad4e3c8607677078e21070f294fd
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899590"
---
# <a name="point-in-time-snapshot"></a>Моментальный снимок на определенный момент времени

В службе "Конфигурация приложений Azure" сохраняются записи с точным временем создания новой пары "ключ — значение" и ее изменения в дальнейшем. Эти записи образуют полную временную шкалу изменений пары "ключ — значение". С помощью хранилища Конфигурации приложений можно восстановить журнал пары "ключ — значение" и воспроизвести ее последнее значение в любой момент времени до текущего. Эта функция позволяет "переместиться назад во времени" и получить старую пару "ключ — значение". Например, вы можете получить вчерашние параметры конфигурации, которые были активны непосредственно перед последним развертыванием, чтобы восстановить предыдущую конфигурацию и откатить приложение.

## <a name="key-value-retrieval"></a>Получение пары "ключ — значение"

Для получения прошлой пары "ключ — значение" укажите время, когда был создан моментальный снимок этой пары, в заголовке HTTP вызова REST API. Пример.

```rest
GET /kv HTTP/1.1
Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT
```

Сейчас в службе "Конфигурация приложений" сохраняется журнал изменений за 7 дней.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание веб-приложения ASP.NET Core](./quickstart-aspnet-core-app.md)  
