---
title: Настройка ограничения исходящего сетевого трафика в Azure HDInsight
description: Узнайте, как настроить ограничение исходящего сетевого трафика для кластеров Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 6771cdb206920c8e3b746e28573de1742543b4c8
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646699"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Настройка исходящего сетевого трафика для кластеров Azure HDInsight с помощью брандмауэра

В этой статье приведены инструкции по защите исходящего трафика из кластера HDInsight с помощью брандмауэра Azure. В описанных ниже действиях предполагается, что вы настраиваете брандмауэр Azure для существующего кластера. При развертывании нового кластера и за брандмауэром сначала создайте кластер HDInsight и подсеть, а затем выполните действия, описанные в этом руководстве.

## <a name="background"></a>Историческая справка

Кластеры Azure HDInsight обычно развертываются в собственной виртуальной сети. Кластер имеет зависимости от служб за пределами этой виртуальной сети, для правильной работы которых требуется сетевой доступ.

Существует несколько зависимостей, требующих входящего трафика. Входящий трафик управления не может быть отправлен через устройство брандмауэра. Исходные адреса для этого трафика известны и публикуются [здесь](hdinsight-management-ip-addresses.md). Вы также можете создать правила группы безопасности сети (NSG) с этими сведениями для защиты входящего трафика к кластерам.

Зависимости исходящего трафика HDInsight почти полностью определяются с помощью полных доменных имен, которым не назначены статические IP-адреса. Отсутствие статических адресов означает, что группы безопасности сети (группы безопасности сети) не могут использоваться для блокировки исходящего трафика из кластера. Адреса часто меняются, что один из них не может настроить правила на основе текущего разрешения имен и использовать его для настройки правил NSG.

Решением для защиты исходящих адресов является использование устройства брандмауэра, которое может управлять исходящим трафиком на основе доменных имен. Брандмауэр Azure может ограничивать исходящий трафик HTTP и HTTPS на основе полного доменного имени в [тегах назначения или полного доменного имени](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>Настройка брандмауэра Azure с помощью HDInsight

Ниже приведена сводка действий по блокировке исходящих данных из существующего HDInsight с помощью брандмауэра Azure.

1. Создайте брандмауэр.
1. Добавление правил приложения в брандмауэр
1. Добавьте сетевые правила в брандмауэр.
1. Создайте таблицу маршрутизации.

### <a name="create-new-subnet"></a>Создание новой подсети

Создайте подсеть с именем **азурефиреваллсубнет** в виртуальной сети, где существует кластер.

### <a name="create-a-new-firewall-for-your-cluster"></a>Создание нового брандмауэра для кластера

Создайте брандмауэр с именем **Test-FW01** , выполнив действия, описанные в руководстве по **развертыванию брандмауэра** [. Развертывание и настройка брандмауэра Azure с помощью портал Azure](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>Настройка брандмауэра с помощью правил приложения

Создайте коллекцию правил приложений, которая позволяет кластеру отправлять и получать важные коммуникации.

1. Выберите новый брандмауэр **Test-FW01** из портал Azure.

1. Последовательно выберите **параметры** > **правила** > **Коллекция правил приложений** >  **+ Добавить коллекцию правил приложений**.

    ![Title: Добавление коллекции правил приложения](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. На экране **Добавление коллекции правил приложений** укажите следующие сведения.

    **Верхний раздел**

    | Свойство|  Значение|
    |---|---|
    |Имя| фваппруле|
    |Приоритет|200|
    |Действия|Allow|

    **Раздел "Теги полного доменного имени"**

    | Имя | Исходный адрес | Тег FQDN | Примечания |
    | --- | --- | --- | --- |
    | Rule_1 | * | WindowsUpdate и HDInsight | Требуется для HDI Services |

    **Раздел полных доменных имен**

    | Имя | Исходные адреса | Протокол: порт | Целевые полные доменные имена | Примечания |
    | --- | --- | --- | --- | --- |
    | Rule_2 | * | HTTPS: 443 | login.windows.net | Разрешает действие входа Windows |
    | Rule_3 | * | HTTPS: 443 | login.microsoftonline.com | Разрешает действие входа Windows |
    | Rule_4 | * | HTTPS: 443, http: 80 | storage_account_name. BLOB. Core. Windows. NET | Замените `storage_account_name` фактическим именем учетной записи хранения. Если кластер создается с помощью WASB, добавьте правило для WASB. Чтобы использовать только HTTPS-подключения, убедитесь, что в учетной записи хранения включено ["требуется безопасное перемещение"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) . |

   ![Title: введите сведения о коллекции правил приложения](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. Выберите **Добавить**.

### <a name="configure-the-firewall-with-network-rules"></a>Настройка брандмауэра с помощью сетевых правил

Создайте сетевые правила для правильной настройки кластера HDInsight.

1. Продолжая с прошлого шага, перейдите к **коллекции правил сети** >  **+ Добавить коллекцию правил сети**.

1. На экране **Добавить коллекцию правил сети** укажите следующие сведения.

    **Верхний раздел**

    | Свойство|  Значение|
    |---|---|
    |Имя| фвнетруле|
    |Приоритет|200|
    |Действия|Allow|

    **Раздел IP-адресов**

    | Имя | Протокол | Исходные адреса | Адреса назначения | Конечные порты | Примечания |
    | --- | --- | --- | --- | --- | --- |
    | Rule_1 | UDP | * | * | 123 | Служба времени |
    | Rule_2 | Любой | * | DC_IP_Address_1, DC_IP_Address_2 | * | Если вы используете Корпоративный пакет безопасности (ESP), добавьте сетевое правило в раздел IP-адресов, который позволяет обмениваться данными с кластерами AAD-DS для ESP. IP-адреса контроллеров домена можно найти в разделе AAD-DS на портале. |
    | Rule_3 | TCP | * | IP-адрес учетной записи Data Lake Storage | * | Если вы используете Azure Data Lake Storage, можно добавить сетевое правило в раздел IP-адресов, чтобы устранить проблемы SNI с ADLS 1-го поколения и Gen2. Этот параметр направляет трафик в брандмауэр, что может привести к увеличению затрат на загрузку больших данных, но этот трафик будет записываться в журнал и отслеживаться в журналах брандмауэра. Определите IP-адрес для учетной записи Data Lake Storage. Вы можете использовать команду PowerShell, например `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")`, чтобы разрешить полное доменное имя в IP-адрес.|
    | Rule_4 | TCP | * | * | 12000 | Используемых Если вы используете Log Analytics, создайте сетевое правило в разделе IP-адреса, чтобы обеспечить взаимодействие с рабочей областью Log Analytics. |

    **Раздел "Теги служб"**

    | Имя | Протокол | Исходные адреса | Теги служб | Порты назначения | Примечания |
    | --- | --- | --- | --- | --- | --- |
    | Rule_7 | TCP | * | SQL | 1433 | Настройте правило сети в разделе "Теги служб" для SQL, который позволит регистрировать и проверять трафик SQL, если конечные точки службы не настроены для SQL Server в подсети HDInsight, что приведет к обходу брандмауэра. |

   ![Title: введите коллекцию правил приложения.](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. Выберите **Добавить**.

### <a name="create-and-configure-a-route-table"></a>Создание и настройка таблицы маршрутов

Создайте таблицу маршрутов со следующими записями:

* Все IP-адреса из [служб работоспособности и управления: все регионы](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) с типом " **Интернет**" следующего прыжка.

* Два IP-адреса для региона, в котором кластер создается из [служб работоспособности и управления: в конкретных регионах](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) , имеющих тип " **Интернет**" следующего прыжка.

* Один виртуальный маршрут виртуального устройства для IP-адреса 0.0.0.0/0 со следующим прыжком к частному IP-адресу брандмауэра Azure.

Например, чтобы настроить таблицу маршрутов для кластера, созданного в регионе US (Восточная часть США), выполните следующие действия.

1. Выберите свой брандмауэр Azure **Test-FW01**. Скопируйте **частный IP-адрес** , указанный на странице **Обзор** . В этом примере мы будем использовать **Пример адреса 10.0.2.4**.

1. Затем перейдите во **все службы** > **сети** > **таблицы маршрутов** и **Создайте таблицу маршрутов**.

1. В новом маршруте выберите **параметры** > **маршруты** >  **+ добавить**. Добавьте следующие маршруты:

| Имя маршрута | Префикс адреса | Тип следующего прыжка | Адрес следующего прыжка |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Интернет | Нет данных |
| 23.99.5.239 | 23.99.5.239/32 | Интернет | Нет данных |
| 168.61.48.131 | 168.61.48.131/32 | Интернет | Нет данных |
| 138.91.141.162 | 138.91.141.162/32 | Интернет | Нет данных |
| 13.82.225.233 | 13.82.225.233/32 | Интернет | Нет данных |
| 40.71.175.99 | 40.71.175.99/32 | Интернет | Нет данных |
| 0.0.0.0 | 0.0.0.0/0 | Виртуальный модуль | 10.0.2.4 |

Завершите настройку таблицы маршрутов:

1. Назначьте созданную таблицу маршрутов в подсети HDInsight, выбрав **подсети** в разделе **Параметры**.

1. Выберите **+ сопоставить**.

1. На экране **связать подсеть** выберите виртуальную сеть, в которой был создан кластер, и **подсеть** , которую вы использовали для кластера HDInsight.

1. Нажмите кнопку **ОК**.

## <a name="edge-node-or-custom-application-traffic"></a>Трафик краевого узла или пользовательского приложения

Описанные выше действия позволят кластеру действовать без проблем. По-прежнему необходимо настроить зависимости для размещения пользовательских приложений, работающих на граничных узлах (если применимо).

Зависимости приложений должны быть идентифицированы и добавлены в брандмауэр Azure или таблицу маршрутов.

Необходимо создать маршруты для трафика приложения, чтобы избежать проблем с маршрутизацией.

Если у ваших приложений есть другие зависимости, их необходимо добавить в брандмауэр Azure. Создайте правила приложения, чтобы разрешать трафик протокола HTTP/HTTPS, и правила сети для всего остального.

## <a name="logging-and-scale"></a>Ведение журнала и масштабирование

Брандмауэр Azure может отправить журналы в несколько различных систем хранения. Инструкции по настройке ведения журнала для брандмауэра см. в разделе Учебник. [мониторинг журналов и метрик брандмауэра Azure](../firewall/tutorial-diagnostics.md).

После завершения настройки ведения журнала, если данные записывается в Log Analytics, можно просмотреть заблокированный трафик с помощью следующего запроса:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Интеграция брандмауэра Azure с журналами Azure Monitor полезна при первом получении приложения, работающего, если вы не знаете о зависимостях приложений. См. дополнительные сведения о журналах Azure Monitor в статье [Анализ данных журнала в Azure Monitor](../azure-monitor/log-query/log-query-overview.md)

Дополнительные сведения об ограничениях масштабирования в брандмауэре Azure и увеличении запросов см. в [этом](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits) документе или на [часто задаваемые вопросы](../firewall/firewall-faq.md).

## <a name="access-to-the-cluster"></a>Доступ к кластеру

После успешной настройки брандмауэра можно использовать внутреннюю конечную точку (`https://CLUSTERNAME-int.azurehdinsight.net`) для доступа к Ambari из виртуальной сети.

Чтобы использовать конечную точку общедоступной конечной точки (`https://CLUSTERNAME.azurehdinsight.net`) или SSH (`CLUSTERNAME-ssh.azurehdinsight.net`), убедитесь в наличии правильных маршрутов в таблице маршрутов и правилах NSG, чтобы избежать описанных [здесь](../firewall/integrate-lb.md)проблем с асимметричной маршрутизацией. В частности, в этом случае необходимо разрешить IP-адрес клиента в правилах входящего NSG, а также добавить его в определяемую пользователем таблицу маршрутов со следующим прыжком, равным `internet`. Если это не настроено правильно, вы увидите ошибку времени ожидания.

## <a name="configure-another-network-virtual-appliance"></a>Настройка другого сетевого виртуального устройства

> [!Important]
> Следующие сведения требуются **только** в том случае, если требуется настроить сетевой виртуальный модуль (NVA), отличный от брандмауэра Azure.

Приведенные выше инструкции помогут настроить брандмауэр Azure, чтобы ограничивать исходящий трафик из кластера HDInsight. Брандмауэр Azure автоматически настраивается на разрешение трафика для многих распространенных важных сценариев. Если вы хотите использовать другой сетевой виртуальный модуль, необходимо вручную настроить ряд дополнительных функций. При настройке сетевого виртуального устройства учитывайте следующее:

* В разделе "Конечные точки" должны быть указаны конечные точки для включенных служб.
* Зависимости IP-адресов предназначены для трафика, отличного от HTTP и S (трафик TCP и UDP).
* Полные доменные конечные точки HTTP и HTTPS можно разместить на устройстве NVA.
* Конечные точки HTTP и HTTPS с подстановочными знаками — это зависимости, которые могут различаться в зависимости от количества квалификаторов.
* Назначьте таблицу маршрутов, созданную в подсети HDInsight.

### <a name="service-endpoint-capable-dependencies"></a>Зависимости, поддерживающие конечную точку службы

| **Конечная точка** |
|---|
| Azure SQL |
| Служба хранилища Azure |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>Зависимости IP-адреса

| **Конечная точка** | **Сведения** |
|---|---|
| \*:123 | Проверка часов NTP. Трафик проверяется в нескольких конечных точках на порте 123. |
| IP-адреса, опубликованные [здесь](hdinsight-management-ip-addresses.md) | Это служба HDInsight |
| Частные IP-адреса AAD DS для кластеров ESP |
| \*: 16800 для активации службы KMS Windows |
| \*12000 для Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>Зависимости FQDN протокола HTTP/HTTPS

> [!Important]
> Приведенный ниже список содержит лишь несколько наиболее важных полных доменных имен. Полный список полных доменных имен для настройки NVA можно найти [в этом файле](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Конечная точка**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Дальнейшие действия

* [Архитектура виртуальной сети Azure HDInsight](hdinsight-virtual-network-architecture.md)
