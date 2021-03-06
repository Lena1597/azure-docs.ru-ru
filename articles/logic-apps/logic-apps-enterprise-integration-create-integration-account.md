---
title: Создание учетных записей интеграции B2B или управление ими
description: Создание, связывание и управление учетными записями интеграции для интеграции Enterprise с Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: 3cdabbd5f527934492ce7ff37ae7d0f756d91fc1
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979419"
---
# <a name="create-and-manage-integration-accounts-for-b2b-enterprise-integrations-in-azure-logic-apps"></a>Создание учетных записей интеграции для интеграции B2B Enterprise в Azure Logic Apps и управление ими

Чтобы создавать [решения для корпоративной интеграции и B2B](../logic-apps/logic-apps-enterprise-integration-overview.md) с помощью [Azure Logic Apps](../logic-apps/logic-apps-overview.md), необходимо создать учетную запись интеграции. По сути, это отдельный ресурс Azure, предоставляющий защищенный, масштабируемый и управляемый контейнер для артефактов интеграции, которые вы определяете и используете в рабочих процессах приложения логики.

Например, можно создавать, хранить артефакты B2B, а также управлять ими, например торговыми партнерами, соглашениями, картами, схемами, сертификатами и пакетными конфигурациями. Кроме того, прежде чем приложение логики сможет работать с этими артефактами и использовать соединители Logic Apps B2B, необходимо [связать учетную запись интеграции](#link-account) с приложением логики. Учетная запись интеграции и приложение логики должны находиться в *одном* расположении или регионе.

> [!TIP]
> Сведения о создании учетной записи интеграции в [среде службы Integration](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Services см. [в разделе Создание учетных записей интеграции в ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#create-integration-account-environment).

В этом разделе показано, как выполнять следующие задачи:

* создание учетной записи интеграции;
* привязка учетной записи интеграции к приложению логики;
* Измените ценовую категорию для учетной записи интеграции.
* Отменить связь учетной записи интеграции с приложением логики.
* перемещение учетной записи интеграции в другую подписку или группу ресурсов Azure;
* удаление учетной записи интеграции.

## <a name="prerequisites"></a>Технические условия

* Подписка Azure. Если у вас еще нет подписки Azure, [зарегистрируйтесь для получения бесплатной учетной записи Azure](https://azure.microsoft.com/free/).

## <a name="create-integration-account"></a>Создание учетной записи интеграции

Для этой задачи можно использовать либо портал Azure, выполнив действия, описанные в этом разделе, [Azure PowerShell](/powershell/module/Az.LogicApp/New-AzIntegrationAccount)или [Azure CLI](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-create).

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетных данных учетной записи Azure.

1. В главном меню Azure выберите команду **Создать ресурс**. В поле поиска введите "учетная запись интеграции" в качестве фильтра и выберите **учетная запись интеграции**.

   ![Создать новую учетную запись интеграции](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

1. В разделе **учетная запись интеграции**выберите **создать**.

   ![Выбор элемента "Добавить" для создания учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

1. Укажите следующие сведения о вашей учетной записи интеграции:

   ![Укажите сведения об учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Свойство | Обязательно для заполнения | Значение | Description |
   |----------|----------|-------|-------------|
   | **Название** | Да | <*integration-account-name*> | Имя учетной записи интеграции, которое может содержать только буквы, цифры, дефисы (`-`), символы подчеркивания (`_`), круглые скобки (`(`, `)`) и точки (`.`). В этом примере используется "Fabrikam-Integration". |
   | **подписка** | Да | <*Azure-subscription-name*> | Имя подписки Azure. |
   | **группа ресурсов** | Да | <*имя_группы_ресурсов_Azure*> | Имя [группы ресурсов Azure](../azure-resource-manager/management/overview.md) , используемой для организации связанных ресурсов. В этом примере создайте новую группу ресурсов с именем "Фабрикаминтегратион-RG". |
   | **Ценовая категория** | Да | <> *уровня ценообразования* | Ценовая категория для учетной записи интеграции, которую можно изменить позже. В этом примере выберите **Free (бесплатный**). Дополнительные сведения см. в следующих статьях: <p>[модель ценообразования для Logic Apps](../logic-apps/logic-apps-pricing.md#integration-accounts) -  <p>[ограничения и конфигурация - Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) <p>[цены на Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) -  |
   | **Местоположение** | Да | <*Azure-region*> | Регион, в котором хранятся метаданные учетной записи интеграции. Выберите то же расположение, что и приложение логики, или создайте приложения логики в том же расположении, что и учетная запись интеграции. В этом примере используйте "Западная часть США". <p>**Примечание**. чтобы создать учетную запись интеграции в [среде службы интеграции (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), выберите эту интегрированную среду сценариев в качестве расположения. Дополнительные сведения см. [в разделе Создание учетных записей интеграции в интегрированной среде сценариев](../logic-apps/add-artifacts-integration-service-environment-ise.md#create-integration-account-environment). |
   | **Служба Log Analytics** | Нет | Выкл., вкл. | Для этого примера не устанавливайте параметр **Off** . |
   |||||

1. По завершении нажмите кнопку **создать**.

   После завершения развертывания Azure откроет учетную запись интеграции.

   ![Azure открывает учетную запись интеграции](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

1. Прежде чем приложение логики сможет использовать учетную запись интеграции, выполните следующие действия, чтобы связать учетную запись интеграции и приложение логики вместе.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Привязка к приложению логики

Чтобы предоставить приложениям логики доступ к учетной записи интеграции, содержащей артефакты B2B, необходимо сначала связать учетную запись интеграции с приложением логики. Как приложение логики, так и учетная запись интеграции должны находиться в одном регионе. Для выполнения этой задачи можно использовать портал Azure. Если вы используете Visual Studio и приложение логики находится в [проекте группы ресурсов Azure](../azure-resource-manager/templates/create-visual-studio-deployment-project.md), вы можете [связать приложение логики с учетной записью интеграции с помощью Visual Studio](../logic-apps/manage-logic-apps-with-visual-studio.md#link-integration-account).

1. На портале Azure найдите и откройте нужное приложение логики.

1. В [портал Azure](https://portal.azure.com)откройте существующее приложение логики или создайте новое приложение логики.

1. В меню приложения логики в разделе **Параметры** выберите **Параметры рабочего процесса**. В разделе **учетная запись интеграции**откройте список **выберите учетную запись интеграции** . Выберите учетную запись интеграции, чтобы связать приложение логики.

   ![Выбор учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/select-integration-account.png)

1. Чтобы завершить связывание, нажмите кнопку **сохранить**.

   ![Выбор учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/save-link.png)

   После успешного связывания учетной записи интеграции Azure выводит сообщение с подтверждением.

   ![Подтверждение успешной привязки в Azure](./media/logic-apps-enterprise-integration-create-integration-account/link-confirmation.png)

Теперь приложение логики может использовать артефакты в учетной записи интеграции и соединители B2B, такие как проверка XML и кодирование неструктурированных файлов или декодирование.  

<a name="change-pricing-tier"></a>

## <a name="change-pricing-tier"></a>Смена ценовой категории

Чтобы увеличить [ограничения](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) для учетной записи интеграции, можно [выполнить обновление до более высокой ценовой](#upgrade-pricing-tier)категории, если она доступна. Например, можно выполнить обновление с уровня "бесплатный" до уровня "базовый" или "Стандартный". Можно также [Перейти на более низкий уровень](#downgrade-pricing-tier), если он доступен. Дополнительные сведения о ценах см. в следующих разделах:

* [Цены на приложения логики](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Модель ценообразования Logic Apps](../logic-apps/logic-apps-pricing.md#integration-accounts)

<a name="upgrade-pricing-tier"></a>

### <a name="upgrade-pricing-tier"></a>Обновить ценовую категорию

Чтобы внести это изменение, можно использовать либо портал Azure, выполнив действия, описанные в этом разделе, или [Azure CLI](#upgrade-tier-azure-cli).

#### <a name="azure-portal"></a>Портал Azure

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетных данных учетной записи Azure.

1. В основном поле поиска Azure введите "учетные записи интеграции" в качестве фильтра и выберите **учетные записи интеграции**.

   ![Поиск учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure отображает все учетные записи интеграции в подписках Azure.

1. В разделе **Учетные записи интеграции** выберите учетную запись интеграции, которую необходимо переместить. В меню учетной записи интеграции выберите **Обзор**.

   ![В меню учетной записи интеграции выберите "Обзор".](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. В области Обзор выберите **Обновить ценовую категорию**, в которой перечислены все доступные более высокие уровни. При выборе уровня изменение вступает в силу немедленно.

<a name="upgrade-tier-azure-cli"></a>

#### <a name="azure-cli"></a>Интерфейс командной строки Azure

1. [Установите Azure CLI необходимые компоненты](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), если это еще не сделано.

1. В портал Azure откройте среду [**Cloud Shell**](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) Azure.

   ![Открытие Azure Cloud Shell](./media/logic-apps-enterprise-integration-create-integration-account/open-azure-cloud-shell-window.png)

1. В командной строке введите [команду **AZ Resource** ](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-update)и задайте для `skuName` более высокий уровень.

   ```Azure CLI
   az resource update --resource-group {ResourceGroupName} --resource-type Microsoft.Logic/integrationAccounts --name {IntegrationAccountName} --subscription {AzureSubscriptionID} --set sku.name={SkuName}
   ```
  
   Например, если имеется уровень "базовый", можно задать для `skuName` значение `Standard`.

   ```Azure CLI
   az resource update --resource-group FabrikamIntegration-RG --resource-type Microsoft.Logic/integrationAccounts --name Fabrikam-Integration --subscription XXXXXXXXXXXXXXXXX --set sku.name=Standard
   ```

<a name="downgrade-pricing-tier"></a>

### <a name="downgrade-pricing-tier"></a>Ценовая категория понижения уровня

Чтобы внести это изменение, используйте [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).

1. [Установите Azure CLI необходимые компоненты](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), если это еще не сделано.

1. В портал Azure откройте среду [**Cloud Shell**](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) Azure.

   ![Открытие Azure Cloud Shell](./media/logic-apps-enterprise-integration-create-integration-account/open-azure-cloud-shell-window.png)

1. В командной строке введите [команду **AZ Resource** ](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-update) и задайте для `skuName` более низкий уровень.

   ```Azure CLI
   az resource update --resource-group <resourceGroupName> --resource-type Microsoft.Logic/integrationAccounts --name <integrationAccountName> --subscription <AzureSubscriptionID> --set sku.name=<skuName>
   ```
  
   Например, если имеется уровень Standard, можно задать для `skuName` значение `Basic`.

   ```Azure CLI
   az resource update --resource-group FabrikamIntegration-RG --resource-type Microsoft.Logic/integrationAccounts --name Fabrikam-Integration --subscription XXXXXXXXXXXXXXXXX --set sku.name=Basic
   ```

## <a name="unlink-from-logic-app"></a>Удаление привязки к приложению логики

Если вы хотите связать приложение логики с другой учетной записью интеграции или больше не используете учетную запись интеграции с приложением логики, удалите ссылку с помощью обозреватель ресурсов Azure.

1. Откройте окно браузера и перейдите в [Обозреватель ресурсов Azure (https://resources.azure.com)](https://resources.azure.com). Войдите, используя те же учетные данные Azure.

   ![Обозреватель ресурсов Azure](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

1. В поле поиска введите имя приложения логики, чтобы можно было найти и выбрать приложение логики.

   ![Поиск и выбор приложения логики](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

1. В строке заголовка обозревателя выберите **чтение и запись**.

   ![Включение режима "Чтение и запись"](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-select-read-write.png)

1. На вкладке **данные** выберите **изменить**.

   ![На вкладке "данные" выберите "Изменить".](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-select-edit.png)

1. В редакторе найдите объект `integrationAccount` и удалите это свойство, которое имеет следующий формат:

   ```json
   {
      // <other-attributes>
      "integrationAccount": {
         "name": "<integration-account-name>",
         "id": "<integration-account-resource-ID>",
         "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Пример.

   ![Найти объект "integrationAccount"](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

1. На вкладке **данные** нажмите кнопку **Вставить** , чтобы сохранить изменения.

   ![Чтобы сохранить изменения, выберите "разместить".](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

1. В портал Azure найдите и выберите свое приложение логики. В разделе **Параметры рабочего процесса**приложения убедитесь, что свойство **учетная запись интеграции** теперь отображается как пустое.

   ![Проверка на предмет отсутствия привязки у учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Перемещение учетной записи интеграции

Вы можете переместить учетную запись интеграции в другую группу ресурсов Azure или подписку Azure. При перемещении ресурсов Azure создает новые идентификаторы ресурсов, поэтому убедитесь, что вместо них используются новые идентификаторы, и обновите все сценарии или средства, связанные с перемещенными ресурсами. Если вы хотите изменить подписку, необходимо также указать существующую или новую группу ресурсов.

Для этой задачи можно использовать либо портал Azure, выполнив действия, описанные в этом разделе, или [Azure CLI](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-move).

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетных данных учетной записи Azure.

1. В основном поле поиска Azure введите "учетные записи интеграции" в качестве фильтра и выберите **учетные записи интеграции**.

   ![Поиск учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure отображает все учетные записи интеграции в подписках Azure.

1. В разделе **Учетные записи интеграции** выберите учетную запись интеграции, которую необходимо переместить. В меню учетной записи интеграции выберите **Обзор**.

   ![В меню учетной записи интеграции выберите "Обзор".](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. Рядом с именем **группы ресурсов** или **подписки**выберите **изменить**.

   ![Изменение группы ресурсов или подписки](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

1. Выберите связанные ресурсы, которые также необходимо переместить.

1. В зависимости от выбранного варианта выполните следующие действия, чтобы изменить группу ресурсов или подписку.

   * Группа ресурсов: в списке **Группа ресурсов** выберите целевую группу ресурсов. Или, чтобы создать другую группу ресурсов, выберите **создать новую группу ресурсов**.

   * Подписка: в списке **Подписка** выберите целевую подписку. В списке **Группа ресурсов** выберите целевую группу ресурсов. Или, чтобы создать другую группу ресурсов, выберите **создать новую группу ресурсов**.

1. Чтобы подтвердить, что все сценарии или инструменты, связанные с перемещенными ресурсами, не будут работать, пока вы не обновите их с помощью новых идентификаторов ресурсов, установите флажок подтверждения и нажмите кнопку **ОК**.

1. После завершения убедитесь, что все скрипты и все сценарии обновлены с учетом новых идентификаторов ресурсов для перемещенных ресурсов.  

## <a name="delete-integration-account"></a>Удаление учетной записи интеграции

Для этой задачи можно использовать либо портал Azure, выполнив действия, описанные в этом разделе, [Azure CLI](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-delete)или [Azure PowerShell](/powershell/module/az.logicapp/remove-azintegrationaccount).

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетных данных учетной записи Azure.

1. В основном поле поиска Azure введите "учетные записи интеграции" в качестве фильтра и выберите **учетные записи интеграции**.

   ![Поиск учетной записи интеграции](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure отображает все учетные записи интеграции в подписках Azure.

1. В разделе **Учетные записи интеграции** выберите учетную запись интеграции, которую необходимо удалить. В меню учетной записи интеграции выберите **Обзор**.

   ![В меню учетной записи интеграции выберите "Обзор".](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. В области Обзор выберите **Удалить**.

   ![На панели "Обзор" выберите "Удалить".](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

1. Чтобы подтвердить удаление учетной записи интеграции, выберите **Да**.

   ![Чтобы подтвердить удаление, выберите "Да".](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Дальнейшие действия

* [Создание торговых партнеров в учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Создание соглашений между партнерами в учетной записи интеграции](../logic-apps/logic-apps-enterprise-integration-agreements.md)
