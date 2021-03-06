---
title: Создание кластера и базы данных Azure Data Explorer с использованием Python
description: Сведения о создании кластера и базы данных Azure Data Explorer с использованием Python.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 8d43965e87ab57d9f0c79c6661a761b06ccb7073
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902101"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Создание кластера и базы данных Azure Data Explorer с использованием Python

> [!div class="op_single_selector"]
> * [Портал](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Шаблон ARM](create-cluster-database-resource-manager.md)

Azure Data Explorer — это быстрая и полностью управляемая служба для анализа большого объема потоковых данных в реальном времени из приложений, с веб-сайтов, с устройств Интернета вещей и т. д. Чтобы использовать обозреватель данных Azure, сначала нужно создать кластер и одну или несколько баз данных в этом кластере. Затем вы должны принять (загрузить) данные в базы данных, чтобы к ним можно было выполнять запросы. В этой статье вы создадите кластер и базу данных с помощью Python.

## <a name="prerequisites"></a>Технические условия

Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/) Azure, прежде чем начинать работу.

## <a name="install-python-package"></a>Установка пакета Python

Чтобы установить пакет Python для Azure Data Explorer (Kusto), откройте окно командной строки с Python в пути. Выполните следующую команду:

```
pip install azure-common
pip install azure-mgmt-kusto
```
## <a name="authentication"></a>Проверка подлинности
Для выполнения примеров в этой статье нам потребуется приложение Azure AD и субъект-служба, которые могут получать доступ к ресурсам. Установите флажок [создать приложение Azure AD](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) , чтобы создать бесплатное приложение Azure AD и назначить роль в области действия подписки. В нем также показано, как получить `Directory (tenant) ID`, `Application ID`и `Client Secret`.

## <a name="create-the-azure-data-explorer-cluster"></a>Создание кластера Azure Data Explorer

1. Создайте кластер, используя приведенную ниже команду:

    ```Python
    from azure.mgmt.kusto import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku
    from azure.common.credentials import ServicePrincipalCredentials

    #Directory (tenant) ID
    tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    #Application ID
    client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    #Client Secret
    client_secret = "xxxxxxxxxxxxxx"
    subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )

    location = 'Central US'
    sku_name = 'Standard_D13_v2'
    capacity = 5
    tier = "Standard"
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku_name, capacity=capacity, tier=tier))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    poller = cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Необходимое имя кластера.|
   | sku_name | *Standard_D13_v2* | Номер SKU, который будет использоваться для кластера. |
   | Уровень | *Standard Edition* | Уровень SKU. |
   | ресурсов | *number* | Число экземпляров кластера. |
   | resource_group_name | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |

    > [!NOTE]
    > **Создание кластера** — это длительная операция. Метод **create_or_update** возвращает экземпляр лрополлер, см. раздел [класс лрополлер](/python/api/msrest/msrest.polling.lropoller?view=azure-python) для получения дополнительных сведений.

1. Чтобы проверить, успешно ли создан кластер, можно выполнить следующую команду:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Если результат содержит параметр `provisioningState` со значением `Succeeded`, это означает, что кластер создан успешно.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Создание базы данных в кластере Azure Data Explorer

1. Создайте базу данных, используя приведенную ниже команду:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = ReadWriteDatabase(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    #Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
    poller =database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

        [!NOTE]
        If you are using Python version 0.4.0 or below, use Database instead of ReadWriteDatabase.

   |**Параметр** | **Рекомендуемое значение** | **Описание поля**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Имя кластера, в котором необходимо создать базу данных.|
   | database_name | *mykustodatabase* | Имя базы данных.|
   | resource_group_name | *testrg* | Имя группы ресурсов, в которой будет создан кластер. |
   | soft_delete_period | *3650 days, 0:00:00* | Время, на протяжении которого данные будут храниться таким образом, чтобы они были доступны для запроса. |
   | hot_cache_period | *3650 days, 0:00:00* | Время, на протяжении которого данные будут храниться в кэше. |

1. Выполните следующую команду, чтобы просмотреть созданную базу данных:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Теперь у вас есть кластер и база данных.

## <a name="clean-up-resources"></a>Очистка ресурсов

* Если вы планируете следовать нашим другим статьям, не заполняйте созданные ресурсы.
* Чтобы очистить ресурсы, удалите кластер. При удалении кластера также удаляются все содержащиеся в нем базы данных. Для удаления кластера используйте следующую команду:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Прием данных с помощью библиотеки Python в Azure Data Explorer](python-ingest-data.md)
