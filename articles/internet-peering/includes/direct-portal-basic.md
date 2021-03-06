---
title: включить файл
titleSuffix: Azure
description: включить файл
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 79cf4b2edd1a25df427cf15f9ee7f2f331ebd2df
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774644"
---
1. Щелкните **создать ресурс** > **Просмотреть все**.

    > [!div class="mx-imgBorder"]
    > ![](../media/setup-seeall.png) пиринга поиска

1. Найдите *пиринг* в поле поиска и нажмите клавишу *Ввод* на клавиатуре. В результатах щелкните ресурс **пиринга** .

    > [!div class="mx-imgBorder"]
    > ![запустить пиринг](../media/setup-launch.png)

1. После запуска **пиринга** изучите страницу, чтобы узнать подробности. Когда все будет готово, нажмите кнопку **создать**.

    > [!div class="mx-imgBorder"]
    > ![создания пиринга](../media/setup-create.png)

1. На странице **Создание пиринга** на вкладке **Основные сведения** заполните поля, как показано ниже.

    > [!div class="mx-imgBorder"]
    > Основы пиринга ![](../media/setup-basics-tab.png)

    * Выберите **подписку**Azure.
    * Для **группы ресурсов**можно выбрать имеющуюся группу ресурсов из раскрывающегося списка или создать новую группу, нажав кнопку **создать**. Для этого примера мы создадим новую группу ресурсов.
    * **Имя** соответствует имени ресурса и может быть любым из выбранных.
    * **Регион** выбирается в том случае, если выбрана существующая группа ресурсов на предыдущем шаге. Если вы решили создать новую группу ресурсов, необходимо также выбрать регион Azure, в котором будет размещаться ресурс. Восточная часть США

        > [!NOTE]
        > Регион, в котором находится группа ресурсов, не зависит от расположения, в котором вы хотите создать пиринг с корпорацией Майкрософт. Но рекомендуется упорядочить ресурсы пиринга в группах ресурсов, которые находятся в ближайших регионах Azure. Пример: для пиринга в Эшберн можно создать группу ресурсов в *восточной части США* или *Восточной США 2*

    * Выберите значение ASN в поле **Peer ASN** .

        > [!IMPORTANT]
        > * Вы можете выбрать ASN с Валидатионстате как "утвержденный" перед отправкой запроса пиринга. Если вы только что отправили запрос PeerAsn, подождите 12 часов или так, чтобы сопоставление ASN было "утверждено". Если выбран параметр ASN, ожидающий проверки, появится сообщение об ошибке. 
        > * Если вы не видите номер ASN, который нужно выбрать, проверьте, выбрана ли правильная подписка. Если это так, убедитесь, что вы уже создали PeerAsn с помощью [связывания однорангового узла ASN с подпиской Azure](../howto-subscription-association-portal.md).

        > [!div class="mx-imgBorder"]
        > Основные сведения о ![пиринга заполнены](../media/setup-direct-basics-filled-tab.png)

    * Чтобы продолжить, щелкните " **Далее: > конфигурации** ".
