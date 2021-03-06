---
title: Справочник по API агента Azure Application Insights
description: Справочник по API агента Application Insights. Enable-Инструментатионенгине. Отслеживайте производительность веб-сайта без повторного развертывания веб-сайта. Работает с веб-приложениями ASP.NET, размещенными локально, в виртуальных машинах или в Azure.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 796c2cc669e238499223d233cf4ddcf740af7c95
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899720"
---
# <a name="application-insights-agent-api-enable-instrumentationengine"></a>API агента Application Insights: enable-Инструментатионенгине

В этой статье описывается командлет, который является членом [модуля PowerShell AZ. аппликатионмонитор](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Описание

Включает модуль инструментирования, настроив некоторые разделы реестра.
Перезапустите IIS, чтобы изменения вступили в силу.

Модуль инструментирования может дополнять данные, собираемые пакетами SDK для .NET.
Он собирает события и сообщения, описывающие выполнение управляемого процесса. Эти события и сообщения включают в себя коды результатов зависимостей, глаголы HTTP и [текст команды SQL](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query).

Включите модуль инструментирования, если:
- Вы уже включили наблюдение с помощью командлета Enable, но не включили модуль инструментирования.
- Вы вручную разрешили свое приложение с помощью пакетов SDK для .NET и хотите получить дополнительные данные телеметрии.

> [!IMPORTANT] 
> Для этого командлета требуется сеанс PowerShell с разрешениями администратора.

> [!NOTE] 
> - Для этого командлета необходимо проверить и принять нашу лицензию и заявление о конфиденциальности.
> - Модуль инструментирования добавляет дополнительные издержки и по умолчанию отключен.

## <a name="examples"></a>Примеры

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Параметры

### <a name="-acceptlicense"></a>-AcceptLicense
**Необязательный параметр.** Используйте этот параметр, чтобы принять условия лицензии и конфиденциальности в установках без монитора.

### <a name="-verbose"></a>-Verbose
**Общий параметр.** Используйте этот параметр для вывода подробных журналов.

## <a name="output"></a>Выходные данные


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Пример выходных данных для успешного включения модуля инструментирования

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Дальнейшие действия

  Просмотр телеметрии:
 - [Изучите метрики](../../azure-monitor/app/metrics-explorer.md) для мониторинга производительности и использования.
- [Поиск событий и журналов](../../azure-monitor/app/diagnostic-search.md) для диагностики проблем.
- Используйте [аналитику](../../azure-monitor/app/analytics.md) для более сложных запросов.
- [Создание панелей мониторинга](../../azure-monitor/app/overview-dashboard.md).
 
 Добавление данных телеметрии:
 - [Создайте веб-тесты](monitor-web-app-availability.md) , чтобы убедиться, что ваш сайт остается активным.
- [Добавьте данные телеметрии веб-клиента](../../azure-monitor/app/javascript.md) , чтобы просмотреть исключения из кода веб-страницы и включить вызовы трассировки.
- [Добавьте в код пакет SDK для Application Insights](../../azure-monitor/app/asp-net.md) , чтобы можно было вставить вызовы трассировки и журнала.
 
 Другие действия с агентом Application Insights:
 - Используйте наше справочное по для [устранения неполадок](status-monitor-v2-troubleshoot.md) агента Application Insights.
 - [Получите конфигурацию](status-monitor-v2-api-get-config.md) , чтобы убедиться, что параметры записаны правильно.
 - [Получение состояния](status-monitor-v2-api-get-status.md) для проверки мониторинга.
