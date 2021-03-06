---
title: Пользователи для приложения в коллекции Azure AD не подготавливаются
description: В этой статье описаны способы решения распространенных проблем, когда пользователи, для которых настроена подготовка в Azure AD, не появляются в приложении в коллекции Azure AD.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0147644e11ec1d54ac38f8099c9e65c25aa94962
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067257"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Пользователи для приложения в коллекции Azure AD не подготавливаются
После настройки автоматической подготовки для приложения (в том числе проверки действительности учетных данных, с помощью которых Azure AD подключается к приложению) пользователи и (или) группы подготавливаются для приложения. Подготовка определяется следующими данными:

-   Какие пользователи и группы были **назначены** приложению. Обратите внимание, что подготовка вложенных групп или групп Office 365 не поддерживается. Дополнительные сведения о назначении см. в разделе [Назначение пользователя или группы корпоративному приложению в Azure Active Directory](../manage-apps/assign-user-or-group-access-portal.md).
-   Было ли **сопоставление атрибутов** включено и настроено для синхронизации допустимых атрибутов из Azure AD в приложение. Дополнительные сведения о сопоставлении атрибутов см. в разделе [Настройка сопоставления атрибутов для подготовки пользователей для приложений SaaS в Azure Active Directory](customize-application-attributes.md).
-   Используется ли **фильтр области**, который фильтрует пользователей на основе значений указанных атрибутов. Дополнительные сведения о фильтрах области см. в статье [Подготовка приложений на основе атрибутов с использованием фильтров области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
  
Если вы следите за тем, чтобы пользователи не были подготовлены, просмотрите [журналы подготовки (Предварительная версия)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) в Azure AD. и найдите в них записи по конкретному пользователю.

Вы можете получить доступ к журналам подготовки в портал Azure, выбрав **Azure Active Directory** &gt; **корпоративные приложения** &gt; **Подготовка журналов (Предварительная версия)** в разделе **действие** . Данные подготовки можно искать на основе имени пользователя или идентификатора в исходной системе или в целевой системе. Дополнительные сведения см. в статье [Подготовка журналов (Предварительная версия)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context). 

Журналы подготовки записывают все операции, выполняемые службой подготовки, включая запросы Azure AD для назначенных пользователей, которые находятся в области для подготовки, запрашивают целевое приложение о существовании этих пользователей, сравнивая объекты пользователей между система. Затем добавьте, измените или отключите учетную запись пользователя в целевой системе на основе сравнения.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Общие проблемные области при подготовке, которые необходимо учитывать
Ниже приведен список общих проблемных областей, в которые можно углубиться, если у вас есть какие-то идеи.

- [Служба подготовки не запускается](#provisioning-service-does-not-appear-to-start)
- [Журналы подготовки говорят, что пользователи пропускаются и не подготавливаются, даже если им назначены](#provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Служба подготовки не запускается
Если для параметра **состояние подготовки** задано значение **включено** в **Azure Active Directory &gt; корпоративные приложения &gt; \[имя приложения\] &gt;подготовка** раздела портал Azure. Однако другие сведения о состоянии на этой странице не отображаются после последующей перезагрузки, скорее всего, служба запущена, но еще не завершила первоначальный цикл. Проверьте **журналы подготовки (Предварительная версия)** , описанные выше, чтобы определить, какие операции выполняет служба, и в случае каких-либо ошибок.

>[!NOTE]
>Начальный цикл может занять от 20 минут до нескольких часов, в зависимости от размера каталога Azure AD и числа пользователей в области для подготовки. Последующие синхронизации после начального цикла выполняются быстрее, так как служба подготовки сохраняет водяные знаки, представляющие состояние обеих систем после начального цикла. Начальный цикл улучшает производительность последующих синхронизаций.
>


## <a name="provisioning-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Журналы подготовки говорят, что пользователи пропускаются и не подготавливаются, даже если они назначены

Когда пользователь отображается как "пропущено" в журналах подготовки, важно ознакомиться с вкладкой " **шаги** " журнала, чтобы определить причину. Ниже приведены распространенные причины и способы устранения ошибок:

- **Настроен фильтр области** **, который фильтрует пользователя на основе значения атрибута**. Дополнительные сведения о фильтрах области см. в [этой статье](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
- **Пользователь "не имеет фактических прав".** Если вы видите это сообщение об ошибке, это свидетельствует о наличии проблемы с записью назначения пользователя, которая хранится в Azure AD. Чтобы устранить эту проблему, отмените назначение пользователя (или группы) для приложения и повторно назначьте пользователя (или группу). Дополнительные сведения о назначении доступа пользователю или группе см. в [этой статье](../manage-apps/assign-user-or-group-access-portal.md).
- **Обязательный атрибут для пользователя отсутствует или его значение не указано.** При настройке подготовки важно просмотреть и настроить сопоставления атрибутов и рабочие процессы, которые определяют, какие свойства пользователя (или группы) передаются из Azure AD в приложение. К ним относится "свойство сопоставления", используемое для уникальной идентификации и сопоставления пользователей или групп между двумя системами. Дополнительные сведения об этом важном процессе см. в разделе [Настройка сопоставления атрибутов для подготовки пользователей для приложений SaaS в Azure Active Directory](customize-application-attributes.md).
- **Сопоставление атрибутов для групп:** подготовка имени группы и сведений о группе наряду с ее участниками (если поддерживается для некоторых приложений). Вы можете включить или отключить эту функцию, включив или отключив **сопоставление** для объектов группы, отображаемых на вкладке **Подготовка** . Если включены группы подготовки, обязательно проверьте сопоставления атрибутов, чтобы убедиться, что для соответствующего поля используется соответствующее поле. Соответствующим идентификатором может быть отображаемое имя или псевдоним электронной почты. Если соответствующее свойство для группы в Azure AD отсутствует или не указано, группа и ее участники не будут подготовлены.

## <a name="next-steps"></a>Дальнейшие действия

[Служба синхронизации Azure AD Connect: общие сведения о декларативной подготовке](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
