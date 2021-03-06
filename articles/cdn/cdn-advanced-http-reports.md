---
title: Анализ статистики использования с помощью расширенных отчетов HTTP Azure CDN | Документация Майкрософт
description: Узнайте, как создавать расширенные отчеты HTTP в Microsoft Azure CDN. Эти отчеты предоставляют подробные сведения об активности в сети CDN.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0b0eec2425f8a1663eb7a09c83a6bad037d1d79c
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594113"
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Анализ статистики использования с помощью расширенных отчетов HTTP Azure CDN
## <a name="overview"></a>Обзор
В этом документе рассматриваются дополнительные возможности HTTP-отчетов в сети CDN Microsoft Azure. Эти отчеты предоставляют подробные сведения об активности в сети CDN.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Доступ к расширенным HTTP-отчетам
1. В колонке профиля CDN нажмите кнопку **Управление** .
   
    ![Кнопка управления в колонке профиля CDN](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    Откроется портал управления CDN.
2. Наведите указатель на вкладку **Аналитика**, а затем выберите всплывающий элемент **Расширенные отчеты HTTP**.  Выберите **Большая HTTP-платформа**.
   
    ![Портал управления CDN — меню дополнительных отчетов](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Отобразятся параметры отчета.

## <a name="geography-reports-map-based"></a>Географические отчеты (с использованием карт)
Есть пять отчетов, использующих карты для отображения регионов, из которых запрашивается ваше содержимое. К ним относятся: «Мировая карта», «Карта США», «Карта Канады», «Карта Европы» и «Карта Азиатско-Тихоокеанского региона».

Каждый отчет на основе карты ранжирует географических объектов (т. е. странах и регионах, карту с указанием для наглядного представления расположения, из которых запрашивается ваше содержимое. Регионы на карте отмечаются разными цветами в соответствии с объемом запросов из этого региона. Регионы, окрашенные в светлые тона, характеризуются пониженным потоком запросов, тогда как более темные тона указывают на повышенный интерес к вашему содержимому.

Непосредственно под картой вы можете видеть подробную информацию о трафике и пропускной способности по каждому региону. Благодаря этому вы получаете информацию о переходах для каждого региона в количественном и процентном выражении — отображается общий объем передаваемых данных (в гигабайтах) и процент переданных данных. Также вы можете просмотреть описание каждого из этих показателей. Наконец, при наведении на регион (т. е. страны или региона, область), имя и процент переходов из региона будет отображаться как всплывающая подсказка.

Ниже приводится краткое описание каждого типа географического отчета.

| Имя отчета | Описание |
| --- | --- |
| Карта мира |Этот отчет позволяет просматривать запросы вашего содержимого CDN со всего мира. Каждой страны или региона разными цветами на карте мира, который указывает процент переходов из этого региона. |
| Карта США |Этот отчет позволяет вам просматривать запросы вашего содержимого CDN из США. Каждый штат отмечен своим цветом, который указывает на процент переходов из этого региона. |
| Карта Канады |Этот отчет позволяет вам просматривать запросы вашего содержимого CDN из Канады. Каждая провинция отмечена своим цветом, который указывает на процент переходов из этого региона. |
| Карта Европы |Этот отчет позволяет вам просматривать запросы вашего содержимого CDN из Европы. Каждой страны или региона отмечена своим цветом, который указывает процент переходов из этого региона. |
| Карта Азиатско-Тихоокеанского региона |Этот отчет позволяет вам просматривать запросы вашего содержимого CDN из Азии. Каждой страны или региона отмечена своим цветом, который указывает процент переходов из этого региона. |

## <a name="geography-reports-bar-charts"></a>Географические отчеты (линейчатые диаграммы)
Есть два дополнительных отчета, предоставляющих статистические сведения для географических данных. Это отчеты «Рейтинг городов» и «Рейтинг стран». Эти отчеты дается оценка городам и странам, соответственно, в зависимости от числа переходов из этих стран или регионов. После создания этого отчета, линейчатая диаграмма будет отображать 10 городами или странах и регионах, которые запросили содержимого через определенную платформу. Из этой диаграммы можно быстро получить сведения о регионах с наибольшим количеством запросов вашего содержимого.

В левой части диаграммы (ось y) отображается количество переходов из определенного региона. Непосредственно под диаграммой (ось x) отображаются наименования 10 основных регионов.

### <a name="using-the-bar-charts"></a>Использование линейчатых диаграмм
* При наведении указателя на наименование региона или количество переходов из региона вы увидите всплывающую подсказку.
* Всплывающая подсказка для отчета рейтинг городов идентифицирует городе по его имени, республики и сокращенное название страны или региона.
* Если не удалось определить город или регион (область, республику, край, округ), из которого поступил запрос, то будет указано "Неизвестно". Если страны или региона является unknown, а затем два знака вопроса (??), будет отображаться.
* Отчет может содержать метрики для "Европы" или "Азиатско-Тихоокеанского региона". Обратите внимание, что эти данные нельзя рассматривать как статистическую информацию по всем IP-адресам в этих регионах. Вместо этого они применяются только запросы, поступившие с IP-адресов, распределенных по Европе или Азиатско-Тихоокеанский, а не к конкретному городу или страны или региона.

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают информацию о количестве переходов для основных 250 регионов, а также сведения о проценте переходов, общем объеме передаваемых данных (в гигабайтах) и проценте переданных данных. Также вы можете просмотреть описание каждого из этих показателей.

Ниже приводится краткое описание обоих отчетов.

| Имя отчета | Описание |
| --- | --- |
| Рейтинг городов |Этот отчет оценивает города в зависимости от количества переходов из этого региона. |
| Рейтинг стран |Этот отчет оценивает страны и регионы, в зависимости от числа переходов из этой страны или региона. |

## <a name="daily-summary"></a>Сводка по дням
Отчет «Сводка по дням» позволяет просматривать общее количество переходов и объем переданных данных через определенную платформу на ежедневной основе. Эти сведения можно использовать для быстрого определения шаблонов действий в сети CDN. Например, используя этот отчет, вы можете определить дни с повышенным или пониженным объемом трафика в сравнении с ожидаемым объемом.

После создания этого отчета линейчатая диаграмма предоставляет визуализацию ежедневного объема запросов с конкретной платформы за определенный период времени. Для каждого дня создается новый отрезок. Например, если выбрать период времени «Прошлая неделя», будет создана линейчатая диаграмма с семью отрезками. На каждом отрезке указывается общее количество переходов за определенный день.

В левой части диаграммы (ось y) отображается количество переходов за день. Непосредственно под диаграммой (ось x), вы найдете отображается метка с указанием даты (формат: YYYY-MM-DD) за каждый день, включены в отчет.

> [!TIP]
> При наведении указателя на отрезок вы увидите всплывающую подсказку с указанием количества переходов за соответствующий день.
> 
> 

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают общее количество переходов и объем переданных данных (в гигабайтах) за каждый день отчета.

## <a name="by-hour"></a>Сводка по времени
Отчет «Сводка по времени» позволяет просматривать общее количество переходов и объем переданных данных через определенную платформу по часам. Эти сведения можно использовать для быстрого определения шаблонов действий в сети CDN. Например, используя этот отчет, вы можете определить периоды времени с повышенным или пониженным объемом трафика в сравнении с ожидаемым объемом.

После создания этого отчета линейчатая диаграмма предоставляет визуализацию объема запросов с конкретной платформы с разбивкой по часам за определенный период времени. Для каждого часа создается новый отрезок. Например, если выбрать период «24 часа», будет создана линейчатая диаграмма с двадцатью четырьмя отрезками. На каждом отрезке указывается общее количество переходов за определенный час.

В левой части диаграммы (ось y) отображается количество переходов за час. Непосредственно под диаграммой (ось x), вы найдете отображается метка с указанием даты и времени (формат: YYYY-MM-DD чч: мм) за каждый час, включены в отчет. Время указывается в 24-часовом формате UTC/GMT.

> [!TIP]
> Если вы наведете указатель на отрезок, вы увидите всплывающую подсказку с указанием количества переходов за соответствующий час.
> 
> 

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают общее количество переходов и объем переданных данных (в гигабайтах) за каждый час отчета.

## <a name="by-file"></a>Сводка по файлам
Отчет «Сводка по файлам» позволяет просматривать общее количество переходов и объем переданных данных через определенную платформу для основных ресурсов. После создания этого отчета линейчатая диаграмма будет отображать 10 самых часто запрашиваемых ресурсов за указанный период времени.

> [!NOTE]
> В этом отчете URL-адреса CNAME Edge преобразуются в соответствующие URL-адреса CDN. Это повышает точность определения переходов для ресурса независимо от URL-адреса CDN или CNAME Edge, используемого для доступа к файлу.
> 
> 

В левой части диаграммы (ось y) указывается количество запросов для каждого ресурса за указанный период времени. Непосредственно под диаграммой (ось x) отображается метка с указанием имен 10 самых часто запрашиваемых файлов.

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают следующую информацию о 250 самых часто запрашиваемых файлах: относительный путь, общее количество переходов, процент переходов, общий объем переданных данных (в гигабайтах) и процент переданных данных.

## <a name="by-file-detail"></a>Подробная сводка по файлам
Отчет «Подробная сводка по файлам» позволяет просматривать общее количество переходов и объем переданных данных через определенную платформу для указанных ресурсов. В верхней части отчета можно выбрать файл, для которого нужно получить сведения. В этом поле отображается список самых запрашиваемых ресурсов по выбранной платформе. Чтобы создать подробную сводку по файлам, выберите нужный ресурс в поле «Сведения о файле». После этого отобразится линейчатая диаграмма с указанием ежедневного количества запросов за указанный период времени.

В левой части диаграммы (ось y) указывается общее количество запросов ресурса за определенный день. Непосредственно под диаграммой (ось x), вы найдете отображается метка с указанием даты (формат: YYYY-MM-DD) сети CDN получено поступления запросов к ресурсу.

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают общее количество переходов и объем переданных данных (в гигабайтах) за каждый день отчета.

## <a name="by-file-type"></a>Сводка по типу файлов
Отчет «Сводка по типу файлов» позволяет просматривать количество запросов и объем трафика для определенного типа файлов. После создания этого отчета кольцевой график будет отображать процент переходов для основных 10 типов файлов.

> [!TIP]
> При наведении указателя на срез кольцевого графика отобразится всплывающая подсказка с названием MIME-типа.
> 
> 

Под кольцевым графиком можно увидеть данные, используемые для создания отчета. Они включают следующую информацию о 250 самых часто используемых типах файлов: расширение файла или MIME-тип, общее количество переходов, процент переходов, общий объем переданных данных (в гигабайтах) и процент переданных данных.

## <a name="by-directory"></a>Сводка по каталогам
Отчет «Сводка по каталогам» позволяет просматривать общее количество переходов и объем переданных данных через определенную платформу для указанного каталога. После создания этого отчета линейчатая диаграмма будет отображать общее количество запросов содержимого из 10 каталогов с наибольшим трафиком.

### <a name="using-the-bar-chart"></a>Использование линейчатой диаграммы
* Наведите указатель на отрезок, чтобы увидеть относительный путь к соответствующему каталогу.
* При расчете объема данных по каталогу содержимое из вложенных папок не учитывается. Т.е. результаты расчетов учитывают только файлы, хранящиеся непосредственно в указанном каталоге.
* В этом отчете URL-адреса CNAME Edge преобразуются в соответствующие URL-адреса CDN. Это повышает точность всей статистики для ресурса независимо от URL-адреса CDN или CNAME Edge, используемого для доступа к файлу.

В левой части диаграммы (ось y) указывается общее количество запросов содержимого, хранящегося в 10 каталогах с наибольшим трафиком. Каждый отрезок на диаграмме соответствует одному каталогу. Для поиска определенного каталога из полного списка 250 наиболее используемых каталогов руководствуйтесь схемой цветовых оттенков.

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают следующую информацию о 250 самых часто используемых каталогах: относительный путь, общее количество переходов, процент переходов, общий объем переданных данных (в гигабайтах) и процент переданных данных.

## <a name="by-browser"></a>Сводка по браузерам
Отчет «Сводка по браузерам» позволяет узнать, какие браузеры использовались при просмотре содержимого. После создания этого отчета круговая диаграмма будет отображать процент переходов для 10 основных браузеров.

### <a name="using-the-pie-chart"></a>Использование круговой диаграммы
* Наведите указатель мыши на срез круговой диаграммы, чтобы увидеть название и версию браузера.
* В этом отчете каждая комбинация браузера и его версии считается отдельным браузером.
* Срез с названием «Другие» показывает процент запросов, обработанных другими браузерами или другими версиями браузеров.

Под круговой диаграммой можно увидеть данные, используемые для создания отчета. Они включают тип и версию браузера, общее количество переходов и процент переходов для 250 самых популярных браузеров.

## <a name="by-referrer"></a>Сводка по источникам ссылок
Отчет «Сводка по источникам ссылок» позволяет просматривать самые популярные источники ссылок на содержимое для выбранной платформы. Источник ссылок — это имя узла, который создал запрос. После создания этого отчета линейчатая диаграмма будет отображать количество переходов для 10 основных источников ссылок.

В левой части диаграммы (ось y) указывается общее количество запросов ресурса для каждого источника ссылок. Каждый отрезок на диаграмме соответствует одному источнику ссылок. Для поиска определенного источника ссылок в полном списке 250 основных источников ссылок руководствуйтесь схемой цветовых оттенков.

Под линейчатой диаграммой можно увидеть данные, используемые для создания отчета. Они включают URL-адрес, общее количество переходов и процент переходов для 250 основных источников ссылок.

## <a name="by-download"></a>Сводка по загрузкам
Отчет «Сводка по загрузкам» позволяет анализировать шаблоны загрузки вашего самого популярного содержимого. В верхней части отчета представлена линейчатая диаграмма, на которой сравниваются попытки загрузки и завершенные загрузки для 10 самых востребованных ресурсов. В зависимости от типа загрузки отрезки имеют разные цвета. Попытки загрузки обозначаются синим, а завершенные загрузки — зеленым цветом.

> [!NOTE]
> В этом отчете URL-адреса CNAME Edge преобразуются в соответствующие URL-адреса CDN. Это повышает точность всей статистики для ресурса независимо от URL-адреса CDN или CNAME Edge, используемого для доступа к файлу.
> 
> 

В левой части диаграммы (ось y) указывается имя файла из списка 10 основных ресурсов. Непосредственно под диаграммой (ось x) приводятся метки с указанием общего количества попыток загрузки и завершенных загрузок.

Непосредственно под диаграммой приводятся следующие сведения для 250 основных ресурсов: относительный путь (включая имя файла), количество завершенных загрузок, количество запросов загрузки и процент запросов, завершившихся успешной загрузкой.

> [!TIP]
> Наша сеть CDN не получает от HTTP-клиентов (т. е. браузеров) информацию о полном завершении загрузки. Поэтому оценка полной загрузки ресурса выполняется согласно кодам состояния и запросам диапазона байтов. При оценке мы в первую очередь проверяем код состояния. Должен быть возвращен код «200 OK». Если этот код возвращен, мы обращаемся к запросам диапазонов байтов и убеждаемся, что они охватывают весь объем ресурса. В конце мы сравниваем объем переданных данных с размером ресурса. Если объем переданных данных равен размеру файла или превышает его, а запросы диапазонов байтов соответствуют этому ресурсу, то переход считается завершенной загрузкой.
> 
> Так как этот отчет является интерпретационным, вы должны учитывать следующие моменты, которые могут влиять на целостность и точность этого отчета.
> 
> * Из-за разного поведения пользовательских клиентов невозможно точно предсказать шаблоны потока трафика. В результате для завершенной загрузки можно получить значение, которое превышает 100 %.
> * Если ресурс загружался через HTTP, этот отчет не сможет точно отобразить состояние загрузки. Это связано с тем, что пользователи могут в любой момент перематывать видео.
> 
> 

## <a name="by-404-errors"></a>Сводка по ошибкам 404
Отчет «Сводка по ошибкам 404» позволяет определить тип содержимого, который чаще всего приводит к возникновению ошибки «404 не найдено». В верхней части отчета находится линейчатая диаграмма с 10 ресурсами, для которых чаще всего возвращается код ошибки «404 не найдено». На этой линейчатой диаграмме сравнивается общее количество запросов с количеством запросов, вернувших ошибку «404 не найдено» для этих ресурсов. Каждый отрезок имеет свой цвет. Желтый цвет указывает, что была возвращена ошибка «404 не найдено». Красный отрезок указывает общее количество запросов для этого ресурса.

> [!NOTE]
> Замечания к этому отчету.
> 
> * Переход означает любой запрос ресурса, независимо от кода состояния.
> * URL-адреса CNAME Edge преобразуются в соответствующие URL-адреса CDN. Это повышает точность всей статистики для ресурса независимо от URL-адреса CDN или CNAME Edge, используемого для доступа к файлу.
> 
> 

В левой части диаграммы (ось y) указывается имя файла из списка 10 основных ресурсов, которые чаще всего приводят к ошибке «404 не найдено». Непосредственно под диаграммой (ось x) приведены метки с указанием общего количества запросов и количества запросов, вернувших ошибку «404 не найдено».

Непосредственно под диаграммой приводятся следующие сведения для 250 основных ресурсов: относительный путь (включая имя файла), количество запросов, завершившихся ошибкой «404 не найдено», количество запросов ресурса и процент запросов, завершившихся ошибкой «404 не найдено».

## <a name="see-also"></a>См. также
* [Обзор Azure CDN](cdn-overview.md)
* [Статистика сети CDN Microsoft Azure в режиме реального времени](cdn-real-time-stats.md)
* [Переопределение стандартного поведения через HTTP с помощью обработчика правил](cdn-rules-engine.md)
* [Анализ производительности Edge](cdn-edge-performance.md)

