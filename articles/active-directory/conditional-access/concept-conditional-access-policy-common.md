---
title: Общие политики условного доступа — Azure Active Directory
description: Часто используемые политики условного доступа для организаций
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: cfb5fb17abd5a433c177d3efc5a4f0a2cec3d4d7
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186142"
---
# <a name="common-conditional-access-policies"></a>Распространенные политики условного доступа

[Параметры безопасности по умолчанию](../fundamentals/concept-fundamentals-security-defaults.md) прекрасно подходят для некоторых организаций, но многие организации требуют большей гибкости, чем они предлагают. Например, многим требуется возможность исключить из политик условного доступа, требующих многофакторную проверку подлинности, определенные учетные записи, такие как учетные записи для аварийного доступа или администрирования с разделением. Для этих организаций распространенные политики, упоминаемые в этой статье, могут быть использованы.

![Политики условного доступа в портал Azure](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Учетные записи для аварийного доступа

Дополнительные сведения об учетных записях для аварийного доступа и их важность можно найти в следующих статьях: 

* [Управление учетными записями аварийного доступа в Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Создание отказоустойчивой стратегии управления доступом с помощью Azure Active Directory](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Типичные политики, развернутые организациями

* [Требовать MFA для администраторов](howto-conditional-access-policy-admin-mfa.md)\*
* [Требовать MFA для\* управления Azure](howto-conditional-access-policy-azure-management.md)
* [Требовать MFA для всех пользователей](howto-conditional-access-policy-all-users-mfa.md)\*
* [Блокировать устаревшие\* проверки подлинности](howto-conditional-access-policy-block-legacy.md)
* [Условный доступ на основе рисков (требуется Azure AD Premium P2)](howto-conditional-access-policy-risk.md)
* [Требовать надежное расположение для регистрации MFA](howto-conditional-access-policy-registration.md)
* [Блокировать доступ по расположению](howto-conditional-access-policy-location.md)
* [Требовать соответствующее устройство](howto-conditional-access-policy-compliant-device.md)

\* этих четырех политик при совместной настройке, будет имитировать функции, включенные [по умолчанию безопасности](../fundamentals/concept-fundamentals-security-defaults.md).

## <a name="next-steps"></a>Следующие шаги

- [Моделирование поведения входа с помощью средства What If условного доступа.](troubleshoot-conditional-access-what-if.md)
- [Используйте режим "только отчет" для условного доступа, чтобы определить влияние новых политик.](concept-conditional-access-report-only.md)
