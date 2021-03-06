---
title: Руководство. Интеграция Azure Active Directory с Perception United States (Non-UltiPro) | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Perception United States (Non-UltiPro).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: e9ba42f780c93486409077383750d0635637e99b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67094838"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Руководство. Интеграция Azure Active Directory с Perception United States (Non-UltiPro)

В этом руководстве вы узнаете, как интегрировать Perception United States (Non-UltiPro) с Azure Active Directory (Azure AD).
Интеграция Perception United States (Non-UltiPro) с Azure AD обеспечивает следующие преимущества:

* С помощью Azure AD вы можете контролировать доступ к Perception United States (Non-UltiPro).
* Вы можете включить автоматический вход пользователей (единый вход) в Perception United States (Non-UltiPro) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с United States (Non-UltiPro) необходимо следующее:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Perception United States (Non-UltiPro) с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Perception United States (Non-UltiPro) поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Добавление Perception United States (Non-UltiPro) из коллекции.

Чтобы настроить интеграцию Perception United States (Non-UltiPro) с Azure AD, необходимо добавить Perception United States (Non-UltiPro) из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Perception United States (Non-UltiPro) из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Perception United States (Non-UltiPro)** , выберите **Perception United States (Non-UltiPro)** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Perception United States (Non-UltiPro) в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Perception United States (Non-UltiPro) с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Perception United States (Non-UltiPro).

Чтобы настроить и проверить единый вход Azure AD в Perception United States (Non-UltiPro), вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Perception United States (Non-UltiPro)](#configure-perception-united-states-non-ultipro-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Perception United States (Non-UltiPro)](#create-perception-united-states-non-ultipro-test-user)** требуется, чтобы в Perception United States (Non-UltiPro) существовал пользователь Britta Simon, связанный с соответствующим пользователем в Azure AD.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure AD в Perception United States (Non-UltiPro), выполните следующие действия.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Perception United States (Non-UltiPro)** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. На странице **Настройка единого входа с помощью SAML** выполните следующие действия.

    ![Сведения о домене и URL-адресах единого входа для приложения Perception United States (Non-UltiPro)](common/idp-intiated.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес: `https://perception.kanjoya.com/sp`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://perception.kanjoya.com/sso?idp=<entity_id>`.

    c. Для приложения **Perception United States (Non-UltiPro)** требуется, чтобы значение **идентификатора Azure AD** (указанное в виде <entity_id>), которое находится в разделе **Настройка Perception United States (Non-UltiPro)** , было закодировано в формат URI. Чтобы получить значение, закодированное в формате URI, перейдите по ссылке **http://www.url-encode-decode.com/** .

    d. Получив это значение, объедините его с **URL-адресом ответа**, как показано ниже:

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    д. Вставьте приведенное выше значение в текстовое поле **URL-адрес ответа**.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать нужный вам **XML метаданных федерации**, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

6. Скопируйте требуемый URL-адрес из раздела **Настройка Perception United States (Non-UltiPro)** .

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Настройка единого входа в Perception United States (Non-UltiPro)

1. В другом окне браузера войдите на корпоративный веб-сайт Perception United States (Non-UltiPro) с правами администратора.

2. На главной панели инструментов щелкните **Account Settings** (Параметры учетной записи).

    ![Пользователь Perception United States (Non-UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. На странице **Account Settings** (Параметры учетной записи) выполните следующие действия:

    ![Пользователь Perception United States (Non-UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. В текстовом поле **Company Name** (Название компании) введите название **компании**.
    
    b. В текстовом поле **Account Name** (Имя учетной записи) введите имя **учетной записи**.

    c. В текстовом поле **Default Reply-To Email** (Электронная почта ответа по умолчанию) введите допустимый адрес **электронной почты**.

    d. Выберите для параметра **SSO Identity Provider** (Поставщик удостоверений единого входа) значение **SAML 2.0**.

4. На странице **настройки единого входа** сделайте следующее:

    ![Настройка единого входа Perception United States (Non-UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Выберите для параметра **SAML NameID Type** (Тип NameID SAML) значение **EMAIL** (Электронная почта).

    b. В текстовом поле **SSO Configuration Name** (Имя конфигурации единого входа) введите имя своей **конфигурации**.
    
    c. В текстовое поле **Identity Provider Name** (Имя поставщика удостоверений) вставьте значение **идентификатора Azure AD**, скопированное на портале Azure. 

    d. В текстовое поле **SAML Domain** (Домен SAML) введите домен, например @contoso.com.

    д. Нажмите кнопку **Upload Again** (Отправить еще раз), чтобы передать **XML-файл метаданных**.

    Е. Нажмите кнопку **Обновить**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите brittasimon@yourcompanydomain.extension. Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив ему доступ к Perception United States (Non-UltiPro).

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Perception United States (Non-UltiPro)** .

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Perception United States (Non-UltiPro)** .

    ![Ссылка на Perception United States (Non-UltiPro) в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Создание тестового пользователя в Perception United States (Non-UltiPro)

В этом разделе описано, как создать пользователя Britta Simon в приложении Perception United States (Non-UltiPro). Обратитесь в [службу поддержки Perception United States (Non-UltiPro)](https://www.ultimatesoftware.com/Contact/ContactUs), чтобы добавить пользователей на платформу Perception United States (Non-UltiPro).

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Perception United States (Non-UltiPro) на Панели доступа, вы автоматически войдете в приложение Perception United States (Non-UltiPro), для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

