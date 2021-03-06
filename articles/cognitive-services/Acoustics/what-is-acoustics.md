---
title: Общие сведения о Project Acoustics
titlesuffix: Azure Cognitive Services
description: Project Acoustics — это акустическая подсистема для трехмерного интерактивного интерфейса, объединяющая физику смоделированных волн с интерактивными элементами управления для проектирования.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 65678f08399f378b8580eed79e49197dd4d84c64
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351150"
---
# <a name="what-is-project-acoustics"></a>Что такое Project Acoustics?
Project Acoustics — это подсистема волновой акустики для трехмерных интерактивных интерфейсов. Она моделирует волновые эффекты (окклюзия, помехи, перемещение и реверберация) в сложных сценах, не требуя выполнения разметки зон вручную или трассировки лучей с использованием большого количества ресурсов ЦП. Кроме того, она содержит игровой модуль и интеграцию аудио ПО промежуточного слоя. Философия Project Acoustics схожа со статическим освещением: моделирование детализированной физики в автономном режиме с целью обеспечения физической основы, а также использование упрощенной среды выполнения с выразительными элементами управления проектированием с целью реализации художественных целей для акустики или виртуального мира.

![Снимок экрана из Gears of War 4 с отображением объемных пикселей Project Acoustics](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Использование волновой физики для интерактивной акустики
Методы акустики на основе лучей позволяют проверять наличие окклюзии с помощью "бросания лучей" от источника к слушателю или управлять реверберацией, оценивая громкость звука в локальной сцене с несколькими лучами. Однако эти методы могут быть ненадежными, так как маленький камешек создает такой же эффект окклюзии, что и булыжник. Лучи не учитывают то, что звук огибает объекты (явление дифракции). Проектирование Project Acoustics фиксирует эти эффекты с помощью проектирования волн. Благодаря этому акустическая модель будет более предсказуемой, точной и отлаженной.

Ключевой инновацией Project Acoustics является соединение акустического проектирования на основе реальных звуковых волн с традиционными принципами проектирования звука. Это решение преобразует результаты проектирования в традиционные параметры аудио DSP для окклюзии, перемещения и реверберации. В этом процессе преобразования конструктор использует элементы управления. Дополнительные сведения об основных технологиях Project Acoustics см. на [странице исследовательского проекта](https://www.microsoft.com/en-us/research/project/project-triton/).

![Анимация, показывающая горизонтальный 2D-срез распространение волны в сцене](media/wave-simulation.gif)

## <a name="video-presentation-from-gdc-2019-30-min"></a>Видеопрезентация с конференции GDC 2019 (приблизительно 30 мин)
[![Видео о Project Acoustics](https://img.youtube.com/vi/uY4G-GUAQIE/0.jpg)](https://www.youtube.com/watch?v=uY4G-GUAQIE "Щелкните, чтобы воспроизвести видео")

## <a name="setup"></a>Настройка
[Интеграция Project Acoustics с Unity](unity-integration.md) осуществляется путем перетаскивания и включает в себя подключаемый аудиомодуль Unity. Дополните элементы управления аудио Unity, прикрепив компонент элементов управления Project Acoustics C# к каждому звуковому объекту.

[Интеграция Project Acoustics с Unreal](unreal-integration.md) включает в себя подключаемые модули редактора и игровые подключаемые модули для Unreal, а также подключаемый модуль микшера Wwise. Настраиваемый аудиокомпонент расширяет привычную функциональность Wwise в Unreal за счет элементов управления для интерактивного проектирования акустики. Элементы управления для проектирования также отображаются в Wwise в подключаемом модуле микшера.

## <a name="workflow"></a>Рабочий процесс
* **Предварительное моделирование.** Начните с настройки моделирования, выбрав геометрию для ответа на акустику, например за счет игнорирования световых лучей. Затем отредактируйте автоматические назначения материалов и выберите области навигации, чтобы управлять выборкой слушателя. Поддержка разметки для реверберации, перемещения и зон вручную отсутствует.
* **Моделирование.** Этап анализа запускается локально. Выполняется вокселизация и другие типы геометрического анализа сцены на основе указанных выше вариантов. Результаты визуализируются в редакторе для проверки настройки сцены. При отправке моделирования данные об объемных пикселях отправляются в Azure, а вы получаете игровой акустический ресурс.
* **Среда выполнения.** Загрузите ресурс на свой уровень. Теперь вы сможете прослушать акустику на этом уровне. Проектируйте акустику в интерактивном режиме в редакторе, используя детальные элементы управления для каждого источника. Получить элементы управления можно из сценариев уровня.

## <a name="runtime-platforms"></a>Платформы среды выполнения
Подключаемые модули среды выполнения Project Acoustics в настоящее время можно развернуть на следующих платформах:
* Windows
* MacOS
* Android
* Xbox One

## <a name="editor-platforms"></a>Платформы редактора
Подключаемый модуль редактора Project Acoustics доступен для следующих платформ:
* Windows
* MacOS (только Unity)

## <a name="download"></a>Скачивание
* [Подключаемый модуль Project Acoustics Unity с примерами](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Подключаемые модули Project Acoustics Unreal и Wwise с примерами](https://www.microsoft.com/download/details.aspx?id=58090)
  * Чтобы получить сведения о двоичных файлах Xbox или другую поддержку, свяжитесь с нами на [форуме](https://github.com/microsoft/ProjectAcoustics/issues).

## <a name="contact-us"></a>Свяжитесь с нами
* [Обсуждение Project Acoustics и отчеты по проблемам](https://github.com/microsoft/ProjectAcoustics/issues)

## <a name="next-steps"></a>Дополнительная информация
* Ознакомьтесь с [кратким руководством по Project Acoustics для Unity](unity-quickstart.md) или [Unreal](unreal-quickstart.md)
* Изучите [философию проектирования звука Project Acoustics](design-process.md)

