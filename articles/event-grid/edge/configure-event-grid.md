---
title: Настройка — служба "Сетка событий Azure" IoT Edge | Документация Майкрософт
description: Настройка в сетке событий на IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 841b5092775353bbe3340dbbd55610026f998a15
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846470"
---
# <a name="event-grid-configuration"></a>Конфигурация сетки событий

Сетка событий предоставляет множество конфигураций, которые можно изменять для каждой среды. В следующем разделе приведена ссылка на все доступные параметры и их значения по умолчанию.

## <a name="tls-configuration"></a>Конфигурация TLS

Общие сведения о проверке подлинности клиента см. в разделе [безопасность и проверка подлинности](security-authentication.md). Примеры использования можно найти в [этой статье](configure-api-protocol.md).

| Имя свойства | Description |
| ---------------- | ------------ |
|`inbound__serverAuth__tlsPolicy`| Политика TLS модуля "Сетка событий". Значение по умолчанию — только HTTPS.
|`inbound__serverAuth__serverCert__source`| Источник сертификата сервера, используемый модулем сетки событий для своей конфигурации TLS. Значение по умолчанию — IoT Edge.

## <a name="incoming-client-authentication"></a>Входящая проверка подлинности клиента

Общие сведения о проверке подлинности клиента см. в разделе [безопасность и проверка подлинности](security-authentication.md). Примеры можно найти в [этой статье](configure-client-auth.md).

| Имя свойства | Description |
| ---------------- | ------------ |
|`inbound__clientAuth__clientCert__enabled`| Включение и отключение проверки подлинности клиента на основе сертификата. Значение по умолчанию — true.
|`inbound__clientAuth__clientCert__source`| Источник для проверки сертификатов клиента. Значение по умолчанию — IoT Edge.
|`inbound__clientAuth__clientCert__allowUnknownCA`| Политика, разрешающая самозаверяющий сертификат клиента. Значение по умолчанию — true.
|`inbound__clientAuth__sasKeys__enabled`| Включение/выключение проверки подлинности клиента на основе ключа SAS. Значение по умолчанию — OFF.
|`inbound__clientAuth__sasKeys__key1`| Одно из значений для проверки входящих запросов.
|`inbound__clientAuth__sasKeys__key2`| Необязательное второе значение для проверки входящих запросов.

## <a name="outgoing-client-authentication"></a>Исходящая проверка подлинности клиента
Общие сведения о проверке подлинности клиента см. в разделе [безопасность и проверка подлинности](security-authentication.md). Примеры можно найти в [этой статье](configure-identity-auth.md).

| Имя свойства | Description |
| ---------------- | ------------ |
|`outbound__clientAuth__clientCert__enabled`| Включение и отключение подключения сертификата удостоверения для исходящих запросов. Значение по умолчанию — true.
|`outbound__clientAuth__clientCert__source`| Источник для получения исходящего сертификата модуля сетки событий. Значение по умолчанию — IoT Edge.

## <a name="webhook-event-handlers"></a>Обработчики событий веб-перехватчика

Общие сведения о проверке подлинности клиента см. в разделе [безопасность и проверка подлинности](security-authentication.md). Примеры можно найти в [этой статье](configure-webhook-subscriber-auth.md).

| Имя свойства | Description |
| ---------------- | ------------ |
|`outbound__webhook__httpsOnly`| Политика, определяющая, будут ли разрешены только подписчики HTTPS. Значение по умолчанию — true (только HTTPS).
|`outbound__webhook__skipServerCertValidation`| Флаг, определяющий, следует ли проверять сертификат подписчика. Значение по умолчанию — true.
|`outbound__webhook__allowUnknownCA`| Политика для управления тем, может ли подписчик предоставлять самозаверяющий сертификат. Значение по умолчанию — true. 

## <a name="delivery-and-retry"></a>Доставка и повторные попытки доставки

Общие сведения об этой функции см. в разделе [Доставка и повторная попытка](delivery-retry.md).

| Имя свойства | Description |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Максимальное число попыток доставки события. Значение по умолчанию — 30.
| `broker__defaultEventTimeToLiveInSeconds` | Срок жизни (TTL) (в секундах), после которого событие будет удалено, если оно не доставлено. Значение по умолчанию — **7200** секунд.

## <a name="output-batching"></a>Пакетная обработка выходных данных

Общие сведения об этой функции см. в статье [Пакетная обработка доставки и вывода данных](delivery-output-batching.md).

| Имя свойства | Description |
| ---------------- | ------------ |
| `api__deliveryPolicyLimits__maxBatchSizeInBytes` | Максимальное значение, допустимое для `ApproxBatchSizeInBytes`ного регулятора. Значение по умолчанию: `1_058_576`.
| `api__deliveryPolicyLimits__maxEventsPerBatch` | Максимальное значение, допустимое для `MaxEventsPerBatch`ного регулятора. Значение по умолчанию: `50`.
| `broker__defaultMaxBatchSizeInBytes` | Максимальный размер запроса на доставку, если указан только `MaxEventsPerBatch`. Значение по умолчанию: `1_058_576`.
| `broker__defaultMaxEventsPerBatch` | Максимальное число событий, добавляемых в пакет, если указано только `MaxBatchSizeInBytes`. Значение по умолчанию: `10`.

## <a name="metrics"></a>Метрики

Дополнительные сведения об использовании метрик с таблицей событий в IoT Edge см. в разделе [мониторинг разделов и подписок](monitor-topics-subscriptions.md) .

| Имя свойства | Description |
| ---------------- | ------------ |
| `metrics__reporterType` | Тип создателя для метрик енпоинт. Значение по умолчанию — `none` и отключает метрики. Значение `prometheus` включает метрики в формате демонстрации Prometheus.