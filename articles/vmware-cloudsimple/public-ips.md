---
title: Решения Azure VMware (AVS) — выделение общедоступных IP-адресов
description: В этой статье описывается, как выделить общедоступные IP-адреса для виртуальных машин в среде частного облака AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 87133f5efb9f096d3fdb0956aab1caac58b4bd94
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024302"
---
# <a name="allocate-public-ip-addresses-for-avs-private-cloud-environment"></a>Выделение общедоступных IP-адресов для среды частного облака AVS

Откройте вкладку общедоступные IP-адреса на странице "сеть", чтобы выделить общедоступные для виртуальных машин виртуальные машины в среде частного облака AVS.

1. [Откройте портал AVS](access-cloudsimple-portal.md) и выберите **сеть** в боковом меню.
2. Выберите **общедоступные IP-адреса**.
3. Щелкните **новый общедоступный IP-адрес**.

    ![Страница общедоступных IP-адресов](media/public-ips-page.png)

4. Введите имя для указания записи IP-адреса.
5. Используйте расположение по умолчанию.
6. При необходимости используйте ползунок для изменения времени ожидания простоя.
7. Введите локальный IP-адрес, для которого требуется назначить общедоступный IP-адрес.
8. Введите связанное DNS-имя.
9. Щелкните **Отправить**.

![Выделение общедоступных IP-адресов](media/network-public-ip-allocate.png)

Начинается задача выделения общедоступного IP-адреса. Состояние задачи можно проверить на странице **действия > задачи** . После завершения выделения новая запись отображается на странице общедоступные IP-адреса.
