---
title: Руководство. Интеграция единого входа Azure Active Directory с SignalFx | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и SignalFx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea81f0046d7f73d845ed49325a3d621e6b7735e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443280"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-signalfx"></a>Руководство. Интеграция единого входа Azure Active Directory с SignalFx

В этом учебнике описано, как интегрировать SignalFx с Azure Active Directory (Azure AD). Интеграция SignalFx с Azure AD обеспечивает следующие возможности.

* С помощью Azure AD вы можете контролировать доступ к SignalFx.
* Вы можете включить автоматический вход пользователей в SignalFx с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>предварительные требования

Чтобы приступить к работе, потребуется следующее:

* подписка Azure AD; Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка SignalFx с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* SignalFx поддерживает единый вход, инициируемый **поставщиком удостоверений**.
* SignalFx поддерживает **JIT**-подготовку пользователей.

## <a name="adding-signalfx-from-the-gallery"></a>Добавление SignalFx из коллекции.

Чтобы настроить интеграцию SignalFx с Azure AD, необходимо добавить SignalFx из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **SignalFx**.
1. Выберите **SignalFx** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.

## <a name="configure-and-test-azure-ad-single-sign-on-for-signalfx"></a>Настройка и проверка единого входа Azure AD для SignalFx

Настройте и проверьте единый вход Azure AD в SignalFx с помощью тестового пользователя **B. Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в SignalFx.

Чтобы настроить и проверить единый вход Azure AD в SignalFx, выполните действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в SignalFx](#configure-signalfx-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя приложения SignalFx](#create-signalfx-test-user)** требуется для того, чтобы в SignalFx существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **SignalFx** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Правка базовой конфигурации SAML](common/edit-urls.png)

1. На странице **Настройка единого входа с помощью SAML** введите значения для следующих полей:

    а. В текстовом поле **Идентификатор** введите URL-адрес: `https://api.signalfx.com/v1/saml/metadata`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://api.signalfx.com/v1/saml/acs/<integration ID>`.

    > [!NOTE]
    > Приведенное выше значение используется только для примера. Это значение следует заменить фактическим URL-адресом ответа, который описывается далее в этом руководстве.

1. Приложение SignalFx ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![image](common/default-attributes.png)

1. В дополнение к описанному выше приложение SignalFx ожидает в ответе SAML несколько дополнительных атрибутов, которые показаны ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.

    | Имя |  Исходный атрибут|
    | ------------------- | -------------------- |
    | User.FirstName  | user.givenname |
    | User.email  | user.mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | user.surname    |

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите пункт **Сертификат (Base64)** и щелкните **Скачать**, чтобы скачать сертификат. Сохраните этот сертификат на компьютере.

    ![Ссылка для скачивания сертификата](common/certificatebase64.png)

1. Требуемые URL-адреса можно скопировать из раздела **Настройка SignalFx**.

    ![Копирование URL-адресов настройки](common/copy-configuration-urls.png)

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

В этом разделе описано, как предоставить пользователю B. Simon доступ к приложению SignalFx, чтобы он мог использовать единый вход Azure.

1. На портале Azure выберите **Корпоративные приложения**, а затем —**Все приложения**.
1. В списке приложений выберите **SignalFx**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать**, расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-signalfx-sso"></a>Настройка единого входа для SignalFx

1. Войдите на свой корпоративный сайт SignalFx как администратор.

1. В верхней части сайта щелкните **Integrations** (Интеграция), чтобы открыть соответствующую страницу.

    ![Интеграция SignalFx](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. Щелкните плитку **Azure Active Directory** в разделе **Login Services** (Службы входа).

    ![SAML SignalFx](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. Щелкните **NEW INTEGRATION** (Новая интеграция) и на вкладке **INSTALL** (Установка) выполните следующие действия:

    ![Страница samlintgpage SignalFx](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    а. В текстовом поле **Name** (Имя) введите имя новой интеграции, например **OurOrgName SAML SSO**.

    b. Скопируйте значение **Integration ID** (Идентификатор интеграции) и, добавив к нему значение **Reply URL** (URL-адрес ответа), замените `<integration ID>` в текстовом поле **URL-адрес ответа** в разделе **Домены и URL-адреса приложения SignalFx** на портале Azure.

    c. Щелкните **Upload File** (Отправить файл), чтобы отправить **сертификат в кодировке Base64**, скачанный с портала Azure, в поле **Certificate** (Сертификат).

    d. В текстовое поле **Issuer URL** (URL-адрес издателя) вставьте значение **Идентификатор Azure AD**, скопированное на портале Azure.

    д) В текстовое поле **Metadata URL** (URL-адрес метаданных) вставьте значение **URL-адрес метаданных**, скопированное на портале Azure.

    е) Выберите команду **Сохранить**.

### <a name="create-signalfx-test-user"></a>Создание тестового пользователя SignalFx

Цель этого раздела — создать пользователя с именем Britta Simon в SignalFx. Приложение SignalFx поддерживает JIT-подготовку. Эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Пользователь будет создан при попытке получить доступ к приложению SignalFx (если он еще не создан).

При первом входе пользователя в SignalFx с использованием единого входа SAML [служба поддержки SignalFx](mailto:kmazzola@signalfx.com) отправит ему по электронной почте ссылку, которую необходимо открыть для проверки подлинности. Это происходит только при первом входе пользователя. При последующих попытках входа проверка по электронной почте не требуется.

> [!Note]
> Чтобы создать пользователя вручную, обратитесь в [службу поддержки SignalFx](mailto:kmazzola@signalfx.com).

## <a name="test-sso"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "SignalFx" на Панели доступа, вы автоматически войдете в приложение SignalFx, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте использовать SignalFx с Azure AD](https://aad.portal.azure.com/)