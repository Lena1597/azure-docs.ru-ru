---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 1ab6243be39bf30bc060ed5745fbf600924743a9
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839230"
---
| Resource | Ограничение |
| --- | --- |
| Объем кэша |1,2 ТБ |
| Базы данных |64 |
| Максимальное число подключенных клиентов |40 000 |
| Кэш Azure для реплик Redis для обеспечения высокой доступности |1 |
| Сегменты кэша уровня "Премиум" с включенной кластеризацией |10 |

Ограничения и размер кэша Redis для Azure отличаются для каждой ценовой категории. Ценовые категории и связанные с ними размеры см. на странице [цен на Azure для Redis](https://azure.microsoft.com/pricing/details/cache/).

Дополнительные сведения об ограничениях конфигурации кэша Redis для Azure см. в разделе о [конфигурации сервера Redis по умолчанию](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Так как настройка экземпляров кэша Redis для Azure и управление ими осуществляется корпорацией Майкрософт, кэш Redis для Azure поддерживает не все команды Redis. Дополнительные сведения см. в разделе о [командах Redis, неподдерживаемых в кэше Redis для Azure](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

