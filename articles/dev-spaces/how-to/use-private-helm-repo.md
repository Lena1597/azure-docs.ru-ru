---
title: Как использовать частный репозиторий Helm в Azure Dev Spaces
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/22/2019
ms.topic: conceptual
description: Используйте частный репозиторий Helm в пространстве разработки Azure.
keywords: DOCKER, Kubernetes, Azure, AKS, служба контейнеров Azure, контейнеры, Helm
manager: gwallace
ms.openlocfilehash: b1579adc00540a429170027b66c5d3e508bcb5d4
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718742"
---
# <a name="use-a-private-helm-repository-in-azure-dev-spaces"></a>Используйте частный репозиторий Helm в Azure Dev Spaces

[Helm][helm] — это диспетчер пакетов для куберентес. Helm использует формат [диаграммы][helm-chart] для упаковки зависимостей. Helm диаграммы хранятся в репозитории, который может быть общедоступным или частным. Azure Dev Spaces получает диаграммы Helm из общедоступных репозиториев при запуске приложения. В случаях, когда репозиторий Helm является частным или Azure Dev Spaces не может получить к нему доступ, можно добавить диаграмму из этого репозитория непосредственно в приложение. Добавление диаграммы напрямую позволяет Azure Dev Spaces запускать приложение без доступа к частному репозиторию Helm.

## <a name="add-the-private-helm-repository-to-your-local-machine"></a>Добавьте частный репозиторий Helm на локальный компьютер.

Чтобы получить доступ к частному репозиторию Helm с локального компьютера, используйте обновление репозитория [Helm][helm-repo-add] и [Helm][helm-repo-update] .

```cmd
helm repo add privateRepoName http://example.com/helm/v1/repo --username user --password 5tr0ng_P@ssw0rd!
helm repo update
```

## <a name="add-the-chart-to-your-application"></a>Добавление диаграммы в приложение

Перейдите в каталог проекта и запустите `azds prep`.

```cmd
azds prep --public
```

> [!TIP]
> Команда `prep` пытается создать [Dockerfile и диаграмму Helm](../how-dev-spaces-works.md#prepare-your-code) для проекта. Azure Dev Spaces использует эти файлы для сборки и выполнения кода. Но вы можете изменить эти файлы, если нужно определить другой способ сборки и запуска проекта.

Создайте файл [требований. YAML][helm-requirements] с диаграммой в каталоге диаграммы приложения. Например, если приложение называется *APP1*, необходимо создать *Charts/APP1/требования. YAML*.

```yaml
dependencies:
    - name: mychart
      version: 0.1.0
      repository:  http://example.com/helm/v1/repo
```

Перейдите в каталог диаграммы приложения и используйте [Обновление зависимостей Helm][helm-dependency-update] , чтобы обновить зависимости Helm для приложения и загрузить диаграмму из частного репозитория.

```cmd
helm dependency update
```

Убедитесь, что в каталог диаграммы приложения добавлен *подкаталог Charts* с файлом *tgz* . Например, *Charts/APP1/Charts/мичарт-0.1.0. tgz*.

Диаграмма из закрытого репозитория Helm была скачана и добавлена в проект. Удалите файл *требований. YAML* , чтобы модули разработки не попытаются обновить эту зависимость.

## <a name="run-your-application"></a>Запуск приложения

Перейдите в корневой каталог проекта и запустите `azds up`, чтобы убедиться, что приложение успешно запущено в вашем пространстве разработки.

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
...
Service 'app1' port 'http' is available at http://app1.1234567890abcdef1234.eus.azds.io/
Service 'app1' port 80 (http) is available at http://localhost:54256
...
```

## <a name="next-steps"></a>Дальнейшие действия

Узнайте больше о [Helm и принципах его работы][helm].

[helm]: https://docs.helm.sh
[helm-chart]: https://helm.sh/docs/topics/charts/
[helm-dependency-update]: https://helm.sh/docs/topics/charts/#managing-dependencies-with-the-dependencies-field
[helm-repo-add]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-repo-update]: https://helm.sh/docs/intro/using_helm/#helm-repo-working-with-repositories
[helm-requirements]: https://helm.sh/docs/topics/charts/#chart-dependencies