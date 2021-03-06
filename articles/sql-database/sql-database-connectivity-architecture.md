---
title: Архитектура подключения
description: В этом документе объясняется архитектура подключения к базе данных Azure SQL в Azure или за пределами Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: fasttrack-edit
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 07/02/2019
ms.openlocfilehash: 6a90e9ba264c4abddf2c26cb7b1761a7a51b1778
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647685"
---
# <a name="azure-sql-connectivity-architecture"></a>Архитектура подключения к SQL Azure

В этой статье описывается архитектура подключения к базе данных SQL Azure и Хранилищу данных SQL Azure, а также как функционируют различные компоненты при передаче трафика к вашему экземпляру SQL Azure. Эти компоненты подключения направляют сетевой трафик к базе данных SQL Azure или Хранилищу данных SQL с помощью клиентов, подключающихся из Azure, и клиентов, подключающихся извне Azure. В этой статье также приведены примеры сценариев, позволяющие изменить параметры подключения, а также рекомендации по изменению параметров подключения по умолчанию.

## <a name="connectivity-architecture"></a>Архитектура подключения

На следующей схеме показана общая архитектура подключений к базе данных SQL Azure.

![Общий вид архитектуры](./media/sql-database-connectivity-architecture/connectivity-overview.png)

Следующие шаги описывают, как установить подключение к базе данных SQL Azure:

- Клиенты подключаются к шлюзу, который использует общедоступный IP-адрес и ожидает передачи данных через порт 1433.
- Шлюз, в зависимости от действующей политики подключения, перенаправляет или проксирует трафик к правильному кластеру баз данных.
- В кластере баз данных трафик перенаправляется в соответствующую базу данных SQL Azure.

## <a name="connection-policy"></a>Политика подключения

База данных SQL Azure поддерживает следующие три варианта для параметра политики подключения сервера Базы данных SQL.

- **Перенаправление (рекомендуется):** Клиенты устанавливают подключения непосредственно к узлу, на котором размещена база данных, что приводит к снижению задержки и повышению пропускной способности. Для подключений, использующих этот режим, клиенты должны
   - Разрешите входящий и исходящий обмен данными от клиента ко всем IP-адресам Azure в регионе на портах в диапазоне 11000 11999.  
   - Разрешите входящий и исходящий обмен данными между клиентом и IP-адресом шлюза базы данных SQL Azure через порт 1433.

- **Прокси-сервер:** В этом режиме все подключения предоставляются через шлюзы базы данных SQL Azure, что приводит к увеличению задержки и снижению пропускной способности. Для подключений, использующих этот режим, клиенты должны разрешать входящий и исходящий обмен данными между клиентом и IP-адресом шлюза базы данных SQL Azure через порт 1433.

- **По умолчанию:** Это политика подключения, которая действует на всех серверах после создания, если политика подключения не была явно изменена на `Proxy` или `Redirect`. Политика по умолчанию`Redirect` для всех подключений клиентов, исходящих внутри Azure (например, с виртуальной машины Azure), и `Proxy`для всех подключений клиентов, исходящих извне (например, подключения с локальной рабочей станции).

 Мы настоятельно рекомендуем `Redirect` политику подключения для `Proxy` политики подключения с минимальной задержкой и высокой пропускной способностью. Тем не менее вам потребуется выполнить дополнительные требования для разрешения сетевого трафика, как описано выше. Если клиент является виртуальной машиной Azure, это можно сделать с помощью групп безопасности сети (NSG) с [тегами службы](../virtual-network/security-overview.md#service-tags). Если клиент подключается из локальной рабочей станции, вам может потребоваться обратиться к администратору сети, чтобы разрешить сетевой трафик через корпоративный брандмауэр.

## <a name="connectivity-from-within-azure"></a>Подключение из Azure

Для подключений из Azure по умолчанию действует политика подключения `Redirect`. Политика подключения `Redirect` означает, что после установления сеанса TCP с базой данных SQL Azure сеанс клиента перенаправляется в правильный кластер баз данных с измененным виртуальным IP-адресом назначения, то есть вместо адреса шлюза Базы данных SQL Azure указывается адрес кластера. После этого все последующие пакеты передаются непосредственно через кластер, минуя шлюз Базы данных SQL Azure. Этот поток трафика представлен на схеме ниже.

![Общий вид архитектуры](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Подключение извне Azure

Для подключений извне Azure по умолчанию действует политика подключения `Proxy`. Политика `Proxy` означает, что устанавливается сеанс TCP через шлюз Базы данных SQL Azure и все последующие пакеты проходят через этот шлюз. Этот поток трафика представлен на схеме ниже.

![Общий вид архитектуры](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> Дополнительно откройте порты 14000-14999, чтобы включить [подключение с помощью DAC](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) .


## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-адреса шлюза Базы данных SQL Azure

В следующей таблице перечислены IP-адреса шлюзов по регионам. Чтобы подключиться к базе данных SQL Azure, необходимо разрешить & сетевого трафика со **всех** шлюзов для региона.

Сведения о переносе трафика в новые шлюзы в конкретных регионах см. в следующей статье: [Миграция трафика базы данных SQL Azure на новые шлюзы](sql-database-gateway-migration.md) .


| Название региона          | IP-адреса шлюза |
| --- | --- |
| Центральная Австралия    | 20.36.105.0 |
| Central2 Австралия   | 20.36.113.0 |
| Восточная Австралия       | 13.75.149.87, 40.79.161.1 |
| Юго-Восточная Австралия | 191.239.192.109, 13.73.109.251 |
| Южная Бразилия         | 104.41.11.5, 191.233.200.14 |
| Центральная Канада       | 40.85.224.249      |
| Восточная Канада          | 40.86.226.166      |
| Центральная часть США           | 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96, 104.208.21.1 | 
| Восточный Китай           | 139.219.130.35     |
| Восточный Китай 2         | 40.73.82.1         |
| Северный Китай          | 139.219.15.17      |
| Северный Китай 2        | 40.73.50.0         |
| Восточная Азия            | 191.234.2.139, 52.175.33.150, 13.75.32.4 |
| Восточная часть США              | 40.121.158.30, 40.79.153.12, 191.238.6.43, 40.78.225.32 |
| Восточная часть США 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0, 191.239.224.107, 104.208.150.3 | 
| Центральная Франция       | 40.79.137.0, 40.79.129.1 |
| Центральная Германия      | 51.4.144.100       |
| Северо-восточная Германия   | 51.5.144.179       |
| Центральная Индия        | 104.211.96.159     |
| Южная Индия          | 104.211.224.146    |
| Западная Индия           | 104.211.160.80     |
| Восточная Япония           | 13.78.61.196, 40.79.184.8, 13.78.106.224, 191.237.240.43, 40.79.192.5 | 
| Западная Япония           | 104.214.148.156, 40.74.100.192, 191.238.68.11, 40.74.97.10 | 
| Республика Корея, центральный регион        | 52.231.32.42       |
| Республика Корея, южный регион          | 52.231.200.86      |
| Центрально-северная часть США     | 23.96.178.199, 23.98.55.75, 52.162.104.33 |
| Северная Европа         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| Северная часть ЮАР   | 102.133.152.0      |
| Западная часть ЮАР    | 102.133.24.0       |
| Центрально-южная часть США     | 13.66.62.124, 23.98.162.75, 104.214.16.32   | 
| Юго-Восточная Азия      | 104.43.15.0, 23.100.117.95, 40.78.232.3   | 
| Центральная часть ОАЭ          | 20.37.72.64        |
| Северная часть ОАЭ            | 65.52.248.0        |
| Южная часть Соединенного Королевства             | 51.140.184.11      |
| Западная часть Соединенного Королевства              | 51.141.8.11        |
| Центрально-западная часть США      | 13.78.145.25       |
| Западная Европа          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| Западная часть США              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| Западная часть США 2            | 13.66.226.202      |
|                      |                    |

## <a name="change-azure-sql-database-connection-policy"></a>Изменение политики подключения для Базы данных SQL Azure

Чтобы изменить политику подключения Базы данных SQL Azure для сервера Базы данных SQL Azure, используйте команду [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

- Если задана политика подключения `Proxy`, то все сетевые пакеты проходят через шлюз Базы данных SQL Azure. Для этого режима необходимо разрешить исходящий трафик только через IP-адрес шлюза Базы данных SQL Azure. В режиме `Proxy` задержка выше, чем в режиме `Redirect`.
- Если задана политика подключения `Redirect`, то все сетевые пакеты проходят напрямую в кластер баз данных. Для этого режима необходимо разрешить исходящий трафик для нескольких IP-адресов.

## <a name="script-to-change-connection-settings-via-powershell"></a>Сценарий для изменения параметров подключения через PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Модуль PowerShell Azure Resource Manager по-прежнему поддерживается базой данных SQL Azure, но вся будущая разработка предназначена для модуля AZ. SQL. Эти командлеты см. в разделе [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Аргументы для команд в модуле AZ и в модулях AzureRm существенно идентичны. Для следующего скрипта требуется [модуль Azure PowerShell](/powershell/azure/install-az-ps).

Приведенный ниже сценарий PowerShell показывает, как изменить политику подключения.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Сценарий изменения параметров подключения через Azure CLI

> [!IMPORTANT]
> Для работы этого сценария требуется [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI в оболочке bash

> [!IMPORTANT]
> Для работы этого сценария требуется [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

В следующем сценарии интерфейса командной строки показано, как изменить политику подключения в оболочке bash.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Azure CLI из командной строки Windows

> [!IMPORTANT]
> Для работы этого сценария требуется [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

В следующем сценарии CLI показано, как изменить политику подключения из командной строки Windows (с Azure CLI установленным).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о том, как изменить политику подключения Базы данных SQL Azure для сервера Базы данных SQL Azure, см. в статье о команде [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Сведения о поведении подключения к базе данных SQL Azure клиентов, использующих ADO.NET 4.5 или более поздней версии, см. в разделе [Порты для ADO.NET 4.5, отличные от порта 1433](sql-database-develop-direct-route-ports-adonet-v12.md).
- Общая информация о разработке приложений приведена в разделе [Обзор разработки приложений баз данных SQL](sql-database-develop-overview.md).
