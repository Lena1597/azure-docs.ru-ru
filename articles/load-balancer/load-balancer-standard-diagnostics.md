---
title: Диагностика с метриками, оповещениями и работоспособностью ресурсов — Azure Load Balancer (цен. категория "Стандартный")
description: Используйте доступные метрики, оповещения и сведения о работоспособности ресурсов для диагностики Load Balancer (цен. категория "Стандартный") Azure.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2019
ms.author: allensu
ms.openlocfilehash: c362829b1babf954868452a3858da1f319008a9a
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990782"
---
# <a name="standard-load-balancer-diagnostics-with-metrics-alerts-and-resource-health"></a>Диагностика Load Balancer (цен. категория "Стандартный") с помощью метрик, оповещений и сведений о работоспособности ресурсов

Load Balancer (цен. категория "Стандартный") Azure предоставляет следующие возможности диагностики:

* **Многомерные метрики и оповещения**. предоставляет возможности многомерной диагностики с помощью [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) для конфигураций подсистемы балансировки нагрузки уровня "Стандартный". Вы можете отслеживать стандартные ресурсы балансировщика нагрузки, управлять ими и устранять неполадки.

* **Работоспособность ресурсов**. страница Load Balancer на странице портал Azure и работоспособность ресурсов (в разделе Monitor) предоставляет раздел Работоспособность ресурсов для Load Balancer (цен. Категория "Стандартный"). 

Эта статья содержит краткий обзор этих возможностей и способов их использования для Load Balancer уровня "Стандартный". 

## <a name = "MultiDimensionalMetrics"></a>Многомерные метрики

Azure Load Balancer предоставляет многомерные метрики с помощью метрик Azure в портал Azure и помогает получать диагностическую информацию о ресурсах подсистемы балансировки нагрузки в режиме реального времени. 

Различные конфигурации Load Balancer уровня "Стандартный" предоставляют следующие метрики:

| Метрика | Тип ресурса | Description | Рекомендуемая статистическая обработка |
| --- | --- | --- | --- |
| Доступность пути к данным (доступность виртуальных IP-адресов)| Общедоступная и внутренняя подсистемы балансировки нагрузки | Load Balancer уровня "Стандартный" непрерывно использует путь к данным из региона к внешнему интерфейсу подсистемы балансировки нагрузки, а затем в стек SDN, поддерживающий виртуальную машину. Пока остаются работоспособные экземпляры, измерение выполняется по тому же пути, что и трафик приложения с балансировкой нагрузки. Проверяется также путь к данным, который используют ваши клиенты. Измерение является невидимым для приложения и не влияет на работоспособность других операций.| Среднее |
| Состояние зонда работоспособности (доступность DIP) | Общедоступная и внутренняя подсистемы балансировки нагрузки | Load Balancer уровня "Стандартный" использует распределенную службу проверки работоспособности, которая контролирует состояние конечной точки приложения в соответствии с параметрами конфигурации. Эта метрика обеспечивает агрегированное или отфильтрованное представление для каждой конечной точки экземпляра в пуле подсистемы балансировки нагрузки. Вы можете посмотреть, как в службе Load Balancer исследуется работоспособность вашего приложения в соответствии с заданными настройками проверки. |  Среднее |
| Пакеты SYN (синхронизация) | Общедоступная и внутренняя подсистемы балансировки нагрузки | Load Balancer уровня "Стандартный" не прерывает TCP-подключения и не взаимодействует с потоками пакетов TCP и UDP. Потоки и их подтверждения всегда происходят между источником и экземпляром виртуальной машины. Чтобы оптимизировать устранение неполадок для всех сценариев протокола TCP, вы можете использовать счетчики пакетов SYN. С их помощью можно определить число попыток подключения TCP. Эта метрика указывает количество принятых пакетов TCP SYN.| Среднее |
| Подключения SNAT | Общедоступная подсистема балансировки нагрузки |Load Balancer уровня "Стандартный" передает число замаскированных исходящих потоков на общедоступный интерфейсный IP-адрес. Количество портов преобразования исходных сетевых адресов (SNAT) ограничено. Эта метрика может дать представление о том, насколько сильно ваше приложение зависит от SNAT для исходящих потоков. Выводятся данные счетчиков для успешного и неудачного выполнения исходящих потоков SNAT. Эти данные помогают устранять неполадки и определять работоспособность исходящих потоков.| Среднее |
| Выделенные порты SNAT | Общедоступная подсистема балансировки нагрузки | Load Balancer (цен. категория "Стандартный") сообщает количество портов SNAT, выделенных для каждого экземпляра сервера | Усреднен. |
| Используемые порты SNAT | Общедоступная подсистема балансировки нагрузки | Load Balancer (цен. категория "Стандартный") сообщает количество портов SNAT, используемых на каждом экземпляре сервера. | Среднее | 
| Счетчики байтов |  Общедоступная и внутренняя подсистемы балансировки нагрузки | Load Balancer уровня "Стандартный" передает количество данных, обработанных каждым внешним интерфейсом. Вы можете заметить, что байты не распределяются одинаково между внутренними экземплярами. Это ожидается, так как алгоритм Load Balancer Azure основан на потоках. | Среднее |
| Счетчики пакетов |  Общедоступная и внутренняя подсистемы балансировки нагрузки | Load Balancer уровня "Стандартный" передает количество пакетов, обработанных каждым внешним интерфейсом.| Среднее |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Просмотр метрик подсистемы балансировки нагрузки на портале Azure

Портал Azure предоставляет метрики подсистемы балансировки нагрузки через страницу метрики, доступную на странице ресурсов балансировщика нагрузки для определенного ресурса и страницы Azure Monitor. 

Чтобы просмотреть метрики для ресурсов Load Balancer уровня "Стандартный", сделайте следующее:
1. Перейдите на страницу метрики и выполните одно из следующих действий.
   * На странице ресурсов подсистемы балансировки нагрузки выберите тип метрики в раскрывающемся списке.
   * На странице Azure Monitor выберите ресурс подсистемы балансировки нагрузки.
2. Выберите соответствующий тип агрегирования.
3. При необходимости настройте требуемые фильтрацию и группирование.

    ![Метрики для Load Balancer (цен. категория "Стандартный")](./media/load-balancer-standard-diagnostics/lbmetrics1anew.png)

    *Рисунок. Метрика доступности пути к данным для Load Balancer (цен. категория "Стандартный")*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Получение многомерных метрик через API-интерфейсы программным образом

Инструкции по получению определений и значений многомерных метрик с помощью API см. в статье [Azure Monitoring REST API walkthrough](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api) (Пошаговое руководство по REST API Azure Monitor). Эти метрики могут быть записаны в учетную запись хранения только с помощью параметра "все метрики". 

### <a name = "DiagnosticScenarios"></a>Распространенные сценарии диагностики и рекомендуемые представления

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Доступен ли путь к данным для виртуального IP-адреса подсистемы балансировки нагрузки?

Метрика доступности виртуального IP-адреса предоставляет сведения о работоспособности пути к данным из региона к вычислительному узлу, где размещены виртуальные машины. Эта метрика отражает работоспособность инфраструктуры Azure. Используя эту метрику, можно:
- отслеживать внешнюю доступность вашей службы;
- получить более подробную информацию о том, нормально ли функционирует платформа, на которой развернута служба, и узнать состояние работоспособности гостевой ОС или экземпляра приложения;
- определить, к чему относится событие: к службе или к базовой плоскости данных. Не путайте эту метрику с состоянием пробы работоспособности (метрика доступности выделенного IP-адреса).

Чтобы получить сведения о доступности пути к данным для ресурсов Load Balancer (цен. категория "Стандартный"):
1. Убедитесь, что выбран правильный ресурс подсистемы балансировки нагрузки. 
2. В раскрывающемся списке **Метрика** выберите **доступность пути к данным**. 
3. В раскрывающемся списке **Агрегирование** выберите **Среднее**. 
4. Кроме того, добавьте фильтр для внешнего IP-адреса или порта переднего плана в качестве измерения с требуемым внешним IP-адресом или интерфейсным портом, а затем сгруппируйте их по выбранному измерению.

![Проверка виртуального IP-адреса](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Рисунок: Load Balancer сведения об интерфейсной части поиска*

Метрика создается с помощью активного внутриполосного измерения. Служба проверки формирует трафик для измерения в пределах региона. Служба активируется сразу после создания развертывания с общедоступным интерфейсом и продолжает работу, пока этот интерфейс не будет удален. 

Периодически создается пакет, соответствующий внешнему интерфейсу и правилу развертывания. Он направляет трафик в регионе от источника к узлу, где расположена виртуальная машина в серверном пуле. Инфраструктура подсистемы балансировки нагрузки выполняет те же операции балансировки нагрузки и преобразования, что и для остального трафика. Эта проба находится в сети внутри конечной точки с балансировкой нагрузки. После того как проба поступит на узел вычислений, где находится работоспособная виртуальная машина в серверном пуле, узел вычислений генерирует ответ для службы проверки работоспособности. Ваша виртуальная машина не видит этот трафик.

Сбой доступности виртуального IP-адреса возникает по следующим причинам:
- в серверном пуле вашего развертывания не осталось исправных виртуальных машин; 
- произошел сбой инфраструктуры.

В целях диагностики можно использовать [метрику доступности пути к данным вместе с состоянием зонда работоспособности](#vipavailabilityandhealthprobes).

Для большинства сценариев используйте тип статистического вычисления **Среднее**.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>Отвечают ли на пробы серверные экземпляры для виртуального IP-адреса?

Метрика состояния для пробы работоспособности предоставляет сведения о работоспособности развертывания приложения. Параметры задаются при настройке проверки работоспособности для подсистемы балансировки нагрузки. Подсистема балансировки нагрузки использует состояние пробы работоспособности, чтобы определить, куда отправлять новые потоки. Пробы работоспособности исходят из адреса инфраструктуры Azure и видны в гостевой ОС виртуальной машины.

Чтобы получить состояние пробы работоспособности для ресурсов Load Balancer (цен. категория "Стандартный"):
1. Выберите метрику **состояния проверки работоспособности** с типом агрегирования **СР** . 
2. Примените фильтр к требуемому интерфейсному IP-адресу или порту (или к обоим).

Пробы работоспособности завершаются ошибками по следующим причинам:
- Вы настроили проверку работоспособности для порта, который не ожидает передачи данных, не отвечает или же использует неправильный протокол. Если ваша служба использует правила прямого ответа от сервера (DSR или плавающий IP-адрес), убедитесь в том, что она ожидает передачи данных по IP-адресу IP-конфигурации сети сетевого интерфейса, а не только через интерфейс замыкания на себя, который настроен с внешним IP-адресом.
- Ваша проверка не разрешена группой безопасности сети, брандмауэром гостевой ОС виртуальной машины или фильтрами прикладного уровня.

Для большинства сценариев используйте тип статистического вычисления **Среднее**.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Как проверить статистику исходящего подключения? 

Метрика подключений SNAT предоставляет сведения о числе успешных и неудачных подключений для [исходящих потоков](https://aka.ms/lboutbound).

Число подключений со сбоями, превышающее нуль, указывает на нехватку портов SNAT. Нужно продолжить исследование, чтобы определить, что может вызвать эти сбои. При нехватке портов SNAT нельзя установить [исходящие потоки](https://aka.ms/lboutbound). Просмотрите статью об исходящих подключениях, чтобы составить представление о сценариях и механизмах работы, а также узнать, как уменьшить риск и избежать нехватки портов SNAT. 

Чтобы получить статистику по подключениям SNAT, сделайте следующее:
1. Выберите тип метрики **SNAT Connections** (Подключения SNAT) и тип агрегирования **Сумма**. 
2. Сгруппируйте подключения SNAT по **состоянию** (успешные и неудачные). Подключения в каждом состоянии будут представлены разными линиями. 

![Подключение SNAT](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Рисунок. Количество подключений SNAT для Load Balancer*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Как проверить попытки входящих и исходящих подключений для службы?

Метрика пакетов SYN предоставляет сведения о числе принятых или отправленных пакетов TCP SYN (для [исходящих потоков](https://aka.ms/lboutbound)), связанных с определенным внешним интерфейсом. Эта метрика позволяет проанализировать попытки подключения TCP к вашей службе.

Для большинства сценариев используйте тип статистической обработки **Всего**.

![Подключение SYN](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Рисунок. Количество подключений SYN для Load Balancer*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Как проверить потребление пропускной способности сети? 

Метрика счетчиков байтов и пакетов предоставляет сведения о числе байтов и пакетов, отправленных или полученных вашей службой для каждого внешнего интерфейса.

Для большинства сценариев используйте тип статистической обработки **Всего**.

Чтобы получить статистику количества байтов или пакетов, сделайте следующее:
1. Выберите тип метрики **Bytes Count** (Количество байтов) и (или) **Packet Count** (Количество пакетов), а также тип агрегирования **Среднее**. 
2. Выполните одно из приведенных ниже действий.
   * Примените фильтр к определенному интерфейсному IP-адресу, интерфейсному порту, IP-адресу серверной части или порту серверной части.
   * Получите общую статистику для ресурсов подсистемы балансировки нагрузки без какой-либо фильтрации.

![Количество байтов](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Рисунок. Количество байтов Load Balancer*

#### <a name = "vipavailabilityandhealthprobes"></a>Как выполнить диагностику развертывания подсистемы балансировки нагрузки?

Сочетание метрик доступности виртуального IP-адреса и пробы работоспособности на одной диаграмме позволяет определить причину и решение проблемы. Благодаря этой информации вы можете удостовериться в том, что Azure работает правильно. Кроме того, с помощью этих сведений можно достоверно определить, что является первопричиной проблемы: конфигурация или приложение.

Вы можете использовать метрики пробы работоспособности, чтобы понять, как Azure рассматривает работоспособность вашего развертывания в соответствии с предоставленной вами конфигурацией. Просмотр проб работоспособности — отличный первый шаг при мониторинге или определении причины.

Вы можете перейти к следующему этапу и использовать метрики доступности виртуального IP-адреса, чтобы получить представление о том, как в Azure исследуется работоспособность базовой плоскости данных, отвечающей за конкретное развертывание. Если вы объедините обе метрики, можно будет определить, где возникла ошибка, как показано в этом примере:

![Объединение метрик доступности пути к данным и состояния проверки работоспособности](./media/load-balancer-standard-diagnostics/lbmetrics-dipnvipavailability-2bnew.png)

*Рисунок: объединение метрик доступности пути к данным и состояния проверки работоспособности*

На диаграмме представлена следующая информация:
- Инфраструктура, в которой размещены виртуальные машины, была недоступна и в начале диаграммы 0%. В дальнейшем инфраструктура была работоспособна, и виртуальные машины были доступны, а в серверной части помещается несколько виртуальных машин. Эта информация обозначается синим следом для доступности пути к данным (доступность виртуального IP-адреса), который позднее 100%. 
- Состояние зонда работоспособности (доступность DIP), обозначенное фиолетовой трассировкой, равно 0% в начале диаграммы. Область в кружке выделяется зеленым цветом, где состояние зонда работоспособности (доступность DIP) стало работоспособным, и в этом случае развертывание клиента могло принимать новые потоки.

Благодаря диаграмме клиенты могут самостоятельно устранять проблемы с развертыванием без необходимости обращаться в службу поддержки или угадывать, были ли другие проблемы. При проверках работоспособности служба была недоступна по причине сбоев из-за неправильной конфигурации или неисправности приложения.

## <a name = "ResourceHealth"></a>Состояние работоспособности ресурсов

Состояние работоспособности ресурсов Load Balancer уровня "Стандартный" можно узнать на странице **Работоспособность ресурсов** в разделе **Мониторинг > Работоспособность служб**.

Чтобы просмотреть работоспособность ресурсов общедоступной службы Load Balancer уровня "Стандартный", сделайте следующее:
1. Выберите **Мониторинг** > **Работоспособность службы**.

   ![Страница мониторинга](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Рисунок. Ссылка "Работоспособность службы" в Azure Monitor*

2. Выберите **Работоспособность ресурсов** и убедитесь, что выбран **идентификатор подписки**, а **для типа ресурса установлено значение Load Balancer**.

   ![Состояние работоспособности ресурсов](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Рисунок. Выбор ресурса для представления работоспособности*

3. В списке щелкните ресурс Load Balancer, чтобы просмотреть историю его состояния работоспособности.

    ![Состояние работоспособности Load Balancer](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Рисунок. Представление работоспособности ресурса Load Balancer*
 
В следующей таблице перечислены различные состояния работоспособности ресурсов и их описания: 

| Состояние работоспособности ресурсов | Description |
| --- | --- |
| Доступно | Ресурс "Стандартный балансировщик нагрузки" исправен и доступен. |
| Рекомендации недоступны | Ресурс "Стандартный балансировщик нагрузки" неисправен. Проведите диагностику работоспособности, последовательно выбрав **Azure Monitor** > **Метрики**<br>(Состояние "*недоступно* " может также означать, что ресурс не подключен к вашей стандартной подсистеме балансировки нагрузки.) |
| Неизвестно | Состояние работоспособности ресурсов для ресурса подсистемы балансировки нагрузки "Стандартный" еще не обновлено.<br>(*Неизвестное* состояние может также означать, что ресурс не подключен к вашей стандартной подсистеме балансировки нагрузки.)  |

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше об [Azure Load Balancer уровня "Стандартный"](load-balancer-standard-overview.md).
- Узнайте больше об [исходящих подключениях подсистемы балансировки нагрузки](https://aka.ms/lboutbound).
- Дополнительные сведения об [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).
- Дополнительные сведения об [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/) и о [способе получения метрик через REST API](/rest/api/monitor/metrics/list).
