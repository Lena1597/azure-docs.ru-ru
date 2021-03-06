---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 8a8619da831dfa5b240bd93d3a046c49cc30affa
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901406"
---
| Ресурс | Общие файлы уровня "Стандарт" | Общие файловые ресурсы уровня "Премиум" |
|----------|---------------|------------------------------------------|
| Минимальный размер общего файла | Не минимально; Оплата по мере использования | 100 гиб; подготовлены |
| Максимальный размер общей папки | 100 Тиб *, 5 тиб | 100 тиб |
| Максимальный размер файла в общей папке | 1 ТиБ | 1 ТиБ |
| Максимальное число файлов в общей папке | Без ограничений | Без ограничений |
| Максимум операций ввода-вывода в секунду на общую папку | 10 000 ОПЕРАЦИЙ ВВОДА-ВЫВОДА *, 1 000 ОПЕРАЦИЙ ВВОДА-ВЫВОДА | 100 000 операций ввода-вывода в секунду |
| Максимальное число хранимых политик доступа на общую папку | 5 | 5 |
| Целевая пропускная способность для одной общей папки | до 300 MiB/с *, до 60 MiB/сек  | См. Дополнительные сведения о входных и выходных данных файловых ресурсов уровня "Премиум"|
| Максимальный исходящий трафик для одной общей папки | См. пропускная способность стандартной общей папки | До 6 204 MiB/с |
| Максимальное число входящих данных для одной общей папки | См. пропускная способность стандартной общей папки | До 4 136 MiB/с |
| Максимальное количество открытых дескрипторов на файл | 2000 открытых маркеров | 2000 открытых маркеров |
| Максимальное число моментальных снимков общих ресурсов | 200 моментальных снимков общих ресурсов | 200 моментальных снимков общих ресурсов |
| Максимальная длина имени объекта максимальное (папок и файлов) | 2 048 символов | 2 048 символов |
| Максимальное количество компонентов в имени пути (в пути \A\B\C\D каждая буква обозначает компонент) | 255 символов | 255 символов |

\*, доступных в большинстве регионов, см. в разделе региональные сведения о [доступности](../articles/storage/files/storage-files-planning.md#regional-availability) в доступных регионах.
