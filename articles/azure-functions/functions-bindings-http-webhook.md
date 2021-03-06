---
title: Триггеры и привязки HTTP в службе "Функции Azure"
description: Узнайте, как использовать триггеры и привязки HTTP в функциях Azure.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: ebf0728184a5fc104e3e1e7d8a199fec328dbdc0
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210180"
---
# <a name="azure-functions-http-triggers-and-bindings-overview"></a>Общие сведения о триггерах и привязках HTTP в функциях Azure

Функции Azure могут вызываться через HTTP-запросы для создания бессерверных интерфейсов API и реагирования на [веб-перехватчики](https://en.wikipedia.org/wiki/Webhook).

| Действие | Тип |
|---------|---------|
| Выполнение функции из HTTP-запроса | [Триггер](./functions-bindings-http-webhook-trigger.md) |
| Возврат HTTP-ответа из функции |[Выходная привязка](./functions-bindings-http-webhook-output.md) |

Код в этой статье по умолчанию использует синтаксис .NET Core, используемый в функциях версии 2. x и более поздних. Сведения о синтаксисе версии 1.x см. на странице с [соответствующими шаблонами функций](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="add-to-your-functions-app"></a>Добавление в приложение функций

### <a name="functions-2x-and-higher"></a>Функции 2. x и более поздних версий

Для работы с триггером и привязками требуется ссылка на соответствующий пакет. Пакет NuGet используется для библиотек классов .NET, в то время как расширение объединяет все остальные типы приложений.

| Язык                                        | Добавить по...                                   | Примечания 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Установка [Пакет NuGet], версия 3. x | |
| C#Script, Java, JavaScript, Python, PowerShell | Регистрация [Пакет расширений]          | [Расширение "инструменты Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) " рекомендуется использовать с Visual Studio Code. |
| C#Скрипт (только в интерактивном режиме в портал Azure)         | Добавление привязки                            | Чтобы обновить существующие расширения привязки без повторной публикации приложения функции, см. статью [Обновление расширений]. |

[core tools]: ./functions-run-local.md
[Пакет расширений]: ./functions-bindings-register.md#extension-bundles
[Пакет NuGet]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http.
[Обновление расширений]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Функции 1.x

Функции 1. x автоматически имеют ссылку на пакет NuGet [Microsoft. Azure. веб-задания](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) , версия 2. x.

## <a name="next-steps"></a>Следующие шаги

- [Выполнение функции из HTTP-запроса](./functions-bindings-http-webhook-trigger.md)
- [Возврат HTTP-ответа из функции](./functions-bindings-http-webhook-output.md)
