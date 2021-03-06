---
title: Параметры файла cookie прокси приложения (Azure Active Directory) | Документация Майкрософт
description: Azure Active Directory (Azure AD) имеет файлы cookie доступа и сеанса для доступа к локальным приложениям через прокси-сервер приложения. В этой статье вы узнаете, как использовать и настраивать параметры файлов cookie.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7287e32fbeff751bddf91bed32afeeae84f9378c
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014518"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Параметры файлов cookie для доступа к локальным приложениям в Azure Active Directory

Azure Active Directory (Azure AD) имеет файлы cookie доступа и сеанса для доступа к локальным приложениям через прокси-сервер приложения. Узнайте, как использовать параметры файлов cookie прокси приложения. 

## <a name="what-are-the-cookie-settings"></a>Что такое параметры файлов cookie

[Прокси приложения](application-proxy.md) использует следующие параметры файлов cookie доступа и сеанса.

| Параметр файла cookie | значение по умолчанию | ОПИСАНИЕ | Рекомендации |
| -------------- | ------- | ----------- | --------------- |
| Использовать файл cookie, предназначенный только для HTTP | **Нет** | **Да**. Позволяет прокси приложения включить флаг HTTPOnly в заголовки ответа HTTP. Этот флаг обеспечивает дополнительные преимущества для безопасности, например, это препятствует клиентскому сценарию (CSS) копировать или изменять файлы cookie.<br></br><br></br>Перед реализацией поддержки параметра "Только HTTP" прокси приложения зашифровал и передал файлы cookie через защищенный канал SSL, чтобы защититься от изменения. | Выберите **Да**, чтобы получить дополнительные преимущества безопасности.<br></br><br></br>Выберите **Нет** для клиентов или агентов пользователей, которым требуется доступ к файлам cookie сеанса. Например, в клиенте для RDP или MTSC, который подключается к серверу шлюза удаленных рабочих столов через прокси приложения, выберите **Нет**.|
| Использование безопасного файла cookie | **Нет** | **Да**. Позволяет прокси приложения включить флаг безопасности в заголовки ответа HTTP. Защищенный файл cookie повышает безопасность передачи по защищенному каналу TLS, такому как HTTPS. Это предотвращает несанкционированное отслеживание файлов cookie сторонними лицами из-за передачи файла cookie в виде обычного текста. | Выберите **Да**, чтобы получить дополнительные преимущества безопасности.|
| Использование постоянных файлов cookie | **Нет** | **Да**. Позволяет прокси приложения настроить действие файлов cookie доступа после закрытия веб-браузера. Сохраняемость длится до окончания срока действия маркера доступа, или пока пользователь вручную не удалит постоянные файлы cookie. | Выберите **Нет** из-за угрозы безопасности, связанной с сохранением аутентификации пользователей.<br></br><br></br>Мы рекомендуем использовать **Да** только для более старых приложений, которые не могут совместно использовать файлы cookie между процессами. Мы советуем обновить приложение, чтобы совместно использовать файлы cookie между процессами, а не применять постоянные файлы cookie. Например, вам могут потребоваться постоянные файлы cookie, чтобы разрешить открывать документы Office в представлении проводника с сайта SharePoint. Без постоянных файлов cookie эта операция может завершиться ошибкой, если в браузере, процессе проводника и процессе Office общий доступ к файлам cookie не предоставляется. |

## <a name="samesite-cookies"></a>Файлы cookie SameSite
Начиная с версии Chrome 80 и в конечном итоге в браузерах, использующих Chromium, файлы cookie, не указывающие атрибут [SameSite](https://web.dev/samesite-cookies-explained) , будут рассматриваться так, как если бы они были заданы как **SameSite = нестрогие**. Атрибут SameSite объявляет, как файлы cookie должны быть ограничены контекстом одного узла. Если задано значение "нестрогий", файл cookie будет отправляться только в запросы на один сайт или навигацию верхнего уровня. Однако прокси приложения требует, чтобы эти файлы cookie сохранялись в контексте стороннего разработчика, чтобы пользователи могли правильно войти в систему во время сеанса. Из-за этого мы обновляем файлы cookie для доступа и сеанса прокси приложения, чтобы избежать негативного воздействия этого изменения. К этим обновлениям относятся:

* Установка для атрибута **SameSite** значения **None**. Это позволяет использовать прокси приложения для правильной отправки файлов cookie в контексте стороннего разработчика.
* Установка параметра **использовать защищенный файл cookie** для использования по умолчанию **Yes** . Chrome также требует, чтобы файлы cookie указали флаг Secure или были отклонены. Это изменение будет применено ко всем существующим приложениям, опубликованным через прокси приложения. Обратите внимание, что файлы cookie для доступа к прокси приложения всегда были настроены на безопасность и передаются только по протоколу HTTPS. Это изменение будет применено только к файлам cookie сеанса.

Эти изменения в файлах cookie прокси приложения будут выдвигаться в течение следующих нескольких недель до даты выпуска Chrome 80.

Кроме того, если серверное приложение имеет файлы cookie, которые должны быть доступны в контексте стороннего разработчика, необходимо явно указать, изменив приложение на использование SameSite = None для этих файлов cookie. Прокси приложения преобразует заголовок Set-cookie в его URL-адреса и будет учитывать параметры этих файлов cookie, заданные приложением серверной части.



## <a name="set-the-cookie-settings---azure-portal"></a>Установка параметров файлов cookie — портал Azure
Чтобы задать параметры файла cookie с помощью портала Azure:

1. Войдите на [портал Azure](https://portal.azure.com). 
2. Выберите **Azure Active Directory** > **Корпоративные приложения** > **Все приложения**.
3. Выберите приложение, для которого необходимо включить параметр файла cookie.
4. Выберите **Прокси приложения**.
5. В разделе **Дополнительные параметры** установите параметр файла cookie как **Да** или **Нет**.
6. Щелкните **Сохранить**, чтобы применить изменения. 

## <a name="view-current-cookie-settings---powershell"></a>Просмотр текущих параметров файлов cookie — PowerShell

Чтобы просмотреть текущие параметры файлов cookie для приложения, используйте следующую команду PowerShell:  

```powershell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>Настройка параметров файлов cookie — PowerShell

В следующих командах PowerShell ```<ObjectId>``` является ObjectId приложения. 

**Файл cookie только для HTTP** 

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Защита файлов cookie**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Постоянные файлы cookie**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
