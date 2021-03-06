---
title: Поддержка аварийного восстановления виртуальных машин Hyper-V в Azure с помощью Azure Site Recovery
description: Краткое описание поддерживаемых компонентов и требований для аварийного восстановления виртуальных машин Hyper-V в Azure с помощью службы Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 1/27/2020
ms.author: raynew
ms.openlocfilehash: d4409fe61bfe1f0a9fe74171f5b1ec471b9a6a26
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2020
ms.locfileid: "76774427"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Таблица поддержки аварийного восстановления локальных виртуальных машин Hyper-V в Azure


В этой статье перечислены компоненты и параметры, поддерживаемые для аварийного восстановления локальных виртуальных машин Hyper-V в Azure с помощью [Azure Site Recovery](site-recovery-overview.md).



## <a name="supported-scenarios"></a>Поддерживаемые сценарии

**Сценарий** | **Сведения**
--- | ---
Hyper-V с Virtual Machine Manager <br> <br>| Вы можете выполнять аварийное восстановление в Azure для виртуальных машин, работающих на узлах Hyper-V под управлением структуры System Center Virtual Machine Manager.<br/><br/> Этот сценарий можно развернуть с помощью портала Azure или PowerShell.<br/><br/> Если узлами Hyper-V управляет Virtual Machine Manager, вы можете также выполнять аварийное восстановление на вторичный локальный сайт. Дополнительные сведения об этом сценарии см. в [этом руководстве](hyper-v-vmm-disaster-recovery.md).
Hyper-V без Virtual Machine Manager | Вы можете выполнять аварийное восстановление в Azure для виртуальных машин, работающих на узлах Hyper-V без управления Virtual Machine Manager.<br/><br/> Этот сценарий можно развернуть с помощью портала Azure или PowerShell.

## <a name="on-premises-servers"></a>Локальные серверы

**Server** | **Требования** | **Сведения**
--- | --- | ---
Hyper-V (без Virtual Machine Manager) |  Windows Server 2019, Windows Server 2016 (включая установку Server Core), Windows Server 2012 R2 с последними обновлениями | Если вы уже настроили Windows Server 2012 R2 или SCVMM 2012 R2 с помощью Azure Site Recovery и планируете обновить операционную систему, следуйте указаниям в [документации.](upgrade-2012R2-to-2016.md) 
Hyper-V (с Virtual Machine Manager) | Virtual Machine Manager 2019, Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Если используется Virtual Machine Manager, узлы Windows Server 2019 должны управляться в Virtual Machine Manager 2019. Аналогичным образом узлы Windows Server 2016 должны управляться в Virtual Machine Manager 2016.<br/><br/> Примечание. Восстановление размещения в альтернативное расположение не поддерживается для узлов Windows Server 2019.


## <a name="replicated-vms"></a>Реплицированные виртуальные машины


В таблице ниже перечислены требования к виртуальной машине. Служба Site Recovery поддерживает все рабочие нагрузки в поддерживаемой операционной системе.

 **Компонент** | **Сведения**
--- | ---
Конфигурация виртуальной машины | Виртуальные машины, которые реплицируются в Azure, должны соответствовать [требованиям Azure](#azure-vm-requirements).
Операционная система на виртуальной машине | Любая гостевая ОС, [поддерживаемая в Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases).<br/><br/> Nano Server Windows Server 2016 не поддерживается.


## <a name="vmdisk-management"></a>Управление виртуальной машиной и диском

**Действие** | **Сведения**
--- | ---
Изменение размера диска на реплицируемой виртуальной машине Hyper-V | Не поддерживается. Отключите репликацию, внесите изменения, а затем снова включите репликацию для виртуальной машины.
Добавление диска к реплицируемой виртуальной машине Hyper-V | Не поддерживается. Отключите репликацию, внесите изменения, а затем снова включите репликацию для виртуальной машины.

## <a name="hyper-v-network-configuration"></a>Конфигурация сети Hyper-V

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Сеть узла: объединение сетевых карт | Да | Да
Сеть узла: виртуальная локальная сеть | Да | Да
Сеть узла: IPv4 | Да | Да
Сеть узла: IPv6 | Нет | Нет
Сеть гостевой виртуальной машины: объединение сетевых карт | Нет | Нет
Сеть гостевой виртуальной машины: IPv4 | Да | Да
Сеть гостевой виртуальной машины: IPv6 | Нет | Да
Сеть гостевой виртуальной машины: статический IP-адрес (Windows) | Да | Да
Сеть гостевой виртуальной машины: статический IP-адрес (Linux) | Нет | Нет
Сеть гостевой виртуальной машины: несколько сетевых карт | Да | Да



## <a name="azure-vm-network-configuration-after-failover"></a>Конфигурация сети виртуальных машин Azure (после отработки отказа)

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | Да | Да
Внутренний балансировщик нагрузки | Да | Да
Внешний балансировщик нагрузки | Да | Да
Azure Traffic Manager | Да | Да
Несколько сетевых адаптеров | Да | Да
Резервирование IP-адресов | Да | Да
IPv4 | Да | Да
Сохранение исходного IP-адреса | Да | Да
Конечные точки служб для виртуальной сети Azure<br/> (без брандмауэров службы хранилища Azure) | Да | Да
Ускорение работы в сети | Нет | Нет


## <a name="hyper-v-host-storage"></a>Хранилище узла Hyper-V

**Память** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | --- 
NFS | Нет данных | Нет данных
SMB 3.0 | Да | Да
Сеть SAN (iSCSI) | Да | Да
Многопутевое (Multipath I/O). Протестировано с использованием:<br></br> Microsoft DSM, EMC PowerPath 5,7 SP4, EMC PowerPath DSM для CLARiiON | Да | Да

## <a name="hyper-v-vm-guest-storage"></a>Гостевое хранилище виртуальной машины Hyper-V

**Память** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
VMDK | Нет данных | Нет данных
VHD (VHDX) | Да | Да
Виртуальная машина поколения 2 | Да | Да
UEFI (EFI)<br></br>Перенесенная виртуальная машина в Azure будет автоматически преобразована в загрузочную виртуальную машину BIOS. Виртуальная машина должна работать под Windows Server 2012 и более поздних версий. Диск ОС должен содержать до пяти разделов или меньше, а размер диска ОС не должен превышать 300 ГБ.| Да | Да
Общий диск кластера | Нет | Нет
Зашифрованный диск | Нет | Нет
NFS | Нет данных | Нет данных
SMB 3.0 | Нет | Нет
RDM | Нет данных | Нет данных
Диски размером более 1 ТБ | Да, до 4095 ГБ | Да, до 4095 ГБ
Диск: логический и физический сектор размером 4 КБ | Не поддерживается: поколение 1 или 2 | Не поддерживается: поколение 1 или 2
Диск: логический и 512-байтовый физический сектор. | Да |  Да
Управление логическими томами (LVM). LVM поддерживается только на дисках данных. Azure предоставляет только один диск с ОС. | Да | Да
Том с чередующимся диском размером более 1 ТБ | Да | Да
Дисковые пространства | Нет | Нет
"Горячее" добавление или удаление диска | Нет | Нет
Исключение диска | Да | Да
Многопутевой (MPIO) | Да | Да

## <a name="azure-storage"></a>Служба хранилища Azure

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Локально избыточное хранилище | Да | Да
Гео-избыточное хранилище | Да | Да
Геоизбыточное хранилище с доступом для чтения | Да | Да
"Холодное" хранилище | Нет | Нет
"Горячее" хранилище| Нет | Нет
Блочные BLOB-объекты | Нет | Нет
Шифрование неактивных данных (SSE)| Да | Да
Шифрование неактивных (CMK) <br></br> (Только для отработки отказа на управляемые диски)| Да (с помощью PowerShell AZ 3.3.0 Module) | Да (с помощью PowerShell AZ 3.3.0 Module)
Хранилище класса Premium | Да | Да
Служба импорта и экспорта | Нет | Нет
Учетные записи хранения Azure с включенным брандмауэром | Да. Для целевого хранилища и кэша. | Да. Для целевого хранилища и кэша.
Изменение учетной записи хранения | Нет. Невозможно изменить целевую учетную запись хранения Azure после включения репликации. Чтобы изменить, отключите и снова включите аварийное восстановление. | Нет


## <a name="azure-compute-features"></a>Вычислительные компоненты Azure

**Компонент** | **Hyper-V с Virtual Machine Manager** | **Hyper-V без Virtual Machine Manager**
--- | --- | ---
Группы доступности | Да | Да
Концентратор | Да | Да  
Управляемые диски | Да, для отработки отказа.<br/><br/> Восстановление размещения для управляемых дисков не поддерживается. | Да, для отработки отказа.<br/><br/> Восстановление размещения для управляемых дисков не поддерживается.

## <a name="azure-vm-requirements"></a>Требования для виртуальных машин Azure

Локальные виртуальные машины, которые вы реплицируете в Azure, должны соответствовать требованиям Azure, приведенным в этой таблице.

**Компонент** | **Требования** | **Сведения**
--- | --- | ---
Операционная система на виртуальной машине | Служба Site Recovery поддерживает все операционные системы, [поддерживаемые Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Архитектура операционной системы на виртуальной машине | 32-разрядная версия (Windows Server 2008)/64-bit | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Размер диска операционной системы | До 2048 ГБ для виртуальных машин поколения 1.<br/><br/> До 300 ГБ для виртуальных машин поколения 2  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Число дисков операционной системы | 1 | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Число дисков с данными | 16 ГБ или меньше  | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Размер виртуального жесткого диска с данными | До 4095 ГБ | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Сетевые адаптеры | Поддерживаются несколько адаптеров |
Общий виртуальный жесткий диск | Не поддерживается | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Диск FC | Не поддерживается | Если не поддерживается, проверка на соответствие обязательным требованиям завершается ошибкой.
Формат жесткого диска | VHD <br/><br/> VHDX | Служба Site Recovery автоматически преобразует формат VHDX в VHD при отработке отказа в Azure. При восстановлении до локальной системы виртуальные машины продолжают использовать формат VHDX.
BitLocker | Не поддерживается | Прежде чем включать репликацию для виртуальной машины, необходимо отключить BitLocker.
имя виртуальной машины; | От 1 до 63 символов, при этом допустимы только буквы, цифры и дефисы Имя виртуальной машины должно начинаться и заканчиваться буквой или цифрой. | Обновите значение в свойствах виртуальной машины в службе Site Recovery.
Тип виртуальной машины | Поколение 1<br/><br/> Поколение 2 — Windows | Поддерживаются виртуальные машины второго поколения с диском ОС типа "Базовый" (включая один или два тома данных в формате VHDX) и объемом менее 300 ГБ.<br></br>Виртуальные машины Linux второго поколения не поддерживаются. [Подробнее](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Действия хранилища служб восстановления

**Действие** |  **Hyper-V с VMM** | **Hyper-V без VMM**
--- | --- | ---
Перемещение хранилища между группами ресурсов<br/><br/> В пределах подписок и между ними | Нет | Нет
Перемещение хранилищ, сетей, виртуальных машин Azure между группами ресурсов<br/><br/> В пределах подписок и между ними | Нет | Нет

> [!NOTE]
> При репликации виртуальных машин Hyper-V из локальной среды в Azure можно выполнять репликацию только в один клиент AD из одного конкретного окружения — Hyper-V или Hyper-V с помощью VMM.


## <a name="provider-and-agent"></a>Поставщик и агент

Чтобы обеспечить совместимость развертывания с параметрами, описанными в этой статье, используйте последние версии поставщика и агента.

**имя**; | **Описание** | **Сведения**
--- | --- | --- 
Поставщик Azure Site Recovery | Координирует взаимодействие между локальными серверами и Azure <br/><br/> Hyper-V (с Virtual Machine Manager): устанавливается на серверах Virtual Machine Manager<br/><br/> Hyper-V (без Virtual Machine Manager): устанавливается на узлах Hyper-V| Последняя версия: 5.1.2700.1 (доступна на портале Azure)<br/><br/> [Новейшие функции и последние исправления](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Агент служб восстановления Microsoft Azure | Координирует репликацию между виртуальными машинами Hyper-V и Azure.<br/><br/> Устанавливается на локальных серверах Hyper-V (с или без Virtual Machine Manager). | Последняя версия агента, доступная на портале






## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [подготовить Azure](tutorial-prepare-azure.md) для аварийного восстановления локальных виртуальных машин Hyper-V.
