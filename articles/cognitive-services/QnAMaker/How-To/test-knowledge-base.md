---
title: Тестирование базы знаний — QnA Maker
titleSuffix: Azure Cognitive Services
description: Тестирование базы знаний QnA Maker является важной частью итерационного процесса для повышения точности возвращаемых ответов. Вы можете протестировать базу знаний через расширенный интерфейс чата, который также позволяет вносить правки.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c139d3a740067e3cecaff90d3171d7b0cb3d52c7
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091752"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>Тестирование базы знаний в QnA Maker

Тестирование базы знаний QnA Maker является важной частью итерационного процесса для повышения точности возвращаемых ответов. Вы можете протестировать базу знаний через расширенный интерфейс чата, который также позволяет вносить правки.

## <a name="interactively-test-in-qna-maker-portal"></a>Интерактивный тест на портале QnA Maker

1. Получите доступ к базе знаний, выбрав ее имя на странице **Мои базы знаний**.
1. Чтобы открыть выдвигающуюся панель тестирования, щелкните **Тестировать** на верхней панели приложения.
1. Введите запрос в текстовом поле и нажмите клавишу ВВОД.
1. Ответ наилучшего соответствия из базы знаний возвращается как отклик.

### <a name="clear-test-panel"></a>Очистка панели тестирования

Чтобы очистить все введенные тестовые запросы и их результаты из консоли тестирования, выберите **Начать сначала** в левом верхнем углу панели тестирования.

### <a name="close-test-panel"></a>Закрытие панели тестирования

Чтобы закрыть панель тестирования, еще раз нажмите кнопку **Тестировать**. При открытой панели тестирования нельзя изменить содержимое базы знаний.

### <a name="inspect-score"></a>Проверка оценки

Проверка сведений в результате тестирования выполняется на панели проверки.

1.  На открытой выдвигающейся панели тестирования нажмите кнопку **Проверка** для получения более подробной информации об этом ответе.

    ![Проверка ответов](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Появится панель проверки. На панели находится намерение с высокой оценкой, а также любые идентифицированные сущности. На панели отображается результат выбранного высказывания.

### <a name="correct-the-top-scoring-answer"></a>Исправьте ответы с высокой оценкой

Если ответы с высокой оценкой неверны, выберите правильный ответ из списка и нажмите **Сохранить и Обучать**.

![Исправьте ответы с высокой оценкой](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Добавление альтернативных вопросов

Можно добавить альтернативные формы вопроса для данного ответа. Чтобы добавить альтернативные ответы, введите их в текстовое поле и нажмите "Ввод". Для сохранения обновлений выберите **Сохранить и Обучать**.

![Добавление альтернативных вопросов](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Добавление нового ответа

Можно добавить новый ответ, если любой из существующих ответов, которые были сопоставлены, неверный или не существует ответа в базе знаний (в КБ нет хорошего совпадения). 

В нижней части списка ответов используйте текстовое поле, чтобы ввести новый ответ, и нажмите клавишу ВВОД, чтобы добавить его. 

Для сохранения этого ответа выберите **Сохранить и Обучать**. Новая пара вопросов и ответов теперь будет добавлена в базу знаний. 

> [!NOTE]
> Все изменения в базе знаний сохраняются только при нажатии кнопки **Сохранить и Обучать**.

### <a name="test-the-published-knowledge-base"></a>Тестирование опубликованной базы знаний

Опубликованную версию базы знаний можно проверить на панели тестирования. После публикации базы знаний выберите поле **опубликованная база знаний** и отправьте запрос, чтобы получить результаты из ОПУБЛИКОВАННОЙ базы знаний.

![Проверка на соответствие опубликованной базе знаний](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="batch-test-with-tool"></a>Пакетный тест с помощью инструмента

Используйте средство пакетного тестирования, если вы хотите:

* определение верхнего ответа и оценки набора вопросов
* Проверка ожидаемого ответа на набор вопросов

Пакетное тестирование предоставляется с помощью средства пакетного тестирования. Это средство доступно как [сжатый исполняемый файл](https://aka.ms/qnamakerbatchtestingtool) для загрузки или как [ C# исходный код](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting). 

[Справочная документация по этому средству](../reference-tsv-format-batch-testing.md) включает:

* пример командной строки средства
* Формат входных и файловых файлов TSV 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Публикация базы знаний](./publish-knowledge-base.md)
