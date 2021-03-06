---
title: Мониторинг Azure Cosmos DB с помощью Azure Monitor для Cosmos DB (Предварительная версия) | Документация Майкрософт
description: В этой статье описывается Azure Monitor для Cosmos DB функции, которая предоставляет владельцам Cosmos DB краткие сведения о проблемах с производительностью и использованием учетных записей CosmosDB.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/27/2019
ms.openlocfilehash: 8e265b592bebfc506ae0116c955403dd1070ad3f
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73166412"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db-preview"></a>Просмотр Azure Monitor для Azure Cosmos DB (Предварительная версия)

Azure Monitor для Azure Cosmos DB (Предварительная версия) предоставляет общее представление о производительности, сбоях, емкости и работоспособности всех ресурсов Azure Cosmos DB в едином интерактивном интерфейсе. Эта статья поможет вам понять преимущества этого нового средства мониторинга, а также изменить и адаптировать интерфейс в соответствии с потребностями вашей организации.   

## <a name="introduction"></a>Общие сведения

Прежде чем углубляться в опыт, необходимо понять, как она представляет и визуализирует информацию. 

Он предоставляет:

* С **точки зрения масштабирования** ваших Azure Cosmos DBных ресурсов во всех подписках в одном расположении с возможностью выборочной области только для тех подписок и ресурсов, которые вы заинтересованы в оценке.

* **Детализированный анализ** конкретного ресурса Azure CosmosDB для диагностики проблем или выполнения подробного анализа по использованию категорий, сбоев, емкости и операциям. Выбор любого из этих параметров обеспечивает подробное представление соответствующих метрик Azure Cosmos DB.  

* **Настраиваемый** . Этот интерфейс основан на Azure Monitor шаблонах книг, позволяющих изменить отображаемые метрики, изменить или задать пороговые значения, согласованные с вашими ограничениями, а затем сохранить их в пользовательской книге. Диаграммы в книгах можно закреплять на панелях мониторинга Azure.  

Эта функция не требует включения или настройки. Эти метрики Azure Cosmos DB собираются по умолчанию.

>[!NOTE]
>Плата за доступ к этой функции не взимается, и плата взимается только за Azure Monitorные дополнительные функции, которые вы настраиваете или включаете, как описано на странице [сведений о ценах на Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/) .


## <a name="accessing-azure-monitor-for-azure-cosmos-db"></a>Доступ к Azure Monitor для Azure Cosmos DB

Чтобы просмотреть сведения об использовании и производительности учетных записей хранения во всех подписках, выполните следующие действия.

1. Войдите на [портале Azure](https://portal.azure.com).

2. Найдите **монитор** и выберите **монитор**.

    ![Поле поиска со словом "монитор" и раскрывающимся списком "службы" с изображением в стиле спидометр](./media/cosmosdb-insights-overview/search-monitor.png)

3. Выберите **Cosmos DB (Предварительная версия)** .

    ![Снимок экрана с книгой обзора Cosmos DB](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Краткое описание

В разделе **Обзор**в таблице отображаются интерактивные Azure Cosmos DB метрики. Результаты можно фильтровать на основе параметров, выбранных в следующих раскрывающихся списках:

* Только **подписки** . перечислены подписки, имеющие ресурс Azure Cosmos DB.  

* **Cosmos DB** — можно выбрать все, подмножество или отдельный Azure Cosmos DB ресурс.

* **Диапазон времени** — по умолчанию отображает данные за последние 4 часа на основе соответствующих параметров.

Плитка счетчика в раскрывающихся списках содержит общее число Azure Cosmos DB ресурсов в выбранных подписках. Существует условное выделение цветом или тепловые карты для столбцов в книге, которые сообщают метрики транзакций. Самый глубокий цвет имеет наибольшее значение, а светлый — на основе наименьших значений. 

Если выбрать стрелку раскрывающегося списка рядом с одним из Azure Cosmos DB ресурсов, будет отображена разбивка метрик производительности на уровне отдельных контейнеров базы данных.

![Развернутое раскрывающееся раскрывающийся список отдельных контейнеров баз данных и связанных с ними структур производительности](./media/cosmosdb-insights-overview/container-view.png)

Если выбрать имя ресурса Azure Cosmos DB, выделенное синим цветом, откроется **Обзор** по умолчанию для связанной учетной записи Azure Cosmos DB. 

### <a name="failures"></a>Сбои

В верхней части страницы выберите **сбои** , после чего откроется часть шаблона книги " **ошибки** ". Он показывает общее количество запросов с распределением ответов, которые составляют эти запросы:

![Снимок экрана сбоев с разбивкой по типу запроса HTTP](./media/cosmosdb-insights-overview/failures.png)

| Код      |  Описание       | 
|-----------|:--------------------|
| `200 OK`  | Одна из следующих операций RESTFUL была успешной: </br>— ПОЛУЧЕНИЕ ресурса. </br> — РАЗМЕСТИТЬ на ресурсе. </br> -POST для ресурса. </br> — POST для ресурса хранимой процедуры для выполнения хранимой процедуры.|
| `201 Created` | Операция POST для создания ресурса выполнена успешно. |
| `404 Not Found` | Операция пытается выполнить действия с ресурсом, который больше не существует. Например, ресурс уже мог быть удален. |

Полный список кодов состояния см. в [статье Azure Cosmos DB код состояния HTTP](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

### <a name="capacity"></a>Ориентированное на объем

В верхней части страницы выберите **емкость** , после чего откроется часть **емкости** шаблона книги. Он показывает, сколько документов имеется, рост документа с течением времени, использование данных и общий объем доступного хранилища, которое осталось.  Это можно использовать для выявления потенциальных проблем с хранилищем и использованием данных.

![Книга емкости](./media/cosmosdb-insights-overview/capacity.png) 

Как и в случае с книгой "Обзор", выбор раскрывающегося списка рядом с Azure Cosmos DBным ресурсом в столбце **подписки** раскрывает подразделение по отдельным контейнерам, которые составляют базу данных.

### <a name="operations"></a>Operations 

В верхней части страницы выберите **операции** , после чего откроется часть шаблона книги **операции** . Она дает возможность видеть запросы, разделенные по типам запросов. 

Итак, в приведенном ниже примере показано, что `eastus-billingint` в основном получает запросы на чтение, но с небольшим количеством Upsert и запросов на создание. В то время как `westeurope-billingint` доступен только для чтения с точки запроса, по крайней мере за последние четыре часа, на которые в настоящий момент находится книга, с помощью параметра временной области.

![Книга операций](./media/cosmosdb-insights-overview/operation.png) 

## <a name="pin-export-and-expand"></a>Закрепление, экспорт и расширение

Вы можете закрепить один из разделов метрик на [панели мониторинга Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) , щелкнув значок канцелярской кнопки в правом верхнем углу раздела.

![Пример закрепления раздела метрики на панели мониторинга](./media/cosmosdb-insights-overview/pin.png)

Чтобы экспортировать данные в формат Excel, щелкните значок со стрелкой вниз слева от значка канцелярской кнопки.

![Значок экспорта книги](./media/cosmosdb-insights-overview/export.png)

Чтобы развернуть или свернуть все раскрывающиеся представления в книге, щелкните значок развертывания слева от значка экспорта:

![Развернуть значок книги](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db-preview"></a>Настройка Azure Monitor для Azure Cosmos DB (Предварительная версия)

Поскольку этот интерфейс основан на Azure Monitor шаблонах книг, вы можете **настроить** > **редактирования** и **сохранения** копии измененной версии в пользовательской книге. 

![Панель настройки](./media/cosmosdb-insights-overview/customize.png)

Книги сохраняются в группе ресурсов либо в разделе **Мои отчеты** , который является частным, либо в разделе **Общие отчеты** , доступном всем пользователям с доступом к группе ресурсов. После сохранения пользовательской книги необходимо перейти в коллекцию книг, чтобы запустить ее.

![Запуск коллекции книг на панели команд](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Дальнейшие действия

* Настройте [оповещения метрик](../platform/alerts-metric.md) и [уведомления о работоспособности службы](../../service-health/alerts-activity-log-service-notifications.md) , чтобы настроить автоматическое оповещение для облегчения обнаружения проблем.

* Ознакомьтесь с книгами сценариев, предназначенными для поддержки, создания новых и настройки существующих отчетов и многого другого, изучив [Создание интерактивных отчетов с помощью Azure Monitor книг](../app/usage-workbooks.md).
