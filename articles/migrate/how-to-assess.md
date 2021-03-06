---
title: Добавление средств оценки в службу "миграция Azure"
description: Узнайте, как добавить средства оценки в службу "миграция Azure".
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.manager: carmonm
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: 64af78abd8f82b41d4a03fbb56c96e3038cef5a5
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185913"
---
# <a name="add-assessment-tools"></a>Добавление средств оценки

В этой статье описывается добавление средств оценки в службу " [Миграция Azure](migrate-overview.md)".

Служба "миграция Azure" предоставляет центр средств для оценки и миграции в Azure. Сюда входят средства миграции Azure, а также другие средства и независимые поставщики программного обеспечения (ISV).

Если вы хотите добавить средство оценки и у вас еще нет проекта службы "миграция Azure", следуйте указаниям в этой [статье](how-to-add-tool-first-time.md).

## <a name="select-a-tool"></a>Выбор средства

Если вы выбираете средство для оценки, не предназначенное для миграции Azure, начните с получения лицензии или регистрации для получения бесплатной пробной версии в соответствии с политикой средства. Средства могут подключаться к службе "миграция Azure". Следуйте инструкциям и документации, чтобы подключить средство к службе "миграция Azure". Дополнительные [сведения](migrate-services-overview.md) о средствах.


## <a name="select-an-assessment-scenario"></a>Выбор сценария оценки

1. В проекте службы "Миграция Azure" щелкните **Обзор**.
2. Выберите сценарий оценки, который вы хотите использовать:

    - Чтобы найти и оценить компьютеры и рабочие нагрузки для миграции в Azure, выберите **Оценка и миграция серверов**.
    - Для оценки локальных компьютеров SQL выберите **Оценка и миграция баз данных**.
    - Чтобы оценить локальные веб-приложения, выберите **Оценка и миграция веб-приложений**.

    ![Сценарий оценки](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Выбор средства оценки сервера 

1. Щелкните **Оценка и миграция серверов**.
2. Если вы еще не добавили средство **оценки в разделе** **средства оценки**, выберите **щелкните здесь, чтобы добавить средство оценки**. Если вы уже добавили средства оценки, в **меню добавить дополнительные средства оценки**выберите **изменить**.

    > [!NOTE]
    > Если вам нужно перейти к другому проекту, в разделе " **Миграция Azure — серверы**" рядом с полем **сведения о другом проекте миграции**щелкните **щелкните здесь**.

3. В **службе "миграция Azure**" выберите средство оценки, которое вы хотите использовать.

    - При использовании оценки сервера Azure для миграции можно настроить, запустить и просмотреть оценки непосредственно в проекте службы "миграция Azure".
    - При использовании другого средства оценки перейдите по ссылке, предоставленной для своего сайта, и выполните оценку в соответствии с предоставленными ими инструкциями.


## <a name="select-a-database-assessment-tool"></a>Выбор средства оценки базы данных

1. Щелкните **Оценка и миграция баз данных**
2. В окне **базы данных**щелкните **Добавить средства**.
3. В окне Добавление средства > **Выбор средства оценки**выберите средство, которое будет использоваться для оценки базы данных.

## <a name="select-a-web-app-assessment-tool"></a>Выберите средство оценки веб-приложений

1. Щелкните **Оценка и миграция веб-приложений**.
2. Перейдите по ссылке на средство миграции для службы приложений Azure. Используйте средство миграции, чтобы:

    - **Оценка приложений в сети**. Вы можете оценить приложения с общедоступным URL-адресом в Интернете, используя службу приложений Azure помощник по миграции.
    - **.NET/PHP**. для внутренних приложений .NET и PHP можно скачать и запустить помощник по миграции.



## <a name="next-steps"></a>Дополнительная информация

Оцените оценку использования Azure Migrate Server для виртуальных машин [VMware](tutorial-prepare-vmware.md) , [Hyper-V](tutorial-prepare-hyper-v.md)или [физических серверов](tutorial-prepare-physical.md)
