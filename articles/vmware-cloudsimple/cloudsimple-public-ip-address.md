---
title: Решения Azure VMware (AVS) — общедоступный IP-адрес
description: Сведения об общедоступных IP-адресах и их преимуществах в решениях Azure VMware (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2cb9d0e33da4447760ae0be216c1dd9868c498bd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024982"
---
# <a name="avs-public-ip-address-overview"></a>Общие сведения о общедоступном IP-адресе AVS

Общедоступный IP-адрес позволяет ресурсам Интернета обмениваться данными между ресурсами частного облака AVS по частному IP-адресу. Частный IP-адрес — это виртуальная машина или программная подсистема балансировки нагрузки в частном облаке "AVS". Общедоступный IP-адрес позволяет предоставлять службы, работающие в частном облаке AVS, в Интернете.

Общедоступный IP-адрес выделяется частным IP-адресом, пока он не будет назначен. Общедоступный IP-адрес может быть назначен только одному частному IP-адресу.

Ресурс, связанный с общедоступным IP-адресом, всегда использует общедоступный IP-адрес для доступа к Интернету. По умолчанию в общедоступном IP-адресе разрешен только исходящий доступ к Интернету.  Для входящего трафика на общедоступном IP-адресе отказано.  Чтобы разрешить входящий трафик, создайте правило брандмауэра для общедоступного IP-адреса для конкретного порта.

## <a name="benefits"></a>Преимущества

Использование общедоступного IP-адреса для обмена данными обеспечивает:

* Предотвращение атак типа "отказ в обслуживании" (от атак DDoS). Эта защита автоматически включается для общедоступного IP-адреса.
* Постоянный мониторинг трафика и устранение распространенных атак на уровне сети в режиме реального времени. Эти механизмы защиты используются корпорацией Майкрософт веб-службы.
* Весь масштаб глобальной сети Azure. Сеть может использоваться для распространения и смягчения трафика атак в разных регионах.  

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о [выделении общедоступного IP-адреса](public-ips.md)