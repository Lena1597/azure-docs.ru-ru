---
title: Управление зависимостями JAR-Azure HDInsight
description: В этой статье обсуждаются рекомендации по управлению зависимостями для архивации Java (JAR) для приложений HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: da3387dd9846847f7643ded43c8cbff8ed8b166e
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2020
ms.locfileid: "77135737"
---
# <a name="jar-dependency-management-best-practices"></a>Рекомендации по управлению зависимостями JAR

Компоненты, установленные в кластерах HDInsight, имеют зависимости от сторонних библиотек. Обычно эти встроенные компоненты ссылаются на конкретную версию общих модулей, например гуава. При отправке приложения с его зависимостями может возникнуть конфликт между разными версиями одного модуля. Если версия компонента, на которую указывает ссылка, является первой, встроенные компоненты могут вызывать исключения из-за несовместимости версий. Однако, если встроенные компоненты вставляют свои зависимости в подкаталоги классов, то приложение может вызывать такие ошибки, как `NoSuchMethod`.

Чтобы избежать конфликта версий, рассмотрите возможность заливки зависимостей приложений.

## <a name="what-does-package-shading-mean"></a>Что означает затенение пакетов?
Заливка предоставляет способ включения и переименования зависимостей. Он перемещает классы и перезаписывает затронутые байты и ресурсы для создания закрытой копии зависимостей.

## <a name="how-to-shade-a-package"></a>Как затенить пакет?

### <a name="use-uber-jar"></a>Использование Uber-JAR
Uber-JAR — это один файл JAR, который содержит JAR и его зависимости. Зависимости в Uber-JAR по умолчанию не затенены. В некоторых случаях это может привести к конфликту версий, если другие компоненты или приложения ссылаются на разные версии этих библиотек. Чтобы избежать этого, можно создать файл Uber-JAR с некоторыми (или всеми) зависимостями от закрашенных зависимостей.

### <a name="shade-package-using-maven"></a>Затенение пакета с помощью Maven
Maven может создавать приложения, написанные как на Java, так и в Scala. Maven-Shader-Plugin позволяет легко создать затененный Uber JAR.

В приведенном ниже примере показан файл `pom.xml` который был обновлен для затенения пакета с помощью Maven-Shader-plugin.  XML-раздел `<relocation>…</relocation>` перемещает классы из `com.google.guava` пакета в `com.google.shaded.guava` пакета, перемещая соответствующие записи JAR-файла и переписывая затронутый байт-код.

После изменения `pom.xml`можно выполнить `mvn package`, чтобы построить затененный Uber-JAR.

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <relocations>
                <relocation>
                  <pattern>com.google.guava</pattern>
                  <shadedPattern>com.google.shaded.guava</shadedPattern>
                </relocation>
              </relocations>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

### <a name="shade-package-using-sbt"></a>Затенение пакета с помощью SBT
SBT также является средством сборки для Scala и Java. У SBT нет подключаемого модуля шейдера, такого как Maven-Shader-plugin. Вы можете изменить `build.sbt` файл, чтобы затенить пакеты. 

Например, чтобы затенить `com.google.guava`, можно добавить приведенную ниже команду в файл `build.sbt`:

```scala
assemblyShadeRules in assembly := Seq(
  ShadeRule.rename("com.google.guava" -> "com.google.shaded.guava.@1").inAll
)
```

Затем можно запустить `sbt clean` и `sbt assembly`, чтобы создать затененный JAR-файл. 

## <a name="next-steps"></a>Дальнейшие действия

* [Использование средств IntelliJ HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox)

* [Создание приложения Scala Maven для Spark в IntelliJ](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-create-standalone-application)
