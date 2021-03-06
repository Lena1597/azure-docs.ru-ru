---
title: Проблемы при входе в приложение Майкрософт | Документы Майкрософт
description: Устранение распространенных проблем, возникающих при входе в приложения Майкрософт с помощью Azure AD (например, Office 365)
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ee8802aeb2a760e255ab4f5e99010dfedc45e0d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108300"
---
# <a name="problems-signing-in-to-a-microsoft-application"></a>Проблемы при входе в приложение Майкрософт

Приложения Майкрософт (такие как Office 365 Exchange, SharePoint, Yammer и т. д.) назначаются и управляются немного иначе, чем приложения SaaS от сторонних разработчиков и другие приложения, которые вы интегрируете с Azure AD для единого входа.

Есть три основных способа, с помощью которых пользователь может получить доступ к приложению, опубликованному корпорацией Майкрософт.

-   Для приложений в Office 365 или других платных наборах доступ пользователям предоставляется посредством **назначения лицензии** — непосредственно для учетной записи или в группе с помощью нашей возможности по групповому назначению лицензий.

-   Для приложений, которые Майкрософт или сторонний разработчик публикуют для свободного использования, пользователи могут получать доступ через **согласие пользователя**. Это означает, что вход в приложение со своей учетной записью Azure AD работы или учебного заведения и предоставляют ему доступ к ограниченному набору данных в своей учетной записью.

-   Для приложений, которые Майкрософт или третья сторона свободно для всех пользователей, пользователи также могут получать доступ через **согласие администратора**. В этом случае администратор решает, что приложение разрешено использовать всем сотрудникам организации, после чего входит в приложение с помощью учетной записи глобального администратора и предоставляет доступ всем пользователям в организации.

Чтобы устранить проблему, приступите к изучению раздела [Общие проблемные области при доступе к приложениям, которые необходимо учитывать](#general-problem-areas-with-application-access-to-consider), а затем получите более подробные сведения в статье о шагах по устранению неполадок при входе в приложение Майкрософт.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Общие проблемные области при доступе к приложениям, которые необходимо учитывать

Ниже указан список общих проблемных областей, которые вы можете изучить более подробно, если будете знать, где именно нужно искать. Однако для быстрого начала работы мы рекомендуем прочитать статью Пошаговое руководство. Инструкции по устранению неполадок при доступе к приложениям Майкрософт.

-   [Проблемы с учетной записью пользователя](#problems-with-the-users-account)

-   [Проблемы с группами](#problems-with-groups)

-   [Проблемы с политиками условного доступа](#problems-with-conditional-access-policies)

-   [Проблемы с согласием для приложений](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Инструкции по устранению неполадок при доступе к приложениям Майкрософт

Ниже приведены некоторые распространенные проблемы, с которыми сталкиваются специалисты, когда пользователям не удается войти в приложение Майкрософт.

- Общие проблемы для первоначальной проверки

  * Убедитесь, что пользователь выполняет вход с **правильным URL-адресом**, а не локальным URL-адресом приложения.

  * Убедитесь, что учетная запись пользователя **не заблокирована**.

  * Убедитесь, что **учетная запись пользователя существует** в Azure Active Directory. [Проверка существования учетной записи пользователя в Azure Active Directory](#problems-with-the-users-account)

  * Проверьте, **включена** ли учетная запись пользователя для входа. [Проверка состояния учетной записи пользователя](#problems-with-the-users-account)

  * Удостоверьтесь в том, что **пароль действителен и пользователь не забыл его**. [Сброс пароля пользователя](#reset-a-users-password) или [Включение самостоятельного сброса пароля](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

  * Проверьте, не блокирует ли **Многофакторная идентификация** доступ пользователей. [Проверка состояния службы Многофакторной идентификации](#check-a-users-multi-factor-authentication-status) или [Проверка контактной информации для проверки подлинности](#check-a-users-authentication-contact-info)

  * Убедитесь, что **политика условного доступа** или политика **защиты идентификации** не блокирует доступ пользователей. [Проверка определенной политики условного доступа](#problems-with-conditional-access-policies) или [проверка политики условного доступа для конкретного приложения](#check-a-specific-applications-conditional-access-policy) или [отключить конкретную политику условного доступа](#disable-a-specific-conditional-access-policy)

  * Убедитесь, что **контактная информация для проверки подлинности** актуальна, чтобы разрешить применение политик Многофакторной идентификации или условного доступа. [Проверка состояния службы Многофакторной идентификации](#check-a-users-multi-factor-authentication-status) или [Проверка контактной информации для проверки подлинности](#check-a-users-authentication-contact-info)

- Для приложений **Майкрософт**, **которым требуется лицензия** (например, Office 365) ниже приведены характерные проблемы, которым нужно уделить внимание после обработки описанных выше общих проблем:

  * Убедитесь, что пользователю **назначена лицензия**. [Проверка назначенных пользователю лицензий](#check-a-users-assigned-licenses) или [Проверка назначенных группе лицензий](#check-a-groups-assigned-licenses)

  * Если лицензия **назначена** **статической группе**, убедитесь, что **пользователь является членом** этой группы. [Проверка членства пользователя в группах](#check-a-users-group-memberships)

  * Если лицензия **назначена** **динамической группе**, убедитесь, что **правило динамической группы задано правильно**. [Проверка критериев членства для динамической группы](#check-a-dynamic-groups-membership-criteria)

  * Если лицензия **назначена** **динамической группе**, убедитесь, что эта группа **завершила обработку** членства и **пользователь является членом** (это может занять некоторое время). [Проверка членства пользователя в группах](#check-a-users-group-memberships)

  *  Подтвердив назначение лицензии, убедитесь, что ее срок действия **не истек**.

  *  Убедитесь, что лицензия предназначена именно для **приложения**, к которому обращаются пользователи.

- Для приложений **Майкрософт**, **которым лицензия не нужна**, можно проверить следующее:

  * Если приложение запрашивает **разрешения уровня пользователя** (например «доступ к почтовому ящику этого пользователя»), убедитесь, что пользователь вошел в приложение и выполнил **операцию согласия на уровне пользователя** чтобы предоставить приложению доступ к данным.

  * Если приложение запрашивает **разрешения уровня администратора** (например, "доступ к почтовым ящикам всех пользователей"), убедитесь, что глобальный администратор выполнил **операцию согласия на уровне администратора от лица всех пользователей** в организации.

## <a name="problems-with-the-users-account"></a>Проблемы с учетной записью пользователя

Доступ к приложению может быть заблокирован из-за проблемы с пользователем, назначенным для приложения. Ниже приведены некоторые способы устранения неполадок, связанных с параметрами учетной записи пользователя.

-   [Проверка существования учетной записи пользователя в Azure Active Directory](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Проверка состояния учетной записи пользователя](#check-a-users-account-status)

-   [Сброс пароля пользователя](#reset-a-users-password)

-   [Включение самостоятельного сброса пароля](#enable-self-service-password-reset)

-   [Проверка состояния службы Многофакторной Идентификации](#check-a-users-multi-factor-authentication-status)

-   [Проверка контактной информации для проверки подлинности](#check-a-users-authentication-contact-info)

-   [Проверка членства пользователя в группах](#check-a-users-group-memberships)

-   [Проверка назначенных пользователю лицензий](#check-a-users-assigned-licenses)

-   [Назначение лицензии пользователю](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Проверка существования учетной записи пользователя в Azure Active Directory

Чтобы проверить, существует ли учетная запись пользователя, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Проверьте свойства объекта пользователя, чтобы убедиться, что они выглядят должным образом и что данные не отсутствуют.

### <a name="check-a-users-account-status"></a>Проверка состояния учетной записи пользователя

Чтобы проверить состояние учетной записи пользователя, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Щелкните **Профиль**.

8.  Убедитесь, что в разделе **Параметры** для параметра **Заблокировать вход** задано значение **Нет**.

### <a name="reset-a-users-password"></a>Сброс пароля пользователя

Чтобы сбросить пароль пользователя, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Нажмите кнопку **Сброс пароля** в верхней части области пользователя.

8.  В открывшейся области **Сброс пароля** нажмите кнопку **Сброс пароля**.

9.  Скопируйте **временный пароль** или **введите новый пароль** для пользователя.

10. Сообщите этот новый пароль пользователю, так как ему потребуется изменить пароль при следующем входе в Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Включение самостоятельного сброса пароля

Чтобы включить самостоятельный сброс пароля, следуйте указанным ниже разделам по развертыванию.

-   [Разрешение пользователям сбрасывать пароли Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Разрешение пользователям сбрасывать или изменять пароли AD](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Проверка состояния службы Многофакторной Идентификации

Чтобы проверить состояние службы Многофакторной идентификации, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. В меню навигации выберите пункт **Пользователи и группы**.

5. Щелкните **Все пользователи**.

6. Нажмите кнопку **Многофакторная идентификация** в верхней части области.

7. После загрузки **портала администрирования Многофакторной идентификации** перейдите на вкладку **Пользователи**.

8. Найдите пользователя в списке пользователей, выполнив поиск, фильтрацию или сортировку.

9. Выберите его из списка пользователей и по своему усмотрению **включите**, **отключите** или **примените** Многофакторную Идентификацию.

   * **Примечание**. Если пользователь находится в состоянии **Принудительно**, можно временно задать состояние **Отключено**, чтобы разрешить ему вернуться в свою учетную запись. После этого можно снова изменить состояние на **Включено**, чтобы при следующем входе пользователь повторно зарегистрировал контактные данные. Кроме того, можно выполнить действия, описанные в разделе [Проверка контактной информации для проверки подлинности](#check-a-users-authentication-contact-info), чтобы проверить или задать эти данные для пользователя.

### <a name="check-a-users-authentication-contact-info"></a>Проверка контактной информации для проверки подлинности

Чтобы проверить контактную информацию для проверки подлинности, используемую для Многофакторной идентификации, условного доступа, защиты идентификации и сброса пароля, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Щелкните **Профиль**.

8.  Прокрутите вниз до раздела **Контактная информация для проверки подлинности**.

9.  **Просмотрите** данные, зарегистрированные для пользователя, и обновите их при необходимости.

### <a name="check-a-users-group-memberships"></a>Проверка членства пользователя в группах

Чтобы проверить членство пользователя в группах, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Щелкните **Группы**, чтобы узнать, в какие группы входит пользователь.

### <a name="check-a-users-assigned-licenses"></a>Проверка назначенных пользователю лицензий

Чтобы проверить назначенные пользователю лицензии, выполните следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Чтобы просмотреть лицензии, которые в настоящее время назначены пользователю, щелкните **Лицензии**.

### <a name="assign-a-user-a-license"></a>Назначение лицензии пользователю 

Чтобы назначить лицензию пользователю, выполните следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все пользователи**.

6.  **Найдите** нужного пользователя и выберите его, **щелкнув соответствующую строку**.

7.  Чтобы просмотреть лицензии, которые в настоящее время назначены пользователю, щелкните **Лицензии**.

8.  Нажмите кнопку **Назначить**.

9.  Выберите **один или несколько продуктов** в списке доступных продуктов.

10. **Необязательно:** чтобы назначить отдельные продукты, щелкните элемент **Параметры назначения**. По завершении нажмите кнопку **ОК**.

11. Чтобы назначить выбранные лицензии пользователю, нажмите кнопку **Назначить**.

## <a name="problems-with-groups"></a>Проблемы с группами

Доступ к приложению может быть заблокирован из-за проблемы с группой, назначенной для приложения. Ниже приведены некоторые способы для устранения неполадок, связанных с группами и членством в них.

-   [Проверка членства в группе](#check-a-groups-membership)

-   [Проверка критериев членства для динамической группы](#check-a-dynamic-groups-membership-criteria)

-   [Проверка назначенных группе лицензий](#check-a-groups-assigned-licenses)

-   [Повторная обработка лицензий группы](#reprocess-a-groups-licenses)

-   [Назначение лицензии группе](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Проверка членства в группе

Чтобы проверить членство в группе, выполните следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все группы**.

6.  **Найдите** нужную группу и выберите ее, **щелкнув соответствующую строку**.

7.  Щелкните **Члены**, чтобы просмотреть список пользователей, назначенных этой группе.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Проверка критериев членства для динамической группы 

Чтобы проверить критерии членства в динамической группе, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор.**

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все группы**.

6.  **Найдите** нужную группу и выберите ее, **щелкнув соответствующую строку**.

7.  Щелкните **Правила динамического членства**.

8.  Просмотрите **простое** или **расширенное** правило, определенное для этой группы, и убедитесь, что пользователь, который должен стать ее членом, соответствует данным критериям.

### <a name="check-a-groups-assigned-licenses"></a>Проверка назначенных группе лицензий

Чтобы проверить назначенные группе лицензии, сделайте следующее:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор.**

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  В меню навигации выберите пункт **Пользователи и группы**.

5.  Щелкните **Все группы**.

6.  **Найдите** нужную группу и выберите ее, **щелкнув соответствующую строку**.

7.  Чтобы просмотреть лицензии, которые в настоящее время назначены группе, щелкните **Лицензии**.

### <a name="reprocess-a-groups-licenses"></a>Повторная обработка лицензий группы

Чтобы повторно обработать назначенные группе лицензии, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор.**

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. В меню навигации выберите пункт **Пользователи и группы**.

5. Щелкните **Все группы**.

6. **Найдите** нужную группу и выберите ее, **щелкнув соответствующую строку**.

7. Чтобы просмотреть лицензии, которые в настоящее время назначены группе, щелкните **Лицензии**.

8. Нажмите кнопку **Повторно обработать**, чтобы обеспечить актуальность лицензий, назначенных членам этой группы. Это действие может занять много времени в зависимости от размера и сложности группы.

   >[!NOTE]
   >Чтобы ускорить процесс, можно временно назначить лицензию пользователю напрямую. [Назначение лицензии пользователю](#problems-with-application-consent)
   >
   >

### <a name="assign-a-group-a-license"></a>Назначение лицензии группе

Чтобы назначить лицензию группе, сделайте следующее:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор.**

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. В меню навигации выберите пункт **Пользователи и группы**.

5. Щелкните **Все группы**.

6. **Найдите** нужную группу и выберите ее, **щелкнув соответствующую строку**.

7. Чтобы просмотреть лицензии, которые в настоящее время назначены группе, щелкните **Лицензии**.

8. Нажмите кнопку **Назначить**.

9. Выберите **один или несколько продуктов** в списке доступных продуктов.

10. **Необязательно:** чтобы назначить отдельные продукты, щелкните элемент **Параметры назначения**. По завершении нажмите кнопку **ОК**.

11. Чтобы назначить выбранные лицензии группе, нажмите кнопку **Назначить**. Это действие может занять много времени в зависимости от размера и сложности группы.

    >[!NOTE]
    >Чтобы ускорить процесс, можно временно назначить лицензию пользователю напрямую. [Назначение лицензии пользователю](#problems-with-application-consent)
    > 
    >

## <a name="problems-with-conditional-access-policies"></a>Проблемы с политиками условного доступа

### <a name="check-a-specific-conditional-access-policy"></a>Проверка определенной политики условного доступа

Для проверки или проверить одну политику условного доступа:

1. Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2. Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3. В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4. Щелкните пункт **Корпоративные приложения** в меню навигации.

5. Нажмите кнопку **условного доступа** элемент навигации.

6. Щелкните политику, которую хотите проверить.

7. Проверка, что нет никаких условий, назначений или другие параметры, которые могут блокировать доступ пользователя.

   >[!NOTE]
   >Вы можете временно отключить эту политику, чтобы убедиться, что она не влияет на операции входа. Для этого установите переключатель **Включить политику** в положение **Нет** и нажмите кнопку **Сохранить**.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Проверка политики условного доступа для конкретного приложения

Проверка одного приложения в настоящее время настроить политику условного доступа:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  Щелкните пункт **Корпоративные приложения** в меню навигации.

5.  Щелкните **Все приложения**.

6.  Найдите приложение, которое вы заинтересованы в приложение или пользователь пытается войти в приложение отобразите имя или приложения.

     >[!NOTE]
     >Если вы не находите приложение, нажмите кнопку **Фильтр** и разверните область до значения **Все приложения**. Если вы хотите отобразить больше столбцов, нажмите кнопку **Столбцы**, чтобы добавить дополнительные сведения для приложений.
     >
     >

7.  Нажмите кнопку **условного доступа** элемент навигации.

8.  Щелкните политику, которую хотите проверить.

9.  Убедитесь, что нет никаких условий, назначений или других параметров, которые могут блокировать доступ пользователей.

     >[!NOTE]
     >Вы можете временно отключить эту политику, чтобы убедиться, что она не влияет на операции входа. Для этого установите переключатель **Включить политику** в положение **Нет** и нажмите кнопку **Сохранить**.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Отключить конкретную политику условного доступа

Для проверки или проверить одну политику условного доступа:

1.  Откройте [**портал Azure**](https://portal.azure.com/) и войдите в систему как **глобальный администратор**.

2.  Откройте **расширение Azure Active Directory**, щелкнув **Все службы** в верхней части главного меню навигации слева.

3.  В поле фильтра поиска введите **Azure Active Directory** и выберите элемент **Azure Active Directory**.

4.  Щелкните пункт **Корпоративные приложения** в меню навигации.

5.  Нажмите кнопку **условного доступа** элемент навигации.

6.  Щелкните политику, которую хотите проверить.

7.  Отключите политику, установив переключатель **Включить политику** в положение **Нет**, и нажмите кнопку **Сохранить**.

## <a name="problems-with-application-consent"></a>Проблемы с согласием для приложений

Доступ к приложению может быть заблокирован из-за того, что не выполнена должная операция предоставления согласия. Ниже приведены некоторые способы для устранения неполадок, связанных с согласием для приложений.

-   [Операция разрешения на уровня пользователя](#perform-a-user-level-consent-operation)

-   [Операция разрешения на уровне администратора для любого приложения](#perform-administrator-level-consent-operation-for-any-application)

-   [Операция разрешения на уровне администратора для приложения с одним клиентом](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Операция разрешения на уровне администратора для мультитенантного приложения](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Операция разрешения на уровня пользователя

-   Для любого приложения с включенным Open ID Connect, которое запрашивает разрешение, переход на экран входа обеспечивает согласие на предоставление приложению прав для пользователя, выполнившего вход.

-   Если вы хотите сделать это программными средствами, см. статью [Запрос на получение согласия одного пользователя](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Операция разрешения на уровне администратора для любого приложения

-   **Только для приложений, разработанных с помощью модели приложения версии 1**, вы можете принудительно реализовать согласие на уровне администратора, добавив " **?prompt=admin\_consent**" в конец URL-адреса входа для приложения.

-   Для **любых приложений, разработанных с помощью модели приложения версии 2**, вы можете принудительно реализовать согласие на уровне администратора, выполнив инструкции из раздела **Запрос разрешений у администратора каталога** статьи [Использование конечной точки предоставления согласия администратора](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Операция разрешения на уровне администратора для приложения с одним клиентом

-   Для **приложений с одним клиентом**, запрашивающих разрешения (как те, которые вы разрабатываете или используете в своей организации), можно выполнить операцию **разрешения на уровне администратора** от имени всех пользователей. Для этого выполните вход в качестве глобального администратора и нажмите кнопку **Предоставить разрешения** в верхней части области **Приложение реестра -&gt; Все приложения -&gt; Выберите приложение -&gt; Необходимые разрешения**.

-   Для **любых приложений, разработанных с помощью модели приложения версии 1 или 2**, вы можете принудительно реализовать согласие на уровне администратора, выполнив инструкции из раздела **Запрос разрешений у администратора каталога** статьи [Использование конечной точки предоставления согласия администратора](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Операция разрешения на уровне администратора для мультитенантного приложения

-   Для **мультитенантных приложений**, запрашивающих разрешения (например, приложения, которое разрабатывают сторонние производители или корпорация Майкрософт), можно выполнить операцию **согласия на уровне администратора**. Войдите в качестве глобального администратора и нажмите кнопку **Предоставить разрешения** в области **Корпоративные приложения -&gt; Все приложения -&gt; Выберите приложение -&gt; Разрешения** (будет доступна в ближайшее время).

-   Вы также можете принудительно реализовать согласие на уровне администратора, выполнив инструкции из раздела **Запрос разрешений у администратора каталога** статьи [Использование конечной точки предоставления согласия администратора](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Дальнейшие действия
[Использование конечной точки предоставления согласия администратора](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

