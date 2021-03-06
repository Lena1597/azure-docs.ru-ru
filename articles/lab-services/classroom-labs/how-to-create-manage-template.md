---
title: Управление шаблоном лаборатории для аудитории в Службах лабораторий Azure | Документация Майкрософт
description: Узнайте, как создать и использовать шаблон лаборатории для аудитории в Службах лабораторий Azure.
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
ms.openlocfilehash: 08fbe9565356dc1b7db952fdd265770fef600ca8
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989048"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Сведения о создании и использовании шаблона лаборатории для аудитории в Службах лабораторий Azure.
На основе шаблона, в роли которого выступает базовый образ виртуальной машины, в лаборатории создаются все виртуальные машины пользователей. Настройте шаблон согласно параметрам, которые вы хотите предоставить пользователям лаборатории. Вы можете указать имя и описание шаблона, которое будет отображаться для пользователей лаборатории. После этого опубликуйте шаблон, чтобы сделать экземпляры шаблона виртуальной машины доступными для пользователей лаборатории. Когда вы опубликуете шаблон, Службы лабораторий Azure создадут виртуальные машины в лаборатории с помощью этого шаблона. Число созданных при этом процессе виртуальных машин соответствует максимальному числу разрешенных в лаборатории пользователей, которое можно настроить в политике использования лаборатории. Все виртуальные машины имеют ту же конфигурацию, что и шаблон.

В этой статье описывается, как создать и контролировать виртуальную машину в качестве шаблона лаборатории для аудитории в Службах лабораторий Azure. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Публикация шаблона при создании лаборатории для аудитории
Сведения о том, как опубликовать шаблон при создании лаборатории, см. в разделе [Создание лаборатории для занятий](how-to-manage-classroom-labs.md#create-a-classroom-lab) .
 
## <a name="set-or-update-template-title-and-description"></a>Настройка или обновление заголовка и описания шаблона
С помощью приведенных ниже инструкций можно задать заголовок и описание в первый раз и обновить их позже. 

1. На странице **шаблон** введите новый **заголовок** для лаборатории.  
2. Введите новое **Описание** шаблона. При перемещении фокуса из текстового поля оно автоматически сохраняется. 

    ![Имя и описание шаблона](../media/how-to-create-manage-template/template-name-description.png)

## <a name="update-a-template-vm"></a>Обновление виртуальной машины шаблона
Чтобы обновить виртуальную машину шаблона, выполните следующие действия.  

1. Дождитесь запуска шаблона виртуальной машины, а затем на панели инструментов выберите **подключиться к шаблону** , чтобы подключиться к виртуальной машине шаблона, и следуйте инструкциям. Если это компьютер с Windows, вы увидите параметр для скачивания RDP-файла. 
1. После подключения к шаблону и внесения изменений он больше не будет иметь ту же настройку, что и виртуальные машины, которые были в последний раз опубликованы для пользователей. Изменения шаблона не будут отражены на существующих виртуальных машинах ваших пользователей, пока не будет выполнена повторная публикация.

    ![Подключение к шаблону виртуальной машины](../media/how-to-create-manage-template/connect-template-vm.png)
    
1. Установите программное обеспечение, необходимое для работы учащихся в лаборатории (например, Visual Studio, Обозреватель службы хранилища Azure и т. д.). 
1. Отключитесь от шаблона виртуальной машины (закрыв сеанс удаленного рабочего стола). 
1. Чтобы **Закрыть** виртуальную машину шаблона, выберите команду " **Закрыть шаблон**". 
1. Выполните действия, описанные в следующем разделе, чтобы **опубликовать** ОБНОВЛЕНную виртуальную машину шаблона. 

## <a name="publish-the-template-vm"></a>Публикация шаблона виртуальной машины  
Если вы не опубликуете шаблон при создании лаборатории, это можно сделать позже. Перед публикацией вы можете подключиться к шаблону виртуальной машины и дополнить его любым программным обеспечением. Когда вы опубликуете шаблон, Службы лабораторий Azure создадут виртуальные машины в лаборатории с помощью этого шаблона. Число виртуальных машин, созданных в этом процессе, равно количеству виртуальных машин, указанных при публикации в первый раз или на странице пула виртуальных машин. Все виртуальные машины имеют ту же конфигурацию, что и шаблон. 

1. На странице **шаблон** выберите **опубликовать** на панели инструментов. 
1. В окне с сообщением **о публикации шаблона** просмотрите текст сообщения и щелкните **Публикация**. Этот процесс может занять некоторое время в зависимости от количества создаваемых виртуальных машин.

    ![Кнопка "Опубликовать"](../media/how-to-create-manage-template/publish-button.png)

    > [!IMPORTANT]
    > Отменить публикацию шаблона будет невозможно. Но вы можете опубликовать шаблон заново. 
1. Состояние процесса публикации можно просмотреть на странице шаблона. Подождите, пока состояние шаблона изменится на **Опубликован**. 

    ![Состояние публикации](../media/how-to-create-manage-template/publish-status.png)
1. Перейдите на страницу **Виртуальные машины** и проверьте, что на ней есть виртуальные машины в состоянии **Не назначено**. Эти виртуальные машины еще не назначены учащимся. Подождите, пока виртуальные машины будут созданы. Они должны находиться в состоянии **Остановлено**. На этой странице вы можете запустить виртуальную машину для учащегося, подключиться к ней, а также остановить или удалить ее. Виртуальные машины на этой странице можете запускать как вы, так и ваши учащиеся. 

    ![Виртуальные машины в остановленном состоянии](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="next-steps"></a>Дальнейшие действия
См. следующие статьи:

- [Создание и администрирование учетных записей лаборатории (для администраторов)](how-to-manage-lab-accounts.md)
- [Создание и администрирование учетных записей лаборатории (для владельцев лаборатории)](how-to-manage-classroom-labs.md)
- [Настройка, администрирование и контроль использования лаборатории (для владельцев лаборатории)](how-to-configure-student-usage.md)
- [Доступ к лабораториям для аудитории (для пользователей лаборатории)](how-to-use-classroom-lab.md)
