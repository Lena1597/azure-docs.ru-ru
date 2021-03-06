---
title: Руководство по Создание политики WAF для Azure Front Door при помощи портала Azure
description: Из этого руководства вы узнаете, как создать политику брандмауэра веб-приложения с помощью портала Azure.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 09/07/2019
ms.author: victorh
ms.openlocfilehash: 991111e01713afe48355aac44a151b98fa828c5f
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186721"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Руководство по Создание политики брандмауэра веб-приложения для Azure Front Door на портале Azure

В этом руководстве показано, как создать простейшую политику брандмауэра веб-приложения (WAF) и применить ее к интерфейсному узлу в Azure Front Door.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * создание политики WAF;
> * связывание ее с интерфейсным узлом;
> * настройка правил WAF.

## <a name="prerequisites"></a>Предварительные требования

Создайте профиль Front Door, следуя инструкциям, описанным в статье [Краткое руководство. Создание профиля Front Door для высокодоступного глобального веб-приложения](../../frontdoor/quickstart-create-front-door.md). 

## <a name="create-a-web-application-firewall-policy"></a>Создание политики брандмауэра веб-приложения

Сначала создайте на портале базовую политику WAF с управляемым набором правил по умолчанию (DRS). 

1. В левой верхней части экрана выберите **Создать ресурс**, выполните поиск по запросу **WAF**, выберите элемент **Брандмауэр веб-приложения (предварительная версия)** и щелкните **Создать**.
2. На вкладке **Основные сведения** страницы **Создание политики WAF** введите или выберите указанные ниже сведения, примите значения по умолчанию для остальных параметров и нажмите кнопку **Проверить и создать**.

    | Параметр                 | Значение                                              |
    | ---                     | ---                                                |
    | Subscription            |Выберите имя своей подписки Front Door.|
    | группа ресурсов.          |Выберите имя группы ресурсов Front Door.|
    | Имя политики             |Введите уникальное имя для политики WAF.|

   ![Создание политики WAF](../media/waf-front-door-create-portal/basic.png)

3. На вкладке **Ассоциация** страницы **Создание политики WAF** выберите **Добавить интерфейсный узел**, введите указанные ниже сведения, а затем нажмите кнопку **Добавить**.

    | Параметр                 | Значение                                              |
    | ---                     | ---                                                |
    | Front Door              | Выберите имя профиля Front Door.|
    | Узел внешнего интерфейса           | Выберите имя узла Front Door и нажмите кнопку **Добавить**.|
    
    > [!NOTE]
    > Если интерфейсный узел связан с политикой WAF, он недоступен для выбора. Сначала необходимо удалить интерфейсный узел из связанной политики, а затем повторно связать его с новой политикой WAF.
1. Выберите **Проверить и создать**, а затем выберите **Создать**.

## <a name="configure-web-application-firewall-rules-optional"></a>Настройка правил брандмауэра веб-приложения (необязательно)

### <a name="change-mode"></a>Изменение режима

После создании политики WAF она по умолчанию находится в режиме **обнаружения**. В режиме **обнаружения** WAF не блокирует запросы. Вместо этого запросы, соответствующие правилам WAF, регистрируются в журналах WAF.
Чтобы увидеть WAF в действии, можно изменить режим с **Обнаружение** на **Предотвращение**. В режиме **предотвращения** запросы, соответствующие правилам, которые определены в наборе правил по умолчанию (DRS), блокируются и регистрируются в журналах WAF.

 ![Изменение режима политики WAF](../media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Настраиваемые правила

Вы можете создать пользовательское правило, выбрав команду **Добавить настраиваемое правило** в разделе **Настраиваемые правила**. Откроется страница настройки пользовательского правила. Ниже приведен пример настройки пользовательского правила для блокировки запроса, если строка запроса содержит **blockme**.

![Изменение режима политики WAF](../media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Набор правил по умолчанию (DRS)

Набор правил по умолчанию, управляемый Azure, включен по умолчанию. Чтобы отключить отдельное правило в группе правил, разверните правила в этой группе, установите **флажок** перед номером правила и щелкните **Отключить** на вкладке выше. Чтобы изменить типы действий для отдельных правил в наборе правил, установите флажок перед номером правила, а затем выберите вкладку **Изменение действия** выше.

 ![Изменение набора правил WAF](../media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Сведения о брандмауэре веб-приложения Azure](../overview.md)
> [Дополнительные сведения об Azure Front Door](../../frontdoor/front-door-overview.md)