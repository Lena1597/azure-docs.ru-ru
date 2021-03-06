---
title: Развертывание выделенных узлов Azure с помощью Azure PowerShell
description: Развертывание виртуальных машин на выделенных узлах с помощью Azure PowerShell.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/01/2019
ms.author: cynthn
ms.openlocfilehash: 5cd82635f3aec2cca251e122aadf96f70d377c8a
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190517"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-azure-powershell"></a>Развертывание виртуальных машин на выделенных узлах с помощью Azure PowerShell

В этой статье рассказывается, как создать [выделенный узел](dedicated-hosts.md) Azure для размещения виртуальных машин. 

Убедитесь, что установлен Azure PowerShell версии 2.8.0 или более поздней, и вы вошли в учетную запись Azure в с помощью `Connect-AzAccount`. 

## <a name="limitations"></a>Ограничения

- Масштабируемые наборы виртуальных машин в настоящее время не поддерживаются на выделенных узлах.
- Поддерживаются следующие серии виртуальных машин: DSv3, ESv3 и серия fsv2. 

## <a name="create-a-host-group"></a>Создание группы узлов

**Группа узлов** — это ресурс, представляющий коллекцию выделенных узлов. Вы создаете группу узлов в регионе и зону доступности и добавляете к ней узлы. При планировании высокого уровня доступности доступны дополнительные параметры. Для выделенных узлов можно использовать один или оба следующих варианта. 
- Охват нескольких зон доступности. В этом случае требуется группа узлов в каждой из зон, которые вы хотите использовать.
- Охватывать несколько доменов сбоя, сопоставленных с физическими стойками. 
 
В любом случае необходимо указать количество доменов сбоя для группы узлов. Если вы не хотите охватывать домены сбоя в группе, используйте число доменов сбоя, равное 1. 

Также можно выбрать использование зон доступности и доменов сбоя. В этом примере создается группа узлов в зоне 1 с 2 доменами сбоя. 


```powershell
$rgName = "myDHResourceGroup"
$location = "East US"

New-AzResourceGroup -Location $location -Name $rgName
$hostGroup = New-AzHostGroup `
   -Location $location `
   -Name myHostGroup `
   -PlatformFaultDomain 2 `
   -ResourceGroupName $rgName `
   -Zone 1
```

## <a name="create-a-host"></a>Создание узла

Теперь создадим выделенный узел в группе узлов. Помимо имени узла, необходимо указать номер SKU для узла. Номер SKU узла фиксирует поддерживаемую серию виртуальных машин, а также создание оборудования для выделенного узла.


Дополнительные сведения о номерах SKU узла и ценах см. на странице [цен на выделенный узел Azure](https://aka.ms/ADHPricing).

Если для группы узлов задано число доменов сбоя, вам будет предложено указать домен сбоя для узла. В этом примере мы устанавливаем домен сбоя для узла в значение 1.


```powershell
$dHost = New-AzHost `
   -HostGroupName $hostGroup.Name `
   -Location $location -Name myHost `
   -ResourceGroupName $rgName `
   -Sku DSv3-Type1 `
   -AutoReplaceOnFailure 1 `
   -PlatformFaultDomain 1
```

## <a name="create-a-vm"></a>Создание виртуальной машины

Создайте виртуальную машину на выделенном узле. 

Если при создании группы узлов была указана зона доступности, то при создании виртуальной машины необходимо будет использовать ту же зону. В этом примере, так как наша группа узлов находится в зоне 1, нам нужно создать виртуальную машину в зоне 1.  


```powershell
$cred = Get-Credential
New-AzVM `
   -Credential $cred `
   -ResourceGroupName $rgName `
   -Location $location `
   -Name myVM `
   -HostId $dhost.Id `
   -Image Win2016Datacenter `
   -Zone 1 `
   -Size Standard_D4s_v3
```

> [!WARNING]
> При создании виртуальной машины на узле, на котором недостаточно ресурсов, виртуальная машина будет создана в состоянии сбоя. 

## <a name="check-the-status-of-the-host"></a>Проверка состояния узла

Вы можете проверить состояние работоспособности узла и количество виртуальных машин, которые можно развернуть на узле с помощью [жетазост](/powershell/module/az.compute/get-azhost) с параметром `-InstanceView`.

```
Get-AzHost `
   -ResourceGroupName $rgName `
   -Name myHost `
   -HostGroupName $hostGroup.Name `
   -InstanceView
```

Выходные данные будут выглядеть следующим образом.

```
ResourceGroupName      : myDHResourceGroup
PlatformFaultDomain    : 1
AutoReplaceOnFailure   : True
HostId                 : 12345678-1234-1234-abcd-abc123456789
ProvisioningTime       : 7/28/2019 5:31:01 PM
ProvisioningState      : Succeeded
InstanceView           : 
  AssetId              : abc45678-abcd-1234-abcd-123456789abc
  AvailableCapacity    : 
    AllocatableVMs[0]  : 
      VmSize           : Standard_D2s_v3
      Count            : 32
    AllocatableVMs[1]  : 
      VmSize           : Standard_D4s_v3
      Count            : 16
    AllocatableVMs[2]  : 
      VmSize           : Standard_D8s_v3
      Count            : 8
    AllocatableVMs[3]  : 
      VmSize           : Standard_D16s_v3
      Count            : 4
    AllocatableVMs[4]  : 
      VmSize           : Standard_D32-8s_v3
      Count            : 2
    AllocatableVMs[5]  : 
      VmSize           : Standard_D32-16s_v3
      Count            : 2
    AllocatableVMs[6]  : 
      VmSize           : Standard_D32s_v3
      Count            : 2
    AllocatableVMs[7]  : 
      VmSize           : Standard_D64-16s_v3
      Count            : 1
    AllocatableVMs[8]  : 
      VmSize           : Standard_D64-32s_v3
      Count            : 1
    AllocatableVMs[9]  : 
      VmSize           : Standard_D64s_v3
      Count            : 1
  Statuses[0]          : 
    Code               : ProvisioningState/succeeded
    Level              : Info
    DisplayStatus      : Provisioning succeeded
    Time               : 7/28/2019 5:31:01 PM
  Statuses[1]          : 
    Code               : HealthState/available
    Level              : Info
    DisplayStatus      : Host available
Sku                    : 
  Name                 : DSv3-Type1
Id                     : /subscriptions/10101010-1010-1010-1010-101010101010/re
sourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts
/myHost
Name                   : myHost
Location               : eastus
Tags                   : {}
```

## <a name="clean-up"></a>Очистка

Вы платите за выделенные узлы, даже если виртуальные машины не развернуты. Следует удалить все узлы, которые сейчас не используются для экономии затрат.  

Узел можно удалить только в том случае, если он не использует больше виртуальных машин. Удалите виртуальные машины с помощью [Remove-AzVM](/powershell/module/az.compute/remove-azvm).

```powershell
Remove-AzVM -ResourceGroupName $rgName -Name myVM
```

После удаления виртуальных машин можно удалить узел с помощью команды [Remove-азост](/powershell/module/az.compute/remove-azhost).

```powershell
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

После удаления всех узлов группу узлов можно удалить с помощью команды [Remove-азостграуп](/powershell/module/az.compute/remove-azhostgroup). 

```powershell
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Вы также можете удалить всю группу ресурсов в одной команде, используя [Remove-азресаурцеграуп](/powershell/module/az.resources/remove-azresourcegroup). Это приведет к удалению всех ресурсов, созданных в группе, включая все виртуальные машины, узлы и группы узлов.
 
```azurepowershell-interactive
Remove-AzResourceGroup -Name $rgName
```


## <a name="next-steps"></a>Следующие шаги

- [Здесь](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)приведен пример шаблона, который использует зоны и домены сбоя для максимальной устойчивости в регионе.

- Кроме того, выделенные узлы можно развернуть с помощью [портал Azure](dedicated-hosts-portal.md).
