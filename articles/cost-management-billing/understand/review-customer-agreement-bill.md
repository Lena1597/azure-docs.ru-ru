---
title: Просмотр счета в рамках Клиентского соглашения в Azure
description: Узнайте, как просматривать счет и использование ресурсов, а затем проверять расходы по счету Клиентского соглашения Майкрософт.
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2019
ms.author: banders
ms.openlocfilehash: 961eef939684b1bfb4ebe1840ff87e7bd836f205
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77121778"
---
# <a name="tutorial-review-your-microsoft-customer-agreement-invoice"></a>Руководство. Просмотр счета в рамках Клиентского соглашения Майкрософт

Вы можете проверить расходы, включенные в счет, проанализировав отдельные транзакции. В учетной записи выставления счетов для Клиентского соглашения Майкрософт в каждом профиле выставления счетов ежемесячно создается счет. В счет входят все расходы за предыдущий месяц. Вы можете просмотреть счета на портале Azure и сравнить расходы в файле сведений об использовании.

Это руководство относится только к клиентам Azure с Клиентским соглашением Майкрософт.

В этом руководстве описано следующее.

> [!div class="checklist"]
> * Проверка выставленных счетов за транзакции на портале Azure
> * Просмотр ожидаемых расходов для прогнозирования следующего счета
> * Анализ расходов на использование Azure

## <a name="prerequisites"></a>Предварительные требования

Необходимо иметь учетную запись выставления счетов для Клиентского соглашения Майкрософт.

Необходимо иметь доступ к Клиентскому соглашению Майкрософт. Вы должны иметь роль Владельца профиля выставления счетов, Участника, Читателя или Менеджера счетов, чтобы смотреть сведения о выставлении счетов и использовании. Дополнительные сведения о ролях выставления счетов для Клиентского соглашения Майкрософт см. в разделе [Billing profile roles and tasks](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks) (Роли и задачи профиля выставления счетов).

Срок службы должен составлять более 30 дней с момента подписки на Azure. Azure выставляет счет только в конце периода выставления счетов.

## <a name="sign-in-to-azure"></a>Вход в Azure

- Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Проверка доступа к Клиентскому соглашению Майкрософт

Проверьте тип соглашения, чтобы узнать, есть ли у вас доступ к учетной записи выставления счетов для Клиентского соглашения Майкрософт.

На портале Azure введите *cost management + billing* (Управление затратами и выставление счетов) в поле поиска, а затем выберите **Cost Management + Billing** (Управление затратами + выставление счетов).

![Снимок экрана с изображением поиска на портале Azure по фразе " cost management + billing" (Управление затратами + выставление счетов)](./media/review-customer-agreement-bill/billing-search-cost-management-billing.png)

Если у вас есть доступ только к одной области выставления счетов, выберите **Свойства** слева. Если тип учетной записи выставления счетов — **Клиентское соглашение Майкрософт**, то у вас есть доступ к учетной записи выставления счетов для Клиентского соглашения Майкрософт.

![Снимок экрана: страница свойств с Клиентским соглашением Майкрософт](./media/review-customer-agreement-bill/billing-mca-property.png)

При наличии доступа к нескольким областям выставления счетов проверьте тип в столбце учетной записи выставления счетов. Если для всех областей в качестве типа учетных записей выставления счетов будет **Клиентское соглашение Майкрософт**, то у вас есть доступ к учетной записи выставления счетов для Клиентского соглашения Майкрософт.

![Снимок экрана: страница со списком учетных записей выставления счетов и Клиентским соглашением Майкрософт](./media/review-customer-agreement-bill/billing-mca-in-the-list.png)

## <a name="review-invoiced-transactions-in-the-azure-portal"></a>Проверка выставленных счетов за транзакции на портале Azure

На портале Azure выберите **Все транзакции** в левой части страницы. В зависимости от уровня доступа вам, возможно, потребуется выбрать учетную запись выставления счетов, профиль выставления счетов или раздел счета, а затем **Все транзакции**.

На странице "Все транзакции" отображаются следующие сведения:

![Снимок экрана со списком транзакций, за которые выставлен счет](./media/review-customer-agreement-bill/mca-billed-transactions-list.png)

|Столбец  |Определение  |
|---------|---------|
|Дата     | Дата транзакции  |
|Идентификатор счета     | Идентификатор счета, за транзакцию по которому был выставлен счет. Если вы отправляете запрос на поддержку, предоставьте идентификатор службе поддержки Azure, чтобы ускорить обработку запроса на поддержку. |
|Тип транзакции     |  Тип транзакции, например покупка, отмена и расходы на использование  |
|Семейство продуктов     | Категория продукта, например вычислительные ресурсы для виртуальных машин или база данных для базы данных SQL Azure.|
|Номер SKU продукта     | Уникальный код, определяющий экземпляр продукта |
|Сумма     |  Сумма транзакции      |
|Раздел счета     | Эта транзакция отображается в этом разделе счета профиля выставления счетов |
|Профиль выставления счетов     | Эта транзакция отображается в этом профиле выставления счетов |

Выполните поиск по идентификатору счета, чтобы отфильтровать транзакции по счету.

### <a name="view-transactions-by-invoice-sections"></a>Просмотр транзакций по разделам счетов

С помощью разделов счетов можно упорядочить затраты в счете профиля выставления счетов. Дополнительные сведения см. в записи блога При создании счета расходы на все разделы в профиле выставления счетов отображаются в счете.

На следующем изображении показаны расходы в разделе счета "Отдел учета" в примере счета.

![Пример изображения, показывающий сведения по разделу счета](./media/review-customer-agreement-bill/invoicesection-details.png)

После определения расходов в разделе счета можно просмотреть транзакции на портале Azure, чтобы интерпретировать сведения о расходах.

Перейдите на страницу "Все транзакции" на портале Azure, чтобы просмотреть транзакции в счете.

Отфильтруйте по имени раздела счета, чтобы просмотреть транзакции.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Просмотр ожидаемых расходов для прогнозирования следующего счета

В учетной записи выставления счетов для Клиентского соглашения Майкрософт расходы являются предполагаемыми и считаются ожидаемыми, пока по ним не будет выставлен счет. На портале Azure можно просмотреть ожидаемые расходы, чтобы спрогнозировать следующий счет. Ожидаемые расходы являются предполагаемыми и не включают налог. Фактические расходы в вашем следующем счете будут отличаться от ожидаемых.

### <a name="view-summary-of-pending-charges"></a>Просмотр сводки ожидаемых расходов

Войдите на [портал Azure](https://portal.azure.com).

Выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

В верхней части экрана выберите вкладку **Сводка**.

В разделе расходов отображаются расходы за текущий и прошлый месяцы.

![Снимок экрана с изображением поиска на портале Azure по фразе " cost management + billing" (Управление затратами + выставление счетов)](./media/review-customer-agreement-bill/mca-billing-profile-summary.png)

Расходы за текущий месяц являются ожидаемыми и счета по ним выставляются при создании счета за месяц. Если счет за прошлый месяц еще не создан, расходы за прошлый месяц также являются ожидаемыми и появятся в следующем счете.

### <a name="view-pending-transactions"></a>Просмотр ожидаемых транзакций

При определении ожидаемых расходов можно оценить расходы, проанализировав отдельные транзакции, которые добавлены к расходам. На этом этапе ожидаемые расходы на использование не отображаются на странице "Все транзакции". Ожидаемые расходы на использование можно просмотреть на странице подписок Azure.

Войдите на [портал Azure](https://portal.azure.com).

На портале Azure введите *cost management + billing* (Управление затратами и выставление счетов) в поле поиска, а затем выберите **Cost Management + Billing** (Управление затратами + выставление счетов).

Выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

Выберите **Все транзакции** в левой части страницы.

Выполните поиск по фразе *ожидаемые*. Используйте фильтр **Временной диапазон**, чтобы просмотреть ожидаемые расходы за текущий или прошлый месяцы.

![Снимок экрана со списком ожидаемых транзакций](./media/review-customer-agreement-bill/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Просмотр ожидаемых расходов на использование

Войдите на [портал Azure](https://portal.azure.com).

На портале Azure введите *cost management + billing* (Управление затратами и выставление счетов) в поле поиска, а затем выберите **Cost Management + Billing** (Управление затратами + выставление счетов).

Выберите профиль выставления счетов. В зависимости от уровня вашего доступа вам, возможно, потребуется выбрать учетную запись выставления счетов. В учетной записи выставления счетов щелкните **Профили выставления счетов** и выберите профиль выставления счетов.

Выберите **Все подписки** в левой части страницы.

На странице подписок Azure для каждой подписки в профиле выставления счетов отображаются расходы за текущий и последний месяцы. Расходы за текущий месяц являются ожидаемыми и счета по ним выставляются при создании счета за месяц. Если счет за прошлый месяц еще не создан, расходы за прошлый месяц также являются ожидаемыми.

![Снимок экрана, на котором показан список подписок Azure для профиля выставления счетов](./media/review-customer-agreement-bill/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Анализ расходов на использование Azure

Для анализа расходов на использование воспользуйтесь CSV-файлом с данными об использовании и расходах Azure. Вы можете скачать файл с данными счета или ожидаемых расходов.

### <a name="download-your-invoice-and-usage-details"></a>Скачивание счета и файла со сведениями об использовании

В зависимости от имеющихся прав доступа вам, возможно, потребуется выбрать учетную запись выставления счетов или профиль выставления счетов во вкладке "Cost Management + Billing" (Управление затратами + выставление счетов). В меню слева в разделе **Выставление счетов** выберите **Счета**. В сетке "Счет" найдите строку счета, которую необходимо загрузить. Щелкните символ скачивания или многоточие (...) в конце строки. В поле **Загрузки** скачайте файл сведений об использовании и счет.

### <a name="view-detailed-usage-by-invoice-section"></a>Просмотр подробных сведений об использовании по разделу счета

Вы можете отфильтровать файл с данными об использовании и расходах Azure, чтобы сверить расходы на использование в разделах счета.

Ниже приведены сведения по сверке расходов на вычисления в разделе счета "Отдел учета".

![Пример изображения, показывающий сведения по разделу счета](./media/review-customer-agreement-bill/invoicesection-details.png)

| PDF-файл счета | CSV-файл с данными об использовании и расходах Azure |
| --- | --- |
|Отдел учета |invoiceSectionName |
|Плата за использование — план Microsoft Azure |productOrderName |
|Службы вычислений |serviceFamily |

Отфильтруйте столбец **invoiceSectionName** в CSV-файле по **отделу учета**. Затем отфильтруйте столбец **productOrderName** в CSV-файле по **плану Microsoft Azure**. Затем отфильтруйте столбец **serviceFamily** в CSV-файле по **Microsoft.Compute**.

![Снимок экрана, показывающий файл с данными об использовании и расходах, отфильтрованный по разделу счета](./media/review-customer-agreement-bill/billing-usage-file-filtered-by-invoice-section.png)

### <a name="view-detailed-usage-by-subscription"></a>Просмотр подробных сведений об использовании по подписке

Вы можете отфильтровать CSV-файл с данными об использовании и расходах Azure, чтобы сверить расходы на использование в подписках. При определении расходов в подписке используйте CSV-файл с данными об использовании и расходах Azure для анализа расходов.

На следующем изображении показан список подписок на портале Azure.

![Снимок экрана, на котором показан список подписок Azure для профиля выставления счетов](./media/review-customer-agreement-bill/mca-billing-profile-subscriptions-list-highlighted.png)

Отфильтруйте столбец **subscriptionName** в CSV-файле с данными об использовании и расходах Azure по **WA_Subscription**, чтобы просмотреть подробные сведения о плате за использование WA_Subscription.

![Снимок экрана, показывающий файл с данными об использовании и расходах, отфильтрованный по подписке](./media/review-customer-agreement-bill/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Оплата счета

Инструкции по оплате счета приведены в нижней части счета. [Узнайте о способах оплаты](mca-understand-your-invoice.md#how-to-pay).

Если вы уже оплатили счет, то можете проверить состояние платежа на странице "Счета" на портале Azure.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы узнали, как выполнять следующие задачи:

> [!div class="checklist"]
> * Проверка выставленных счетов за транзакции на портале Azure
> * Просмотр ожидаемых расходов для прогнозирования следующего счета
> * Анализ расходов на использование Azure

Выполните инструкции из краткого руководства, чтобы начать использование анализа затрат.

> [!div class="nextstepaction"]
> [Начало анализа затрат](../costs/quick-acm-cost-analysis.md)
