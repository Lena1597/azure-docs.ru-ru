---
title: Общие сведения об Azure Cognitive Services
titleSuffix: Azure Cognitive Services
description: Azure Cognitive Services — это API-интерфейсы, пакеты SDK и службы, которые можно использовать для создания интеллектуальных приложений с помощью Microsoft Azure.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: overview
ms.date: 12/19/2019
ms.author: nitinme
ms.openlocfilehash: 332f33bb4046a9ca9d6abf9bec75f60bb4ca9e32
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76169098"
---
# <a name="what-are-azure-cognitive-services"></a>Общие сведения об Azure Cognitive Services

Azure Cognitive Services — это API-интерфейсы, пакеты SDK и службы, которые помогают создавать интеллектуальные приложения разработчикам без опыта работы со средствами искусственного интеллекта и обработки и анализа данных. Azure Cognitive Services позволяют разработчикам легко добавлять когнитивные функции в свои приложения. Задача Azure Cognitive Services — помочь разработчикам создавать приложения, которые умеют видеть, слышать, разговаривать и даже в некоторой степени размышлять. Службы Azure Cognitive Services можно разделить на пять категорий, которые связаны со зрением, речью, языком, поиском в Интернете и принятием решений.

## <a name="vision-apis"></a>API, связанные со зрением

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Компьютерное зрение](https://docs.microsoft.com/azure/cognitive-services/computer-vision/ "API Компьютерного зрения")|Служба "Компьютерное зрение" предоставляет доступ к передовым алгоритмам обработки изображений и возврата данных.|
|[Пользовательское визуальное распознавание](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/home "Служба "Пользовательское визуальное распознавание"")|Пользовательская служба визуального распознавания позволяет создавать пользовательские классификаторы изображений.|
|[Распознавание лиц](https://docs.microsoft.com/azure/cognitive-services/face/ "Распознавание лиц")| Служба "Распознавание лиц" обеспечивает доступ к расширенным алгоритмам, позволяя определять и распознавать лица на основе атрибутов.|
|[Распознаватель документов](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/ "Распознаватель документов") (предварительная версия)|Распознаватель документов идентифицирует и извлекает пары "ключ —значение" и данные таблиц из документов, а затем выводит структурированные данные, включая отношения в исходном файле.|
|[Распознаватель рукописного текста](https://docs.microsoft.com/azure/cognitive-services/ink-recognizer/ "Распознаватель рукописного текста") (предварительная версия)|Распознаватель рукописного текста позволяет распознавать и анализировать данные мазка кистью рукописного ввода, документы и рукописное содержимое, а также выводить структуру документа со всеми распознанными объектами.|
|[Индексатор видео](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview "Индексатор видео")|Индексатор видео позволяет извлекать аналитические сведения из видео.|

## <a name="speech-apis"></a>API, связанные с речью

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Служба распознавания речи](https://docs.microsoft.com/azure/cognitive-services/speech-service/ "Служба "Речь"")|Служба "Речь" позволяет добавлять в приложения функции с поддержкой речи.|
|[API Распознавания говорящего](https://docs.microsoft.com/azure/cognitive-services/speaker-recognition/home "API Распознавания говорящего") (предварительная версия)|API распознавания говорящего обеспечивает доступ к алгоритмам идентификации говорящего.|
|[Распознавание речи Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home "API распознавания речи Bing") (предварительная версия)|API распознавания речи Bing позволяет без усилий добавлять в приложения функции с поддержкой речи.|
|[Перевод речи](https://docs.microsoft.com/azure/cognitive-services/translator-speech/ "Перевод речи") (предварительная версия)|Перевод речи — это служба машинного перевода.|

> [!NOTE]
> Хотите узнать о [Когнитивном поиске Azure](https://docs.microsoft.com/azure/search/)? Хотя для некоторых задач используется Cognitive Services, это другая технология поиска, поддерживающая иные сценарии.


## <a name="language-apis"></a>API, связанные с языком

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Распознавание речи (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/ "Распознавание речи")|Интеллектуальная служба распознавания речи (LUIS) позволяет приложению понимать желания пользователей, выраженные словами.|
|[QnA Maker](https://docs.microsoft.com/azure/cognitive-services/qnamaker/index "QnA Maker")|Служба QnA Maker позволяет создавать службу для работы с вопросами и ответами на основе частично структурированного содержимого.|
|[Анализ текста](https://docs.microsoft.com/azure/cognitive-services/text-analytics/ "Анализ текста")|Служба "Анализ текста" — это служба обработки естественного языка, выполняющая анализ тональности необработанного текста, извлечение ключевых фраз и определение языка.|
|[Перевод текстов](https://docs.microsoft.com/azure/cognitive-services/translator/ "Перевод текстов")|Служба "Перевод текстов" предназначена для машинного перевода текстов практически в реальном времени.|


## <a name="search-apis"></a>Интерфейсы API для поиска

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Поиск новостей Bing](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/ "API Поиска новостей Bing")|Служба "Поиск новостей Bing" выполняет поиск новостных статей релевантных запросу пользователя.|
|[Поиск видео Bing](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search/ "API Поиска видео Bing")|Служба "Поиск видео Bing" выполняет поиск видео, релевантных запросу пользователя.|
|[Поиск в Интернете Bing](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/ "API Поиска в Интернете Bing")|Служба "Поиск в Интернете Bing" выполняет поиск материалов в Интернете, релевантных запросу пользователя.|
|[Автозаполнение Bing](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest "API Автозаполнения Bing")|Служба "Автозаполнение Bing" предлагает отправлять в Bing часть слова поискового запроса и получать список предлагаемых запросов.|
|[Пользовательский поиск Bing](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search "Пользовательский поиск Bing")|Служба "Пользовательский поиск Bing" позволяет выполнять пользовательскую настройку функции поиска требуемых материалов.|
|[Поиск сущностей Bing](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/ "API Поиска сущностей Bing")|Служба "Поиск сущностей Bing" возвращает сведения о сущностях, которые Bing определяет как релевантные запросу пользователя.|
|[Поиск изображений Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search "API Поиска изображений Bing")|Служба "Поиск изображений Bing" отображает изображения, релевантные запросу пользователя.|
|[Визуальный поиск Bing](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search "Визуальный поиск Bing")|Служба "Визуальный поиск Bing" возвращает аналитические сведения об изображении, например визуально схожие изображения, ресурсы, на которых можно купить продукты, найденные на изображении, а также связанные результаты поиска.|
|[Поиск местных компаний Bing](https://docs.microsoft.com/azure/cognitive-services/bing-local-business-search/ "Поиск местных компаний Bing")| С помощью API Поиска местных компаний Bing для приложений можно находить контактную информацию о местных компаниях, а также сведения об их расположении на основе поисковых запросов.|
|[Проверка орфографии Bing](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/ "API Проверки орфографии Bing")|Служба "Проверка орфографии Bing" предназначена для контекстной проверки грамматики и орфографии.|

## <a name="decision-apis"></a>API-интерфейсы принятия решений

|Имя службы|Описание службы|
|:-----------|:------------------|
|[Детектор аномалий](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/ "Детектор аномалий") (предварительная версия)|Детектор аномалий позволяет отслеживать и обнаруживать отклонения в данных временных рядов.|
|[Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview "Content Moderator")|Content Moderator позволяет отслеживать потенциально оскорбительное, нежелательное и представляющее риск содержимое.|
|[Персонализатор](https://docs.microsoft.com/azure/cognitive-services/personalizer/ "Персонализатор")|Персонализатор позволяет выбирать максимально удобный режим работы для своих пользователей, изучая их поведение в реальном времени.|

## <a name="use-free-trials"></a>Воспользуйтесь бесплатными пробными версиями

При [регистрации для получения бесплатных пробных версий](https://azure.microsoft.com/try/cognitive-services/ "Справка по регистрации") требуется только указать адрес электронной почты и выполнить несколько простых действий. У вас должна быть учетная запись Майкрософт. Вы получите уникальную пару ключей для каждого запрошенного API. Второй ключ — просто запасной. Эти ключи необходимо хранить в секрете. Бесплатные пробные версии имеют ограничение по частоте проведения транзакций в секунду или минуту, а также по объему использования в месяц. Транзакция — это просто вызов API. Вы можете обновиться до платных уровней, чтобы снять ограничения.

## <a name="subscription-management"></a>Управление подпиской

После входа с учетной записью Майкрософт в разделе [Мои подписки](https://www.microsoft.com/cognitive-services/subscriptions "Мои подписки") можно просмотреть используемые продукты, оставшуюся квоту и возможность добавления других продуктов в подписку.

## <a name="upgrade-to-unlock-limits"></a>Обновление для снятия ограничений

Все API-интерфейсы предлагаются в бесплатной пробной версии с ограничениями по использованию и пропускной способности.  Вы можете увеличить эти ограничения, используя платное предложение и выбирая соответствующую ценовую категорию при развертывании службы на портале Azure. [Узнайте больше о предложениях и ценах](https://azure.microsoft.com/pricing/details/cognitive-services/ "Предложения и цены"). Для этого потребуется настроить учетную запись подписчика Azure, указав данные кредитной карты и номер телефона. Если у вас есть особые пожелания или вы хотите пообщаться с торговым представителем, щелкните "Свяжитесь с отделом продаж" в верхней части страницы "Цены Azure".

## <a name="regional-availability"></a>Доступность по регионам

API-интерфейсы в Cognitive Services размещаются в растущей сети центров обработки данных корпорации Майкрософт. Вы можете найти локализацию для каждого API в [списке регионов Azure](https://azure.microsoft.com/regions).

Ищете регион, который пока не поддерживается? Сообщите нам, отправив запрос функции на наш [форум UserVoice](https://cognitive.uservoice.com/).

## <a name="supported-cultural-languages"></a>Поддерживаемые региональные языки

 Cognitive Services поддерживает широкий спектр региональных языков на уровне службы. Дополнительные сведения о доступности языков для каждого API можно найти в [списке поддерживаемых языков](language-support.md).

## <a name="securing-resources"></a>Защита ресурсов

Службы Azure Cognitive Services предоставляют многоуровневую модель безопасности, включая [проверки подлинности](authentication.md) с помощью учетных данных Azure Active Directory, действительного ключа ресурса и [виртуальных сетей Azure](cognitive-services-virtual-networks.md).

## <a name="container-support"></a>Поддержка контейнеров

 Cognitive Services предоставляет контейнеры для развертывания в облаке Azure или локально. Дополнительные сведения о [контейнерах Cognitive Services](cognitive-services-container-support.md).

## <a name="certifications-and-compliance"></a>Соответствие требованиям и сертификаты

Службы Cognitive Services получили такие сертификаты, как сертификация CSA STAR, FedRAMP Moderate и HIPAA BAA.

Вы можете [скачать](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) сертификаты для собственных аудитов и проверок безопасности.

Чтобы понять, как работает конфиденциальность и управление данными, перейдите в [центр управления безопасностью](https://servicetrust.microsoft.com/).

## <a name="support"></a>Поддержка

Службы Cognitive Services предоставляют несколько [вариантов поддержки](cognitive-services-support-options.md).

## <a name="next-steps"></a>Дальнейшие действия

* [Создание учетной записи Cognitive Services](cognitive-services-apis-create-account.md)
