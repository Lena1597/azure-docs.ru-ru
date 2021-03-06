---
title: Руководство по Интеграция Azure Active Directory с Origami | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Origami.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: fd347f4eb5f77dacc3c9fd61d0e885e9b3ee7959
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67095636"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Руководство по Интеграция Azure Active Directory с Origami

В этом учебнике описано, как интегрировать Origami с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением Origami обеспечивает следующие преимущества.

* С помощью Azure Active Directory вы можете контролировать доступ к Origami.
* Вы можете включить автоматический вход пользователей в Origami (единый вход) с помощью учетных записей Azure Active Directory.
* Вы можете управлять учетными записями централизованно на портале Azure.

Дополнительные сведения об интеграции приложений SaaS с Azure AD см. в статье [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Origami, вам потребуется:

* подписка Azure AD (если у вас нет среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/));
* подписка Origami с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Origami поддерживает единый вход, инициированный **поставщиком услуг**.

## <a name="adding-origami-from-the-gallery"></a>Добавление Origami из коллекции

Чтобы настроить интеграцию Origami с Azure AD, необходимо добавить Origami из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Origami из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Origami**, выберите **Origami** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![Origami в списке результатов](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure Active Directory в Origami с использованием тестового пользователя **Britta Simon**.
Для обеспечения работы единого входа необходимо установить связь между пользователем Azure Active Directory и соответствующим пользователем в Origami.

Чтобы настроить и проверить единый вход Azure AD в Origami, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Настройка единого входа в Origami](#configure-origami-single-sign-on)** необходима, чтобы настроить параметры единого входа на стороне приложения.
3. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
5. **[Создание тестового пользователя Origami](#create-origami-test-user)** требуется для того, чтобы в приложении Origami существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure Active Directory.
6. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы проверить работу конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано включение единого входа Azure AD на портале Azure.

Чтобы настроить единый вход Azure Active Directory в Origami, сделайте следующее.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Origami** выберите **Единый вход**.

    ![Ссылка "Настройка единого входа"](common/select-sso.png)

2. В диалоговом окне **Выбрать метод единого входа** выберите режим **SAML/WS-Fed**, чтобы включить единый вход.

    ![Режим выбора единого входа](common/select-saml-option.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения Origami](common/sp-signonurl.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://live.origamirisk.com/origami/account/login?account=<companyname>`.

    > [!NOTE]
    > Это значение приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов Origami](https://wordpress.org/support/theme/origami). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

5. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** щелкните **Загрузить**, чтобы загрузить требуемый **сертификат (Base64)** из предложенных вариантов, и сохраните его на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

6. Требуемый URL-адрес можно скопировать из раздела **Настройка Origami**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

    а) URL-адрес входа.

    b. Идентификатор Azure AD

    c. URL-адрес выхода.

### <a name="configure-origami-single-sign-on"></a>Настройка единого входа в Origami

1. Войдите в систему, используя учетную запись Origami с правами администратора.

2. В верхнем меню щелкните **Администратор**.
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_51.png)

3. На странице диалогового окна "Настройка единого входа" сделайте следующее.
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_531.png)

    a. Выберите пункт **Включить единый вход**.

    b. В текстовое поле **Identity Provider's Sign-in Page URL** (URL-адрес страницы входа поставщика удостоверений) вставьте значение **URL-адреса входа**, скопированное на портале Azure.

    c. В текстовое поле **Identity Provider's Sign-in Page URL** (URL-адрес страницы выхода поставщика удостоверений) вставьте значение **URL-адреса выхода**, скопированное на портале Azure.

    d. Нажмите кнопку **Browse** (Обзор), чтобы отправить файл метаданных, скачанный с портала Azure.

    д. Нажмите кнопку **Сохранить изменения**.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](common/users.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![Кнопка "Новый пользователь"](common/new-user.png)

3. В разделе свойств пользователя сделайте следующее:

    ![Диалоговое окно "Пользователь"](common/user-properties.png)

    а. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension** .  
    Например BrittaSimon@contoso.com.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure, предоставив ему доступ к Origami.

1. На портале Azure выберите **Корпоративные приложения**, **Все приложения**, а затем — **Origami**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Origami**.

    ![Ссылка на Origami в списке "Приложения"](common/all-applications.png)

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

4. Нажмите кнопку **Добавить пользователя**, а затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"](common/add-assign-user.png)

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор ролей** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="create-origami-test-user"></a>Создание тестового пользователя Origami

В этом разделе описано, как создать пользователя Britta Simon в приложении Origami. 

1. Войдите в систему, используя учетную запись Origami с правами администратора.

2. В верхнем меню щелкните **Администратор**.
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_51.png)

3. В диалоговом окне **Users and Security** (Пользователи и безопасность) нажмите кнопку **Users** (Пользователи).
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_54.png)

4. Нажмите кнопку **Add New User**(Добавить нового пользователя).
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_55.png)

5. В диалоговом окне добавления нового пользователя выполните следующие действия.
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_56.png)

    a. В текстовом поле **Имя пользователя** введите адрес электронной почты пользователя, например **brittasimon\@contoso.com**.

    b. В текстовом поле **Пароль** введите пароль.

    c. В текстовом поле **подтверждения пароля** введите пароль еще раз.

    d. В текстовом поле **First Name** (Имя) введите имя, допустим, **Britta**.

    д. В текстовом поле **Last Name** (Фамилия) введите фамилию, предположим, **Simon**.

    Е. Выберите команду **Сохранить**.
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_57.png)

6. Назначьте **роли пользователя** и уровень **клиентского доступа** для пользователя. 
   
    ![Настройка единого входа](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="test-single-sign-on"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Origami на Панели доступа, вы автоматически войдете в приложение Origami, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что такое условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

