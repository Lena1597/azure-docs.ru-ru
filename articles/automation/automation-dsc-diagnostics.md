---
title: Пересылка данных отчетов о настройке состояния службы автоматизации Azure в журналы Azure Monitor
description: В этой статье показано, как отправить данные отчетов о настройке требуемого состояния (DSC) из конфигурации состояния службы автоматизации Azure в журналы Azure Monitor для предоставления дополнительных сведений и управления.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 69801909c6bc8d215ca7dd3ccb7ac349201e8774
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198570"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-azure-monitor-logs"></a>Пересылка данных отчетов о настройке состояния службы автоматизации Azure в журналы Azure Monitor

В конфигурации состояния службы автоматизации Azure данные о состоянии узла сосохранены в течение 30 дней.
Вы можете отправить данные о состоянии узла в рабочую область Log Analytics, если вы предпочитаете хранить эти данные в течение более длительного периода.
Сведения о состоянии соответствий узлов и отдельных ресурсов DSC в конфигурации узла можно просмотреть на портале Azure или с помощью PowerShell.
С помощью журналов Azure Monitor можно:

- получать сведения о соответствии требованиям для управляемых узлов и отдельных ресурсов;
- активировать сообщение электронной почты или предупреждение (в зависимости от состояния соответствия);
- создавать сложные запросы к управляемым узлам;
- сопоставлять состояние соответствия в учетных записях службы автоматизации;
- визуализировать журнал соответствия узла с течением времени.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Предварительные требования

Чтобы начать отправку отчетов о конфигурации состояния автоматизации в журналы Azure Monitor, вам потребуется:

- Выпуск за ноябрь 2016 года или более поздний выпуск [Azure PowerShell](/powershell/azure/overview) (вер. 2.3.0).
- Учетная запись службы автоматизации Azure. Дополнительные сведения см. в статье [Введение в службу автоматизации Azure](automation-intro.md).
- Рабочая область Log Analytics с предложением службы управления & автоматизации. Дополнительные сведения см. в статье [Начало работы с Log Analytics в Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).
- Как минимум один узел службы "Настройка состояния службы автоматизации Azure". Дополнительные сведения см. в статье подключение [компьютеров для управления с помощью конфигурации состояния службы автоматизации Azure](automation-dsc-onboarding.md).
- Модуль [xDscDiagnostics](https://www.powershellgallery.com/packages/xDscDiagnostics/2.7.0.0) версии 2.7.0.0 или более поздней. Инструкции по установке см. в разделе [Устранение неполадок настройки требуемого состояния службы автоматизации Azure](./troubleshoot/desired-state-configuration.md#steps-to-troubleshoot-desired-state-configuration-dsc).

## <a name="set-up-integration-with-azure-monitor-logs"></a>Настройка интеграции с журналами Azure Monitor

Чтобы начать импорт данных из Automation DSC Azure в журналы Azure Monitor, выполните следующие действия.

1. Войдите в свою учетную запись Azure в PowerShell. См. раздел [Вход с помощью Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
1. Получите идентификатор ресурса учетной записи службы автоматизации, выполнив следующий командлет PowerShell. Если у вас есть несколько учетных записей службы автоматизации, выберите идентификатор ресурса для учетной записи, которую требуется настроить.

   ```powershell
   # Find the ResourceId for the Automation account
   Get-AzResource -ResourceType 'Microsoft.Automation/automationAccounts'
   ```

1. Получите идентификатор ресурса рабочей области Log Analytics, выполнив следующий командлет PowerShell. Если у вас есть несколько рабочих областей, выберите идентификатор ресурса для рабочей области, которую требуется настроить.

   ```powershell
   # Find the ResourceId for the Log Analytics workspace
   Get-AzResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
   ```

1. Выполните следующий командлет PowerShell, заменив `<AutomationResourceId>` и `<WorkspaceResourceId>` значениями *ResourceId* из каждого из предыдущих шагов.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Category 'DscNodeStatus'
   ```

1. Если вы хотите отключить импорт данных из конфигурации состояния службы автоматизации Azure в журналы Azure Monitor, выполните следующий командлет PowerShell.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Category 'DscNodeStatus'
   ```

## <a name="view-the-state-configuration-logs"></a>Просмотр журналов State Configuration

После настройки интеграции с журналами Azure Monitor для данных конфигурации состояния службы автоматизации их можно просмотреть, выбрав **журналы** в разделе **мониторинг** в левой области страницы Конфигурация состояния (DSC).

![Журналы](media/automation-dsc-diagnostics/automation-dsc-logs-toc-item.png)

Откроется панель **поиска по журналам** с областью запроса, заданной для ресурса учетной записи службы автоматизации. Можно выполнять поиск в журналах конфигурации состояний для операций DSC, выполняя поиск в журналах Azure Monitor. Записи для операций DSC хранятся в таблице AzureDiagnostics. Например, чтобы найти несоответствующие узлы, введите следующий запрос.

```AzureDiagnostics
| where Category == 'DscNodeStatus' 
| where OperationName contains 'DSCNodeStatusData'
| where ResultType != 'Compliant'
```
Сведения о фильтрации:

* Фильтрация по *DscNodeStatusData* для возврата операций для каждого узла конфигурации состояния.
* Фильтрация по *дскресаурцестатусдата* для возврата операций для каждого ресурса DSC, который вызывается в конфигурации узла, примененной к этому ресурсу. 
* Примените фильтр по *дскресаурцестатусдата* , чтобы получить сведения об ошибке для всех ресурсов DSC, которые не удалось выполнить.

Дополнительные сведения о создании запросов журналов для поиска данных см. в разделе [Обзор запросов журналов в Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Отправка сообщения электронной почты при сбое проверки соответствия State Configuration

Наши клиенты часто интересуются возможностью отправки электронного сообщения или SMS-сообщения при возникновении ошибки в конфигурации DSC.

Чтобы создать правило генерации оповещений, начните с создания поиска по журналам для записей отчета о конфигурации состояния, которые должны вызывать предупреждение. Щелкните кнопку **+ Новое правило генерации оповещений**, чтобы создать и настроить правило генерации оповещений.

1. На странице обзора рабочей области Log Analytics щелкните **журналы**.
1. Поищите по журналам свое оповещение при помощи следующего поискового запроса: `Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Если вы настроили для рабочей области журналы из более чем одной учетной записи службы автоматизации или подписки, то можете группировать оповещения по подписке или учетной записи службы автоматизации. Создайте имя учетной записи службы автоматизации из поля ресурса в поиске по DscNodeStatusData.
1. Чтобы открыть экран **Создать правило**, щелкните **+ Новое правило генерации оповещений** в верхней части страницы. 

Дополнительные сведения о параметрах настройки предупреждения см. в разделе [Создание правила генерации оповещений](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Поиск ресурсов DSC со сбоями по всем узлам

Одним из преимуществ использования журналов Azure Monitor является то, что можно выполнять поиск непройденных проверок на разных узлах.
Чтобы найти все экземпляры ресурсов DSC, которые завершились сбоем, выполните следующие действия.

1. На странице обзора рабочей области Log Analytics щелкните **журналы**.
1. Поищите по журналам свое оповещение при помощи следующего поискового запроса: `Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Просмотр состояния узла DSC за предыдущие периоды

Чтобы визуализировать журнал состояния узла DSC с течением времени, можно использовать следующий запрос:

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`

В этом запросе отображается диаграмма состояния узла с течением времени.

## <a name="azure-monitor-logs-records"></a>Записи журналов Azure Monitor

Система диагностики службы автоматизации Azure создает две категории записей в журналах Azure Monitor:

* Данные о состоянии узла (DscNodeStatusData)
* Данные о состоянии ресурса (Дскресаурцестатусдата)

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Свойство | Описание |
| --- | --- |
| Время создания |Дата и время запуска проверки соответствия. |
| OperationName |DscNodeStatusData. |
| ResultType |Подтверждено ли соответствие узла. |
| NodeName_s |Имя управляемого узла. |
| NodeComplianceStatus_s |Подтверждено ли соответствие узла. |
| DscReportStatus |Успешно ли прошла проверка соответствия. |
| ConfigurationMode | Применение конфигурации к узлу. Возможны следующие значения: <ul><li>*ApplyOnly* — DSC применяет конфигурацию и не выполняет дальнейшие действия, пока новая конфигурация не будет отправлена в целевой узел или получена с сервера. После первого применения новой конфигурации DSC не проверяет наличие отклонений от настроенного ранее состояния. DSC пытается применить конфигурацию до тех пор, пока не вступит в силу значение *ApplyOnly* . </li><li>*ApplyAndMonitor* — значение по умолчанию. LCM применяет все новые конфигурации. Если после начального применения новой конфигурации состояние целевого узла отклоняется от требуемого, DSC сообщает о расхождении в журналах. DSC пытается применить конфигурацию до тех пор, пока не вступит в силу значение *ApplyAndMonitor* .</li><li>*ApplyAndAutoCorrect*: DSC применяет все новые конфигурации. Если после начального применения новой конфигурации состояние целевого узла отклоняется от требуемого, DSC сообщает о расхождении в журналах, а затем повторно применяет текущую конфигурацию.</li></ul> |
| HostName_s | Имя управляемого узла. |
| IPAddress | Адрес IPv4 управляемого узла. |
| Категория | Дскнодестатус. |
| Ресурс | Имя учетной записи службы автоматизации Azure. |
| Tenant_g | GUID, идентифицирующий клиента для вызывающего объекта. |
| NodeId_g |Идентификатор GUID для определения управляемого узла. |
| DscReportId_g |Идентификатор GUID для определения отчета. |
| LastSeenTime_t |Дата и время последнего просмотра отчета. |
| ReportStartTime_t |Дата и время начала отчета. |
| ReportEndTime_t |Дата и время завершения отчета. |
| NumberOfResources_d |Количество ресурсов DSC, которые вызывались в примененной к узлу конфигурации. |
| SourceSystem | Как Azure Monitor журналы собирают данные. Всегда "Azure" для системы диагностики Azure. |
| ResourceId |Идентификатор учетной записи службы автоматизации Azure. |
| ResultDescription | Описание этой операции. |
| SubscriptionId | Идентификатор подписки Azure (GUID) для учетной записи службы автоматизации. |
| ResourceGroup | Имя группы ресурсов для учетной записи службы автоматизации. |
| ResourceProvider | NNTP. Средствами. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |Идентификатор GUID, который является идентификатором корреляции отчета о соответствии. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Свойство | Описание |
| --- | --- |
| Время создания |Дата и время запуска проверки соответствия. |
| OperationName |DscResourceStatusData|
| ResultType |Подтверждено ли соответствие ресурса. |
| NodeName_s |Имя управляемого узла. |
| Категория | Дскнодестатус. |
| Ресурс | Имя учетной записи службы автоматизации Azure. |
| Tenant_g | GUID, идентифицирующий клиента для вызывающего объекта. |
| NodeId_g |Идентификатор GUID для определения управляемого узла. |
| DscReportId_g |Идентификатор GUID для определения отчета. |
| DscResourceId_s |Имя для экземпляра ресурса DSC. |
| DscResourceName_s |Имя ресурса DSC. |
| DscResourceStatus_s |Подтверждено ли соответствие ресурса DSC. |
| DscModuleName_s |Имя модуля PowerShell, который содержит ресурс DSC. |
| DscModuleVersion_s |Версия модуля PowerShell, который содержит ресурс DSC. |
| DscConfigurationName_s |Имя примененной к узлу конфигурации. |
| ErrorCode_s | Код ошибки при сбое ресурса. |
| ErrorMessage_s |Сообщение об ошибке при сбое ресурса. |
| DscResourceDuration_d |Период выполнения ресурса DSC в секундах. |
| SourceSystem | Как Azure Monitor журналы собирают данные. Всегда имеет значение *Azure* для системы диагностики Azure. |
| ResourceId |Указывает учетную запись службы автоматизации Azure. |
| ResultDescription | Описание этой операции. |
| SubscriptionId | Идентификатор подписки Azure (GUID) для учетной записи службы автоматизации. |
| ResourceGroup | Имя группы ресурсов для учетной записи службы автоматизации. |
| ResourceProvider | NNTP. Средствами. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |Идентификатор GUID, который является ИДЕНТИФИКАТОРом корреляции отчета о соответствии. |

## <a name="summary"></a>Сводка

Отправляя данные конфигурации состояния автоматизации в журналы Azure Monitor, вы сможете получить более подробные сведения о состоянии узлов конфигурации состояния автоматизации, выполнив следующие действия.

- настроить оповещения, уведомляющие вас о проблемах;
- с помощью пользовательских представлений и поисковых запросов визуализировать результаты модуля Runbook, состояние задания Runbook и другие связанные ключевые индикаторы или метрики.

Журналы Azure Monitor обеспечивают более высокую оперативную видимость данных конфигурации состояния службы автоматизации и позволяют быстрее решать инциденты.

## <a name="next-steps"></a>Следующие шаги

- Общие сведения см. в статье с [обзором "Настройка состояния службы автоматизации Azure"](automation-dsc-overview.md).
- Чтобы приступить к работе со службой "Настройка состояния службы автоматизации Azure", см. сведения в [этой статье](automation-dsc-getting-started.md).
- Сведения о компилировании конфигураций DSC, которые затем можно назначить целевым узлам, см. в статье [Компилирование конфигураций в Azure Automation DSC](automation-dsc-compile.md).
- Справочник по командлетам PowerShell для службы "Настройка состояния службы автоматизации Azure" см. в [этой статье](/powershell/module/azurerm.automation/#automation).
- Сведения о ценах см. на странице [с ценами на службу "Настройка состояния службы автоматизации Azure"](https://azure.microsoft.com/pricing/details/automation/).
- Пример использования службы "Настройка состояния службы автоматизации Azure" и Chocolatey в конвейере непрерывного развертывания см. в [этой статье](automation-dsc-cd-chocolatey.md).
- Дополнительные сведения о том, как создавать различные поисковые запросы и просматривать журналы конфигурации состояния автоматизации с помощью журналов Azure Monitor, см. [в разделе Поиск по журналам в Azure Monitor журналах](../log-analytics/log-analytics-log-searches.md) .
- Дополнительные сведения о Azure Monitor журналах и источниках сбора данных см. [в разделе Сбор данных службы хранилища Azure в журналах Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md) .
