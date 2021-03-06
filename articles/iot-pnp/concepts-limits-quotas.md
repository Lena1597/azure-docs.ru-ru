---
title: Ограничения и квоты Plug and Play предварительной версии IoT | Документация Майкрософт
description: Ознакомьтесь с ограничениями, квотами и регулированием, которые применяются при использовании предварительной версии IoT Plug and Play.
author: miagdp
ms.author: miag
ms.date: 12/26/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 48ecaaba6d956efd9da75d0582fa06d231cb3f80
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/28/2019
ms.locfileid: "75531383"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>Ограничения предварительной версии, квоты и регулирования для Интернета вещей Plug and Play

В этой статье описываются ограничения, квоты и регулирование, которые применяются в общедоступной предварительной версии Plug and Play IoT. Существуют существующие [квоты и регулирование для центра Интернета вещей](../iot-hub/iot-hub-devguide-quotas-throttling.md) , которые также применяются.

## <a name="iot-hub"></a>Центр Интернета вещей

Для общедоступной предварительной версии следующие ограничения и квоты применяются к центру Интернета вещей:

| Ограничения, ограничения и регулировки | Значение | Примечания |
|-----|-----|-----|
| Число моделей возможностей устройства (Дкмс) или интерфейсов, которые могут быть зарегистрированы для каждого концентратора. | 1500 ||
| Максимальное число интерфейсов, которые могут быть зарегистрированы для каждого устройства | 40 ||
| Максимальное число Дкмс, которые могут быть зарегистрированы для каждого устройства | 1 ||
| Максимальный размер интерфейса или файла DCM | 512 знаков ||
| Максимальный размер имени интерфейса | 256 знаков ||
| Максимальный размер имени свойства  | 64 байт, глубина 7 уровней (и первый уровень зарезервирован для `$iotin`) | Допустимые символы: a – z, A – Z, 0-9 (не в качестве первого символа) и символ подчеркивания. |
| Максимальный размер значения свойства | 512 байт ||
| Максимальный размер имени команды | 100 байт ||
| Размер двойника устройства | То же, что и [ограничения центра Интернета вещей](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) ||
| Вызовы API разрешения в номере SKU (независимо от единиц) | 100 запросов в секунду ||

## <a name="model-repository"></a>Репозиторий модели

Для общедоступной предварительной версии к хранилищу моделей применяются следующие ограничения и квоты.

| Ограничения, ограничения и регулировки| Значение |
|-----|-----|
| Число репозиториев модели компании на клиент Azure Active Directory | 1 |
| Число ключей авторизации на репозиторий моделей | 10  |
| Количество моделей (Дкмс или интерфейсов) на репозиторий моделей компании| 1500  |
| Число моделей (Дкмс или интерфейсов) в репозитории общедоступных моделей для каждого Azure Active Directory клиента| 1500  |
| Число Дкмс или интерфейсов, удаляемых в репозитории модели компании | 10 запросов в секунду (QPS)|
| Число репозиториев модели, создаваемых или обновленных клиентом| 1 QPS |
| Число создаваемых, обновленных или удаленных ключей авторизации в репозитории модели | 1 QPS|
| Число Дкмс, создаваемых в репозитории модели компании | 10 QPS |
| Число интерфейсов, создаваемых в репозитории модели компании | 10 QPS|
| Число Дкмс, создаваемых в репозитории общедоступной модели | 10 QPS|
| Число интерфейсов, создаваемых в репозитории общедоступной модели | 10 QPS|

## <a name="parser-library"></a>Библиотека средства синтаксического анализа

Библиотека средства синтаксического анализа соответствует ограничениям, которые применяются к [языку определения Digital двойника](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL).

## <a name="next-steps"></a>Дальнейшие действия

Предлагаемый следующий шаг — Узнайте, как [подключиться к устройству Plug and Play IOT и взаимодействовать с ним](./howto-develop-solution.md).
