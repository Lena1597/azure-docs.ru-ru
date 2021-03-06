---
title: Подключение к виртуальному рабочему столу Windows из iOS — Azure
description: Как подключиться к виртуальному рабочему столу Windows с помощью клиента iOS.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/08/2020
ms.author: helohr
ms.openlocfilehash: aba2202f0d33609400588e379a4ed3bb9bb798d9
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367531"
---
# <a name="connect-with-the-ios-client"></a>Подключение с помощью клиента iOS

> Применимо к: iOS 13,0 или более поздней версии. Совместимо с iPhone, iPad и iPod Touch.

Доступ к ресурсам виртуальных рабочих столов Windows можно получить с вашего устройства iOS с помощью скачиваемого клиента. В этом руководство вы узнаете, как настроить клиент iOS.

## <a name="install-the-ios-client"></a>Установка клиента iOS

Чтобы приступить к работе, [Скачайте](https://aka.ms/rdios) и установите клиент на устройстве iOS.

## <a name="subscribe-to-a-feed"></a>Подпишитесь на веб-канал

Подпишитесь на веб-канал, предоставленный администратором, чтобы получить список управляемых ресурсов, к которым можно получить доступ на устройстве iOS.

Для подписки на веб-канал:

1. В центре подключений выберите **+** , а затем выберите **Добавить рабочую область**.
2. Введите URL-адрес канала в поле **URL-адрес канала** . URL-адрес канала может быть либо URL-адресом, либо адресом электронной почты.
   - Если вы используете URL-адрес, воспользуйтесь одним из предоставленных администратором. Обычно URL-адрес <https://rdweb.wvd.microsoft.com>.
   - Чтобы использовать электронную почту, введите свой адрес электронной почты. Это указывает клиенту на необходимость поиска URL-адреса, связанного с адресом электронной почты, если администратор настроил сервер таким образом.
3. Нажмите кнопку **Далее**.
4. При появлении запроса предоставьте свои учетные данные.
   - В поле **имя пользователя**предоставьте имя пользователя с разрешением на доступ к ресурсам.
   - В поле **Password (пароль**) введите пароль, связанный с именем пользователя.
   - Кроме того, может появиться запрос на предоставление дополнительных факторов, если администратор настроил проверку подлинности таким образом.
5. Нажмите кнопку **сохранить**.

После этого центр подключений должен отобразить удаленные ресурсы.

После того как вы подписались на веб-канал, содержимое веб-канала будет регулярно обновляться. Ресурсы можно добавлять, изменять или удалять в зависимости от изменений, внесенных администратором.

## <a name="next-steps"></a>Следующие шаги

Чтобы узнать больше о том, как использовать клиент iOS, ознакомьтесь с документацией по [началу работы с клиентом iOS](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/) .
