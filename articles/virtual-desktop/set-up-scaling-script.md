---
title: В сеансе масштабирования размещена служба автоматизации Azure — Azure
description: Автоматическое масштабирование узлов сеансов виртуальных рабочих столов Windows с помощью службы автоматизации Azure.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: helohr
ms.openlocfilehash: f38fc45411c89351eb9a50a48f22d22905ee34e6
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367251"
---
# <a name="scale-session-hosts-using-azure-automation"></a>Масштабирование узлов сеансов с помощью службы автоматизации Azure

Вы можете сократить общую стоимость развертывания виртуальных рабочих столов Windows путем масштабирования виртуальных машин. Это означает завершение работы и освобождение виртуальных машин узла сеансов в часы непикового использования, их включение и повторное выделение в часы пиковой нагрузки.

В этой статье вы узнаете о средстве масштабирования, созданном с помощью службы автоматизации Azure, и Azure Logic Apps, который будет автоматически масштабировать виртуальные машины узла сеансов в среде виртуальных рабочих столов Windows. Чтобы узнать, как использовать средство масштабирования, перейдите к [предварительным требованиям](#prerequisites).

## <a name="how-the-scaling-tool-works"></a>Принцип работы средства масштабирования

Средство масштабирования предоставляет возможность неэкономичной автоматизации для клиентов, желающих оптимизировать затраты на виртуальные машины узла сеансов.

Средство масштабирования можно использовать для:
 
- Планирование запуска и остановки виртуальных машин на основе пиковых и непиковых рабочих часов.
- Масштабировать виртуальные машины на основе числа сеансов на ядро ЦП.
- Масштабирование в виртуальных машинах в часы наименьшей нагрузки, при этом минимальное количество виртуальных машин узла сеансов не выполняется.

Инструмент масштабирования использует сочетание модулей Runbook PowerShell для Azure Automation, веб-перехватчиков и Azure Logic Apps для работы. При запуске средства Azure Logic Apps вызывает веб-перехватчик для запуска модуля Runbook службы автоматизации Azure. Затем модуль Runbook создает задание.

Во время пикового использования задание проверяет текущее количество сеансов и емкость виртуальной машины текущего работающего узла сеансов для каждого пула узлов. Эта информация используется для вычисления того, могут ли виртуальные машины с работающими узлами сеансов поддерживать существующие сеансы на основе параметра *сессионсрешолдперкпу* , определенного для файла **креатеазурелогикапп. ps1** . Если виртуальные машины узла сеансов не поддерживают существующие сеансы, задание запускает дополнительные виртуальные машины узла сеансов в пуле узлов.

>[!NOTE]
>*Сессионсрешолдперкпу* не ограничивает количество сеансов на виртуальной машине. Этот параметр определяет, нужно ли запускать новые виртуальные машины для балансировки нагрузки подключений. Чтобы ограничить количество сеансов, необходимо выполнить инструкции [Set-рдшостпул](/powershell/module/windowsvirtualdesktop/set-rdshostpool/) , чтобы соответствующим образом настроить параметр *макссессионлимит* .

Во время непикового использования задание определяет, какие виртуальные машины узла сеансов должны завершить работу в соответствии с параметром *минимумнумберофрдш* . Задание задаст виртуальным машинам узла сеанса режим стока, чтобы предотвратить подключение новых сеансов к узлам. Если для параметра *лимитсекондстофорцелогоффусер* задано ненулевое положительное значение, сценарий будет уведомлять всех пользователей, выполнивших вход в систему, сохранить свою работу, подождать настроенное время, а затем вынудить пользователей выйти из нее. После выхода из системы всех пользовательских сеансов на виртуальной машине узла сеансов этот сценарий завершит работу виртуальной машины.

Если для параметра *лимитсекондстофорцелогоффусер* задано значение 0, задание разрешит параметр конфигурации сеанса в указанных групповых политиках для управления сеансами входа пользователя. Чтобы просмотреть эти групповые политики, перейдите к разделу **Конфигурация компьютера** > **политики** > **Административные шаблоны** > **компонентов Windows** > **служб терминалов** > **ограничения времени сеанса** > **сервера терминалов** . Если на виртуальной машине узла сеансов есть активные сеансы, это задание оставляет виртуальную машину узла сеансов работающей. Если активных сеансов нет, задание завершит работу виртуальной машины узла сеансов.

Задание выполняется периодически в зависимости от заданного интервала повторения. Этот интервал можно изменить в зависимости от размера среды виртуальных рабочих столов Windows, но помните, что запуск и завершение работы виртуальных машин может занять некоторое время, поэтому не забывайте учитывать задержку. Рекомендуется устанавливать интервал повторения каждые 15 минут.

Однако средство также имеет следующие ограничения.

- Это решение применимо только к виртуальным машинам с узлами сеансов в составе пула.
- Это решение управляет виртуальными машинами в любом регионе, но может использоваться только в той же подписке, что и учетная запись службы автоматизации Azure, и Azure Logic Apps.

>[!NOTE]
>Средство масштабирования управляет режимом балансировки нагрузки для пула узлов, который он масштабирует. Она устанавливает балансировку нагрузки в ширину и в часы пиковой загрузки.

## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступить к настройке средства масштабирования, убедитесь, что у вас есть следующие компоненты:

- [Клиент виртуального рабочего стола Windows и пул узлов](create-host-pools-arm-template.md)
- Виртуальные машины пула узлов сеансов, настроенные и зарегистрированные в службе виртуальных рабочих столов Windows
- Пользователь с [доступом участника](../role-based-access-control/role-assignments-portal.md) в подписке Azure

Компьютер, используемый для развертывания средства, должен иметь: 

- Windows PowerShell 5,1 или более поздней версии
- Модуль Microsoft AZ PowerShell

Если у вас все готово, давайте приступим к работе.

## <a name="create-an-azure-automation-account"></a>Создание учетной записи службы автоматизации Azure

Сначала вам потребуется учетная запись службы автоматизации Azure для запуска модуля Runbook PowerShell. Вот как можно настроить учетную запись:

1. Откройте Windows PowerShell от имени администратора.
2. Выполните следующий командлет, чтобы войти в учетную запись Azure.

     ```powershell
     Login-AzAccount
     ```

     >[!NOTE]
     >Ваша учетная запись должна иметь права участника в подписке Azure, в которой требуется развернуть Средство масштабирования.

3. Выполните следующий командлет, чтобы скачать скрипт для создания учетной записи службы автоматизации Azure:

     ```powershell
     Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazureautomationaccount.ps1" -OutFile "your local machine path\ createazureautomationaccount.ps1"
     ```

4. Выполните следующий командлет, чтобы выполнить скрипт и создать учетную запись службы автоматизации Azure:

     ```powershell
     .\createazureautomationaccount.ps1 -SubscriptionID <azuresubscriptionid> -ResourceGroupName <resourcegroupname> -AutomationAccountName <name of automation account> -Location "Azure region for deployment"
     ```

5. Выходные данные командлета будут содержать URI веб-перехватчика. Обязательно Составьте запись универсального кода ресурса (URI), так как она будет использоваться в качестве параметра при настройке расписания выполнения для Azure Logic Apps.

После настройки учетной записи службы автоматизации Azure Войдите в свою подписку Azure и убедитесь, что ваша учетная запись службы автоматизации Azure и соответствующий Runbook появлялись в указанной группе ресурсов, как показано на следующем рисунке:

![Изображение страницы обзора Azure с созданной учетной записью службы автоматизации и модулем Runbook.](media/automation-account.png)

Чтобы проверить, где должен находиться веб-перехватчик, перейдите в список ресурсов в левой части экрана и выберите **веб-перехватчик**.

## <a name="create-an-azure-automation-run-as-account"></a>Создание учетной записи запуска от имени службы автоматизации Azure

Теперь, когда у вас есть учетная запись службы автоматизации Azure, вам также потребуется создать учетную запись запуска от имени службы автоматизации Azure для доступа к ресурсам Azure.

[Учетная запись запуска от имени службы автоматизации Azure](../automation/manage-runas-account.md) обеспечивает проверку подлинности для управления ресурсами в Azure с помощью командлетов Azure. При создании учетной записи запуска от имени он создает нового пользователя субъекта-службы в Azure Active Directory и назначает роль участника пользователю субъекта-службы на уровне подписки. учетная запись запуска от имени Azure — это отличный способ безопасной проверки подлинности с помощью сертификаты и имя субъекта-службы без необходимости хранить имя пользователя и пароль в объекте учетных данных. Дополнительные сведения о проверке подлинности "Запуск от имени" см. в разделе [ограничение разрешений учетной записи запуска](../automation/manage-runas-account.md#limiting-run-as-account-permissions).

Любой пользователь, который является членом роли администраторов Подписки и соадминистратором подписки, может создать учетную запись запуска от имени, следуя инструкциям в следующем разделе.

Чтобы создать учетную запись запуска от имени в учетной записи Azure, сделайте следующее:

1. На портале Azure щелкните **Все службы**. В списке ресурсов введите и выберите **учетные записи службы автоматизации**.

2. На странице **учетные записи службы автоматизации** выберите имя учетной записи службы автоматизации.

3. На панели в левой части окна выберите **учетные записи запуска от имени** в разделе Параметры учетной записи.

4. Выберите **учетную запись запуска от имени Azure**. Когда появится панель **Добавление учетной записи запуска от имени Azure** , просмотрите Общие сведения и нажмите кнопку **создать** , чтобы начать процесс создания учетной записи.

5. Подождите несколько минут, пока Azure создаст учетную запись запуска от имени. Ход создания можно отслеживать в меню в разделе уведомления.

6. По завершении процесса он создаст ресурс с именем AzureRunAsConnection в указанной учетной записи службы автоматизации. Ресурс подключения содержит идентификатор приложения, идентификатор клиента, идентификатор подписки и отпечаток сертификата. Запомните идентификатор приложения, так как он будет использоваться позже.

### <a name="create-a-role-assignment-in-windows-virtual-desktop"></a>создание назначения роли в Виртуальном рабочем столе Windows;

Далее необходимо создать назначение ролей, чтобы AzureRunAsConnection мог взаимодействовать с виртуальным рабочим столом Windows. Не забудьте использовать PowerShell для входа с учетной записью, имеющей разрешения на создание назначений ролей.

Сначала скачайте и импортируйте [модуль PowerShell для виртуальных рабочих столов Windows](/powershell/windows-virtual-desktop/overview/) , который будет использоваться в сеансе PowerShell, если вы этого еще не сделали. Выполните следующие командлеты PowerShell, чтобы подключиться к Виртуальному рабочему столу Windows и отобразить клиенты.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"

Get-RdsTenant
```

При обнаружении клиента с пулами узлов, которые необходимо масштабировать, следуйте инструкциям в статье [Создание учетной записи службы автоматизации Azure](#create-an-azure-automation-account) и использование имени клиента, полученного из предыдущего командлета, в следующем командлете для создания назначения роли:

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Contributor" -ApplicationId <applicationid> -TenantName <tenantname>
```

## <a name="create-the-azure-logic-app-and-execution-schedule"></a>Создание приложения логики Azure и расписания выполнения

Наконец, вам потребуется создать приложение логики Azure и настроить расписание выполнения для нового инструмента масштабирования.

1.  Откройте Windows PowerShell с правами администратора.

2.  Выполните следующий командлет, чтобы войти в учетную запись Azure.

     ```powershell
     Login-AzAccount
     ```

3. Выполните следующий командлет, чтобы скачать файл сценария креатеазурелогикапп. ps1 на локальном компьютере.

     ```powershell
     Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazurelogicapp.ps1" -OutFile "your local machine path\ createazurelogicapp.ps1"
     ```

4. Выполните следующий командлет, чтобы войти в виртуальный рабочий стол Windows, используя учетную запись с разрешениями "владелец RDS" или "участник RDS".

     ```powershell
     Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
     ```

5. Выполните следующий сценарий PowerShell, чтобы создать приложение логики Azure и расписание выполнения.

     ```powershell
     $resourceGroupName = Read-Host -Prompt "Enter the name of the resource group for the new Azure Logic App"
     
     $aadTenantId = Read-Host -Prompt "Enter your Azure AD tenant ID"

     $subscriptionId = Read-Host -Prompt "Enter your Azure Subscription ID"

     $tenantName = Read-Host -Prompt "Enter the name of your WVD tenant"

     $hostPoolName = Read-Host -Prompt "Enter the name of the host pool you’d like to scale"

     $recurrenceInterval = Read-Host -Prompt "Enter how often you’d like the job to run in minutes, e.g. ‘15’"

     $beginPeakTime = Read-Host -Prompt "Enter the start time for peak hours in local time, e.g. 9:00"

     $endPeakTime = Read-Host -Prompt "Enter the end time for peak hours in local time, e.g. 18:00"

     $timeDifference = Read-Host -Prompt "Enter the time difference between local time and UTC in hours, e.g. +5:30"

     $sessionThresholdPerCPU = Read-Host -Prompt "Enter the maximum number of sessions per CPU that will be used as a threshold to determine when new session host VMs need to be started during peak hours"

     $minimumNumberOfRdsh = Read-Host -Prompt "Enter the minimum number of session host VMs to keep running during off-peak hours"

     $limitSecondsToForceLogOffUser = Read-Host -Prompt "Enter the number of seconds to wait before automatically signing out users. If set to 0, users will be signed out immediately"

     $logOffMessageTitle = Read-Host -Prompt "Enter the title of the message sent to the user before they are forced to sign out"

     $logOffMessageBody = Read-Host -Prompt "Enter the body of the message sent to the user before they are forced to sign out"

     $location = Read-Host -Prompt "Enter the name of the Azure region where you will be creating the logic app"

     $connectionAssetName = Read-Host -Prompt "Enter the name of the Azure RunAs connection asset"

     $webHookURI = Read-Host -Prompt "Enter the URI of the WebHook returned by when you created the Azure Automation Account"

     $automationAccountName = Read-Host -Prompt "Enter the name of the Azure Automation Account"

     $maintenanceTagName = Read-Host -Prompt "Enter the name of the Tag associated with VMs you don’t want to be managed by this scaling tool"

     .\createazurelogicapp.ps1 -ResourceGroupName $resourceGroupName `
       -AADTenantID $aadTenantId `
       -SubscriptionID $subscriptionId `
       -TenantName $tenantName `
       -HostPoolName $hostPoolName `
       -RecurrenceInterval $recurrenceInterval `
       -BeginPeakTime $beginPeakTime `
       -EndPeakTime $endPeakTime `
       -TimeDifference $timeDifference `
       -SessionThresholdPerCPU $sessionThresholdPerCPU `
       -MinimumNumberOfRDSH $minimumNumberOfRdsh `
       -LimitSecondsToForceLogOffUser $limitSecondsToForceLogOffUser `
       -LogOffMessageTitle $logOffMessageTitle `
       -LogOffMessageBody $logOffMessageBody `
       -Location $location `
       -ConnectionAssetName $connectionAssetName `
       -WebHookURI $webHookURI `
       -AutomationAccountName $automationAccountName `
       -MaintenanceTagName $maintenanceTagName
     ```

     После выполнения скрипта приложение логики должно отобразиться в группе ресурсов, как показано на следующем рисунке.

     ![Изображение страницы обзора для примера приложения логики Azure.](media/logic-app.png)

Чтобы внести изменения в расписание выполнения, например изменить интервал повторения или часовой пояс, перейдите к планировщику автомасштабирования и выберите **изменить** , чтобы перейти к конструктору Logic Apps.

![Изображение конструктора Logic Apps. Меню повторение и веб-перехватчик позволяют пользователю изменять время повторения и файл веб-перехватчика.](media/logic-apps-designer.png)

## <a name="manage-your-scaling-tool"></a>Управление средством масштабирования

Теперь, когда вы создали средство масштабирования, вы можете получить доступ к его выходным данным. В этом разделе описаны некоторые функции, которые могут оказаться полезными.

### <a name="view-job-status"></a>Просмотр состояния задания

Вы можете просмотреть сводное состояние всех заданий Runbook или просмотреть более подробное состояние конкретного задания Runbook в портал Azure.

Справа от выбранной учетной записи службы автоматизации в разделе "Статистика заданий" можно просмотреть список сводок всех заданий Runbook. При открытии страницы **задания** в левой части окна отображаются текущие состояния заданий, время начала и время завершения.

![Снимок экрана со страницей "состояние задания".](media/jobs-status.png)

### <a name="view-logs-and-scaling-tool-output"></a>Просмотр журналов и масштабирование выходных данных средства

Чтобы просмотреть журналы операций масштабирования и масштабирования, откройте модуль Runbook и выберите имя задания.

Перейдите к модулю Runbook (имя по умолчанию — Ввдаутоскалерунбук) в группе ресурсов, в которой размещена учетная запись службы автоматизации Azure, и выберите **Обзор**. На странице Обзор выберите задание в разделе недавние задания, чтобы просмотреть его выходные данные средства масштабирования, как показано на следующем рисунке.

![Изображение окна вывода для инструмента масштабирования.](media/tool-output.png)
