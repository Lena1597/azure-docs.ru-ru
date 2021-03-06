---
title: Поддерживаемые версии — база данных Azure для MySQL
description: Узнайте, какие версии сервера MySQL поддерживаются в службе "база данных Azure для MySQL".
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: 05d4ecd58f6febff75212f1ad88b60be4f23c2a7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75454338"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Поддерживаемые версии сервера базы данных Azure для MySQL

Служба "База данных Azure для MySQL" разработана на базе [MySQL Community Edition](https://www.mysql.com/products/community/) с использованием подсистемы InnoDB.

MySQL использует схему именования X.Y.Z. X является основным номером версии, Y является дополнительным номером версии, а Z — выпуском для исправления ошибок. Дополнительные сведения о схеме см. в [документации MySQL](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

> [!NOTE]
> В службе для перенаправления подключений к экземплярам сервера используется шлюз. После установки подключения в клиенте MySQL отображается версия MySQL, установленная в шлюзе, а не фактическая версия, которая работает на вашем экземпляре сервера MySQL. Чтобы определить версию MySQL на экземпляре сервера, выполните команду `SELECT VERSION();` в командной строке MySQL.

Сейчас база данных Azure для MySQL поддерживает перечисленные ниже версии.

## <a name="mysql-version-56"></a>MySQL версии 5.6

Исправление ошибки: 5.6.45

Дополнительные сведения об улучшениях и исправлениях в этой версии см. в [заметках о выпуске](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-45.html) MySQL.

## <a name="mysql-version-57"></a>MySQL версии 5.7

Исправление ошибки: 5.7.27

Дополнительные сведения об улучшениях и исправлениях в этой версии см. в [заметках о выпуске](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-27.html) MySQL.

## <a name="mysql-version-80"></a>MySQL версии 8,0

Исправление ошибки: 8.0.15

Дополнительные сведения об улучшениях и исправлениях в этой версии см. в [заметках о выпуске](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) MySQL.

## <a name="managing-updates-and-upgrades"></a>Управление обновлениями
Служба автоматически управляет установкой исправлений для обновления версии исправлений. Например, с версии 5.7.20 до 5.7.21.  

Сейчас обновления основного и дополнительного номера версии не поддерживаются. Например, обновление с версии MySQL 5.6 до MySQL 5.7 не поддерживается. Чтобы выполнить обновление с версии 5.6 до 5.7, создайте дамп и восстановите его на сервере, который был создан с новой версией ядра. Для дополнительных сведений см. [эту статью](./concepts-migrate-dump-restore.md).

## <a name="next-steps"></a>Дальнейшие действия

Сведения о квотах и ограничениях для конкретных ресурсов с учетом вашего **уровня служб** представлены в статье [Уровни служб в базе данных Azure для MySQL](./concepts-pricing-tiers.md).
