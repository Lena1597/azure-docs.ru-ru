---
title: Потоковая передача журналов облачного приложения Azure весны в режиме реального времени
description: Как мгновенно просматривать журналы приложений с помощью потоковой передачи журналов
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.openlocfilehash: 27978d367ded7a31d73949cd675ae9e6f8cb887c
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264005"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Потоковая передача журналов облачного приложения Azure весны в режиме реального времени
Azure Веснного облака позволяет выполнять потоковую передачу журналов в Azure CLI для получения журналов консоли приложений в режиме реального времени для устранения неполадок. Вы также можете [анализировать журналы и метрики с помощью параметров диагностики](./diagnostic-services.md).

## <a name="prerequisites"></a>Технические условия

* Установите [расширение Azure CLI](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) для пружинного облака, минимальной версии 0.2.0.
* Экземпляр **Azure веснного облака** с выполняющимся приложением, например [пружинное приложение Cloud](./spring-cloud-quickstart-launch-app-cli.md).

## <a name="use-cli-to-tail-logs"></a>Использование интерфейса командной строки для заключительного фрагмента журналов

Чтобы избежать повторного указания имени группы ресурсов и экземпляра службы, задайте имя группы ресурсов по умолчанию и имя кластера.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
В следующих примерах группа ресурсов и имя службы будут пропущены в командах.

### <a name="tail-log-for-app-with-single-instance"></a>Заключительный фрагмент журнала для приложения с одним экземпляром
Если приложение с именем auth-Service имеет только один экземпляр, журнал экземпляра приложения можно просмотреть с помощью следующей команды:
```
az spring-cloud app log tail -n auth-service
```
Будут возвращены журналы:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Заключительный фрагмент журнала для приложения с несколькими экземплярами
Если для приложения существует несколько экземпляров с именем `auth-service`, журнал экземпляра можно просмотреть с помощью параметра `-i/--instance`. 

Сначала можно получить имена экземпляров приложения с помощью следующей команды.

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
С результатами:

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Затем можно выполнить потоковую передачу журналов экземпляра приложения с параметром `-i/--instance`:

```
az spring-cloud app log tail -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Кроме того, можно получить сведения об экземплярах приложения из портал Azure.  Выбрав **приложения** в левой области навигации в облачной службе Azure "Весна", выберите **экземпляры приложения**.

### <a name="continuously-stream-new-logs"></a>Непрерывный поток новых журналов
По умолчанию `az spring-cloud ap log tail` выводит только существующие журналы, переданные в консоль приложения, а затем завершает работу. Если вы хотите создать потоковую передачу новых журналов, добавьте-f (--следовать):  

```
az spring-cloud app log tail -n auth-service -f
``` 
Чтобы проверить все поддерживаемые параметры ведения журнала, выполните следующие действия.
``` 
az spring-cloud app log tail -h 
```

## <a name="next-steps"></a>Дальнейшие действия

* [Анализ журналов и метрик с помощью параметров диагностики](./diagnostic-services.md)

 





