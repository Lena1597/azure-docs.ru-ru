---
title: Управление IoT Central в Azure PowerShell | Документация Майкрософт
description: В этой статье описывается создание приложений IoT Central и управление ими из Azure PowerShell.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 1598451ce184db5a25cac28870b70a446aef123c
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198826"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Управление IoT Central в Azure PowerShell

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Вместо создания приложений IoT Central и управления ими на веб-сайте [диспетчера приложений Azure IOT Central](https://aka.ms/iotcentral) можно использовать [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) для управления приложениями.

## <a name="prerequisites"></a>Предварительные требования

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы хотите запустить Azure PowerShell на локальном компьютере, ознакомьтесь с разделом об [установке модуля Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps). При локальном запуске Azure PowerShell используйте командлет **Connect-AzAccount**, чтобы войти в Azure, прежде чем использовать командлеты, представленные в этой статье.

> [!TIP]
> Если вам нужно выполнить команды PowerShell в другой подписке Azure, см. статью [Изменение активной подписки](/powershell/azure/manage-subscriptions-azureps?view=azps-3.4.0#change-the-active-subscription).

## <a name="install-the-iot-central-module"></a>Установка модуля IoT Central

Выполните следующую команду, чтобы проверить, установлен ли [модуль IoT Central](https://docs.microsoft.com/powershell/module/az.iotcentral/) в среду PowerShell.

```powershell
Get-InstalledModule -name Az.I*
```

Если в списке установленных модулей нет **Az.IotCentral**, выполните следующую команду.

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Создание приложения

Используйте командлет [New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp), чтобы создать приложение IoT Central в подписке Azure. Например:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "ST1" -Template "iotc-pnp-preview@1.0.0" `
  -DisplayName "My Custom Display Name"
```

Сначала скрипт создает группу ресурсов в регионе восточной части США для приложения. В следующей таблице описаны параметры, используемые в команде **New-AzIotCentralApp**:

|Параметр         |Описание |
|------------------|------------|
|ResourceGroupName |Группа ресурсов, в которой содержится приложение. В подписке уже должна существовать эта группа ресурсов. |
|Расположение |По умолчанию этот командлет использует расположение из группы ресурсов. В настоящее время можно создать IoT Centralное приложение в **Австралии**, **Азиатско-Тихоокеанский регион**, **Европе**или **США** географических регионах.  |
|Имя              |Имя приложения на портале Azure. |
|Поддомен         |Поддомен в URL-адресе приложения. В примере URL-адрес приложения — https://mysubdomain.azureiotcentral.com. |
|Sku               |В настоящее время можно использовать либо **ST1** , либо **ST2**. Ознакомьтесь с разделом [Цены на Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
|Шаблон          | Шаблон приложения для использования. Дополнительные сведения приведены в таблице ниже. |
|DisplayName       |Имя приложения, отображаемое на пользовательском интерфейсе. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-iot-central-applications"></a>Просмотр приложений IoT Central

Используйте командлет [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp), чтобы получить список приложений IoT Central и просмотреть метаданные.

## <a name="modify-an-application"></a>Изменение приложения

Используйте командлет [Set-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) для обновления метаданных приложения IoT Central. Например, чтобы изменить отображаемое имя приложения:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Удаление приложения

Используйте командлет [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) для удаления приложения IoT Central. Например:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Следующие шаги

Вы узнали, как управлять приложениями Azure IoT Central в Azure PowerShell, а значит, вы готовы к следующему шагу:

> [!div class="nextstepaction"]
> [Администрирование приложения](howto-administer.md)
