---
title: Примеры шаблонов Azure Resource Manager для виртуальной сети | Документация Майкрософт
description: Сведения о разных шаблонах Azure Resource Manager для развертывания виртуальных сетей Azure.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 5971b3251812ea4193e71173943ffc0d48207567
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75975367"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Примеры шаблонов Azure Resource Manager для виртуальной сети

В следующей таблице представлены ссылки на примеры шаблонов Azure Resource Manager. Шаблон можно развернуть с помощью [портала Azure](../azure-resource-manager/templates/deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) или [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json). См. дополнительные сведения о [создании шаблонов ](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json), а также [структуре и синтаксисе шаблонов Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Синтаксис JSON и используемые в шаблонах свойства см. в статье о [типах ресурсов Microsoft.Network](/azure/templates/microsoft.network/allversions).


| Задача | Description |
|----|----|
|[Создание виртуальной сети с двумя подсетями](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| Создание виртуальной сети с двумя подсетями.|
|[Маршрутизация трафика через виртуальный сетевой модуль](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Создание виртуальной сети с тремя подсетями. Развертывание виртуальной машины в каждой подсети. Создание таблицы маршрутов, содержащей маршруты для направления трафика из одной подсети в другую через виртуальную машину в третьей подсети. Связывание таблицы маршрутов и одной из подсетей.|
|[Создание конечной точки службы для виртуальной сети для службы хранилища Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|Создание виртуальной сети с двумя подсетями и одной виртуальной машиной в каждой из них. Включение конечной точки службы хранилища Azure для одной из подсетей, а также защита новой учетной записи хранения в этой подсети.|
|[Подключение между двумя виртуальными сетями](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Создание двух виртуальных сетей и пирингового подключения между ними.|
|[Создание виртуальной машины с несколькими IP-адресами](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Создание виртуальной машины Windows или Linux с несколькими IP-адресами.|
|[Настройка виртуальной сети с двойным стеком (IPv4 и IPv6)](https://github.com/Azure/azure-quickstart-templates/tree/master/ipv6-in-vnet)|Развертывание виртуальной сети с двойным стеком (IPv4 и IPv6), двумя виртуальными машинами и Azure Load Balancer категории "Базовый" с общедоступными IP-адресами IPv4 и IPv6. |
