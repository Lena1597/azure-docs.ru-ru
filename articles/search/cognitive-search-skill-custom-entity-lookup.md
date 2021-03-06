---
title: Навык поиска нестандартных сущностей
titleSuffix: Azure Cognitive Search
description: Извлечение различных пользовательских сущностей из текста в конвейере поиска с помощью Когнитивный поиск. Этот навык в настоящее время доступен в общедоступной предварительной версии.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 5c820b7e11c06f2d785da036f5174298caf56da6
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960611"
---
#    <a name="custom-entity-lookup-cognitive-skill-preview"></a>Пользовательский навык уточняющего запроса сущности (Предварительная версия)

> [!IMPORTANT] 
> Этот навык в настоящее время доступен в общедоступной предварительной версии. Для предварительной версии функции соглашение об уровне обслуживания не предусмотрено. Мы не рекомендуем использовать ее в рабочей среде. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). В настоящее время нет поддержки портала или пакета SDK для .NET.

Навык **поиска настраиваемой сущности** ищет текст из пользовательского списка слов и фраз, определяемых пользователем. Используя этот список, он помечает все документы с любыми совпадающими сущностями. Навык также поддерживает степень нечеткого соответствия, которую можно применить для поиска совпадений, которые похожи, но не совсем точны.  

Этот навык не привязан к Cognitive Services API и может использоваться бесплатно в период действия предварительной версии. Однако вы все равно должны [присоединить ресурс Cognitive Services](https://docs.microsoft.com/azure/search/cognitive-search-attach-cognitive-services), чтобы переопределить дневное ограничение на обогащение. Ограничение ежедневно применяется для бесплатного доступа к Cognitive Services при доступе через Когнитивный поиск Azure.

## <a name="odatatype"></a>@odata.type  
Microsoft. Skills. Text. Кустоментитилукупскилл 

## <a name="data-limits"></a>Ограничения данных
+ Максимальный поддерживаемый размер входной записи составляет 256 МБ. Если необходимо разбить данные перед отправкой в пользовательский навык поиска сущности, рассмотрите возможность использования уровня " [разделение текста](cognitive-search-skill-textsplit.md)".
+ Поддерживаемая Таблица определения максимального числа сущностей — 10 МБ, если она указана с помощью параметра *ентитиесдефитионури* . 
+ Если сущности определены встроенным путем использования параметра *инлининтитиесдефинитион* , максимальный поддерживаемый размер РАВЕН 10 КБ.

## <a name="skill-parameters"></a>Параметры навыков

Параметры зависят от регистра.

| Имя параметра     | Description |
|--------------------|-------------|
| ентитиесдефинитионури | Путь к файлу JSON или CSV, содержащему целевой текст для сопоставления. Это определение сущности считывается в начале выполнения индексатора; все обновления этого файла не будут реализованы до последующего выполнения. Эта конфигурация должна быть доступна по протоколу HTTPS. Ожидаемую схему CSV или JSON см. в разделе " [Пользовательский формат определения сущности](#custom-entity-definition-format) " ниже.|
|инлининтитиесдефинитион | Встроенные определения сущностей JSON. Этот параметр заменяет параметр Ентитиесдефинитионури, если он есть. Встроенное может быть указано не более 10 КБ конфигурации. Ожидаемую схему JSON см. в разделе [Определение настраиваемой сущности](#custom-entity-definition-format) ниже. |
|defaultLanguageCode |  Используемых Код языка входного текста, используемого для маркировки и отделения входного текста. Поддерживаются следующие языки: `da, de, en, es, fi, fr, it, ko, pt`. Значение по умолчанию — английский (`en`). При передаче формата languagecode-countrycode используется только часть languagecode формата.  |


## <a name="skill-inputs"></a>Входные данные навыков

| Ввод имени      | Description                   |
|---------------|-------------------------------|
| text          | Текст для анализа.          |
| languageCode  | Необязательный параметр. Значение по умолчанию — `"en"`.  |


## <a name="skill-outputs"></a>Выходные данные навыка


| Имя вывода     | Description                   |
|---------------|-------------------------------|
| Сущности | Массив объектов, содержащих сведения о найденных совпадениях и связанных метаданных. Все идентифицированные сущности могут содержать следующие поля:  <ul> <li> *имя*. обнаружена сущность верхнего уровня. Сущность представляет "нормализованную" форму. </li> <li> *ID*: уникальный идентификатор сущности, определенный пользователем в "пользовательском формате определения сущности".</li> <li> *Описание*: Описание сущности, определенное пользователем в "пользовательском формате определения сущности". </li> <li> *Тип:* Тип сущности, определенный пользователем в "пользовательском формате определения сущности".</li> <li> *подтип:* Подтип сущности в соответствии с определением пользователя в "пользовательском формате определения сущности".</li>  <li> *Matches*: коллекция, которая описывает каждое из совпадений для данной сущности в исходном тексте. Каждое совпадение будет иметь следующие члены: </li> <ul> <li> *Text*: необработанный текст, совпадающий с исходным документом. </li> <li> *offset*: расположение, в котором обнаружено совпадение в тексте. </li> <li> *Длина*: длина сопоставленного текста. </li> <li> *матчдистанце*: количество символов, отличающихся этим совпадением, от исходного имени сущности или псевдонима.  </li> </ul> </ul>
  |

## <a name="custom-entity-definition-format"></a>Пользовательский формат определения сущности

Существует три разных способа предоставить список пользовательских сущностей для пользовательского навыка поиска сущности. Список можно указать в. CSV-файл,. JSON или как встроенное определение как часть определения навыка.  

Если файл определения имеет значение. CSV или. JSON, путь к файлу должен быть указан как часть параметра *ентитиесдефитионури* . В этом случае файл загружается один раз в начале каждого запуска индексатора. Файл должен быть доступен, если предполагается выполнение индексатора.

Если определение предоставляется встроенным, оно должно быть предоставлено как встроенное в качестве содержимого параметра *инлининтитиесдефинитион* навыка. 

### <a name="csv-format"></a>Формат CSV

Вы можете предоставить определение настраиваемых сущностей для поиска в файле с разделителями-запятыми (CSV), указав путь к файлу и установив его в параметре навыка *ентитиесдефитионури* . Путь должен находиться в расположении HTTPS. Размер файла определения может доставлять до 10 МБ.

Формат CSV является простым. Каждая строка представляет собой уникальную сущность, как показано ниже:

```
Bill Gates, BillG, William H. Gates
Microsoft, MSFT
Satya Nadella 
```

В этом случае есть три сущности, которые могут быть возвращены как найденные сущности (Билл Гейтс, Сатья Наделла, Microsoft), но они будут идентифицированы, если в тексте сопоставлены какие-либо термины в строке (псевдонимы). Например, если в документе найдена строка «Уильям H. Gates», будет возвращено соответствие для сущности «Билл Гейтс».

### <a name="json-format"></a>Формат JSON

Вы также можете предоставить определение настраиваемых сущностей для поиска в JSON-файле. Формат JSON обеспечивает более высокую гибкость, так как позволяет определять правила сопоставления для каждого термина. Например, можно указать расстояние нечеткого сопоставления (Дамерау-Левенштейна distance) для каждого термина, а также определить, должно ли сопоставление учитываться с учетом регистра. 

 Как и в случае с CSV-файлами, необходимо указать путь к JSON-файлу и задать его в параметре навык *ентитиесдефитионури* . Путь должен находиться в расположении HTTPS. Размер файла определения может доставлять до 10 МБ.

Базовое определение списка пользовательских сущностей JSON может быть списком сущностей для сопоставления:

```json
[ 
    { 
        "name" : "Bill Gates"
    }, 
    { 
        "name" : "Microsoft"
    }, 
    { 
        "name" : "Satya Nadella"
    }
]
```

Более сложный пример определения JSON может дополнительно предоставить идентификатор, описание, тип и подтип каждой сущности, а также другие *псевдонимы*. При сопоставлении термина псевдонима также будет возвращена сущность:

```json
[ 
    { 
        "name" : "Bill Gates",
        "description" : "Microsoft founder." ,
        "aliases" : [ 
            { "text" : "William H. Gates", "caseSensitive" : false },
            { "text" : "BillG", "caseSensitive" : true }
        ]
    }, 
    { 
        "name" : "Xbox One", 
        "type": "Harware",
        "subtype" : "Gaming Device",
        "id" : "4e36bf9d-5550-4396-8647-8e43d7564a76",
        "description" : "The Xbox One product"
    }, 
    { 
        "name" : "LinkedIn" , 
        "description" : "The LinkedIn company", 
        "id" : "differentIdentifyingScheme123", 
        "fuzzyEditDistance" : 0 
    }, 
    { 
        "name" : "Microsoft" , 
        "description" : "Microsoft Corporation", 
        "id" : "differentIdentifyingScheme987", 
        "defaultCaseSensitive" : false, 
        "defaultFuzzyEditDistance" : 1, 
        "aliases" : [ 
            { "text" : "MSFT", "caseSensitive" : true }
        ]
    } 
] 
```

В приведенных ниже таблицах подробно описаны различные параметры конфигурации, которые можно задать при определении сущностей для сопоставления.

|  Имя поля  |        Description  |
|--------------|----------------------|
| name | Дескриптор сущности верхнего уровня. Совпадения в выходных данных навыков будут сгруппированы по этому имени, и оно должно представлять "нормализованную" форму текста.  |
| description  | Используемых Это поле можно использовать в качестве транзитного для пользовательских метаданных о сопоставленном тексте (s). Значение этого поля будет отображаться с каждым совпадением сущности в выходных данных навыка. |
| type | Используемых Это поле можно использовать в качестве транзитного для пользовательских метаданных о сопоставленном тексте (s). Значение этого поля будет отображаться с каждым совпадением сущности в выходных данных навыка. |
| subtype | Используемых Это поле можно использовать в качестве транзитного для пользовательских метаданных о сопоставленном тексте (s). Значение этого поля будет отображаться с каждым совпадением сущности в выходных данных навыка. |
| идентификатор | Используемых Это поле можно использовать в качестве транзитного для пользовательских метаданных о сопоставленном тексте (s). Значение этого поля будет отображаться с каждым совпадением сущности в выходных данных навыка. |
| caseSensitive | Используемых Значение по умолчанию — false. Логическое значение, указывающее, следует ли учитывать регистр символов при сравнении с именем сущности. Примеры нечувствительных к регистру совпадений для Microsoft: Microsoft, microSoft, MICROSOFT |
| фуззедитдистанце | Используемых Значение по умолчанию — 0. Максимальное значение 5. Обозначает допустимое количество символов, которые по-прежнему будут соответствовать имени сущности. Возвращается наименьшее возможное значение нечеткости для любого заданного соответствия.  Например, если расстояние правки равно 3, "Windows 10" будет по-прежнему соответствовать "Windows", "Windows 10" и "Windows 7". <br/> Если параметр чувствительность к регистру имеет значение false, разница в регистрах не учитывается в отношении допустимости нечеткости, но в противном случае —. |
| дефаулткасесенситиве | Используемых Изменяет значение чувствительности к регистру по умолчанию для этой сущности. Он используется для изменения значения по умолчанию всех псевдонимов caseSensitive. |
| дефаултфуззедитдистанце | Используемых Изменяет значение расстояния нечеткого редактирования по умолчанию для этой сущности. Его можно использовать для изменения значения по умолчанию для всех псевдонимов Фуззедитдистанце значений. |
| псевдоним | Используемых Массив сложных объектов, которые можно использовать для указания альтернативного написания или синонимов имени корневой сущности. |

| Свойства псевдонима | Description |
|------------------|-------------|
| text  | Альтернативное написание или представление имени некоторой целевой сущности.  |
| caseSensitive | Используемых Действует так же, как и параметр "caseSensitive" корневой сущности, но применяется только к этому псевдониму. |
| фуззедитдистанце | Используемых Действует так же, как параметр корневой сущности "Фуззедитдистанце" выше, но применяется только к этому псевдониму. |


### <a name="inline-format"></a>Встроенный формат

В некоторых случаях может быть удобнее предоставить список пользовательских сущностей для согласованного сопоставления с определением навыка. В этом случае можно использовать аналогичный формат JSON, описанный выше, но он встроен в определение навыка.
Встроенными могут быть определены только конфигурации размером менее 10 КБ (сериализованный размер). 

##  <a name="sample-definition"></a>Пример определения

Ниже показан пример определения навыка с использованием встроенного формата.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.CustomEntityLookupSkill",
    "context": "/document",
    "inlineEntitiesDefinition": 
    [
      { 
        "name" : "Bill Gates",
        "description" : "Microsoft founder." ,
        "aliases" : [ 
            { "text" : "William H. Gates", "caseSensitive" : false },
            { "text" : "BillG", "caseSensitive" : true }
        ]
      }, 
      { 
        "name" : "Xbox One", 
        "type": "Harware",
        "subtype" : "Gaming Device",
        "id" : "4e36bf9d-5550-4396-8647-8e43d7564a76",
        "description" : "The Xbox One product"
      }
    ],    
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "entities",
        "targetName": "matchedEntities"
      }
    ]
  }
```
Кроме того, если вы решили предоставить указатель на файл определения сущностей, ниже показан пример определения навыка с использованием формата Ентитиесдефинитионури:

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.CustomEntityLookupSkill",
    "context": "/document",
    "entitiesDefinitionUri": "https://myblobhost.net/keyWordsConfig.csv",    
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "entities",
        "targetName": "matchedEntities"
      }
    ]
  }

```

##  <a name="sample-input"></a>Пример ввода

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The company microsoft was founded by Bill Gates. Microsoft's gaming console is called Xbox",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Пример выходных данных

```json
  { 
    "values" : 
    [ 
      { 
        "recordId": "1", 
        "data" : { 
          "entities": [
            { 
              "name" : "Microsoft", 
              "description" : "This document refers to Microsoft the company", 
              "id" : "differentIdentifyingScheme987", 
              "matches" : [ 
                { 
                  "text" : "microsoft", 
                  "offset" : 13, 
                  "length" : 9, 
                  "matchDistance" : 0 
                }, 
                { 
                  "text" : "Microsoft",
                  "offset" : 49, 
                  "length" : 9, 
                  "matchDistance" : 0
                }
              ] 
            },
            { 
              "name" : "Bill Gates",
              "description" : "William Henry Gates III, founder of Microsoft.", 
              "matches" : [
                { 
                  "text" : "Bill Gates",
                  "offset" : 37, 
                  "length" : 10,
                  "matchDistance" : 0 
                }
              ]
            }
          ] 
        } 
      } 
    ] 
  } 
```

## <a name="see-also"></a>См. также

+ [Встроенные навыки](cognitive-search-predefined-skills.md)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Навык распознавания сущностей (для поиска хорошо известных сущностей)](cognitive-search-skill-entity-recognition.md)