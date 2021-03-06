---
title: Общие сведения об обогащении ИИ
titleSuffix: Azure Cognitive Search
description: Извлечение содержимого, обработка естественной речи и изображений для создания доступного для поиска содержимого при индексации в службе "Когнитивный поиск Azure" с применением когнитивных навыков и алгоритмов искусственного интеллекта.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 11/04/2019
ms.openlocfilehash: e6ee75f4a7e00e8c21079e1336756db20221750f
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76838009"
---
# <a name="introduction-to-ai-in-azure-cognitive-search"></a>Общие сведения об ИИ в Когнитивном поиске Azure

Обогащение с помощью ИИ — это возможность индексирования службы "Когнитивный поиск Azure", используемая для извлечения текста из изображений, больших двоичных объектов и других неструктурированных источников данных. Она помогает обогащать содержимое, чтобы сделать его более доступным для поиска в индексе или хранилище знаний. Извлечение и обогащение реализуются через *когнитивные навыки*,которые присоединены к конвейеру индексирования. В службу встроены когнитивные навыки приведенных ниже категорий. 

+ Навыки **обработки естественного языка** включают в себя [распознавание сущностей](cognitive-search-skill-entity-recognition.md), [распознавание языка](cognitive-search-skill-language-detection.md), [извлечение ключевых фраз](cognitive-search-skill-keyphrases.md), обработку текста, [определение тональности](cognitive-search-skill-sentiment.md), и [обнаружение персональных данных](cognitive-search-skill-pii-detection.md). С помощью этих навыков неструктурированный текст может принимать новые формы, сопоставляемые с полями поиска и фильтрации в индексе.

+ Навыки **обработки изображений** охватывают [распознавание текста (OCR)](cognitive-search-skill-ocr.md) и идентификацию [визуальных компонентов](cognitive-search-skill-image-analysis.md), таких как обнаружение лица, интерпретация изображений, распознавание изображений (известные люди и ориентиры) или атрибутов, таких как цвета или ориентация изображений. Вы можете создавать текстовые представления содержимого изображения, по которым можно выполнять поиск, используя все возможности запросов службы "Когнитивный поиск Azure".

![Схема конвейера обогащения](./media/cognitive-search-intro/cogsearch-architecture.png "обзор конвейера обогащения")

Когнитивные навыки в службе "Когнитивный поиск Azure" основываются на заранее подготовленных моделях машинного обучения в API-интерфейсах Cognitive Services: [API компьютерного зрения](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) и [анализ текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

На этапе приема данных применяется обработка естественного языка и изображений, при этом результаты становятся частью структуры документа в индексе поиска службы "Когнитивный поиск Azure". Данные получают в виде набора данных Azure, после чего они передаются по конвейеру индексирования с использованием необходимых [встроенных навыков](cognitive-search-predefined-skills.md). Архитектура является расширяемой, поэтому, если встроенных навыков недостаточно, вы можете создавать и прикреплять [пользовательские навыки](cognitive-search-create-custom-skill-example.md) для интеграции пользовательской обработки. Например, в модуле сущности или классификаторе документов можно выполнять задачи в специализированных областях: финансы, научные публикации или медицина.

> [!NOTE]
> По мере расширения области путем увеличения частоты обработки и добавления большего количества документов или дополнительных алгоритмов ИИ, вам нужно будет [присоединить оплачиваемый ресурс Cognitive Services](cognitive-search-attach-cognitive-services.md). Плата взимается при вызове API в Cognitive Services и извлечении изображений при распознавании документов в службе "Когнитивный поиск Azure". За извлечение текста из документов плата не взимается.
>
> Плата за выполнение встроенных навыков взимается в рамках существующей [модели оплаты Cognitive Services по мере использования](https://azure.microsoft.com/pricing/details/cognitive-services/). Плата за извлечение изображений указана на [странице с ценами на службу "Когнитивный поиск Azure"](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="when-to-use-cognitive-skills"></a>Сценарии использования когнитивных навыков

Рекомендуем использовать встроенные когнитивные навыки, если необработанное содержимое является неструктурированным текстом, содержимым изображения или содержимым, для которого требуется распознавание языка и перевод. Применение искусственного интеллекта с помощью встроенных когнитивных навыков может помочь распознать содержимое, что расширяет его ценность и возможности применения в приложениях поиска и обработки и анализа данных. 

Кроме того, вы можете добавить пользовательский навык, если у вас сторонний или собственный открытый исходный код, который нужно интегрировать в конвейер. В эту категорию попадают модели классификации, которые позволяют определить ключевые характеристики различных типов документов, но можно также использовать любой пакет, который придает содержимому значимость.

### <a name="more-about-built-in-skills"></a>Дополнительные сведения о встроенных навыках

Набор навыков, в котором объединены встроенные навыки, хорошо подходит для следующих сценариев использования.

+ Отсканированные документы (JPEG), которые нужно сделать доступными для полнотекстового поиска. Вы можете присоединить навык оптического распознавания символов (OCR) для идентификации, извлечения и приема текста из файлов JPEG.

+ Файлы PDF с изображением и текстом. Текст в файлах PDF можно извлечь во время индексирования, не выполняя шаги обогащения, но добавление обработки изображений и естественного языка часто может обеспечить лучший результат, чем стандартное индексирование.

+ Многоязычное содержимое, к которому необходимо применить распознавание языка и, возможно, перевод текста.

+ Неструктурированные или частично структурированные документы с содержимым, которое имеет присущее значение или контекст, скрытые в большом документе. 

  В частности, большие двоичные объекты часто содержат большой объем содержимого, которое помещено в единое "поле". Присоединив навыки обработки изображения и естественного языка к индексатору, можно сформировать сведения, существующие в необработанном содержимом, но не предоставленные в виде отдельных полей. Некоторые полезные, готовые к использованию встроенные когнитивные навыки включают в себя: извлечение ключевых фраз, анализ тональности и распознавание сущностей (людей, организаций и расположений).

  Кроме того, встроенные навыки также можно использовать для реструктуризации содержимого с помощью операций разделения текста, слияния и формирования фигур.

### <a name="more-about-custom-skills"></a>Дополнительные сведения о пользовательских навыках

Пользовательские навыки могут поддерживать более сложные сценарии, такие как распознавание форм или обнаружение настраиваемых сущностей с использованием модели, которую вы предоставляете и переносите в [веб-интерфейс пользовательских навыков](cognitive-search-custom-skill-interface.md). Несколько примеров пользовательских навыков включают в себя [Распознаватель документов](/azure/cognitive-services/form-recognizer/overview), интеграцию [API Поиска сущностей Bing](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example) и [распознавание настраиваемых сущностей](https://github.com/Microsoft/SkillsExtractorCognitiveSearch).


## <a name="components-of-an-enrichment-pipeline"></a>Компоненты конвейера обогащения

Конвейер обогащения основан на [*индексаторах*](search-indexer-overview.md), которые сканируют источники данных и создают по ним полные индексы. Теперь к этим индексаторам можно подключать навыки, которые отслеживают и обогащают определенные документы по настроенным вами наборам навыков. После индексации созданное содержимое будет доступно через поисковые запросы [любых типов, поддерживаемых в службе "Когнитивный поиск Azure"](search-query-overview.md).  Если вы еще не знакомы с индексаторами, ознакомьтесь с простым примером, представленным в этом разделе.

### <a name="step-1-connection-and-document-cracking-phase"></a>Шаг 1. Подключение и этап открытия документов

На вход конвейера подается неструктурированный текст или нетекстовое содержимое (например, JPEG-файлы с изображениями или отсканированными документами). Эти данные нужно разместить в службе хранилища данных Azure, доступной для индексатора. Индексаторы умеют открывать исходные документы и извлекать из них текстовые данные.

![Этап распознавания документов](./media/cognitive-search-intro/document-cracking-phase-blowup.png "распознавание документов")

 В качестве источников данных поддерживаются хранилища больших двоичных объектов Azure, хранилища таблиц Azure, база данных SQL Azure и Azure Cosmos DB. Текстовое содержимое можно извлечь из файлов следующих типов: PDF, Word, PowerPoint и CSV. Дополнительные сведения см. в разделе [Поддерживаемые форматы документов](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Шаг 2. Когнитивных навыки и этап обогащения

Обогащение выполняется *когнитивными навыками* с атомарными операциями. Например, выделив текстовое содержимое из PDF-файла, к нему можно применить распознавание языка, распознавание сущностей или извлечение ключевых фраз, чтобы создать в индексе новые поля данных, не существующие в самом источнике. Совокупность таких возможностей, реализованная в вашем конвейере, называется *набором навыков*.  

![Этап обогащения](./media/cognitive-search-intro/enrichment-phase-blowup.png "этап обогащения")

Набор навыков основывается на [встроенных когнитивных навыках](cognitive-search-predefined-skills.md) или [пользовательских навыках](cognitive-search-create-custom-skill-example.md), которые вы создаете и подключаете к нему. Можно использовать как минимальные, так и очень сложные наборы навыков. Они определяют не только типы обработки, но и порядок операций. Набор навыков в сочетании с сопоставлением полей, которые включены в индексатор, полностью определяют весь конвейер обогащения данных. Дополнительные сведения о том, как собрать все эти фрагменты в единое целое, вы найдете в статье [Создание набора навыков в конвейере обогащения](cognitive-search-defining-skillset.md).

В конвейере создается коллекция обогащенных документов. Вы можете выбрать, какие из элементов этих обогащенных документов нужно сопоставить с индексируемыми полями в индексе поиска. Например, если вы применили навыки извлечения ключевых фраз и распознавания сущностей, в обогащенных документах будут созданы соответствующие поля, которые можно будет сопоставить с полями индекса. В статье [Создание ссылок на аннотации в наборе навыков когнитивного поиска](cognitive-search-concept-annotations-syntax.md) вы найдете дополнительные сведения о структурах ввода и вывода.

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Добавление элемента knowledgeStore для сохранения обогащений

В [предварительной версии REST API api-version=2019-05-06 службы "Поиск"](search-api-preview.md) в наборы навыков добавляется определение `knowledgeStore`, которое обеспечивает подключение к хранилищу Azure, и проекции, описывающие способ сохранения обогащений. 

Добавив хранилище знаний к набору навыков, можно спроецировать представление обогащений для сценариев, в которых не предусмотрен полнотекстовой поиск. Подробные сведения см. в статье о [хранилище знаний (предварительная версия)](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>Шаг 3. Индекс поиска и доступа с помощью запросов

Когда обработка завершится, в службе "Когнитивный поиск Azure" будет создан индекс поиска, состоящий из обогащенных документов с возможностью текстового поиска по ним. [Запросы по индексу](search-query-overview.md) позволяют разработчикам и пользователям получить доступ к обогащенному содержимому, созданному конвейером индексирования. 

![Индекс со значком поиска](./media/cognitive-search-intro/search-phase-blowup.png "индекс со значком поиска")

Такой индекс действует так же, как любой другой индекс в службе "Когнитивный поиск Azure", и вы можете дополнить его пользовательскими анализаторами, применить поисковые запросы нечетких соответствий, добавить фильтрацию или настроить профили оценки, чтобы сформировать результаты поиска.

Индексы создаются на основе схемы индекса, в которой определены поля, атрибуты и другие конструкции для конкретного индекса, в том числе профили оценки и карты синонимов. После создания и первичного заполнения индекса можно выполнять индивидуальные операции индексирования для учета новых и обновленных исходных документов. Некоторые изменения данных потребуют полного перестроения индекса. Пока структура схемы не будет достаточно стабильной, ограничьтесь небольшим набором данных. Дополнительные сведения см. в статье [How to rebuild an Azure Search index](search-howto-reindex.md) (Как перестроить индекс для службы "Поиск Azure").

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Основные возможности и концепции

| Концепция | Описание| Ссылки |
|---------|------------|-------|
| Набор навыков | Именованный ресурс верхнего уровня, содержащий коллекцию навыков. Набор навыков определяет конвейер обогащения. Он применяется индексатором во время индексирования. | Ознакомьтесь со статьей [How to create a skillset in an enrichment pipeline](cognitive-search-defining-skillset.md) (Создание набора навыков в конвейере обогащения) |
| Когнитивный навык | Атомарное преобразование в конвейере обогащения. Часто этот компонент извлекает или предполагает структуру, расширяя доступное понимание входных данных. Результат преобразования почти всегда имеет текстовый вид, например при обработке естественной речи и обработке изображений извлекается текст из изображений или создается текст на их основе. Выходные данные навыка можно сопоставить с определенным полем индекса или передать в качестве входных данных для последующих операций обогащения. Вы можете использовать стандартные навыки, предоставленные корпорацией Майкрософт, или создать и развернуть собственные настраиваемые навыки. | [Встроенные когнитивные навыки](cognitive-search-predefined-skills.md) |
| Извлечение данных | Охватывает широкий спектр операций обработки, но при обогащении ИИ навык распознавания сущностей чаще всего используется для извлечения данных (сущности) из источника, который не предоставляет эту информацию естественным образом. | Ознакомьтесь со статьями о навыках [распознавания сущностей](cognitive-search-skill-entity-recognition.md) и [извлечения документов (предварительная версия)](cognitive-search-skill-document-extraction.md)| 
| Обработка изображений | Формирует текст на основе изображения, например распознает ориентиры или извлекает текст из изображения. Самые стандартные варианты применения — это распознавание текста из файла отсканированного документа (например, JPEG) или поиск названий улиц на фотографиях со знаками и вывесками. | Ознакомьтесь со статьей о [навыке анализа изображений](cognitive-search-skill-image-analysis.md) или [навыке распознавания текста](cognitive-search-skill-ocr.md)
| Обработка естественного языка | Обработка текста для получения аналитических сведений о текстовых входных данных. Определение языка, анализ тональности и извлечения ключевых фраз — вот основные примеры навыков, относящихся к обработке естественной речи.  | Ознакомьтесь со статьями о навыках [извлечения ключевых фраз](cognitive-search-skill-keyphrases.md), [распознавания языка](cognitive-search-skill-language-detection.md), [перевода текста](cognitive-search-skill-text-translation.md), [анализа тональности](cognitive-search-skill-sentiment.md), [ и распознавания персональных данных (предварительная версия)](cognitive-search-skill-pii-detection.md) |
| Открытие документов | Процесс извлечения или создание текстового содержимого из нетекстовых источников во время индексирования. В качестве примера можно упомянуть оптическое распознавание символов, но чаще этот термин относится к базовой функциональности индексатора, то есть к извлечению содержимого из файлов приложения. Для успешного открытия документов важно, чтобы источник данных сообщал расположение исходных файлов, а в определении индексатора были настроены сопоставления для полей. | Ознакомьтесь со статьей с [общими сведениями об индексаторах](search-indexer-overview.md) |
| Формирование | Объединение фрагментов текста в более крупные структуры или разделение фрагментов текста на меньшие части для дальнейшей обработки. | Ознакомьтесь со статьями о [навыке формирования](cognitive-search-skill-shaper.md), [слияния текста](cognitive-search-skill-textmerger.md), [разделения текста](cognitive-search-skill-textsplit.md) |
| Обогащенные документы | Переходная внутренняя структура, создаваемая во время обработки, конечный результат которой отражается в индексе поиска. Набор навыков определяет, какие улучшения выполняются. Сопоставления полей определяют, какие элементы данных добавляются в индекс. При необходимости можно создать хранилище знаний для сохранения и изучения улучшенных документов с помощью таких инструментов, как Обозреватель службы хранилища, Power BI или любой другой инструмент, который подключается к хранилищу BLOB-объектов Azure. | Ознакомьтесь со статьей [Что собой представляет хранилище знаний в службе "Поиск Azure"](knowledge-store-concept-intro.md). |
| Индексатор |  Программа-обходчик, которая извлекает доступные для поиска данные и метаданные из внешнего источника данных, а затем заполняет индекс, сопоставляя поля в индексе и источнике данных для открытия документа. Чтобы применить обогащения ИИ, индексатор вызывает набор навыков и передает соответствующие обогащения в определенные целевые поля индекса. Определение индексатора содержит все инструкции и ссылки для операций конвейера, который вызывается при запуске индексатора. После дополнительной настройки вы сможете повторно использовать обработанное содержимое и работать только с теми этапами и навыками, которые изменились. | Ознакомьтесь со статьями об [индексаторах](search-indexer-overview.md) и [добавочном обогащении (предварительная версия)](cognitive-search-incremental-indexing-conceptual.md). |
| Источник данных  | Объект, используемый для подключения индексатора к внешнему источнику данных из числа поддерживаемых в Azure типов. | Ознакомьтесь со статьей с [общими сведениями об индексаторах](search-indexer-overview.md) |
| Индекс | Сохраняемый индекс поиска в службе "Когнитивный поиск Azure", составленный в соответствии со схемой индекса, в которой определяются структура и способы использования полей. | Ознакомьтесь со статьей о [создании простого индекса](search-what-is-an-index.md) | 
| Хранилище знаний | Учетная запись хранения, в которой помимо поискового индекса можно формировать и проецировать обогащенные документы. | См [Introduction to knowledge store](knowledge-store-concept-intro.md) (Общие сведения о хранилище знаний) | 
| Кэш | Учетная запись хранения, которая содержит кэшированные выходные данные, созданные в результате выполнения конвейера обогащения. Включение кэша позволяет сохранить существующие выходные данные, не затронутые изменениями, в набор навыков или другие компоненты конвейера обогащения. | Подробнее о [добавочном обогащении данных](cognitive-search-incremental-indexing-conceptual.md). | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>С чего начать?

**Шаг 1. [Краткое руководство. Создание службы "Поиск Azure" на портале](search-create-service-portal.md)** 

**Шаг 2. Выполнение кратких руководств и изучение примеров**

+ [Краткое руководство (портал)](cognitive-search-quickstart-blob.md)
+ [Руководство (HTTP-запросы)](cognitive-search-tutorial-blob.md)
+ [Пример. Создание пользовательского навыка для обогащения ИИ (C#)](cognitive-search-create-custom-skill-example.md)

Мы рекомендуем пользоваться бесплатной версией службы в учебных целях. Но учтите, что число бесплатных транзакций ограничено 20 документами в день. Чтобы выполнить краткое руководство и учебник за один день, используйте небольшой набор файлов (из 10 документов), что позволит выполнить оба упражнения, или удалите индексатор, который использовался в кратком руководстве или учебнике, чтобы обнулить счетчик.

**Шаг 3. Изучите API.**

Вы можете использовать REST API версии `api-version=2019-05-06` с запросами или с пакетом SDK для .NET. Если вы изучаете хранилище знаний, используйте предварительную версию REST API (`api-version=2019-05-06-Preview`).

На этом шаге для создания решения обогащения ИИ используются интерфейсы REST API. Только два API-интерфейса можно добавить или расширить для обогащения ИИ. Все остальные API сохраняют тот же синтаксис, что и в общедоступной версии.

| REST API | Описание |
|-----|-------------|
| [Создание источника данных](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Ресурс, определяющий внешний источник данных с исходными данными, которые будут применены для создания обогащенных документов.  |
| [Создание набора навыков (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Этот API предназначен только для обогащения ИИ. Ресурс, в котором описано использование [встроенных навыков](cognitive-search-predefined-skills.md) и [пользовательских когнитивных навыков](cognitive-search-custom-skill-interface.md) в конвейере обогащения данных во время индексирования. |
| [Создание индекса](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Схема, описывающая индекс службы "Когнитивный поиск Azure". Поля в этом индексе сопоставляются с полями в источнике данных или с полями, созданным на этапе обогащения (например, поле с названиями организаций, созданное навыком распознавания сущностей). |
| [Создание индексатора (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Ресурс, определяющий компоненты для процесса индексирования, в том числе источник данных, набор навыков, сопоставлений полей источника и промежуточных структур с полями целевого индекса, определение индекса. При запуске индексатора активируется процесс приема и обогащения данных. На выходе формируется индекс поиска, который основан на схеме индекса и заполнен исходными данными, обогащенными с помощью наборов навыков. Этот существующий API расширен для сценариев Когнитивного поиска с добавлением свойства набора навыков. |

**Контрольный список. стандартный рабочий процесс**

1. Выделите репрезентативную выборку из набора исходных данных в Azure. Индексирование занимает немало времени, поэтому лучше начать с небольшого репрезентативного набора данных, который вы будете поэтапно развивать параллельно с разработкой решения.

1. Создайте [объект источника данных](https://docs.microsoft.com/rest/api/searchservice/create-data-source) в службе "Когнитивный поиск Azure", чтобы указать строку подключения для получения данных.

1. Создайте [набор навыков](https://docs.microsoft.com/rest/api/searchservice/create-skillset) с действиями для обогащения.

1. Определите [схему индекса](https://docs.microsoft.com/rest/api/searchservice/create-index). Коллекция *Поля* содержит поля источника данных. Также здесь нужно предусмотреть заглушки для дополнительных полей, в которых будет сохраняться содержимое, созданное на этапе обогащения.

1. Определите [индексатор](https://docs.microsoft.com/rest/api/searchservice/create-skillset) со ссылками на источник данных, набор навыков и индекс.

1. Добавьте в этот индексатор *outputFieldMappings*. Этот раздел сопоставляет выходные данные, полученные от набора навыков (шаг 3), с полями входных данных в схеме индекса (шаг 4).

1. Отправьте только что созданный запрос на *создание индексатора* (запрос POST с определением индексатора в тексте запроса), чтобы определить индексатор в службе "Когнитивный поиск Azure". Именно этот шаг запускает ваш индексатор и начинает работу конвейера.

1. Выполните тестовые запросы для оценки результатов, и при необходимости измените код, обновив набор навыков, схемы или конфигурацию индексатора.

1. Выполните [сброс индексатора](search-howto-reindex.md), прежде чем перестроить конвейер.

Дополнительные сведения о некоторых вопросах и проблемах см. в статье [Советы по устранению неполадок с когнитивным поиском](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Дальнейшие действия

+ [Ссылки на документацию по обогащению ИИ](cognitive-search-resources-documentation.md)
+ [Краткое руководство. по обогащению ИИ с помощью пошагового руководства по порталу](cognitive-search-quickstart-blob.md)
+ [Руководство. Дополнительные сведения об API-интерфейсах обогащения ИИ](cognitive-search-tutorial-blob.md)
+ [Что собой представляет хранилище знаний в службе "Поиск Azure"](knowledge-store-concept-intro.md)
+ [Создание хранилища знаний в REST](knowledge-store-create-rest.md)
