---
title: Запрос данных с помощью библиотеки Python в Azure Data Explorer
description: Из этой статьи вы узнаете, как запрашивать данные из Azure обозреватель данных с помощью Python.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 2c32e67bb2b47a24036a341ea4e1b83037abbaee
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827527"
---
# <a name="query-data-using-the-azure-data-explorer-python-library"></a>Запрос данных с помощью библиотеки Python в Azure Data Explorer

Обозреватель данных Azure — это быстрая и высокомасштабируемая служба для изучения данных журналов и телеметрии. Обозреватель данных Azure предоставляет [клиентскую библиотеку данных для Python](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Эта библиотека позволяет выполнять запросы данных из кода. В этой статье вы подключитесь к таблице в кластере *справки* , который мы настроили для помощи в обучении. Затем вы запросите таблицу в этом кластере и получите результаты.

Эта статья также доступна в качестве [записной книжки Azure](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueryKusto.ipynb).

## <a name="prerequisites"></a>предварительные требования

* Учетная запись электронной почты организации, которая входит в Azure Active Directory (AAD)

* [Python](https://www.python.org/downloads/) на компьютере, на котором ведется разработка

## <a name="install-the-data-library"></a>Установка библиотеки данных

Установите библиотеку *azure-kusto-data*.

```
pip install azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Добавление операторов и констант импорта

Импортируйте классы из библиотеки, а также *pandas*, библиотеку анализа данных.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
from azure.kusto.data.helpers import dataframe_from_result_table
import pandas as pd
```

Чтобы проверить подлинность приложения, обозреватель данных Azure использует идентификатор клиента AAD. Чтобы найти идентификатор клиента, используйте следующий URL-адрес, заменив домен на *имя_вашего_домена*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Например, если ваш домен называется *contoso.com*, URL-адрес будет следующим: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Щелкните этот URL-адрес, чтобы просмотреть результаты. Первая строка выглядит следующим образом:

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

В данном случае идентификатор клиента — `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. Перед выполнением этого кода задайте значение для AAD_TENANT_ID.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_CLUSTER = "https://help.kusto.windows.net/"
KUSTO_DATABASE = "Samples"
```

Создайте строку подключения. В этом примере используется проверка подлинности устройства для доступа к кластеру. Можно также использовать [сертификат приложения AAD](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L24), [ключ приложения AAD](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L20) и [имя пользователя и пароль AAD](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py#L34).

```python
KCSB = KustoConnectionStringBuilder.with_aad_device_authentication(
    KUSTO_CLUSTER)
KCSB.authority_id = AAD_TENANT_ID
```

## <a name="connect-to-azure-data-explorer-and-execute-a-query"></a>Подключение к обозревателю данных Azure и выполнение запроса

Выполните запрос к кластеру и сохраните выходные данных в кадр данных. При выполнении этого кода возвращается примерно такое сообщение: *Чтобы войти, воспользуйтесь браузером и откройте страницу https://microsoft.com/devicelogin. Введите код F3W4VWZDM для прохождения проверки подлинности*. Следуйте инструкциям по входу, а затем выполните следующий блок кода.

```python
KUSTO_CLIENT = KustoClient(KCSB)
KUSTO_QUERY = "StormEvents | sort by StartTime desc | take 10"

RESPONSE = KUSTO_CLIENT.execute(KUSTO_DATABASE, KUSTO_QUERY)
```

## <a name="explore-data-in-dataframe"></a>Изучение данных в кадре данных

После входа запрос возвращает результаты, которые сохраняются в кадре данных. С результатами можно работать так же, как с любыми кадрами данных.

```python
df = dataframe_from_result_table(RESPONSE.primary_results[0])
df
```

Вы увидите десять самых первых результатов из таблицы StormEvents.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Краткое руководство. Прием данных с помощью библиотеки Python в Azure Data Explorer](python-ingest-data.md)
