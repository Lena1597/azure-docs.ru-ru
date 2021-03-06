---
title: PowerShell. Добавление внешнего пользователя в лабораторию в Azure DevTest Labs
description: В этой статье представлен сценарий Azure PowerShell, который добавляет внешнего пользователя в лабораторию в Azure DevTest Labs.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: e9efe5476da051d905feff3d6803cb60ea0c4beb
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760493"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>Добавление внешнего пользователя в лабораторию в Azure DevTest Labs с помощью PowerShell

Этот пример сценария PowerShell добавляет внешнего пользователя в лабораторию в Azure DevTest Labs. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Технические условия
* **Лаборатория.** Этот сценарий требует наличия лаборатории. 

## <a name="sample-script"></a>Пример скрипта

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Описание скрипта

Этот сценарий использует следующие команды: 

| Get-Help | Примечания |
|---|---|
| [Get-Азадусер](/powershell/module/az.resources/get-azaduser) | Извлекает объект-пользователь из Azure Active Directory. |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | Присваивает указанную роль заданному субъекту в определенной области. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](https://docs.microsoft.com/powershell/).

Дополнительные примеры сценариев PowerShell для службы "Службы лабораторий Azure" приведены [здесь](../samples-powershell.md).
