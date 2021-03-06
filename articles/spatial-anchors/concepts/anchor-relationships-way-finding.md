---
title: Связи привязки и способ поиска
description: Сведения об концептуальной модели отношений привязки. Узнайте, как подключать привязки в пространстве и использовать ближайший API для выполнения сценария поиска.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: f2fd8f4b7d03be8822c3ec12e2be589054942ce3
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2019
ms.locfileid: "74270600"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Связи привязки и способ поиска в пространственных привязках Azure

С помощью связей привязки можно создавать подключенные привязки в пробелом, а затем задавать такие вопросы:

* Находятся ли привязки рядом?
* На каком расстоянии?

## <a name="examples"></a>Примеры

В таких случаях можно использовать подключенные привязки, например:

* Работнику необходимо выполнить задачу, включающую посещение различных расположений в промышленных фабриках. Фабрика имеет пространственные привязки в каждом расположении. С помощью HoloLens или мобильного приложения можно переадресовать рабочую роль от одного места к другому. Сначала приложение запрашивает соседние пространственные привязки, а затем направляет рабочую роль в следующее расположение. Приложение визуально отображает общее направление и расстояние в следующем расположении.

* Музей создает пространственные привязки на общедоступных дисплеях. Вместе эти привязки формируют одночасовый обзор основных общедоступных экранов музей. На общедоступном экране Посетители могут открыть приложение смешанной реальности музей на своем мобильном устройстве. Затем они указывают на свою телефонную камеру вокруг места, чтобы увидеть общее направление и расстояние на другом общедоступном экране. По мере того как пользователь переходит к общедоступному дисплею, приложение обновляет общее направление и расстояние для помощи пользователю.

## <a name="set-up-way-finding"></a>Настройка способа поиска

Приложение, использующее направление и расстояние между привязками для предоставления рекомендаций, заключается в использовании *способа поиска*. Способ поиска отличается от поворачивания навигации. При включении навигации пользователи проходят через стены, двери и между этажами. С помощью способа поиска пользователь получает подсказки о общем направлении назначения. Но вывод или знание пространства также помогает пользователю перемещаться по структуре в место назначения.

Чтобы создать способ поиска, сначала Подготовьте пространство для удобства и разработайте приложение, с которым пользователи будут взаимодействовать. Ниже приведены основные шаги.

1. **Планирование пространства**: Определите, какие расположения в пространстве будут входить в процесс поиска. В наших сценариях Супервизор фабрики или биообъектный координатор может решить, какие расположения следует включить в процесс поиска.
2. **Привязки соединения**: для создания пространственных привязок посетите выбранные расположения. Это можно сделать в режиме администрирования приложения конечного пользователя или в другом приложении полностью. Вы будете подключаться к другим привязкам или связывать их с другими. Служба поддерживает эти связи.
3. **Начните работу с конечным пользователем**. пользователи запускают приложение для нахождение привязки, которая может находиться в любом из выбранных расположений. Общая структура должна определять местоположения, в которых пользователи могут вводить опыт.
4. **Поиск ближайших привязок**: после того, как пользователь найдет привязку, приложение может запросить соседние привязки. Эта процедура возвращает объект-фрагмент между устройством и этими привязками.
5. **Руководство пользователя**. приложение может использовать объект a для каждой из этих привязок, чтобы предоставить рекомендации по общему направлению и расстоянию пользователя. Например, канал камеры в приложении может отображать значок и стрелку для представления каждого потенциального назначения, как показано на следующем рисунке.
6. **Уточнение рекомендаций**. по мере того как пользователь проходит проверку, приложение может периодически вычислить новое значение между устройством и точкой привязки. Приложение продолжит уточнять указания руководств, помогающие пользователю приступить к назначению.

    ![Пример того, как приложение может продемонстрировать способ поиска руководства](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Соединить привязки

Чтобы создать способ поиска, необходимо сначала поместить привязки в выбранные расположения. В этом разделе мы предполагаем, что администратор приложения уже завершил эту работу.

### <a name="connect-anchors-in-a-single-session"></a>Подключение привязок в одном сеансе

Для соединения с привязками:

1. Перейдите к первому расположению и создайте привязку A с помощью Клаудспатиаланчорсессион.
2. Пошаговое руководство по второму расположению. Базовая платформа MR/AR отслеживает перемещение.
3. Создайте привязку B с помощью того же Клаудспатиаланчорсессион. Привязки A и B теперь подключены. Служба пространственных привязок поддерживает эту связь.
4. Продолжите процедуру для оставшихся привязок.

### <a name="connect-anchors-in-multiple-sessions"></a>Подключение привязок в нескольких сеансах

Можно подключать пространственные привязки к нескольким сеансам. С помощью этого метода можно создать и подключить несколько привязок за один раз, а затем создать и подключить дополнительные привязки.

Чтобы подключить привязки к нескольким сеансам:

1. Приложение создает несколько привязок в одном Клаудспатиаланчорсессион.
2. В другое время приложение находит одну из этих привязок (например, привязку A) с помощью нового Клаудспатиаланчорсессион.
3. Пошаговое руководство по новому расположению. Базовая платформа смешанной реальности или Расширенная реальность отслеживает перемещение.
4. Создайте привязку C с помощью того же Клаудспатиаланчорсессион. Привязки A, B и C теперь подключены. Служба пространственных привязок поддерживает эту связь.

Вы можете продолжить эту процедуру для большего количества привязок и других сеансов с течением времени.

### <a name="verify-anchor-connections"></a>Проверка подключений привязки

Приложение может проверить соединение двух привязок, выполнив запрос для ближайших привязок. Если результат запроса содержит целевую привязку, подключение привязки проверяется. Если привязки не подключены, приложение может повторить попытку подключения.

Ниже приведены некоторые причины, по которым привязки могут не подключаться:

* Базовая платформа смешанной реальности или Расширенная реальность потеряла отслеживание во время процесса соединения привязок.
* Из-за сетевой ошибки во время связи со службой пространственных привязок не удалось сохранить подключение привязки.

### <a name="find-sample-code"></a>Поиск примера кода

Чтобы найти пример кода, демонстрирующий подключение к привязкам и выполнение ближайших запросов, см. раздел [примеры приложений для пространственных привязок](https://github.com/Azure/azure-spatial-anchors-samples).
