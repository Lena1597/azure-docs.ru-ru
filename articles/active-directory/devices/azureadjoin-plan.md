---
title: Планирование реализации Azure Active Directory Join
description: Описание действий, необходимых для реализации присоединенных к Azure AD устройств в вашей среде.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67c42de09c75b7dd6737b80071f1f6eba094b132
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2020
ms.locfileid: "76512425"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Инструкции. Планирование реализации присоединения к Azure AD

Присоединение к Azure AD позволяет подключать устройства непосредственно к Azure AD без необходимости в присоединении к локальной службе Active Directory, в то же время поддерживая продуктивность и безопасность работы пользователей. Присоединение к Azure AD готово к использованию на предприятиях как в масштабных, так и в ограниченных развертываниях.   

В этой статье содержатся сведения, необходимые для планирования реализации присоединения к Azure AD.
 
## <a name="prerequisites"></a>Технические условия

Предполагается, что вы ознакомлены с [общими сведениями об управлении устройствами в Azure Active Directory](../device-management-introduction.md).

## <a name="plan-your-implementation"></a>Планирование реализации

Чтобы спланировать реализацию присоединение к Azure AD, необходимо ознакомиться с:

|   |   |
|---|---|
|![Проверить][1]|Изучение сценариев|
|![Проверить][1]|Изучение инфраструктуры удостоверений|
|![Проверить][1]|Оценка управления устройствами|
|![Проверить][1]|Рекомендации для приложений и ресурсов|
|![Проверить][1]|Изучение вариантов подготовки|
|![Проверить][1]|Настройка службы Enterprise State Roaming|
|![Проверить][1]|Настройка условного доступа|

## <a name="review-your-scenarios"></a>Изучение сценариев 

В некоторых случаях присоединение к гибридной службе Azure AD будет предпочтительным, но присоединение к Azure AD позволяет перейти к облачной модели с Windows. Если вы планируете модернизировать управление устройствами и сократить ИТ-расходы, связанные с устройствами, то присоединение к Azure AD сформирует отличную основу для достижения этих целей.  
 
Присоединение к Azure AD рекомендуется, если ваши цели отвечают следующим условиям:

- Вы внедряете Microsoft 365 как набор бизнес-приложений для своих пользователей.
- Вам нужно управлять устройствами с помощью облачного решения по управлению устройствами.
- Вам нужно упростить подготовку устройств для географически распределенных пользователей.
- Вы планируете модернизировать инфраструктуру приложений.

## <a name="review-your-identity-infrastructure"></a>Изучение инфраструктуры удостоверений  

Присоединение к Azure AD работает как для управляемых, так и для федеративных сред.  

### <a name="managed-environment"></a>Управляемая среда

Управляемую среду можно развернуть при помощи [синхронизации хэша паролей](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) или [сквозной аутентификации](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) с удобным единым входом.

В этих сценариях не требуется настраивать сервер федерации для проверки подлинности.

### <a name="federated-environment"></a>Федеративная среда

У федеративной среды должен быть поставщик удостоверений, поддерживающий протоколы WS-Trust и WS-Fed:

- **WS-Fed.** Этот протокол необходим для присоединения устройства к Azure AD.
- **WS-Trust.** Этот протокол необходим для входа на присоединенном к Azure AD устройстве.

При использовании AD FS нужно включить следующие конечные точки WS-Trust: `/adfs/services/trust/2005/usernamemixed`
 `/adfs/services/trust/13/usernamemixed`
 `/adfs/services/trust/2005/certificatemixed`
 `/adfs/services/trust/13/certificatemixed`

Если ваш поставщик удостоверений не поддерживает эти протоколы, то присоединение Azure AD не работает по умолчанию. 

>[!NOTE]
> Сейчас присоединение к Azure AD не работает с [AD FS 2019, настроенных с внешними поставщиками проверки подлинности в качестве основного метода проверки](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/additional-authentication-methods-ad-fs#enable-external-authentication-methods-as-primary)подлинности. Присоединение к Azure AD по умолчанию используется для проверки пароля в качестве основного метода, что приводит к сбоям проверки подлинности в этом сценарии.


### <a name="smartcards-and-certificate-based-authentication"></a>Смарт-карты и проверка подлинности на основе сертификатов

Вы можете использовать смарт-карты или проверку подлинности на основе сертификатов, чтобы подключать устройства к Azure AD. Однако смарт-карты можно использовать для входа на устройствах, подключенных к Azure AD, если у вас настроены службы AD FS.

**Рекомендация.** Реализуйте Windows Hello для бизнеса, чтобы обеспечить надежную проверку подлинности без паролей на устройствах с Windows 10.

### <a name="user-configuration"></a>Конфигурация пользователей

Если вы создаете пользователей в:

- **локальной службе Active Directory**, необходимо синхронизировать их в Azure AD при помощи [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis); 
- **Azure AD**, дополнительная настройка не требуется.

Локальные имена участников-пользователей, которые отличаются от имен участников-пользователей Azure AD, не поддерживаются на устройствах, присоединенных к Azure AD. Если пользователи применяют локальное имя участника-пользователя, то следует запланировать переход на использование основного имени участника-пользователя в Azure AD.

## <a name="assess-your-device-management"></a>Оценка управления устройствами

### <a name="supported-devices"></a>Поддерживаемые устройства

Присоединение к Azure AD:

- применимо только к устройствам с Windows 10; 
- не применимо к предыдущим версиям Windows или другим операционным системам. Если у вас есть устройства с Windows 7 или Windows 8.1, то необходимо перейти на Windows 10, чтобы развернуть присоединение к Azure AD.
- Не поддерживается на устройствах с доверенным платформенным модулем (TPM) в режиме FIPS.
 
**Рекомендация.** Всегда используйте последний выпуск Windows 10, чтобы воспользоваться обновленными функциями.

### <a name="management-platform"></a>Платформа управления

Управление устройствами для устройств, присоединенных к Azure AD, основано на платформе MDM, такой как Intune, и CSP. В Windows 10 есть встроенный агент MDM, работающий со всеми совместимыми решениями MDM.

> [!NOTE]
> Групповые политики не поддерживаются в устройствах, присоединенных к Azure AD, так как они не подключены к локальным Active Directory. Управление устройствами, присоединенными к Azure AD, возможно только через MDM.

Существует два подхода к управлению устройствами, подключенными к Azure AD:

- **Только MDM** — устройством управляет только поставщик MDM, например Intune. Все политики доставляются в рамках регистрации MDM. Для пользователей Azure AD Premium или EMS развертывание MDM выполняется автоматически в ходе присоединения к Azure AD.
- **Совместное управление** — устройством управляют поставщик MDM и SCCM. При таком подходе агент SCCM устанавливается на устройстве с MDM для администрирования определенных аспектов.

Если вы используете групповые политики, оцените четность политик MDM с помощью [средства анализа миграции MDM (MMAT)](https://github.com/WindowsDeviceManagement/MMAT). 

Просмотрите поддерживаемые и неподдерживаемые политики, чтобы определить, можно ли использовать решение MDM вместо групповых политик. Что касается неподдерживаемых политик, рассмотрите следующие вопросы:

- Необходимы ли неподдерживаемые политики для присоединенных к Azure AD устройств или пользователей?
- Применимы ли неподдерживаемые политики в облачном развертывании?

Если решение MDM недоступно в коллекции приложений Azure AD, вы можете добавить его с помощью процесса, описанного в статье [Azure Active Directory integration with MDM](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm) (Интеграция Azure Active Directory с MDM). 

При совместном управлении можно использовать SCCM для управления определенными аспектами устройств, в то время как политики доставляются через платформу MDM. Microsoft Intune предоставляет возможность совместного управления с SCCM. Дополнительные сведения о совместном управлении для устройств Windows 10 см. в статье [что такое совместное управление?](https://docs.microsoft.com/configmgr/core/clients/manage/co-management-overview). Если вы используете продукт MDM, отличный от Intune, обратитесь к поставщику MDM, чтобы узнать о применимых сценариях совместного управления.

**Рекомендация.** Рассмотрите возможность управления только с помощью MDM для устройств, подключенных к Azure AD.

## <a name="understand-considerations-for-applications-and-resources"></a>Рекомендации для приложений и ресурсов

Рекомендуем перенести приложения из локальной среды в облако, чтобы повысить удобство работы пользователей и управления доступом. Однако подключенные к Azure AD устройства могут легко предоставлять доступ как к локальным, так и к облачным приложениям. Дополнительные сведения см. в статье [How SSO to on-premises resources works on Azure AD joined devices](azuread-join-sso.md) (Как единый вход для локальных ресурсов работает на подключенных к Azure AD устройствах).

В следующих разделах представлены рекомендации для различных типов приложений и ресурсов.

### <a name="cloud-based-applications"></a>Облачные приложения

Если приложение добавлено в коллекцию приложений Azure AD, пользователи получают возможность единого входа с устройств, присоединенных к Azure AD. Дополнительная настройка не требуется. Пользователям доступен единый вход через браузеры Microsoft Edge и Chrome. Для Chrome необходимо развернуть [расширение Windows 10 Accounts](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). 

Все приложения Win32, которые:

- используют диспетчер учетных веб-записей (WAM) для запросов маркеров, также получают возможность единого входа на устройствах подключенных к Azure AD; 
- не используют WAM, могут запрашивать проверку подлинности пользователей. 

### <a name="on-premises-web-applications"></a>Локальные веб-приложения

Если ваши приложения разработаны по заказу и/или размещаются в локальной среде, то их необходимо добавить в список доверенных сайтов браузера, чтобы:

- обеспечить работу встроенной проверки подлинности Windows; 
- предоставить пользователям возможность единого входа без запроса. 

Если вы используете AD FS, см. статью [Проверка единого входа в службы федерации Active Directory и управление им](https://docs.microsoft.com/previous-versions/azure/azure-services/jj151809(v%3dazure.100)). 

**Рекомендация.** Рассмотрите возможность размещения в облаке (например, Azure) и интеграции с Azure AD для большего удобства.

### <a name="on-premises-applications-relying-on-legacy-protocols"></a>Локальные приложения, использующие устаревшие протоколы

Пользователи получают возможность единого входа с присоединенных к Azure AD устройств, у которых есть доступ к контроллеру домена. 

**Рекомендация.** Разверните [прокси-сервер Azure AD App](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy), чтобы обеспечить безопасный доступ для этих приложений.

### <a name="on-premises-network-shares"></a>Локальные сетевые папки

Пользователи получают возможность единого входа с подключенных к Azure AD устройств, у которых есть доступ к локальному контроллеру домена.

### <a name="printers"></a>Принтеры

Для принтеров необходимо развернуть [гибридную облачную службу печати](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy), чтобы обнаруживать принтеры на присоединенных к Azure AD устройствах. 

Принтеры невозможно автоматически обнаруживать в облачной среде, но пользователи также могут добавлять их напрямую с помощью UNC-пути. 

### <a name="on-premises-applications-relying-on-machine-authentication"></a>Локальные приложения, использующие проверку подлинности компьютера

Присоединенные к Azure AD устройства не поддерживают локальные приложения, использующие проверку подлинности компьютера. 

**Рекомендация.** Рекомендуем прекратить использование этих приложений и перейти на их современные аналоги.

### <a name="remote-desktop-services"></a>Службы удаленных рабочих столов

Для подключения к удаленным рабочим столам на устройствах, подключенных к Azure AD, хост-компьютер должен быть присоединен либо к Azure AD, либо к гибридной службе Azure AD. Удаленные рабочие столы устройств, которые не присоединены к этой службе или на которых не установлена ОС Windows, не поддерживаются. Дополнительные сведения см. в статье [Подключение к удаленному компьютеру, присоединенному к Azure Active Directory](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc)

## <a name="understand-your-provisioning-options"></a>Изучение вариантов подготовки

Вы можете подготовить к работе присоединение к Azure AD, используя описанные ниже подходы.

- **Самообслуживание в OOBE или параметрах.** В режиме самообслуживания пользователи выполняют присоединение к Azure AD через интерфейс запуска при первом включении компьютера с Windows (OOBE) или через параметры Windows. Дополнительные сведения см. в статье [Присоединение рабочего устройства к сети организации](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network). 
- **Windows Autopilot** позволяет заранее настраивать устройства, чтобы выполнять присоединение к Azure AD через OOBE было проще. Дополнительные сведения см. в статье [Общие сведения о Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot). 
- **Массовая регистрация** позволяет администратору выполнить присоединение к Azure AD, настраивая устройства с помощью средства массовой подготовки. Дополнительные сведения см. в статье [Массовая регистрация для устройств Windows](https://docs.microsoft.com/intune/windows-bulk-enroll).
 
Ниже приведено сравнение этих трех подходов. 
 
|   | Самостоятельная настройка | Windows Autopilot | Массовая регистрация |
| --- | --- | --- | --- |
| Для настройки требуется участие пользователя | Да | Да | Нет |
| Требуется участие ИТ-персонала | Нет | Да | Да |
| Применимые потоки | OOBE и параметры | Только OOBE | Только OOBE |
| Права локального администратора для основного пользователя | Да, по умолчанию | Настраивается | Нет |
| Требуется поддержка изготовителя устройства | Нет | Да | Нет |
| Поддерживаемые версии | 1511+ | 1709+ | 1703+ |
 
Чтобы выбрать подходы к развертыванию, изучите таблицу выше и ознакомьтесь с приведенными ниже рекомендациями по их реализации.  

- Ваши пользователи достаточно квалифицированы, чтобы выполнить настройку самостоятельно? 
   - Для таких пользователей лучше всего подойдет самообслуживание. Вы также можете упростить настройку при помощи Windows Autopilot.  
- Ваши пользователи работают удаленно или в здании предприятия? 
   - Удобную настройку для удаленных пользователей лучше всего обеспечивает самообслуживание или Autopilot. 
- Вы предпочитаете, чтобы настройку выполняли пользователи или администраторы? 
   - Если развертывание выполняет администратор, оптимальным вариантом будет массовая регистрация, позволяющая настроить устройства, прежде чем передавать их пользователям.     
- Вы приобретаете устройства у 1–2 изготовителей или используете широкий ассортимент устройств от различных поставщиков?  
   - Если вы покупаете устройства у небольшого количества изготовителей, которые также поддерживают Autopilot, то вам может быть полезна более тесная интеграция с Autopilot. 

## <a name="configure-your-device-settings"></a>Настройка параметров устройства

На портале Azure можно управлять развертыванием подключенных к Azure AD устройств в организации. Чтобы настроить соответствующие параметры, на **странице Azure Active Directory** выберите `Devices > Device settings`.

### <a name="users-may-join-devices-to-azure-ad"></a>Пользователи могут присоединять устройства к Azure AD

Задайте для этого параметра значение **Все** или **Выбранные** в зависимости от области развертывания и от того, каким сотрудникам требуется разрешать настройку устройства, подключенного к Azure AD. 

![Пользователи могут присоединять устройства к Azure AD](./media/azureadjoin-plan/01.png)

### <a name="additional-local-administrators-on-azure-ad-joined-devices"></a>Дополнительные локальные администраторы на устройствах, присоединенных к Azure AD

Нажмите **Выбранные** и выберите пользователей, которых требуется добавить в группу локальных администраторов на всех устройствах, присоединенных к Azure AD. 

![Дополнительные локальные администраторы на устройствах, присоединенных к Azure AD](./media/azureadjoin-plan/02.png)

### <a name="require-multi-factor-auth-to-join-devices"></a>Обязательная многофакторная проверка подлинности для присоединения устройств

Выберите **Да**, если пользователям необходимо проходить MFA для присоединения устройств к Azure AD. Для пользователей, присоединяющих устройства к Azure AD с помощью MFA, вторым фактором становится само устройство.

![Обязательная многофакторная проверка подлинности для присоединения устройств](./media/azureadjoin-plan/03.png)

## <a name="configure-your-mobility-settings"></a>Настройка параметров мобильности

Прежде чем настраивать параметры мобильности, может потребоваться добавить поставщика MDM.

**Добавление поставщика MDM**:

1. На странице **Azure Active Directory** в разделе **Управление** щелкните `Mobility (MDM and MAM)`. 
1. Нажмите **Добавить приложение**.
1. Выберите поставщика MDM из списка.

   ![Добавление приложения](./media/azureadjoin-plan/04.png)

Выберите поставщика MDM и настройте соответствующие параметры. 

### <a name="mdm-user-scope"></a>Область пользователей MDM

Выберите **Некоторые** или **Все** в зависимости от области развертывания. 

![Область пользователей MDM](./media/azureadjoin-plan/05.png)

В зависимости от области происходит одно из следующих событий: 

- **Пользователь находится в области MDM.** Если у вас есть подписка Azure AD Premium, регистрация MDM выполняется автоматически в ходе присоединения к Azure AD. У всех пользователей в области должна быть подходящая лицензия для MDM. Если регистрация MDM в этом сценарии не удастся, присоединение к Azure AD также будет отменено.
- **Пользователь не входит в область MDM.** Если пользователи находятся вне области MDM, присоединение к Azure AD выполняется без регистрации MDM. В результате устройство становится неуправляемым.

### <a name="mdm-urls"></a>URL-адреса MDM

С конфигурацией MDM связано URL-адреса:

- URL-адрес условий использования MDM;
- URL-адрес обнаружения MDM; 
- URL-адрес соответствия MDM.

![Добавление приложения](./media/azureadjoin-plan/06.png)

У каждого URL-адреса есть заранее определенное значение по умолчанию. Если эти поля пусты, обратитесь к поставщику MDM для получения дополнительных сведений.

### <a name="mam-settings"></a>Параметры MAM

MAM не применяется для присоединения к Azure AD. 

## <a name="configure-enterprise-state-roaming"></a>Настройка службы Enterprise State Roaming

Если вы хотите включить роуминг состояния в Azure AD, чтобы пользователи могли синхронизировать свои параметры между устройствами, см. статью [Включение службы Enterprise State Roaming в Azure Active Directory](enterprise-state-roaming-enable.md). 

**Рекомендация.** Включайте этот параметр даже для устройств, присоединенных к гибридной службе Azure AD.

## <a name="configure-conditional-access"></a>Настройка условного доступа

Если для устройств, присоединенных к Azure AD, настроен поставщик MDM, то он отмечает устройство как соответствующее требованиям, как только для него включается управление. 

![Устройства, соответствующие требованиям](./media/azureadjoin-plan/46.png)

Эту реализацию можно использовать для использования [управляемых устройств для доступа к облачным приложениям с помощью условного доступа](../conditional-access/require-managed-devices.md).

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство по присоединению нового устройства с Windows 10 с помощью Azure AD во время первого запуска](azuread-joined-devices-frx.md)
> [Присоединение рабочего устройства к сети организации](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network)

<!--Image references-->
[1]: ./media/azureadjoin-plan/12.png
