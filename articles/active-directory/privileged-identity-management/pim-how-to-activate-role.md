---
title: Активация ролей Azure AD в PIM-Azure Active Directory | Документация Майкрософт
description: Узнайте, как активировать роли Azure AD в Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6007762c897337170dec69c3486302aa62723480
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756290"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Активировать мои роли Azure AD в PIM

Компонент Azure Active Directory (Azure AD) Privileged Identity Management (PIM) упрощает управление привилегированным доступом пользователей к ресурсам в Azure AD и других веб-службах Майкрософт, например Office 365 или Microsoft Intune.  

Если вам назначена роль временного администратора, то это значит, что вы можете активировать эту роль, когда вам потребуется выполнить привилегированные задачи. Например, если вы управляете компонентами Office 365 нерегулярно, корпоративные администраторы привилегированных ролей могут не назначить вам роль постоянного глобального администратора, так как эта роль влияет и на другие службы. Вместо этого они могут назначить вам более подходящие для работы с Azure AD роли, например роль администратора Exchange Online. При необходимости вы можете запросить активацию роли, чтобы получить административный доступ на определенный период времени.

Эта статья предназначена для администраторов, которым необходимо активировать роль Azure AD в управление привилегированными пользователями.

## <a name="activate-a-role"></a>Активация роли

Если необходимо выполнить роль Azure AD, можно запросить активацию с помощью параметра навигации **My Roles (Мои роли** ) в Управление привилегированными пользователями.

1. Войдите на [портале Azure](https://portal.azure.com/).

1. Откройте страницу **Azure AD Privileged Identity Management**. Сведения о том, как добавить плитку управление привилегированными пользователями на панель мониторинга, см. в разделе [Начало работы с управление привилегированными пользователями](pim-getting-started.md).

1. Выберите **Роли Azure AD**.

1. Щелкните **Мои роли** , чтобы просмотреть список соответствующих РОЛЕЙ Azure AD.

    ![Роли Azure AD. Мои роли, отображающие список доступных или активных ролей](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Найдите роль, которую необходимо активировать.

    ![Роли Azure AD. в списке "доступные роли" отображается ссылка "активировать"](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Щелкните **Активировать**, чтобы открыть область сведений об активации роли.

1. Если ваша роль требует многофакторной проверки подлинности (MFA), щелкните **Чтобы продолжить, подтвердите свою личность**. Проверка подлинности выполняется один раз за сеанс.

    ![Проверка многофакторной панели удостоверений с помощью MFA перед активацией роли](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Щелкните **Подтверждение вашей личности** и следуйте инструкциям для настройки дополнительной проверки безопасности.

    ![Страница «дополнительная проверка безопасности» с вопросом о том, как связаться с вами](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Щелкните **Активировать**, чтобы открыть область активации.

    ![Область активации для указания времени начала, длительности, билета и причины](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. При необходимости укажите время начала настраиваемой активации.

1. Укажите длительность активации.

1. В поле **Причина активации** введите основание для запроса активации. Для некоторых ролей требуется предоставить номер запроса на устранение неисправности.

    ![Готовая область активации с настраиваемым временем начала, длительностью, билетом и причиной](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Щелкните **Активировать**.

    Если роль не требует утверждения, отображается область **состояния активации** , на которой отображается состояние активации.

    ![Страница состояния активации с тремя этапами активации](./media/pim-how-to-activate-role/activation-status.png)

    После завершения всех этапов щелкните ссылку **выхода** , чтобы выйти из портал Azure. После входа на портал теперь можно использовать эту роль.

    Если [роль требует утверждения](./azure-ad-pim-approval-workflow.md) для активации, в правом верхнем углу браузера появится уведомление о том, что запрос ожидает утверждения.

    ![Запрос на активацию ожидает уведомления об утверждении](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Просмотр состояния запросов

Вы можете просмотреть состояние запросов, ожидающих активации.

1. Откройте страницу Azure AD Privileged Identity Management.

1. Выберите **Роли Azure AD**.

1. Щелкните **Мои запросы**, чтобы просмотреть список ваших запросов.

    ![Роли Azure AD — список моих запросов](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Деактивация роли

Активированная роль автоматически деактивируется через определенный период времени (соответствующую длительность).

Если вы завершили задачи администратора раньше, роль также можно отключить вручную в Azure AD Privileged Identity Management.

1. Откройте страницу Azure AD Privileged Identity Management.

1. Выберите **Роли Azure AD**.

1. Щелкните **Мои роли**.

1. Щелкните **Активные роли**, чтобы просмотреть список активных ролей.

1. Найдите роль, которая вам больше не нужна, а затем щелкните **Деактивировать**.

## <a name="cancel-a-pending-request"></a>Отмена ожидающего запроса

Если вы не активируете роль, требующую утверждения, вы можете в любой момент отменить запрос, ожидающий утверждения.

1. Откройте страницу Azure AD Privileged Identity Management.

1. Выберите **Роли Azure AD**.

1. Щелкните **Мои запросы**.

1. Нажмите кнопку **Отмена** для роли, которую хотите отменить.

    При нажатии кнопки Отмена запрос будет отменен. Чтобы активировать роль снова, необходимо будет отправить новый запрос на активацию.

   ![Список "Мои запросы" с выделенной кнопкой "Отмена"](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Устранение неполадок

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Разрешения не предоставляются после активации роли

При активации роли в управление привилегированными пользователями Активация может не мгновенно распространяться на все порталы, для которых требуется привилегированная роль. Иногда, даже если изменение распространилось, из-за веб-кэширования на портале оно может начать действовать не сразу. Если активация отложена, то следует выполнить следующие действия.

1. Закройте портал Azure, а затем войдите на него снова.

    При активации роли Azure AD вы увидите этапы активации. По завершении всех этапов появится ссылка **Выйти**. Эту ссылку можно использовать для выхода. В большинстве случаев задержка активации будет устранена.

1. В управление привилегированными пользователями убедитесь, что вы указаны в списке как член роли.

## <a name="next-steps"></a>Дальнейшие действия

- [Активация ролей ресурсов Azure в управление привилегированными пользователями](pim-resource-roles-activate-your-roles.md)
