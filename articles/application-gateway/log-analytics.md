---
title: Проверка журналов WAF с помощью Azure Log Analytics
titleSuffix: Azure Application Gateway
description: В этой статье показано, как можно использовать Log Analytics Azure для проверки журналов брандмауэра веб-приложения шлюза приложений.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 9fe4462a71852e5f66268f798f6f0418f2dd39c4
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048082"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Использование Log Analytics для проверки журналов брандмауэра веб-приложения Шлюза приложений

После работоспособности WAF шлюза приложений можно включить журналы, чтобы проверить, что происходит с каждым запросом. Журналы брандмауэра дают представление об оценке, сопоставлении и блокировке WAF. С помощью Log Analytics можно просмотреть данные в журналах брандмауэра, чтобы получить еще больше ценных сведений. Дополнительные сведения о создании рабочей области Log Analytics см. в разделе [создание log Analytics рабочей области на портал Azure](../azure-monitor/learn/quick-create-workspace.md). Дополнительные сведения о запросах журналов см. [в разделе Общие сведения о запросах журналов в Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="import-waf-logs"></a>Импорт журналов WAF

Чтобы импортировать журналы брандмауэра в Log Analytics, см. раздел [работоспособность серверной части, журналы диагностики и метрики для шлюза приложений](application-gateway-diagnostics.md#diagnostic-logging). При наличии журналов брандмауэра в рабочей области Log Analytics можно просматривать данные, записывать запросы, создавать визуализации и добавлять их на панель мониторинга портала.

## <a name="explore-data-with-examples"></a>Просмотр данных с помощью примеров

Для просмотра необработанных данных в журнале брандмауэра можно выполнить следующий запрос:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Он будет выглядеть следующим образом:

![Запрос Log Analytics](media/log-analytics/log-query.png)

Вы можете детализировать данные, построить графики или создать визуализации. В качестве отправной точки см. следующие запросы:

### <a name="matchedblocked-requests-by-ip"></a>Совпадающие или заблокированные запросы по IP-адресу

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>Совпадающие или заблокированные запросы по URI

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>Правила с наибольшим соответствием

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>Пять основных сопоставленных групп правил

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Добавить на панель мониторинга

После создания запроса его можно добавить на панель мониторинга.  Выберите **закрепить на панели мониторинга** в правом верхнем углу рабочей области log Analytics. С помощью предыдущих четырех запросов, закрепленных на примере панели мониторинга, можно быстро увидеть данные:

![Панель мониторинга](media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Дополнительная информация

[Работоспособность серверной части, журналы диагностики и метрики для шлюза приложений](application-gateway-diagnostics.md)
