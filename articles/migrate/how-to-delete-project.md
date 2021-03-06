---
title: Удаление проекта службы "миграция Azure"
description: Описание процесса создания проекта службы "миграция Azure" и добавления средства оценки и миграции.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/22/2019
ms.author: raynew
ms.openlocfilehash: 55842d36cddb2a7851ff5bd7002c20e9873158f5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512734"
---
# <a name="delete-an-azure-migrate-project"></a>Удаление проекта службы "миграция Azure"

В этой статье описывается, как удалить проект службы " [Миграция Azure](migrate-overview.md) ".


## <a name="before-you-start"></a>Перед началом работы

Перед удалением проекта выполните следующие действия.

- При удалении проекта, проект и метаданные обнаруженного компьютера удаляются.
- Если вы подключили рабочую область Log Analytics к средству оценки серверов для анализа зависимостей, решите, хотите ли вы удалить рабочую область. 
    - Рабочая область не удаляется автоматически. Удалите его вручную.
    - Прежде чем удалить рабочую область, убедитесь, что она используется. Ту же рабочую область Log Analytics можно использовать для нескольких сценариев.
    - Перед удалением проекта можно найти ссылку на рабочую область в разделе " **Миграция Azure" — серверы** > **Azure Migrate-Server — Оценка**серверов в **рабочей области OMS**.
    - Чтобы удалить рабочую область после удаления проекта, найдите рабочую область в соответствующей группе ресурсов и следуйте [этим инструкциям](../azure-monitor/platform/delete-workspace.md).


## <a name="delete-a-project"></a>Удаление проекта


1. В портал Azure откройте группу ресурсов, в которой был создан проект.
2. На странице Группа ресурсов выберите пункт **Показывать скрытые типы**.
3. Выберите проект и связанные ресурсы, которые необходимо удалить.
    - Тип ресурса для проектов службы "миграция Azure" — **Microsoft. Migrate/мигратепрожектс**.
    - В следующем разделе изучите ресурсы, созданные для обнаружения, оценки и миграции в проекте службы "миграция Azure".
    - Если группа ресурсов содержит только проект службы "миграция Azure", можно удалить всю группу ресурсов.
    - Если вы хотите удалить проект из предыдущей версии службы "миграция Azure", шаги одинаковы. Тип ресурса для этих проектов — **Migration Project**.


## <a name="created-resources"></a>Созданные ресурсы

В этих таблицах представлены ресурсы, созданные для обнаружения, оценки и миграции в проекте службы "миграция Azure".

> [!NOTE]
> Удалите хранилище ключей с осторожностью, так как оно может содержать ключи безопасности.

### <a name="vmwarephysical-server"></a>VMware или физический сервер

**Ресурс** | **Тип**
--- | ---
"Апплианценаме" KV | Хранилище ключей
Сайт "апплианценаме" | Microsoft. Оффазуре/Вмвареситес
Проекта | Microsoft. Migrate/мигратепрожектс
Проект "имя_проекта" | Microsoft. Migrate/Ассессментпрожектс
"Имя_проекта" рсваулт | Хранилище служб восстановления
"Имя_проекта" — Мигратеваулт-* | Хранилище служб восстановления
мигратеапплигвса * | Учетная запись хранения
мигратеапплилса * | Учетная запись хранения
мигратеаппликса * | Учетная запись хранения
мигратеаппликв * | Хранилище ключей
migrateapplisbns16041 | Пространство имен служебной шины

### <a name="hyper-v-vm"></a>Виртуальная машина Hyper-V 

**Ресурс** | **Тип**
--- | ---
Проекта | Microsoft. Migrate/мигратепрожектс
Проект "имя_проекта" | Microsoft. Migrate/Ассессментпрожектс
HyperV * KV | Хранилище ключей
Сайт HyperV * | Microsoft. Оффазуре/Хипервситес
"Имя_проекта" — Мигратеваулт-* | Хранилище служб восстановления


## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как добавить дополнительные средства [оценки](how-to-assess.md) и [миграции](how-to-migrate.md) . 
