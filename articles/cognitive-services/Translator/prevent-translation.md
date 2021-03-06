---
title: Запрет перевода содержимого — API перевода текстов
titleSuffix: Azure Cognitive Services
description: Запрет перевода содержимого с помощью API перевода текстов. API перевода текстов позволяет помечать содержимое тегами, чтобы оно не переводилось.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: swmachan
ms.openlocfilehash: 15a36451c18d65df6667f24284f3f69f3d1c06b8
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326761"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Запрет перевода содержимого с помощью API перевода текстов

API перевода текстов позволяет помечать содержимое тегами, чтобы оно не переводилось. Например, можно пометить тегами код, название торговой марки, а также слово или фразу, которая не имеет смысла при локализации.

## <a name="methods-for-preventing-translation"></a>Способы запрета перевода
1. Для экранирования используйте тег Twitter @somethingtopassthrough или #somethingtopassthrough. Отмените экранирование после перевода. Это регулярное выражение для допустимых тегов Twitter: `\B@[A-Za-z]+[A-Za-z0-9_]+)`. Тег должен начинаться с символа "@", за которым следует символ, а затем один или несколько символов, цифр или символ подчеркивания. Рекомендуется, чтобы теги были короткими, а открывающим тегом — пробел.

2. Пометьте содержимое с помощью `notranslate`. Это можно сделать, только если входные Тексттипе заданы в формате HTML.

   Пример:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Используйте [динамический словарь](dynamic-dictionary.md), чтобы настроить требуемый перевод.

4. Не передавайте строку в API перевода текстов для перевода.

5. Пользовательский переводчик. Используйте [словарь в пользовательском трансляторе](custom-translator/what-is-dictionary.md) , чтобы предписывает перевод фразы с вероятностью 100%.


## <a name="next-steps"></a>Дополнительная информация
> [!div class="nextstepaction"]
> [Запрет перевода при вызове API перевода текстов](reference/v3-0-translate.md)
