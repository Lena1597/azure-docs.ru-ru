---
title: Создание пространства имен и очереди Служебной шины Azure с помощью шаблона Azure
description: Краткое руководство. Создание пространства имен и очереди служебной шины с помощью шаблона диспетчера ресурсов Azure
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 978111596330d7d6b324c1ecc07fd424c7fd47b7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427008"
---
# <a name="quickstart-create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Краткое руководство. Создание пространства имен и очереди служебной шины с помощью шаблона диспетчера ресурсов Azure

В этой статье показывается, как использовать шаблон Azure Resource Manager, создающий пространство имен служебной шины и очередь в нем. Здесь объясняется, как указать развертываемые ресурсы и определить параметры, указываемые при развертывании. Этот шаблон можно использовать для собственных развертываний или настроить его в соответствии с вашими требованиями.

Дополнительные сведения о создании шаблонов см. в статье [Создание шаблонов Azure Resource Manager][Authoring Azure Resource Manager templates].

Полный шаблон приведен в разделе [Шаблон пространства имен служебной шины и очереди][Service Bus namespace and queue template] в GitHub.

> [!NOTE]
> Для скачивания и развертывания можно использовать указанные ниже шаблоны диспетчера ресурсов Azure.
> 
> * [Создание пространства имен служебной шины с очередью и правилом авторизации](service-bus-resource-manager-namespace-auth-rule.md)
> * [Создание пространства имен служебной шины с разделом и подпиской](service-bus-resource-manager-namespace-topic.md)
> * [Создайте пространство имен служебной шины](service-bus-resource-manager-namespace.md)
> * [Создание пространства имен служебной шины с разделом, подпиской и правилом с помощью шаблона Azure Resource Manager](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Чтобы узнать о новых шаблонах, в коллекции [шаблонов быстрого запуска Azure][Azure Quickstart Templates] выполните поиск по фразе **служебная шина**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-will-you-deploy"></a>Что вы развернете?

С помощью этого шаблона вы развернете пространство имен служебной шины с очередью.

[Очереди служебной шины](service-bus-queues-topics-subscriptions.md#queues) доставляют сообщения конкурирующим получателям по типу FIFO (в порядке очереди).

Чтобы выполнить развертывание автоматически, нажмите следующую кнопку.

[![Развертывание в Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Параметры

С помощью диспетчера ресурсов Azure можно определить параметры значений, которые должны указываться на этапе развертывания шаблона. В шаблоне есть раздел `Parameters` , содержащий все значения параметров. Для этих значений необходимо определить параметры, которые будут зависеть от развертываемого проекта либо от среды, в которой выполняется развертывание. Не задавайте параметры для значений, которые не меняются. Значение каждого параметра в шаблоне определяет развертываемые ресурсы.

Ниже описаны параметры, которые определяет шаблон.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Имя создаваемого пространства имен служебной шины.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
Имя очереди, создаваемой в пространстве имен служебной шины.

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
Версия API служебной шины для шаблона.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       }
```

## <a name="resources-to-deploy"></a>Развертываемые ресурсы
Создает стандартное пространство имен служебной шины типа **Messaging**с очередью.

```json
{
    "resources": [{
        "apiVersion": "2017-04-01",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
            "name": "Standard"
        },
        "properties": {},
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }]
}
```

Сведения о синтаксисе и свойствах JSON см. в справочнике по шаблонам о [пространствах имен](/azure/templates/microsoft.servicebus/namespaces) и [очередях](/azure/templates/microsoft.servicebus/namespaces/queues).

## <a name="commands-to-run-deployment"></a>Команды для выполнения развертывания
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell
New-AzResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Дальнейшие действия
См. следующий раздел, в котором показано, как создать правило авторизации для пространства имен или очереди: [Создание правила авторизации Служебной шины для пространства имен и очереди с помощью шаблона Azure Resource Manager](service-bus-resource-manager-namespace-auth-rule.md)

Узнайте, как управлять этими ресурсами, просмотрев следующие статьи:

* [Управление служебной шиной с помощью PowerShell](service-bus-manage-with-ps.md)
* [Управление ресурсами служебной шины с помощью обозревателя служебной шины](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
