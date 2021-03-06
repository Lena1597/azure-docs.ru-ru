---
title: Включение удаленного рабочего стола для Linux в службах лабораторий Azure | Документация Майкрософт
description: Узнайте, как включить удаленный рабочий стол для виртуальных машин Linux в лаборатории в службах лаборатории Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: cb9a3e2b9ddcd0f74bfa4978f0bc3f4eb0688257
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73583718"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Включение удаленного рабочего стола для виртуальных машин Linux в лаборатории в службах лаборатории Azure
В этой статье показано, как выполнить следующие задачи.

- Включение удаленного рабочего стола для виртуальной машины Linux
- Как преподаватели могут подключаться к виртуальной машине шаблона через подключение к удаленному рабочему столу (RDP).

## <a name="enable-remote-desktop-for-linux-vm"></a>Включение удаленного рабочего стола для виртуальной машины Linux
Во время создания лаборатории преподаватели могут включить **Подключение к удаленному рабочему столу** для образов **Linux** . Параметр **включить подключение к удаленному рабочему столу** отображается, если для шаблона выбран образ Linux. Если этот параметр включен, преподаватели могут подключаться к виртуальным машинам шаблона и учащихся через RDP (удаленный рабочий стол). 

![Включение подключения к удаленному рабочему столу для образа Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

В окне **включение подключение к удаленному рабочему столуного** сообщения выберите **продолжить с удаленный рабочий стол**. 

![Включение подключения к удаленному рабочему столу для образа Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> При включении **подключения к удаленному рабочему столу** открывается только порт **RDP** на компьютерах Linux. Если протокол удаленного рабочего стола уже установлен и настроен в образе виртуальной машины (например, образ виртуальной машины Ubuntu для обработки и анализа данных), вы можете подключаться к виртуальным машинам по протоколу RDP без выполнения каких-либо дополнительных действий.
> 
> Если на образе виртуальной машины не установлен и не настроен протокол RDP, необходимо подключиться к компьютеру Linux с помощью SSH в первый раз и установить пакеты RDP и GUI, чтобы вы или студенты могли подключиться к компьютеру Linux по протоколу RDP позднее. Дополнительные сведения см. в [статье Установка и настройка удаленный рабочий стол для подключения к виртуальной машине Linux в Azure](../../virtual-machines/linux/use-remote-desktop.md). Затем вы публикуете образ, чтобы студенты могли выполнять RDP на виртуальных машинах Linux учащихся. 

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы
В настоящее время подключение к удаленному рабочему столу поддерживается для следующих операционных систем:

- openSUSE Leap 42.3
- Версия 7.5 на основе CentOS
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="connect-to-the-template-vm"></a>Подключение к шаблону виртуальной машины 
Преподаватели должны подключаться к виртуальной машине шаблона сначала с помощью SSH, а также устанавливать пакеты RDP и GUI. Затем преподаватели могут использовать RDP для подключения к виртуальной машине шаблона: 

1. Если вы видите пункт **Настройка шаблона** на панели инструментов, выберите его. Затем нажмите кнопку **продолжить** в диалоговом окне **Настройка шаблона** . Это действие запускает виртуальную машину шаблона.  

    ![Настройка шаблона](../media/how-to-enable-remote-desktop-linux/customize-template.png)
2. После запуска шаблона виртуальной машины можно выбрать пункт **подключить шаблон** , а затем **подключиться через SSH** на панели инструментов. 

    ![Подключение к шаблону через RDP после создания лаборатории](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 
3. Появится следующее диалоговое окно **Подключение к виртуальной машине** . Нажмите кнопку **копирования** рядом с текстовым полем, чтобы скопировать его в буфер обмена. Сохраните строку подключения SSH. Используйте эту строку подключения из терминала SSH (например [PuTTY](https://www.putty.org/)) для подключения к виртуальной машине.
 
    ![Строка подключения SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)
4. Установите пакеты RDP и GUI, чтобы вы или студенты могли подключиться к компьютеру Linux по протоколу RDP позднее. Дополнительные сведения см. в [статье Установка и настройка удаленный рабочий стол для подключения к виртуальной машине Linux в Azure](../../virtual-machines/linux/use-remote-desktop.md). Затем вы публикуете образ, чтобы студенты могли выполнять RDP на виртуальных машинах Linux учащихся.
5. После установки этих пакетов можно использовать **шаблон подключиться к** панели инструментов, а затем выбрать **подключиться через RDP** для подключения к шаблону виртуальной машины через RDP. Сохраните RDP-файл и используйте его для подключения к виртуальной машине шаблона через RDP. 

## <a name="next-steps"></a>Дальнейшие действия
После того, как инструктор включил функцию подключения к удаленному рабочему столу, студенты могут подключаться к своим виртуальным машинам через RDP или SSH. Дополнительные сведения см. [в статье Использование удаленного рабочего стола для виртуальных машин Linux в учебной лаборатории](how-to-use-remote-desktop-linux-student.md). 