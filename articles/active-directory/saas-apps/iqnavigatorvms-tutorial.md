---
title: Руководство. Интеграция единого входа Azure Active Directory с IQNavigator VMS | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в IQNavigator VMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f61681fbe3ec93467db67290fde6548ce62425fc
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "75560536"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-iqnavigator-vms"></a>Руководство. Интеграция единого входа Azure Active Directory с IQNavigator VMS

В этом учебнике вы узнаете, как интегрировать IQNavigator VMS с Azure Active Directory (Azure AD). Интеграция IQNavigator VMS с Azure AD обеспечивает следующие возможности:

* Управление доступом к IQNavigator VMS в Azure AD.
* Автоматический вход пользователей в IQNavigator VMS с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>предварительные требования

Чтобы приступить к работе, потребуется следующее:

* подписка Azure AD; Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка IQNavigator VMS с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.


* В IQNavigator VMS поддерживается единый вход, инициируемый **поставщиком удостоверений**.

> [!NOTE]
> Идентификатор этого приложения — фиксированное строковое значение, поэтому в одном клиенте можно настроить только один экземпляр.



## <a name="adding-iqnavigator-vms-from-the-gallery"></a>Добавление IQNavigator VMS из коллекции

Чтобы настроить интеграцию IQNavigator VMS с Azure AD, необходимо добавить IQNavigator VMS из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе**Добавление из коллекции** в поле поиска введите **IQNavigator VMS**.
1. Выберите **IQNavigator VMS** в области результатов, а затем добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-iqnavigator-vms"></a>Настройка и проверка единого входа Azure AD для IQNavigator VMS

Настройте и проверьте единый вход Azure AD в IQNavigator VMS с помощью тестового пользователя с именем **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в IQNavigator VMS.

Чтобы настроить и проверить единый вход Azure AD в IQNavigator VMS, выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в IQNavigator VMS](#configure-iqnavigator-vms-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя в IQNavigator VMS](#create-iqnavigator-vms-test-user)** требуется для того, чтобы в IQNavigator VMS существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **IQNavigator VMS** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Правка базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес: `iqn.com`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`.

    c. Щелкните **Задать дополнительные URL-адреса**.

    d. В текстовом поле **Состояние ретранслятора** введите URL-адрес в формате `https://<subdomain>.iqnavigator.com`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими URL-адресом ответа и значением состояния ретранслятора. Чтобы получить эти значения, обратитесь к [группе поддержки IQNavigator VMS](https://www.beeline.com/support-iqn/). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение IQNavigator ожидает в утверждении идентификатора имени значение уникального идентификатора пользователя. Клиент может сопоставить правильное значение для утверждения идентификатора имени. В данном случае для примера мы сопоставили user.UserPrincipalName. Однако вам нужно сопоставить правильное значение, соответствующее параметрам вашей организации.

    ![image](common/edit-attribute.png)

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю B. Simon использовать единый вход Azure, предоставив этому пользователю доступ к IQNavigator VMS.

1. На портале Azure выберите **Корпоративные приложения**, а затем —**Все приложения**.
1. Из списка приложений выберите **IQNavigator VMS**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-iqnavigator-vms-sso"></a>Настройка единого входа для IQNavigator VMS

Чтобы настроить единый вход на стороне **IQNavigator VMS**, необходимо отправить **URL-адрес метаданных федерации приложения** в [группу поддержки IQNavigator VMS](https://www.beeline.com/support-iqn/). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-iqnavigator-vms-test-user"></a>Создание тестового пользователя IQNavigator VMS

В этом разделе описано, как создать пользователя Britta Simon в приложении IQNavigator VMS. Обратитесь к  [группе поддержки IQNavigator VMS](https://www.beeline.com/support-iqn/), чтобы добавить пользователей на платформу IQNavigator VMS. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку IQNavigator VMS на панели доступа, вы автоматически войдете в приложение IQNavigator VMS, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать IQNavigator VMS с Azure AD](https://aad.portal.azure.com/)

