---
title: Руководство по Интеграция единого входа Azure Active Directory с Alibaba Cloud Service (единый вход на основе ролей) | Документация Майкрософт
description: Из этой статьи вы узнаете, как настроить единый вход Azure Active Directory в Alibaba Cloud Service (единый вход на основе ролей).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3667841e-acfc-4490-acf5-80d9ca3e71e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99b9173c817cc3ecf4b9a34ec6906af0b4de70e6
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71120770"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-alibaba-cloud-service-role-based-sso"></a>Руководство по Интеграция единого входа Azure Active Directory с Alibaba Cloud Service (единый вход на основе ролей)

В этом руководстве описано, как интегрировать Alibaba Cloud Service (единый вход на основе ролей) с Azure Active Directory (Azure AD). Интеграция Azure Active Directory с Alibaba Cloud Service (единый вход на основе ролей) предоставляет следующие возможности:

* Управление доступом к Alibaba Cloud Service (единый вход на основе ролей) с помощью Azure AD.
* Включение автоматического входа пользователей в Alibaba Cloud Service (единый вход на основе ролей) с использованием учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее:

* подписка Azure AD Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка на Alibaba Cloud Service (единый вход на основе ролей) с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Alibaba Cloud Service (единый вход на основе ролей) поддерживает единый вход, инициированный **поставщиком удостоверений**.

## <a name="adding-alibaba-cloud-service-role-based-sso-from-the-gallery"></a>Добавление Alibaba Cloud Service (единый вход на основе ролей) из коллекции

Чтобы настроить интеграцию Alibaba Cloud Service (единый вход на основе ролей) с Azure AD, необходимо добавить Alibaba Cloud Service (единый вход на основе ролей) из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Alibaba Cloud Service (единый вход на основе ролей)** .
1. Выберите **Alibaba Cloud Service (единый вход на основе ролей)** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.
5. На странице **Alibaba Cloud Service (единый вход на основе ролей)** щелкните **Свойства** в области навигации слева, скопируйте **идентификатор объекта** и сохраните его на своем компьютере для последующего использования.

    ![Конфигурация свойств](./media/alibaba-cloud-service-role-based-sso-tutorial/Properties.png)


## <a name="configure-and-test-azure-ad-single-sign-on-for-alibaba-cloud-service-role-based-sso"></a>Настройка и тестирование единого входа Azure AD для Alibaba Cloud Service (единый вход на основе ролей)

Настройте и проверьте единый вход Azure AD в Alibaba Cloud Service (единый вход на основе ролей) с помощью тестового пользователя **B.Simon**. Чтобы обеспечить работу единого входа, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Alibaba Cloud Service (единый вход на основе ролей).

Чтобы настроить и проверить единый вход Azure AD в Alibaba Cloud Service (единый вход на основе ролей), выполните действия в следующих стандартных блоках:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
2. **[Настройка единого входа на основе ролей в Alibaba Cloud Service](#configure-role-based-single-sign-on-in-alibaba-cloud-service)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Настройка единого входа в Alibaba Cloud Service (единый вход на основе ролей)](#configure-alibaba-cloud-service-role-based-sso-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя Alibaba Cloud Service (единый вход на основе ролей)](#create-alibaba-cloud-service-role-based-sso-test-user)** нужно для того, чтобы в Alibaba Cloud Service (единый вход на основе ролей) существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
3. **[Проверка единого входа](#test-sso)** необходима, чтобы убедиться в корректной работе конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **Alibaba Cloud Service (единый вход на основе ролей)** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Правка базовой конфигурации SAML](common/edit-urls.png)

4. Если у вас есть **файл метаданных поставщика служб**, выполните следующие действия в разделе **Базовая конфигурация SAML**:

    >[!NOTE]
    >Метаданные поставщика службы можно получить по этому [URL-адресу](https://signin.alibabacloud.com/saml-role/sp-metadata.xml).

    a. Щелкните **Отправить файл метаданных**.

    b. Щелкните **значок папки**, выберите файл метаданных и нажмите кнопку **Отправить**.

    c. После успешной отправки файла метаданных значения **Идентификатор** и **URL-адрес ответа** будут заполнены автоматически в разделе "Alibaba Cloud Service (единый вход на основе ролей)":

    > [!Note]
    > Если поля **Идентификатор** и **URL-адрес ответа** автоматически не заполняются, введите эти значения вручную в соответствии с поставленной задачей.

1. Alibaba Cloud Service (единый вход на основе ролей) требует настройки ролей в Azure AD. Утверждение роли уже настроено, поэтому вам не нужно менять настройки, но нужно его создать в AAD по инструкциям из [этой статьи](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management).

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** найдите элемент **XML метаданных федерации** и выберите **Скачать**, чтобы скачать сертификат и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/metadataxml.png)

1. В разделе **Настройка Alibaba Cloud Service (единый вход на основе ролей)** скопируйте требуемый URL-адрес.

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

В этом разделе описано, как разрешить пользователю B.Simon использовать единый вход Azure, предоставив этому пользователю доступ к Alibaba Cloud Service (единый вход на основе ролей).

1. На портале Azure выберите **Корпоративные приложения**, а затем —**Все приложения**.
1. Выберите **Alibaba Cloud Service (единый вход на основе ролей)** из списка приложений.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. На вкладке **Пользователи и группы** выберите u2 из списка пользователей и щелкните **Выбрать**. Затем щелкните **Назначить**.

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test01.png)

1. Просмотрите назначенную роль и протестируйте приложение Alibaba Cloud Service (единый вход на основе ролей).

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test02.png)

    >[!NOTE]
    >После назначения пользователя (u2) созданная роль будет автоматически связана с пользователем. Если вы создали несколько ролей, при необходимости свяжите соответствующую роль с пользователем. Если вы хотите реализовать единый вход Azure AD на основе ролей для нескольких учетных записей Alibaba Cloud, повторите предыдущие шаги.

## <a name="configure-role-based-single-sign-on-in-alibaba-cloud-service"></a>Настройка единого входа на основе ролей для Alibaba Cloud Service

1. Войдите в [консоль управления доступом к ресурсам](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fram.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.8.7d904e167h6Yg9) Alibaba Cloud с помощью учетной записи Account1.

2. В области навигации слева выберите **SSO** (Единый вход).

3. На вкладке **Role-based SSO** (Единый вход на основе ролей), нажмите **Create IdP** (Создать поставщика удостоверений).

4. На отображаемой странице введите `AAD` в поле IdP Name (Имя поставщика удостоверений), введите описание в поле **Note** (Примечание), нажмите **Upload**, чтобы отправить файл метаданных федерации, который вы скачали ранее, и щелкните **OК**.

5. После успешного создания поставщика удостоверений нажмите **Create RAM Role** (Создать роль для управления доступом к ресурсам).

6. В поле **RAM Role Name** (Имя роли для управления доступом к ресурсам) введите `AADrole`, выберите `AAD` из раскрывающегося списка **Select IdP** (Выбор поставщика удостоверений) и щелкните "OК".

    >[!NOTE]
    >При необходимости вы можете назначить роли разрешения. Мы рекомендуем, чтобы после создания поставщика удостоверений и соответствующей роли вы сохранили имена ARN поставщика удостоверений и роли для последующего использования. Вы можете получить имена ARN на страницах со сведениями о поставщике удостоверений и роли.

7. Свяжите роль для управления доступом к ресурсам Alibaba Cloud (AADrole) с пользователем Azure AD (u2): Чтобы связать роль для управления доступом к ресурсам с пользователем Azure AD, создайте роль в Azure AD, выполнив такие шаги:

    a. Войдите в [обозреватель Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer?spm=a2c63.p38356.879954.9.7d904e167h6Yg9).

    b. Нажмите кнопку **Изменить разрешения**, чтобы получить необходимые разрешения для создания роли.

    ![Конфигурация Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph01.png)

    c. Выберите указанные ниже разрешения в списке и нажмите кнопку **Изменить разрешения**, как показано на следующем рисунке.

    ![Конфигурация Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph02.png)

    >[!NOTE]
    >После предоставления разрешений повторно войдите в обозреватель Graph.

    d. На странице обозревателя Graph выберите **GET** в первом раскрывающемся списке и **beta** во втором. Затем введите `https://graph.microsoft.com/beta/servicePrincipals` в поле рядом с раскрывающимися списками и щелкните **Выполнить запрос**.

    ![Конфигурация Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph03.png)

    >[!NOTE]
    >Если вы используете несколько каталогов, в поле запроса можно ввести `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`.

    д. В разделе **Предварительный просмотр ответа** извлеките свойство appRoles из субъекта-службы для последующего использования.

    ![Конфигурация Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph05.png)

    >[!NOTE]
    >Свойство appRoles можно найти, введя `https://graph.microsoft.com/beta/servicePrincipals/<objectID>` в поле запроса. Обратите внимание, что `objectID` является идентификатором объекта, который вы скопировали на странице **Свойства** Azure AD.

    Е. Вернитесь в обозреватель Graph, измените метод с **GET** на **PATCH**, вставьте указанное ниже содержимое в раздел **Текст запроса** и щелкните **Выполнить запрос**:
    ```
    { 
    "appRoles": [
        { 
        "allowedMemberTypes":[
            "User"
        ],
        "description": "msiam_access",
        "displayName": "msiam_access",
        "id": "41be2db8-48d9-4277-8e86-f6d22d35****",
        "isEnabled": true,
        "origin": "Application",
        "value": null
        },
        { "allowedMemberTypes": [
            "User"
        ],
        "description": "Admin,AzureADProd",
        "displayName": "Admin,AzureADProd",
        "id": "68adae10-8b6b-47e6-9142-6476078cdbce",
        "isEnabled": true,
        "origin": "ServicePrincipal",
        "value": "acs:ram::187125022722****:role/aadrole,acs:ram::187125022722****:saml-provider/AAD"
        }
    ]
    }
    ```
    > [!NOTE]
    > `value` — это ARN поставщика удостоверений и роли, созданных вами в консоли управления доступом к ресурсам. При необходимости здесь можно добавить несколько ролей. Azure AD будет отправлять значение этих ролей в качестве значения утверждения в ответе SAML. Но вы сможете добавить новые роли только после части `msiam_access` для операции исправления. Чтобы упростить процесс создания, мы рекомендуем вам использовать генератор идентификаторов (например, GUID Generator) для создания идентификаторов в реальном времени.

    ж. Когда к субъекту-службе будет применена необходимая роль, свяжите роль с пользователем Azure AD (u2), выполнив шаги в разделе **Назначение тестового пользователя Azure AD** этого руководства.

### <a name="configure-alibaba-cloud-service-role-based-sso-sso"></a>Настройка единого входа для Alibaba Cloud Service (единый вход на основе ролей)

Чтобы настроить единый вход на стороне **Alibaba Cloud Service (единый вход на основе ролей)** , нужно отправить скачанный **XML-файл метаданных федерации** и соответствующие URL-адреса, скопированные на портале Azure, в [службу поддержки Alibaba Cloud Service (единый вход на основе ролей)](https://www.aliyun.com/service/). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="create-alibaba-cloud-service-role-based-sso-test-user"></a>Создание тестового пользователя Alibaba Cloud Service (единый вход на основе ролей)

В этом разделе описано, как создать пользователя Britta Simon в приложении Alibaba Cloud Service (единый вход на основе ролей). Обратитесь в [службу поддержки Alibaba Cloud Service (единый вход на основе ролей)](https://www.aliyun.com/service/), чтобы добавить пользователей на эту платформу. Перед использованием единого входа необходимо создать и активировать пользователей.

## <a name="test-sso"></a>Проверка единого входа 

После завершения работы с предыдущими конфигурациями протестируйте приложение Alibaba Cloud Service (единый вход на основе ролей), выполнив следующие шаги:

1. На портале Azure перейдите на страницу **Alibaba Cloud Service (единый вход на основе ролей)** , выберите **Единый вход** и нажмите **Тест**.

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test03.png)

2. Щелкните **Войдите от имени текущего пользователя**.

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test04.png)

3. На странице выбора учетной записи выберите u2.

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test05.png)

4. Отобразится следующая страница с указанием того, что единый вход на основе ролей успешно выполнен.

    ![Конфигурация тестирования](./media/alibaba-cloud-service-role-based-sso-tutorial/test06.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Руководства по интеграции приложений SaaS с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Единый вход в приложениях в Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Что представляет собой условный доступ в Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Попробуйте Alibaba Cloud Service (единый вход на основе ролей) в работе с Azure AD](https://aad.portal.azure.com/)

