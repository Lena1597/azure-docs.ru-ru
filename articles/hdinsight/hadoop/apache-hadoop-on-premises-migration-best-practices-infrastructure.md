---
title: 'Инфраструктура: локальное Apache Hadoop в Azure HDInsight'
description: Ознакомьтесь с рекомендациями по использованию инфраструктуры в рамках миграции локальных кластеров Hadoop в Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: d7ee8ae121e3cbb9760a87c95d12109a9b05e0c5
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951519"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>Миграция локальных кластеров Apache Hadoop в HDInsight Azure — рекомендации по использованию инфраструктуры

В этой статье представлены рекомендации по управлению инфраструктуры кластеров Azure HDInsight. В этом цикле статей приведены рекомендации, применимые при перемещении локальных систем Apache Hadoop в Azure HDInsight.

## <a name="plan-for-hdinsight-cluster-capacity"></a>Планирование емкости кластера HDInsight

Ниже перечислены ключевые варианты для планирования пропускной способности кластера HDInsight.

**Регион**  
Регион Azure определяет, где физически подготавливается кластер. Чтобы свести к минимуму задержки при чтении и записи, кластер должен находиться в том же регионе, что и данные.

**Расположение и размер хранилища**  
Хранилище по умолчанию должно находиться в том же регионе, что и кластер. Для кластера с 48 узлами рекомендуется использовать от 4 до 8 учетных записей хранения. Хотя общий объем хранилища уже может быть вполне достаточным, каждая учетная запись хранения обеспечивает дополнительную пропускную способность сети для вычислительных узлов. При наличии нескольких учетных записей хранения используйте случайное имя без префикса для каждой из них. Назначение случайного именования — уменьшить вероятность возникновения узких мест в хранилище (и регулирования) и типичных сбоев во всех учетных записях. Для повышения производительности используйте только один контейнер на учетную запись хранения.

**Размер и тип виртуальной машины (теперь поддерживает серии G)**  
У каждого типа кластера имеется набор типов узлов, и у каждого типа узла имеются определенные параметры, определяющие размер и тип виртуальной машины. Размер и тип виртуальной машины определяется вычислительной мощностью ЦП, объемом оперативной памяти и задержкой сети. Модифицированную рабочую нагрузку можно использовать для определения оптимального размера и типа виртуальной машины для каждого типа узла.

**Число рабочих узлов**  
Начальное число рабочих узлов можно определить с помощью смоделированных рабочих нагрузок. Кластер можно масштабировать позже, добавляя больше рабочих узлов для соответствия потребностям пиковых нагрузок. После этого кластер можно масштабировать обратно, если дополнительные рабочие узлы не требуются.

Дополнительные сведения см. в статье [Планирование загрузки кластеров HDInsight](../hdinsight-capacity-planning.md).

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Использование рекомендованного типа виртуальной машины для кластера

Сведения о рекомендуемых типах виртуальных машин для каждого типа кластера HDInsight см. в статье [Конфигурация узлов и размеры виртуальных машин по умолчанию для кластеров](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters).

## <a name="check-hadoop-components-availability-in-hdinsight"></a>Проверка доступности компонентов Hadoop в HDInsight

Каждая версия HDInsight — это облачное распределение набора компонентов экономичной системы Hadoop. Дополнительные сведения о компонентах HDInsight и их текущих версиях см. в статье [Что представляют собой различные компоненты Hadoop, доступные в HDInsight](../hdinsight-component-versioning.md).

Вы также можете использовать пользовательский интерфейс Apache Ambari или Ambari REST API для проверки компонентов Hadoop и версий в HDInsight.

Приложения или компоненты, которые были доступны в локальных кластерах, но не входят в состав кластеров HDInsight, могут быть добавлены на пограничном узле или виртуальной машине в той же виртуальной сети, что и кластер HDInsight. Стороннее приложение Hadoop, которое недоступно на Azure HDInsight, может быть установлено с помощью опции "Приложение" в кластере HDInsight. Пользовательские приложения Hadoop могут быть установлены на кластере HDInsight с помощью "действий сценариев". В следующей таблице перечислены некоторые из распространенных приложений и их возможности интеграции с HDInsight.

|**Приложения**|**Интеграция**
|---|---|
|Воздушный поток|Узел IaaS или HDInsight
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Отсутствует (только HDP)
|Datameer|Краевой узел HDInsight
|Datastax (Cassandra)|IaaS (CosmosDB — альтернатива в Azure)
|DataTorrent|IaaS 
|Drill|IaaS 
|Ignite|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|IaaS (CosmosDB — альтернатива в Azure)
|NiFi|IaaS 
|Presto|Узел IaaS или HDInsight
|Python 2;|PaaS 
|Python 3|PaaS 
|R|PaaS 
|SAS|IaaS 
|Vertica|IaaS (хранилище данных SQL — альтернатива в Azure)
|Tableau|IaaS 
|Waterline|Краевой узел HDInsight
|Наборы потоков|Ребро HDInsight 
|Palantir|IaaS 
|SailPoint|IaaS 

Дополнительные сведения см. в статье [Компоненты Hadoop, доступные в разных версиях HDInsight](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions)

## <a name="customize-hdinsight-clusters-using-script-actions"></a>Настройка кластеров HDInsight с помощью действий сценариев

HDInsight предоставляет метод конфигурации кластера с именем **действие сценария**. Действие сценария — это bash-скрипт, который выполняется на узлах в кластере HDInsight и может использоваться для установки дополнительных компонентов и изменения параметров конфигурации.

Действия сценариев должны храниться в URI, доступном из кластера HDInsight. Они могут использоваться во время создания или после создания кластера, а также могут быть ограничены только для определенных типов узлов.

Сценарий может сохраняться или выполняться один раз. Сохраняемые сценарии применяются при настройке новых рабочих узлов, добавленных в кластер при операциях масштабирования. Сохраняемый сценарий может также применять изменения к узлу другого типа, например головному, при операциях масштабирования.

HDInsight предоставляет предварительно созданные сценарии для установки следующих компонентов в кластерах HDInsight:

- добавление учетной записи хранения Azure;
- Установка Hue
- установка Presto;
- установка Solr;
- установка Giraph;
- предварительная загрузка библиотек Hive.
- Установка или обновление Mono

> [!Note]  
> HDInsight не обеспечивает прямую поддержку пользовательских компонентов Hadoop или компонентов, установленных с помощью действий сценариев.

Действия сценариев можно опубликовать в Azure Marketplace как приложение HDInsight.

Дополнительные сведения см. в следующих статьях:

- [Установка сторонних приложений Apache Hadoop в Azure HDInsight](../hdinsight-apps-install-applications.md)
- [Настройка кластеров HDInsight с помощью действий сценариев](../hdinsight-hadoop-customize-cluster-linux.md)
- [Публикация приложения HDInsight в Azure Marketplace](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Настройка конфигураций HDInsight с помощью начальной загрузки

Изменения в конфигурациях в файлах конфигурации, таких как `core-site.xml`, `hive-site.xml` и `oozie-env.xml`, можно сделать с помощью начальной загрузки. Следующий скрипт является примером с помощью командлета PowerShell [AZ Module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) [New-аздинсигхтклустерконфиг](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster):

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzHDInsightClusterConfig '
    | Set—AzHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Дополнительные сведения см. в статье [Настройка кластеров HDInsight с помощью начальной загрузки](../hdinsight-hadoop-customize-cluster-bootstrap.md).  См. также [Управление кластерами HDInsight с помощью REST API Apache Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>Клиентские инструменты доступа из граничных узлов кластера HDInsight Hadoop

Пустой граничный узел — это виртуальная машина Linux, где установлены и настроены те же клиентские средства, что и на головных узлах, но без служб Hadoop. Граничный узел используется для следующих целей:

- доступ к кластеру;
- тестирование клиентских приложений;
- размещение клиентских приложений.

Граничные узлы можно создать и удалить на портале Azure и их можно использовать во время создания или после создания кластера. После создания граничного узла можно подключиться к нему с помощью SSH и запустить клиентские средства для доступа к кластеру Hadoop в HDInsight. `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22` является конечной точкой SSH в граничном узле.


Дополнительные сведения см. в статье [Использование пустых граничных узлов в кластерах Apache Hadoop в HDInsight](../hdinsight-apps-use-edge-node.md)


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Использование функции увеличения и уменьшения масштаба кластеров

HDInsight обеспечивает гибкость, предоставляя возможность увеличивать и уменьшать масштаб определенного количества рабочих узлов в кластерах. Эта функция позволяет сжимать кластер в нерабочие часы или в выходные дни, а также разворачивать его при пиковых бизнес-требованиях. Дополнительные сведения см. здесь:

* [Масштабирование кластеров HDInsight](../hdinsight-scaling-best-practices.md).
* [Масштабирование кластеров](../hdinsight-administer-use-portal-linux.md#scale-clusters).

## <a name="use-hdinsight-with-azure-virtual-network"></a>Использование HDInsight с виртуальной сетью Azure

Виртуальные сети Azure позволяют ресурсам Azure (например, виртуальным машинам Azure) безопасно обмениваться данными через локальные сети и Интернет путем фильтрации и маршрутизации сетевого трафика.

Использование виртуальной сети Azure в HDInsight обеспечивает реализацию следующих сценариев.

- Подключение к HDInsight непосредственно из локальной сети.
- Подключение к хранилищам данных HDInsight в виртуальной сети Azure.
- Прямой доступ к службам Hadoop, недоступным из Интернета. Например, возможность напрямую работать с API Kafka или использовать API Java для HBase.

HDInsight можно добавить в новую или существующую виртуальную сеть Azure. Если HDInsight добавляется в существующую виртуальную сеть, необходимо обновить существующие группы безопасности сети и определенные пользователем маршруты, чтобы обеспечить неограниченный доступ к [нескольким IP-адресам](../hdinsight-management-ip-addresses.md) в центре обработки данных Azure. Кроме того, убедитесь, что не нужно блокировать трафик к [портам](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ports), которые используются службами HDInsight.

> [!Note]  
> В настоящее время HDInsight не поддерживает принудительное туннелирование. Принудительное туннелирование — это настройка подсети, которая принудительно отправляет исходящий интернет-трафик на устройство для его проверки и ведения журнала. Перед установкой HDInsight в подсеть либо удалите принудительное туннелирование, либо создайте новую подсеть для HDInsight. HDInsight также не поддерживает ограничение исходящего сетевого подключение.

Дополнительные сведения см. в следующих статьях:

- [Что такое виртуальная сеть Azure?](../../virtual-network/virtual-networks-overview.md)
- [Расширение возможностей HDInsight с помощью виртуальной сети Azure](../hdinsight-plan-virtual-network-deployment.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Безопасное подключение к другим службам Azure с использованием конечных точек служб для виртуальной сети Azure

HDInsight поддерживает [конечные точки службы виртуальной сети](../../virtual-network/virtual-network-service-endpoints-overview.md), которые позволяют безопасно подключаться к хранилищу BLOB-объектов Azure, Azure Data Lake Storage 2-го поколения, Cosmos DB и базам данных SQL. Включив конечную точку службы для Azure HDInsight, вы обеспечите защищенный маршрут трафика из центра обработки данных Azure. Этот повышенный уровень безопасности на сетевом уровне позволяет заблокировать большие учетные записи хранилища данных для их конкретных виртуальных сетей (VNET) и по-прежнему использовать кластеры HDInsight для доступа и обработки этих данных.

Дополнительные сведения см. в следующих статьях:

- [Конечные точки служб для виртуальной сети](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [How to enhance HDInsight security with service endpoints](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/) (Как повысить безопасность HDInsight с помощью конечных точек службы)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>Подключение HDInsight к локальной сети

HDInsight можно подключить к локальной сети с помощью виртуальной сети Azure и VPN-шлюза. Чтобы установить подключение, можно использовать следующие действия:

- используйте HDInsight в виртуальной сети Azure, которая подключается к локальной сети;
- настройте разрешения DNS-имен между виртуальной и локальной сетями;
- настройте группы безопасности сети или определяемые пользователем маршруты для управления сетевым трафиком.

Дополнительные сведения см. в статье [Подключение HDInsight к локальной сети](../connect-on-premises-network.md)

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь со следующей статьей в этой серии: рекомендации по [хранению в локальной среде для Azure HDInsight Hadoop миграции](apache-hadoop-on-premises-migration-best-practices-storage.md).
