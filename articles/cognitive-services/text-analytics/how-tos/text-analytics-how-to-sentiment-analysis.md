---
title: Анализ тональности с помощью REST API "Анализ текста"
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как определить тональность в тексте с помощью REST API "Анализ текста" Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: 214c071e0d01908e2d46c932fcf87906de834102
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644687"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Руководство. Определение тональность с помощью API Анализа текста

Функция анализа тональности в API Анализа текста оценивает полученный текст и возвращает оценки тональности оценки и метки для каждого предложения. Это полезно для обнаружения положительных и отрицательных тональностей в социальных сетях, отзывах клиентов, на форумах и т. п. Модели ИИ для этого API предоставляются службой, вам нужно лишь отправить содержимое для анализа.

> [!TIP]
> API анализа текста также предоставляет образ контейнера Docker под управлением Linux для распознавания языка, поэтому вы можете [установить и запустить контейнер API анализа текста](text-analytics-how-to-install-containers.md) в непосредственной близости к своим данным.

Анализ тональности поддерживает широкий спектр языков, и еще несколько сейчас находятся в режиме предварительной версии. Дополнительные сведения см. в [списке поддерживаемых языков](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Основные понятия

API Анализа текста использует алгоритм классификации машинного обучения для создания оценки тональности в диапазоне от 0 до 1. Оценки, близкие к 1, указывают на позитивную тональность, а оценки, близкие к 0, — на негативную. Анализ тональности выполняется по всему документу, а не для отдельных сущностей в тексте. Это означает, что оценки тональности возвращаются на уровне документа или предложения. 

Используемая модель заранее обучена по широкому набору текстов со связями тональности. Она использует для анализа текста несколько разных методик, включая обработку текста, анализ частей речи, размещения слов и связей между словами. Дополнительные сведения об алгоритме см. в статье [Introducing Text Analytics in the Azure ML Marketplace](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/) (Представление Анализа текста в Azure ML Marketplace). В настоящее время нет возможности предоставления собственных данных для обучения. 

Имеется тенденция к более высокой точности оценки, если документы содержат несколько предложений, а не большой блок текста. На этапе оценки объективности модель определяет, является ли документ в целом объективным или содержит тональности. Документ, который в основном объективен, не продвигается к фразе обнаружения тональности, что приводит к оценке 0.50, без дальнейшей обработки. Для документов, которые остаются в конвейере, следующая фаза генерирует оценку выше или ниже 0.50. Оценка зависит от степени тональности, обнаруженной в документе.

## <a name="sentiment-analysis-versions-and-features"></a>Версии и компоненты анализа тональности

API Анализа текста предоставляет две версии анализа тональности — версия 2 и версия 3. Анализ тональности версии 3 (общедоступная предварительная версия) значительно улучшает точность и детализацию классификации и оценки текста в API.

> [!NOTE]
> * Формат запроса и [ограничения данных](../overview.md#data-limits) для анализа тональности версии 3 по сравнению с предыдущей версией не изменились.
> * Анализ тональности версии 3 доступен в таких регионах: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` и `West US 2`.

| Компонент                                   | Анализ тональности версии 2 | Анализ тональности версии 3 |
|-------------------------------------------|-----------------------|-----------------------|
| Методы для одиночных и пакетных запросов    | X                     | X                     |
| Оценки тональности для всего документа  | X                     | X                     |
| Оценки тональности для отдельных предложений |                       | X                     |
| Метки тональности                        |                       | X                     |
| управления версиями моделей;                   |                       | X                     |

#### <a name="version-2tabversion-2"></a>[Версия 2](#tab/version-2)

### <a name="sentiment-scoring"></a>Оценка тональности

Анализатор тональности позволяет классифицировать текст как преимущественно положительный или отрицательный. Он присваивает оценку в диапазоне от 0 до 1. Значения, близкие к 0,5 — нейтральные или неопределенные. Оценка 0,5 указывает нейтральность. Если строку невозможно проанализировать на определение тональности или у нее нет тональности, показатель всегда является именно 0,5. Например, если передать строку на испанском языке с кодом английского языка, оценка будет 0,5.


#### <a name="version-3-public-previewtabversion-3"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

### <a name="sentiment-scoring"></a>Оценка тональности

Анализ тональности v3 классифицирует текст с присвоением меток тональности (как описано ниже). Возвращаемые оценки обозначают уверенность модели в том, что текст имеет положительное, отрицательное или нейтральное значение. Более высокие значения соответствуют большей уверенности. 

### <a name="sentiment-labeling"></a>Метки тональности

Анализ тональности версии 3 может возвращать оценки и метки на уровне предложения и документа. Оценки и метки: `positive`, `negative` и `neutral`. На уровне документа может также возвращаться метка тональности `mixed` без значения оценки. Тональность документа определяется следующим образом.

| Тональность предложения                                                                            | Полученная метка документа |
|-----------------------------------------------------------------------------------------------|-------------------------|
| В документе есть по меньшей мере одно предложение `positive`. Остальные предложения оценены как `neutral`. | `positive`              |
| В документе есть по меньшей мере одно предложение `negative`. Остальные предложения оценены как `neutral`. | `negative`              |
| В документе есть по меньшей мере одно предложение `negative` и одно предложение `positive`.    | `mixed`                 |
| Все предложения в документе оценены как `neutral`.                                                  | `neutral`               |

### <a name="model-versioning"></a>управления версиями моделей;

> [!NOTE]
> Управление версиями модели для анализа тональности доступно начиная с версии `v3.0-preview.1`.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="example-c-code"></a>Пример кода C#:

Пример приложения на C#, которое вызывает эту версию анализа тональности, можно найти на сайте [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

---

## <a name="sending-a-rest-api-request"></a>Отправка запроса к REST API 

### <a name="preparation"></a>Подготовка

Анализ тональности дает более качественный результат, если ему передаются более мелкие блоки текста. Это является противоположностью извлечению ключевой фразы, которая выполняется лучше на более крупных блоках текста. Для получения наилучших результатов обеих операций советуем реструктуризировать входные данные соответствующим образом.

Необходимы документы JSON в следующем формате: ID, текст и язык.

Размер документа должен быть менее 5120 символов. На каждую коллекцию может приходиться до 1000 элементов (ИД). Коллекция передается в тексте запроса.

## <a name="structure-the-request"></a>Структурирование запроса

Создайте запрос POST. Вы можете использовать [средство Postman](text-analytics-how-to-call-api.md) или **консоль тестирования API**, доступные по следующим ссылкам, чтобы быстро оформить и отправить запрос. 

#### <a name="version-2tabversion-2"></a>[Версия 2](#tab/version-2)

[Справочник по анализу тональности версии 2](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

#### <a name="version-3-public-previewtabversion-3"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

[Справочник по анализу тональности версии 3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)

---

Задайте конечную точку HTTPS для анализа тональности с помощью ресурса API анализа текста в Azure или созданного при [установке и запуске контейнеров анализа текста](text-analytics-how-to-install-containers.md) экземпляра. Обязательно укажите правильный URL-адрес конечной точки для выбранной версии. Пример:
    
[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

#### <a name="version-2tabversion-2"></a>[Версия 2](#tab/version-2)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`

#### <a name="version-3-public-previewtabversion-3"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/sentiment`

---

Включите ключ API Анализа текста в заголовок запроса. В тексте запроса укажите набор документов JSON, которые подготовлены для этого анализа.

### <a name="example-sentiment-analysis-request"></a>Пример запроса для анализа тональности 

Ниже приведен пример содержимого, который вы можете представить для анализа тональности. Формат запроса одинаков для обеих версий API.
    
```json
{
    "documents": [
    {
        "language": "en",
        "id": "1",
        "text": "Hello world. This is some input text that I love."
    },
    {
        "language": "en",
        "id": "2",
        "text": "It's incredibly sunny outside! I'm so happy."
    }
    ],
}
```

### <a name="post-the-request"></a>Передача запроса

Анализ выполняется при получении запроса. Сведения о размере и числе запросов, которые можно отправлять в минуту и секунду, см. в разделе об [ограничениях данных](../overview.md#data-limits).

API Анализа текста не учитывает состояние. Никакие данные в учетной записи не сохраняются, а все результаты немедленно возвращаются в ответе.


### <a name="view-the-results"></a>Просмотр результатов

Анализатор тональности позволяет классифицировать текст как преимущественно положительный или отрицательный. Он присваивает оценку в диапазоне от 0 до 1. Значения, близкие к 0,5 — нейтральные или неопределенные. Оценка 0,5 указывает нейтральность. Если строку невозможно проанализировать на определение тональности или у нее нет тональности, показатель всегда является именно 0,5. Например, если передать строку на испанском языке с кодом английского языка, оценка будет 0,5.

Вывод возвращается немедленно. Результаты можно передать в приложение, которое принимает JSON, или сохранить результаты в файл в локальной системе. Затем импортируйте выходные данные в приложение, которое можно использовать для сортировки, поиска и управления данными.

#### <a name="version-2tabversion-2"></a>[Версия 2](#tab/version-2)

### <a name="sentiment-analysis-v2-example-response"></a>Пример ответа от API анализа тональности версии 2

Ответы от API анализа тональности версии 2 содержат оценки тональности для каждого отправленного документа.

```json
{
  "documents": [{
    "id": "1",
    "score": 0.98690706491470337
  }, {
    "id": "2",
    "score": 0.95202046632766724
  }],
  "errors": []
}
```

#### <a name="version-3-public-previewtabversion-3"></a>[Версия 3 (общедоступная предварительная версия)](#tab/version-3)

### <a name="sentiment-analysis-v3-example-response"></a>Пример ответа от API анализа тональности версии 3

Ответы от API анализа тональности версии 3 содержат метки и оценки тональности для каждого проанализированного предложения и документа. `documentScores` не возвращается, если метка тональности для документа имеет значение `mixed`.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```
---

## <a name="summary"></a>Сводка

В этой статье вы изучили основные понятия и рабочий процесс анализа тональности с помощью API Анализа текста. В разделе "Сводка" сделайте следующее.

+ Анализ тональности доступен для некоторых языков в двух версиях.
+ В тексте запроса передаются документы JSON, которые содержат идентификатор, текст и код языка.
+ Запрос POST передается в конечную точку `/sentiment` используя личный [ключ доступа и конечную точку](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource), допустимые для вашей подписки.
+ Выходные данные ответа, состоящие из оценки тональности для каждого идентификатора документа, могут передаваться в любое приложение, принимающее JSON. Например, в Excel или Power BI.

## <a name="see-also"></a>См. также раздел

* [Text Analytics overview](../overview.md) (Общие сведения об анализе текста)
* [Использование клиентской библиотеки Анализа текста](../quickstarts/text-analytics-sdk.md)
* [Новые возможности](../whats-new.md)
