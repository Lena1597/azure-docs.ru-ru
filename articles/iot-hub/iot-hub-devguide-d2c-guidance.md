---
title: Параметры отправки сообщений из устройства в облако с помощью Центра Интернета вещей Azure | Документация Майкрософт
description: Руководство разработчика. Рекомендации по использованию сообщений, отправляемых с устройства в облако, сообщаемых свойств и отправки файлов для обмена данными между облаком и устройством.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: fffa064b912a96b05feb901d1d2d44533c4681b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60885522"
---
# <a name="device-to-cloud-communications-guidance"></a>Руководство по обмену данными между устройством и облаком

Центр Интернета вещей предоставляет три варианта отправки сведений из приложения для устройства в серверную часть решения.

* [Сообщения, отправляемые с устройства в облако](iot-hub-devguide-messages-d2c.md), используются для телеметрии и оповещений с учетом временных рядов.

* [Сообщаемые свойства двойника устройства](iot-hub-devguide-device-twins.md) предназначены для передачи сведений о состоянии устройства, таких как доступные возможности, условия и состояние длительных рабочих процессов. Например, настройка и обновления программного обеспечения.

* [Передача файлов](iot-hub-devguide-file-upload.md) используется для отправки файлов мультимедиа и больших пакетов телеметрии, передаваемых периодически подключаемыми устройствами или сжатых для экономии пропускной способности.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Ниже приведено подробное сравнение различных параметров обмена данными между устройством и облаком.

|  | Отправка сообщений с устройства в облако | Сообщаемые свойства двойника устройства | Передача файлов |
| ---- | ------- | ---------- | ---- |
| Сценарий | Временные интервалы и оповещения телеметрии. Например, пакеты данных датчика размером 256 КБ отправляются каждые 5 минут. | Доступные возможности и условия. Текущий режим подключения устройства, например мобильная связь или Wi-Fi. Синхронизация длительных рабочих процессов, таких как конфигурация и обновления программного обеспечения. | Файлы мультимедиа. Большие пакеты данных телеметрии (обычно сжатые). |
| Хранение и извлечение | Временно хранятся в Центре Интернета вещей (до 7 дней). Только последовательное чтение. | Сохраняются в Центре Интернета вещей в двойнике устройства. Извлекаются с помощью [языка запросов Центра Интернета вещей](iot-hub-devguide-query-language.md). | Хранятся в учетной записи хранения Azure, предоставленной пользователем. |
| Размер | До 256 КБ сообщений. | Максимальный размер передаваемого свойства — 8 КБ. | Максимальный размер файла, поддерживаемый хранилищем BLOB-объектов Azure. |
| Frequency | Высокий. Дополнительные сведения см. в статье [Руководство. Квоты и регулирование в Центре Интернета вещей](iot-hub-devguide-quotas-throttling.md). | Средний. Дополнительные сведения см. в статье [Руководство. Квоты и регулирование в Центре Интернета вещей](iot-hub-devguide-quotas-throttling.md). | Низкий. Дополнительные сведения см. в статье [Руководство. Квоты и регулирование в Центре Интернета вещей](iot-hub-devguide-quotas-throttling.md). |
| Протокол | Доступно при использовании всех протоколов. | Доступно при использовании MQTT или AMQP. | Доступно при использовании любого протокола, но на устройстве требуется HTTPS. |

Приложению может потребоваться отправка сведений как в виде телеметрии временных рядов, так и в виде оповещений, которые должны быть доступны в двойнике устройства. В этом сценарии можно выбрать один из следующих вариантов:

* Приложение устройства отправляет сообщение с устройства в облако и сообщает об изменении свойств.
* Серверная часть решения может сохранить данные в тегах двойника устройства после получения сообщения.

Так как сообщения, отправляемые с устройства в облако, поддерживают более высокую пропускную способность, чем обновления двойника устройства, желательно иногда избегать обновления двойника устройства после получения каждого такого сообщения.