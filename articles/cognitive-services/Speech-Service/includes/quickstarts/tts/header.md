---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: e9f02f95693552180a0eed1550cc59d8f975416b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961521"
---
При работе с этим кратким руководством будет использоваться [пакет SDK службы "Речь"](~/articles/cognitive-services/speech-service/speech-sdk.md) для преобразования текста в синтезированную речь. В службе преобразования текста в речь предоставляется множество вариантов синтезированных голосов в рамках [поддерживаемых языков для преобразования текста в речь](../../../language-support.md#text-to-speech). После выполнения нескольких предварительных требований преобразование синтезированной речи с помощью говорящих по умолчанию укладывается всего в четыре шага:
> [!div class="checklist"]
> * Создайте объект `SpeechConfig`, содержащий ключ и регион подписки.
> * Создайте объект `SpeechSynthesizer`, используя приведенный выше объект `SpeechConfig`.
> * Используйте объект `SpeechSynthesizer` для диктовки текста.
> * Проверьте возвращенный результат `SpeechSynthesisResult` на наличие ошибок.
