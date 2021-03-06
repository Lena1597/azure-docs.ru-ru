---
title: включить файл
description: включить файл
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 10/25/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: fca8c3b0b1bd4d22720a6d15313e297d05b7fac9
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021253"
---
1. Откройте портал Azure ([https://portal.azure.com](https://portal.azure.com)).

1. Нажмите кнопку **Создать ресурс**.

    ![Создайте ресурс](./media/functions-create-function-app-portal/function-app-create-resource.png)

1. Щелкните **Вычислительные ресурсы** > **Приложение-функция**.

    ![Создание приложения-функции на портале Azure](./media/functions-premium-create/function-app-create-start.png)

1. Используйте настройки приложения-функции, указанные в таблице под рисунком.

    ![Основы](./media/functions-premium-create/function-app-create-basics.png)

    | Параметр      | Рекомендуемое значение  | Description |
    | ------------ | ---------------- | ----------- |
    | **подписка** | Ваша подписка | Подписка, в которой создано приложение-функция. |
    | **[Группа ресурсов](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Имя новой группы ресурсов, в которой создается приложение-функция. |
    | **Имя приложения-функции** | Глобально уникальное имя | Имя, которое идентифицирует ваше новое приложение-функцию. Допустимые символы: `a-z` (без учета регистра), `0-9`и `-`.  |
    |**Опубликовать**| Код | Параметр для публикации файлов кода или контейнера Docker. |
    | **Стек среды выполнения** | Предпочитаемый язык | Выберите среду выполнения, которая поддерживает нужный функциональный язык программирования. Выберите **.NET** для функций C# и F#. |
    |**Регион**| Предпочтительный регион | Выберите ближайший [регион](https://azure.microsoft.com/regions/) или регион рядом с другими службами, к которому получают доступ ваши функции. |

    Нажмите кнопку **Далее: размещение >** .

1. Введите следующие параметры размещения.

    ![Размещение](./media/functions-premium-create/function-app-premium-create-hosting.png)

    | Параметр      | Рекомендуемое значение  | Description |
    | ------------ | ---------------- | ----------- |
    | **[Учетная запись хранения](../articles/storage/common/storage-account-create.md)** |  Глобально уникальное имя |  Создайте учетную запись хранения для использования приложением-функцией. Имя учетной записи хранения должно содержать от 3 до 24 символов и состоять только из цифр и строчных букв. Можно также использовать существующую учетную запись при условии, что она соответствует [требованиям учетной записи хранилища](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    |**Операционная система**| Предпочтительная операционная система | Операционная система предварительно выбирается с учетом выбранного стека среды выполнения, но при необходимости ее можно изменить. |
    | **[План](../articles/azure-functions/functions-scale.md)** | "Премиум" | В поле Тип плана выберите **Premium (Предварительная версия)** и выберите значения по умолчанию для параметров *план Windows* и *SKU и размер* . |

    Нажмите кнопку **Далее: мониторинг >** .

1. Введите следующие параметры мониторинга.

    ![Мониторинг](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

    | Параметр      | Рекомендуемое значение  | Description |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | По умолчанию | Создает ресурс Application Insights с таким же *именем приложения* в ближайшем поддерживаемом регионе. Развернув этот параметр, можно задать **новое имя ресурса** или выбрать другое **расположение** в [географическом регионе Azure](https://azure.microsoft.com/global-infrastructure/geographies/), где будут хранится данные. |

    Выберите **Просмотр и создание** , чтобы просмотреть выбранные параметры конфигурации приложения.

1. Выберите **Создать**, чтобы подготовить и развернуть приложение-функцию.