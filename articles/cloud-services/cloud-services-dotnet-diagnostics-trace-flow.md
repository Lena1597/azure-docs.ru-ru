---
title: Трассировка потока в приложении облачных служб с помощью система диагностики Azure
titleSuffix: Azure Cloud Services
description: Добавление сообщений трассировки в приложения Azure для отладки, измерения производительности, мониторинга, анализа трафика и выполнения других задач.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: tagore
ms.openlocfilehash: 47a33ba27dd6d2df626d93695c421303bace6a0b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75386516"
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Трассировка потока в приложении облачных служб с помощью системы диагностики Azure
Трассировка — это мониторинг выполнения запущенного приложения. Сведения об ошибках и выполнении приложений можно записывать в журналы, текстовые файлы или на устройства для последующего анализа с помощью классов [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace), [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) и [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource). Дополнительные сведения о трассировке см. в статье [Tracing and Instrumenting Applications](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications) (Трассировка и инструментирование приложений).

## <a name="use-trace-statements-and-trace-switches"></a>Использование трассировочных операторов и переключателей
Чтобы реализовать трассировку в приложении облачных служб, добавьте класс [DiagnosticMonitorTraceListener](/previous-versions/azure/reference/ee758610(v=azure.100)) в конфигурацию приложения и вызовите метод System.Diagnostics.Trace или System.Diagnostics.Debug в коде приложения. Используйте файл конфигурации *app.config* для рабочих ролей и *web.config* для веб-ролей. Во время создания размещенной службы с помощью шаблона Visual Studio система диагностики Azure автоматически добавляется в проект. При этом класс DiagnosticMonitorTraceListener включается в соответствующий файл конфигурации для добавляемых ролей.

Дополнительные сведения о размещении трассировочных операторов см. в статье [How to: Add Trace Statements to Application Code](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code) (Добавление трассировочных операторов в код приложения).

Процессом и масштабом трассировки можно управлять. Для этого добавьте в код [переключатели трассировки](/dotnet/framework/debug-trace-profile/trace-switches). Так вы сможете отслеживать состояние приложения в рабочей среде. Это особенно важно в бизнес-приложениях, которые используют различные компоненты, выполняющиеся на нескольких компьютерах. Дополнительные сведения см. в статье [How to: Configure Trace Switches](/dotnet/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches) (Настройка переключателей трассировки).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Настройка прослушивателя трассировки в приложении Azure
Использование элементов Trace, Debug и TraceSource предполагает настройку прослушивателей для сбора и записи отправляемых сообщений. Прослушиватели собирают, хранят и перенаправляют сообщения трассировки. Они направляют выходные данные трассировки в соответствующий целевой объект, например журнал, окно или текстовый файл. Система диагностики Azure использует класс [DiagnosticMonitorTraceListener](/previous-versions/azure/reference/ee758610(v=azure.100)) .

Перед выполнением следующей процедуры необходимо инициализировать монитор диагностики Azure. Сведения о том, как это сделать, см. в статье [Включение системы диагностики Azure в облачных службах Azure](cloud-services-dotnet-diagnostics.md).

Обратите внимание, что при использовании шаблонов Visual Studio конфигурация прослушивателя добавляется автоматически.

### <a name="add-a-trace-listener"></a>Добавление прослушивателя трассировки
1. Откройте файл web.config или app.config для своей роли.
2. Добавьте в файл следующий код. Измените атрибут Version, чтобы использовать номер версии сборки, которая указана в ссылках. Версия сборки не обязательно меняется с каждым выпуском пакета SDK для Azure, а только если она обновляется.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Убедитесь, что у вас есть ссылка проекта на сборку Microsoft.WindowsAzure.Diagnostics. Обновите номер версии в приведенном выше коде XML в соответствии с версией сборки Microsoft.WindowsAzure.Diagnostics, используемой в ссылке.
   > 
   > 
3. Сохраните файл конфигурации.

Дополнительные сведения о прослушивателях см. [здесь](/dotnet/framework/debug-trace-profile/trace-listeners).

Добавив прослушиватель, вы можете добавить в код трассировочные операторы.

### <a name="to-add-trace-statement-to-your-code"></a>Добавление трассировочного оператора в код
1. Откройте исходный файл приложения. Например, файл \<RoleName >. cs для рабочей роли или веб-роли.
2. Добавьте следующую директиву using, если она еще не добавлена:
    ```
        using System.Diagnostics;
    ```
3. Добавьте трассировочные операторы, чтобы записывать сведения о состоянии приложения. Вывод оператора трассировки можно форматировать разными способами. Дополнительные сведения об этом см. в статье [How to: Add Trace Statements to Application Code](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code) (Добавление трассировочных операторов в код приложения).
4. Сохраните исходный файл.




