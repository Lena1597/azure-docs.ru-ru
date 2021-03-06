---
title: Учебник по резервному копированию базы данных SAP HANA в Azure с помощью интерфейса командной строки
description: Из этого учебника вы узнаете, как выполнять резервное копирование баз данных SAP HANA, запущенных на виртуальной машине Azure, в хранилище Служб восстановления для Azure Backup через интерфейс командной строки Azure.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 6d35b6ebfc6f47f5775c24407b645b97112387c9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75470809"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Руководство. Резервное копирование баз данных SAP HANA на виртуальных машинах Azure с помощью Azure CLI

Azure CLI используется для создания ресурсов Azure и управления ими из командной строки или с помощью скриптов. В этой документации содержатся сведения о резервном копировании базы данных SAP HANA и активации резервного копирования по запросу с помощью Azure CLI. Эти действия можно также выполнить с помощью [портала Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

В этом документе предполагается, что у вас уже есть виртуальная машина Azure с установленной базой данных SAP HANA. (Вы также можете [создать виртуальную машину с помощью Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli)). Изучив это руководство, вы сможете выполнить следующие действия:

> [!div class="checklist"]
>
> * Создание хранилища служб восстановления
> * Регистрация экземпляра SAP HANA и обнаружение на нем баз данных.
> * Включение резервного копирования для базы данных SAP HANA.
> * Активирование резервного копирования по запросу

Изучите [поддерживаемые на текущий момент сценарии](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support) работы с SAP HANA.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Чтобы установить и использовать интерфейс командной строки локально, необходимо запустить Azure CLI версии xx.xxx.x или более поздней. Чтобы получить необходимую версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-recovery-services-vault"></a>Создание хранилища служб восстановления

Хранилище Служб восстановления — это логический контейнер, в котором хранятся данные резервного копирования для каждого защищенного ресурса, например виртуальных машин Azure или выполняемых на виртуальных машинах рабочих нагрузок, таких как базы данных SQL или HANA. Когда выполняется задание резервного копирования для защищенного ресурса, в хранилище служб восстановления создается точка восстановления. Позже вы сможете использовать одну из этих точек восстановления, чтобы восстановить данные до определенной точки во времени.

Создайте хранилище служб восстановления с помощью команды [az backup vault create](https://docs.microsoft.com/cli/azure/backup/vault#az-backup-vault-create). Укажите те же группу ресурсов и расположение, что и для виртуальной машины, которую необходимо защитить. Узнайте, как создать виртуальную машину с помощью Azure CLI, изучив [краткое руководство по виртуальным машинам](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

В этом учебнике мы будем использовать следующие ресурсы:

* группа ресурсов с именем *saphanaResourceGroup*;
* виртуальная машина с именем *saphanaVM*;
* ресурсы в расположении *westus2*.

Мы создадим хранилище с именем *saphanaVault*.

```azurecli-interactive
az backup vault create --resource-group saphanaResourceGroup \
    --name saphanaVault \
    --location westus2
```

По умолчанию в качестве хранилища служб восстановления задано геоизбыточное хранилище. Геоизбыточное хранилище гарантирует, что данные резервного копирования реплицируются во вторичный регион, который находится в сотнях километров от первичного региона. Если вам необходимо изменить параметр избыточности хранилища, используйте командлет [az backup vault backup-properties set](https://docs.microsoft.com/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set).

```azurecli
az backup vault backup-properties set \
    --name saphanaVault  \
    --resource-group saphanaResourceGroup \
    --backup-storage-redundancy "LocallyRedundant/GeoRedundant"
```

Чтобы убедиться, что хранилище успешно создано, используйте командлет [az backup vault list](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-list). Вы увидите следующий ответ:

```output
Location   Name             ResourceGroup
---------  ---------------  -------------  
westus2    saphanaVault     saphanaResourceGroup
```

## <a name="register-and-protect-the-sap-hana-instance"></a>Регистрация и защита экземпляра SAP HANA

Чтобы службы Azure смогли обнаружить экземпляр SAP HANA (виртуальную машину с установленной на ней SAP HANA), необходимо запустить на компьютере с SAP HANA [скрипт предварительной регистрации](https://aka.ms/scriptforpermsonhana). Перед запуском этого скрипта убедитесь, что выполнены все [предварительные требования](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#prerequisites). Дополнительные сведения о задачах этого скрипта см. в разделе [о настройке разрешений](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#setting-up-permissions).

После выполнения скрипта вы сможете зарегистрировать экземпляр SAP HANA в хранилище Служб восстановления, которое мы создали ранее. Чтобы зарегистрировать экземпляр, используйте командлет [az backup container register](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-register). *VMResourceId* обозначает идентификатор ресурса для виртуальной машины, которую вы создали для установки SAP HANA.

```azurecli-interactive
az backup container register --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --location westus2 \
    --workload-type SAPHANA \
    --backup-management-type AzureWorkload \
    --resource-id VMResourceId
```

>[!NOTE]
>Если эта виртуальная машина размещена не в той же группе ресурсов, что и хранилище, укажите *saphanaResourceGroup* в качестве группы ресурсов, в которой создано хранилище.

При регистрации экземпляра SAP HANA на нем автоматически обнаруживаются все существующие базы данных. Однако для обнаружения новых баз данных, которые могут быть добавлены в будущем, выполните действия из раздела [об обнаружении новых баз данных, добавленных в зарегистрированный экземпляр SAP HANA](tutorial-sap-hana-manage-cli.md#protect-new-databases-added-to-an-sap-hana-instance).

Чтобы убедиться, что экземпляр SAP HANA успешно зарегистрирован в хранилище, используйте командлет [az backup container list](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list). Вы увидите следующий ответ:

```output
Name                                                    Friendly Name    Resource Group        Type           Registration Status
------------------------------------------------------  --------------   --------------------  ---------      ----------------------
VMAppContainer;Compute;saphanaResourceGroup;saphanaVM   saphanaVM        saphanaResourceGroup  AzureWorkload  Registered
```

>[!NOTE]
> Столбец name в представленном выше примере выходных данных ссылается на имя контейнера. Это имя контейнера будет использоваться в следующих разделах для настройки и активации резервного копирования. В нашем примере имя имеет значение *VMAppContainer;Compute;saphanaResourceGroup;saphanaVM*.

## <a name="enable-backup-on-sap-hana-database"></a>Включение резервного копирования для базы данных SAP HANA

Командлет [az backup protectable-item list](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-list) возвращает список всех баз данных, обнаруженных в экземпляре SAP HANA, который вы зарегистрировали на предыдущем шаге.

```azurecli-interactive
az backup protectable-item list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --workload-type SAPHANA \
    --output table
```

В списке должна появиться новая база данных, для которой вы решили включить резервное копирование, и теперь он будет выглядеть следующим образом:

```output
Name                           Protectable Item Type    ParentName    ServerName    IsProtected
-----------------------------  ----------------------   ------------  -----------   ------------
saphanasystem;hxe              SAPHanaSystem            HXE           hxehost       NotProtected  
saphanadatabase;hxe;systemdb   SAPHanaDatabase          HXE           hxehost       NotProtected
saphanadatabase;hxe;hxe        SAPHanaDatabase          HXE           hxehost       NotProtected
```

Как видно из приведенных выше выходных данных, идентификатор безопасности SAP HANA имеет значение HXE. В нашем примере мы настроим резервное копирование для базы данных *saphanadatabase;hxe;hxe*, которая находится на сервере *hxehost*.

Чтобы защитить и настроить резервное копирование поочередно для каждой базы данных, мы применим командлет [az backup protection enable-for-azurewl](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-enable-for-azurewl). Укажите имя политики, которую вы намерены использовать. Чтобы создать политику через интерфейс командной строки, используйте командлет [az backup policy create](https://docs.microsoft.com//cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create). В этом учебнике мы будем использовать политику *sapahanaPolicy*.

```azurecli-interactive
az backup protection enable-for-azurewl --resource-group saphanaResourceGroup \
    --policy-name saphanaPolicy \
    --protectable-item-name saphanadatabase;hxe;hxe  \
    --protectable-item-type SAPHANADatabse \
    --server-name hxehost \
    --workload-type SAPHANA \
    --output table
```

Вы можете проверить, завершена ли настройка резервного копирования, с помощью командлета [az backup job list](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list). Этот командлет возвращает следующий результат:

```output
Name                                  Operation         Status     Item Name   Start Time UTC
------------------------------------  ---------------   ---------  ----------  -------------------  
e0f15dae-7cac-4475-a833-f52c50e5b6c3  ConfigureBackup   Completed  hxe         2019-12-03T03:09:210831+00:00  
```

Командлет [az backup job list](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) возвращает список всех заданий резервного копирования (запланированных и по требованию), которые были выполнены или выполняются в текущий момент в защищенной базе данных, а также другие операции, такие как регистрация, настройка резервного копирования, удаление данных резервных копий и т. д.

## <a name="trigger-an-on-demand-backup"></a>Активирование резервного копирования по запросу

В разделе выше описано, как настроить резервное копирование по расписанию, а в этом разделе мы сосредоточимся на активации резервного копирования по запросу. Для этого мы применим командлет [az backup protection backup-now](https://docs.microsoft.com/cli/azure/backup/protection#az-backup-protection-backup-now).

>[!NOTE]
> Политика хранения для создаваемой по запросу резервной копии определяется базовой политикой хранения для базы данных.

```azurecli-interactive
az backup protection backup-now --resource-group saphanaResourceGroup \
    --item-name saphanadatabase;hxe;hxe \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --backup-type Full
    --retain-until 01-01-2040
    --output table
```

Этот командлет возвращает следующий результат:

```output
Name                                  ResourceGroup
------------------------------------  -------------
e0f15dae-7cac-4475-a833-f52c50e5b6c3  saphanaResourceGroup
```

В ответе вы получите имя задания. Это имя можно использовать для отслеживания состояния задания с помощью командлета [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show).

>[!NOTE]
>Вы можете не только запланировать создание полной или разностной резервной копии, но и активировать его вручную. Резервные копии журналов активируются и управляются SAP HANA самостоятельно и автоматически.
>
> Azure Backup в настоящее время не поддерживает добавочные резервные копии.

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы узнать, как восстановить базу данных SAP HANA на виртуальной машине Azure с помощью CLI, перейдите к [этому руководству](tutorial-sap-hana-restore-cli.md).

* Сведения о резервном копировании базы данных SAP HANA, работающей на виртуальной машине Azure, с помощью портала Azure см. в [этой статье](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).
