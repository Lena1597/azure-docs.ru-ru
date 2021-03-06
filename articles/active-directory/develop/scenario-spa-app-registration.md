---
title: Регистрация одностраничных приложений — платформа Microsoft Identity | Службы
description: Узнайте, как создать одностраничное приложение (регистрация приложения)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 5b18b10748e0587920c6965f1d235376da928469
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701898"
---
# <a name="single-page-application-app-registration"></a>Одностраничное приложение: регистрация приложения

На этой странице объясняются особенности регистрации приложений для одностраничного приложения (SPA).

Выполните действия по [регистрации нового приложения на платформе удостоверений Майкрософт](quickstart-register-app.md)и выберите поддерживаемые учетные записи для приложения. Сценарий SPA может поддерживать проверку подлинности с учетными записями в организации или в любой организации и личных учетных записях Майкрософт.

Далее вы узнаете о конкретных аспектах регистрации приложений, которые относятся к одностраничным приложениям.

## <a name="register-a-redirect-uri"></a>Регистрация URI перенаправления

Неявный поток отправляет маркеры в перенаправление в одностраничное приложение, выполняющееся в веб-браузере. Поэтому важно зарегистрировать URI перенаправления, в котором приложение может получить токены. Убедитесь, что URI перенаправления точно соответствует URI для вашего приложения.

В [портал Azure](https://go.microsoft.com/fwlink/?linkid=2083908)перейдите к зарегистрированному приложению. На странице **Проверка подлинности** приложения выберите **веб-** платформу. Введите значение URI перенаправления для приложения в поле **URI перенаправления** .

## <a name="enable-the-implicit-flow"></a>Включение неявного потока

На той же странице **аутентификации** в разделе **Дополнительные параметры**необходимо также включить **неявное разрешение GRANT**. Если приложение входит только в пользователей и получает маркеры идентификации, достаточно установить флажок **маркеры идентификации** .

Если приложению также необходимо получить маркеры доступа для вызова API, убедитесь, что флажок **маркеры доступа** также установлен. Дополнительные сведения см. в разделе [маркеры идентификации](./id-tokens.md) и [маркеры доступа](./access-tokens.md).

## <a name="api-permissions"></a>Разрешения API

Одностраничные приложения могут вызывать API от имени пользователя, выполнившего вход. Они должны запрашивать делегированные разрешения. Дополнительные сведения см. [в разделе Добавление разрешений для доступа к веб-API](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Конфигурация кода приложения](scenario-spa-app-configuration.md)
