---
title: Обновление агента зависимостей Azure Monitor для виртуальных машин | Документация Майкрософт
description: В этой статье описывается обновление агента зависимостей Azure Monitor для виртуальных машин с помощью командной строки, мастера установки и других методов.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/30/2019
ms.openlocfilehash: 548a578365b03162396fb8618718ab1e7ce5b081
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75400799"
---
# <a name="how-to-upgrade-the-azure-monitor-for-vms-dependency-agent"></a>Обновление агента зависимостей Azure Monitor для виртуальных машин

После первоначального развертывания агента зависимостей Azure Monitor для виртуальных машин обновляются обновления, включающие исправления ошибок или поддержку новых функций или функций.  Эта статья поможет вам понять доступные методы и выполнить обновление вручную или с помощью службы автоматизации.

## <a name="upgrade-options"></a>Варианты обновления 

Агент зависимостей для Windows и Linux можно обновить до последнего выпуска вручную или автоматически в зависимости от сценария развертывания и среды, в которой выполняется компьютер. Для обновления агента можно использовать следующие методы.

|Среда |Метод установки |Метод обновления |
|------------|--------------------|---------------|
|Виртуальная машина Azure | Расширение виртуальной машины агента зависимостей для [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) и [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) | Агент автоматически обновляется по умолчанию, если не настроен шаблон Azure Resource Manager для отказаться от установки свойства *autoUpgradeMinorVersion* в **значение false**. Обновление дополнительной версии, в которой отключено автоматическое обновление, и обновление основной версии, выполните тот же метод-Uninstall и переустановите расширение. |
| Пользовательские образы виртуальных машин Azure | Установка агента зависимостей для Windows и Linux вручную | Обновление виртуальных машин до последней версии агента необходимо выполнить из командной строки, в которой выполняется пакет установщика Windows или самораспаковывающийся и устанавливаемый комплект сценариев оболочки для Linux.|
| Виртуальные машины, не относящиеся к Azure | Установка агента зависимостей для Windows и Linux вручную | Обновление виртуальных машин до последней версии агента необходимо выполнить из командной строки, в которой выполняется пакет установщика Windows или самораспаковывающийся и устанавливаемый комплект сценариев оболочки для Linux. |

## <a name="upgrade-windows-agent"></a>Обновление агента Windows 

Чтобы обновить агент на виртуальной машине Windows до последней версии, не установленной с помощью расширения виртуальной машины агента зависимостей, запустите из командной строки, скрипта или другого решения по автоматизации или с помощью мастера установки файла installdependencyagent-Windows. exe.  

Последнюю версию агента Windows можно скачать [отсюда](https://aka.ms/dependencyagentwindows).

### <a name="using-the-setup-wizard"></a>Использование мастера установки

1. Войдите в систему компьютера, используя учетную запись с правами администратора.

2. Выполните **файла installdependencyagent-Windows. exe** , чтобы запустить мастер установки.

3. В диалоговом окне **dependency Agent установка 9.9.1** нажмите кнопку **я принимаю** , чтобы принять условия лицензионного соглашения.

5. В диалоговом окне **dependency Agent удаление 9.9.0** нажмите кнопку **Далее**. На странице состояние отображается ход удаления предыдущей версии.

6. В диалоговом окне **dependency Agent удаления 9.9.0** нажмите кнопку **Удалить** , чтобы продолжить удаление предыдущей версии из пути, указанного в диалоговом окне. 

7. В диалоговом окне **dependency Agent удаление 9.9.0** , отобразится ход удаления и после завершения откроется страница **Завершение dependency Agent удаления** . Нажмите кнопку **Готово**.

8. В диалоговом окне **dependency Agent установки 9.9.1** появится ход выполнения установки. Когда появится страница **завершение dependency Agent удаления** , нажмите кнопку **Готово**. 

### <a name="from-the-command-line"></a>Из командной строки

1. Войдите в систему компьютера, используя учетную запись с правами администратора.

2. Выполните следующую команду.

    ```dos
    InstallDependencyAgent-Windows.exe /S /RebootMode=manual
    ```

    Параметр `/RebootMode=manual` предотвращает автоматическую перезагрузку компьютера, если некоторые процессы используют файлы из предыдущей версии и заблокировали их. 

3. Чтобы убедиться, что обновление прошло успешно, проверьте `install.log` для получения подробных сведений о настройке. Каталогом журналов является *%Programfiles%\Microsoft Dependency Agent\logs*.

## <a name="upgrade-linux-agent"></a>Обновление агента Linux 

Обновление с предыдущих версий агента зависимостей в Linux поддерживается и выполняется после той же команды, что и Новая установка.

Последнюю версию агента Windows можно скачать [отсюда](https://aka.ms/dependencyagentlinux).

1. Войдите в систему компьютера, используя учетную запись с правами администратора.

2. Выполните следующую команду в качестве корневого`sh InstallDependencyAgent-Linux64.bin -s`. 

Если Dependency Agent не запускается, просмотрите подробные сведения об ошибке в записях журналов. В агентах Linux каталог журнала находится в расположении */var/opt/microsoft/dependency-agent/log*. 

## <a name="next-steps"></a>Дальнейшие действия

Если вы хотите отключить мониторинг виртуальных машин в течение определенного периода времени или полностью удалить Azure Monitor для виртуальных машин, см. статью [Отключение мониторинга виртуальных машин в Azure Monitor для виртуальных машин](vminsights-optout.md).
