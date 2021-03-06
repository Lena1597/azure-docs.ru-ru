---
title: Ограничения ресурсов для службы Azure NetApp Files | Документация Майкрософт
description: Описание ограничений для Azure NetApp Filesных ресурсов и способов запроса увеличения лимита ресурсов.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: b-juche
ms.openlocfilehash: 6fcea0aaecb860e07c2066877494c05b51f43ca4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976253"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Ограничения ресурсов для службы Azure NetApp Files

Понимание ограничений ресурсов для службы Azure NetApp Files помогает в управлении томами.

## <a name="resource-limits"></a>Ограничения ресурсов

В следующей таблице описаны ограничения ресурсов для Azure NetApp Files.

|  Ресурс  |  Ограничение по умолчанию  |  Регулируемый запрос поддержки Via  |
|----------------|---------------------|--------------------------------------|
|  Число учетных записей NetApp в каждом регионе Azure   |  10    |  ДА   |
|  Число пулов мощностей на учетную запись NetApp   |    25     |   ДА   |
|  Число томов на пул емкости     |    500   |    ДА     |
|  Число моментальных снимков на том       |    255     |    Нет        |
|  Число подсетей, делегированных в Azure NetApp Files (Microsoft. NetApp/Volumes) на виртуальную сеть Azure    |   1   |    Нет    |
|  Число IP-адресов в виртуальной сети (включая одноранговые виртуальных сетей), которые могут получить доступ к Azure NetApp Files   |    1000   |    ДА   |
|  Минимальный размер одного пула емкости   |  4 ТиБ     |    Нет  |
|  Максимальный размер одного пула емкости    |  500 ТиБ   |   Нет   |
|  Минимальный размер одного тома    |    100 ГБ    |    Нет    |
|  Максимальный размер одного тома     |    100 тиб    |    Нет    |
|  Максимальное количество файлов ([MAXSIZE](#maxfiles)) на том     |    100 млн    |    ДА    |    
|  Максимальный размер одного файла     |    16 ТиБ    |    Нет    |    

## Ограничения MAXSIZE<a name="maxfiles"></a> 

Azure NetApp Files тома имеют ограничение с именем *MAXSIZE*. Ограничение MAXSIZE — число файлов, которое может содержаться в томе. Ограничение MaxSize для тома Azure NetApp Files индексируется в зависимости от размера (квоты) тома. Ограничение MaxSize для тома увеличивается или уменьшается по частоте 20 000 000 файлов на Тиб подготовленного тома. 

Служба динамически корректирует ограничение MaxSize для тома на основе его подготовленного размера. Например, для тома, изначально настроенного с размером 1 Тиб, MAXSIZE ограничение в 20 000 000. Последующие изменения размера тома приведут к автоматической повторной настройке ограничения MAXSIZE на основе следующих правил. 

|    Размер тома (квота)     |  Автоматическая перенастройка ограничения MAXSIZE    |
|----------------------------|-------------------|
|    < 1 тиб                 |    20 000 000     |
|    > = 1 Тиб, но < 2 тиб    |    40 000 000     |
|    > = 2 Тиб, но < 3 тиб    |    60 млн     |
|    > = 3 Тиб, но < 4 тиб    |    80 000 000     |
|    > = 4 тиб                |    100 млн    |

Для любого размера тома можно отправить [запрос в службу поддержки](#limit_increase) , чтобы увеличить ограничение maxsize свыше 100 000 000.

## Увеличение лимита запросов<a name="limit_increase"></a> 

Вы можете создать запрос в службу поддержки Azure, чтобы увеличить регулируемые ограничения из приведенной выше таблицы. 

Из портал Azureной плоскости навигации: 

1. Щелкните **Справка и поддержка**.
2. Щелкните **+ новый запрос в службу поддержки**.
3. На вкладке основы укажите следующие сведения. 
    1. Тип проблемы: выберите **пределы службы и подписки (квоты)** .
    2. Подписки. Выберите подписку на ресурс, для которого требуется увеличить квоту.
    3. Тип квоты: выберите **хранилище: пределы Azure NetApp Files**.
    4. Нажмите кнопку **Далее: решения**.
4. На вкладке сведения выполните следующие действия.
    1. В поле Описание укажите следующие сведения для соответствующего типа ресурса.

        |  Ресурс  |    Родительские ресурсы      |    Запрошенные новые ограничения     |    Причина увеличения квоты       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Учетная запись. |  *Идентификатор подписки*   |  *Запрошен новый максимальный номер **учетной записи***    |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  пул    |  *Идентификатор подписки, URI учетной записи*  |  *Запрошен новый максимальный номер **пула***   |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  Том  |  *Идентификатор подписки, URI учетной записи, URI пула*   |  *Запрошен новый максимальный номер **тома***     |  *Какой сценарий или вариант использования запрашивает запрос?*  |
        |  MAXSIZE  |  *Идентификатор подписки, URI учетной записи, URI пула, URI тома*   |  *Запрошено новое максимальное число **MAXSIZE***     |  *Какой сценарий или вариант использования запрашивает запрос?*  |    

    2. Укажите соответствующий метод поддержки и укажите сведения о контракте.

    3. Чтобы создать запрос, нажмите кнопку **Далее: Проверка + создать** . 


## <a name="next-steps"></a>Дальнейшие действия  

- [Общие сведения об иерархии хранилища Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Модель затрат для Azure NetApp Files](azure-netapp-files-cost-model.md)
