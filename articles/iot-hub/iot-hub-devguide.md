---
title: Руководство разработчика для Центра Интернета вещей Azure | Документация Майкрософт
description: Руководство разработчика для Центра Интернета вещей Azure включает в себя разделы, посвященные конечным точкам, безопасности, реестру удостоверений, управлению устройствами, прямым методам, двойникам устройств, передаче файлов, заданиям, языку запросов Центра Интернета вещей и обмену сообщениями.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 1ff7d430edd3f638ad5efcc5a89604e4ed732211
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400156"
---
# <a name="azure-iot-hub-developer-guide"></a>Руководство разработчика для Центра Интернета вещей Azure

Центр Интернета вещей Azure — это полностью управляемая служба, которая обеспечивает надежный и защищенный двунаправленный обмен данными между миллионами устройств и серверной частью решения.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Центр Интернета вещей Azure обеспечивает следующие возможности:

* безопасную связь благодаря использованию уникальных учетных данных безопасности устройства и контроля доступа;

* несколько вариантов глобального взаимодействия между устройствами и облаком;

* запрашиваемое хранилище, содержащее сведения о состоянии каждого устройства и его метаданные;

* простое подключение устройств, так как используются библиотеки устройств для большинства популярных языков и платформ.

В этом руководстве разработчика для Центра Интернета вещей содержатся следующие статьи:

* [Руководство по обмену данными между устройством и облаком](iot-hub-devguide-d2c-guidance.md): помощь в принятии решения об использовании сообщений, отправляемых с устройства в облако, сообщаемых свойств двойника устройства или передачи файлов.

* [Руководство по обмену данными между облаком и устройством](iot-hub-devguide-c2d-guidance.md): помощь в принятии решения об использовании прямых методов, требуемых свойств двойника устройства и сообщений, отправляемых из облака на устройство.

* Статья [Отправка сообщений с устройства в облако и из облака на устройство с помощью Центра Интернета вещей](iot-hub-devguide-messaging.md) содержит сведения о функциях обмена сообщениями (между устройством и облаком в обоих направлениях), которые предоставляет Центр Интернета вещей.

  * [Использование маршрутизации сообщений для отправки с устройства в облако на различные конечные точки](iot-hub-devguide-messages-d2c.md).

  * [Чтение сообщений, пересылаемых с устройства в облако, из встроенной конечной точки](iot-hub-devguide-messages-read-builtin.md).

  * [Использование правил маршрутизации и пользовательских конечных точек для сообщений, отправляемых с устройства в облако](iot-hub-devguide-messages-read-custom.md).

  * [Отправка сообщений, пересылаемых из облака на устройство, из Центра Интернета вещей](iot-hub-devguide-messages-c2d.md).

  * [Создание и чтение сообщений Центра Интернета вещей](iot-hub-devguide-messages-construct.md).

* [Отправка файлов с помощью Центра Интернета вещей](iot-hub-devguide-file-upload.md): сведения о том, как передавать файлы с устройства. В этой статье также содержатся сведения по темам, таким как уведомления, отправляемые процессом передачи.

* [Общие сведения о реестре удостоверений в Центре Интернета вещей](iot-hub-devguide-identity-registry.md): информация о том, какие сведения хранятся в каждом реестре удостоверений Центра Интернета вещей. В статье также описано, как можно получить доступ к этим сведениям и изменять их.

* [Управление доступом к Центру Интернета вещей](iot-hub-devguide-security.md): сведения о модели безопасности, которая обеспечивает доступ к функциям Центра Интернета вещей для устройств и облачных компонентов. Статья содержит сведения об использовании маркеров и сертификатов X.509, а также о разрешениях, которые вы можете предоставлять.

* [Общие сведения о двойниках устройств и их использование в Центре Интернета вещей](iot-hub-devguide-device-twins.md): сведения о концепции *двойника устройства*. В статье также описаны функциональные возможности устройства, которое предоставляет двойники, например синхронизация устройства с близнецом устройства. Эта статья также содержит сведения о данных, хранящихся в двойнике устройства.

* [Общие сведения о прямых методах и информация о вызове этих методов из Центра Интернета вещей](iot-hub-devguide-direct-methods.md): данные о жизненном цикле прямого метода. В статье описано, как вызывать методы на устройство из внутреннего приложения и как обрабатывать прямой метод на устройстве.

* [Планирование заданий на нескольких устройствах](iot-hub-devguide-jobs.md): сведения о планировании заданий на нескольких устройствах. В статье описывается, как отправлять задания, которые выполняют задачи, такие как выполнение прямого метода и обновление устройства с помощью двойника устройства. В ней также описывается, как запрашивать состояние задания.

* [Справочник — выбор протокола связи](iot-hub-devguide-protocols.md) содержит описание протоколов связи, поддерживаемых Центром Интернета вещей для взаимодействия с устройствами, а также список портов, которые должны быть открыты.

* [Руководство. Конечные точки Центра Интернета вещей](iot-hub-devguide-endpoints.md): сведения о конечных точках, которые каждый Центр Интернета вещей предоставляет для операций управления и среды выполнения. Кроме того, из этой статьи вы узнаете, как создать дополнительные конечные точки в Центре Интернета вещей и использовать полевой шлюз для подключения к конечным точкам Центра Интернета вещей в нестандартных сценариях.

* [Язык запросов Центра Интернета вещей для двойников устройств и двойников модулей, заданий и маршрутизации сообщений](iot-hub-devguide-query-language.md) содержит сведения о языке запросов, который можно использовать для получения сведений о двойниках устройств и заданиях из Центра Интернета вещей.

* [Руководство. Квоты и регулирование в Центре Интернета вещей](iot-hub-devguide-quotas-throttling.md): общие сведения о квотах, установленных в службе Центра Интернета вещей, и о регулировании, которое выполняется при превышении квоты.

* [Сведения о ценах на Центр Интернета вещей Azure](iot-hub-devguide-pricing.md): общие сведения о различных номерах SKU и ценообразовании для Центра Интернета вещей, а также сведения о том, как различные функциональные возможности Центра Интернета вещей тарифицируются как сообщения.

* [Понимание и использование пакетов SDK для Центра Интернета вещей Azure](iot-hub-devguide-sdks.md): перечень пакетов SDK для Azure IoT для разработки приложений для устройств и служб, взаимодействующих с Центром Интернета вещей. Статья содержит ссылки на электронную документацию по API.

* [Взаимодействие с Центром Интернета вещей с помощью протокола MQTT](iot-hub-mqtt-support.md): подробные сведения о том, как в Центре Интернета вещей поддерживается протокол MQTT. В статье описывается поддержка протокола MQTT, встроенная в пакеты SDK для Azure IoT, а также предоставляются сведения об использовании протокола MQTT напрямую.

* [Глоссарий терминов, связанных с Центром Интернета вещей](iot-hub-devguide-glossary.md): список распространенных терминов, связанных с Центром Интернета вещей.