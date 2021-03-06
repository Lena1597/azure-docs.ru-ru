---
title: Хранилище запросов — База данных Azure для MariaDB
description: Сведения о функции хранилища запросов в службе "база данных Azure для MariaDB", чтобы помочь вам в отслеживании производительности с течением времени.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: fbc814b5d263e20cea1d961891afb19894b78965
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772222"
---
# <a name="monitor-azure-database-for-mariadb-performance-with-query-store"></a>Мониторинг производительности базы данных Azure для MariaDB с помощью хранилища запросов

**Применимо к:** База данных Azure для MariaDB 10,2

Функция хранилища запросов в базе данных Azure для MariaDB позволяет контролировать производительность запросов с течением времени. Хранилище запросов упрощает устранение неполадок, позволяя быстро выявлять самые медленные и ресурсоемкие запросы. Хранилище запросов автоматически ведет журнал запросов и статистики выполнения и сохраняет их для просмотра. Этот компонент разделяет данные по периодам, давая представление о закономерностях использования баз данных. Данные для всех пользователей, баз данных и запросов хранятся в базе данных схемы **MySQL** в экземпляре базы данных Azure для MariaDB.

## <a name="common-scenarios-for-using-query-store"></a>Распространенные сценарии использования хранилища запросов

Хранилище запросов можно использовать в ряде сценариев, включая следующие:

- Обнаружение регрессионных запросов
- определение числа выполнений запроса за данный период времени;
- сравнение среднего времени выполнения запроса за периоды времени для выявления больших расхождений;

## <a name="enabling-query-store"></a>Включение хранилища запросов

Хранилище запросов является дополнительной функцией, поэтому оно не активно на сервере по умолчанию. Хранилище запросов включено или отключено глобально для всех баз данных на заданном сервере и не может быть включено или отключено для каждой базы данных.

### <a name="enable-query-store-using-the-azure-portal"></a>Включение хранилища запросов с помощью портала Azure

1. Войдите в портал Azure и выберите сервер базы данных Azure для MariaDB.
1. В разделе меню **Параметры** выберите **Параметры сервера**.
1. Найдите параметр query_store_capture_mode.
1. Задайте для параметра значение все и **Сохраните**.

Чтобы включить статистику ожидания в хранилище запросов, сделайте следующее:

1. Найдите параметр query_store_wait_sampling_capture_mode.
1. Задайте для параметра значение все и **Сохраните**.

Дождитесь, пока первый пакет данных будет сохранен в базе данных MySQL до 20 минут.

## <a name="information-in-query-store"></a>Данные в хранилище запросов

Хранилище запросов включает два хранилища:

- Хранилище статистики времени выполнения для сохранения статистических данных о выполнении запроса.
- Хранилище статистики ожидания для сохранения сведений о статистике ожидания.

Чтобы максимально сокращать использование пространства, статистические данные о выполнении в хранилище статистики времени выполнения суммируются в фиксированное, настраиваемое временное окно. Сведения в этих хранилищах отображаются путем запроса представлений хранилища запросов.

Следующий запрос возвращает сведения о запросах в хранилище запросов:

```sql
SELECT * FROM mysql.query_store;
```

Или этот запрос для статистики ожидания:

```sql
SELECT * FROM mysql.query_store_wait_stats;
```

## <a name="finding-wait-queries"></a>Поиск запросов ожидания

> [!NOTE]
> Статистика ожидания не должна включаться в часы пиковой рабочей нагрузки или быть включена на неопределенное время для конфиденциальных рабочих нагрузок. <br>Для рабочих нагрузок, работающих с высокой загрузкой ЦП или на серверах, настроенных с пониженным виртуальных ядер, будьте внимательны при включении статистики ожидания. Он не должен быть включен в течение неограниченного времени. 

Типы событий ожидания объединяют разные события ожидания в группы по принципу сходства. Хранилище запросов предоставляет тип события ожидания, имя определенного события ожидания и запрашиваемый запрос. Возможность сопоставлять эти сведения об ожидании со статистикой времени выполнения запроса позволяет получить более глубокое понимание аспектов, влияющих на характеристики производительности запросов.

Ниже приведены некоторые примеры получения более подробных сведений о рабочей нагрузке с помощью статистики ожидания в хранилище запросов.

| **Наблюдение** | **Действие** |
|---|---|
|Ожидания с высоким уровнем блокировки | Проверьте текст затронутых запросов и определите целевые сущности. Найдите в хранилище запросов другие запросы, изменяющие ту же сущность, которые часто выполняются и (или) имеют большую длительность. Определив эти запросы, рекомендуется изменить логику приложения, чтобы улучшить параллелизм, или использовать менее строгий уровень изоляции. |
|Ожидания с большим числом операций ввода-вывода буфера | Найдите в хранилище запросов запросы с большим числом физических операций чтения. Если они соответствуют запросам с высокими ожиданиями ввода-вывода, рассмотрите возможность введения индекса в базовую сущность для поиска вместо проверок. Это позволит свести к минимуму затраты на операции ввода-вывода запросов. Ознакомьтесь с **рекомендациями по повышению производительности** серверов на портале: возможно, для этого сервера есть рекомендации по индексам, которые позволят оптимизировать запросы. |
|Ожидания с высокой загрузкой памяти | Найдите в хранилище запросов запросы, использующие больше всего памяти. Эти запросы, скорее всего, дополнительно задерживают выполнение соответствующих запросов. Ознакомьтесь с **рекомендациями по повышению производительности** для сервера на портале: возможно, есть рекомендации по индексам, которые позволят оптимизировать запросы.|

## <a name="configuration-options"></a>Варианты настройки

При включении хранилища запросов оно сохраняет данные с 15-минутным периодом агрегирования: не более 500 уникальных запросов на период.

Ниже приведены настраиваемые параметры для хранилища запросов.

| **Параметр** | **Описание** | **По умолчанию** | **Range** |
|---|---|---|---|
| query_store_capture_mode | Включите или отключите функцию хранилища запросов в зависимости от значения. Примечание. Если performance_schema отключен, включение query_store_capture_mode будет включать performance_schema и подмножество инструментов схемы производительности, необходимых для этой функции. | ALL | НЕТ, ВСЕ |
| query_store_capture_interval | Интервал записи в хранилище запросов в минутах. Позволяет указать интервал агрегирования метрик запроса | 15 | 5 - 60 |
| query_store_capture_utility_queries | Включение или отключение для сбора всех запросов служебной программы, которые выполняются в системе. | НЕТ | ДА, НЕТ |
| query_store_retention_period_in_days | Временное окно в днях для сохранения данных в хранилище запросов. | 7 | 1–30 |

Следующие параметры применяются исключительно к статистике ожидания.

| **Параметр** | **Описание** | **По умолчанию** | **Range** |
|---|---|---|---|
| query_store_wait_sampling_capture_mode | Позволяет включать и выключать статистику ожидания. | НЕТ | НЕТ, ВСЕ |
| query_store_wait_sampling_frequency | Изменяет частоту выборки времени ожидания в секундах. от 5 до 300 секунд. | 30 | 5-300 |

> [!NOTE]
> В настоящее время **query_store_capture_mode** заменяет эту конфигурацию, что означает, что **query_store_capture_mode** и **QUERY_STORE_WAIT_SAMPLING_CAPTURE_MODE** должны быть включены для всех, чтобы статистика ожидания работала. Если **query_store_capture_mode** выключен, то статистика ожидания отключается, так как в статистике ожидания используется включенная performance_schema и query_text, захваченная хранилищем запросов.

Используйте [портал Azure](howto-server-parameters.md) , чтобы получить или задать другое значение для параметра.

## <a name="views-and-functions"></a>Представления и функции

Просмотр и управление хранилищем запросов осуществляеются с помощью следующих представлений и функций. Все пользователи в [открытой роли выбор привилегий](howto-create-users.md#create-additional-admin-users) могут использовать эти представления для просмотра данных в хранилище запросов. Эти представления доступны только в базе данных **MySQL** .

Запросы нормализованы: обратите внимание на их структуру после удаления литералов и констант. Если два запроса идентичны, за исключением литеральных значений, они будут иметь один хэш.

### <a name="mysqlquery_store"></a>MySQL. query_store

Это представление возвращает все данные в хранилище запросов. Для каждого отдельного идентификатора базы данных, идентификатора пользователя и идентификатора запроса используется отдельная строка.

| **Имя** | **Тип данных** | **IS_NULLABLE** | **Описание** |
|---|---|---|---|
| `schema_name`| varchar (64) | НЕТ | Имя схемы |
| `query_id`| bigint (20) | НЕТ| Уникальный идентификатор, сформированный для конкретного запроса. Если тот же запрос выполняется в другой схеме, будет создан новый идентификатор |
| `timestamp_id` | Timestamp| НЕТ| Отметка времени выполнения запроса. Это основано на конфигурации query_store_interval|
| `query_digest_text`| longtext| НЕТ| Нормализованный текст запроса после удаления всех литералов|
| `query_sample_text` | longtext| НЕТ| Первый внешний вид фактического запроса с литералами|
| `query_digest_truncated` | bit| ДА| Был ли текст запроса усечен. Если длина запроса превышает 1 КБ, будет указано значение Да.|
| `execution_count` | bigint (20)| НЕТ| Количество выполнений запроса для этого идентификатора метки времени/в течение заданного периода времени|
| `warning_count` | bigint (20)| НЕТ| Число предупреждений, созданных во время внутреннего запроса|
| `error_count` | bigint (20)| НЕТ| Количество ошибок, созданных этим запросом в течение интервала|
| `sum_timer_wait` | Double| ДА| Общее время выполнения этого запроса в течение интервала|
| `avg_timer_wait` | Double| ДА| Среднее время выполнения для этого запроса в течение интервала|
| `min_timer_wait` | Double| ДА| Минимальное время выполнения этого запроса|
| `max_timer_wait` | Double| ДА| Максимальное время выполнения|
| `sum_lock_time` | bigint (20)| НЕТ| Общий объем времени, затраченного на все блокировки для выполнения этого запроса в течение этого периода времени|
| `sum_rows_affected` | bigint (20)| НЕТ| Число затронутых строк|
| `sum_rows_sent` | bigint (20)| НЕТ| Число строк, отправленных клиенту|
| `sum_rows_examined` | bigint (20)| НЕТ| Число проверенных строк.|
| `sum_select_full_join` | bigint (20)| НЕТ| Число полных соединений|
| `sum_select_scan` | bigint (20)| НЕТ| Число просмотров SELECT |
| `sum_sort_rows` | bigint (20)| НЕТ| Число отсортированных строк|
| `sum_no_index_used` | bigint (20)| НЕТ| Количество случаев, когда запрос не использовал индексы|
| `sum_no_good_index_used` | bigint (20)| НЕТ| Сколько раз подсистема выполнения запросов не использовала хорошие индексы|
| `sum_created_tmp_tables` | bigint (20)| НЕТ| Общее число созданных временных таблиц|
| `sum_created_tmp_disk_tables` | bigint (20)| НЕТ| Общее число временных таблиц, созданных на диске (создает операции ввода-вывода)|
| `first_seen` | Timestamp| НЕТ| Первое вхождение (UTC) запроса во время выполнения статистического окна|
| `last_seen` | Timestamp| НЕТ| Последнее вхождение (UTC) запроса во время этого окна агрегирования|

### <a name="mysqlquery_store_wait_stats"></a>MySQL. query_store_wait_stats

Это представление возвращает данные событий ожидания в хранилище запросов. Для каждого отдельного идентификатора базы данных, идентификатора пользователя, идентификатора запроса и события используется отдельная строка.

| **Имя**| **Тип данных** | **IS_NULLABLE** | **Описание** |
|---|---|---|---|
| `interval_start` | Timestamp | НЕТ| Начало интервала (15-минутное приращение)|
| `interval_end` | Timestamp | НЕТ| Окончание интервала (15-минутное приращение)|
| `query_id` | bigint (20) | НЕТ| Созданный уникальный идентификатор для нормализованного запроса (из хранилища запросов)|
| `query_digest_id` | varchar (32) | НЕТ| Нормализованный текст запроса после удаления всех литералов (из хранилища запросов) |
| `query_digest_text` | longtext | НЕТ| Первый внешний вид фактического запроса с литералами (из хранилища запросов) |
| `event_type` | varchar (32) | НЕТ| Категория события ожидания |
| `event_name` | varchar (128) | НЕТ| Имя события ожидания |
| `count_star` | bigint (20) | НЕТ| Число событий ожидания, выбранных в течение интервала для запроса |
| `sum_timer_wait_ms` | Double | НЕТ| Общее время ожидания (в миллисекундах) этого запроса в течение интервала |

### <a name="functions"></a>Функции Azure

| **Имя**| **Описание** |
|---|---|
| `mysql.az_purge_querystore_data(TIMESTAMP)` | Очистка всех данных хранилища запросов до заданной отметки времени |
| `mysql.az_procedure_purge_querystore_event(TIMESTAMP)` | Удаляет все данные событий ожидания до заданной отметки времени |
| `mysql.az_procedure_purge_recommendation(TIMESTAMP)` | Удаляет рекомендации, срок действия которых предшествует заданной метке времени |

## <a name="limitations-and-known-issues"></a>Ограничения и известные проблемы

- Если сервер MariaDB имеет параметр `default_transaction_read_only` on, хранилище запросов не может записывать данные.
- Функции хранилища запросов могут быть прерваны, если обнаруживаются длинные запросы в Юникоде (\>= 6000 байт).
- Срок хранения статистики ожидания составляет 24 часа.
- Статистика ожидания использует образец TI для записи доли событий. Частоту можно изменить с помощью параметра `query_store_wait_sampling_frequency`.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об [аналитике производительности запросов](concepts-query-performance-insight.md)
