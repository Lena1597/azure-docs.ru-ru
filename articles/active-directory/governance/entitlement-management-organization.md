---
title: Добавление подключенной Организации в управление назначением Azure AD — Azure Active Directory
description: Узнайте, как разрешить пользователям за пределами Организации запрашивать пакеты доступа, чтобы вы могли сотрудничать над проектами.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 01/22/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c1b6f5ebffa39d3b735e85df794e37329e3aa2e
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548906"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Добавление подключенной Организации в управление назначением Azure AD

Управление назначением Azure AD позволяет сотрудничать с людьми за пределами Организации. Если вы часто работаете с пользователями во внешнем каталоге или домене Azure AD, вы можете добавить их в качестве подключенной Организации. В этой статье описывается, как добавить подключенную организацию, чтобы разрешить пользователям за пределами Организации запрашивать ресурсы в каталоге.

## <a name="what-is-a-connected-organization"></a>Что такое подключенная Организация?

Подключенная Организация — это внешний каталог или домен Azure AD, с которым связана связь.

Например, предположим, что вы работаете в банке Woodgrove Bank и хотите сотрудничать с двумя внешними организациями. Эти две организации имеют различные конфигурации:

- В институте проектирования графики используется Azure AD, а пользователи имеют имя участника-пользователя, заканчивающееся на `graphicdesigninstitute.com`
- Contoso еще не использует Azure AD. Пользователи Contoso имеют имя участника-пользователя, заканчивающееся на `contoso.com`.

В этом случае можно настроить две подключенные Организации. Вы создадите одну подключенную организацию для создания графического проекта и одну для contoso. Если затем добавить эти две подключенные Организации к политике, пользователи каждой организации с именем участника-пользователя, соответствующим политике, могут запрашивать пакеты доступа. Пользователи с именем участника-пользователя, имеющего домен graphicdesigninstitute.com, будут соответствовать Организации, подключенной к Институту, и могут отправлять запросы, а пользователи с именем участника-пользователя, имеющим домен contoso.com, будут соответствовать Организации, подключенной к Contoso, также разрешено запрашивать пакеты. Более того, так как для работы с графическим дизайном используется Azure AD, все пользователи с именем участника, соответствующими [проверенному домену](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) , добавленному в клиент, например графикдесигнинституте. пример, также смогут запрашивать пакеты доступа, используя ту же политику.

![Пример подключенной Организации](./media/entitlement-management-organization/connected-organization-example.png)

Как пользователи из каталога или домена Azure AD будут проходить проверку подлинности, зависит от типа аутентификации. Существуют следующие типы проверки подлинности для подключенных организаций.

- Azure AD
- [Прямая Федерация](../b2b/direct-federation.md)
- [Одноразовый пароль](../b2b/one-time-passcode.md) (домен)

Пример добавления подключенной организации см. в следующем видео:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Добавление подключенной организации

Выполните следующие действия, чтобы добавить внешний каталог или домен Azure AD в качестве подключенной Организации.

**Предварительная роль:** Глобальный администратор, администратор пользователей или приглашенный гость

1. На портале Azure щелкните **Azure Active Directory** и выберите **Управление удостоверениями**.

1. В меню слева щелкните **подключенные Организации** , а затем щелкните **Добавить подключенную организацию**.

    ![Управление удостоверениями — подключенные Организации — добавить подключенную организацию](./media/entitlement-management-organization/connected-organization.png)

1. На вкладке **Основные сведения** введите отображаемое имя и описание для Организации.

    ![Добавление подключенной Организации — вкладка «основы»](./media/entitlement-management-organization/organization-basics.png)

1. На вкладке **каталог и домен** щелкните **Добавить каталог + домен** , чтобы открыть панель выбор каталогов и доменов.

1. Введите имя домена для поиска каталога или домена Azure AD. Необходимо ввести полное доменное имя.

1. Убедитесь, что это правильная организация по указанному имени и типу проверки подлинности. Как пользователи будут входить в систему, зависит от типа проверки подлинности.

    ![Добавление подключенной Организации — выбор каталогов и доменов](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Нажмите кнопку **Добавить** , чтобы добавить каталог или домен Azure AD. В настоящее время можно добавить только один каталог Azure AD или домен для каждой подключенной Организации.

    > [!NOTE]
    > Все пользователи из каталога или домена Azure AD смогут запрашивать этот пакет доступа. Сюда входят пользователи в Azure AD из всех поддоменов, связанных с каталогом, если только эти домены не заблокированы списком разрешенных или запрещенных для Azure B2B. Дополнительные сведения см. в статье [Предоставление или отзыв приглашений пользователям B2B из отдельных организаций](../b2b/allow-deny-list.md).

1. После добавления каталога или домена Azure AD нажмите кнопку **выбрать**.

    Организация появится в списке.

    ![Добавление подключенной Организации — вкладка "каталоги"](./media/entitlement-management-organization/organization-directory-domain.png)

1. На вкладке **спонсоры** добавьте дополнительные спонсоры для этой подключенной Организации.

    Спонсоры — это внутренние или внешние пользователи, которые уже находятся в каталоге, который является точкой контакта связи с этой подключенной Организацией. Внутренние спонсоры — это пользователи, входящие в ваш каталог. Внешние спонсоры — гостевые пользователи из подключенной Организации, которые были ранее приглашены и уже находятся в вашем каталоге. Спонсоры могут использоваться как утверждающие, когда пользователи в этой подключенной организации запрашивают доступ к этому пакету доступа. Сведения о том, как пригласить гостевого пользователя в каталог, см. [в разделе добавление Azure Active Directory пользователей службы совместной работы B2B в портал Azure](../b2b/add-users-administrator.md).

    При нажатии кнопки " **Добавить/удалить**" отображается область выбора внутреннего или внешнего спонсора. На этой панели отображается Нефильтрованный список пользователей и групп в каталоге.

    ![Доступ к пакету — политика — Добавление подключенной Организации — вкладка «спонсоры»](./media/entitlement-management-organization/organization-sponsors.png)

1. На вкладке " **Проверка и создание** " Проверьте параметры организации и нажмите кнопку " **создать**".

    ![Доступ к пакету — политика — Добавление подключенной Организации — проверка и создание](./media/entitlement-management-organization/organization-review-create.png)

## <a name="delete-a-connected-organization"></a>Удаление подключенной Организации

Если у вас больше нет связи с внешним каталогом или доменом Azure AD, можно удалить подключенную организацию.

**Предварительная роль:** Глобальный администратор, администратор пользователей или приглашенный гость

1. На портале Azure щелкните **Azure Active Directory** и выберите **Управление удостоверениями**.

1. В меню слева щелкните **подключенные Организации** , а затем щелкните, чтобы открыть подключенную организацию.

1. На странице Обзор нажмите кнопку **Удалить** , чтобы удалить подключенную организацию.

    В настоящее время можно удалить только подключенную организацию, если нет подключенных пользователей.

    ![Управление удостоверениями — подключенные Организации — удалить подключенную организацию](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Дальнейшие действия

- [Управление доступом для внешних пользователей](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Для пользователей, не наличныхся в вашем каталоге](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
