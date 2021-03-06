---
title: Запрос на увеличение квоты
description: На этой странице описывается, как создать запрос в службу поддержки, чтобы увеличить квоты для отдельных баз данных, серверов и управляемых экземпляров базы данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 02/04/2020
ms.openlocfilehash: fb576b81adeec99e4080c744749097390d1add1d
ms.sourcegitcommit: 9add86fb5cc19edf0b8cd2f42aeea5772511810c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2020
ms.locfileid: "77111102"
---
# <a name="request-quota-increases-for-azure-sql-database"></a>Увеличение квот запросов для базы данных SQL Azure

В этой статье объясняется, как запросить увеличение квоты для базы данных SQL Azure для отдельных баз данных, серверов и управляемых экземпляров. В нем также объясняется, как включить доступ к подписке для региона.

## <a id="newquota"></a>Создать новый запрос в службу поддержки

Выполните следующие действия, чтобы создать новый запрос на поддержку из портал Azure для базы данных SQL.

1. В меню [портал Azure](https://portal.azure.com) выберите пункт **Справка и поддержка**.

   ![Ссылка "Справка и поддержка"](./media/quota-increase-request/help-plus-support.png)

1. В окне **Справка и поддержка**выберите **новый запрос в службу поддержки**.

    ![Создать новый запрос в службу поддержки](./media/quota-increase-request/new-support-request.png)

1. Для поля **Тип проблемы** выберите **Ограничения службы и подписки (квоты)** .

   ![Выберите тип проблемы](./media/quota-increase-request/select-quota-issue-type.png)

1. В поле **Подписка**выберите подписку, для которой необходимо увеличить квоту.

   ![Выберите подписку для повышения квоты](./media/quota-increase-request/select-subscription-support-request.png)

1. В поле **тип квоты**выберите один из следующих типов квот:

   - **База данных SQL** для квот эластичных баз данных и пулов.
   - **Управляемый экземпляр базы данных SQL** для управляемых экземпляров.

   Затем выберите **Далее: solutions > >** .

   ![Выберите тип квоты](./media/quota-increase-request/select-quota-type.png)

1. В окне **сведения** выберите **указать сведения** , чтобы ввести дополнительные сведения.

   ![Ссылка "предоставить сведения"](./media/quota-increase-request/provide-details-link.png)

Если щелкнуть **указать сведения** , откроется окно **сведения о квоте** , в котором можно добавить дополнительные сведения. В следующих разделах описаны различные варианты для **базы данных SQL** и типов квот **управляемый экземпляр базы данных SQL** .

## <a id="sqldbquota"></a>Типы квот базы данных SQL

В следующих разделах описаны три варианта увеличения квоты для типов квот **базы данных SQL** :

- Единиц транзакций базы данных (DTU) на сервер
- Серверов на подписку
- Включение доступа к подписке для региона

### <a name="database-transaction-units-dtus-per-server"></a>Единиц транзакций базы данных (DTU) на сервер

Чтобы запросить увеличение количества единиц DTU на сервер, выполните следующие действия.

1. Выберите тип квоты **единиц транзакций базы данных (DTU) на сервер** .

1. В списке **ресурс** выберите целевой ресурс.

1. В поле **Новая Квота** введите новое ограничение DTU, которое вы запрашиваете.

   ![Сведения о квоте DTU](./media/quota-increase-request/quota-details-dtus.png)

Дополнительные сведения см. [в разделе ограничения ресурсов для отдельных баз данных с использованием модели приобретения DTU](sql-database-dtu-resource-limits-single-databases.md) и [ограничений ресурсов для эластичных пулов с использованием модели приобретения DTU](sql-database-dtu-resource-limits-elastic-pools.md).

### <a name="servers-per-subscription"></a>Серверов на подписку

Чтобы запросить увеличение числа серверов на подписку, выполните следующие действия.

1. Выберите тип квоты " **серверов на подписку** ".

1. В списке **Расположение** выберите используемый регион Azure. Квота для каждой подписки в каждом регионе.

1. В поле **Новая Квота** введите запрос на максимальное количество серверов в этом регионе.

   ![Сведения о квоте серверов](./media/quota-increase-request/quota-details-servers.png)

Дополнительные сведения см. в статье [ограничения ресурсов базы данных SQL и управление ресурсами](sql-database-resource-limits-database-server.md).

### <a name="enable-subscription-access-to-a-region"></a>Включение доступа к подписке для региона

Некоторые типы предложений доступны не в каждом регионе. Может появиться следующее сообщение об ошибке:

`This location is not available for subscription`

Если вашей подписке требуется доступ в определенном регионе, используйте параметр **другой запрос квоты** для запроса доступа. В запросе укажите сведения о предложении и номере SKU, которые необходимо включить для региона. Сведения о вариантах предложения и SKU см. в разделе [цены на базу данных SQL Azure](https://azure.microsoft.com/pricing/details/sql-database/single/).

![Другие сведения о квоте](./media/quota-increase-request/quota-details-whitelisting.png)

## <a id="sqlmiquota"></a>Тип квоты управляемого экземпляра

Для SQL Server типа квоты **управляемый экземпляр** выполните следующие действия.

1. В списке **регион** выберите целевой регион Azure.

1. Введите новые ограничения, которые вы запрашиваете для **подсети** и **Виртуальное ядро**.

   ![Сведения о квоте управляемого экземпляра](./media/quota-increase-request/quota-details-managed-instance.png)

Дополнительные сведения см. в статье [Общие сведения об ограничениях ресурсов управляемого экземпляра базы данных SQL Azure](sql-database-managed-instance-resource-limits.md).

## <a name="submit-your-request"></a>Отправка запроса

Последним шагом является заполнение оставшихся сведений о запросе квоты базы данных SQL. Затем выберите **Далее: Проверка и создание > >** и после просмотра сведений о запросе нажмите кнопку **создать** , чтобы отправить запрос.

## <a name="next-steps"></a>Дальнейшие действия

После отправки запроса он будет проверен. Вы будете связываться с ответом на информацию, указанную в форме.

Дополнительные сведения о других ограничениях Azure см. в статье [Подписка Azure, пределы, квоты и ограничения службы](../azure-resource-manager/management/azure-subscription-service-limits.md).
