---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 09aa5affefc576606984ea7116b3d9bda4ba83d5
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704259"
---
### <a name="run-multiple-containers-on-the-same-host"></a>Запуск нескольких контейнеров на одном узле

Если вы планируете запускать несколько контейнеров при открытых портах, обязательно назначьте каждому контейнеру отдельный открытый порт. Например, запускайте первый контейнер на порте 5000, а второй — на порте 5001.

Этот контейнер и другой контейнер Azure Cognitive Services могут одновременно работать на одном и том же узле. Также одному запущенному контейнеру Cognitive Services могут соответствовать сразу несколько контейнеров.
