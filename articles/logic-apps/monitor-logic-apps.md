---
title: Мониторинг состояния, Просмотр журнала и Настройка оповещений
description: Устранение неполадок приложений логики путем проверки состояния выполнения, просмотра журнала триггеров и включения оповещений в Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 495877f1c839de2cf3583a37180054c91bd9f139
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76907777"
---
# <a name="monitor-run-status-review-trigger-history-and-set-up-alerts-for-azure-logic-apps"></a>Мониторинг состояния выполнения, Просмотр журнала триггеров и Настройка оповещений для Azure Logic Apps

После [создания и запуска приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md)можно проверить состояние выполнения приложения логики, [историю выполнения](#review-runs-history), [Журнал триггеров](#review-trigger-history)и производительность. Чтобы получать уведомления о сбоях или других возможных проблемах, настройте [оповещения](#add-azure-alerts). Например, можно создать оповещение, которое обнаруживает "более пяти запусков в течение часа, которые завершились сбоем".

Для наблюдения за событиями в реальном времени и расширенной отладки настройте ведение журнала диагностики для приложения логики с помощью [журналов Azure Monitor](../azure-monitor/overview.md). Эта служба Azure помогает отслеживать облачные и локальные среды, чтобы можно было легко поддерживать доступность и производительность. Затем можно находить и просматривать события, такие как события триггера, события запуска и события действий. Хранение этих сведений в [журналах Azure Monitor](../azure-monitor/platform/data-platform-logs.md)позволяет создавать [запросы журналов](../azure-monitor/log-query/log-query-overview.md) , помогающие находить и анализировать эти сведения. Эти диагностические данные также можно использовать с другими службами Azure, такими как служба хранилища Azure и концентраторы событий Azure. Дополнительные сведения см. в статье [мониторинг приложений логики с помощью Azure Monitor](../logic-apps/monitor-logic-apps-log-analytics.md).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

<a name="review-runs-history"></a>

## <a name="review-runs-history"></a>Просмотр журнала запусков

Каждый раз, когда триггер срабатывает для элемента или события, обработчик Logic Apps создает и запускает отдельный экземпляр рабочего процесса для каждого элемента или события. По умолчанию каждый экземпляр рабочего процесса выполняется параллельно, поэтому рабочий процесс не должен ожидать перед запуском. Вы можете проверить, что произошло во время этого запуска, включая состояние каждого шага в рабочем процессе, а также входные и выходные данные для каждого шага.

1. В [портал Azure](https://portal.azure.com)найдите и откройте приложение логики в конструкторе приложений логики.

   Чтобы найти приложение логики, в главном поле поиска Azure введите `logic apps`, а затем выберите **Logic Apps**.

   ![Поиск и выбор службы "Logic Apps"](./media/monitor-logic-apps/find-your-logic-app.png)

   В портал Azure показаны все приложения логики, связанные с подписками Azure. Этот список можно отфильтровать по имени, подписке, группе ресурсов, расположению и т. д.

   ![Просмотр приложений логики, связанных с подписками](./media/monitor-logic-apps/logic-apps-list-in-subscription.png)

1. Выберите приложение логики и щелкните **Обзор**.

   На панели "Обзор" в разделе " **журнал запусков**" отображаются все предыдущие, текущие и все ожидающие выполнения приложения логики. Если в списке отображается много запусков и вы не можете найти нужную запись, попробуйте отфильтровать список. Если вы не нашли ожидаемые данные, попробуйте щелкнуть **Обновить** на панели инструментов.

   ![Обзор, журнал запусков и другие сведения о приложении логики](./media/monitor-logic-apps/overview-pane-logic-app-details-run-history.png)

   Ниже приведены возможные состояния запуска приложения логики.

   | Состояние | Description |
   |--------|-------------|
   | **Отменено** | Рабочий процесс был запущен, но получил запрос на отмену |
   | **Сбой** | Не удалось выполнить по крайней мере одно действие, и последующие действия в рабочем процессе не были настроены для обработки сбоя. |
   | **Выполнение** | Рабочий процесс выполняется в данный момент. <p>Это состояние также может отображаться для регулируемых рабочих процессов или из-за текущего плана ценообразования. Чтобы узнать больше, ознакомьтесь с [ограничениям действий на странице цен](https://azure.microsoft.com/pricing/details/logic-apps/). Если вы настроили [ведение журнала диагностики](../logic-apps/monitor-logic-apps.md), можно получить сведения о любых происходящих событиях регулирования. |
   | **Успешно** | Все действия выполнены успешно. <p>**Примечание**. Если какие-либо ошибки произошли в определенном действии, в рабочем процессе будет выполнено следующее действие. |
   | **Ожидающи** | Рабочий процесс не запущен или приостановлен, например, из-за более раннего рабочего процесса, который еще работает. |
   |||

1. Чтобы проверить шаги и другие сведения для конкретного запуска, в разделе **журнал запусков**выберите этот запуск.

   ![Выберите конкретный запуск для проверки](./media/monitor-logic-apps/select-specific-logic-app-run.png)

   На панели **Запуск приложения логики** отображается каждый шаг в выбранном запуске, состояние выполнения каждого шага и время, затраченное на выполнение каждого шага, например:

   ![Каждое действие в определенном запуске](./media/monitor-logic-apps/logic-app-run-pane.png)

   Чтобы просмотреть эти сведения в виде списка, на панели инструментов **Запуск приложения логики** выберите **сведения о выполнении**.

   ![На панели инструментов выберите "сведения о запуске".](./media/monitor-logic-apps/select-run-details-on-toolbar.png)

   В представлении сведения о выполнении отображаются все шаги, их состояние и другие сведения.

   ![Ознакомьтесь с подробными сведениями о каждом шаге выполнения.](./media/monitor-logic-apps/review-logic-app-run-details.png)

   Например, можно получить свойство **идентификатора корреляции** запуска, которое может потребоваться при использовании [REST API для Logic Apps](https://docs.microsoft.com/rest/api/logic).

1. Чтобы получить дополнительные сведения о конкретном шаге, выберите любой из вариантов:

   * В области **Запуск приложения логики** выберите шаг, чтобы расширить фигуру. Теперь вы можете просматривать такие сведения, как входные данные, выходные данные и любые ошибки, произошедшие на этом шаге, например:

     ![На панели выполнения приложения логики просмотрите шаг "сбой"](./media/monitor-logic-apps/specific-step-inputs-outputs-errors.png)

   * В области **сведения о выполнении приложения логики** выберите нужный шаг.

     ![На панели сведений о запуске просмотрите шаг "сбой"](./media/monitor-logic-apps/select-failed-step-in-failed-run.png)

     Теперь можно просмотреть такие сведения, как входные и выходные данные для этого шага, например:

   > [!NOTE]
   > Все сведения о времени выполнения и события шифруются в службе Logic Apps. Они расшифровываются только в том случае, когда пользователь запрашивает просмотр этих данных. Вы можете [Скрыть входные и выходные данные в журнале выполнения](../logic-apps/logic-apps-securing-a-logic-app.md#obfuscate) или управлять доступом пользователей к этим сведениям с помощью [управления доступом на основе ролей (RBAC) Azure](../role-based-access-control/overview.md).

<a name="review-trigger-history"></a>

## <a name="review-trigger-history"></a>Просмотр журнала триггера

Каждое выполнение приложения логики начинается с триггера. В журнале триггеров перечислены все попытки запуска приложения логики и сведения о входных и выходных данных для каждой попытки триггера.

1. В [портал Azure](https://portal.azure.com)найдите и откройте приложение логики в конструкторе приложений логики.

   Чтобы найти приложение логики, в главном поле поиска Azure введите `logic apps`, а затем выберите **Logic Apps**.

   ![Поиск и выбор службы "Logic Apps"](./media/monitor-logic-apps/find-your-logic-app.png)

   В портал Azure показаны все приложения логики, связанные с подписками Azure. Этот список можно отфильтровать по имени, подписке, группе ресурсов, расположению и т. д.

   ![Просмотр приложений логики, связанных с подписками](./media/monitor-logic-apps/logic-apps-list-in-subscription.png)

1. Выберите приложение логики и щелкните **Обзор**.

1. В меню приложения логики выберите **Обзор**. В разделе " **Сводка** " в разделе " **Оценка**" выберите **Просмотреть журнал триггеров**.

   ![Просмотр журнала триггеров для приложения логики](./media/monitor-logic-apps/overview-pane-logic-app-details-trigger-history.png)

   На панели Журнал триггеров отображаются все попытки активации, выполняемые приложением логики. Каждый раз, когда триггер срабатывает для элемента или события, обработчик Logic Apps создает отдельный экземпляр приложения логики, который запускает рабочий процесс. По умолчанию все экземпляры выполняются параллельно, чтобы ни один рабочий процесс не ожидал запуска. Поэтому, если приложение логики активирует несколько элементов одновременно, для каждого элемента отображается запись триггера с одинаковыми датой и временем.

   ![Несколько попыток активировать различные элементы](./media/monitor-logic-apps/logic-app-trigger-history.png)

   Ниже приведены возможные состояния для попытки активации.

   | Состояние | Description |
   |--------|-------------|
   | **Сбой** | Произошла ошибка. Чтобы просмотреть созданные сообщения об ошибках для триггера, завершившегося сбоем, выберите соответствующую попытку активации и щелкните **Выходные данные**. Например, вы можете обнаружить недопустимые входные данные. |
   | **Пропущено** | Триггер проверил конечную точку, но не нашел данные. |
   | **Успешно** | Триггер проверил конечную точку и нашел доступные данные. Как правило, рядом с этим состоянием также отображается состояние "Срабатывания". Если это не так, возможно, определение триггера содержит какое-либо условие или команду `SplitOn`, которая не была выполнена. <p>Это состояние может применяться к запускаемому вручную триггеру, а также повторяющему или опрашивающему триггеру. Триггер может быть успешно запущен, но сам запуск по-прежнему может завершиться ошибкой, если действия порождают необработанные ошибки. |
   |||

   > [!TIP]
   > Триггер можно проверить, не дожидаясь следующего повторения. На панели инструментов обзора выберите **запустить триггер**и выберите триггер, который вызывает проверку. Или щелкните **Запустить** на панели инструментов конструктора приложений логики.

1. Чтобы просмотреть сведения об определенной попытке активации, на панели триггера выберите событие триггера. Если в списке отображается много попыток запуска триггера и вы не можете найти нужную запись, попробуйте отфильтровать список. Если вы не нашли ожидаемые данные, попробуйте щелкнуть **Обновить** на панели инструментов.

   ![Просмотр конкретной попытки триггера](./media/monitor-logic-apps/select-trigger-event-for-review.png)

   Теперь можно просматривать сведения о выбранном событии триггера, например:

   ![Просмотр сведений о конкретном триггере](./media/monitor-logic-apps/view-specific-trigger-details.png)

<a name="add-azure-alerts"></a>

## <a name="set-up-monitoring-alerts"></a>Настройка оповещений мониторинга

Чтобы получать оповещения на основе конкретных метрик или превышения пороговых значений для приложения логики, настройте [оповещения в Azure Monitor](../azure-monitor/platform/alerts-overview.md). Узнайте подробнее о [метриках в Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). Чтобы настроить оповещения без использования [Azure Monitor](../log-analytics/log-analytics-overview.md), выполните следующие действия.

1. В меню приложения логики в разделе **мониторинг**выберите **оповещения** > **новое правило генерации оповещений**.

   ![Добавление оповещения для приложения логики](./media/monitor-logic-apps/add-new-alert-rule.png)

1. На панели **создать правило** в разделе **ресурс**выберите приложение логики, если оно еще не выбрано. В разделе **условие**выберите **Добавить** , чтобы можно было определить условие, которое запускает оповещение.

   ![Добавление условия для правила](./media/monitor-logic-apps/add-condition-for-rule.png)

1. На панели **Настройка логики сигнала** найдите и выберите сигнал, для которого требуется получить оповещение. Можно использовать поле поиска или отсортировать сигналы в алфавитном порядке, выбрав заголовок столбца **имя сигнала** .

   Например, если вы хотите отправить оповещение при сбое триггера, выполните следующие действия.

   1. В столбце **имя сигнала** найдите и выберите сигнал **сбой триггеров** .

      ![Выберите сигнал для создания оповещения](./media/monitor-logic-apps/find-and-select-signal.png)

   1. В области сведений, которая открывается для выбранного сигнала в разделе **логика оповещения**, настройте условие, например:

   1. В качестве **оператора**выберите **больше или равно**.

   1. В качестве **типа агрегирования**выберите **Count**.

   1. В качестве **порогового значения**введите `1`.

   1. В разделе **Предварительный просмотр условия**убедитесь, что условие отображается правильно.

   1. В разделе **оценено на основе**настройте интервал и частоту выполнения правила оповещения. Для **гранулярности статистической обработки (period)** выберите период для группирования данных. В поле **частота вычисления**выберите частоту проверки условия.

   1. Когда будете готовы, нажмите кнопку **Готово**.

   Вот завершенное условие:

   ![Настройка условия для оповещения](./media/monitor-logic-apps/set-up-condition-for-alert.png)

   На странице **Создание правила** теперь отображается созданное вами условие и затраты на выполнение этого оповещения.

   ![Новое оповещение на странице "Создание правила"](./media/monitor-logic-apps/finished-alert-condition-cost.png)

1. Укажите имя, необязательное описание и степень серьезности оповещения. Либо оставьте параметр **включить правило после** включения параметра Создание, либо отключите его, пока вы не будете готовы к включению правила.

1. Когда все будет готово, выберите **создать правило генерации оповещений**.

> [!TIP]
> Чтобы запустить приложение логики из оповещения, в рабочий процесс можно включить [триггер запроса](../connectors/connectors-native-reqres.md), что позволяет выполнять такие задачи, как указано ниже:
> 
> * [Публикация в Slack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Отправка текста](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Добавление сообщения в очередь](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

## <a name="next-steps"></a>Дальнейшие действия

* [Мониторинг приложений логики с помощью Azure Monitor](../logic-apps/monitor-logic-apps-log-analytics.md)