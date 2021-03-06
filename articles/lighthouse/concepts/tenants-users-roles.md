---
title: Арендаторы, роли и пользователи в сценариях Azure Lighthouse
description: Изучите принципы действия арендаторов, пользователей и ролей Azure Active Directory, а также узнайте о том, как их можно использовать в сценариях Azure Lighthouse.
ms.date: 01/16/2020
ms.topic: conceptual
ms.openlocfilehash: 344e104201a83b3589dae6dbd3b02e49e4575e00
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156341"
---
# <a name="tenants-roles-and-users-in-azure-lighthouse-scenarios"></a>Арендаторы, роли и пользователи в сценариях Azure Lighthouse

Прежде чем подключать клиентов для [делегированного управления ресурсами Azure](azure-delegated-resource-management.md), важно понять, как работают арендаторы, пользователи и роли Azure Active Directory (Azure AD) и как их можно использовать в сценариях Azure Lighthouse.

*Арендатор* — это выделенный и доверенный экземпляр Azure AD. Как правило, арендатор представляет отдельную организацию. Делегированное управление ресурсами Azure позволяет логически проецировать ресурсы одного арендатора на другого. Это позволяет пользователям в управляющем арендаторе (например, принадлежащем поставщику услуг) обращаться к делегированным ресурсам в арендаторе клиента или позволяет [предприятиям с несколькими арендаторами централизовать операции управления](enterprise.md).

Для обеспечения этой логической проекции подписка (либо одна или несколько групп ресурсов в подписке) в арендаторе клиента должна быть *подключена* для управления делегированными ресурсами Azure. Этот процесс подключения можно выполнить либо [с помощью шаблонов Azure Resource Manager](../how-to/onboard-customer.md), либо посредством [публикации общедоступного или частного предложения в Azure Marketplace](../how-to/publish-managed-services-offers.md).

Какой бы метод подключения ни был выбран, необходимо определить *авторизации*. Каждая авторизация указывает учетную запись пользователя в управляющем арендаторе, которая будет иметь доступ к делегированным ресурсам, и встроенную роль, которая задает разрешения, предоставляемые каждому из этих пользователей для данных ресурсов.

## <a name="role-support-for-azure-delegated-resource-management"></a>Поддержка ролей для делегированного управления ресурсами Azure

При определении авторизации каждой учетной записи пользователя должна быть назначена одна из встроенных ролей [управления доступом на основе ролей (RBAC)](../../role-based-access-control/built-in-roles.md). Пользовательские роли и [роли классического администратора подписки](../../role-based-access-control/classic-administrators.md) не поддерживаются.

В настоящее время поддерживаются все [встроенные роли](../../role-based-access-control/built-in-roles.md) для управления делегированными ресурсами Azure, за исключением следующих:

- Роль [Владелец](../../role-based-access-control/built-in-roles.md#owner) не поддерживается.
- Любые встроенные роли с разрешением [DataActions](../../role-based-access-control/role-definitions.md#dataactions) не поддерживаются.
- Встроенная роль [Администратор доступа пользователей](../../role-based-access-control/built-in-roles.md#user-access-administrator) поддерживается, но только для [назначения ролей для управляемого удостоверения в арендаторе клиента](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant). Никакие другие разрешения, обычно предоставляемые этой ролью, применяться не будут. При определении пользователя с этой ролью необходимо также указать встроенные роли, которые этот пользователь может назначить управляемым удостоверениям.

> [!NOTE]
> После добавления в Azure соответствующей новой встроенной роли ее можно назначить при подключении [клиента с помощью шаблонов Azure Resource Manager](../how-to/onboard-customer.md). Возможна задержка, прежде чем вновь добавленная роль станет доступной в Портал Cloud Partner при [публикации предложения управляемой службы](../how-to/publish-managed-services-offers.md).

## <a name="best-practices-for-defining-users-and-roles"></a>Рекомендации по определению пользователей и ролей

При создании авторизаций рекомендуется следующее.

- В большинстве случаев разрешения необходимо назначить группе пользователей или субъекту-службе Azure AD, а не ряду отдельных учетных записей пользователей. В этом случае вы сможете добавлять или удалять доступ для отдельных пользователей, не обновляя и повторно публикуя план при изменении требований к доступу.
- Обязательно следуйте принципам минимальных привилегий, чтобы у пользователей были только разрешения, необходимые для выполнения их заданий. Это помогает снизить вероятность непреднамеренных ошибок. Дополнительные сведения см. в статье [Recommended security practices](../concepts/recommended-security-practices.md) (Рекомендации по безопасности).
- Добавьте пользователя с ролью [Managed Services Registration Assignment Delete](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) (Удаление назначенных регистраций управляемых служб), чтобы можно было при необходимости [удалить доступ к делегированию](../how-to/onboard-customer.md#remove-access-to-a-delegation) позже. Если эта роль не назначена, делегированные ресурсы могут быть удалены только пользователем в арендаторе клиента.
- Убедитесь, что любой пользователь, которому нужно [просматривать страницу "Мои клиенты" на портале Azure](../how-to/view-manage-customers.md), имеет роль [Читатель](../../role-based-access-control/built-in-roles.md#reader) (или другую встроенную роль, которая предоставляет доступ для чтения).

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше о [рекомендациях по обеспечению безопасности для делегированного управления ресурсами Azure](recommended-security-practices.md).
- Подключите клиентов к делегированному управлению ресурсами Azure с помощью [шаблонов Azure Resource Manager](../how-to/onboard-customer.md) или путем [публикации предложения частных или общедоступных управляемых служб в Azure Marketplace](../how-to/publish-managed-services-offers.md).
