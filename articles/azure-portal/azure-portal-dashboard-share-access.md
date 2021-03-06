---
title: Совместное использование панелей мониторинга портал Azure с помощью управления доступом на основе ролей
description: В этой статье описывается, как предоставить общий доступ к панели мониторинга на портале Azure с помощью управления доступом на основе ролей.
services: azure-portal
documentationcenter: ''
author: mgblythe
manager: mtillman
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/29/2020
ms.author: mblythe
ms.openlocfilehash: e8d251cef9e67cb8fc0c11df8ce546383f75a679
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900887"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Предоставление общего доступа к панелям мониторинга Azure с помощью управления доступом на основе ролей

После настройки панели мониторинга ее можно опубликовать и использовать совместно с другими пользователями в организации. Вы разрешаете другим пользователям просматривать панель мониторинга с помощью [управления доступом на основе ролей](../role-based-access-control/role-assignments-portal.md) (RBAC) Azure. Назначение роли пользователю или группе пользователей. Эта роль определяет, могут ли пользователи просматривать или изменять опубликованную панель мониторинга.

Все опубликованные панели мониторинга реализуются как ресурсы Azure. Они существуют как управляемые элементы в рамках подписки и содержатся в группе ресурсов. С точки зрения контроля доступа панели мониторинга ничем не отличаются от других ресурсов, например виртуальной машины и учетной записи хранения.

> [!TIP]
> Отдельные элементы на панели мониторинга принудительно применяют собственные требования к контролю доступа (в зависимости от отображаемых ими ресурсов). Вы можете широко предоставлять общий доступ к панели мониторинга, одновременно защищая данные на отдельных плитках.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Общие сведения о контроле доступа для панелей мониторинга

С помощью управления доступом на основе ролей пользователям можно назначать роли на трех разных уровнях:

* Подписка
* resource group
* ресурс

Назначенные разрешения наследуются от подписки к ресурсу. Опубликованная панель мониторинга — это ресурс. Возможно, у вас уже есть пользователи, которым назначены роли для подписки, которая применяется к опубликованной панели мониторинга.

Допустим, у вас есть подписка Azure, и нескольким членам вашей группы назначены роли *владельца*, *участника* или *читателя* этой подписки. Пользователи, являющиеся владельцами или участниками, могут перечислять, просматривать, создавать, изменять и удалять панели мониторинга в рамках подписки. Пользователи, которые являются читателями, могут перечислять и просматривать панели мониторинга, но не могут изменять или удалять их. Пользователи с доступом для чтения могут вносить локальные изменения на опубликованную панель мониторинга, например при устранении проблемы, но не могут публиковать эти изменения обратно на сервер. Они могут создать собственную копию панели мониторинга для себя.

Также можно назначить разрешения для группы ресурсов, содержащей несколько панелей мониторинга, или для отдельной панели мониторинга. Например, вы можете решить, что группа пользователей должна располагать ограниченными разрешениями в подписке, но иметь доступ более высокого уровня к определенной панели мониторинга. Назначьте этих пользователей роли для этой панели мониторинга.

## <a name="publish-dashboard"></a>Публикация панели мониторинга

Предположим, что вы настроили панель мониторинга, к которой вы хотите предоставить общий доступ группе пользователей в вашей подписке. Ниже показано, как предоставить общий доступ к панели мониторинга группе, именуемой диспетчерами хранилища. Вы можете присвоить группе любое имя. Дополнительные сведения см. [в разделе Управление группами в Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Перед назначением доступа необходимо опубликовать панель мониторинга.

1. На панели мониторинга выберите **Общий доступ**.

    ![Выберите общий доступ для панели мониторинга.](./media/azure-portal-dashboard-share-access/share-dashboard-for-access-control.png)

1. В окне **общий доступ + управление доступом**выберите **опубликовать**.

    ![Публикация панели мониторинга](./media/azure-portal-dashboard-share-access/publish-dashboard-for-access-control.png)

     По умолчанию общий доступ публикует вашу панель мониторинга в группе ресурсов с именем **панели мониторинга**.

Панель мониторинга опубликована. Если разрешения, наследуемые от подписки, подходят, вам больше не нужно предпринимать никаких действий. Другие пользователи в организации могут получать доступ к панели мониторинга и изменять ее на основе роли уровня подписки.

## <a name="assign-access-to-a-dashboard"></a>Назначение доступа к панели мониторинга

Группе пользователей можно назначить роль для этой панели мониторинга.

1. После публикации панели мониторинга в области **общий доступ и управление доступом**выберите **Управление пользователями**.

    ![Управление пользователями для панели мониторинга](./media/azure-portal-dashboard-share-access/manage-users-for-access-control.png)

    Чтобы получить доступ к доступу **+ Управление доступом** с панели мониторинга, выберите параметр **общий доступ** или отменить **общий доступ** .

1. Выберите **назначения ролей** , чтобы просмотреть существующих пользователей, которым уже назначена роль для этой панели мониторинга.

1. Щелкните **Добавить**, чтобы добавить нового пользователя или группу.

    ![Добавление пользователя для доступа к панели мониторинга](./media/azure-portal-dashboard-share-access/manage-users-existing-users.png)

1. Выберите роль, которая представляет предоставляемые разрешения. В этом примере щелкните **Участник**.

1. Выберите пользователя или группу, которым будет назначена роль. Если вы не видите пользователя или группу, которые вы ищете в списке, используйте поле поиска. Список доступных групп зависит от групп, созданных в Active Directory.

1. По завершении добавления пользователей или групп нажмите кнопку **ОК**.

    Новое назначение будет добавлено в список пользователей. Его **доступ** указан как **назначенный** , а не **унаследованный**.

    ![назначенные роли](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Дальнейшие действия

* Список ролей см. [в статье встроенные роли для ресурсов Azure](../role-based-access-control/built-in-roles.md).
* Дополнительные сведения об управлении ресурсами см. [в статье Управление ресурсами Azure с помощью портал Azure](resource-group-portal.md).

