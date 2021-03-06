---
title: Обзор рабочих нагрузок Red Hat в Azure | Документация Майкрософт
description: Сведения о предложениях продуктов Red Hat, доступных в Azure
services: virtual-machines-linux
author: asinn826
manager: borisb2015
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 8ca249a5f6c300a39548e4e16927d7a20acae1a8
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942622"
---
# <a name="red-hat-workloads-on-azure"></a>Рабочие нагрузки Red Hat в Azure
Для поддержки рабочих нагрузок Red Hat в Azure предоставляется широкий набор предложений. В основе рабочих нагрузок RHEL лежат образы Red Hat Enterprise Linux (RHEL) и инфраструктура обновления Red Hat (RHUI).

## <a name="red-hat-enterprise-linux-rhel-images"></a>Образы Red Hat Enterprise Linux (RHEL)
Azure предоставляет широкий ассортимент образов RHEL. Их можно использовать с двумя разными моделям лицензирования: с оплатой по мере использования (PAYG) или с собственной подпиской (BYOS). Новые образы RHEL публикуются в Azure по мере необходимости, когда появляются и обновляются версии RHEL в рамках естественного жизненного цикла.

### <a name="pay-as-you-go-payg-images"></a>Образы с оплатой по мере использования (PAYG)
Azure предлагает разные образы RHEL PAYG. Эти образы предоставляют полное право на использование RHEL и подключены к источнику обновлений (инфраструктура обновления Red Hat). Эти образы предполагают дополнительную оплату за право использования и обновление RHEL. Среди прочего, доступны следующие варианты образа RHEL PAYG:
* RHEL категории "Стандартный";
* RHEL для SAP;
* RHEL для SAP с высоким уровнем доступности и службами обновления.

Вы можете использовать образы PAYG, если вы не хотите утруждаться раздельной оплатой нужного количества подписок.

### <a name="red-hat-gold-images-rhel-byos"></a>Образы Red Hat Gold (`rhel-byos`)
Azure также предлагает образы Red Hat Gold. Эти образы полезны тем клиентам, у которых уже есть подписки Red Hat и которые хотят использовать их в Azure. Чтобы использовать существующие подписки Red Hat для доступа к облаку Red Hat, их необходимо включить в Azure. Доступ к этим образам предоставляется автоматически, если подписки Red Hat включены для доступа к облаку и соответствуют требованиям доступа. Использование этих образов позволяет клиенту избежать двойной оплаты, которая может происходить при использовании образов PAYG.
* [Chapter 2. Enabling and maintaining subscriptions for Cloud Access](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs) (Глава 2. Включение и обслуживание подписок на "Доступ к облаку")
* [Red Hat Enterprise Linux Bring-Your-Own-Subscription Gold Images in Azure](./byos.md) (Образы Gold подписки "Использование собственных ресурсов" для Red Hat Enterprise Linux)

> [!NOTE]
> Примечание о двойной оплате. Двойной оплатой называется ситуация, когда пользователь дважды оплачивает подписки RHEL. Обычно такое происходит, когда клиент подключает права к виртуальной машине RHEL PAYG с помощью subscription-manager. Например, если клиент применит subscription-manager для подключения прав на пакеты SAP к образу RHEL PAYG, с него будет косвенно взиматься двойная оплата за RHEL — как дополнительная плата за PAYG и как оплата подписки SAP. Этого не происходит при использовании образов BYOS.

## <a name="red-hat-update-infrastructure-rhui"></a>Инфраструктура обновления Red Hat (RHUI)
Azure предоставляет инфраструктуру обновления Red Hat только для виртуальных машин PAYG RHEL. RHUI по сути является зеркалом сетей доставки содержимого Red Hat, которое доступно только для виртуальных машин PAYG RHEL в Azure. Вы получите доступ к соответствующим пакетам в зависимости от того, какой образ RHEL вы развернули. Например, образ RHEL для SAP получает доступ к пакетам SAP в дополнение к базовым пакетам RHEL.

### <a name="rhui-update-behavior"></a>Алгоритм обновления RHUI
Образы RHEL, подключенные к RHUI, по умолчанию обновляются до последней дополнительной версии RHEL при каждом запуске `yum update`. Такое поведение означает, что виртуальная машина RHEL 7.4 может быть обновлена до RHEL 7.7, когда на ней выполняется операция `yum update`. Это нормальное поведение RHUI. Однако вы можете этого избежать, переключив обновление с обычных репозиториев RHEL на [репозитории поддержки расширенных обновлений (Extended Update Support)](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms).

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об [образах RHEL в Azure](./redhat-images.md)
* Дополнительные сведения об [инфраструктуре обновления Red Hat в Azure](./redhat-rhui.md)
* Дополнительные сведения о предложении [Образ Red Hat Gold (`rhel-byos`)](./byos.md)
