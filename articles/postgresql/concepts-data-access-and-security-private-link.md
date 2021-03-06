---
title: Частная ссылка для базы данных Azure для PostgreSQL — один сервер (Предварительная версия)
description: Узнайте, как работает частная связь для базы данных Azure для PostgreSQL-Single Server.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: e3667a60a326838b490f9082fd55bdc92d038cf9
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898369"
---
# <a name="private-link-for-azure-database-for-postgresql-single-server-preview"></a>Частная ссылка для базы данных Azure для PostgreSQL — один сервер (Предварительная версия)

Частная ссылка позволяет подключаться к различным службам PaaS в Azure с помощью частной конечной точки. Частная связь Azure, по сути, предоставляет службы Azure в частной виртуальной сети (VNet). Доступ к ресурсам PaaS можно получить, используя частный IP-адрес, как и любой другой ресурс в виртуальной сети.

Чтобы получить список служб PaaS, поддерживающих функции закрытых ссылок, ознакомьтесь с [документацией по](https://docs.microsoft.com/azure/private-link/index)частному каналу. Частная конечная точка — это частный IP-адрес в определенной [виртуальной сети](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) и подсети.

> [!NOTE]
> Эта функция доступна во всех регионах Azure, где база данных Azure для PostgreSQL поддерживает общего назначения и ценовые категории, оптимизированные для памяти.

## <a name="data-exfiltration-prevention"></a>Предотвращение кражи данных

Data ex-фильтрация в базе данных Azure для PostgreSQL Single Server — когда полномочный пользователь, например администратор базы данных, может извлекать данные из одной системы и перемещать их в другое расположение или систему за пределами Организации. Например, пользователь перемещает данные в учетную запись хранения, принадлежащую третьей стороне.

Рассмотрим ситуацию с пользователем, работающим с PGAdmin, в виртуальной машине Azure, которая подключается к базе данных Azure для PostgreSQL с помощью одного сервера, подготовленного в западной части США. В приведенном ниже примере показано, как ограничить доступ с помощью общедоступных конечных точек в базе данных Azure для PostgreSQL на одном сервере с использованием средств управления доступом к сети.

* Отключите весь трафик службы Azure к базе данных Azure для PostgreSQL с помощью общедоступной конечной точки, установив флажок *Разрешить службам Azure* отключать. Убедитесь, что IP-адреса или диапазоны не разрешены для доступа к серверу через [правила брандмауэра](https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules) или [конечные точки службы виртуальной сети](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet).

* Разрешите трафик только для одного сервера базы данных Azure для PostgreSQL с использованием частного IP-адреса виртуальной машины. Дополнительные сведения см. в статьях по правилам [конечной точки службы](concepts-data-access-and-security-vnet.md) и [брандмауэра виртуальной сети.](howto-manage-vnet-using-portal.md)

* На виртуальной машине Azure Ограничьте область исходящего подключения, используя группы безопасности сети (группы безопасности сети) и теги службы, как показано ниже.

    * Укажите правило NSG, разрешающее трафик для *тега службы = SQL. WestUS* — разрешает подключение к базе данных Azure для PostgreSQL только для одного сервера в западной части США
    * Укажите правило NSG (с более высоким приоритетом) для запрета трафика для *тега службы = SQL* — запрет подключений к базе данных PostgreSQL во всех регионах</br></br>

После завершения установки виртуальная машина Azure может подключаться только к серверу базы данных Azure для PostgreSQL в регионе "Западная часть США". Однако подключение не ограничено одной базой данных Azure для PostgreSQL одного сервера. Виртуальная машина по-прежнему может подключаться к любому отдельному серверу базы данных Azure для PostgreSQL в регионе "Западная часть США", включая базы данных, которые не входят в подписку. Хотя мы сократили масштабы кражи данных в приведенном выше сценарии до определенного региона, мы не полностью устранили ее.</br>

С помощью частного канала теперь можно настроить такие элементы управления доступом к сети, как группы безопасности сети, чтобы ограничить доступ к частной конечной точке. Затем отдельные ресурсы Azure PaaS сопоставляются с конкретными частными конечными точками. Вредоносная программа предварительной оценки может получить доступ только к сопоставленному ресурсу PaaS (например, к базе данных Azure для PostgreSQL одного сервера) и к другому ресурсу.

## <a name="on-premises-connectivity-over-private-peering"></a>Локальные подключения через частный пиринг

При подключении к общедоступной конечной точке из локальных компьютеров необходимо добавить IP-адрес в брандмауэр на основе IP-адресов, используя правило брандмауэра уровня сервера. Хотя эта модель отлично подходит для предоставления доступа к отдельным компьютерам для рабочих нагрузок разработки или тестирования, управлять ею в рабочей среде сложно.

С помощью закрытой ссылки можно включить локальный доступ к частной конечной точке с помощью [Express Route](https://azure.microsoft.com/services/expressroute/) (ER), частного пиринга или [VPN-туннеля](https://docs.microsoft.com/azure/vpn-gateway/). Затем они могут отключить доступ через общедоступную конечную точку и не использовать брандмауэр на основе IP-адресов.

## <a name="configure-private-link-for-azure-database-for-postgresql-single-server"></a>Настройка частного канала для базы данных Azure для PostgreSQL одиночного сервера

### <a name="creation-process"></a>Процесс создания

Для включения закрытой ссылки требуются частные конечные точки. Это можно сделать с помощью следующих руководств.

* [Портал Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal)
* [CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-cli)

### <a name="approval-process"></a>Процесс утверждения
После того как администратор сети создаст частную конечную точку (PE), администратор PostgreSQL может управлять подключением к частной конечной точке (PEC) к базе данных Azure для PostgreSQL.

> [!NOTE]
> Сейчас база данных Azure для PostgreSQL Single Server поддерживает только автоматическое утверждение для частной конечной точки.

* Перейдите к ресурсу сервера базы данных Azure для PostgreSQL в портал Azure. 
    * Выберите подключения частной конечной точки в левой области.
    * Отображает список всех подключений к частным конечным точкам (ПЕКС)
    * Соответствующая частная конечная точка (PE) создана

![Выбор закрытого портала конечной точки](media/concepts-data-access-and-security-private-link/select-private-link-portal.png)

* Выберите отдельное PEC из списка, щелкнув его.

![Выберите закрытую конечную точку, ожидающие утверждения](media/concepts-data-access-and-security-private-link/select-private-link.png)

* Администратор сервера PostgreSQL может одобрить или отклонить PEC и при необходимости добавить короткий текст ответа.

![Выберите сообщение частной конечной точки](media/concepts-data-access-and-security-private-link/select-private-link-message.png)

* После утверждения или отклонения список будет отражать соответствующее состояние вместе с текстом ответа.

![выберите конечное состояние частной конечной точки](media/concepts-data-access-and-security-private-link/show-private-link-approved-connection.png)

## <a name="use-cases-of-private-link-for-azure-database-for-postgresql"></a>Варианты использования частной ссылки для базы данных Azure для PostgreSQL

Клиенты могут подключаться к частной конечной точке из той же виртуальной сети, одноранговой виртуальной сети в том же регионе или через подключение типа "виртуальная сеть — виртуальная сеть" между регионами. Кроме того, клиенты могут подключаться из локальной среды с помощью ExpressRoute, частного пиринга или VPN-туннелирования. Ниже приведена упрощенная схема, на которой показаны распространенные варианты использования.

![Выберите Общие сведения о конечной точке частного назначения](media/concepts-data-access-and-security-private-link/show-private-link-overview.png)

### <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Подключение из виртуальной машины Azure в одноранговой виртуальной сети (VNet)
Настройте [пиринг виртуальных сетей](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-powershell) , чтобы установить подключение к базе данных Azure для PostgreSQL-Single Server из виртуальной машины Azure в одноранговой виртуальной сети.

### <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>Подключение из виртуальной машины Azure в среде виртуальных сетей
Настройте [подключение VPN-шлюза](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) между виртуальными сетями, чтобы установить подключение к базе данных Azure для PostgreSQL-Single Server из виртуальной машины Azure в другом регионе или подписке.

### <a name="connecting-from-an-on-premises-environment-over-vpn"></a>Подключение из локальной среды через VPN
Чтобы установить подключение из локальной среды к базе данных Azure для PostgreSQL-Single Server, выберите и реализуйте один из следующих вариантов:

* [Подключение "точка — сеть"](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)
* [VPN-подключение типа "сеть —сеть"](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)
* [Канал ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager)

## <a name="private-link-combined-with-firewall-rules"></a>Частная ссылка в сочетании с правилами брандмауэра

При использовании частной ссылки в сочетании с правилами брандмауэра возможны следующие ситуации и результаты.

* Если не настроить правила брандмауэра, по умолчанию трафик не сможет получить доступ к базе данных Azure для PostgreSQL на одном сервере.

* Если вы настраиваете общедоступный трафик или конечную точку службы и создаете частные конечные точки, то соответствующий тип правила брандмауэра разрешается различными типами входящего трафика.

* Если вы не настраиваете общедоступную конечную точку и не создаете частные конечные точки, то один сервер базы данных Azure для PostgreSQL доступен только через частные конечные точки. Если не настроить общедоступный трафик или конечную точку службы после того, как все утвержденные частные конечные точки будут отклонены или удалены, трафик не сможет получить доступ к базе данных Azure для PostgreSQL на одном сервере.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о функциях безопасности для одного сервера базы данных Azure для PostgreSQL см. в следующих статьях:

* Сведения о настройке брандмауэра для одного сервера базы данных Azure для PostgreSQL см. в разделе [Поддержка брандмауэра](https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules).

* Чтобы узнать, как настроить конечную точку службы виртуальной сети для базы данных Azure для PostgreSQL на одном сервере, см. раздел [Настройка доступа из виртуальных сетей](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet).

* Общие сведения о подключении к базе данных Azure для PostgreSQL с одним сервером см. в статье [архитектура службы "база данных Azure для PostgreSQL](https://docs.microsoft.com/azure/postgresql/concepts-connectivity-architecture) ".