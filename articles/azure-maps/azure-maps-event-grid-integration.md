---
title: Реагирование на отображение событий с помощью сетки событий | Карты Microsoft Azure
description: В этой статье вы узнаете, как реагировать на события Microsoft Azure Maps с помощью сетки событий.
author: farah-alyasari
ms.author: v-faalya
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: f7e5be8d4aca85c9574db18ec9df8a7f30850d06
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210112"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Реагирование на события Azure Maps c помощью Сетки событий 

Azure Maps интегрируется со службой "Сетка событий Azure", чтобы пользователи могли отправлять уведомления о событиях в другие службы и запускать нисходящие процессы. Цель этой статьи — помочь вам настроить бизнес-приложения для прослушивания Azure Maps событий. Это позволяет пользователям реагировать на критические события в надежном, масштабируемом и безопасном режиме. Например, пользователи могут создавать приложения для обновления базы данных, создавать билеты и предоставлять уведомления по электронной почте при каждом входе устройства в геозоны.

Сетка событий Azure — это полностью управляемая служба маршрутизации событий, использующая модель публикации и подписки. Служба "Сетка событий" имеет встроенную поддержку служб Azure, таких как [функции Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview) и [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview). Он может передавать оповещения о событиях в службы, не относящиеся к Azure, с помощью веб-перехватчиков. Полный список обработчиков событий, которые поддерживает служба "Сетка событий", см. в статье [An introduction to Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) (Общие сведения о службе "Сетка событий Azure").


![Функциональная модель Сетки событий Azure](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Типы событий Azure Maps

Сетка событий использует [подписки на события](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) для маршрутизации сообщений о событиях подписчикам. Учетная запись Azure Maps выводит следующие типы событий. 

| Тип события | Описание |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Вызывается, когда полученные координаты были перемещены извне из заданного геозоны в в пределах |
| Microsoft.Maps.GeofenceExited | Вызывается, когда полученные координаты перемещаются из заданного геозоны в внешний |
| Microsoft.Maps.GeofenceResult | Создается каждый раз, когда запрос геозоны возвращает результат с любым состоянием. |

## <a name="event-schema"></a>Схема событий

В следующем примере показана схема для Жеофенцересулт:

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="tips-for-consuming-events"></a>Советы по потреблению событий

Приложения, которые обрабатывают события геозоны Azure Maps, должны соответствовать нескольким рекомендациям.

* Настройте несколько подписок для маршрутизации событий в один обработчик событий. Не следует предполагать, что события поступают из определенного источника. Всегда проверяйте раздел сообщения, чтобы убедиться, что сообщение пришло от предполагаемого источника.
* Используйте поле `X-Correlation-id` в заголовке ответа, чтобы узнать, не устарели ли ваши сведения об объектах. Сообщения могут прибывать не по порядку или с задержкой.
* При вызове запроса GET или POST в API геозоны с параметром mode, для которого задано значение `EnterAndExit`, создается событие Enter или Exit для каждой геометрии в геозоны, для которой состояние изменилось с предыдущего вызова геозоны API.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о том, как использовать геозоны для операций управления на сайте конструкции, см. в статье

> [!div class="nextstepaction"] 
> [Настройка геозоны с использованием Azure Maps](tutorial-geofence.md)
