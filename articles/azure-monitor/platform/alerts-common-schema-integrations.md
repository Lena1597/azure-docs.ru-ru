---
title: Как интегрировать общую схему предупреждений с Logic Apps
description: Узнайте, как создать приложение логики, использующее общую схему оповещений для обработки всех ваших оповещений.
ms.service: azure-monitor
ms.subservice: alerts
ms.topic: conceptual
author: ananthradhakrishnan
ms.author: robb
ms.date: 05/27/2019
ms.openlocfilehash: 50a6067d271ad824f17df1ece36c3dd919c7b55b
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555653"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Как интегрировать общую схему предупреждений с Logic Apps

В этой статье показано, как создать приложение логики, использующее общую схему оповещений для обработки всех ваших оповещений.

## <a name="overview"></a>Краткое описание

[Общая схема предупреждений](https://aka.ms/commonAlertSchemaDocs) предоставляет стандартизованную и расширяемую схему JSON для всех различных типов оповещений. Общая схема предупреждений наиболее полезна при программном использовании — через веб-перехватчики, модули Runbook и приложения логики. В этой статье мы продемонстрируем, как можно создать одно приложение логики для обработки всех ваших оповещений. Те же принципы могут применяться и к другим программным методам. Приложение логики, описываемое в этой статье, создает четко определенные переменные для [полей "важный"](alerts-common-schema-definitions.md#essentials), а также описывает способ обработки логики определенного [типа оповещений](alerts-common-schema-definitions.md#alert-context) .


## <a name="prerequisites"></a>Технические условия 

В этой статье предполагается, что читатель знаком с 
* Настройка правил оповещений ([Метрика](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [Журнал](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [Журнал действий](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Настройка [групп действий](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* Включение [общей схемы предупреждений](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) из групп действий

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Создание приложения логики, использующего общую схему предупреждений

1. Выполните действия, описанные [в статье Создание приложения логики](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app). 

1.  Выберите триггер **При получении HTTP-запроса**.

    ![Триггеры приложения логики](media/action-groups-logic-app/logic-app-triggers.png "Триггеры приложений логики")

1.  Выберите **Изменить**, чтобы изменить триггер HTTP-запроса.

    ![Триггеры HTTP-запросов](media/action-groups-logic-app/http-request-trigger-shape.png "Триггеры HTTP-запросов")


1.  Скопируйте и вставьте следующую схему:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. Выберите **+** **Новый шаг**, а затем щелкните **Добавить действие**.

    ![Добавление действия](media/action-groups-logic-app/add-action.png "Добавление действия")

1. На этом этапе вы можете добавить различные соединители (Microsoft Teams, временной резерв, Salesforce и т. д.) в соответствии с конкретными бизнес-требованиями. Вы можете использовать встроенные поля "все". 

    ![Ключевые поля](media/alerts-common-schema-integrations/logic-app-essential-fields.png "Ключевые поля")
    
    Кроме того, условную логику можно создать на основе типа оповещения, используя параметр "выражение".

    ![Выражение приложения логики](media/alerts-common-schema-integrations/logic-app-expressions.png "Выражение приложения логики")
    
     [Поле "мониторингсервице"](alerts-common-schema-definitions.md#alert-context) позволяет уникальным образом идентифицировать тип оповещения, на основе которого можно создать условную логику.

    
    Например, приведенный ниже фрагмент кода проверяет, является ли предупреждение оповещением журнала на основе Application Insights, и, если да, выводит результаты поиска. В противном случае выводится «НД».

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     Дополнительные сведения о [написании выражений приложения логики](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions).

    


## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные [сведения о группах действий](../../azure-monitor/platform/action-groups.md).
* Дополнительные [сведения о схеме общих предупреждений](https://aka.ms/commonAlertSchemaDocs).

