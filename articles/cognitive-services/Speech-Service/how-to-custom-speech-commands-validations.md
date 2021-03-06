---
title: Как добавить проверки в пользовательские параметры команды (Предварительная версия)
titleSuffix: Azure Cognitive Services
description: В этой статье объясняется, как добавить проверки в параметр в пользовательских командах.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: cf6e4e4f0bfab43fb738f8415022e55fcbcbd05a
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156460"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Как добавить проверки в пользовательские параметры команды (Предварительная версия)

В этой статье вы узнаете, как добавлять проверки в параметры и запрашивать исправление.

## <a name="prerequisites"></a>Технические условия

Необходимо выполнить действия, описанные в следующих статьях:

- [Краткое руководство. Создание настраиваемой команды (Предварительная версия)](./quickstart-custom-speech-commands-create-new.md)
- [Краткое руководство. Создание настраиваемой команды с параметрами (Предварительная версия)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Создание команды SetTemperature

Чтобы продемонстрировать проверки, создадим новую команду, позволяющую пользователю задать температуру.

1. Открытие созданного ранее приложения настраиваемых команд в [Speech Studio](https://speech.microsoft.com/)
1. Создание новой команды **SetTemperature**
1. Добавление параметра для целевой температуры
1. Добавление проверки для параметра температуры
   > [!div class="mx-imgBorder"]
   > ![добавить проверку диапазона](media/custom-speech-commands/validations-add-temperature.png)

   | Параметр           | Рекомендуемое значение                                          | Description                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | Имя              | Температура                                              | Описательное имя для параметра команды                                                    |
   | Обязательно для заполнения          | true                                                     | Флажок, указывающий, требуется ли значение этого параметра перед выполнением команды |
   | Шаблон ответа | "— Какая температура вам нравится?"                     | Запрос на запрос значения этого параметра, если он неизвестен                              |
   | Тип              | Число                                                   | Тип параметра, например число, строка или Дата и время                                      |
   | Проверка        | Минимальное значение: 60, максимальное значение: 80                             | Для числовых параметров допустимый диапазон значений для параметра                             |
   | Шаблон ответа | "-Увы, можно установить только между 60 и 80 градусами"      | Запрашивать обновленное значение, если проверка не пройдена                                       |

1. Добавление примеров предложений

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Добавление правила завершения для подтверждения результата

   | Параметр    | Рекомендуемое значение                                           | Description                                        |
   | ---------- | --------------------------------------------------------- | -------------------------------------------------- |
   | Имя правила  | Сообщение с подтверждением                                      | Имя, описывающее назначение правила.          |
   | Условия | Обязательный параметр — температура                          | Условия, определяющие, когда может выполняться правило    |
   | Действия    | Спичреспонсе-"-ОК", установка равна {температура} градусов " | Действие, выполняемое, если условие правила имеет значение true |

> [!TIP]
> В этом примере для подтверждения результата используется речевой ответ. Примеры выполнения команды с помощью действия клиента см. в разделе [как выполнять команды на клиенте с помощью пакета SDK для распознавания речи (Предварительная версия)](./how-to-custom-speech-commands-fulfill-sdk.md) .

## <a name="try-it-out"></a>Попробуйте в деле

Выберите панель "тестирование" и попробуйте несколько взаимодействий.

- Входные данные: установите температуру в 72 градусов.
- Выходные данные: "ОК, значение — 72 градусов"

- Входные данные: установите температуру в 45 градусов.
- Выходные данные: "я могу установить только между 60 и 80 градусами"
- Входные данные: сделайте его 72 градусов
- Выходные данные: "ОК, значение — 72 градусов"

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Как добавить подтверждение в настраиваемую команду (Предварительная версия)](./how-to-custom-speech-commands-confirmations.md)
