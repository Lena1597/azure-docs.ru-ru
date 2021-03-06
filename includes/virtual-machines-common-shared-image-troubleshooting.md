---
title: включить файл
description: включить файл
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 40ba5a935e78cd75c4fcd7729e44f1cdf6c2859b
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772849"
---
Если возникнут проблемы при выполнении операций с коллекциями общих образов, определениями и версиями образов, выполните команду, которая дала сбой, в режиме отладки. Режим отладки активируется путем передачи параметра **-debug** в интерфейсе командной строки и параметра **-Debug** в PowerShell. Обнаружив ошибку, следуйте инструкциям в этом документе для ее устранения.


## <a name="unable-to-create-a-shared-image-gallery"></a>Не удалось создать коллекцию общих образов

Возможные причины:

*Недопустимое имя коллекции.*

Допустимыми знаками для имени коллекции являются прописные или строчные буквы, цифры и точки. Имя коллекции не может содержать дефисы. Измените имя коллекции и повторите попытку. 

*Имя коллекции не является уникальным в рамках подписки.*

Выберите другое имя коллекции и повторите попытку.


## <a name="unable-to-create-an-image-definition"></a>Не удалось создать определение образа 

Возможные причины:

*Недопустимое имя определения образа.*

Допустимые символы для определения образа: прописные или строчные буквы, цифры, точки, тире и точки. Измените имя определения образа и повторите попытку.

*Обязательные свойства для создания определения образа не заполнены.*

Такие свойства, как имя, издатель, предложение, SKU и тип ОС, являются обязательными. Проверьте, передаются ли все свойства.

Убедитесь, что значение **OSType** определения образа (Linux или Windows) такое же, как у исходного управляемого образа, который вы используете для создания версии образа. 


## <a name="unable-to-create-an-image-version"></a>Не удалось создать версию образа 

Возможные причины:

*Недопустимое имя версии образа.*

Допустимыми знаками для имени версии образа являются цифры и точки. Числа должны быть в диапазоне 32-битного целого числа. Формат: *основной номер версии.дополнительный номер версии.исправление*. Измените имя версии образа и повторите попытку.

*Исходный управляемый образ, из которого создается версия образа, не найден.* 

Проверьте, существует ли исходный образ и находится ли он в том же регионе, что и версия образа.

*Управляемый образ еще не подготовлен к работе.*

Убедитесь, что состояние подготовки исходного управляемого образа — **Успешно**.

*Список целевых регионов не включает исходный регион.*

Список целевых регионов должен содержать исходный регион версии образа. Убедитесь, что вы включили исходный регион в список целевых регионов, куда нужно реплицировать версию образа с помощью Azure.

*Репликация во все целевые регионы не завершена.*

Используйте параметр **--expand ReplicationStatus**, чтобы проверить, завершена ли репликация во все указанные целевые регионы. Если это не так, дождитесь завершения задания. Это может занять до 6 часов. Если происходит сбой, выполните команду еще раз, чтобы создать и реплицировать версию образа. Если у вас много целевых регионов для репликации версии образа, то рассмотрите возможность выполнения репликации поэтапно.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>Не удалось создать виртуальную машину или масштабируемый набор 

Возможные причины:

*У пользователя, пытающегося создать виртуальную машину или масштабируемый набор виртуальных машин, нет доступа на чтение к версии образа.*

Обратитесь к владельцу подписки и попросите его предоставить доступ на чтение к версии образа или родительским ресурсам (например, коллекции общих образов или определению образа) через компонент [управления доступом на основе ролей](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

*Версия образа не найдена.*

Убедитесь, что вы пытаетесь создать виртуальную машину или масштабируемый набор виртуальных машин в регионе, который включен в список целевых регионов версии образа. Если регион уже находится в списке целевых регионов, то проверьте, завершено ли задание репликации. Вы можете использовать параметр **-ReplicationStatus**, чтобы проверить, завершена ли репликация во все указанные целевые регионы. 

*Создание виртуальной машины или масштабируемого набора виртуальных машин занимает много времени.*

Убедитесь, что значение **OSType** версии образа, на основе которой вы пытаетесь создать виртуальную машину или масштабируемый набор виртуальных машин, соответствует значению **OSType** исходного управляемого образа, который использовался для создания версии образа. 

## <a name="unable-to-share-resources"></a>Не удается предоставить общий доступ к ресурсам

Совместное использование общей коллекции образов, определения образа и версий изображений в подписках включается с помощью [управления доступом на основе ролей](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

## <a name="replication-is-slow"></a>Медленная репликация

Используйте параметр **--expand ReplicationStatus**, чтобы проверить, завершена ли репликация во все указанные целевые регионы. Если это не так, дождитесь завершения задания. Это может занять до 6 часов. Если происходит сбой, активируйте команду еще раз, чтобы создать и реплицировать версию образа. Если у вас много целевых регионов для репликации версии образа, то рассмотрите возможность выполнения репликации поэтапно.

## <a name="azure-limits-and-quotas"></a>Ограничения и квоты Azure 

[Ограничения и квоты Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) применяются ко всем ресурсам коллекции общих образов, определения и версии образа. Убедитесь, что вы не превысили пределы для своей подписки. 



