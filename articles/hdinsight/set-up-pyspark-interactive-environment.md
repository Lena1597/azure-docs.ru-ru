---
title: PySpark интерактивной среды с помощью средств Azure HDInsight
description: Сведения о создании и отправке запросов и скриптов с помощью средств Azure HDInsight для Visual Studio Code.
keywords: VScode,средства Azure HDInsight,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: db2336fb79207ada24b71e0e64f0aaaab543e4da
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/31/2019
ms.locfileid: "73241542"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Настройка интерактивной среды PySpark для Visual Studio Code

Следующие шаги показывают, как настроить интерактивную среду PySpark в VS Code.

Мы используем команду **python или pip** для сборки виртуального окружения в основной папке. Если вы хотите использовать другую версию, вам нужно вручную изменить версию по умолчанию команды **python или pip**. Дополнительные сведения см. в статье о команде [update-alternatives](https://linux.die.net/man/8/update-alternatives).

1. Установите [Python](https://www.python.org/downloads/) и [PIP](https://pip.pypa.io/en/stable/installing/).

   + Установите Python отсюда: [https://www.python.org/downloads/](https://www.python.org/downloads/).
   + Установите PIP с [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) (если он не установлен из установки Python).
   + Убедитесь, что Python и PIP установлены успешно с помощью следующих команд. (необязательный параметр)

        ![Проверка команды версии для Python PIP](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > Рекомендуется вручную установить Python вместо использования версии macOS по умолчанию.

2. Установите **virtualenv**, выполнив команду, приведенную ниже.

   ```
   pip install virtualenv
   ```

## <a name="other-packages"></a>Другие пакеты

При появлении сообщения об ошибке установите необходимые пакеты, выполнив следующие команды:

   ![Установка пакета libkrb5 для Python](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```
sudo apt-get install libkrb5-dev
```

```
sudo apt-get install python-dev
```

Перезапустите VS Code, а затем снова перейдите к редактору сценариев, который работает на базе **интерактивной среды PySpark службы HDInsight**.

## <a name="next-steps"></a>Дальнейшие действия

### <a name="demo"></a>Демонстрация
* HDInsight для VS Code: [видео](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Средства и расширения
* [Использование средств Azure HDInsight для Visual Studio Code](hdinsight-for-vscode.md)
* [Создание приложений Apache Spark для кластера HDInsight с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin.md)
* [Удаленная или локальная отладка приложений Spark в кластере HDInsight с помощью Azure Toolkit for IntelliJ через SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Удаленная отладка приложений Apache Spark в HDInsight через VPN с помощью Azure Toolkit for IntelliJ](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Использование средств HDInsight в Azure Toolkit for Eclipse для создания приложений Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Использование записных книжек Zeppelin с кластером Apache Spark в Azure HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Ядра для записной книжки Jupyter в кластерах Spark в Azure HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Использование внешних пакетов с записными книжками Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Установка записной книжки Jupyter на компьютере и ее подключение к кластеру Apache Spark в Azure HDInsight (предварительная версия)](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Визуализация данных Apache Hive с Microsoft Power BI с использованием ODBC в Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Use Apache Zeppelin to run Apache Hive queries in Azure HDInsight](./interactive-query/hdinsight-connect-hive-zeppelin.md) (Выполнение запросов Apache Hive в Azure HDInsight с помощью Apache Zeppelin)
