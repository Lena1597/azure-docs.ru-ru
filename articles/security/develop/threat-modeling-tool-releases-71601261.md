---
title: Microsoft Threat Modeling Tool выпуск 1/29/2019 — Azure
description: Документирование заметок о выпуске средства моделирования угроз
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 01/25/2019
ms.openlocfilehash: cec5abe358189184648c5a40a809b570f243fbf4
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "75548779"
---
# <a name="threat-modeling-tool-update-release-71601261---1292019"></a>Threat Modeling Tool, выпуск обновления 7.1.60126.1 от 29.01.2019

29 января 2019 г. была выпущена версия 7.1.60126.1 средства Microsoft Threat Modeling Tool со следующими изменениями.

- Минимальная требуемая версия .NET была увеличена до [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262).
- Минимальная требуемая версия Windows повышена до [юбилейного обновления Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) из-за зависимости от .NET.
- В меню параметров средства добавлена функция переключения для проверки моделей.
- Обновлено несколько ссылок в свойствах угроз.
- В интерфейс начального экрана внесено несколько незначительных изменений.
- Threat Modeling Tool теперь наследует параметры TLS операционной системы сервера виртуальных машин и поддерживается в средах, требующих использования TLS 1.2 или более поздней версии.

## <a name="feature-changes"></a>Изменения функций

### <a name="model-validation-option"></a>Параметр проверки модели

На основе отзывов клиентов в средство был добавлен параметр для включения или отключения проверки модели. Ранее, если шаблон использовал один однонаправленный поток данных между двумя объектами, вы могли получить сообщение об ошибке в кадре messages, о котором говорится: Обжектснаме требует хотя бы одного "Any". Отключение проверки модели предотвратит отображение этих предупреждений в представлении.

Параметр включения и выключения проверки модели можно найти в меню "Файл->Настройки->Параметры". Значением по умолчанию для этого параметра является "Отключено".

![Параметр проверки модели](./media/threat-modeling-tool-releases-71601261/tmt_model_validation_option.png)

## <a name="system-requirements"></a>Системные требования

- Поддерживаемые операционные системы
  - [Юбилейное обновление Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) или более поздней версии
- Требуемая версия .NET
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) или более поздней версии
- Дополнительные требования
  - Для получения обновлений средства и шаблонов требуется подключение к Интернету.

## <a name="known-issues"></a>Известные проблемы

### <a name="unsupported-systems"></a>Неподдерживаемые системы

#### <a name="issue"></a>Проблема

Пользователи систем Windows 10, которые не могут установить .NET 4.7.1 или более поздней версии, например Windows 10 Enterprise LTSB (версии 1507), не смогут открыть средство после обновления. Эти старые версии Windows больше не поддерживаются платформами для Threat Modeling Tool и не должны устанавливать последнее обновление.

#### <a name="workaround"></a>Обходной путь

Пользователи Windows 10 Enterprise LTSB (версии 1507), которые установили последнее обновление, могут вернуться к предыдущей версии Threat Modeling Tool через диалоговое окно удаления в "Приложениях и возможностях".

## <a name="documentation-and-feedback"></a>Документация и отзывы

- Документация по Threat Modeling Tool находится на сайте [docs.microsoft.com](threat-modeling-tool.md) и содержит информацию [об использовании средства](threat-modeling-tool-getting-started.md).

## <a name="next-steps"></a>Дальнейшие действия

Скачайте последнюю версию [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
