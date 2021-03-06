---
title: Запрос данных из среды предварительного просмотра с C# помощью-Azure Time Series Insights | Документация Майкрософт
description: Узнайте, как запрашивать данные из среды службы "аналитика временных рядов Azure" с помощью приложения, C#написанного на.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 76e3ac85a6725976ebd14dac1805079613c94ec6
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980993"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Запрос данных из среды предварительной версии службы "аналитика временных рядов Azure" с помощьюC#

В C# этом примере показано, как запрашивать данные из [предварительной версии API доступа к данным](https://docs.microsoft.com/rest/api/time-series-insights/preview) в средах предварительной версии службы "аналитика временных рядов Azure".

> [!TIP]
> Просмотрите образцы C# кода предварительной версии на [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample).

## <a name="summary"></a>Сводка

Приведенный ниже пример кода демонстрирует следующие возможности.

* Поддержка автоматического создания пакетов SDK в Azure Auto- [RESTful](https://github.com/Azure/AutoRest).
* Получение маркера доступа с помощью Azure Active Directory с помощью [Microsoft. IdentityModel. Clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).
* Как передать полученный маркер доступа в заголовке `Authorization` последующих запросов API доступа к данным. 
* В примере показан интерфейс консоли, демонстрирующий, как выполняются HTTP-запросы к:

    * [API сред предварительной версии](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [Получение API доступности сред](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) и [API схемы событий](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [Предварительный просмотр API запросов](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [Получение API событий](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents), получение API [серии](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries)и [Получение API серии статистических](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries) данных
    * [API модели временных рядов](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [API пакетной](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch) службы для [получения иерархий API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) и иерархий
        * [API пакета](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch) API и типов [Get](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get)
        * API-интерфейс [получения экземпляров API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) и [экземпляров](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch)
* Возможности расширенного [поиска](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features) и [целевого сервера](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) .

## <a name="prerequisites-and-setup"></a>Предварительные требования и установка

Перед компиляцией и запуском примера кода выполните следующие шаги.

1. [Подготавливает предварительную версию среды службы "аналитика временных рядов Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment) ".
1. Настройте среду службы "аналитика временных рядов Azure" для Azure Active Directory, как описано в статье [Проверка подлинности и авторизация](time-series-insights-authentication-and-authorization.md). 
1. Запустите [женератекоде. bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) , как указано в [readme.md](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) , чтобы создать предварительные версии зависимостей клиента Time Series Insights.
1. Откройте решение `TSIPreviewDataPlaneclient.sln` и задайте `DataPlaneClientSampleApp` в качестве проекта по умолчанию в Visual Studio.
1. Установите необходимые зависимости проекта, выполнив описанные [ниже](#project-dependencies) действия, и скомпилируйте пример в исполняемый файл `.exe`.
1. Запустите файл `.exe`, дважды щелкнув его.

## <a name="project-dependencies"></a>Зависимости проектов

Рекомендуется использовать последнюю версию Visual Studio:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) — версия 16.4.2 +

В примере кода имеется несколько обязательных зависимостей, которые можно просмотреть в файле [Packages. config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) .

Скачайте пакеты в Visual Studio 2019, выбрав параметр **build** > **Build Solution** . 

Кроме того, добавьте каждый пакет с помощью [NuGet 2.12 +](https://www.nuget.org/). Пример.

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>Пример кода C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Пример кода можно выполнить без изменения переменных среды по умолчанию.
> * Пример кода компилируется в исполняемое консольное приложение .NET.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о запросах см. в [справочнике по API запросов](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Узнайте, как [подключить приложение JavaScript с помощью клиентского пакета SDK к службе](https://github.com/microsoft/tsiclient) "аналитика временных рядов".
