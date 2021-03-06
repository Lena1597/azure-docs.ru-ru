---
title: Решения Azure VMware (AVS). Создание виртуальной машины в Azure с помощью шаблонов виртуальных машин
description: В этой статье описывается создание виртуальных машин в Azure с помощью шаблонов виртуальных машин в инфраструктуре VMware для частного облака AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 533aaab13f1b957e709f66b23b511fc199ee0285
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015207"
---
# <a name="create-a-virtual-machine-in-azure-using-vm-templates-on-the-vmware-infrastructure"></a>Создание виртуальной машины в Azure с помощью шаблонов виртуальных машин в инфраструктуре VMware

Вы можете создать виртуальную машину в портал Azure, используя шаблоны виртуальных машин в инфраструктуре VMware, которые администратор AVS включил для вашей подписки.

## <a name="sign-in-to-azure"></a>Войдите в Azure

Войдите на [портал Azure](https://portal.azure.com).

## <a name="create-avs-virtual-machine"></a>Создать виртуальную машину AVS

1. Выбор пункта **Все службы**.

2. Поиск **виртуальных машин AVS**.

3. Нажмите кнопку **Добавить**.

    ![Создать виртуальную машину AVS](media/create-cloudsimple-virtual-machine.png)

4. Введите Основные сведения нажмите кнопку **Далее: размер**.

    > [!NOTE]
    > Для создания виртуальной машины AVS в Azure требуется шаблон виртуальной машины. Этот шаблон виртуальной машины должен существовать в частном облаке vCenter. Создайте виртуальную машину в частном облаке из пользовательского интерфейса vCenter с требуемой операционной системой и конфигурациями. С помощью инструкций в разделе [клонирование виртуальной машины в шаблон в веб-клиенте vSphere](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE_copy.html)создайте шаблон.

    ![Создание виртуальной машины AVS — основы](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Поле | Description |
    | ------------ | ------------- |
    | Подписка | Подписка Azure, связанная с частным облаком.  |
    | Группа ресурсов | Группа ресурсов, которой будет назначена виртуальная машина. Можно выбрать имеющуюся группу или создать новую. |
    | Имя | Имя, идентифицирующее виртуальную машину.  |
    | Расположение | Регион Azure, в котором размещена эта виртуальная машина.  |
    | Частное облако | Частное облако AVS, в котором вы хотите создать виртуальную машину. |
    | Пул ресурсов | Сопоставленный пул ресурсов для виртуальной машины. Выберите из доступных пулов ресурсов. |
    | Шаблон vSphere | шаблон vSphere для виртуальной машины.  |
    | Имя пользователя | Имя пользователя администратора виртуальной машины (для шаблонов Windows)|
    | Пароль <br>Подтверждение пароля | Пароль для администратора виртуальной машины (для шаблонов Windows).  |

5. Выберите количество ядер и объем памяти для виртуальной машины и нажмите кнопку **Далее: конфигурации**. Установите флажок, если требуется предоставить полную виртуализацию ЦП операционной системе на виртуальной машине, чтобы приложения, требующие виртуализации оборудования, могли работать на виртуальных машинах без двоичного перевода или виртуализации. Дополнительные сведения см. в статье VMware [Expose VMware Hardware Assisted Virtualization](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html) (Предоставление аппаратной виртуализации VMware).

    ![Создание виртуальной машины AVS (размер)](media/create-cloudsimple-virtual-machine-size.png)

6. Настройте сетевые интерфейсы и диски, как описано в следующих таблицах, и щелкните **проверить и создать**.

    ![Создание виртуальной машины AVS — конфигурации](media/create-cloudsimple-virtual-machine-configurations.png)

    Для параметра сетевые интерфейсы щелкните **Добавить сетевой интерфейс** и настройте следующие параметры.

    | Control | Description |
    | ------------ | ------------- |
    | Имя | Введите имя для идентификации интерфейса.  |
    | Сеть | Выберите из списка настроенной распределенной группы портов в vSphere частного облака.  |
    | Адаптера | Выберите адаптер vSphere из списка доступных типов, настроенных для виртуальной машины. Дополнительные сведения см. в статье базы знаний VMware [Выбор сетевого адаптера для виртуальной машины](https://kb.vmware.com/s/article/1001805). |
    | Включение при загрузке | Выберите, следует ли включить оборудование сетевого адаптера при загрузке виртуальной машины. По умолчанию этот параметр имеет значение **Enable** (Включить). |

    Для параметра диски щелкните **Добавить диск** и настройте следующие параметры.

    | Элемент | Description |
    | ------------ | ------------- |
    | Имя | Введите имя для идентификации диска.  |
    | Размер | Выберите один из доступных размеров.  |
    | Контроллер SCSI | Выберите контроллер SCSI для диска.  |
    | Режим | Определяет, как диск участвует в моментальных снимках. Выберите один из следующих параметров. <br> -Независимый постоянный: все данные, записываемые на диск, записываются без возможности восстановления.<br> — Независимое от постоянного: изменения, записанные на диск, отбрасываются при выключении или сбросе виртуальной машины. Режим независимого временного хранения позволяет восстановить состояние виртуальной машины, просто перезапустив ее. Дополнительные сведения см. в [документации VMware](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html).

7. После завершения проверки проверьте параметры и нажмите кнопку **создать**. Чтобы внести изменения, щелкните вкладки вверху или щелкните.

    ![Создание виртуальной машины AVS — проверка](media/create-cloudsimple-virtual-machine-review.png)

## <a name="view-list-of-avs-virtual-machines"></a>Просмотр списка виртуальных машин AVS

1. Выбор пункта **Все службы**.

2. Поиск **виртуальных машин AVS**.

3. Выберите, в котором было создано частное облако.

    ![Список виртуальных машин AVS](media/list-cloudsimple-virtual-machines.png)

Список виртуальных машин AVS включает виртуальные машины, созданные на основе портал Azure.  Виртуальные машины, созданные в частном облаке vCenter в сопоставленном пуле ресурсов vCenter, будут показаны в списке.  
