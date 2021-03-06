---
title: Руководство. Настройка Fuze для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической инициализации и отзыва учетных записей пользователей в Fuze.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: a58402297380116f83214e52ae7f2796412755b9
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77057913"
---
# <a name="tutorial-configure-fuze-for-automatic-user-provisioning"></a>Учебник. Настройка Fuze для автоматической подготовки пользователей

Цель этого руководства — продемонстрировать шаги, которые необходимо выполнить в Fuze и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической инициализации и отзыва пользователей и групп в Fuze.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Сейчас этот соединитель предоставляется в общедоступной предварительной версии. Дополнительные сведения об общих условиях использования продуктов в предварительной версии см. в документе [Дополнительные условия использования Предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* Клиент Azure AD.
* [Клиент Fuze](https://www.fuze.com/).
* Учетная запись пользователя в Fuze с разрешениями администратора.

## <a name="assigning-users-to-fuze"></a>Назначение пользователей в Fuze

Azure Active Directory использует концепцию, называемую *назначениями* , чтобы определить, какие пользователи должны получать доступ к выбранным приложениям. В контексте автоматической подготовки учетных записей пользователей синхронизируются только пользователи и группы, назначенные приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей следует решить, какие пользователи и (или) группы в Azure AD должны иметь доступ к Fuze. После принятия решения вы можете назначить этих пользователей и (или) группы для Fuze, следуя приведенным ниже инструкциям.

* [Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-fuze"></a>Важные советы по назначению пользователей в Fuze

* Рекомендуется назначить одного пользователя Azure AD в Fuze для тестирования конфигурации автоматической подготовки пользователей. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя в Fuze необходимо выбрать в диалоговом окне назначения любую допустимую роль конкретного приложения (если она доступна). Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="setup-fuze-for-provisioning"></a>Настройка Fuze для подготовки

Перед настройкой Fuze для автоматической подготовки пользователей с помощью Azure AD необходимо включить подготовку SCIM для Fuze. 

1. Начните с обращения к представителям Fuze для получения следующих необходимых сведений:

    * Список SKU продуктов Fuze, которые сейчас используются в вашей компании.
    * Список кодов местонахождения для местонахождения компании.
    * Список кодов отделов для вашей компании.

2. Эти номера SKU и коды можно найти в документах по контрактам и конфигурации Fuze или обратившись к своему представителя Fuze.

3. После получения требований ваш представителя Fuze предоставит вам маркер проверки подлинности Fuze, необходимый для обеспечения интеграции. Это значение будет указано в поле Секретный токен на вкладке Подготовка приложения Fuze в портал Azure.

## <a name="add-fuze-from-the-gallery"></a>Добавление Fuze из коллекции

Перед настройкой Fuze для автоматической подготовки пользователей с помощью Azure AD необходимо добавить Fuze из коллекции приложений Azure AD в список управляемых приложений SaaS.

**Чтобы добавить Fuze из коллекции приложений Azure AD, выполните следующие действия.**

1. В **[портал Azure](https://portal.azure.com)** на панели навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, нажмите кнопку **новое приложение** в верхней части области.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите **Fuze**, выберите **Fuze** на панели результатов и нажмите кнопку **добавить** , чтобы добавить это приложение.

    ![Fuze в списке результатов](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-fuze"></a>Настройка автоматической подготовки пользователей в Fuze 

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в Fuze на основе назначений пользователей и групп в Azure AD.

> [!TIP]
> Вы также можете включить единый вход на основе SAML для Fuze, следуя инструкциям, приведенным в [руководстве по единому входу Fuze](fuze-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две возможности дополняют друг друга.

### <a name="to-configure-automatic-user-provisioning-for-fuze-in-azure-ad"></a>Чтобы настроить автоматическую подготовку учетных записей пользователей для Fuze в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **корпоративные приложения**, а затем выберите **все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Fuze**.

    ![Ссылка на Fuze в списке "Приложения"](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Вкладка "подготовка"](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Вкладка "подготовка"](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите `https://api.fuze.com/scim/v2` в поле **URL-адрес клиента**. Введите значение **маркера проверки подлинности scim** , полученное ранее от представителя Fuze в **маркере секрета**. Щелкните **проверить подключение** , чтобы убедиться, что Azure AD может подключиться к Fuze. Если подключение не выполняется, убедитесь, что у учетной записи Fuze есть разрешения администратора, и повторите попытку.

    ![Токен URL-адреса клиента](common/provisioning-testconnection-tenanturltoken.png)

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Выберите команду **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей с Fuze**.

    ![Сопоставления пользователей Fuze](media/fuze-provisioning-tutorial/image01.png)

9. Проверьте атрибуты пользователя, которые синхронизированы из Azure AD в Fuze в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в Fuze для операций обновления. Нажмите кнопку **Сохранить**, чтобы зафиксировать все изменения.

    ![Сопоставления пользователей Fuze](media/fuze-provisioning-tutorial/image00.png)

10. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Чтобы включить службу подготовки Azure AD для Fuze, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

12. Определите пользователей и (или) группы, которые вы хотите подготавливать к Fuze, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

15. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов по подготовке, в которых описаны все действия, выполняемые службой подготовки Azure AD в Fuze.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Ограничения соединителя

* Fuze поддерживает настраиваемые атрибуты **scim,** называемые назначениями. Эти атрибуты могут быть созданы и не обновлены. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md).