---
title: Поддерживаемые стили карт | Карты Microsoft Azure
description: В этой статье вы узнаете о различных стилях рендеринга карт, поддерживаемых картами Microsoft Azure.
author: farah-alyasari
ms.author: v-faalya
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 9cdfd0d029057e36e010203b7c35a5aafee4b574
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77208290"
---
# <a name="azure-maps-supported-map-styles"></a>Стили карт, поддерживаемые в службе Azure Maps
Служба Azure Maps поддерживает несколько различных встроенных стилей карт, как описано ниже.

## <a name="road"></a>road
Карта **дороги** — это стандартная карта, которая отображает дороги, природные и искусственные объекты, а также ярлыки для этих объектов.

![стиль схемы дороги](./media/supported-map-styles/road.png)

**Применимые API:**
* [Изображение карты](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Фрагмент карты](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android

## <a name="blank-and-blank_accessible"></a>пусто и blank_accessible

**Пустые** и **blank_accessible** стили карт предоставляют пустой холст для визуализации данных. Стиль **blank_accessible** будет по-прежнему предоставлять обновления средства чтения с экрана сведениями о расположении, даже если базовая схема не отображается.

> [!Note]
> В веб-пакете SDK можно изменить цвет фона на карте, установив стиль CSS `background-color` элемента Map DIV.

**Применимые API:**
* Элемент управления картой веб-пакета SDK

## <a name="satellite"></a>спутник 
Стиль **спутник** представляет собой комбинацию спутниковых и аэроснимков.

![стиль карты вспомогательных плиток](./media/supported-map-styles/satellite.png)

**Применимые API:**
* [Фрагмент спутниковой карты](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android

## <a name="satellite_road_labels"></a>satellite_road_labels
Этот стиль карты представляет собой гибрид дорог и ярлыков, наложенных поверх спутниковых и аэроснимков.

![satellite_road_labels стиль схемы](./media/supported-map-styles/satellite_road_labels.png)

**Применимые API:**
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android

## <a name="grayscale_dark"></a>grayscale_dark
**Темно-серый** — это темная версия стиля дорожной карты.

![gray_scale стиль схемы](./media/supported-map-styles/grayscale_dark.png)

**Применимые API:**
* [Изображение карты](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Фрагмент карты](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Элемент управления картой веб-пакета SDK 
* Элемент управления картой Android


## <a name="grayscale_light"></a>grayscale_light
**источник оттенков серого** — это небольшая версия стиля схемы дороги.

![стиль схемы оттенков серого](./media/supported-map-styles/grayscale_light.png)

**Применимые API:**
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android


## <a name="night"></a>ночь
**Ночь** — это темная версия стиля дорожной карты с цветными дорогами и символами.

![стиль карт ночью](./media/supported-map-styles/night.png)

**Применимые API:**
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android

## <a name="road_shaded_relief"></a>road_shaded_relief
**road shaded relief** — это основной стиль Azure Maps, дополненный рельефами Земли.

![Затененный стиль карт рельефов](./media/supported-map-styles/shaded-relief.png)

**Применимые API:**
* [Фрагмент карты](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Элемент управления картой веб-пакета SDK
* Элемент управления картой Android


## <a name="next-steps"></a>Следующие шаги

Узнайте, как задать стиль схемы в Azure Maps.

> [!div class="nextstepaction"]
> [Выбор стиля карты](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
