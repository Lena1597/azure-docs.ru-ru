---
title: Подключение к Azure Data Explorer из Azure Databricks с помощью Python
description: Здесь показано, как использовать библиотеку Python в Azure Databricks для доступа к данным из Azure Data Explorer с помощью одного из двух методов проверки подлинности.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 03dee0570faa863ca411ed91f2a6ec85a1e38380
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985685"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-by-using-python"></a>Подключение к Azure Data Explorer из Azure Databricks с помощью Python

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) — это платформа аналитики на основе Apache Spark, оптимизированная для платформы Microsoft Azure. В этой статье показано, как использовать библиотеку Python в Azure Databricks для доступа к данным из Azure Data Explorer. Существует несколько способов выполнения проверки подлинности в Azure Data Explorer, включая имя пользователя устройства и приложение Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Технические условия

- [Создайте кластер и базу данных Azure Data Explorer](/azure/data-explorer/create-cluster-database-portal).
- [Создайте рабочую область Azure Databricks](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace). В разделе **Служба Azure Databricks** в раскрывающемся списке **Ценовая категория** выберите **Премиум**. Это дает возможность использовать секреты Azure Databricks для хранения учетных данных и ссылаться на них в записных книжках и заданиях.

- [Создайте кластер](https://docs.azuredatabricks.net/user-guide/clusters/create.html) в Azure Databricks со следующими спецификациями (минимальные параметры, необходимые для запуска примеров записных книжек):

   ![Спецификации для создания кластера](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-the-python-library-on-your-azure-databricks-cluster"></a>Установка библиотеки Python в кластере Azure Databricks

Чтобы установить [библиотеки Python](/azure/kusto/api/python/kusto-python-client-library) в кластере Azure Databricks, выполните следующие действия.

1. Перейдите в рабочую область Azure Databricks и [создайте библиотеку](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library).
2. [Передайте пакет Python PyPI или Python Egg](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg).
   - Загрузите, установите и подключите библиотеку к кластеру Databricks.
   - Введите имя PyPI **azure-kusto-data**.

## <a name="connect-to-azure-data-explorer-by-using-a-device-login"></a>Подключение к Azure Data Explorer с помощью учетной записи устройства

[Импортируйте записную книжку](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) с помощью записной книжки[Query-ADX-device-login](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb). Затем вы можете подключиться к Azure Data Explorer, используя свои учетные данные.

## <a name="connect-to-adx-by-using-an-azure-ad-app"></a>Подключение к ADX с помощью приложения Azure AD

1. Создайте приложение Azure AD, выполнив [подготовку приложения Azure AD](/azure/kusto/management/access-control/how-to-provision-aad-app).
1. Предоставьте доступ к Azure AD App в вашей базе данных Azure Data Explorer следующим образом:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | имя базы данных |
    | ```AAD App ID``` | идентификатор приложения Azure AD |
    | ```AAD Tenant ID``` | идентификатор клиента Azure AD |

### <a name="find-your-azure-ad-tenant-id"></a>Поиск идентификатора клиента Azure AD

Чтобы проверить подлинность приложения, Azure Data Explorer использует идентификатор клиента Azure AD. Чтобы найти идентификатор клиента, используйте следующий URL-адрес. Замените его доменом *имя_домена*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Например, если ваш домен называется *contoso.com*, URL-адрес будет следующим: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Чтобы просмотреть результаты выберите этот URL-адрес. Первая строка выглядит следующим образом: 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Ваш идентификатор клиента — `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Хранение и защита идентификатора и ключа приложения Azure AD 

Храните и защищайте идентификатор и ключ приложения Azure AD с помощью [секретов](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) Azure Databricks следующим образом:
1. [Настройте интерфейс командной строки](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli).
1. [Установите интерфейс командной строки](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 
1. [Настройте проверку подлинности](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Настройте [секреты](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) с помощью приведенных ниже примеров команд:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Импорт записной книжки
[Импортируйте записную книжку](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook), используя экземпляр [Query-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb), для подключения к Azure Data Explorer. Замените значения заполнителей собственными именами кластера и базы данных, а также идентификатором клиента Azure AD.
