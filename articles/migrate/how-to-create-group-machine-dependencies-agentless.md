---
title: Группирование компьютеров в службе "миграция Azure" с помощью визуализации зависимостей без агента
description: Описывает создание групп с использованием зависимостей компьютера без агента.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/18/2019
ms.author: hamusa
ms.openlocfilehash: c8ddd343cd00b24506382521361ebad33ad112a7
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049753"
---
# <a name="set-up-agentless-dependency-visualization-for-assessment"></a>Настройка визуализации зависимостей без агента для оценки

В этой статье описывается, как настроить сопоставление зависимостей без агента в службе "миграция Azure": Оценка сервера. 

> [!IMPORTANT]
> Визуализация зависимостей без агента сейчас доступна в предварительной версии для виртуальных машин Azure VMware, обнаруженных с помощью устройства миграции Azure.
> Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Эта предварительная версия охватывается службой поддержки клиентов и может использоваться для рабочих нагрузок.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="about-dependency-mapping"></a>О сопоставлении зависимостей

Сопоставление зависимостей позволяет визуализировать зависимости между компьютерами, которые необходимо оценить и перенести. Сопоставление зависимостей обычно используется, если требуется оценить компьютеры с более высоким уровнем достоверности.

- В службе "миграция Azure": Оценка серверов позволяет собирать компьютеры вместе в группы для оценки. Обычно группы состоят из компьютеров, которые требуется перенести вместе, и сопоставление зависимостей помогает перекрестно проверить зависимости компьютеров, чтобы можно было точно сгруппировать компьютеры.
- С помощью сопоставления можно обнаружить взаимозависимые системы, которые необходимо перенести вместе. Можно также определить, обслуживает ли работающая система пользователей, или же является кандидатом на списание вместо миграции.
- Визуализация зависимостей позволяет убедиться, что ничего не осталось, и избежать неожиданных сбоев во время миграции.

## <a name="about-agentless-visualization"></a>О визуализации без агента

Для визуализации зависимостей без агента не требуется устанавливать агенты на компьютерах. Он работает путем записи данных о подключении TCP с компьютеров, для которых он включен.

- После запуска обнаружения зависимостей устройство собирает данные с компьютеров через интервал опроса, равный пяти минутам.
- Собираются следующие данные:
    - TCP-подключения
    - Имена процессов, имеющих активные соединения
    - Имена установленных приложений, запускающих описанные выше процессы
    - Нет. подключений, обнаруженных при каждом интервале опроса

## <a name="current-limitations"></a>Текущие ограничения

- Визуализация зависимостей без агента сейчас доступна только для виртуальных машин VMware.
- Сейчас вы не можете добавить или удалить сервер из группы в представлении "анализ зависимостей".
- Схема зависимостей для группы серверов в настоящее время недоступна.
- В настоящее время невозможно скачать данные зависимостей в табличном формате.

## <a name="before-you-start"></a>Перед началом работы

- Убедитесь, что вы [создали](how-to-add-tool-first-time.md) проект "миграция Azure".
- Анализ зависимостей без агента в настоящее время доступен только для компьютеров VMware.
- Если вы уже создали проект, убедитесь, что вы [добавили](how-to-assess.md) средство Azure Migrate: Server для оценки серверов.
- Убедитесь, что вы обнаружили компьютеры VMware в службе "миграция Azure". Это можно сделать, настроив устройство Azure Migrate для [VMware](how-to-set-up-appliance-vmware.md). Устройство обнаруживает локальные компьютеры и отправляет метаданные и данные производительности в службу "миграция Azure": Оценка сервера. [Дополнительные сведения](migrate-appliance.md)
- [Ознакомьтесь с требованиями](migrate-support-matrix-vmware.md#agentless-dependency-visualization) к настройке визуализации зависимостей без агента.



## <a name="create-a-user-account-for-discovery"></a>Создание учетной записи пользователя для обнаружения

Настройте учетную запись пользователя с необходимыми разрешениями, чтобы оценка сервера могла получить доступ к виртуальной машине для обнаружения. Можно указать одну учетную запись пользователя.

- **Требуемое разрешение на виртуальных машинах Windows**. учетная запись пользователя должна быть локальной или администратором домена.
- **Требуемое разрешение на виртуальные машины Linux**. для учетной записи требуются права доступа root. Кроме того, учетная запись пользователя должна иметь следующие две возможности в файлах/бин/нетстат и/бин/ЛС: CAP_DAC_READ_SEARCH и CAP_SYS_PTRACE.

## <a name="add-the-user-account-to-the-appliance"></a>Добавление учетной записи пользователя в устройство

Необходимо добавить учетную запись пользователя в устройство.

Добавьте учетную запись следующим образом:

1. Откройте приложение управления устройством. Перейдите на панель **Укажите сведения о vCenter** .
2. В разделе **обнаружение приложения и зависимостей на виртуальных машинах** щелкните **Добавить учетные данные** .
3. Выберите **операционную систему**.
4. Укажите понятное имя для учетной записи.
5. Укажите **имя пользователя** и **пароль**
6. Выберите команду **Сохранить**.
7. Нажмите кнопку **сохранить и начать обнаружение**.

    ![Добавление учетной записи пользователя виртуальной машины](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Запустить обнаружение зависимостей

Выберите компьютеры, на которых необходимо включить обнаружение зависимостей.

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Щелкните значок **анализ зависимостей** .
3. Нажмите кнопку **Добавить серверы**.
3. На странице **Добавление серверов** выберите устройство, которое будет обнаруживать соответствующие компьютеры.
4. В списке компьютер выберите компьютеры.
5. Нажмите кнопку **Добавить серверы**.

    ![Запустить обнаружение зависимостей](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

После запуска обнаружения зависимостей вы сможете визуализировать зависимости 6 часов.

## <a name="visualize-dependencies"></a>Визуализация зависимостей

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Найдите компьютер, для которого требуется просмотреть карту зависимостей.
3. Щелкните **Просмотреть зависимости** в столбце **зависимости** .
4. Измените период времени, для которого необходимо просмотреть карту, с помощью раскрывающегося списка **Длительность времени** .
5. Разверните группу **клиентов** , чтобы получить список компьютеров, имеющих зависимость от выбранного компьютера.
6. Разверните группу **портов** , чтобы получить список компьютеров, имеющих зависимость от выбранного компьютера.
7. Чтобы перейти к представлению карт зависимых компьютеров, щелкните имя компьютера и нажмите кнопку **загрузить карту сервера** .

    ![Развертывание группы портов сервера и загрузка схемы сервера](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Развернуть группу клиентов ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Разверните выбранный компьютер, чтобы просмотреть сведения об уровне процесса для каждой зависимости.

    ![Развертывание сервера для отображения процессов](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Сведения о процессе для зависимости не всегда доступны. Если он недоступен, зависимость отображается с процессом, помеченным как "Неизвестный процесс".

## <a name="stop-dependency-discovery"></a>Отключить обнаружение зависимостей

Выберите компьютеры, на которых требуется отключить обнаружение зависимостей.

1. В **службе "миграция Azure": Оценка сервера**щелкните **обнаруженные серверы**.
2. Щелкните значок **анализ зависимостей** .
3. Нажмите кнопку **удалить серверы**.
3. На странице **Удаление серверов** выберите **устройство** , которое обнаруживает виртуальные машины, на которых вы хотите отключить обнаружение зависимостей.
4. В списке компьютер выберите компьютеры.
5. Нажмите кнопку **удалить серверы**.


## <a name="next-steps"></a>Дальнейшие действия

[Сгруппируйте компьютеры](how-to-create-a-group.md)
