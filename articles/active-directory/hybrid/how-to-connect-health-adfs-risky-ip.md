---
title: Отчет Azure AD Connect Health с AD FS рискованным IP-адресом | Документация Майкрософт
description: Описание отчета Azure AD Connect Health AD FS рискованный IP-адрес.
services: active-directory
documentationcenter: ''
ms.reviewer: zhiweiwangmsft
author: billmath
manager: daveba
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: defdf8118f1b07f8d6ddc4d232cda0fc423ef9f6
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76897261"
---
# <a name="risky-ip-report-public-preview"></a>Отчет о рискованных IP-адресах (Предварительная версия)
Клиенты AD FS могут предоставлять конечные точки проверки пароля в Интернете, чтобы предоставлять службы проверки подлинности пользователям для получения доступа к приложениям SaaS, таким как Office 365. В этом случае вредоносный субъект может попытаться выполнить вход в систему AD FS, чтобы угадать пароль пользователя и получить доступ к ресурсам приложения. AD FS предоставляют функцию блокировки учетной записи экстрасети для предотвращения этих типов атак, начиная с AD FS в Windows Server 2012 R2. Если используется более ранняя версия, настоятельно рекомендуется обновить систему AD FS до Windows Server 2016. <br />

Кроме того, с одного IP-адреса возможно предпринять несколько попыток входа к нескольким пользователям. В этих случаях количество попыток на пользователя может быть ниже порогового значения для защитной блокировки учетной записи в AD FS. Azure Active Directory Connect Health теперь предоставляет "Отчет о ненадежном IP-адресе", который определяет это условие и уведомляет администраторов, когда это происходит. Ниже приведены ключевые преимущества этого отчета: 
- обнаружение IP-адресов, превышающих порог неудачных попыток входа на основе пароля;
- поддержка неудачных попыток входа из-за неправильного пароля или состояния блокировки экстрасети;
- уведомление по электронной почте для предупреждения администраторов о таком событии с помощью настраиваемых параметров электронной почты;
- настраиваемые параметры порога, которые соответствуют политике безопасности организации;
- скачиваемые отчеты для автономного анализа и интеграции с другими системами через автоматизацию.

> [!NOTE]
> Чтобы применять этот отчет, необходимо убедиться, что включен аудит AD FS. Дополнительные сведения см. в разделе [Включение аудита для AD FS](how-to-connect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Для доступа к предварительной версии требуются разрешения глобального администратора или [читателя сведений о безопасности](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader).  
> 

## <a name="what-is-in-the-report"></a>Что такое отчет?
IP-адреса клиента действия входа со сбоем, агрегированные через прокси-серверы веб приложения. Каждый элемент в отчете о ненадежном IP-адресе показывает статистические данные о неудачных действиях входа AD FS, превышающих указанный порог. Предоставляются следующие сведения: ![Портал Azure AD Connect Health](./media/how-to-connect-health-adfs/report4a.png).

| Элемент отчета | Description |
| ------- | ----------- |
| Метка времени | Показывает метку времени на основе локального времени портала Azure во время запуска окна времени обнаружения.<br /> Все ежедневные события создаются до полуночи в формате UTC. <br />У почасовых событий есть метка времени, округленная до начала часа. Время начала первого действия можно найти в firstAuditTimestamp в экспортированном файле. |
| Тип триггера | Показывает тип окна времени обнаружения. Есть почасовые и ежедневные типы триггеров агрегации. Это может быть полезно для обнаружения быстрых атак методом подбора и медленных атак, в которых число попыток распределяется на весь день. |
| IP-адрес | Один ненадежный IP-адрес, по которому выполнено одно из таких действий входа: неправильный пароль или блокировка экстрасети. Это может быть адрес IPv4 или IPv6. |
| Число ошибок с неправильным вводом пароля | Число ошибок с неправильным вводом пароля по IP-адресу в течение временного окна обнаружения. Ошибки с неправильным вводом пароля могут возникать несколько раз для определенных пользователей. Обратите внимание, что сюда не входят неудачные попытки из-за истечения срока действия паролей. |
| Число ошибок с блокировкой экстрасети | Число ошибок с блокировкой экстрасети по IP-адресу в течение временного окна обнаружения. Ошибки блокировки экстрасети могут происходить несколько раз для определенных пользователей. Они обнаруживаются, только если блокировка экстрасети настроена в AD FS (версии 2012 R2 или более поздние). <b>Примечание.</b> Настоятельно рекомендуется включить эту возможность, если вы разрешаете вход в экстрасеть с использованием паролей. |
| Попытки уникальных пользователей | Число попыток использования учетных записей уникальных пользователей по IP-адресу в течение временного окна обнаружения. При этом предоставляется механизм для различения шаблона атаки одного пользователя и нескольких пользователей.  |

Например, из элемента отчета ниже видно, что в часовом окне 18:00–19:00 28.02.2018 по IP-адресу <i>104.2XX.2XX.9</i> не было ошибок с неправильным вводом пароля и было 284 ошибки блокировки экстрасети. В критерии были включены 14 уникальных пользователей. Событие действия превысило установленное почасовое пороговое значение. 

![Azure AD Connect Health](./media/how-to-connect-health-adfs/report4b.png)

> [!NOTE]
> - В списке отчетов будут отображаться только действия, превышающие указанный порог. 
> - Этот отчет может отслеживаться до 30 дней.
> - В этом отчете об оповещениях не показываются IP-адреса Exchange или частные IP-адреса. Они по-прежнему включаются в список экспорта. 
>

![Azure AD Connect Health](./media/how-to-connect-health-adfs/report4c.png)

## <a name="load-balancer-ip-addresses-in-the-list"></a>IP-адреса балансировщика нагрузки в списке
Вычисления подсистемы балансировки нагрузки помешали действиям по выполнению входа и достигли порога оповещений. Если вы видите IP-адреса подсистемы балансировки нагрузки, очень вероятно, что внешняя подсистема балансировки нагрузки не отправляет IP-адрес клиента при передаче запроса прокси-серверу веб-приложения. Для отправки IP-адреса клиента необходимо правильно настроить подсистему балансировки нагрузки. 

## <a name="download-risky-ip-report"></a>Скачать отчет о рискованных IP-адресах 
С помощью функции **Загрузить** весь список ненадежных IP-адресов за прошедшие 30 дней можно экспортировать с портала Connect Health. Результат будет включать все завершившиеся со сбоем действия входа AD FS в каждом временном окне обнаружения, чтобы вы могли настроить фильтрацию после экспорта. Помимо выделенных агрегаций на портале в результатах экспорта также показаны дополнительные сведения о действиях входа, завершившихся со сбоем, по IP-адресам:

|  Элемент отчета  |  Description  | 
| ------- | ----------- | 
| firstAuditTimestamp | Показывает первую метку времени, когда завершившиеся со сбоем действия запускались во время окна времени обнаружения.  | 
| lastAuditTimestamp | Показывает последнюю метку времени, когда завершившиеся со сбоем действия завершились в окне времени обнаружения.  | 
| attemptCountThresholdIsExceeded | Флаг, определяющий превышение текущего порога предупреждения для текущих действий.  | 
| isWhitelistedIpAddress | Флаг, определяющий, что IP-адрес отфильтрован из оповещения и отчета. Частные IP-адреса (<i>10.x.x.x, 172.x.x.x & 192.168.x.x</i>) и IP-адреса Exchange отфильтровываются и помечаются как True. Если вы видите диапазоны частных IP-адресов, очень вероятно, что внешняя подсистема балансировки нагрузки не отправляет IP-адрес клиента при передаче запроса прокси-серверу веб-приложения.  | 

## <a name="configure-notification-settings"></a>Настройка параметров уведомлений
Контакты администратора отчета можно обновить с использованием **параметров уведомлений**. По умолчанию уведомление по электронной почте о ненадежном IP-адресе выключено. Его можно включить, переключив кнопку в разделе "Получать по электронной почте уведомления о превышении пороговых значений для неудавшихся действий на IP-адресах". Как и обычные параметры уведомлений об оповещении в Connect Health, с его помощью можно настроить выделенный список получателей уведомлений об отчете о ненадежных IP-адресах. Кроме того, можно уведомлять всех глобальных администраторов во время внесения изменений. 

## <a name="configure-threshold-settings"></a>Настройка параметров пороговых значений
Порог оповещений можно обновить с помощью параметров порогового значения. Для начала в системе есть пороговое значение, установленное по умолчанию. В параметрах порогового значения отчета о ненадежных IP-адресах есть четыре категории:

![Azure AD Connect Health](./media/how-to-connect-health-adfs/report4d.png)

| Элемент порогового значения | Description |
| --- | --- |
| (Неверное имя или пароль + блокировка экстрасети) / день  | Параметр порогового значения, позволяющий сообщить о действии и активировать уведомление об оповещении, когда число неправильных паролей и блокировок экстрасети превышает его значение за **день**. |
| (Неверное имя или пароль + блокировка экстрасети) / час | Параметр порогового значения, позволяющий сообщить о действии и активировать уведомление об оповещении, когда число неправильных паролей и блокировок экстрасети превышает его значение за **час**. |
| Блокировка экстрасети / день | Параметр порогового значения, позволяющий сообщить о действии и активировать уведомление об оповещении, когда число блокировок экстрасети превышает его значение за **день**. |
| Блокировка экстрасети / час| Параметр порогового значения, позволяющий сообщить о действии и активировать уведомление об оповещении, когда число блокировок экстрасети превышает его значение за **час**. |

> [!NOTE]
> - Изменение порогового значения в отчете произойдет через час после изменения параметра. 
> - Имеющиеся элементы отчета не будут затронуты изменением порогового значения. 
> - Рекомендуем проанализировать количество событий, наблюдаемых в вашей среде, и соответствующим образом настроить пороговые значения. 
>
>

## <a name="faq"></a>Часто задаваемые вопросы
**Почему в отчете отображаются диапазоны частных IP-адресов?**  <br />
Частные IP-адреса (<i>10.x.x.x, 172.x.x.x & 192.168.x.x</i>) и IP-адреса Exchange отфильтровываются и помечаются как True в списке разрешенных IP-адресов. Если вы видите диапазоны частных IP-адресов, очень вероятно, что внешняя подсистема балансировки нагрузки не отправляет IP-адрес клиента при передаче запроса прокси-серверу веб-приложения.

**Почему в отчете отображаются IP-адреса подсистемы балансировки нагрузки?**  <br />
Если вы видите IP-адреса подсистемы балансировки нагрузки, очень вероятно, что внешняя подсистема балансировки нагрузки не отправляет IP-адрес клиента при передаче запроса прокси-серверу веб-приложения. Для отправки IP-адреса клиента необходимо правильно настроить подсистему балансировки нагрузки. 

**Как заблокировать IP-адрес?**  <br />
Следует добавить идентифицированный вредоносный IP-адрес в брандмауэр или заблокировать в Exchange.   <br />

**Почему я не вижу никаких элементов в этом отчете?** <br />
- Ошибки при действиях входа не превышают параметры пороговых значений.
- Убедитесь, что в списке серверов AD FS нет активного оповещения "Служба работоспособности не обновлена".  Дополнительные сведения см. в [этой статье](how-to-connect-health-data-freshness.md).
- На фермах AD FS отключен аудит.

**Почему я не вижу доступа к отчету?**  <br />
Для доступа требуются разрешения глобального администратора или [читателя сведений о безопасности](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader). Для получения доступа обратитесь к администратору.


## <a name="next-steps"></a>Дальнейшие действия
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Установка агента Azure AD Connect Health](how-to-connect-health-agent-install.md)
